## 第六章：Nginx 实战部署 Vue3 + Django + MySQL

> 🏷️ **本章定位：⭐ 开发必会 —— 前后端分离项目部署的核心技能，从 Nginx 配置到 Vue3/Django/MySQL 全栈上线，开发日常高频，务必掌握**

> 📌 **本章学习方式**：每一小节都遵循"🎯 作用说明 → 概念 → 一步一步操作 → 验证 → 常见坑"的结构。建议在 CentOS 7 虚拟机里跟着敲一遍，所有命令已验证在 CentOS 7.6 + Nginx 1.20.1 可跑无错。

> 📌 **本章依赖**：默认你已学完第三章（服务管理、systemctl、yum）和第二章（文件权限、目录操作）。Nginx 安装部分第三章 11 节已讲过，本章只做"速引 + 配置详解"，重点放在**配置和部署**。



---

## 1. Nginx 是什么、为什么需要它（必会）

> 🏷️ ⭐ 开发必会 —— 不理解 Nginx 的定位，后面的每一行配置都是死记硬背。先花 10 分钟把"为什么"想明白。

### 1.1 Nginx 简介（了解）

> 🏷️ 📖 概念了解 —— 读懂即可

**Nginx**（读作 "engine-x"，引擎 X）是一个开源的 Web 服务器 / 反向代理服务器。由俄罗斯程序员 Igor Sysoev 在 2004 年发布，现在是全球使用率最高的 Web 服务器之一（据 W3Techs 统计，活跃网站中超过 1/3 用 Nginx）。

它最擅长两件事：

1. **发静态文件**：HTML/CSS/JS/图片，又快又省内存（事件驱动，一个 worker 进程能扛上万个连接）。
2. **反向代理**：把请求按 URL 规则转发给后端的 Python/Java/Node 应用，自己再当个"前台接待"做负载均衡、HTTPS、限流、日志。

> 📌 **类比**：把 Nginx 想象成餐厅前台。客人（浏览器）进门，前台自己能上的凉菜（静态文件）直接上；热菜（动态请求）前台记下来转给后厨（Django），后厨做好了前台再端出去。客人从头到尾只跟前台打交道，根本不知道后厨在哪。

### 1.2 一个请求从浏览器到数据库（必会）

> 🏷️ ⭐ 开发必会 —— 这是全栈部署的"地图"，看懂这张图，后面每个组件的作用都对上号

下面这张图是 Vue3 + Django + MySQL 部署架构，**全章所有配置都围绕它展开**：

```
┌──────────────┐
│   浏览器      │  用户打开网页 / 点按钮发请求
└──────┬───────┘
       │  HTTP（80/443）
       ▼
┌──────────────────────────────────────────────┐
│  Nginx（80 端口）—— 全站唯一对外的大门         │
│  职责①：托管 Vue3 打包后的静态文件（dist）     │
│  职责②：把 /api/ 开头的请求"反向代理"给 Django │
│  职责③：发 Django collectstatic 收集的静态文件 │
└──────┬───────────────────────┬───────────────┘
       │ /（找文件）            │ /api/（转发）
       ▼                       ▼
┌──────────────────┐   ┌────────────────────────┐
│ dist/ 目录        │   │ gunicorn（127.0.0.1:8000）│
│ index.html/js/css│   │ 里面跑着 Django 程序       │
└──────────────────┘   └───────────┬────────────┘
                                   │ 查数据
                                   ▼
                          ┌─────────────────┐
                          │ MySQL（3306）    │
                          └─────────────────┘
```

**两个典型请求的完整路径：**

1. **打开 `http://服务器IP/`**：浏览器 → Nginx 在 dist 目录找到 `index.html` 直接返回（**全程不碰 Django**，所以静态页面极快）。
2. **打开 `http://服务器IP/api/users/`**：浏览器 → Nginx 发现是 `/api/` 开头 → 转发给 `127.0.0.1:8000` 的 gunicorn → Django 查 MySQL → JSON 原路返回。

**每个组件是干嘛的（一句话版）：**

| 组件 | 角色 | 类比 |
|------|------|------|
| **Nginx** | Web 服务器：发静态文件又快又稳；同时当"前台接待"，动态请求转给后端 | 餐厅前台：凉菜自己上，热菜转交后厨 |
| **gunicorn** | Python 的"应用服务器"，真正运行 Django 代码、返回 JSON | 后厨（Django 自带的 `runserver` 是试吃摊，不能营业） |
| **Django** | 你的后端业务代码（API、admin 后台） | 菜谱+厨师 |
| **MySQL** | 数据库 | 仓库 |
| **Vue3 dist** | `npm run build` 产出的一堆静态文件 | 印好的菜单宣传单 |

### 1.3 Nginx 在前后端分离架构中的三大职责（必会）

> 🏷️ ⭐ 开发必会 —— 后面所有配置都是围绕这三大职责展开

| 职责 | 解决什么问题 | 对应配置 |
|------|------------|---------|
| **① 托管前端静态文件** | Vue 打包后的 dist 不能让 Django 发（慢且浪费），交给 Nginx 直接发 | `root` + `index` + `try_files` |
| **② 反向代理后端 API** | gunicorn 只监听 `127.0.0.1:8000`，公网访问不到，需要 Nginx 把 `/api/` 请求转给它 | `location /api/` + `proxy_pass` |
| **③ 发 Django 静态文件** | Django admin 样式、collectstatic 收集的静态文件交给 Nginx 发，否则后台丑且慢 | `location /static/` + `alias` |

### 1.4 为什么不让 Django 直接对外（必会）

> 🏷️ ⭐ 开发必会 —— 面试高频，也是理解架构的关键

理论上 `python manage.py runserver 0.0.0.0:80` 也能让 Django 直接对外，**但生产环境绝对不能这么做**，三个原因：

1. **静态文件性能差**：Django 自带的服务器是单线程调试用，发图片/JS/CSS 又慢又浪费 Python 进程。Nginx 是 C 写的事件驱动，发静态文件快几十倍。
2. **安全性**：gunicorn 只监听 `127.0.0.1:8000`（只有本机能访问），不暴露公网。攻击者扫不到 8000 端口，攻击面小很多。
3. **职责分离**：Nginx 还负责 HTTPS、限流、日志、gzip 压缩、多域名分发……这些都是 Nginx 擅长的，不该让 Django 干。

> 🔑 **记住这条铁律**：公网只开 80（HTTP）和 443（HTTPS）端口，8000（gunicorn）和 3306（MySQL）一律不对公网开放。Nginx 是全站唯一对外的大门。

---

## 2. 安装 Nginx（必会）

> 🏷️ ⭐ 开发必会 —— 部署的第一步，本章只做"速引 + 验证"，详细原理见第三章 11 节

### 2.1 安装方式对比表（必会）

> 🏷️ ⭐ 开发必会 —— 选错方式后面改配置位置全错

| 安装方式 | 版本 | 配置位置 | 难度 | 推荐场景 |
|---------|------|---------|------|---------|
| **yum + EPEL**（本章主推） | 1.20.1 | `/etc/nginx/` | ⭐ 简单 | 命令行党、初学者、生产环境 |
| **官方源 yum** | 最新主线版 | `/etc/nginx/` | ⭐⭐ 略复杂 | 需要最新特性的场景 |
| **源码编译** | 任意 | 自定义 | ⭐⭐⭐⭐ 难 | 需要非默认模块（极少） |
| **宝塔面板** | 1.22.x | `/www/server/nginx/` | ⭐ 极简 | 图形界面党、新手 |

> 📌 **本章统一用 yum + EPEL 装 1.20.1**，所有后续配置都基于这个路径。宝塔用户看每节末尾的"宝塔对照操作"。

### 2.2 CentOS 7 EOL 源修复前置（必会）

> 🏷️ ⭐ 开发必会 —— CentOS 7 官方源 2024-06-30 下线，不修源装不了任何东西

CentOS 7 已于 2024 年 6 月 30 日停止维护（EOL），官方源 `mirrorlist.centos.org` 已离线，`yum install` 会报 `Could not retrieve mirrorlist` 或 404。**装 Nginx 前必须先把 base 源和 EPEL 源换成阿里云的归档源**。

**第一步：替换 base 源为阿里云 vault**

```bash
# 备份原 repo 文件（可选，但建议）
sudo cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak 2>/dev/null

# 用 tee 覆盖写入（cat > 在 sudo 下权限不够，必须用 tee）
sudo tee /etc/yum.repos.d/CentOS-Base.repo > /dev/null << 'EOF'
[base]
name=CentOS-7 - Base
baseurl=https://mirrors.aliyun.com/centos-vault/7.9.2009/os/$basearch/
enabled=1
gpgcheck=0

[updates]
name=CentOS-7 - Updates
baseurl=https://mirrors.aliyun.com/centos-vault/7.9.2009/updates/$basearch/
enabled=1
gpgcheck=0

[extras]
name=CentOS-7 - Extras
baseurl=https://mirrors.aliyun.com/centos-vault/7.9.2009/extras/$basearch/
enabled=1
gpgcheck=0
EOF
```

**第二步：替换 EPEL 源为阿里云 epel-archive**

```bash
sudo tee /etc/yum.repos.d/epel.repo > /dev/null << 'EOF'
[epel]
name=EPEL 7 - $basearch
baseurl=https://mirrors.aliyun.com/epel-archive/7/$basearch/
enabled=1
gpgcheck=0
EOF
```

> ⚠️ **常见坑**：
> - 不要写 `7/Everything/$basearch/`，会 404；正确路径是 `7/$basearch/`。
> - 不要用 `sudo cat > file << EOF`，sudo 只作用于 cat，重定向 `>` 是当前用户的权限，会报"权限不够"。必须用 `sudo tee file > /dev/null << EOF`。

**第三步：刷新缓存**

```bash
sudo yum clean all          # 清掉旧缓存
sudo yum makecache          # 重建缓存（这一步会拉取阿里云的源元数据，可能要 1~2 分钟）
```

看到 `Metadata Cache Created` 就成功了。

### 2.3 用 yum 安装 Nginx 一步一步

> 🏷️ ⭐ 开发必会 —— 一条命令搞定

源修好后，装 Nginx 就一条命令：

```bash
sudo yum install -y nginx
```

**这一步发生了什么**：
1. yum 从 EPEL 源下载 nginx 1.20.1 的 rpm 包及依赖
2. 自动创建以下目录结构（这是后续所有配置的基础）：

| 路径 | 作用 |
|------|------|
| `/etc/nginx/nginx.conf` | 主配置文件 |
| `/etc/nginx/conf.d/` | 站点配置目录（**我们写的配置都放这里**，主配置会自动 include） |
| `/etc/nginx/mime.types` | 文件扩展名到 MIME 类型的映射（决定 Content-Type） |
| `/usr/share/nginx/html/` | 默认网站根目录（装好就一个 index.html） |
| `/var/log/nginx/` | 日志目录（access.log + error.log） |
| `/usr/sbin/nginx` | 可执行文件 |

### 2.4 启动、开机自启、防火墙放行（必会）

> 🏷️ ⭐ 开发必会 —— 服务管理三件套，第三章 5 节已讲过

```bash
# ① 启动 nginx 并设置开机自启（--now 同时启动，第三章 systemctl 讲过）
sudo systemctl enable --now nginx

# ② 查看状态（看到 active (running) 就 OK）
sudo systemctl status nginx

# ③ 防火墙放行 80 端口（HTTP），CentOS 7 默认开 firewalld
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https   # 顺手放 443，后面 HTTPS 要用
sudo firewall-cmd --reload                            # 重新加载规则才生效
```

> ⚠️ **常见坑**：firewalld 的 `--permanent` 写入持久化配置，但**不影响当前会话**；不加 `--permanent` 只影响当前会话，重启就丢。必须 `--permanent` 写入 + `--reload` 激活。两步缺一不可（第三章 5.4 节详细讲过）。

### 2.5 验证安装成功（必会）

> 🏷️ ⭐ 开发必会 —— 三种验证方式，从本机到公网

```bash
# ① 本机自测：curl 自己（看到 HTML 就说明 Nginx 在跑）
curl http://127.0.0.1/

# ② 看端口：80 端口被 nginx 进程占用
sudo ss -tlnp | grep :80
# 输出类似：LISTEN 0 511 *:80 *:* users:(("nginx",pid=1234,fd=6))

# ③ 浏览器访问：在 Windows 浏览器输入 http://服务器IP/
# 看到 Nginx 欢迎页（红色大字 "Welcome to nginx!"）就成功
```

> ⚠️ **如果浏览器打不开但 curl 能通**：99% 是防火墙没放行 80，或者云服务器安全组没放行（阿里云/腾讯云在控制台有个"安全组"，跟系统 firewalld 是两套，都要放行）。

### 2.6 用 whereis 查看 Nginx 装到哪了 + 目录含义（必会）

> 🏷️ ⭐ 开发必会 —— 装完先搞清楚"东西在哪"，后面改配置、看日志、找证书才不抓瞎

**第一步：用 whereis 查 nginx 的所有相关路径**

```bash
whereis nginx
# 输出类似：
# nginx: /usr/sbin/nginx /etc/nginx /usr/lib64/nginx /usr/share/nginx /usr/share/man/man8/nginx.8.gz
```

**whereis 输出怎么读**：`whereis 命令` 会把和 nginx 相关的**四类路径**全列出来，每类用空格分隔：

| 字段 | 含义 | 本章关心吗 |
|------|------|-----------|
| `/usr/sbin/nginx` | **可执行文件**位置（命令本体） | ⭐ 排错时手动执行用，如 `/usr/sbin/nginx -V` 看编译参数 |
| `/etc/nginx` | **配置文件**目录（所有 .conf 在这） | ⭐⭐⭐ 最常用，所有配置都在这 |
| `/usr/lib64/nginx` | **模块**目录（动态加载的 .so 文件） | 一般不动 |
| `/usr/share/nginx` | **默认站点目录**（html/ 在里面） | 装好自带欢迎页的位置 |
| `/usr/share/man/...` | man 帮助文档 | 不用管 |

> 📌 **whereis vs which 的区别**（容易混）：
> - `which nginx` 只告诉你"敲 nginx 这个命令时系统用哪个文件"（一个路径），如 `/usr/sbin/nginx`
> - `whereis nginx` 把"可执行文件 + 配置 + 模块 + 帮助"全列出来（多个路径），看安装布局用 whereis 更全

**第二步：用 ls 逐个看目录里有什么**

```bash
# ① 配置目录（最常打交道）
ls -l /etc/nginx/
# 典型输出：
# drwxr-xr-x 2 root root  6 ... conf.d      ← 站点配置写这里（★ 后面我们写的 .conf 都放这）
# -rw-r--r-- 1 root root 1007 ... fastcgi_params
# -rw-r--r-- 1 root root 2837 ... mime.types  ← 文件扩展名→Content-Type 映射
# -rw-r--r-- 1 root root 1451 ... nginx.conf  ← ★ 主配置文件（3.3 节详解）
# ...

# ② 站点配置子目录（★ 我们要写的配置都放这里）
ls -l /etc/nginx/conf.d/
# 装好后默认有一个 default.conf（welcome 页的配置）
# 我们后面会在这里新建 myproject.conf

# ③ 默认站点目录（Nginx 欢迎页就在这）
ls /usr/share/nginx/html/
# 输出：50x.html  index.html   ← 就是 curl 127.0.0.1 看到的那个页面

# ④ 日志目录（502/403 排错必看）
ls -l /var/log/nginx/
# 输出：access.log  error.log   ← 访问日志和错误日志

# ⑤ 可执行文件位置（升级/排错时用）
ls -l /usr/sbin/nginx
# -rwxr-xr-x 1 root root ... /usr/sbin/nginx
```

**第三步：用 nginx -V 看编译参数（确认配置文件路径）**

```bash
# -V（大写）显示编译时的路径配置，比 whereis 更权威
nginx -V 2>&1 | grep -oE 'conf-path=[^ ]*|error-log-path=[^ ]*|http-log-path=[^ ]*'
# 输出：
# conf-path=/etc/nginx/nginx.conf
# error-log-path=/var/log/nginx/error.log
# http-log-path=/var/log/nginx/access.log
```

> 📌 `nginx -V` 会输出一堆 `--with-xxx` 和 `--xxx-path=` 参数，是编译时定的"默认路径"。改配置前看一眼，避免改错地方（比如宝塔装的 nginx，conf-path 不是 `/etc/nginx/nginx.conf`）。

**核心目录速查表（必记）**

| 路径 | 作用 | 何时用 |
|------|------|-------|
| `/etc/nginx/nginx.conf` | 主配置文件 | 3.3 节详解，几乎不动 |
| `/etc/nginx/conf.d/*.conf` | **站点配置目录** | ⭐ **我们写的配置都放这里** |
| `/var/log/nginx/access.log` | 访问日志 | 排错看请求是否到达 |
| `/var/log/nginx/error.log` | 错误日志 | 502/403 第一时间看 |
| `/usr/share/nginx/html/` | 默认站点目录 | 装好自带的欢迎页，生产不用 |
| `/usr/sbin/nginx` | 可执行文件 | 手动执行 `nginx -t`、`nginx -V` |

> 🔑 **记忆口诀**：**配置在 `/etc/nginx/`，日志在 `/var/log/nginx/`，命令在 `/usr/sbin/nginx`**。开发日常 90% 时间在 `/etc/nginx/conf.d/` 写配置，剩下 10% 在 `/var/log/nginx/` 看日志。

#### 拓展：前端打包好的页面（dist）该放哪？要不要放 /usr/share/nginx/html/？（必会）

> 🏷️ ⭐ 开发必会 —— 新手最常问的问题，也是最容易放错地方导致后续一堆坑

**先说结论**：`/usr/share/nginx/html/` 是 Nginx **装好自带的默认站点目录**，里面就一个 `index.html`（就是 curl 看到的欢迎页）。**技术上能放前端打包产物，但不推荐**。生产环境应该建独立目录放。

**为什么不推荐放 `/usr/share/nginx/html/`**：

| 原因 | 说明 |
|------|------|
| ① 系统目录，升级会被覆盖 | Nginx 用 yum 升级/重装时，`/usr/share/nginx/html/` 可能被官方默认文件覆盖，你的 dist 就没了 |
| ② 多项目混在一起 | 一个服务器跑多个项目时，全塞 `/usr/share/nginx/html/` 根本分不清谁是谁 |
| ③ 权限是 root，改文件麻烦 | 这个目录所有者是 root，每次上传/改文件都要 sudo，scp 也得用 root |
| ④ 不符合"软件和项目分离"规范 | 系统软件（nginx 本体）和你的业务代码（前端 dist）应该分开管，方便备份、迁移 |

**推荐做法：建独立的站点目录 `/www/项目名/`**

#### 拓展补充：为什么选 `/www/`？和 `/var/www/`、`/opt/`、`/srv/` 有什么区别？（了解）

> 🏷️ 📖 概念了解 —— 目录选择不是技术对错，是约定俗成，看懂后自己选一个统一的即可

**常见候选目录对比表**：

| 目录 | 谁的约定 | 适合放什么 | 评价 |
|------|---------|-----------|------|
| `/var/www/` | **Apache HTTPD 传统**（Debian/Ubuntu + Apache 装好后默认站点在这） | Web 站点 | ⭐ Apache 党用，Nginx 不强制 |
| `/opt/` | **FHS 规范**（第三方独立软件，完整目录树如 `/opt/google/chrome/`） | 完整的第三方软件包 | 不适合放一堆零散 Web 项目 |
| `/srv/` | **FHS 规范**（系统提供的服务数据） | 理论上最适合放 Web 站点 | 实际很少人用，教程也少 |
| `/www/` | **Nginx + 宝塔生态约定**（宝塔默认所有站点放这） | Web 项目 | ⭐⭐ 国内最常见，路径短 |
| `/data/` / `/app/` | 企业自定义 | 业务数据/应用 | 大厂常见，自定义规范 |

**关键澄清**：

1. **`/var/www/` 不是 Linux 通用标准**，是 Apache HTTPD 的传统默认目录。Debian/Ubuntu 用 apt 装 Apache 后，默认站点配置 `DocumentRoot /var/www/html`，所以才"看起来很标准"。但 **CentOS + Nginx 没有这个默认目录**，yum 装 Nginx 默认是 `/usr/share/nginx/html/`。

2. **`/opt/` 的设计意图**是放"完整的、自包含的第三方软件"（整个软件目录树，如 `/opt/nginx/`、`/opt/jdk/`），不太适合放"一堆零散的 Web 项目 dist 目录"。

3. **`/srv/` 是 FHS 规范里"服务数据"的目录**，理论上最适合放 Web 站点，但实际运维圈用得少，宝塔、各种教程都不推，社区资源也少。

4. **`/www/` 不是 FHS 标准目录**，但是 Nginx + 宝塔面板生态约定俗成的目录，国内教程和运维最常见。好处是路径短、和宝塔对齐、迁移方便。

**选择建议**（按场景）：

| 你的场景 | 推荐目录 | 理由 |
|---------|---------|------|
| **用宝塔面板** | `/www/` | 宝塔默认，面板操作最顺 |
| **纯命令行 + 国内教程跟学** | `/www/` | 和绝大多数教程、宝塔生态对齐，遇到问题好查 |
| **Apache 背景/Debian 系** | `/var/www/` | 符合 Apache 党习惯 |
| **企业有运维规范** | 跟企业规范（常见 `/data/`、`/app/`） | 团队统一最重要 |
| **追求 FHS 规范** | `/srv/www/` | 理论最规范，但社区资源少 |

> 📌 **本章统一用 `/www/`** 的三个理由：① 和宝塔生态对齐，迁移到宝塔无缝衔接；② 路径短，配置写起来清爽；③ 国内教程最常见，遇到问题搜得到参考。**关键不是选哪个，是统一**——一个服务器里别一半项目放 `/var/www/`、一半放 `/opt/`，管理起来崩溃。

> 🔑 **一句话记忆**：**目录选择没有对错，只有约定**。Nginx 自带默认是 `/usr/share/nginx/html/`（系统目录别用），Apache 党用 `/var/www/`，宝塔党用 `/www/`，本章跟宝塔用 `/www/`。

```bash
# 第一步：建项目专属目录（一个项目一个目录，互不影响）
sudo mkdir -p /www/myproject/frontend/dist

# 第二步：把前端打包产物（dist 内容）放到这里
# 本地 npm run build 后，把 dist 目录里的内容传上来
# 用 scp：
scp -r D:\code\myproject-frontend\dist/* root@192.168.233.131:/www/myproject/frontend/dist/

# 第三步：改所有者，让 nginx 用户能读（避免 403）
sudo chown -R nginx:nginx /www/myproject/frontend/dist
sudo chmod -R 755 /www/myproject/frontend/dist

# 第四步：在 Nginx 配置里用 root 指向这个目录（第 5 节详讲）
# server {
#     root /www/myproject/frontend/dist;   ← 指向我们建的目录，不是默认的
#     ...
# }
```

**目录规划推荐表**（一个服务器跑多个项目的标准布局）：

```
/www/                          ← 所有项目统一放这（自建，非系统目录）
├── myproject/                 ← 项目 A
│   ├── frontend/
│   │   └── dist/              ← Vue3 打包产物
│   ├── backend/               ← Django 后端代码
│   └── logs/                  ← 项目日志
├── projectB/                  ← 项目 B
│   ├── frontend/dist/
│   └── ...
└── demo/                      ← 测试站点
    └── index.html
```

**对比表：放默认目录 vs 放 /www/**

| 维度 | 放 `/usr/share/nginx/html/` | 放 `/www/项目名/frontend/dist/` |
|------|----------------------------|--------------------------------|
| 升级风险 | Nginx 重装可能被覆盖 | 无影响，独立目录 |
| 多项目 | 混在一起 | 一项目一目录，清晰 |
| 权限 | root，改文件要 sudo | 可设 nginx 用户，方便 |
| 备份迁移 | 和系统软件混在一起难分离 | 直接打包 `/www/` 即可 |
| 规范性 | 不符合"软件/业务分离" | 符合企业规范 |

> 🔑 **一句话记忆**：**系统目录（`/usr/share/nginx/`）放 Nginx 自带的东西，业务目录（`/www/`）放你的项目**。前端 dist 永远放 `/www/项目名/frontend/dist/`，然后用 `root` 指向它。`/usr/share/nginx/html/` 只在第一次装完 Nginx 用 curl 自测欢迎页时看一眼，后面基本不碰。

#### 实战下一步：把 /www 下的前端项目跑起来（必会）

> 🏷️ ⭐ 开发必会 —— 假设你已经把纯前端项目放到 `/www/` 下了，接下来用 Nginx 让它对外访问，5 步搞定

**🎯 这一小节解决什么问题**：你已经把前端项目（比如 Vue 打包的 dist，或者一个简单的 index.html）放到 `/www/myproject/dist/` 下了，但浏览器还访问不到——因为 Nginx 还不知道你要用这个目录。下面 5 步把"放好的文件"变成"浏览器能访问的网站"。

**假设你的目录结构**（如果你的略有不同，把路径换成你自己的即可）：

```
/www/myproject/
└── dist/                 ← 前端打包产物
    ├── index.html
    └── assets/
        ├── index-a1b2.js
        └── style-c3d4.css
```

**第一步：确认文件位置和内容**

```bash
# 看一下前端文件确实在这
ls -l /www/myproject/dist/
# 应该看到 index.html 和 assets/ 等

# 确认 index.html 非空
ls -lh /www/myproject/dist/index.html
# 如果是 0 字节，说明上传失败，重新传一次
```

**第二步：改所有者让 nginx 能读（避免 403）**

```bash
# 把目录所有者改成 nginx 用户（Nginx worker 进程以 nginx 身份运行，读不到就 403）
sudo chown -R nginx:nginx /www/myproject/
sudo chmod -R 755 /www/myproject/
# -R 递归改所有子目录和文件
# 755 = 所有者可读写执行、组和其他人可读可执行（不能写，安全）
```

> ⚠️ **重要提示：CentOS 7 还有第二道权限关卡 SELinux**！上面 `chown`/`chmod` 只是第一道（DAC）。如果做完这两步还是 403，99% 是 SELinux（MAC）拦的。**两种解法任选其一**：
>
> **方法 A（推荐：打标签，保留 SELinux 安全性）**
> ```bash
> sudo chcon -R -t httpd_sys_content_t /www/myproject/dist
> ```
> 详见下方 **【chcon 是什么？SELinux 是第三道权限关卡】** 小节。
>
> **方法 B（开发环境简单粗暴：关掉 SELinux）**
> ```bash
> sudo setenforce 0
> sudo sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config
> ```

#### 拓展：这两步不做会怎么样？（必会）

> 🏷️ ⭐ 开发必会 —— 理解权限原理，遇到 403 不慌

**先说结论**：**不一定立刻 403，取决于你怎么上传的文件**。但强烈建议做，因为权限不确定会导致"有时能跑、有时不能"的玄学问题。

**分情况看**：

| 上传方式 | 文件所有者 | 默认权限 | nginx 能读吗 | 结果 |
|---------|----------|---------|-------------|------|
| `scp root@...` 上传 | `root:root` | 文件 644、目录 755 | ✓ 能读（"其他人"有 r 权限） | **能访问**（但不规范） |
| `scp lwc@...` 上传 | `lwc:lwc` | 文件 644、目录 755 | ✓ 能读（"其他人"有 r 权限） | **能访问**（但不规范） |
| 某些工具/特殊 umask | `root:root` | 文件 600、目录 700 | ✗ 读不到 | **403 Forbidden** |
| Windows 打包后上传（权限丢失） | `root:root` | 权限可能很乱 | ✗ 可能读不到 | **403 Forbidden** |

**为什么"能读"也要做这两步**：

1. **`chown` 改所有者的意义**：
   - 不做：文件所有者是 root 或上传用户，nginx 是"其他人"，靠"其他人 r 权限"读
   - 做了：文件所有者直接是 nginx，nginx 用"所有者权限"读，**最稳**
   - 好处：以后 nginx 用户能直接改/删文件（如 collectstatic 写入 static 目录），不用再 sudo

2. **`chmod` 改权限的意义**：
   - 不做：权限取决于上传工具默认值（scp/rsync/WinSCP/宝塔 默认都不一样），**不可控**
   - 做了：明确所有者 rwx、组 r-x、其他人 r-x，**确定可读且安全**
   - 755 是 Web 静态文件的"标准权限"：所有者可改、其他人只能读不能写

**最坏情况演示（不做这两步的真实后果）**：

```bash
# 假设你用 scp 上传，文件所有者是 root，某个子目录权限是 700
$ ls -l /www/myproject/dist/
drwx------ 2 root root  4096 ... assets/   ← 700，只有 root 能进
-rw------- 1 root root   134 ... index.html ← 600，只有 root 能读

# nginx 用户尝试访问：
$ sudo -u nginx cat /www/myproject/dist/index.html
cat: /www/myproject/dist/index.html: Permission denied   ← 读不到

# 浏览器访问 → Nginx 读不到 → 返回 403 Forbidden
```

**排错：怎么判断是不是权限问题**：

```bash
# ① 看 nginx 错误日志（最直接）
sudo tail -f /var/log/nginx/error.log
# 看到 "Permission denied" = 权限问题

# ② 用 namei 查每一层目录的权限（nginx 要穿过每一层）
namei -l /www/myproject/dist/index.html
# 输出每一层的权限，看哪个是 drw------- 或 drwx------

# ③ 模拟 nginx 用户访问测试
sudo -u nginx cat /www/myproject/dist/index.html
# 能输出内容 = 权限 OK；报 Permission denied = 权限有问题
```

**权限数字速记**（看懂 755/644/600 怎么来的）：

#### 拓展补充：/www/ 这个目录本身要不要改所有者为 nginx？（必会）

> 🏷️ ⭐ 开发必会 —— 涉及 Linux 目录权限的核心规则，理解了再不会卡 403

**先说结论**：**不改也能跑，但改了更统一**。关键是 `/www/` 本身的权限必须是 755（让 nginx 能"穿过"它），所有者改成 nginx 不是必须的。

**为什么"每一层目录"都关键**：

nginx 访问 `/www/myproject/dist/index.html` 必须能**穿过每一层目录**：

```
/                       ← 根目录（默认 755，所有人 r-x，不用管）
└── /www/               ← ★ 你手动建的，默认 root:root 755
    └── /www/myproject/ ← chown 改成 nginx:nginx 755
        └── /www/myproject/dist/  ← nginx:nginx 755
            └── /www/myproject/dist/index.html  ← nginx:nginx 644
```

**Linux 目录权限核心规则**（必记）：
- 目录的 `x` 权限 = "能否进入这个目录"（cd / ls / 访问子文件）
- 目录的 `r` 权限 = "能否列出目录内容"（ls 看文件名）
- **nginx 要访问深层文件，必须每一层目录都有 x 权限**，缺一层就 403

**分情况分析**：

| `/www/` 所有者 | `/www/` 权限 | nginx 身份 | 能进入 /www/ 吗 | 结果 |
|---------------|-------------|-----------|----------------|------|
| `root:root`（默认） | `755`（drwxr-xr-x） | "其他人" | ✓ 有 r-x 能进入 | **能跑** |
| `root:root`（默认） | `750`（drwxr-x---） | "其他人" | ✗ 没有 x 进不去 | **403** |
| `root:root`（默认） | `700`（drwx------） | "其他人" | ✗ 没有 x 进不去 | **403** |
| `nginx:nginx`（改了） | `755` | "所有者" | ✓ 有 rwx 能进入 | **能跑** |
| `nginx:nginx`（改了） | `750` | "所有者" | ✓ 所有者有 rwx | **能跑** |

**关键结论**：

1. **`/www/` 默认 root:root 755 → nginx 能用**：因为 755 给"其他人"r-x 权限，nginx 作为"其他人"能进入，所以不改所有者也能跑。
2. **改所有者为 nginx 更统一**：让 `/www/` 整棵树所有者一致，避免"上层 root、下层 nginx"的混合状态，方便统一管理。
3. **`/www/` 权限必须 755**（或更宽松）：不能是 700/750，否则 nginx 作为"其他人"进不去（如果不改所有者）。

**推荐两种做法**（任选其一）：

**做法 A（最简单，推荐）**：整个 /www 树都给 nginx

```bash
# 把 /www 整棵树所有者改成 nginx（包括 /www 本身）
sudo chown -R nginx:nginx /www/
sudo chmod -R 755 /www/
# 所有者统一为 nginx，权限统一 755，最省心
```

**做法 B（更安全，多项目场景）**：/www 保持 root，只改项目子目录

```bash
# /www/ 保持 root:root 755（默认，不用动）
# 只改各个项目子目录
sudo chown -R nginx:nginx /www/myproject/
sudo chmod -R 755 /www/myproject/
sudo chown -R nginx:nginx /www/projectB/
sudo chmod -R 755 /www/projectB/
# 好处：不同项目可以由不同用户管理，/www 本身由 root 管
# 前提：/www 本身必须是 755，否则 nginx 进不去
```

**验证 /www 权限对不对**：

```bash
# ① 看 /www 本身权限（应该是 drwxr-xr-x = 755）
ls -ld /www/
# drwxr-xr-x 2 nginx nginx 4096 ... /www/   ← 改了所有者
# 或
# drwxr-xr-x 2 root  root  4096 ... /www/   ← 没改所有者，但 755 也能用

# ② 用 namei 查访问路径每一层权限
namei -l /www/myproject/dist/index.html
# 输出每一层，看有没有 drw------- 或 drwx------ 的，有就 403

# ③ 模拟 nginx 用户测试能否进入 /www
sudo -u nginx ls /www/
# 能列出内容 = OK；Permission denied = 权限有问题
```

> ⚠️ **常见坑**：用 `sudo mkdir -p /www/myproject/dist` 建的目录默认是 root:root 755，能用。但如果你之后用 `chmod 700 /www/` 把 `/www/` 改成 700 了（比如想"保护"它），nginx 立刻 403——因为 nginx 作为"其他人"没了 x 权限。**目录权限千万别用 700，至少 755**。

> 🔑 **一句话记忆**：**目录权限看 x，所有者看统一**。`/www/` 本身保持 755（不管所有者是谁，nginx 都能进入）；项目子目录 `chown -R nginx:nginx` + `chmod -R 755` 让 nginx 用所有者身份访问。**最省心的做法**：`sudo chown -R nginx:nginx /www/ && sudo chmod -R 755 /www/`，整棵树统一。

#### 拓展补充：chcon 是什么？SELinux 是第三道权限关卡（了解，但有坑）

> 🏷️ 📖 概念了解 —— 开发通常关 SELinux，但要知道坑在哪；centos7 最容易卡半天的"玄学 403"基本都是它

**你看到的命令**：
```bash
sudo chcon -R -t httpd_sys_content_t /www/myproject/dist
```

**逐字拆解**：

| 部分 | 含义 |
|------|------|
| `chcon` | **ch**ange **con**text —— 改文件的 SELinux 上下文（标签） |
| `-R` | 递归（改目录下所有文件） |
| `-t httpd_sys_content_t` | 把标签类型（type）改成 `httpd_sys_content_t` —— 这是 nginx/httpd **允许读**的标签 |
| `/www/myproject/dist` | 要改标签的目录 |

**它解决什么问题**：传统权限（chmod 755 + chown nginx）都对了，nginx 还是 403——因为 **SELinux 不让 nginx 读"没标签"的文件**。

**Linux 有两道权限关卡**（必理解）：

| 关卡 | 控制者 | 命令 | 不通过的现象 |
|------|--------|------|-------------|
| **第一道：DAC**（自主访问控制） | 传统 rwx 权限 | `chmod` / `chown` | 403 Forbidden（Permission denied） |
| **第二道：MAC**（强制访问控制） | SELinux 标签 | `chcon` / `semanage` | 403 Forbidden（但 rwx 全对） |

> 🔑 **核心**：第一道通过了，第二道还要再过一遍。两道都过 nginx 才能读文件。**关 SELinux = 跳过第二道**，只看第一道。

**为什么有"坑"**：

1. **CentOS 7 默认开 SELinux**（enforcing 模式），新手不知道还有第二道关卡
2. **`chcon` 改的标签是临时的**：`chcon` 改的标签会被 `restorecon` 命令重置回默认值，重启后某些情况也会丢
3. **SELinux 拒绝不报错在 nginx 日志**：nginx error.log 只写 `Permission denied`，和 DAC 拒绝一模一样，**看不出来是 SELinux 拦的**
4. **默认标签是 `default_t` 或 `user_home_t`**：你用 `scp` 上传或手动 `mkdir` 的文件，默认不是 `httpd_sys_content_t`，nginx 读不了

**判断是不是 SELinux 拦的**：

```bash
# ① 看 SELinux 状态（Enforcing = 开启）
getenforce
# Enforcing  ← 开着，可能是它拦的
# Disabled   ← 关着，肯定不是它

# ② 看文件 SELinux 标签
ls -Z /www/myproject/dist/index.html
# -rw-r--r--. root root unconfined_u:object_r:default_t:s0  index.html
#                                          ↑ 这里是 default_t = nginx 读不了
# 想要的是 httpd_sys_content_t

# ③ 看 SELinux 拒绝日志（最权威）
sudo tail /var/log/audit/audit.log | grep nginx
# 看到 "denied" 就是 SELinux 拦的
# 或用 aureport 看
sudo aureport -a | tail
```

**解决方法（三种，按推荐度排序）**：

**方法 1：永久打标签（推荐，生产环境用）**

```bash
# 先装 semanage 工具（CentOS 7 minimal 可能没装）
sudo yum install -y policycoreutils-python

# 1. 添加永久规则：让 /www/myproject/ 下所有文件都自动是 httpd 标签
sudo semanage fcontext -a -t httpd_sys_content_t "/www/myproject(/.*)?"

# 2. 应用规则（把现有文件标签刷新）
sudo restorecon -Rv /www/myproject/
# -v 显示改了哪些文件的标签

# 之后新上传的文件也自动是这个标签（只要在 /www/myproject/ 下）
```

**方法 2：临时打标签（快速验证用，重启可能丢）**

```bash
sudo chcon -R -t httpd_sys_content_t /www/myproject/dist
# 立即生效，但 restorecon 会重置；临时调试用它最快
```

**方法 3：关闭 SELinux（最省心，开发环境推荐）**

```bash
# 临时关闭（重启恢复）
sudo setenforce 0

# 永久关闭（重启也是关的）
sudo vi /etc/selinux/config
# 把 SELINUX=enforcing 改成 SELINUX=disabled
# 保存后重启系统生效：sudo reboot
```

**三种方法对比**：

| 方法 | 难度 | 安全性 | 适用场景 | 坑 |
|------|------|--------|---------|-----|
| 永久标签（semanage + restorecon） | ★★ | ⭐⭐⭐ 最安全 | 生产环境、公司服务器 | 命令长，需要装包 |
| 临时标签（chcon） | ★ | ⭐⭐ | 快速验证、临时调试 | restorecon 会重置 |
| 关 SELinux | ★ | ⭐ 不安全 | 开发环境、学习环境 | 生产别关，被运维骂 |

**开发环境为什么建议直接关 SELinux**：

1. **学习曲线陡**：SELinux 策略要写一大套规则，开发不需要
2. **报错信息误导**：日志里只写 Permission denied，和 DAC 拒绝一样，排查半天
3. **90% 的公司生产环境也关**：除了金融/政府项目，绝大多数互联网公司 SELinux 都是 disabled
4. **宝塔面板默认会提示关 SELinux**：装宝塔时第一步就是检查并关闭 SELinux

**怎么判断要不要管 SELinux**：

```bash
getenforce
# Disabled     → 不用管，问题不在这里
# Permissive   → 不用管（只警告不拦）
# Enforcing    → ★ 可能是它拦的，按上面三种方法处理
```

> ⚠️ **经典踩坑场景**：chmod 755 + chown nginx 都做了，`sudo -u nginx cat index.html` 也能读到内容，但浏览器访问就是 403。**这就是 SELinux 拦的**——`sudo -u nginx` 绕过了 SELinux（因为你是 root 切换的），但 nginx 服务进程跑在 `httpd_t` 域里，受 SELinux 限制。用 `getenforce` + `ls -Z` + `/var/log/audit/audit.log` 三件套判断。

> 🔑 **一句话记忆**：**chmod 是第一道关卡，chcon 是第二道关卡**。CentOS 7 默认开 SELinux 时第二道也生效。开发环境直接 `setenforce 0` + 改 `/etc/selinux/config` 关掉最省心；生产环境用 `semanage fcontext` + `restorecon` 永久打标签。**排查 403 三件套**：`getenforce` + `ls -Z` + `/var/log/audit/audit.log`。

**权限数字速记**（看懂 755/644/600 怎么来的）：

| 数字 | 权限 | 谁能干啥 |
|------|------|---------|
| 7 | rwx | 读+写+执行（所有者常用） |
| 5 | r-x | 读+执行（目录必须 x 才能进入） |
| 4 | r-- | 只读 |
| 0 | --- | 啥都不能 |

- **目录**必须要有 `x` 权限才能进入（cd/ls），所以目录权限通常是 755 或 750
- **文件**不需要 `x`（除非是脚本），所以文件权限通常是 644 或 640
- `chmod -R 755` 是简单粗暴：目录和文件都给 755，文件多个 x 权限无害（但不是最规范）
- 更规范的做法：`chmod -R 755` 目录 + `find /www -type f -exec chmod 644 {} \;` 文件单独设 644

> 🔑 **一句话记忆**：**`chown` 决定"谁能改"，`chmod` 决定"谁能读"**。不做不一定立刻 403（取决于上传默认权限），但**做了就确定不 403**。生产环境别赌，老老实实做这两步。

**第三步：写 Nginx 站点配置**

```bash
sudo vi /etc/nginx/conf.d/myproject.conf
```

写入以下内容（最小可用配置，带逐行注释）：

```nginx
server {
    listen 80;                              # 监听 80 端口（HTTP 默认，访问时 URL 不用带端口）
    server_name 192.168.233.133;            # ★ 改成你的服务器 IP（有域名写域名）
    root /www/myproject/dist;              # ★ 指向你放前端文件的目录
    index index.html;                       # 访问 / 时默认返回 index.html

    location / {
        try_files $uri $uri/ /index.html;  # ★ Vue/React history 模式必加，刷新不 404
    }
}

或者
server {
    listen 80;
    server_name _;   # 所有host都匹配，不用写IP
    root /www/myproject/dist; 
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

> 📌 **如果是纯静态 HTML 网站**（没有 Vue Router history 模式），`try_files` 那行可以不加，但加了也无害，建议都加上。

**第四步：黄金三步生效**

```bash
# ① 测语法：看到 syntax is ok / test is successful 才能继续
sudo nginx -t

# ② 平滑重载（不断开已有连接）
sudo systemctl reload nginx

# ③ 本机自测
curl http://127.0.0.1/
# 返回 index.html 的内容（一堆 <div> 或 <html>...）就成功了
```

**第五步：防火墙放行 80 + 浏览器访问**

```bash
# 防火墙放行 HTTP（80 端口）
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --reload

# 验证规则生效
sudo firewall-cmd --list-all | grep services
# 应该看到 services: dhcpv6-client http https ssh
```

**浏览器访问**：在 Windows 浏览器输入 `http://你的服务器IP/`，看到你的前端页面就成功了。

**常见问题速查表**：

| 现象 | 原因 | 解决 |
|------|------|------|
| **403 Forbidden** | nginx 用户读不到文件（所有者是 root） | `sudo chown -R nginx:nginx /www/myproject/` |
| **404 Not Found** | root 路径写错，或目录里没 index.html | `ls` 确认路径和文件存在 |
| **刷新某页面 404** | 没加 `try_files`（Vue/React history 模式） | 在 `location /` 里加 `try_files $uri $uri/ /index.html;` |
| **页面白屏** | JS/CSS 路径错（vite.config.js 的 base 没改） | F12 看 Network 哪些资源 404，改 base |
| **curl 通但浏览器打不开** | 防火墙没放 80，或云服务器安全组没放行 | `firewall-cmd --add-service=http` + 云控制台安全组 |
| **中文乱码** | 没声明字符集 | 配置里加 `charset utf-8;` |
| **nginx -t 报错** | 配置语法错（少分号、少括号） | 看报错行号，对照修复 |

> 🔑 **这一小节是"纯前端最小部署"**：没有后端、没有数据库，5 步就能让一个前端项目上线。后面要加后端 Django（第 7 节）、加 MySQL（第 8 节）、完整 Vue3+Django+MySQL（第 9 节），都是在这一小节基础上**加 location、加 proxy_pass**，骨架不变。

---

## 3. Nginx 配置文件结构（必会）

> 🏷️ ⭐ 开发必会 —— 改配置前必须先认识配置文件的结构和加载机制，否则改错地方等于白改

### 3.1 配置文件在哪：yum vs 宝塔对比（必会）

> 🏷️ ⭐ 开发必会 —— 选错位置改了等于没改，是新手最常踩的坑

| 安装方式 | 主配置文件 | 站点配置放哪（推荐写这里） |
|----------|-----------|--------------------------|
| yum 装的 Nginx | `/etc/nginx/nginx.conf` | `/etc/nginx/conf.d/*.conf`（主配置里有 `include /etc/nginx/conf.d/*.conf;` 自动加载） |
| **宝塔装的 Nginx** | `/www/server/nginx/conf/nginx.conf` | **不要手改主配置**，在面板「网站 → 站点设置 → 配置文件」里改，实际落在 `/www/server/panel/vhost/nginx/站点名.conf` |

> ⚠️ **宝塔环境最常见的坑**：把配置写进 `/etc/nginx/conf.d/`，但宝塔的 nginx.conf 根本不 include 这个目录，结果"语法对、不生效、还 403/404"。**用宝塔就老实在面板的站点配置里改。**

**本章后续所有命令行配置都默认写到 `/etc/nginx/conf.d/` 下，文件名随意但后缀必须是 `.conf`**。例如：

```bash
sudo vi /etc/nginx/conf.d/myproject.conf     # 我们的项目配置
```

### 3.2 配置四层结构 events/http/server/location（必会）

> 🏷️ ⭐ 开发必会 —— 先建立大局观，再写配置不迷路

```nginx
# nginx.conf 的骨架（不用背，认识层级即可）
events { }                  # ① 连接相关，保持默认不用动

http {                      # ② 所有网站配置都在 http { } 里面
    server {                # ③ 一个 server { } = 一个网站（按端口+域名区分）
        location / { }      # ④ 一个 location = 这个网站里"某类 URL"的处理规则
        location /api/ { }  # 可以有多个 location，按 URL 前缀分工
    }
}
```

**层级关系（必须记住）**：

```
http
 └── server（一个网站）
      ├── location /（处理前端页面）
      ├── location /api/（转发给后端）
      └── location /static/（发静态文件）
```

**每一层是干嘛的**：

| 层级 | 作用 | 类比 |
|------|------|------|
| `events` | 配置连接数、网络模型，保持默认不动 | 大楼的水电总闸 |
| `http` | 所有网站配置的容器，定义全局 http 设置 | 整栋办公楼 |
| `server` | 一个网站，按 `listen` 端口 + `server_name` 域名区分 | 一层楼里的一个公司 |
| `location` | 网站内某类 URL 的处理规则 | 公司里的不同部门 |

> 📌 **我们写配置的视角**：90% 的时间都在写 `server { }` 和它里面的 `location { }`。`events` 和 `http` 几乎不动。

### 3.3 主配置文件 nginx.conf 逐段解读（必会）

> 🏷️ ⭐ 开发必会 —— 看一遍知道每段干嘛的，不背

```bash
sudo cat /etc/nginx/nginx.conf
```

**关键段落（去掉注释后的精简版）**：

```nginx
user nginx;                          # nginx worker 进程以哪个用户运行
worker_processes auto;                # worker 进程数（auto = CPU 核数）
error_log /var/log/nginx/error.log;  # 错误日志位置（502/403 先看它）
pid /run/nginx.pid;                  # 进程号文件

events {
    worker_connections 1024;          # 每个 worker 最大连接数，保持默认即可
}

http {
    include       /etc/nginx/mime.types;   # 加载 MIME 类型映射
    default_type  application/octet-stream; # 默认 Content-Type

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    # ↑ access.log 的日志格式（排错时看 status 字段）

    access_log  /var/log/nginx/access.log  main;  # 访问日志位置

    sendfile        on;                # 零拷贝发文件，性能优化，保持 on
    keepalive_timeout  65;            # 长连接超时（秒），保持默认

    include /etc/nginx/conf.d/*.conf;  # ★ 加载站点配置目录下所有 .conf 文件
}
```

> 🔑 **最重要的一行**：`include /etc/nginx/conf.d/*.conf;` —— 这就是为什么我们把配置写到 `conf.d/` 下就能自动生效。Nginx 启动时会把这个目录下所有 `.conf` 文件拼到 `http { }` 里面。

**开发者只需要关注的字段**：
- `user nginx`：worker 进程身份。如果 dist 目录权限是 `lwc:lwc`，nginx 用户读不到会 403（11.4 节详讲）。
- `error_log` / `access_log`：日志位置，排错必看。
- `include /etc/nginx/conf.d/*.conf`：站点配置加载机制（下一节详讲）。

### 3.4 站点配置加载机制 include（必会）

> 🏷️ ⭐ 开发必会 —— 理解 include 才知道为什么写进 conf.d 就生效

**加载流程**：

1. Nginx 启动 → 读 `/etc/nginx/nginx.conf`（主配置）
2. 主配置里 `http { }` 块末尾有 `include /etc/nginx/conf.d/*.conf;`
3. Nginx 把 `conf.d/` 目录下所有 `*.conf` 文件内容**原地展开**到 `http { }` 里
4. 等价于：你写的 `server { }` 直接写在 `http { }` 里

**图示**：

```
nginx.conf                          你的配置文件
─────────────                       ───────────────
http {                              # /etc/nginx/conf.d/myproject.conf
    ...其他全局配置...             server {
                                       listen 80;
    include /etc/nginx/conf.d/*.conf;  └── 这里被展开 ──┐
}                                                      │
                                                       ▼
                              http {
                                  server {            ← 你的配置在这里
                                      listen 80;
                                  }
                              }
```

**实践建议**：
- **一个站点一个文件**：项目叫 `myproject`，配置文件就叫 `/etc/nginx/conf.d/myproject.conf`，方便管理。
- **后缀必须是 `.conf`**：写 `.conf.bak` 或 `.txt` 都不会被加载。
- **多个 server 共存**：可以建多个 `.conf` 文件，每个一个网站，按 `server_name` 区分。

### 3.5 改完配置必做的两件事：nginx -t 与 reload（必会）

> 🏷️ ⭐ 开发必会 —— 不做这两步，配置等于白改，还可能把服务搞挂

**改配置的固定三步**：

```bash
# ① 测试语法：看到 syntax is ok / test is successful 才能继续
sudo nginx -t
# 输出：
# nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
# nginx: configuration file /etc/nginx/nginx.conf test is successful

# ② 平滑重载：不断开已有连接，重新读配置（用 reload 不是 restart）
sudo systemctl reload nginx

# ③ 验证：curl 自测或浏览器访问
curl http://127.0.0.1/
```

**`reload` vs `restart` 的区别（必会）**：

| 命令 | 行为 | 何时用 |
|------|------|-------|
| `systemctl reload nginx` | 平滑重载，旧连接处理完才切换，**不中断服务** | 改配置后，**90% 用这个** |
| `systemctl restart nginx` | 强制重启，瞬间断开所有连接 | 升级版本、改了 worker_processes 等需要重启的配置 |

> ⚠️ **常见坑**：改完配置不 `nginx -t` 直接 reload，万一语法错了 reload 会失败，但**老配置还在跑**，你以为生效了其实没生效。**永远先 `-t` 再 reload。**

> 🔑 **黄金三步**：`nginx -t` → `systemctl reload nginx` → `curl` 验证。每次改配置都走这三步，避免 90% 的配置问题。

---

## 4. 静态网站部署：把一个网页发出去（必会）

> 🏷️ ⭐ 开发必会 —— 最简单的 Nginx 用法，理解它就理解了 80% 的 Nginx 配置逻辑

### 4.1 🎯 作用说明

**这一节解决什么问题**：让你亲手把一个 HTML 文件通过 Nginx 发出去，浏览器访问 IP 能看到内容。这是所有 Nginx 用法的"最小单元"——理解了它，后面的 Vue3 部署、反向代理都是在它基础上加东西。

**适用场景**：

- 部署纯静态官网（公司主页、个人博客静态版）
- 测试 Nginx 是否正常工作
- 为后续 Vue3 部署打基础（Vue3 dist 本质就是一堆静态文件）

### 4.2 准备测试页面

> 🏷️ ⭐ 开发必会 —— 先有内容，再配置 Nginx

**第一步：建站点目录**

```bash
# 建一个放网页的目录（路径可自选，这里用 /www/demo）
sudo mkdir -p /www/demo

# 改所有者（让 nginx 用户能读，否则可能 403，11.4 节详讲）
sudo chown -R nginx:nginx /www/demo
```

> 📌 **目录命名建议**：用 `/www/` 作为所有站点根目录（不是 `/var/www/`，那是 Apache 的约定，Nginx 用 `/www/` 或 `/usr/share/nginx/html/` 都行，统一就好）。一个项目一个子目录：`/www/myproject/`、`/www/demo/`。

**第二步：写测试页面**

```bash
# 写一个简单的 HTML 页面（用 echo 重定向，比 vi 快）
sudo bash -c 'echo "<h1>Hello Nginx</h1>" > /www/demo/index.html'

# 验证文件创建成功
ls -l /www/demo/
# 输出：-rw-r--r-- 1 nginx nginx 23 ... index.html

# 看内容
cat /www/demo/index.html
# 输出：<h1>Hello Nginx</h1>
```

> ⚠️ **为什么用 `sudo bash -c '...'`？** 因为 `echo "..." > /file` 里的重定向 `>` 是当前用户权限，`sudo echo` 只 sudo 了 echo 本身，重定向还是当前用户权限。要么用 `sudo bash -c`，要么用 `sudo tee`，要么先 sudo vi 再写。

### 4.3 写最小站点配置

> 🏷️ ⭐ 开发必会 —— Nginx 配置的最小可用形态

**第一步：新建配置文件**

```bash
sudo vi /etc/nginx/conf.d/demo.conf
```

**第二步：写入以下内容（带逐行注释）**

```nginx
server {
    listen 80;                    # 监听 80 端口（HTTP 默认端口，所以访问时 URL 不用带端口）
    server_name 192.168.153.134; # 匹配哪个域名/IP；没有域名就写服务器 IP
    root /www/demo;               # 网站根目录：URL 对应的文件都从这个目录找
    index index.html;             # 访问"目录"时默认返回哪个文件
}
```

**第三步：黄金三步生效**

```bash
sudo nginx -t                    # ① 测语法
sudo systemctl reload nginx       # ② 重载
curl http://127.0.0.1/            # ③ 验证（返回 <h1>Hello Nginx</h1> 就成功）
```

**浏览器访问**：在 Windows 浏览器输入 `http://192.168.233.131/`，看到大字 "Hello Nginx" 就成功了。

**cking遇到的问题**:

如果这里报403的错误

**如果再次出现 403 Forbidden**：别慌，立刻执行 `sudo setenforce 0` 临时关闭 SELinux 再 `curl` 一次。如果通了，说明是 SELinux 拦截了 `/www` 目录。

解决办法

第二步:永久解决 SELinux 拦截（推荐，比直接关安全）

不要直接去关掉整个 SELinux，咱们只需要给 `/www` 目录打上一个“允许 Nginx 读取”的安全标签就行。执行这条命令：

```
sudo chcon -R -t httpd_sys_content_t /www
sudo systemctl reload nginx
```

然后再次 `curl http://127.0.0.1/`，应该就成功了。

*(注：如果你嫌麻烦，想彻底关掉 SELinux，可以改 `/etc/selinux/config` 把 `SELINUX=enforcing` 改成 `disabled`，然后重启虚拟机。但考试或生产环境，不推荐这样做。)*

🔍 第三步：如果第二步还不行，检查“普通文件权限”

打开你写的配置文件确认一下：

```
cat /etc/nginx/conf.d/demo.conf
```

- 如果里面写的是 `server_name 192.168.233.131;` 而你的真实 IP 是 `192.168.153.134`，Nginx 找不到匹配的 server，就会跳到默认服务器，返回 403。

- 检查目录和文件权限：

  ```
  ls -ld /www /www/demo
  ls -l /www/demo/index.html
  ```

  目录必须是 `755`（对应 `drwxr-xr-x`），文件必须是 `644`（对应 `-rw-r--r--`）。如果权限不对，执行：

  ```
  sudo chmod 755 /www /www/demo
  sudo chmod 644 /www/demo/index.html
  ```

💡 第四步：确认 Nginx 运行用户

如果上面都没问题，Nginx 进程是以什么身份运行的？
检查一下全局配置：

```
grep "^user" /etc/nginx/nginx.conf
```

通常输出是 `user nginx;`。而你之前已经 `chown -R nginx:nginx /www/demo` 了，匹配得上，没问题。

问题结束



### 4.4 root 与 index 指令详解（必会）

> 🏷️ ⭐ 开发必会 —— 两个最基础的指令，必须理解

**`root` 指令**：网站根目录

```nginx
root /www/demo;
```

- 访问 `http://IP/` → Nginx 拿 URL 路径 `/` → 拼到 root 后 → 找 `/www/demo/`
- 访问 `http://IP/a.css` → 找 `/www/demo/a.css`
- 访问 `http://IP/images/logo.png` → 找 `/www/demo/images/logo.png`

**`index` 指令**：访问目录时默认返回哪个文件

```nginx
index index.html;          # 访问 / 时返回 /www/demo/index.html
index index.html index.htm; # 可写多个，按顺序找，第一个存在就用
```

**完整请求处理流程（必记）**：

```
浏览器访问 http://192.168.153.134/
   │
   ▼
Nginx 收到请求，URL 路径 = /
   │
   ▼
用 root 拼接：/www/demo/ + / = /www/demo/
   │
   ▼
发现是目录，查 index 指令：index.html
   │
   ▼
最终文件：/www/demo/index.html
   │
   ▼
读取并返回给浏览器
```

### 4.5 server_name 匹配规则（必会）

> 🏷️ ⭐ 开发必会 —— 一个 Nginx 能跑多个网站，靠 server_name 区分

```nginx
server {
    listen 80;
    server_name www.aaa.com;    # 只匹配 www.aaa.com
    ...
}

server {
    listen 80;
    server_name www.bbb.com;    # 只匹配 www.bbb.com
    ...
}
```

**匹配规则优先级**（从高到低）：

| 写法 | 匹配方式 | 示例 |
|------|---------|------|
| 精确域名 | 完全匹配 | `server_name www.aaa.com;` |
| 通配符在前 | `*.aaa.com` 匹配 `www.aaa.com`、`api.aaa.com` | `server_name *.aaa.com;` |
| 通配符在后 | `www.aaa.*` 匹配 `www.aaa.com`、`www.aaa.cn` | `server_name www.aaa.*;` |
| 正则 | `~^www\d+\.aaa\.com$` | `server_name ~^www\d+\.aaa\.com$;` |
| `_`（默认） | 匹配所有没命中的请求（兜底） | `server_name _;` |

**没有域名时（开发阶段最常见）**：直接写服务器 IP

```nginx
server_name 192.168.153.134;
```

> ⚠️ **坑**：多个 server 都用 `server_name _;` 或都不写 server_name，Nginx 会用**第一个** server 作为默认站点，所有 IP 访问都进它。开发时多站点容易串，建议每站写明确 server_name。

### 4.6 location 匹配规则速查（必会）

> 🏷️ ⭐ 开发必会 —— 一个网站内按 URL 分流到不同处理方式

**语法**：

```nginx
location [修饰符] 路径 {
    处理规则
}
```

**修饰符优先级**（从高到低，必记）：

| 修饰符 | 含义 | 示例 | 匹配什么 |
|--------|------|------|---------|
| `=` | 精确匹配（最高优先级） | `location = /50x.html` | 只匹配 `/50x.html`，一字不差 |
| `^~` | 前缀匹配（不再走正则） | `location ^~ /static/` | `/static/` 开头的请求，不再检查正则 |
| `~` / `~*` | 正则匹配（区分/不区分大小写） | `location ~ \.php$` | 所有 `.php` 结尾的请求 |
| 无 | 普通前缀匹配（最低） | `location /api/` | `/api/` 开头的请求 |

**前缀匹配的"最长匹配"规则**：Nginx 会找最长匹配的 location，不是第一个匹配的。

```nginx
# 请求 /api/users/ 同时匹配下面两个，但选 /api/
location /api/ { ... }      # ← 命中这个（更长）
location / { ... }
```

**实战常用三件套**（Vue3 + Django 标配）：

```nginx
location / {              # ① 前端所有路径
    try_files $uri $uri/ /index.html;
}

location /api/ {          # ② 后端 API
    proxy_pass http://127.0.0.1:8000;
}

location /static/ {       # ③ Django 静态文件
    alias /www/myproject/backend/static/;
}
```

---

## 5. Vue3 前端部署（必会）

> 🏷️ ⭐ 开发必会 —— 前后端分离部署的第一块，Nginx 托管 Vue 打包产物

### 5.1 🎯 作用说明

**这一节解决什么问题**：把本地开发的 Vue3 项目打包成静态文件（dist），上传到服务器，用 Nginx 托管，让浏览器能访问到完整的前端页面。

**适用场景**：
- Vue3 + Element Plus / Ant Design Vue 等管理后台前端
- Vue3 企业官网、商城前端
- 任何 Vue3 SPA（单页应用）部署

**前置条件**：本地已有一个能跑的 Vue3 项目（用 Vite 或 vue-cli 创建的都行）。

### 5.2 第一步：本地打包 Vue3 项目

> 🏷️ ⭐ 开发必会 —— npm run build 产出 dist

**在本地（Windows）项目目录执行**：

```bash
# 进入项目根目录（package.json 所在目录）
cd D:\code\myproject-frontend

# 装依赖（首次或改了 package.json 后必做）
npm install

# 打包生产版本
npm run build
```

**这一步发生了什么**：
1. Vite/webpack 把 Vue 单文件组件编译成普通 HTML/CSS/JS
2. 把 import 的所有依赖打包合并、压缩、加哈希
3. 输出到 `dist/` 目录

**dist 目录结构**（典型）：

```
dist/
├── index.html                # 入口 HTML
├── assets/
│   ├── index-a1b2c3.js       # 主 JS（带哈希，避免缓存）
│   ├── index-d4e5f6.css      # 主 CSS
│   └── logo-7g8h9i.png       # 图片
└── favicon.ico
```

> ⚠️ **打包前必改的配置**：Vite 项目的 `vite.config.js` 要设 `base`：

```javascript
// vite.config.js
export default defineConfig({
  base: '/',    // 部署到根路径用 '/'；部署到子路径如 /app/ 用 '/app/'
  // ...
})
```

如果部署到 `http://IP/` 根路径，`base: '/'` 就行；如果部署到 `http://IP/app/`，必须改 `base: '/app/'`，否则资源 404。

### 5.3 第二步：上传 dist 到服务器

> 🏷️ ⭐ 开发必会 —— 三种方式任选其一

**方式一：用 scp 上传（命令行党推荐）**

```bash
# Windows PowerShell / Git Bash 都能用 scp
# 把本地 dist 目录上传到服务器 /www/myproject/frontend/
scp -r D:\code\myproject-frontend\dist root@192.168.233.131:/www/myproject/frontend/
```

**方式二：用 WinSCP / FileZilla 图形化上传**

1. 下载 WinSCP
2. 连接服务器（SFTP 协议，IP + 用户名 + 密码）
3. 左边本地、右边服务器，拖拽 `dist` 目录到 `/www/myproject/frontend/`

**方式三：宝塔面板上传**

1. 宝塔面板 → 文件 → 进入 `/www/myproject/frontend/`
2. 右上角"上传" → 选本地 dist 目录 → 上传

**服务器上验证**：

```bash
ls /www/myproject/frontend/dist/
# 应该看到 index.html、assets/ 等

# 确认 index.html 存在且非空
ls -lh /www/myproject/frontend/dist/index.html
```

> ⚠️ **权限问题（最常见）**：上传后文件所有者可能是 root，但 Nginx 用 nginx 用户运行，读不到会 403。改一下所有者：

```bash
sudo chown -R nginx:nginx /www/myproject/frontend/dist
# 或者更宽松：让所有人可读
sudo chmod -R 755 /www/myproject/frontend/dist
```

### 5.4 第三步：配置 Nginx 托管 dist

> 🏷️ ⭐ 开发必会 —— 把 root 指向 dist

**写配置**：

```bash
sudo vi /etc/nginx/conf.d/myproject.conf
```

**内容**（带逐行注释）：

```nginx
server {
    listen 80;                                    # 监听 80 端口
    server_name 192.168.233.131;                  # 服务器 IP（有域名写域名）

    # ★ 把 root 指向 Vue 打包产物的 dist 目录
    root /www/myproject/frontend/dist;
    index index.html;                             # 入口文件

    # ★ Vue Router history 模式必须的配置（下一节详讲）
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

**生效**：

```bash
sudo nginx -t
sudo systemctl reload nginx
```

### 5.5 第四步：try_files 解决 history 模式刷新 404（必会）

> 🏷️ ⭐ 开发必会 —— Vue Router history 模式部署最高频的坑，必须理解

**问题背景**：Vue3 项目用 Vue Router 的 history 模式时，路由是靠浏览器端的 JavaScript 拦截的。打包后只有**一个** `index.html`，页面跳转全靠浏览器端 JS。

**踩坑场景**：

1. 用户访问 `http://IP/` → Nginx 找 `/www/dist/index.html` → 返回 → Vue Router 接管 → 一切正常
2. 用户在 `/users/3` 页面**按 F5 刷新** → 浏览器请求 `http://IP/users/3` → Nginx 去找 `/www/dist/users/3` 这个文件 → **找不到，返回 404**

**解决**：`try_files` 指令告诉 Nginx "找不到文件就回退到 index.html"：

```nginx
location / {
    try_files $uri $uri/ /index.html;
}
```

**`try_files` 三个参数的含义**（按顺序尝试，第一个找到就用，找不到继续下一个）：

1. `$uri`：URL 路径本身（如 `/assets/index-a1b2.js`）→ dist 里有这个 js 文件就直接发
2. `$uri/`：当目录找（如果 URL 是 `/images/` 这种）
3. **`/index.html`：都找不到，返回首页**，剩下的路由交给 Vue Router 解析 → 刷新不再 404

**完整请求处理对比**：

| 请求 | 不加 try_files | 加 try_files |
|------|---------------|--------------|
| `/`（首页） | ✓ 找到 index.html | ✓ 找到 index.html |
| `/assets/a.js` | ✓ 找到文件 | ✓ 找到文件 |
| `/users/3`（Vue 路由） | ✗ **404** | ✓ 回退 index.html → Vue Router 接管 |

> 🔑 **核心记忆点**：`try_files $uri $uri/ /index.html;` 是前后端分离部署出镜率最高的一行配置，**Vue Router history 模式部署必加**。如果用 hash 模式（URL 带 `#`，如 `/#/users/3`）则不需要，但 hash 模式 URL 难看，生产基本用 history。

### 5.6 第五步：验证访问

```bash
# ① 本机 curl 验证
curl http://127.0.0.1/
# 应该返回 index.html 的内容（一堆 <div id="app">...</div>）

# ② curl 请求 Vue 路由（验证 try_files 生效）
curl http://127.0.0.1/users/3
# 也应该返回 index.html（不是 404），说明 try_files 工作了

# ③ 浏览器访问
# http://192.168.233.131/
# 看到你的 Vue 页面就成功
```

> ⚠️ **常见坑速查**：
> - **页面白屏**：打开浏览器 F12 → Network，看 js/css 是不是 404。99% 是 `vite.config.js` 的 `base` 没改对，或者 root 路径写错。
> - **F5 刷新 404**：try_files 那行没加，或者 location 写错。
> - **403 Forbidden**：dist 目录权限不对，nginx 用户读不到。`sudo chown -R nginx:nginx /www/myproject/` 解决。

### 5.7 宝塔面板对照操作

> 🏷️ ⭐ 开发必会 —— 宝塔党专属流程

**第一步：建站点**

1. 宝塔面板 → 网站 → 添加站点
2. 域名填服务器 IP（或域名）
3. 根目录填 `/www/myproject/frontend/dist`
4. PHP 版本选"纯静态"
5. 点击提交

**第二步：改配置**

1. 网站 → 站点设置 → 配置文件
2. 在 `location / { }` 里加 `try_files $uri $uri/ /index.html;`
3. 保存（宝塔会自动 `nginx -t` + reload）

**对比表**：

| 操作 | 命令行 | 宝塔 |
|------|-------|------|
| 新建站点 | 写 `.conf` 文件 | 网站 → 添加站点 |
| 改配置 | vi 编辑 conf | 站点设置 → 配置文件 |
| 生效 | `nginx -t` + reload | 保存自动生效 |
| 配置文件路径 | `/etc/nginx/conf.d/*.conf` | `/www/server/panel/vhost/nginx/*.conf` |

---

## 6. 反向代理：把 /api/ 交给 Django（必会）

> 🏷️ ⭐ 开发必会 —— Nginx 三大职责之二，前后端分离部署的桥梁

### 6.1 🎯 作用说明

**这一节解决什么问题**：Vue3 前端访问后端 API 时，浏览器发 `/api/users/`，Nginx 把这个请求"转发"给后端 Django（gunicorn），把 Django 返回的 JSON 再转回浏览器。

**适用场景**：
- 任何前后端分离项目（Vue + Django/Flask/Node/Java）
- 微服务网关（Nginx 按路径分发到不同后端服务）
- 负载均衡（一个 Nginx 把请求轮询分发给多个后端实例）

**为什么不直接让浏览器请求 Django**：
- Django（gunicorn）只监听 `127.0.0.1:8000`，公网访问不到
- 浏览器跨域限制：前端在 80 端口，后端在 8000 端口，是不同源，会有 CORS 问题
- 用 Nginx 反向代理后，前端和 API 都在 80 端口，**同源**，没有跨域问题

### 6.2 什么是反向代理（必会）

> 🏷️ ⭐ 开发必会 —— 区分正向代理和反向代理

**正向代理 vs 反向代理**：

| 类型 | 谁用 | 代理谁 | 客户端知道吗 |
|------|------|-------|------------|
| **正向代理** | 客户端 | 代理客户端去访问服务器 | 知道（如 VPN、SS） |
| **反向代理** | 服务器 | 代理服务器接收客户端请求 | **不知道**（如 Nginx） |

**类比**：
- **正向代理**：你找代购帮你买东西，代购是"正向代理"，你知道有代购这一层
- **反向代理**：你打 10086 客服，客服系统内部会自动转接到不同部门的接线员，你不知道转给谁了，你以为一直在跟 10086 说话——这个客服系统就是"反向代理"

**Nginx 反向代理的工作流程**：

```
浏览器请求 /api/users/
   │
   ▼
Nginx 收到，发现是 /api/ 开头
   │
   ▼
Nginx 把请求转发给 127.0.0.1:8000（gunicorn）
   │
   ▼
gunicorn 里的 Django 处理，返回 JSON
   │
   ▼
Nginx 收到 JSON，转手返回给浏览器
   │
   ▼
浏览器拿到 JSON，整个过程以为在跟 Nginx（80 端口）说话
```

### 6.3 proxy_pass 基本用法

> 🏷️ ⭐ 开发必会 —— 反向代理的核心指令

**最简配置**：

```nginx
location /api/ {
    proxy_pass http://127.0.0.1:8000;
}
```

**逐字段解释**：
- `location /api/`：匹配所有 `/api/` 开头的请求
- `proxy_pass`：转发目标，这里是本机 8000 端口的 gunicorn
- `127.0.0.1`：本机回环地址（gunicorn 和 Nginx 在同一台机器，走本机最快）
- `8000`：gunicorn 监听的端口

**完整反向代理配置（带 4 个 header）**：

```nginx
location /api/ {
    proxy_pass http://127.0.0.1:8000;                    # 转发目标

    proxy_set_header Host $host;                          # 把原始域名带给后端
    proxy_set_header X-Real-IP $remote_addr;              # 把访客真实 IP 带给后端
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;  # 代理链 IP
    proxy_set_header X-Forwarded-Proto $scheme;          # 告诉后端用户用的是 http/https
}
```

### 6.4 ⚠️ proxy_pass 带不带 / 的大坑（必会）

> 🏷️ ⭐ 开发必会 —— 面试高频，实战高频，必须搞清楚

**两种写法，效果天差地别**：

| 写法 | 效果（请求 `/api/users/`） |
|------|--------------------------|
| `proxy_pass http://127.0.0.1:8000;`（不带路径） | **保留 `/api/` 前缀**，Django 收到 `/api/users/` |
| `proxy_pass http://127.0.0.1:8000/;`（带 `/`） | **剥掉 `/api/` 前缀**，Django 收到 `/users/` |

**原理**：
- 不带 `/`：`proxy_pass` 后面只有协议+IP+端口，Nginx 把**原始 URL 完整**传给后端
- 带 `/`：`proxy_pass` 后面有路径（即使是根路径 `/`），Nginx 用这个路径**替换** location 匹配的部分

**详细举例**：

```nginx
# 写法 A：不带 /
location /api/ {
    proxy_pass http://127.0.0.1:8000;
}
# 请求 /api/users/ → Django 收到 /api/users/

# 写法 B：带 /
location /api/ {
    proxy_pass http://127.0.0.1:8000/;
}
# 请求 /api/users/ → Django 收到 /users/（剥掉了 /api/）
```

**什么时候用哪种**：

| Django 路由配置 | Nginx 写法 |
|---------------|-----------|
| `urlpatterns = [path('api/users/', users_view)]`（带 /api 前缀） | `proxy_pass http://127.0.0.1:8000;`（不带 /） |
| `urlpatterns = [path('users/', users_view)]`（不带 /api 前缀） | `proxy_pass http://127.0.0.1:8000/;`（带 /） |

> 📌 **本章统一用"不带 /"的写法**，因为 Django 的 `urls.py` 也会写 `path('api/users/', ...)` 前缀，前后端路径一致，最不容易乱。**第一次部署建议用这种**，避免脑子转不过来。

> 🔑 **为什么是 127.0.0.1 而不是服务器公网 IP？** gunicorn 和 Nginx 在同一台机器，走本机回环地址最快，且 gunicorn 只监听回环地址，公网摸不到它，更安全。

### 6.5 proxy_set_header 四个头的作用（必会）

> 🏷️ ⭐ 开发必会 —— 不加这 4 行，Django 拿不到真实 IP、HTTPS 信息

**为什么不加会有问题**：经过 Nginx 转发后，Django 看到的请求信息是"Nginx 发的"，而不是用户原始的。具体来说：

| 不加的 header | Django 看到的 | 实际应该是 |
|--------------|--------------|-----------|
| Host | `127.0.0.1:8000`（Nginx 转发目标） | 用户访问的域名/IP |
| X-Real-IP | `127.0.0.1`（Nginx 本机） | 用户真实 IP |
| X-Forwarded-Proto | `http`（Nginx 到 gunicorn 是 http） | 用户用的是 http 还是 https |

**4 个 header 的作用**：

```nginx
proxy_set_header Host $host;
# 作用：把用户访问的域名带给 Django
# 必要性：Django 的 ALLOWED_HOSTS 要靠它验证，否则 400 Bad Request

proxy_set_header X-Real-IP $remote_addr;
# 作用：把访客真实 IP 带给 Django
# 必要性：日志/风控/限流都需要真实 IP，否则日志全是 127.0.0.1

proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
# 作用：经过的代理链 IP（用户 IP + 中间所有代理 IP）
# 必要性：多层代理时溯源用户，单层代理时和 X-Real-IP 一样

proxy_set_header X-Forwarded-Proto $scheme;
# 作用：告诉 Django 用户用的是 http 还是 https
# 必要性：Django 做 HTTPS 重定向、生成 https URL 要靠它
```

**`$` 开头的都是 Nginx 变量**：
- `$host`：用户访问的域名（来自请求头 Host）
- `$remote_addr`：用户的真实 IP（TCP 连接对端 IP）
- `$proxy_add_x_forwarded_for`：自动拼接当前 XFF + 用户 IP
- `$scheme`：http 或 https

### 6.6 完整反向代理配置

> 🏷️ ⭐ 开发必会 —— Vue3 + Django 反向代理标准配置

```nginx
server {
    listen 80;
    server_name 192.168.233.131;

    root /www/myproject/frontend/dist;
    index index.html;

    # ① 前端：Vue Router history 模式
    location / {
        try_files $uri $uri/ /index.html;
    }

    # ② 后端 API：转发给 gunicorn（Django）
    location /api/ {
        proxy_pass http://127.0.0.1:8000;       # 不带 /，保留 /api/ 前缀
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # ③ Django admin 后台也要代理（如果用后台的话）
    location /admin/ {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

**生效并验证**：

```bash
sudo nginx -t
sudo systemctl reload nginx

# 验证反向代理工作（gunicorn 还没起会 502，这是正常的，第七节起 gunicorn）
curl http://127.0.0.1/api/users/
# 如果 gunicorn 没起：502 Bad Gateway（说明 Nginx 配置对，但后端没起）
# 如果 gunicorn 起了：返回 JSON 数据
```

> ⚠️ **502 是好事还是坏事？**：502 说明 Nginx 配置**对了**（请求转发到 8000 了），但 8000 端口的 gunicorn 没起。这是第七节要解决的问题。如果 curl 返回 404，说明 location 没匹配上，配置写错了。

---

## 7. Django 后端配置（必会）

> 🏷️ ⭐ 开发必会 —— 让 Django 在生产环境跑起来，第七节解决 502 问题

### 7.1 🎯 作用说明

**这一节解决什么问题**：把 Django 项目部署到服务器，用 gunicorn 跑起来作为后端，让 Nginx 反向代理能拿到响应。第七节是第六节反向代理的"另一半"——把后端服务跑起来。

**适用场景**：
- 任何 Django 项目生产部署
- 配合 Nginx 反向代理的标准 Django 部署

**前置条件**：
- 已安装 Python 3（CentOS 7 自带 Python 3.6，够用；要更高版本用 anaconda）
- 已有 Django 项目代码（本地能跑）

### 7.2 第一步：准备 Django 项目结构

> 🏷️ ⭐ 开发必会 —— 统一目录结构，方便后续配置

**推荐目录结构**：

```
/www/myproject/
├── frontend/
│   └── dist/                # Vue3 打包产物（第五节已部署）
├── backend/
│   ├── myproject/           # Django 项目根目录（manage.py 所在）
│   │   ├── manage.py
│   │   ├── myproject/       # settings.py 所在的包
│   │   │   └── settings.py
│   │   ├── apps/            # 业务 app
│   │   └── requirements.txt # 依赖清单
│   ├── static/              # collectstatic 收集的静态文件
│   ├── media/               # 用户上传文件
│   ├── venv/                # Python 虚拟环境（可选）
│   └── gunicorn_conf.py     # gunicorn 配置文件
└── logs/                    # 日志目录
```

**第一步：建目录并上传代码**

```bash
# 在服务器上建项目目录
sudo mkdir -p /www/myproject/{frontend,backend/static,backend/media,logs}

# 改所有者（让当前用户 lwc 能写，nginx 能读）
sudo chown -R lwc:nginx /www/myproject
sudo chmod -R 750 /www/myproject

# 上传 Django 代码到 /www/myproject/backend/
# 用 scp：
scp -r D:\code\myproject-backend\myproject lwc@192.168.233.131:/www/myproject/backend/
```

**第二步：装 Python 依赖**

```bash
# 进入项目目录
cd /www/myproject/backend

# 创建虚拟环境（推荐，避免污染系统 Python）
python3 -m venv venv

# 激活虚拟环境
source venv/bin/activate

# 升级 pip（国内源，避免超时）
pip install --upgrade pip -i https://mirrors.aliyun.com/pypi/simple/

# 装依赖
pip install -r myproject/requirements.txt -i https://mirrors.aliyun.com/pypi/simple/

# 装 gunicorn 和 mysqlclient（生产必备）
pip install gunicorn mysqlclient -i https://mirrors.aliyun.com/pypi/simple/
```

> ⚠️ **mysqlclient 安装失败的坑**：需要系统级 MySQL 开发库，先装：
> ```bash
> sudo yum install -y mysql-devel python3-devel gcc
> ```
> 如果还装不上，可以改用 PyMySQL（纯 Python，不需要开发库），在 settings.py 同级目录的 `__init__.py` 加：
> ```python
> import pymysql
> pymysql.install_as_MySQLdb()
> ```

### 7.3 第二步：settings.py 生产配置（必会）

> 🏷️ ⭐ 开发必会 —— 开发和生产配置不一样，必须改

**打开 settings.py**：

```bash
vi /www/myproject/backend/myproject/myproject/settings.py
```

**必改的 6 个配置**（带逐行注释）：

```python
# ① 关闭 DEBUG（生产必须 False，否则报错页面会泄露源码）
DEBUG = False

# ② 允许访问的 Host（写服务器 IP 和域名，* 不安全）
ALLOWED_HOSTS = ['192.168.233.131', 'your-domain.com', '127.0.0.1']

# ③ 静态文件配置
STATIC_URL = '/static/'
STATIC_ROOT = '/www/myproject/backend/static/'   # collectstatic 收集到这里
# STATIC_ROOT 是生产环境独有的，开发用 STATICFILES_DIRS

# 媒体文件（用户上传的）
MEDIA_URL = '/media/'
MEDIA_ROOT = '/www/myproject/backend/media/'

# ④ 数据库改用 MySQL（第八节详讲）
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'myproject',          # 数据库名
        'USER': 'djangouser',         # 数据库用户名
        'PASSWORD': 'YourStrongPwd123!',
        'HOST': '127.0.0.1',          # MySQL 在本机
        'PORT': '3306',
    }
}

# ⑤ CORS 配置（如果前后端不同源才需要，同源不需要）
# 装依赖：pip install django-cors-headers
INSTALLED_APPS += ['corsheaders']
MIDDLEWARE = ['corsheaders.middleware.CorsMiddleware'] + MIDDLEWARE
CORS_ALLOWED_ORIGINS = [
    'http://192.168.233.131',
    'http://your-domain.com',
]
# 如果前后端同源（同一个 Nginx，同一个端口），不用配 CORS

# ⑥ 安全相关配置（HTTPS 部署时打开）
SESSION_COOKIE_SECURE = True       # 让 cookie 只走 HTTPS 传输
CSRF_COOKIE_SECURE = True
# 部署 HTTPS 时打开，纯 HTTP 部署保持 False，否则登录不上去
```

> ⚠️ **`DEBUG = False` 的坑**：
> - 改成 False 后，静态文件（admin 样式、CSS）Django **不主动发了**，必须用 Nginx 发（7.7 节配置），否则 admin 后台丑且没有样式。
> - 报错页面变成"Server Error (500)"，不显示详细错误，要查日志才知道。
> - `ALLOWED_HOSTS` 必须包含访问的 IP/域名，否则返回 400 Bad Request。

### 7.4 第三步：collectstatic 收集静态文件（必会）

> 🏷️ ⭐ 开发必会 —— 把 admin 样式、第三方 app 静态文件统一收集到一个目录

**为什么要 collectstatic**：
- 开发时 Django 自动发静态文件（DEBUG=True）
- 生产时 DEBUG=False，Django 不发了，要交给 Nginx 直接发
- 但 Django 的 admin 样式、第三方 app 的静态文件散落在各处，Nginx 不可能一个个找
- `collectstatic` 把所有静态文件**收集到 STATIC_ROOT**，Nginx 指向这一个目录就行

**执行**：

```bash
cd /www/myproject/backend
source venv/bin/activate           # 激活虚拟环境

# 收集静态文件到 STATIC_ROOT
python myproject/manage.py collectstatic --noinput

# 看输出
# 输出类似：Copied 130 static files to '/www/myproject/backend/static'.

# 验证
ls /www/myproject/backend/static/
# 应该看到 admin/、rest_framework/ 等目录

# 改所有者（让 nginx 能读）
sudo chown -R nginx:nginx /www/myproject/backend/static
```

### 7.5 第四步：安装并启动 gunicorn（必会）

> 🏷️ ⭐ 开发必会 —— Django 的生产服务器

**为什么不用 `runserver`**：
- `python manage.py runserver` 是开发调试用的，**单线程**、性能差、有热重载（不稳定）
- 生产环境必须用 gunicorn/uwsgi：多 worker、稳定、性能高

**手动启动测试**：

```bash
cd /www/myproject/backend
source venv/bin/activate

# 手动启动 gunicorn 测试（前台跑，Ctrl+C 退出）
gunicorn myproject.wsgi:application --bind 127.0.0.1:8000 --workers 3

# 参数解释：
# myproject.wsgi:application  Django 的 wsgi 模块路径（项目名.wsgi:application）
# --bind 127.0.0.1:8000       监听本机 8000（只本机能访问，公网摸不到）
# --workers 3                  开 3 个 worker 进程（一般 CPU 核数 * 2 + 1）

# 另开一个终端验证
curl http://127.0.0.1:8000/api/users/
# 返回 JSON 就成功（Ctrl+C 关掉 gunicorn，下一步用 systemd 管理）
```

> 📌 **worker 数怎么定**：经验公式 `CPU 核数 * 2 + 1`。比如 2 核服务器用 5 个 worker，4 核用 9 个。worker 太多反而抢资源变慢。

### 7.6 第五步：用 systemd 管理 gunicorn（必会）

> 🏷️ ⭐ 开发必会 —— 让 gunicorn 开机自启、崩了自动重启

**手动启动 gunicorn 的两大问题**：
1. SSH 断开 gunicorn 就死
2. 崩了没人重启

**用 systemd 解决**：systemd 是 CentOS 7 的服务管理器（第三章 5 节讲过），把 gunicorn 注册成系统服务，开机自启 + 崩溃自动重启。

**第一步：创建 service 文件**

```bash
sudo vi /etc/systemd/system/gunicorn.service
```

**内容**（带逐行注释）：

```ini
[Unit]
Description=gunicorn daemon for myproject       # 服务描述
After=network.target mysql.service              # 在 network 和 mysql 之后启动

[Service]
Type=notify                                     # systemd 通知模式（gunicorn 19.9+ 支持）
User=lwc                                        # 以哪个用户跑（不要用 root，安全）
Group=nginx                                     # 组设为 nginx，方便读 dist/static
WorkingDirectory=/www/myproject/backend/myproject  # 工作目录（manage.py 所在）
EnvironmentFile=/www/myproject/backend/gunicorn.env  # 环境变量文件（可选）
ExecStart=/www/myproject/backend/venv/bin/gunicorn \
          myproject.wsgi:application \
          --bind 127.0.0.1:8000 \
          --workers 3 \
          --timeout 120 \
          --access-logfile /www/myproject/logs/gunicorn_access.log \
          --error-logfile /www/myproject/logs/gunicorn_error.log
ExecReload=/bin/kill -s HUP $MAINPID            # 重载命令
KillMode=mixed
TimeoutStopSec=5                                # 停止超时
PrivateTmp=true

[Install]
WantedBy=multi-user.target                      # 开机自启目标
```

**可选：环境变量文件**

```bash
# 如果 settings.py 用 os.environ 读敏感配置（密码、密钥），建这个文件
vi /www/myproject/backend/gunicorn.env
# 内容（按需）：
DJANGO_SETTINGS_MODULE=myproject.settings
SECRET_KEY=your-secret-key-here
DB_PASSWORD=YourStrongPwd123!
```

**第二步：启动并设置开机自启**

```bash
# 让 systemd 重新加载配置文件（新建或改 service 文件后必做）
sudo systemctl daemon-reload

# 启动并设开机自启
sudo systemctl enable --now gunicorn

# 查看状态
sudo systemctl status gunicorn
# 看到 active (running) 就成功

# 测试反向代理是否通
curl http://127.0.0.1/api/users/
# 这次应该返回 JSON，不再是 502
```

**常用管理命令**：

```bash
sudo systemctl restart gunicorn    # 改了代码后重启
sudo systemctl status gunicorn     # 看状态
sudo systemctl stop gunicorn       # 停止
journalctl -u gunicorn -f          # 实时看日志（systemd 服务日志）
```

> ⚠️ **常见坑**：
> - **改了代码不生效**：必须 `sudo systemctl restart gunicorn`，gunicorn 不会热重载（生产不要热重载，怕半截代码上线）
> - **服务起不来**：先 `journalctl -u gunicorn -n 50 --no-pager` 看错误，99% 是路径错、虚拟环境没激活、依赖没装、settings.py 报错

### 7.7 第六步：Nginx 代理 Django + 静态文件（必会）

> 🏷️ ⭐ 开发必会 —— 完整 Nginx 配置，前端 + 后端 + 静态文件三合一

**完整配置**（带逐行注释）：

```bash
sudo vi /etc/nginx/conf.d/myproject.conf
```

```nginx
server {
    listen 80;
    server_name 192.168.233.131 your-domain.com;   # IP 和域名都写上，空格分隔
    charset utf-8;                                 # 响应头声明 UTF-8，防中文乱码

    # ① Vue3 前端静态文件
    root /www/myproject/frontend/dist;
    index index.html;

    # ② Vue Router history 模式：找不到文件一律回退到首页
    location / {
        try_files $uri $uri/ /index.html;
    }

    # ③ 后端 API：转发给 gunicorn（不带 /，保留 /api/ 前缀）
    location /api/ {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # ④ Django admin 后台（也要代理，否则访问不到）
    location /admin/ {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # ⑤ Django 静态文件（collectstatic 收集的，admin 样式等）
    location /static/ {
        alias /www/myproject/backend/static/;   # 用 alias 替换 /static/
        expires 30d;                             # 浏览器缓存 30 天
        access_log off;                          # 静态请求不写日志，减少噪音
    }

    # ⑥ 用户上传的媒体文件
    location /media/ {
        alias /www/myproject/backend/media/;
    }

    # ⑦ 其他高频配置
    client_max_body_size 50M;    # 允许上传的最大文件（默认 1M，上传图片必调）

    access_log /www/myproject/logs/nginx_access.log;   # 访问日志
    error_log  /www/myproject/logs/nginx_error.log;    # 错误日志（502/403 先看它）

    gzip on;                                    # 开启压缩，js/css 体积降 70%
    gzip_types text/css application/javascript application/json;
    gzip_min_length 1k;                         # 小于 1k 不压
}
```

**生效并完整测试**：

```bash
sudo nginx -t
sudo systemctl reload nginx

# 完整测试矩阵
curl http://127.0.0.1/                          # 前端首页（index.html）
curl http://127.0.0.1/users/3                  # Vue 路由（应返回 index.html）
curl http://127.0.0.1/api/users/               # 后端 API（应返回 JSON）
curl http://127.0.0.1/admin/                   # Django admin（应返回登录页）
curl http://127.0.0.1/static/admin/css/base.css # Django admin 样式（应返回 CSS）
```

### 7.8 root 与 alias 的区别（必会）

> 🏷️ ⭐ 开发必会 —— 面试高频，配置必懂

**核心区别**：

| 指令 | 行为 | 示例 |
|------|------|------|
| `root` | **拼接** URL 路径到 root 后面 | `root /www/static;` + 访问 `/static/a.css` → 找 `/www/static/static/a.css` |
| `alias` | **替换** location 匹配部分 | `alias /www/static/;` + 访问 `/static/a.css` → 找 `/www/static/a.css` |

**详细举例**：

```nginx
# 写法 A：root
location /static/ {
    root /www/myproject/backend;   # 注意：root 指向 backend 目录
}
# 访问 /static/admin/css/base.css
# → Nginx 拼接：/www/myproject/backend + /static/admin/css/base.css
# → 实际找：/www/myproject/backend/static/admin/css/base.css

# 写法 B：alias（推荐）
location /static/ {
    alias /www/myproject/backend/static/;   # 注意：alias 指向 static 目录，结尾有 /
}
# 访问 /static/admin/css/base.css
# → Nginx 替换：用 alias 路径替换 /static/
# → 实际找：/www/myproject/backend/static/admin/css/base.css
```

**记忆口诀**：
- **location 路径和磁盘目录同名用 root**：`location /static/` + `root /www/backend;`（实际目录是 `/www/backend/static/`）
- **location 路径和磁盘目录不同名用 alias**：`location /static/` + `alias /www/backend/staticfiles/;`
- **alias 结尾的 `/` 不要漏**：`alias /www/static;`（漏了）vs `alias /www/static/;`（对），漏了会导致拼接错误

> 🔑 **本章推荐用 alias**，因为 `location /static/` 直接替换为 `alias /www/.../static/;`，路径对应关系最清晰。

---

## 8. MySQL 集成（必会）

> 🏷️ ⭐ 开发必会 —— 数据库是后端的存储层，Django 配 MySQL 是企业级标配

### 8.1 🎯 作用说明

**这一节解决什么问题**：让 Django 连上 MySQL，存取数据。SQLite 是开发用的玩具，生产环境必须用 MySQL/PostgreSQL。

**适用场景**：
- 任何 Django 生产部署
- 数据量较大、并发较高的项目

**为什么不直接用 SQLite**：
- SQLite 是单文件数据库，并发写入会锁表，性能差
- MySQL 是 C/S 架构，支持多连接、多并发、事务、复制
- 企业部署基本都用 MySQL

### 8.2 第一步：安装 MySQL（速引第三章）

> 🏷️ 📦 环境搭建 —— 详细步骤见第三章 10 节，这里只做速引

```bash
# 装 MySQL 8.0 官方源（CentOS 7 EOL 后必须先修源，第二章 2.2 节讲过）
sudo rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-7.noarch.rpm

# 装 MySQL Server
sudo yum install -y mysql-community-server

# 启动并设开机自启
sudo systemctl enable --now mysqld

# 查看初始密码
sudo grep 'temporary password' /var/log/mysqld.log
# 输出：A temporary password is generated for root@localhost: xxxxxx

# 用初始密码登录
mysql -u root -p
# 粘贴上面的临时密码

# 改 root 密码（MySQL 8 密码策略要求大小写+数字+符号，至少 8 位）
ALTER USER 'root'@'localhost' IDENTIFIED BY 'YourRootPwd123!';
```

> 📌 **详细安装步骤见第三章 10.3 节**，这里只列关键命令。装好 MySQL 并能 `mysql -u root -p` 登录后继续。

### 8.3 第二步：建库建用户授权（必会）

> 🏷️ ⭐ 开发必会 —— 给 Django 单独建库建用户，不要用 root

**为什么要单独建用户**：
- root 权限太大，Django 用它连库不安全
- 一个项目一个数据库 + 一个用户，互不影响

**登录 MySQL 执行 SQL**：

```bash
mysql -u root -p
# 输入 root 密码
```

```sql
-- ① 建数据库（指定 utf8mb4 字符集，支持 emoji）
CREATE DATABASE myproject CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

-- ② 建用户（限制只能从本机 127.0.0.1 登录，更安全）
CREATE USER 'djangouser'@'127.0.0.1' IDENTIFIED BY 'YourStrongPwd123!';

-- ③ 授权（只给 myproject 库的所有权限，不给其他库）
GRANT ALL PRIVILEGES ON myproject.* TO 'djangouser'@'127.0.0.1';

-- ④ 刷新权限（必须，否则授权不生效）
FLUSH PRIVILEGES;

-- ⑤ 验证
SHOW DATABASES;
SELECT user, host FROM mysql.user WHERE user='djangouser';
-- 看到 djangouser 127.0.0.1 就成功

-- 退出
EXIT;
```

**字段解释**：
- `'djangouser'@'127.0.0.1'`：用户名@允许登录的 IP，`127.0.0.1` 表示只能本机连
- `IDENTIFIED BY '密码'`：设置密码
- `GRANT ALL PRIVILEGES ON myproject.*`：给 myproject 库的所有表授权
- `FLUSH PRIVILEGES`：让授权立即生效（必须执行）

**测试新用户能登录**：

```bash
mysql -u djangouser -p
# 输入 YourStrongPwd123!
# 在 mysql> 提示符下：
SHOW DATABASES;
# 应该看到 myproject 库
USE myproject;
EXIT;
```

### 8.4 第三步：Django 配置 MySQL（必会）

> 🏷️ ⭐ 开发必会 —— 改 settings.py 的 DATABASES 配置

**编辑 settings.py**：

```bash
vi /www/myproject/backend/myproject/myproject/settings.py
```

**找到 DATABASES 配置改成 MySQL**：

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',   # 引擎改成 mysql
        'NAME': 'myproject',                      # 数据库名（上一步建的）
        'USER': 'djangouser',                     # 用户名（上一步建的）
        'PASSWORD': 'YourStrongPwd123!',          # 密码
        'HOST': '127.0.0.1',                      # MySQL 地址（本机）
        'PORT': '3306',                           # 端口（默认 3306）
        'OPTIONS': {
            'charset': 'utf8mb4',                 # 字符集，支持 emoji
        },
    }
}
```

**密码硬编码的安全做法**（生产推荐）：从环境变量读，不写在代码里

```python
import os

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': os.environ.get('DB_NAME', 'myproject'),
        'USER': os.environ.get('DB_USER', 'djangouser'),
        'PASSWORD': os.environ.get('DB_PASSWORD', 'default-fallback'),
        'HOST': os.environ.get('DB_HOST', '127.0.0.1'),
        'PORT': os.environ.get('DB_PORT', '3306'),
    }
}
```

环境变量写到 7.6 节创建的 `gunicorn.env` 文件里，systemd 启动时自动加载。

### 8.5 第四步：数据迁移 makemigrations/migrate（必会）

> 🏷️ ⭐ 开发必会 —— 把 Django 模型同步到 MySQL，建表

**两个命令的区别**：

| 命令 | 作用 | 何时用 |
|------|------|-------|
| `makemigrations` | 把模型变更生成迁移文件（Python 代码） | 改了 models.py 后 |
| `migrate` | 执行迁移文件，真正改数据库 | 部署时 / 改模型后 |

**执行**：

```bash
cd /www/myproject/backend
source venv/bin/activate

# ① 生成迁移文件（改了 models.py 后执行）
python myproject/manage.py makemigrations

# ② 执行迁移（建表、改表）
python myproject/manage.py migrate

# 看到 Applying xxx... OK 就成功
```

**验证表建好了**：

```bash
mysql -u djangouser -p
USE myproject;
SHOW TABLES;
# 应该看到 django_migrations、auth_user、django_session 等表
EXIT;
```

**创建超级用户**（admin 后台登录用）：

```bash
python myproject/manage.py createsuperuser
# 输入用户名、邮箱、密码
```

### 8.6 第五步：防火墙与端口策略（必会）

> 🏷️ ⭐ 开发必会 —— 安全铁律：MySQL 端口不对公网开放

**为什么 3306 不对外开放**：
- Nginx 和 Django 在本机访问 MySQL，走 127.0.0.1，不需要公网
- 公网开放 3306 = 给黑客扫数据库漏洞的机会
- MySQL 默认监听 127.0.0.1，但要检查配置

**第一步：检查 MySQL 监听地址**

```bash
sudo ss -tlnp | grep 3306
# 看输出：
# 127.0.0.1:3306    ← 只监听本机，安全
# 0.0.0.0:3306      ← 监听所有地址，危险！要改

# 如果监听 0.0.0.0，改配置：
sudo vi /etc/my.cnf
# 在 [mysqld] 下加：
# bind-address = 127.0.0.1
sudo systemctl restart mysqld
```

**第二步：防火墙不放行 3306**

```bash
# 检查防火墙当前规则
sudo firewall-cmd --list-all

# 不要执行（除非你真的需要远程连数据库，且用 SSH 隧道更安全）：
# sudo firewall-cmd --permanent --add-service=mysql
# sudo firewall-cmd --reload
```

**端口策略总结**：

| 端口 | 服务 | 是否对公网开放 | 原因 |
|------|------|--------------|------|
| 80 | Nginx (HTTP) | ✓ 开放 | 用户访问 |
| 443 | Nginx (HTTPS) | ✓ 开放 | 用户访问 |
| 8000 | gunicorn | ✗ 关闭 | 只本机访问，Nginx 转发 |
| 3306 | MySQL | ✗ 关闭 | 只本机访问，Django 直连 |

> 🔑 **安全铁律**：公网只开 80 和 443，其他端口（8000/3306）一律不对公网开放。需要远程管理用 SSH 隧道，不用直接开端口。

---

## 9. 完整部署流程清单（必会）

> 🏷️ ⭐ 开发必会 —— 把前面所有步骤串起来，从零到上线的完整流程

### 9.1 🎯 作用说明

**这一节解决什么问题**：把第 2-8 节零散的步骤串成一个完整的部署流程，照着这一节走就能把项目从零部署上线。这是本章的"汇总节"，部署时直接对照着做。

**适用场景**：
- 新项目首次部署
- 换服务器迁移部署
- 部署清单参考

### 9.2 部署架构总览图

> 🏷️ ⭐ 开发必会 —— 一张图回顾全章

```
┌──────────────┐
│   浏览器      │  http://IP/  或  http://IP/api/users/
└──────┬───────┘
       │ 80/443
       ▼
┌──────────────────────────────────────────────┐
│  Nginx（80 端口）—— 全站唯一对外的大门         │
│  职责①：托管 Vue3 dist（location /）          │
│  职责②：转发 /api/ 给 gunicorn（location /api/）│
│  职责③：发 Django 静态文件（location /static/）│
└──────┬───────────────────────┬───────────────┘
       │ /                     │ /api/、/admin/
       ▼                       ▼
┌──────────────────┐   ┌────────────────────────┐
│ dist/ 目录        │   │ gunicorn（127.0.0.1:8000）│
│ index.html/js/css│   │ systemd 管理，开机自启      │
└──────────────────┘   └───────────┬────────────┘
                                   │ 127.0.0.1:3306
                                   ▼
                          ┌─────────────────┐
                          │ MySQL（3306）    │
                          │ 库 myproject      │
                          │ 用户 djangouser    │
                          └─────────────────┘
```

### 9.3 完整 Nginx 配置模板（Vue3 + Django + 静态/媒体文件）

> 🏷️ ⭐ 开发必会 —— 直接改路径就能用的生产配置

```bash
sudo vi /etc/nginx/conf.d/myproject.conf
```

```nginx
# ============================================================
# Vue3 + Django + MySQL 完整 Nginx 配置
# 路径要改成你自己的项目路径
# ============================================================
server {
    listen 80;
    server_name 192.168.233.131 your-domain.com;   # ★ 改成你的 IP/域名
    charset utf-8;

    # ---------- ① 前端 Vue3 ----------
    root /www/myproject/frontend/dist;             # ★ 改成你的 dist 路径
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;          # Vue Router history 模式
    }

    # ---------- ② 后端 API ----------
    location /api/ {
        proxy_pass http://127.0.0.1:8000;          # 转发给 gunicorn（不带 /，保留 /api/ 前缀）
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # ---------- ③ Django admin 后台 ----------
    location /admin/ {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # ---------- ④ Django 静态文件 ----------
    location /static/ {
        alias /www/myproject/backend/static/;      # ★ collectstatic 收集目录
        expires 30d;
        access_log off;
    }

    # ---------- ⑤ 用户上传文件 ----------
    location /media/ {
        alias /www/myproject/backend/media/;
    }

    # ---------- ⑥ 其他配置 ----------
    client_max_body_size 50M;                       # 上传文件大小限制

    access_log /www/myproject/logs/nginx_access.log;
    error_log  /www/myproject/logs/nginx_error.log;

    gzip on;
    gzip_types text/css application/javascript application/json;
    gzip_min_length 1k;
}
```

### 9.4 从零到上线 12 步清单

> 🏷️ ⭐ 开发必会 —— 部署时对照着勾选

| 步骤 | 操作 | 命令/位置 | 验证 |
|------|------|----------|------|
| 1 | 修 CentOS 7 源 | 2.2 节命令 | `yum makecache` 不报错 |
| 2 | 装 Nginx + 启动 | `yum install -y nginx` + `systemctl enable --now nginx` | `curl 127.0.0.1` 看到 Nginx 欢迎页 |
| 3 | 装 MySQL + 启动 | 见 8.2 节 | `mysql -u root -p` 能登录 |
| 4 | 建库建用户 | 8.3 节 SQL | `SHOW DATABASES` 看到 myproject |
| 5 | 装 Python 依赖 | `pip install -r requirements.txt` + `gunicorn` + `mysqlclient` | `import gunicorn` 不报错 |
| 6 | 改 settings.py | 7.3 节 6 项配置 | `python manage.py check` 不报错 |
| 7 | collectstatic | `python manage.py collectstatic --noinput` | `ls backend/static/` 有 admin/ 等 |
| 8 | 数据迁移 | `python manage.py migrate` | MySQL `SHOW TABLES` 有 django_session 等 |
| 9 | 创建 gunicorn service | 7.6 节 | `systemctl status gunicorn` active |
| 10 | 写 Nginx 配置 | 9.3 节模板 | `nginx -t` 通过 |
| 11 | reload Nginx | `systemctl reload nginx` | 浏览器访问看到页面 |
| 12 | 防火墙放行 80/443 | `firewall-cmd --permanent --add-service={http,https} --reload` | 外网浏览器能访问 |

### 9.5 域名与 IP 配置说明

> 🏷️ ⭐ 开发必会 —— 有域名和没域名配置不同

**情况一：只有 IP（开发/测试环境）**

```nginx
server_name 192.168.233.131;
```

**情况二：有域名（生产环境）**

```nginx
server_name your-domain.com www.your-domain.com;
```

**情况三：一个 Nginx 跑多个项目（按域名区分）**

```nginx
# 项目 A
server {
    listen 80;
    server_name aaa.com;
    root /www/projectA/frontend/dist;
    location /api/ { proxy_pass http://127.0.0.1:8001; }
    ...
}

# 项目 B
server {
    listen 80;
    server_name bbb.com;
    root /www/projectB/frontend/dist;
    location /api/ { proxy_pass http://127.0.0.1:8002; }
    ...
}
```

> 📌 **域名解析**：在域名服务商（阿里云/腾讯云）的 DNS 设置里加 A 记录，把域名指向服务器公网 IP。改完 DNS 生效要 10 分钟到几小时不等。

### 9.6 宝塔面板对照操作

> 🏷️ ⭐ 开发必会 —— 宝塔党部署流程

| 步骤 | 命令行 | 宝塔操作 |
|------|-------|---------|
| 装 Nginx | `yum install nginx` | 软件商店 → 安装 Nginx |
| 装 MySQL | 8.2 节命令 | 软件商店 → 安装 MySQL |
| 建 MySQL 库 | `mysql -e "CREATE DATABASE..."` | 数据库 → 添加数据库 |
| 上传前端 dist | scp | 文件 → 上传 |
| 上传后端代码 | scp | 文件 → 上传 |
| 配 Nginx | vi conf.d/xxx.conf | 网站 → 添加站点 → 配置文件 |
| 起后端 | systemd + gunicorn.service | Python 项目管理器（宝塔插件） |
| 防火墙 | firewall-cmd | 安全 → 放行端口 |
| SSL 证书 | 10.4 节 certbot | 网站 → SSL → Let's Encrypt |

> 📌 **宝塔部署 Django 后端**：宝塔有"Python 项目管理器"插件，能图形化管理 gunicorn，类似 systemd 但可视化。装这个插件比手写 service 文件简单。

---

## 10. HTTPS 配置（必会）

> 🏷️ ⭐ 开发必会 —— 生产环境标配，浏览器都标"不安全"了，必须会配 HTTPS

### 10.1 🎯 作用说明

**这一节解决什么问题**：给 Nginx 配置 HTTPS，让浏览器地址栏显示绿色小锁，传输加密防窃听。生产环境必配。

**适用场景**：
- 任何面向用户的生产环境
- 微信小程序、App 后台 API（强制 HTTPS）
- 涉及登录、支付的项目（强制 HTTPS）

**HTTPS 解决的问题**：
- **加密**：传输内容加密，HTTP 抓包看不到明文
- **认证**：证书证明网站身份，防钓鱼
- **完整性**：传输过程防篡改

### 10.2 HTTPS 与证书概念（了解）

> 🏷️ 📖 概念了解 —— 读懂即可

**HTTPS = HTTP + TLS/SSL**：在 HTTP 下面加了一层加密。

**证书的作用**：相当于网站的身份证，由权威机构（CA）签发。浏览器内置了主流 CA 的公钥，能验证证书真伪。

**证书类型对比**：

| 类型 | 价格 | 申请速度 | 推荐场景 |
|------|------|---------|---------|
| **自签证书** | 免费 | 即时 | 内网测试、开发环境 |
| **Let's Encrypt** | 免费 | 1 分钟 | 公网域名、生产环境 |
| 商业 DV 证书 | 几十~几百元/年 | 10 分钟 | 商业项目 |
| 商业 OV/EV 证书 | 几千~几万/年 | 几天 | 银行、政企 |

> 📌 **本章只讲自签和 Let's Encrypt 两种**，开发基本够用。

### 10.3 方式一：自签证书（内网测试用）

> 🏷️ 📦 环境搭建 —— 内网/IP 访问用，浏览器会标"不安全"但能用

**生成自签证书**：

```bash
# 建证书存放目录
sudo mkdir -p /etc/nginx/ssl

# 生成私钥和证书（一条命令搞定）
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/nginx/ssl/selfsigned.key \
    -out /etc/nginx/ssl/selfsigned.crt \
    -subj "/C=CN/ST=Shanghai/L=Shanghai/O=MyCompany/CN=192.168.233.131"

# 参数解释：
# -x509    生成自签证书（不是 CSR 请求）
# -nodes   私钥不加密（启动 nginx 不用输密码）
# -days 365  有效期 365 天
# -newkey rsa:2048  生成 2048 位 RSA 私钥
# -keyout  私钥输出路径
# -out     证书输出路径
# -subj    证书主题（CN 填 IP 或域名）

# 验证
ls -l /etc/nginx/ssl/
# 应该看到 selfsigned.crt 和 selfsigned.key
```

> ⚠️ **自签证书的局限**：浏览器不信任自签证书，访问时会红框警告"不安全"，需手动点"继续访问"。生产环境必须用 Let's Encrypt 或商业证书。

### 10.4 方式二：Let's Encrypt 免费证书（公网域名）

> 🏷️ ⭐ 开发必会 —— 公网域名首选，免费、自动续期、浏览器信任

**前置条件**：
- 有公网域名（如 your-domain.com）
- 域名已解析到服务器公网 IP
- 80 端口已开放（申请证书时 Let's Encrypt 会访问 80 验证）

**装 certbot**：

```bash
# 装 EPEL 源（如果还没装的话，第二章 2.2 节）
sudo yum install -y epel-release

# 装 certbot 和 nginx 插件
sudo yum install -y certbot python2-certbot-nginx
```

**申请证书**（一条命令自动改 Nginx 配置）：

```bash
# 申请证书并自动改 nginx 配置
sudo certbot --nginx -d your-domain.com -d www.your-domain.com

# 参数：
# --nginx        使用 nginx 插件，自动改配置
# -d             指定域名（可多个，第一个是主域名）

# 交互式问答：
# 1. 输入邮箱（接收证书过期通知）
# 2. 同意服务条款 A
# 3. 是否分享邮箱 N
# 4. 选择要启用 HTTPS 的网站（如果只有一个就选 1）

# 成功后看到：
# Successfully obtained certificate
# 重启 nginx 自动加载证书
```

**自动续期**（Let's Encrypt 证书 90 天过期，必须续期）：

```bash
# 测试续期命令（不真正续，只模拟）
sudo certbot renew --dry-run

# 添加定时任务（系统 cron 已经自动加了，验证一下）
sudo crontab -l | grep certbot
# 应该看到：0 0,12 * * * certbot renew --quiet

# 不用管它，自动续期
```

> ⚠️ **常见坑**：
> - **域名没解析**：申请时 Let's Encrypt 访问 `http://your-domain.com/.well-known/...` 验证，域名不通就申请失败
> - **80 端口没开**：防火墙和云服务安全组都要放行 80
> - **国内服务器**：部分国内云服务器对 Let's Encrypt 验证服务器有屏蔽，可能申请失败。改用阿里云/腾讯云免费证书（控制台 → SSL 证书 → 申请免费证书）

### 10.5 Nginx 配置 HTTPS 一步一步

> 🏷️ ⭐ 开发必会 —— 手动配置 HTTPS（不用 certbot 自动改时）

**完整 HTTPS 配置模板**：

```bash
sudo vi /etc/nginx/conf.d/myproject.conf
```

```nginx
# ============================================================
# HTTP 自动跳转 HTTPS（80 端口）
# ============================================================
server {
    listen 80;
    server_name your-domain.com www.your-domain.com;
    return 301 https://$host$request_uri;    # 永久重定向到 HTTPS
}

# ============================================================
# HTTPS 主站（443 端口）
# ============================================================
server {
    listen 443 ssl;
    server_name your-domain.com www.your-domain.com;

    # ★ 证书路径
    ssl_certificate     /etc/nginx/ssl/selfsigned.crt;   # 证书
    ssl_certificate_key /etc/nginx/ssl/selfsigned.key;   # 私钥

    # SSL 优化（必加，否则评分低）
    ssl_protocols TLSv1.2 TLSv1.3;             # 只用 TLS 1.2+，老协议不安全
    ssl_ciphers HIGH:!aNULL:!MD5;              # 高强度加密套件
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 10m;

    charset utf-8;

    # 下面所有配置和 HTTP 版完全一样
    root /www/myproject/frontend/dist;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;    # ★ 这里告诉 Django 用户用 https
    }

    location /admin/ {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static/ {
        alias /www/myproject/backend/static/;
        expires 30d;
        access_log off;
    }

    location /media/ {
        alias /www/myproject/backend/media/;
    }

    client_max_body_size 50M;
    access_log /www/myproject/logs/nginx_access.log;
    error_log  /www/myproject/logs/nginx_error.log;

    gzip on;
    gzip_types text/css application/javascript application/json;
    gzip_min_length 1k;
}
```

**生效**：

```bash
sudo nginx -t
sudo systemctl reload nginx

# 防火墙放行 443
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

**验证**：

```bash
# 本机自测（自签证书用 -k 跳过验证）
curl -k https://127.0.0.1/

# 浏览器访问
# https://your-domain.com
# Let's Encrypt 证书应该显示绿色小锁
```

### 10.6 HTTP 自动跳转 HTTPS（必会）

> 🏷️ ⭐ 开发必会 —— 用户输 http:// 也能自动跳到 https://

**配置**（已包含在 10.5 模板里）：

```nginx
server {
    listen 80;
    server_name your-domain.com www.your-domain.com;
    return 301 https://$host$request_uri;
}
```

**原理**：
- `return 301`：返回 301 永久重定向
- `https://$host$request_uri`：拼出新 URL，`$host` 是访问的域名，`$request_uri` 是原始路径

**效果**：
- 用户访问 `http://your-domain.com/users/3` → 浏览器自动跳到 `https://your-domain.com/users/3`

> ⚠️ **坑**：certbot --nginx 会自动加这个跳转，如果手动配置要自己加，否则用户输 http 会直接进 HTTP 站点（如果还保留 80 端口的 server）。

---

## 11. 常见坑与排错手册（必会）

> 🏷️ ⭐ 开发必会 —— 部署遇到问题先来这里查

### 11.1 🎯 作用说明

**这一节解决什么问题**：把部署高频坑整理成手册，遇到问题对照排查。90% 的部署问题都在这节里。

**适用场景**：
- 部署过程中遇到 5xx/4xx 错误
- 前端报错、API 不通、admin 没样式
- 上线后突然不能用

### 11.2 502 Bad Gateway（gunicorn 没起/挂了）

> 🏷️ ⭐ 开发必会 —— 反向代理最常见错误

**含义**：Nginx 配置对了，把请求转发给 8000 端口，但 8000 没人接（gunicorn 没起或挂了）。

**排查清单**：

```bash
# ① gunicorn 服务状态
sudo systemctl status gunicorn
# 如果是 inactive/dead，启动它：
sudo systemctl start gunicorn

# ② 看 gunicorn 日志
sudo journalctl -u gunicorn -n 50 --no-pager
# 找 Traceback，99% 是 settings.py 报错、依赖没装、路径错

# ③ 检查 8000 端口是否在监听
sudo ss -tlnp | grep 8000
# 没输出 = gunicorn 没起；有输出 = 端口在听，问题在 Nginx 配置

# ④ 手动起 gunicorn 测试（绕过 systemd 看报错）
cd /www/myproject/backend
source venv/bin/activate
gunicorn myproject.wsgi:application --bind 127.0.0.1:8000 --workers 3
# 看终端输出，如果有报错会直接显示
```

**常见原因**：

| 原因 | 解决 |
|------|------|
| gunicorn 服务没启动 | `sudo systemctl start gunicorn` |
| settings.py 报错（ALLOWED_HOSTS、DB 配置） | 看日志修配置 |
| 依赖没装全（mysqlclient、corsheaders） | `pip install -r requirements.txt` |
| worker 数太多内存爆了 | 改 service 文件降 workers 数 |
| WorkingDirectory 路径错 | 改 service 文件里的路径 |

### 11.3 404 Not Found（前端路由/静态文件找不到）

> 🏷️ ⭐ 开发必会 —— 路径错或 location 没匹配上

**排查清单**：

```bash
# ① 看请求实际打到了哪个 location
sudo tail -f /var/log/nginx/access.log
# 看 IP 之后的路径，对照 location 配置

# ② 前端路由刷新 404：try_files 没加或写错
# 检查 location / 里有没有 try_files $uri $uri/ /index.html;

# ③ 静态文件 404：alias 路径错或文件不存在
ls /www/myproject/backend/static/   # 文件在不在
# 在浏览器 F12 → Network 看请求 URL，对照 location 配置
```

**常见原因**：

| 现象 | 原因 | 解决 |
|------|------|------|
| Vue 路由刷新 404 | try_files 没加 | 加 `try_files $uri $uri/ /index.html;` |
| JS/CSS 404 | dist 路径错或 base 没改 | 检查 root 和 vite.config.js 的 base |
| admin 静态 404 | collectstatic 没做或 alias 错 | 跑 collectstatic，检查 alias 路径 |
| API 404 | location 没匹配或 Django 路由错 | 检查 location /api/，检查 Django urls.py |

### 11.4 403 Forbidden（权限/缺 index）

> 🏷️ ⭐ 开发必会 —— nginx 用户读不到文件

**含义**：Nginx 找到了文件，但没权限读，或目录没 index 文件。

**排查清单**：

```bash
# ① 看 nginx 错误日志（最直接）
sudo tail -f /var/log/nginx/error.log
# 看到 Permission denied = 权限问题
# 看到 Directory index forbidden = 缺 index 指令

# ② 检查文件权限
ls -l /www/myproject/frontend/dist/index.html
# 所有者应该是 nginx:nginx 或其他用户对 nginx 可读

# ③ 检查目录权限（每层目录都要可执行 x 权限）
namei -l /www/myproject/frontend/dist/index.html
# 看 drwxr-xr-x 每一层，nginx 用户要有 x 权限才能进入

# ④ 改所有者解决权限
sudo chown -R nginx:nginx /www/myproject/
sudo chmod -R 755 /www/myproject/
```

**常见原因**：

| 现象 | 原因 | 解决 |
|------|------|------|
| 全站 403 | root 目录 nginx 没权限 | `chown -R nginx:nginx /www/` |
| 单文件 403 | 文件所有者不对 | `chown nginx:nginx 文件` |
| 目录 403 | 缺 index 指令 | 加 `index index.html;` |
| SELinux 拦截 | CentOS 7 默认开 SELinux | `chcon -Rt httpd_sys_content_t /www/` |

### 11.5 CORS 跨域错误（前端报错）

> 🏷️ ⭐ 开发必会 —— 前端浏览器 F12 看到 CORS 错误

**现象**：浏览器 F12 Console 看到：
```
Access to XMLHttpRequest at 'http://...' from origin 'http://...'
has been blocked by CORS policy
```

**原因**：前端域名和后端 API 域名不同源（不同端口也算不同源）。

**解决方式**（推荐优先级从高到低）：

**方式一：同源部署（最推荐，本章方案）**
- 前端和 API 都在同一个 Nginx（80/443 端口），同域名
- 浏览器视为同源，没有 CORS 问题
- 本章 9.3 节配置就是这种

**方式二：Django 配 CORS**（前后端不同源才用）

```python
# settings.py
INSTALLED_APPS += ['corsheaders']
MIDDLEWARE = ['corsheaders.middleware.CorsMiddleware'] + MIDDLEWARE

# 允许特定前端
CORS_ALLOWED_ORIGINS = [
    'http://192.168.233.131',
    'http://localhost:5173',  # Vite 开发服务器
]

# 或者允许所有（不安全，仅开发用）
# CORS_ALLOW_ALL_ORIGINS = True
```

### 11.6 静态文件 404（collectstatic 没做/alias 写错）

> 🏷️ ⭐ 开发必会 —— admin 后台没样式就是这个问题

**现象**：Django admin 后台打开是白底黑字，没有蓝绿色样式。

**排查**：

```bash
# ① collectstatic 做了吗
ls /www/myproject/backend/static/
# 没看到 admin/ 目录 = 没做 collectstatic

# ② 做 collectstatic
cd /www/myproject/backend
source venv/bin/activate
python myproject/manage.py collectstatic --noinput

# ③ Nginx alias 路径对吗
# 应该是 alias /www/myproject/backend/static/;（结尾有 /）

# ④ 浏览器 F12 → Network → 刷新 admin → 看 CSS 请求是不是 200
```

### 11.7 排错三板斧：日志 + curl + nginx -t

> 🏷️ ⭐ 开发必会 —— 任何 Nginx 问题都靠这三板斧定位

**第一板斧：看日志**

```bash
# Nginx 访问日志（看请求是否到达、状态码）
sudo tail -f /var/log/nginx/access.log

# Nginx 错误日志（看 502/403 原因）
sudo tail -f /var/log/nginx/error.log

# gunicorn 日志（看 Django 报错）
sudo journalctl -u gunicorn -f

# 项目日志（如果配了 access_log/error_log 到项目目录）
sudo tail -f /www/myproject/logs/nginx_error.log
```

**第二板斧：curl 测试**（绕过浏览器，本机直接测）

```bash
# 测前端
curl http://127.0.0.1/
# 测 Vue 路由
curl http://127.0.0.1/users/3
# 测 API
curl http://127.0.0.1/api/users/
# 测 admin
curl http://127.0.0.1/admin/
# 测静态文件
curl http://127.0.0.1/static/admin/css/base.css

# 加 -I 只看响应头（看 status 和 Content-Type）
curl -I http://127.0.0.1/api/users/

# 加 -v 看详细过程（看请求头和响应头）
curl -v http://127.0.0.1/api/users/
```

**第三板斧：nginx -t 验证配置**

```bash
# 测语法（必做）
sudo nginx -t

# 看当前生效的完整配置（拼好的所有 include）
sudo nginx -T | grep -A 20 "server_name your"
```

> 🔑 **排错黄金法则**：先 `nginx -t` 确认配置没错，再 `curl` 本机自测定位层级（前端/Nginx/后端哪一层挂了），最后 `tail -f 日志` 看具体错误。这三步下来 90% 的问题都能定位。

---

## 本章小结

### 命令速查表

> 🏷️ ⭐ 开发必会 —— 部署时直接对照

| 命令 | 作用 |
|------|------|
| `sudo nginx -t` | 测配置语法（改配置必做） |
| `sudo systemctl reload nginx` | 平滑重载配置（不断连接） |
| `sudo systemctl restart nginx` | 强制重启（断连接） |
| `sudo systemctl status nginx` | 看 Nginx 状态 |
| `sudo systemctl status gunicorn` | 看后端状态 |
| `sudo systemctl restart gunicorn` | 改了代码后重启后端 |
| `sudo journalctl -u gunicorn -f` | 实时看后端日志 |
| `sudo tail -f /var/log/nginx/error.log` | 实时看 Nginx 错误日志 |
| `sudo tail -f /var/log/nginx/access.log` | 实时看 Nginx 访问日志 |
| `curl http://127.0.0.1/` | 本机自测 |
| `sudo ss -tlnp \| grep :80` | 查端口占用 |
| `sudo firewall-cmd --permanent --add-service=http` | 防火墙放行 HTTP |
| `sudo firewall-cmd --reload` | 重载防火墙规则 |
| `python manage.py collectstatic --noinput` | 收集静态文件 |
| `python manage.py migrate` | 数据迁移 |
| `sudo certbot --nginx -d 域名` | 申请 HTTPS 证书 |
| `sudo openssl req -x509 ...` | 生成自签证书 |

### 配置速查表

> 🏷️ ⭐ 开发必会 —— 高频配置指令对照

| 指令 | 作用 | 示例 |
|------|------|------|
| `listen 80;` | 监听端口 | `listen 443 ssl;` 监听 443 |
| `server_name` | 匹配域名/IP | `server_name aaa.com;` |
| `root` | 网站根目录（拼接） | `root /www/dist;` |
| `alias` | 替换 location 路径 | `alias /www/static/;` |
| `index` | 默认入口文件 | `index index.html;` |
| `try_files` | 找不到回退 | `try_files $uri $uri/ /index.html;` |
| `proxy_pass` | 反向代理目标 | `proxy_pass http://127.0.0.1:8000;` |
| `proxy_set_header` | 转发请求头 | `proxy_set_header Host $host;` |
| `client_max_body_size` | 上传文件大小限制 | `client_max_body_size 50M;` |
| `gzip on;` | 开启压缩 | 减小 JS/CSS 传输体积 |
| `expires` | 浏览器缓存时间 | `expires 30d;` |
| `access_log` | 访问日志路径 | `access_log /var/log/nginx/access.log;` |
| `error_log` | 错误日志路径 | `error_log /var/log/nginx/error.log;` |
| `ssl_certificate` | HTTPS 证书 | `ssl_certificate /etc/nginx/ssl/xxx.crt;` |
| `return 301` | 永久重定向 | `return 301 https://$host$request_uri;` |

### 学习边界：学完本章你能做什么

> 🏷️ ⭐ 开发必会 —— 明确能力边界，知道下一步学什么

**学完本章你能做到**：

- ✅ 在 CentOS 7 上独立部署一个 Vue3 + Django + MySQL 全栈项目
- ✅ 写出生产级的 Nginx 配置（前端托管 + 反向代理 + 静态文件）
- ✅ 配置 HTTPS（自签 + Let's Encrypt）
- ✅ 排查 90% 的部署错误（502/404/403/CORS）
- ✅ 用 systemd 管理 gunicorn 后端服务
- ✅ MySQL 建库建用户、Django 配置连接

**本章没讲、可以下一步学的**：

- 📖 **Nginx 负载均衡**：一个域名分发到多台后端服务器（upstream 模块）
- 📖 **Nginx 限流**：`limit_req` 防刷接口
- 📖 **Nginx 缓存**：`proxy_cache` 缓存后端响应
- 📖 **uwsgi 替代 gunicorn**：另一种 Python 应用服务器
- 📖 **Docker 部署**：容器化替代手动部署
- 📖 **CI/CD 自动部署**：git push 自动上线（Jenkins/GitLab CI）
- 📖 **Nginx Lua 扩展**：OpenResty 用 Lua 写自定义逻辑

> 📌 **学习建议**：把本章的部署流程在虚拟机里完整走一遍，遇到问题对照第 11 节排错。能独立部署一个完整项目，就达到企业中级开发水平。负载均衡、Docker、CI/CD 是进阶内容，工作中遇到再学。

