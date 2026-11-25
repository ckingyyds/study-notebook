# 在 Linux 虚拟机里用 Tomcat 发布网页

> **来源**：2026-11-25 下午课堂录屏（46 分 30 秒）
> **环境**：VMware 虚拟机 + CentOS + FinalShell 远程连接
> **一句话**：把一个写好的静态网页，从"我电脑上的一个文件夹"，变成"别人在浏览器里输网址就能看到"。

---

## 一、先弄清楚：什么叫"发布"

### 1.1 开发完 ≠ 上线

你在 PyCharm 里写前端、在本地写完代码，这些**都只存在于你自己的电脑里**。客户 / 老师 / 同学要的不是你的代码，而是一个能打开的网址。

所以"发布"（部署）的本质是：

```
浏览器  --输入 IP + 端口-->  服务器  -->  Web 容器  -->  你的网页
```

### 1.2 两个容易混淆的"服务器"

| 叫法 | 是什么 | 例子 |
| :--- | :--- | :--- |
| **服务器（硬件/系统层面）** | 一台装了 Linux 的机器，负责"能被人找到" | 你现在这台 CentOS 虚拟机 |
| **Web 容器 / Web 服务器（软件层面）** | 装在 Linux 里的一款软件，负责"监听请求、把网页发回去" | **Tomcat**、Nginx、Apache(httpd) |

**只把网页文件传到 Linux 上，是打不开的。** 必须再装一个 Web 容器软件，让网页"挂"在它上面。

> 你输入 IP，只能找到这台机器；再输入端口号，才能找到机器上跑着的某个服务。所以 Tomcat 的地址永远是 `IP:8080`，光有 IP 不行。

### 1.3 两种部署方案（这节课的主线）

| 方案 | 做法 | 特点 |
| :--- | :--- | :--- |
| **传统方式** | 在 Linux 里手动装 JDK → 手动装 Tomcat → 手动传网页 | 步骤多、环境要自己配、换机器要全部重来 |
| **Docker 方式** | 拉一个装好 JDK+Tomcat 的镜像，一条命令跑起来 | 快、环境现成、做集群只是多跑几条命令 |

这节课先走完传统方式，再用 Docker 做一遍同样的事做对比（Docker 部分见另一份笔记《Docker入门与项目部署》）。

---

## 二、准备工作

### 2.1 需要的两个安装包

| 文件 | 作用 |
| :--- | :--- |
| `jdk-8uXXX-linux-x64.tar.gz` | Java 运行环境。**Tomcat 自己是用 Java 写的，没有 JDK 它跑不起来** |
| `apache-tomcat-8.5.100.tar.gz` | Tomcat 本体（这节课用的 8.5 版本） |

顺序不能反：**先装 JDK，再装 Tomcat**。

### 2.2 用 FinalShell 把包传进虚拟机

传统方式下，从 Windows 往 Linux 传文件有个很简单的办法——FinalShell 自带的文件面板。

1. 连上虚拟机后，点下方那个**向上的小箭头**，会展开一个文件目录面板；
2. 这个面板里能看到 Linux 的文件系统（相当于图形界面版的目录树）；
3. 双击进入 `/root`（root 用户的家目录）；
4. 点面板上的**上传**按钮 → 选中 Windows 桌面上那两个压缩包 → 确定。

传完之后，在终端里确认一下：

```bash
pwd          # 看看自己在哪
cd /root     # 进到 root 家目录
ls           # 应该能看到那两个 .tar.gz
```

> **小知识**：用哪个账号登录，默认就进入哪个账号的家目录。root 用户登录 → `/root`；普通用户 `zhangsan` 登录 → `/home/zhangsan`。

---

## 三、安装 JDK

### 3.1 解压

```bash
cd /root
tar -xvf jdk-8uXXX-linux-x64.tar.gz
tar -xvf apache-tomcat-8.5.100.tar.gz
ls
```

解压完会多出两个目录（名字很长，带版本号）。

原始压缩包已经没用了，可以删掉：

```bash
rm -rf jdk-8uXXX-linux-x64.tar.gz
rm -rf apache-tomcat-8.5.100.tar.gz
```

### 3.2 把它们挪到 /opt 并改名

第三方软件一般不直接扔在 `/root` 或根目录下，习惯放在 `/usr` 或 `/opt`。这里用 `/opt`。

```bash
mv jdk1.8.0_XXX /opt/
mv apache-tomcat-8.5.100 /opt/
cd /opt
ls
```

**改名**（后面配环境变量时能少敲一长串）：

```bash
cd /opt
mv jdk1.8.0_XXX jdk8
mv apache-tomcat-8.5.100 tomcat8
ls                # 现在应该是 jdk8 和 tomcat8 两个目录
```

> **注意**：`mv` 在同一个目录里操作，效果就是"改名"。

### 3.3 为什么一定要配环境变量

解压出来的 JDK 目录里，`bin` 目录下才有 `java`、`javac` 这两个命令：

```bash
cd /opt/jdk8/bin
ls                # 能看到 java、javac
./java -version   # 这样也能运行，但每次都得写全路径，太麻烦
```

配环境变量的目的就是：**让系统在任何目录下都能直接输入 `java` 就找到它**。

这和你在 Windows 上装完 Python 后要把 Python 加进 PATH，是同一件事。

### 3.4 修改 /etc/profile

Linux 里全局环境变量的配置文件是 `/etc/profile`。

```bash
vi /etc/profile
```

> 如果提示 `vi: command not found`（极简安装的 CentOS 常见），先装一下：
> ```bash
> yum install -y vim-enhanced
> ```
> 然后就能用 `vi` / `vim` 了。

按 `i` 进入插入模式，在文件**最后**追加两行：

```bash
export JAVA_HOME=/opt/jdk8
export PATH=$PATH:$JAVA_HOME/bin
```

按 `Esc`，输入 `:wq` 回车保存退出。

**这两行分别在干什么：**

| 行 | 含义 |
| :--- | :--- |
| `export JAVA_HOME=/opt/jdk8` | 声明一个全局变量 `JAVA_HOME`，指向 JDK 安装目录。**Tomcat 启动时会主动去找这个变量**，找不到就报错 |
| `export PATH=$PATH:$JAVA_HOME/bin` | 把 JDK 的 `bin` 目录追加到系统原有的 `PATH` 后面。`$PATH` 是引用原来的值（里面本来就有东西，不能覆盖） |

> 为什么不能只写第二行？因为 Tomcat 启动脚本里写死了要读 `JAVA_HOME`，光有 PATH 它能找到 `java` 命令，但读不到 `JAVA_HOME` 这个变量，还是会出问题。所以两行分开写最稳。

### 3.5 让配置生效

改完 `/etc/profile` **不会立刻生效**，两种办法：

```bash
# 办法一：手动加载一次（推荐，不用重启）
source /etc/profile

# 办法二：重启虚拟机（reboot）
```

### 3.6 验证

```bash
java -version
javac
echo $JAVA_HOME
```

能打印出版本号（1.8.x）就说明成功了。

---

## 四、装 Tomcat 并启动

### 4.1 认识 Tomcat 的目录

```bash
cd /opt/tomcat8
ls
```

| 目录 | 作用 |
| :--- | :--- |
| `bin/` | 启动、关闭脚本都在这里 |
| `conf/` | 配置文件（端口号就在 `server.xml` 里改） |
| `webapps/` | **发布项目的地方**（重点） |
| `logs/` | 日志 |
| `lib/` | 依赖的 jar 包 |

### 4.2 启动

```bash
cd /opt/tomcat8/bin
ls                       # 能看到 startup.sh、shutdown.sh
./startup.sh
```

看到 `Tomcat started.` 就说明起来了。

**关闭：**

```bash
cd /opt/tomcat8/bin
./shutdown.sh
```

> **Windows 上对应的是** `startup.bat` / `shutdown.bat`；Linux 上是 `.sh` 脚本。

---

## 五、把一个网页"发布"出去

### 5.1 webapps 目录就是"发布区"

```bash
cd /opt/tomcat8/webapps
ls
```

默认会有这几个目录：

```
docs  examples  host-manager  manager  ROOT
```

- **`ROOT`** 就是 Tomcat 自带的一个示例项目。你不放自己的项目时，访问 `IP:8080` 默认打开的就是 `ROOT/index.jsp`。
- `docs`、`examples` 是官方文档和示例，可以直接删。

### 5.2 发布自己的网页

**最简单的做法**：把写好的网页文件夹整个丢进 `webapps` 里。

```bash
# 假设你的项目叫 mysite（里面有 index.html、css、js、img）
cd /opt/tomcat8/webapps
# 用 FinalShell 上传，或者用解压/拷贝的方式放进来
ls
```

然后在浏览器里访问：

```
http://192.168.x.x:8080/mysite/index.html
```

就出来了。

**如果想让用户只输 IP:8080 就打开你的页面**，那就把你的网页内容**替换到 `webapps/ROOT/` 里**（先把原来 ROOT 里的东西清空）。

### 5.3 关于 404

访问 `IP:8080` 出现 **404** 不一定是你配错了，可能是：

- 项目确实没放进去；
- 或者路径少写了一层（比如要访问 `/mysite/` 但你访问的是 `/`）；
- 或者——**8080 端口被别的程序占着**（这个坑课上就踩到了，见第八节）。

---

## 六、从浏览器访问：IP + 端口 + 防火墙

### 6.1 先查虚拟机的 IP

```bash
ip addr
```

找到 `ens33` 那一块的 `inet`，课上这台是 `192.168.253.185`。**你自己的虚拟机会是别的地址，以你查到的为准**。

### 6.2 为什么必须带 :8080

| 部分 | 作用 |
| :--- | :--- |
| IP | 找到这台机器 |
| 端口 | 找到这台机器上的 Tomcat |

Tomcat 默认端口是 **8080**，在 `/opt/tomcat8/conf/server.xml` 里能改。

> 一台机器上可以同时跑 QQ、微信、浏览器，就是靠端口区分。IP 只能定位到机器，端口才能定位到具体服务。

### 6.3 访问不通？先看防火墙

CentOS 默认开着 firewalld，它会把 8080 挡住。

**查状态：**

```bash
systemctl status firewalld
```

看到 `active (running)` 说明防火墙在开着。

**课上用的办法（直接关掉，最快）：**

```bash
systemctl stop firewalld
systemctl status firewalld     # 变成 inactive (dead)
```

**更规范的办法（只开放 8080，不动其他）——（补充）：**

```bash
# 永久放行 8080 端口
firewall-cmd --zone=public --add-port=8080/tcp --permanent
# 重载生效
firewall-cmd --reload
# 查看已开放的端口
firewall-cmd --list-ports
```

> 学习环境关掉防火墙最省事；真实服务器上建议用放行端口的方式，别整个关掉。

---

## 七、老师这台机器上的"历史残留"——你自己做时会不一样

课上老师演示时，虚拟机是**以前用过很多次**的，很多结果并不是"从零操作"得来的。你新装的虚拟机会和视频里不一样，**这不是你做错了**。

| 环节 | 老师的机器上发生了什么 | 你的新虚拟机会发生什么 |
| :--- | :--- | :--- |
| `mv` 到 `/opt` | `/opt` 下**已经有** `jdk8`、`tomcat8`（以前装的），所以他移动后出现重名，干脆把新解压的那份删掉了 | 移动会顺利成功，不会重名 |
| `/etc/profile` | 里面**已经写过** `JAVA_HOME`、`PATH` 那两行 | 文件末尾是空的，你要自己追加 |
| `java -version` | 直接就出版本号（因为早就配好了） | 必须先 `source /etc/profile` 才生效 |
| `webapps/ROOT/index.jsp` | 里面被老师加过一行 `<h1>Hello Tomcat</h1>`，用来验证"确实访问到了这个页面" | 是 Tomcat 原版页面，标题是 Apache Tomcat 的官方主页 |
| `webapps/` 目录 | 多了一个 `mi` 目录（老师以前放的"小米"网页作业） | 只有默认的 docs / examples / manager / ROOT |
| `docker images` | 已经有 `nginx`、`mysql`、`tomcat:8` 三个镜像 | 是空的（一条记录都没有） |
| `docker ps -a` | 有一堆以前跑过的、已退出的容器 | 空的 |
| 访问 `IP:8080` | **先出了 404**，因为以前 Docker 里跑的三个 Tomcat 容器还占着 8080，浏览器访问到的是容器的 Tomcat（容器里没项目 → 404），而不是本机装的 Tomcat | 只要 Tomcat 起来、防火墙放行，就能直接看到页面 |

**课上那个 404 的完整过程**（很典型的排查案例）：

1. 本机装好 Tomcat，`startup.sh` 起来了；
2. 浏览器访问 `IP:8080` → 404；
3. `docker ps` 一看，里面有 3 个 tomcat 容器还在跑，占着 8080；
4. `docker stop <id>...` 把容器全停掉；
5. 重启本机 Tomcat（`./shutdown.sh` → `./startup.sh`）；
6. 还不行 → 查防火墙，`systemctl stop firewalld`；
7. 刷新，页面出来了，看到了自己加的 `Hello Tomcat`。

> **记住这个教训**：端口冲突时，症状往往是"访问到了别人"，而不是"访问不到"。

---

## 八、完整流程速查

```bash
# 1. 上传安装包到 /root（FinalShell 文件面板）

# 2. 解压
cd /root
tar -xvf jdk-8uXXX-linux-x64.tar.gz
tar -xvf apache-tomcat-8.5.100.tar.gz

# 3. 挪到 /opt 并改名
mv jdk1.8.0_XXX /opt/jdk8
mv apache-tomcat-8.5.100 /opt/tomcat8

# 4. 配环境变量
vi /etc/profile
#   末尾追加：
#   export JAVA_HOME=/opt/jdk8
#   export PATH=$PATH:$JAVA_HOME/bin
source /etc/profile

# 5. 验证 JDK
java -version

# 6. 启动 Tomcat
/opt/tomcat8/bin/startup.sh

# 7. 放网页
cp -r /root/my-web/* /opt/tomcat8/webapps/ROOT/
#   或者整个文件夹丢进 webapps，访问 IP:8080/文件夹名/

# 8. 关防火墙（学习环境）
systemctl stop firewalld

# 9. 查 IP 并访问
ip addr
# 浏览器打开 http://IP:8080
```

---

## 九、几个常见坑（补充）

| 现象 | 原因 | 解决 |
| :--- | :--- | :--- |
| `java: command not found` | 没配环境变量，或配了没 `source` | 检查 `/etc/profile` 末尾两行，执行 `source /etc/profile` |
| `Neither the JAVA_HOME nor the JRE_HOME environment variable is defined` | 只配了 PATH，没配 `JAVA_HOME` | 补上 `export JAVA_HOME=/opt/jdk8` |
| 启动成功但浏览器访问不了 | 防火墙 / 端口冲突 | `systemctl stop firewalld`，`ss -lntp \| grep 8080` 看谁占着 |
| 只能在自己电脑上访问，别人打不开 | 虚拟机是 VMware 的 NAT 网络，IP 是内网地址 | 这是正常的，内网 IP 只有宿主机/同一局域网能访问。真要对外，需要用公网服务器 |
| 关掉终端 Tomcat 就停了 | 用 `./startup.sh` 是在当前会话起的，正常后台运行不会因为关终端停；如果停了多半是没启动成功 | 查 `logs/catalina.out` |
| 中文乱码 | 文件编码不是 UTF-8 | 把页面保存为 UTF-8，或在 `server.xml` 的 Connector 加 `URIEncoding="UTF-8"` |

---

## 十、这一节和 Django 有什么关系

这门课是 Django 阶段，为什么要先学用 Tomcat 发网页？

因为后面部署 Django 项目时，思路完全一样：

```
IP:端口  →  找服务器  →  找服务（容器）  →  返回页面
```

只不过 Django 用的是 Python 的 WSGI 服务器（gunicorn / uWSGI）+ Nginx，而不是 Tomcat。今天这套"传文件 → 解压 → 配环境变量 → 起服务 → 放页面 → 开防火墙 → 浏览器验证"的动作，换成任何 Web 服务都是同一个套路。