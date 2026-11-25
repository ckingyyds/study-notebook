#  Docker 指南

## 零、什么是 Docker？为什么要安装？

### 0.1 一句话理解 Docker

**Docker 是一个"打包 + 隔离 + 运行"应用的工具。** 它把你的应用连同它依赖的环境（操作系统库、Python 版本、MySQL 驱动、Nginx 配置……）一起打成一个标准化的"集装箱"（叫**镜像 image**），然后把这个集装箱搬到任何一台装了 Docker 的机器上跑（叫**容器 container**），跑出来的效果和在你本机一模一样。

打个比方：
- **没 Docker 的时代**：你把一棵树连根带土挖出来，寄给朋友，朋友家土壤酸碱度不一样，树死了。
- **有 Docker 的时代**：你把树连同它原本的土壤、花盆、浇水说明一起打包寄过去，朋友原样摆好就能活。

那个"花盆 + 土 + 树 + 说明"的整体，就是 Docker 镜像。

### 0.2 它解决了什么痛点？

| 没有 Docker 时的痛点 | Docker 怎么解决 |
|---|---|
| "在我电脑上能跑啊！" —— 本机能跑，上服务器就报错 | 开发、测试、生产环境完全一致（都是同一个镜像） |
| 装个 MySQL 要下载、改密码、配字符集，半小时起步 | `docker run` 一行命令，30 秒拉起一个干净的 MySQL |
| 一台机器装多个项目，Python 版本、端口、依赖互相打架 | 每个容器各自隔离，互不干扰，想删就删 |
| 新同事入职配环境要一整天 | 拉镜像跑容器，10 分钟搞定 |
| 部署 Django 项目要手动装 Python、装依赖、配 Nginx、配 Gunicorn… | 写一个 Dockerfile，一条命令打包成镜像，到哪都能跑 |

### 0.3 为什么要安装 Docker？（开发必知视角）

如果你是下面这类开发者，**强烈建议装 Docker**：

1. **做 Django / Flask / FastApi 后端开发**
   - 本机跑一套，部署到服务器再跑一套？环境对不上心态崩。
   - 用 Docker 把 Python 解释器、requirements.txt、gunicorn 一起打包，本地和线上一个样。

2. **项目要用 MySQL / Redis / Nginx / Elasticsearch 等中间件**
   - 不用 Docker：去官网下安装包、改 my.cnf、设密码、卸载残留……折腾一上午。
   - 用 Docker：一行命令拉起一个干净实例，不想要了 `docker rm` 就走，宿主机一干二净。

3. **做 RAG / AI 应用（你的方向）**
   - 跑大模型用 Ollama？Docker 一行命令拉起来。
   - 向量库 Chroma / Milvus / Qdrant？都有官方镜像，不用 Docker 你得手动装依赖、编译、配端口。

4. **前后端分离项目（ Djangoserver2 + DjangoQt）**
   - 前端、后端、数据库、Nginx 各跑一个容器，用 `docker-compose` 一条命令编排起来，开发环境秒搭。

5. **团队协作 / 部署上线**
   - 你写完代码 push 到 Git，CI 自动构建镜像，服务器 `docker pull` 拉新镜像重启容器，10 秒发布完。
   - 不用 Docker 你得 SSH 上去 git pull、pip install、migrate、collectstatic、restart gunicorn……每一步都可能出错。

### 0.4 装完 Docker 你能干什么？

装完 Docker 后，下面这些事情都会变成"一行命令"：

```bash
# 30 秒拉起一个 MySQL（带密码、数据持久化）
docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=123456 -v /data/mysql:/var/lib/mysql mysql:8

# 30 秒拉起一个 Nginx，宿主 8080 端口直接访问
docker run -d --name web -p 8080:80 nginx

# 30 秒拉起 Ollama 大模型服务
docker run -d --name ollama -p 11434:11434 -v /data/ollama:/root/.ollama ollama/ollama

# 把自己的 Django 项目打包成镜像
docker build -t mydjango:v1 .

# 一条命令启动"前端 + 后端 + 数据库 + Nginx"整套环境
docker-compose up -d
```

### 0.5 小结

> **一句话总结**：Docker 让"装环境"这件事从"半天的体力活"变成"一条命令的体力活"。对开发者来说，它不是可选项，而是 2026 年的标配基建。

下面正式开始 CentOS 安装 Docker 的流程。

---

## 一、删除旧版本的 Docker

### 1.1 检查已安装的 Docker 版本

```bash
docker --version
```

如果输出类似以下内容，说明已安装 Docker：

```
Docker version 20.10.21, build baeda1f
```

### 1.2 停止 Docker 服务

```bash
sudo systemctl stop docker
sudo systemctl stop docker.socket
sudo systemctl stop containerd
```

### 1.3 卸载已安装的 Docker 包

```bash
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```

### 1.4 删除 Docker 相关数据目录

```bash
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

### 1.5 确认卸载完成

```bash
docker --version
```

如果输出以下内容，说明卸载成功：

```
-bash: docker: command not found
```

## 二、安装必要的依赖

### 2.1 安装 yum-utils

```bash
sudo yum install -y yum-utils
```

### 2.2 安装 device-mapper-persistent-data 和 lvm2

```bash
sudo yum install -y device-mapper-persistent-data lvm2
```

### 2.3 安装其他必要工具

```bash
sudo yum install -y wget curl
```

**这两个工具分别是干什么的？**

| 工具 | 作用 | 在 Docker 安装/使用中的典型场景 |
|------|------|----------------------------|
| `wget` | 命令行下载工具，支持 HTTP/HTTPS/FTP，能递归下载、断点续传 | 下载仓库配置文件、离线镜像包等（如 `wget -O /etc/yum.repos.d/docker-ce.repo https://...`） |
| `curl` | 命令行 URL 传输工具，支持多种协议，常用于请求接口、测试服务 | 测试容器是否正常响应（如 `curl http://localhost:8080`）、拉取在线脚本（如 `curl -fsSL https://get.docker.com \| sh`） |

> **为什么要装它们？** CentOS 7 最小安装（minimal）默认**不带** `wget` 和 `curl`，而后续配置阿里云镜像源、测试容器服务、下载额外镜像或脚本时几乎都会用到这两个命令，所以提前装好避免"用到时才发现没装"。如果用的是完整版（DVD）安装的 CentOS，通常已自带，可跳过本步。

> **两者区别（一句话）：** `wget` 更适合"把文件下载下来"（简单直接、支持断点续传）；`curl` 更适合"跟网络服务交互"（支持多种协议、可发送 POST/PUT 等请求、显示响应头）。日常下载文件优先 `wget`，调试接口优先 `curl`。

## 三、设置 Docker 仓库（推荐使用阿里云镜像源）

### 3.1 问题说明

**可能遇到的错误：**

执行 `sudo yum install -y docker-ce docker-ce-cli containerd.io` 时，可能会提示：

```
已加载插件：fastestmirror, langpacks 
Loading mirror speeds from cached hostfile 
 * base: mirrors.aliyun.com 
 * extras: mirrors.aliyun.com 
 * updates: mirrors.aliyun.com 
没有可用软件包 docker-ce。 
没有可用软件包 docker-ce-cli。 
没有可用软件包 containerd.io。 
错误：无须任何处理
```

**原因：** Docker 官方仓库地址可能无法访问或配置不正确。

**解决方案：** 使用阿里云镜像源（推荐）。 还有可能是yum的源失效了，要修复yum。参考第三章 修复yum。

### 3.2 使用阿里云镜像源（推荐）

**在哪里操作？（重点）**

这些命令全部在 **CentOS 虚拟机的终端** 里直接执行：

- **不是**在 Windows 电脑上执行
- **不是**打开某个文件手动修改（不需要编辑器）
- **不是**在 Docker 里执行（此时 Docker 还没装好）

**如何进入 CentOS 终端（两种方式任选其一）：**

- 方式一：在 VMware/VirtualBox 里打开 CentOS 虚拟机，登录后打开桌面上的"终端（Terminal）"。
- 方式二：在 Windows 上用 SSH 工具远程连接虚拟机（如 XShell、FinalShell、MobaXterm），或在 PowerShell 里执行 `ssh root@192.168.1.100`（IP 换成你的虚拟机 IP）。

**操作步骤（逐条复制 → 粘贴到 CentOS 终端 → 回车）：**

**第 1 步：创建阿里云 Docker 仓库文件**

```bash
# 使用阿里云的 Docker 镜像源（整段复制粘贴，回车即可）
sudo tee /etc/yum.repos.d/docker-ce.repo <<-'EOF'
[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/$releasever/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg
EOF
```

> 说明：`tee 文件名 <<-'EOF' ... EOF` 的意思是"把 EOF 之间的内容直接写入指定文件"，文件不存在会自动创建，已存在会覆盖。所以**你不需要手动打开这个文件去修改**，整段粘贴回车就等于改好了。

**第 2 步：清理 yum 缓存并生成新缓存**

```bash
sudo yum clean all
sudo yum makecache
```

**第 3 步：验证仓库是否添加成功**

```bash
# 查看文件内容（能看到 baseurl=https://mirrors.aliyun.com/... 即写入成功）
cat /etc/yum.repos.d/docker-ce.repo

# 查看仓库列表（能看到 docker-ce-stable 即配置成功）
yum repolist | grep docker
```

**常见疑问：**

| 疑问 | 答案 |
|------|------|
| 是在终端粘贴命令，还是要打开文件改？ | 直接在 CentOS 终端粘贴整段命令即可，`tee` 命令会自动创建并写入文件 |
| Windows 本机需要改什么吗？ | 不需要，Windows 只是用来 SSH 远程连接（如果用方式二） |
| 执行提示权限不足？ | 确认命令前有 `sudo`（root 用户登录则可省略 sudo） |
| 改完不生效？ | 必须先执行第 2 步 `yum clean all` 和 `yum makecache` 才会生效 |

### 3.3 （可选）使用 Docker 官方仓库

如果你想使用官方仓库，可以执行以下步骤：

```bash
# 添加 Docker 官方 GPG 密钥
sudo yum install -y gpg
curl -fsSL https://download.docker.com/linux/centos/gpg | sudo gpg --dearmor -o /etc/pki/rpm-gpg/RPM-GPG-KEY-docker

# 添加 Docker 官方仓库
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

# 清理缓存并生成新缓存
sudo yum clean all
sudo yum makecache
```

### 3.4 查看可用的 Docker 版本

```bash
yum list docker-ce --showduplicates | sort -r
```

输出示例：

```
docker-ce.x86_64    3:24.0.6-1.el7    docker-ce-stable
docker-ce.x86_64    3:24.0.5-1.el7    docker-ce-stable
docker-ce.x86_64    3:24.0.4-1.el7    docker-ce-stable
docker-ce.x86_64    3:24.0.3-1.el7    docker-ce-stable
...
```

## 四、安装 Docker 引擎

### 4.1 安装最新版本的 Docker

```bash
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

### 4.2 安装指定版本的 Docker（可选）

```bash
# 安装指定版本，例如 24.0.6
sudo yum install -y docker-ce-24.0.6 docker-ce-cli-24.0.6 containerd.io
```

### 4.3 确认安装成功

```bash
docker --version
```

输出示例：

```
Docker version 24.0.6, build ed223bc
```

## 五、启动 Docker 服务

### 5.1 启动 Docker 服务

```bash
sudo systemctl start docker
```

### 5.2 设置 Docker 开机自启

```bash
sudo systemctl enable docker
```

### 5.3 检查 Docker 服务状态

```bash
sudo systemctl status docker
```

输出示例：

```
● docker.service - Docker Application Container Engine
   Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; vendor preset: disabled)
   Active: active (running) since Mon 2023-10-09 10:30:00 CST; 1min ago
     Docs: https://docs.docker.com
 Main PID: 1234 (dockerd)
    Tasks: 10
   Memory: 120.0M
   CGroup: /system.slice/docker.service
           └─1234 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
```

## 六、验证 Docker 安装

### 6.1 运行 Hello World 容器

**为什么要运行这个？目的是什么？**

`hello-world` 是 Docker 官方提供的**安装验证测试镜像**（整个镜像只有 13KB），作用相当于装完软件后点"测试一下"：

- 前面的步骤只是把 Docker **装上了**，但装上 ≠ 能正常工作
- 这一步用最小的镜像**验证整条链路是否通畅**，一条命令测完所有关键环节
- 它专为测试设计：体积小下载快、输出一句话就自动退出，不占资源

**它具体验证了 4 个环节（缺一个就说明安装有问题）：**

| 环节 | 验证内容 | 失败说明什么 |
|------|----------|--------------|
| 1. 客户端 → 引擎 | `docker` 命令能联系上 Docker 服务 | Docker 服务没启动（回到第五章启动） |
| 2. 引擎 → 镜像仓库 | 能联网从仓库下载镜像 | 网络不通（见第七章配置加速） |
| 3. 创建并运行容器 | 镜像能正常启动成容器 | Docker 运行环境有问题 |
| 4. 输出回显 | 容器内的输出能显示到你的终端 | 整条链路正常，一般不会单独失败 |

**看到 `Hello from Docker!` = 前面的安装全部成功，可以放心进入后续章节。**

执行命令：

```bash
sudo docker run hello-world
```

执行后 Docker 在后台自动做了这几件事（输出的英文就是在说这个过程）：
1. 检查本地有没有 `hello-world` 镜像 → 没有
2. 联网从镜像仓库下载 `hello-world` 镜像
3. 用这个镜像创建并运行一个容器
4. 容器打印一句话后自动退出

输出示例：

```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

如果报错：

```
[qyz@lee01 ~]$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
docker: Error response from daemon: Get "https://registry-1.docker.io/v2/": dial tcp 130.211.15.150:443: connect: connection refused.
```

❤️说明要进行国内镜像的docker加速的配置。查看并执行，七、配置 Docker 加速





### 6.2 查看已下载的镜像

```bash
sudo docker images
```

输出示例：

```
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
hello-world   latest    feb5d9fea6a5   2 months ago   13.3kB
```

### 6.3 查看运行中的容器

```bash
sudo docker ps -a
```

输出示例：

```
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS     NAMES
abc123def456   hello-world   "/hello"   10 seconds ago   Exited (0) 9 seconds ago             hopeful_mendel
```

### 6.4 Docker 常用命令速查（总结）

安装验证完成后，后续操作 Docker 几乎都靠命令行。这里把最常用的命令按"**镜像 / 容器 / 系统**"三类汇总，作为日常速查表。**初学阶段掌握前 10 条即可应付 90% 场景**，其余用到再查。

#### ① 镜像（Image）相关命令

| 命令 | 作用 | 示例 |
|------|------|------|
| `docker pull <镜像名>:<标签>` | 从仓库下载镜像（不写标签默认 `latest`） | `docker pull nginx:1.24` |
| `docker images` | 列出本地所有镜像 | `docker images` |
| `docker rmi <镜像ID或名>` | 删除镜像（被容器占用时会报错） | `docker rmi nginx:1.24` |
| `docker tag <镜像> <新名>:<新标签>` | 给镜像打标签（重命名/备份用） | `docker tag nginx:1.24 mynginx:v1` |
| `docker save -o <文件>.tar <镜像>` | 把镜像导出成 tar 文件（离线迁移） | `docker save -o nginx.tar nginx:1.24` |
| `docker load -i <文件>.tar` | 从 tar 文件导入镜像 | `docker load -i nginx.tar` |
| `docker search <关键词>` | 在 Docker Hub 搜索镜像 | `docker search mysql` |
| `docker image prune -a` | 清理所有未使用的镜像（释放磁盘） | `docker image prune -a` |

#### ② 容器（Container）相关命令

| 命令 | 作用 | 示例 |
|------|------|------|
| `docker run [选项] <镜像>` | **创建并启动**容器（最核心命令） | `docker run -d --name web -p 8080:80 nginx` |
| `docker ps` | 列出**正在运行**的容器 | `docker ps` |
| `docker ps -a` | 列出**所有**容器（含已停止） | `docker ps -a` |
| `docker start <容器名/ID>` | 启动已停止的容器 | `docker start web` |
| `docker stop <容器名/ID>` | 优雅停止容器（发 SIGTERM） | `docker stop web` |
| `docker restart <容器名/ID>` | 重启容器 | `docker restart web` |
| `docker rm <容器名/ID>` | 删除**已停止**的容器（运行中需先 stop） | `docker rm web` |
| `docker rm -f <容器名/ID>` | 强制删除运行中的容器 | `docker rm -f web` |
| `docker logs <容器名/ID>` | 查看容器日志（排错必备） | `docker logs web` |
| `docker logs -f <容器名/ID>` | 实时跟踪日志（类似 `tail -f`） | `docker logs -f web` |
| `docker exec -it <容器> <命令>` | 进入运行中的容器执行命令 | `docker exec -it web bash` |
| `docker top <容器名/ID>` | 查看容器内进程 | `docker top web` |
| `docker stats` | 实时显示各容器 CPU/内存/网络占用 | `docker stats` |
| `docker cp <宿主文件> <容器>:<路径>` | 把宿主机文件复制进容器 | `docker cp app.py web:/app/` |
| `docker cp <容器>:<路径> <宿主文件>` | 把容器内文件复制出来 | `docker cp web:/etc/nginx/nginx.conf ./` |
| `docker inspect <容器名/ID>` | 查看容器详细配置（IP、挂载等） | `docker inspect web` |
| `docker container prune` | 清理所有已停止的容器 | `docker container prune` |

#### ③ 系统/服务相关命令

| 命令 | 作用 | 示例 |
|------|------|------|
| `sudo systemctl start docker` | 启动 Docker 服务 | `sudo systemctl start docker` |
| `sudo systemctl stop docker` | 停止 Docker 服务 | `sudo systemctl stop docker` |
| `sudo systemctl restart docker` | 重启 Docker 服务（改配置后必做） | `sudo systemctl restart docker` |
| `sudo systemctl status docker` | 查看 Docker 服务状态 | `sudo systemctl status docker` |
| `sudo systemctl enable docker` | 设置开机自启 | `sudo systemctl enable docker` |
| `docker info` | 显示 Docker 系统信息（版本、加速器、存储驱动等） | `docker info` |
| `docker version` | 显示 Docker 版本 | `docker version` |
| `docker system df` | 显示镜像/容器/数据卷占用磁盘 | `docker system df` |
| `docker system prune -a` | **一键清理**所有无用资源（慎用，会删未用镜像） | `docker system prune -a` |

#### ④ 最常用的 `docker run` 选项速记

`docker run` 是使用频率最高的命令，常用选项：

| 选项 | 含义 | 记忆方法 |
|------|------|---------|
| `-d` | 后台运行（detached） | **d**etached = 后台 |
| `--name <名字>` | 给容器起名字（不写会随机生成） | name = 名字 |
| `-p <宿主端口>:<容器端口>` | 端口映射（**最常用**） | **p**ort = 端口 |
| `-v <宿主路径>:<容器路径>` | 挂载目录/文件（数据持久化） | **v**olume = 卷 |
| `-e <变量名>=<值>` | 设置环境变量 | **e**nvironment = 环境 |
| `-it` | 交互模式 + 分配终端（进容器用） | **i**nteractive + **t**erminal |
| `--restart=always` | 容器挂了/重启系统后自动拉起 | restart = 重启 |
| `--network <网络名>` | 加入指定网络（容器互通用） | network = 网络 |

**典型组合示例：**

```bash
# 后台跑一个 nginx，名字叫 web，宿主 8080 映射到容器 80，开机自启
docker run -d \
  --name web \
  -p 8080:80 \
  --restart=always \
  nginx:1.24

# 跑一个 mysql，设置 root 密码，数据持久化到宿主 /data/mysql
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e MYSQL_ROOT_PASSWORD=123456 \
  -v /data/mysql:/var/lib/mysql \
  --restart=always \
  mysql:8.0

# 进入正在运行的 web 容器开个终端
docker exec -it web bash
```

> **学习建议：** 别死记硬背，先把 `docker run`、`docker ps`、`docker logs`、`docker exec`、`docker stop/rm` 这 5 条用熟，后面装 MySQL、Nginx 时会反复用到，自然就记住了。后续章节遇到新选项会逐一解释。

## 七、配置 Docker 加速（推荐）

### 7.1 为什么要配置加速？（目的说明）

**问题根源：** `docker pull` 拉取镜像时，默认从 Docker 官方仓库（Docker Hub）下载，而 Docker Hub 服务器在国外。国内直连普遍遇到：

- 下载速度极慢（几十 KB/s，一个大镜像要等几个小时）
- 经常直接超时失败：`net/http: request canceled while waiting for connection`（11.2 节有这个报错的解决办法）

**加速的原理：** "镜像加速器"就是部署在**国内**的 Docker Hub 缓存代理服务器。配置后 `docker pull` 优先走国内节点下载，速度快且稳定：

```
不配置加速：
docker pull ───→ 国外 Docker Hub（慢、易超时）

配置加速后：
docker pull ───→ ① 先从国内加速器下载（快）
              └─ ② 加速器没有的 → 自动回源 Docker Hub 兜底
```

**三点说明：**

- 加速器只是"换了一条下载通道"，镜像内容和官方完全一致，不影响安全
- 它不是 VPN，只对 `docker pull` 拉镜像生效，不影响其他网络访问
- 配置一次永久生效，之后所有 `docker pull`（第九章拉 MySQL、第十二章拉 Ollama 等）都自动走加速通道

**是主流做法吗？—— 是的，国内用 Docker 基本人人都配，属于"标配"操作：**

- 阿里云、腾讯云、华为云等云厂商都提供镜像加速服务，官方文档也都推荐配置
- 企业生产环境一般用自建 Harbor 私有仓库或专属加速地址
- 不配置的话拉镜像大概率超时，后续章节的 `docker pull` 都会卡住

**和 3.2 节的"镜像源"是一回事吗？** —— 不是，两者容易混淆但作用完全不同：

| 对比项 | 3.2 节的 yum 镜像源 | 本章的 Docker 镜像加速 |
|--------|--------------------|----------------------|
| 加速什么 | 安装 Docker **软件本身**（yum 下载 rpm 包） | `docker pull` 下载**容器镜像** |
| 配置文件 | `/etc/yum.repos.d/docker-ce.repo` | `/etc/docker/daemon.json` |
| 使用时机 | 装 Docker 之前（一次性） | 装好 Docker 之后（一次配置永久生效） |

**注意：** 公共加速器地址会不定期失效（2024 年后不少老加速器已关停）。如果哪天 `docker pull` 又变慢或超时，换一批新的加速器地址、重新执行一遍 7.2～7.3 即可（11.2 节有更多备选地址）。

### 7.2 创建 daemon.json 配置文件

`daemon.json` 是 Docker 引擎的主配置文件（默认不存在，需要自己创建）：

```bash
sudo tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "https://docker.m.daocloud.io",
        "https://docker.1ms.run",
        "https://hub-mirror.c.163.com",
        "https://mirror.baidubce.com",
        "https://docker.nju.edu.cn"
    ]
}
EOF
```

> 说明：
> - 可以配置多个加速器，Docker 按顺序尝试，第一个失败自动换下一个
> - 内容是 JSON 格式，冒号、逗号、引号缺一不可，改错会导致 Docker 起不来

**目前国内可用的常用加速器（建议挑 3~5 个配置即可）：**

| 加速器地址 | 提供方 | 说明 |
|----------|--------|------|
| `https://docker.m.daocloud.io` | DaoCloud | 稳定，国内大厂提供，推荐放第一个 |
| `https://docker.1ms.run` | 1ms.run | 速度较快，社区维护 |
| `https://hub-mirror.c.163.com` | 网易 | 稳定，老牌镜像源 |
| `https://mirror.baidubce.com` | 百度 | 百度云提供 |
| `https://docker.nju.edu.cn` | 南京大学 | 高校镜像，教育网速度快 |
| `https://docker.mirrors.ustc.edu.cn` | 中科大 | 高校镜像，偶尔不稳定 |
| `https://dockerproxy.com` | DockerProxy | 社区维护，可用性波动较大 |

> **注意：**
> - 2024 年起，部分公有加速器（如阿里云、腾讯云的旧地址、官方 `registry-1.docker.io.cn`）已陆续关闭或仅限自家云客户使用，**不要再用**。
> - 加速器可用性会变化，**建议配 3~5 个做冗余**，单个挂掉不影响使用。
> - 配置完一定要执行 7.3 步重启 Docker 才能生效。
> - 如果某个加速器返回 401/403 或拉镜像卡住，把那一行删掉再重启 Docker 即可。

### 7.3 重启 Docker 服务

修改 daemon.json 后必须重启 Docker 才能生效：

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### 7.4 验证加速配置

```bash
sudo docker info | grep -A 5 "Registry Mirrors"
```

输出示例：

```
 Registry Mirrors:
  https://docker.m.daocloud.io/
  https://docker.1ms.run/
```

能看到这两个地址说明配置已生效。再实际拉一个小镜像测试下载速度：

```bash
docker pull alpine
```

如果能正常拉取（约 7MB）且速度明显快，说明加速配置工作正常。

## 八、配置普通用户使用 Docker（可选）

### 8.1 创建 docker 用户组（cat /etc/group | grep docker）如有

```
[qyz@lee01 ~]$ cat /etc/group | grep docker 
docker:x:981:
```

说明已经有docker，不需要创建docker组，否则你可以执行下面的创建。

```bash
sudo groupadd docker
```

### 8.2 将当前用户加入 docker 用户组

```bash
sudo usermod -aG docker $USER
```

### 8.3 重新登录用户（要执行这一步，否则还是需要sudo去执行8.4）

```bash
newgrp docker
```

或者退出当前终端重新登录。

### 8.4 验证配置

```bash
docker run hello-world
```

如果不需要 sudo 就能运行，说明配置成功。

## 九、在 Docker 中安装 MySQL

### 9.1 拉取 MySQL 镜像

```bash
# 拉取最新版本的 MySQL
docker pull mysql:latest

# 拉取指定版本的 MySQL（推荐）
docker pull mysql:8.0

# 拉取 MySQL 5.7 版本
docker pull mysql:5.7
```

### 9.2 查看 MySQL 镜像

```bash
docker images
```

输出示例：

```
REPOSITORY    TAG       IMAGE ID       CREATED        SIZE
mysql         8.0       3218b38490ce   2 weeks ago    516MB
mysql         latest    3218b38490ce   2 weeks ago    516MB
```

### 9.3 创建 MySQL 数据目录（推荐）

#### 为什么要做这一步？

**核心目的：让 MySQL 的数据"住"在宿主机上，而不是容器里。**

Docker 容器有一个致命特性：**容器被删除（`docker rm`）时，容器内部的所有数据都会一起消失**。MySQL 的数据默认存在容器内的 `/var/lib/mysql` 目录，如果你哪天 `docker rm mysql` 重建容器，所有数据库、表、数据**全部没了**，找不回来。

解决办法就是用 **数据卷挂载（`-v`）**，把容器内的 `/var/lib/mysql` 映射到宿主机的 `/data/mysql`。这样：
- 数据实际写在宿主机的 `/data/mysql`
- 容器删了、重建了，只要挂载路径不变，数据**依然还在**
- 就像把硬盘从旧电脑拔下来插到新电脑上，文件一个都不丢

#### 两条命令分别干什么？

```bash
# 创建数据目录，用于持久化存储
mkdir -p /data/mysql
```
- `mkdir -p`：递归创建目录（父目录不存在也一起建）
- `/data/mysql`：宿主机上专门放 MySQL 数据的地方，**别跟容器内路径搞混**

```bash
# 设置权限
sudo chown -R 1000:1000 /data/mysql
```
- `chown -R 1000:1000`：把目录的**所有者和所属组**都改成 UID 1000
- **为什么是 1000？** 因为 MySQL 官方镜像里，mysqld 进程是用 `mysql` 用户跑的，这个用户的 UID 恰好是 `1000`。如果宿主机目录不归 1000 所有，容器内的 mysql 用户就**没权限写数据**，启动会报 `Permission denied`。
- `-R`：递归，子目录和文件一起改

> **一句话总结**：这一步是给 MySQL 的数据"在宿主机上安个家"，并把家门钥匙交给容器内的 mysql 用户。没这一步，数据会随容器消失，或者根本写不进去。

```bash
# 创建数据目录，用于持久化存储
mkdir -p /data/mysql

# 设置权限
sudo chown -R 1000:1000 /data/mysql
```

### 9.4 运行 MySQL 容器

#### 为什么要做这一步？

前面 9.1 拉了镜像、9.3 建了数据目录，这一步是**把镜像"跑起来"变成一个真正能用的 MySQL 服务**。

打个比方：
- **镜像** = 一张"系统安装光盘"，放在那不动是没用的
- **容器** = 用光盘装好、开机运行的电脑，能对外提供服务

这一步 `docker run` 干了 4 件关键的事：

1. **启动 MySQL 进程**：让 mysqld 真正跑起来，开始监听 3306 端口
2. **暴露端口**：`-p 3306:3306` 把容器的 3306 映射到宿主机，这样你的 Django、Navicat 才能连上来
3. **初始化账号**：通过 `-e` 环境变量设置 root 密码、创建默认库和用户，不用再手动进容器配置
4. **（方式二）持久化 + 自启**：挂载数据目录保证数据不丢，`--restart=always` 保证服务器重启后 MySQL 自动起来

> **不做这一步会怎样？** 镜像只是躺在硬盘上的文件，没有运行中的 MySQL 进程，你的应用连不上数据库。

**方式一：简单运行（测试用）**

```bash
docker run -d \
    --name mysql \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=123456 \
    mysql:8.0
```

**方式二：持久化运行（生产环境推荐）**

```bash
docker run -d \
    --name mysql \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=123456 \
    -e MYSQL_DATABASE=mydb \
    -e MYSQL_USER=qyz \
    -e MYSQL_PASSWORD=050169qwer \
    -v /data/mysql:/var/lib/mysql \
    --restart=always \
    mysql:8.0
```

**参数说明：**

| 参数 | 说明 |
|------|------|
| `-d` | 后台运行容器 |
| `--name mysql` | 容器名称 |
| `-p 3306:3306` | 端口映射（主机:容器） |
| `-e MYSQL_ROOT_PASSWORD=123456` | 设置 root 密码 |
| `-e MYSQL_DATABASE=mydb` | 创建默认数据库 |
| `-e MYSQL_USER=myuser` | 创建普通用户 |
| `-e MYSQL_PASSWORD=mypassword` | 设置普通用户密码 |
| `-v /data/mysql:/var/lib/mysql` | 数据卷挂载（持久化数据） |
| `--restart=always` | 容器重启策略（自动重启） |

### 9.5 查看 MySQL 容器状态

```bash
# 查看运行中的容器
docker ps

# 查看所有容器
docker ps -a
```

输出示例：

```
CONTAINER ID   IMAGE      COMMAND                  CREATED         STATUS         PORTS                 NAMES
abc123def456   mysql:8.0  "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   0.0.0.0:3306->3306/tcp   mysql
```

### 9.6 验证 MySQL 安装

**方法一：查看容器日志**

```bash
docker logs mysql
```

输出示例（关键部分）：

```
2023-10-09T10:30:00.000000Z 0 [System] [MY-010931] [Server] /usr/sbin/mysqld: ready for connections. Version: '8.0.35'  socket: '/var/run/mysqld/mysqld.sock'  port: 3306  MySQL Community Server - GPL.
```

**方法二：进入容器测试**

```bash
# 进入 MySQL 容器
docker exec -it mysql bash

# 在容器内登录 MySQL
mysql -u root -p
```

输入密码后，如果看到以下提示，说明 MySQL 安装成功：

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.35 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

```
另外一个写法：docker exec -it mysql mysql -uroot -p    # 输入密码，这个是直接进入mysql
```

**方法三：在宿主机测试连接**

```bash
# 在宿主机使用 mysql 客户端连接（需要安装 mysql-client）
mysql -h 127.0.0.1 -P 3306 -u root -p   
```

**方法四：使用其他工具测试连接**

使用 Navicat、DataGrip 等数据库工具连接：
- 主机：虚拟机 IP 地址（如 192.168.1.100）
- 端口：3306
- 用户：root
- 密码：123456

### 9.7 常用 MySQL 容器操作

```bash
# 停止 MySQL 容器
docker stop mysql

# 启动 MySQL 容器
docker start mysql

# 重启 MySQL 容器
docker restart mysql

# 删除 MySQL 容器（数据会保留在 /data/mysql）
docker rm mysql

# 查看 MySQL 容器详情
docker inspect mysql

# 复制文件到容器
docker cp /host/path/file.sql mysql:/container/path/

# 从容器导出数据库
docker exec mysql mysqldump -u root -p mydb > /data/mysql/mydb.sql

# 导入数据库到容器
docker exec -i mysql mysql -u root -p mydb < /data/mysql/mydb.sql
```

### 9.8 MySQL 配置说明

**修改 MySQL 配置文件：**

```bash
# 在宿主机创建配置文件
mkdir -p /data/mysql/conf

# 创建 my.cnf 文件
sudo tee /data/mysql/conf/my.cnf <<-'EOF'
[mysqld]
character-set-server=utf8mb4
collation-server=utf8mb4_unicode_ci
max_connections=1000
innodb_buffer_pool_size=512M
EOF

# 重新运行容器并挂载配置文件
docker run -d \
    --name mysql \
    -p 3306:3306 \
    -e MYSQL_ROOT_PASSWORD=123456 \
    -v /data/mysql:/var/lib/mysql \
    -v /data/mysql/conf:/etc/mysql/conf.d \
    --restart=always \
    mysql:8.0
```

## 十一、在 Docker 中安装 Nginx

### 11.1 什么是 Nginx

Nginx 是一个高性能的 HTTP 和反向代理服务器，特点：
- 高性能、高并发
- 内存占用低
- 配置简单灵活
- 支持负载均衡、缓存、SSL 等

### 11.2 拉取 Nginx 镜像

```bash
# 拉取最新版本的 Nginx
docker pull nginx:latest

# 拉取指定版本的 Nginx
docker pull nginx:1.28

# 查看镜像
docker images | grep nginx
```

输出示例：

```
nginx           latest    0a97eee8041e   2 days ago     187MB
nginx           1.25      0a97eee8041e   2 days ago     187MB
```

#### 🔧 如果拉取失败（网络问题）

如果执行 `docker pull nginx:latest` 时出现以下错误：

```
Error response from daemon: Get "https://registry-1.docker.io/v2/": net/http: request canceled while waiting for connection
```

说明无法访问 Docker 官方仓库，需要配置国内镜像加速：

```bash
# 删除旧配置
sudo rm /etc/docker/daemon.json

# 创建新配置（包含 DNS、超时等设置）
sudo tee /etc/docker/daemon.json <<-'EOF'
{
    "registry-mirrors": [
        "https://docker.m.daocloud.io",
        "https://docker.1ms.run",
        "https://hub-mirror.c.163.com",
        "https://mirror.baidubce.com"
    ],
    "dns": ["8.8.8.8", "114.114.114.114"],
    "max-concurrent-downloads": 10,
    "max-download-attempts": 5,
    "log-driver": "json-file",
    "log-level": "warn"
}
EOF

# 重启 Docker
sudo systemctl daemon-reload
sudo systemctl restart docker

# 验证配置
sudo docker info | grep -A 5 "Registry Mirrors"
```

**验证成功输出：**

```
 Registry Mirrors:
  https://docker.m.daocloud.io/
  https://docker.1ms.run/
  https://hub-mirror.c.163.com/
  https://mirror.baidubce.com/
```

配置完成后，重新拉取镜像：

```bash
docker pull nginx:latest
```

### 11.3 运行 Nginx 容器

**方式一：简单运行（测试用）**

```bash
docker run -d \
    --name nginx \
    -p 80:80 \
    nginx:latest
```

**方式二：持久化运行（生产环境推荐）**

#### 为什么要做这一步？

和 MySQL 一样的道理：**容器一删，里面的东西全没**。Nginx 容器里有 3 类重要数据必须"搬"到宿主机：

| 容器内路径 | 是什么 | 不挂载的后果 |
|---|---|---|
| `/usr/share/nginx/html` | 网站静态文件（html/css/js/图片） | 容器重建后，你传上去的网站文件全没了 |
| `/etc/nginx/conf.d` | Nginx 配置文件（default.conf 等） | 每次重建容器都要重写配置，非常痛苦 |
| `/var/log/nginx` | 访问日志、错误日志 | 出问题时查不到日志，无法排查 |

所以方式二做了 3 件事：
1. 在宿主机建 3 个目录，给这 3 类数据"安家"
2. 用 `-v` 把宿主机目录映射到容器内对应路径
3. 以后改网站、改配置、看日志，**直接在宿主机操作**，不用进容器

> **对比方式一**：方式一只是"跑起来看看"，容器一删所有东西归零；方式二才是"真正能用"的部署，数据和配置都持久化在宿主机上。**生产环境必须用方式二。**

#### 三个挂载目录分别干什么？

```bash
-v /data/nginx/html:/usr/share/nginx/html   # 网站文件
-v /data/nginx/conf:/etc/nginx/conf.d       # 配置文件
-v /data/nginx/logs:/var/log/nginx          # 日志文件
```

- **html**：你把前端打包好的 `dist` 目录文件扔到 `/data/nginx/html`，浏览器直接就能访问，不用进容器拷贝
- **conf**：你在宿主机写 `default.conf`、改端口、配反向代理，改完 `docker restart nginx` 或 `nginx -s reload` 就生效
- **logs**：访问日志 `access.log` 和错误日志 `error.log` 直接躺在宿主机，`tail -f` 就能实时看，排查问题必备

```bash
# 创建目录结构
mkdir -p /data/nginx/html
mkdir -p /data/nginx/conf
mkdir -p /data/nginx/logs

# 创建默认页面
sudo tee /data/nginx/html/index.html <<-'EOF'
<!DOCTYPE html>
<html>
<head>
    <title>Welcome to Nginx!</title>
</head>
<body>
    <h1>Hello, Nginx is running in Docker!</h1>
    <p>This is a test page.</p>
</body>
</html>
EOF

# 运行容器
docker run -d \
    --name nginx \
    -p 80:80 \
    -p 443:443 \
    -v /data/nginx/html:/usr/share/nginx/html \
    -v /data/nginx/conf:/etc/nginx/conf.d \
    -v /data/nginx/logs:/var/log/nginx \
    --restart=always \
    nginx:latest
```

**参数说明：**

| 参数 | 说明 |
|------|------|
| `-d` | 后台运行容器 |
| `--name nginx` | 容器名称 |
| `-p 80:80` | HTTP 端口映射 |
| `-p 443:443` | HTTPS 端口映射 |
| `-v /data/nginx/html:/usr/share/nginx/html` | 网站文件挂载 |
| `-v /data/nginx/conf:/etc/nginx/conf.d` | 配置文件挂载 |
| `-v /data/nginx/logs:/var/log/nginx` | 日志文件挂载 |
| `--restart=always` | 容器重启策略（自动重启） |

### 11.4 查看 Nginx 容器状态

```bash
# 查看运行中的容器
docker ps | grep nginx

# 查看容器日志
docker logs nginx
```

输出示例：

```
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                   NAMES
abc123def456   nginx     "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   0.0.0.0:80->80/tcp, 0.0.0.0:443->443/tcp   nginx
```

### 11.5 验证 Nginx 安装

**方法一：查看容器日志**

```bash
docker logs nginx
```

输出示例：

```
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/10/09 10:30:00 [notice] 1#1: using the "epoll" event method
2023/10/09 10:30:00 [notice] 1#1: nginx/1.25.2
2023/10/09 10:30:00 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2023/10/09 10:30:00 [notice] 1#1: OS: Linux 3.10.0-1160.83.1.el7.x86_64
2023/10/09 10:30:00 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1048576:1048576
2023/10/09 10:30:00 [notice] 1#1: start worker processes
2023/10/09 10:30:00 [notice] 1#1: start worker process 31
2023/10/09 10:30:00 [notice] 1#1: start worker process 32
```

**方法二：在宿主机测试**

```bash
curl http://localhost
```

输出示例：

```html
<!DOCTYPE html>
<html>
<head>
    <title>Welcome to Nginx!</title>
</head>
<body>
    <h1>Hello, Nginx is running in Docker!</h1>
    <p>This is a test page.</p>
</body>
</html>
```

**方法三：使用浏览器访问**

打开浏览器，访问 `http://虚拟机IP`，如果看到 Nginx 默认页面或自定义页面，说明安装成功。

### 11.6 Nginx 配置说明

> **先建立整体认知**：Nginx 的配置是**层层嵌套**的，像套娃一样：
> ```
> nginx.conf（主配置，管全局）
>   └── http {}（HTTP 协议相关，管所有网站）
>         └── server {}（一个虚拟主机 = 一个网站）
>               └── location {}（按 URL 路径匹配，决定怎么处理）
> ```
> 我们下面写的这些 `.conf` 文件，本质上就是一个个 `server {}` 块，会被主配置自动 `include` 进来。

#### 11.6.1 基本静态网站配置（default.conf）

这个配置的作用：**让 Nginx 当一个静态文件服务器**，访问 `http://服务器IP/xxx` 时，从指定目录里把对应文件返回给浏览器。

```bash
sudo tee /data/nginx/conf/default.conf <<-'EOF'
server {                                  # 一个 server 块 = 一个网站（虚拟主机）
    listen 80;                            # 监听 80 端口（HTTP 默认端口）
    server_name localhost;                # 匹配的域名，localhost 或没匹配到其他时走这里

    root /usr/share/nginx/html;           # 网站根目录：所有请求都从这个目录下找文件
    index index.html index.htm;           # 访问目录时，默认返回的首页文件名（按顺序找）

    location / {                          # 匹配所有以 / 开头的 URL（即所有请求）
        try_files $uri $uri/ =404;        # 找文件的顺序：
                                          #   $uri      → 直接找同名文件（如 /a.png）
                                          #   $uri/     → 找同名目录下的首页（如 /a/ → /a/index.html）
                                          #   =404     → 都找不到就返回 404
    }

    error_page 404 /404.html;             # 发生 404 时，返回 /404.html 这个页面
    location = /404.html {                # = 表示精确匹配 /404.html 这个路径
        root /usr/share/nginx/html;       # 404.html 也在这个目录下找
    }

    error_page 500 502 503 504 /50x.html; # 服务器错误（5xx）时返回 /50x.html
    location = /50x.html {
        root /usr/share/nginx/html;
    }
}
EOF

docker restart nginx
```

**逐条解释**：

| 指令 | 作用 | 生活化理解 |
|---|---|---|
| `listen 80` | 监听 80 端口 | 相当于"在 80 号窗口接客"，浏览器默认敲 80 端口 |
| `server_name` | 匹配域名 | 同一个 Nginx 可以托管多个网站，靠域名区分 |
| `root` | 文件根目录 | 告诉 Nginx"去这个文件夹里翻文件" |
| `index` | 默认首页 | 访问目录时，优先显示哪个文件 |
| `location /` | 路径匹配 | `location` 是 Nginx 最核心的概念，决定不同 URL 怎么走 |
| `try_files` | 按顺序找文件 | 像查字典：先查这个词，再查这个词的目录，查不到就 404 |
| `error_page` | 错误页 | 出错时给用户看的友好页面，而不是 Nginx 默认的丑页面 |

---

#### 11.6.2 反向代理配置（proxy.conf）

这个配置的作用：**Nginx 自己不干活，把请求转发给后端服务**（比如你的 Django 跑在 8000 端口）。

为什么叫"反向"代理？打个比方：
- **正向代理**：你翻墙用的 VPN，你知道真实服务器是谁，代理替你访问。
- **反向代理**：用户只知道 Nginx，不知道后面的 Django 是谁。Nginx 像前台接待，把请求转给后台的开发。

```bash
sudo tee /data/nginx/conf/proxy.conf <<-'EOF'
server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://backend_server:8080;   # 把请求转发到后端的 8080 端口
        proxy_set_header Host $host;             # 把原始域名传给后端（后端要靠它判断是哪个站点）
        proxy_set_header X-Real-IP $remote_addr; # 把真实客户端 IP 传给后端（$remote_addr 是 Nginx 拿到的客户端 IP）
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; # 传递完整的代理链路 IP（每经过一个代理就追加一个）
        proxy_set_header X-Forwarded-Proto $scheme; # 告诉后端原始请求是 http 还是 https
    }
}
EOF

docker restart nginx
```

**逐条解释**：

| 指令 | 作用 | 为什么必须写 |
|---|---|---|
| `proxy_pass` | 转发目标地址 | 核心指令，没有它就不是反向代理了 |
| `Host $host` | 传递原始域名 | 后端 Django 可能要根据 Host 判断用哪个配置，不转会丢信息 |
| `X-Real-IP` | 真实客户端 IP | 不写的话，后端拿到的 IP 全是 Nginx 的，日志里看不到真实用户 |
| `X-Forwarded-For` | 代理链路 IP | 经过多层代理时，记录完整路径，方便排查 |
| `X-Forwarded-Proto` | 原始协议 | 后端要知道用户是 http 还是 https 访问的，否则跳转链接会出错 |

> **开发必知**：这 4 个 `proxy_set_header` 几乎是**反向代理的标配**，写 Django/Flask 项目时，`request.META['REMOTE_ADDR']` 能不能拿到真实 IP，就靠 `X-Real-IP` 和 `X-Forwarded-For`。

---

#### 11.6.3 负载均衡配置（loadbalancing.conf）

这个配置的作用：**把请求分摊到多台后端服务器**，提升并发能力和可用性。一台挂了，其他台还能继续服务。

```bash
sudo tee /data/nginx/conf/loadbalancing.conf <<-'EOF'
upstream backend {                         # 定义一个后端服务器组，名字叫 backend
    server server1:8080 weight=1;          # 成员 1，权重 1
    server server2:8080 weight=1;          # 成员 2，权重 1
    server server3:8080 weight=2;          # 成员 3，权重 2（分到的请求是前两台的 2 倍）
}

server {
    listen 80;
    server_name www.example.com;

    location / {
        proxy_pass http://backend;         # 转发到上面定义的 backend 组，Nginx 自动选一台
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
EOF

docker restart nginx
```

**逐条解释**：

| 指令 | 作用 |
|---|---|
| `upstream backend {}` | 定义服务器池，名字随便起，下面 `proxy_pass` 要引用 |
| `server 地址:端口 weight=N` | 池里的一台机器，`weight` 是权重，越大分到的请求越多 |
| `proxy_pass http://backend` | 转发到池子，Nginx 默认按**轮询 + 权重**分配请求 |

> **分配比例怎么算**？上面 3 台权重是 1:1:2，总共 4 份。每来 4 个请求，server1 拿 1 个，server2 拿 1 个，server3 拿 2 个。配置高的机器可以给大权重。

---

#### 11.6.4 HTTPS / SSL 配置（ssl.conf）

这个配置的作用：**开启 HTTPS 加密传输**，让用户访问 `https://你的域名` 时，数据是加密的，不会被中间人窃听或篡改。

```bash
# 1. 生成自签名证书（仅测试用！正式上线要用 Let's Encrypt 等免费证书）
mkdir -p /data/nginx/certs
openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /data/nginx/certs/nginx.key \      # 私钥文件（绝不能泄露）
    -out /data/nginx/certs/nginx.crt \         # 证书文件（可以公开）
    -subj "/CN=localhost"                       # CN=Common Name，写域名，测试用 localhost

# 2. 创建 HTTPS 配置
sudo tee /data/nginx/conf/ssl.conf <<-'EOF'
server {
    listen 443 ssl;                          # 监听 443 端口（HTTPS 默认），并开启 SSL
    server_name localhost;

    ssl_certificate /etc/nginx/conf.d/certs/nginx.crt;      # 证书文件路径（容器内路径）
    ssl_certificate_key /etc/nginx/conf.d/certs/nginx.key;  # 私钥文件路径

    ssl_protocols TLSv1.2 TLSv1.3;           # 只允许较新的 TLS 版本（禁用不安全的 SSLv3、TLS1.0）
    ssl_ciphers HIGH:!aNULL:!MD5;            # 加密套件：只允许高强度的，禁用空加密和 MD5

    root /usr/share/nginx/html;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
}

# 3. 把所有 HTTP 请求自动跳转到 HTTPS
server {
    listen 80;
    server_name localhost;
    return 301 https://$server_name$request_uri;   # 301 永久重定向到 https
}
EOF

# 4. 重新运行容器（多挂载一个证书目录）
docker stop nginx
docker rm nginx

docker run -d \
    --name nginx \
    -p 80:80 \
    -p 443:443 \
    -v /data/nginx/html:/usr/share/nginx/html \
    -v /data/nginx/conf:/etc/nginx/conf.d \
    -v /data/nginx/logs:/var/log/nginx \
    -v /data/nginx/certs:/etc/nginx/conf.d/certs \   # 多挂这一个，把证书传进容器
    --restart=always \
    nginx:latest
```

**逐条解释**：

| 指令 | 作用 | 生活化理解 |
|---|---|---|
| `listen 443 ssl` | 在 443 端口开启加密监听 | 443 是 HTTPS 的"专用窗口"，`ssl` 表示要加密 |
| `ssl_certificate` | 证书文件（公钥） | 相当于"营业许可证"，浏览器会校验它是不是真的 |
| `ssl_certificate_key` | 私钥文件 | 相当于"许可证的防伪印章"，只有服务器持有 |
| `ssl_protocols` | 允许的 TLS 版本 | 只让用安全的加密方式，老旧的 SSLv3 会被攻击，必须禁 |
| `ssl_ciphers` | 加密算法套件 | 同上，只挑安全的算法组合 |
| `return 301 https://...` | 301 永久重定向 | 用户敲 `http://` 时，自动跳转到 `https://`，地址栏变绿锁 |

> **重要提醒**：上面用 `openssl` 生成的是**自签名证书**，浏览器会弹"不安全"警告。**生产环境必须用正规证书**（推荐 Let's Encrypt 免费证书），否则用户不敢用。

---

#### 11.6.5 改完配置怎么生效？

```bash
# 方式 1：重启容器（简单粗暴，但会短暂中断服务）
docker restart nginx

# 方式 2：只重新加载配置（推荐，不中断服务）
docker exec nginx nginx -s reload

# 改之前先检查配置有没有语法错误（强烈建议！）
docker exec nginx nginx -t
```

**开发建议**：每次改完配置先跑 `nginx -t`，没问题再 `reload`。这是运维的基本素养，能避免把线上搞挂。

### 11.7 常用 Nginx 容器操作

```bash
# 停止 Nginx 容器
docker stop nginx

# 启动 Nginx 容器
docker start nginx

# 重启 Nginx 容器
docker restart nginx

# 删除 Nginx 容器
docker rm nginx

# 查看 Nginx 容器详情
docker inspect nginx

# 查看 Nginx 容器资源使用情况
docker stats nginx

# 进入 Nginx 容器
docker exec -it nginx bash

# 查看 Nginx 配置文件
docker exec nginx cat /etc/nginx/nginx.conf

# 查看 Nginx 版本
docker exec nginx nginx -v

# 验证 Nginx 配置
docker exec nginx nginx -t

# 重新加载 Nginx 配置（不重启容器）
docker exec nginx nginx -s reload
```

### 11.8 Nginx 日志管理

```bash
# 查看访问日志
cat /data/nginx/logs/access.log

# 查看错误日志
cat /data/nginx/logs/error.log

# 实时查看日志
tail -f /data/nginx/logs/access.log

# 查看最近 100 条日志
tail -n 100 /data/nginx/logs/access.log

# 统计访问量
awk '{print $1}' /data/nginx/logs/access.log | sort | uniq -c | sort -rn

# 统计状态码
awk '{print $9}' /data/nginx/logs/access.log | sort | uniq -c | sort -rn
```

### 11.9 Nginx 性能优化建议

**1. 调整 worker 进程数**

```bash
# 修改主配置文件
sudo tee /data/nginx/conf/nginx.conf <<-'EOF'
user nginx;
worker_processes auto;
worker_cpu_affinity auto;

events {
    worker_connections 1024;
    use epoll;
}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    sendfile on;
    tcp_nopush on;
    tcp_nodelay on;

    keepalive_timeout 65;

    gzip on;
    gzip_disable "msie6";
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    include /etc/nginx/conf.d/*.conf;
}
EOF

# 重启容器
docker restart nginx
```

**2. 配置缓存**

```bash
# 添加缓存配置到虚拟主机配置
sudo tee /data/nginx/conf/default.conf <<-'EOF'
server {
    listen 80;
    server_name localhost;

    root /usr/share/nginx/html;
    index index.html index.htm;

    location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
        expires 30d;
        add_header Cache-Control "public, no-transform";
    }

    location / {
        try_files $uri $uri/ =404;
    }
}
EOF

# 重启容器
docker restart nginx
```

### 11.10 常见问题解决

**问题 1：端口被占用**

```bash
# 查看端口占用情况
netstat -tlnp | grep 80

# 停止占用端口的服务
sudo systemctl stop httpd
sudo systemctl disable httpd

# 或者使用其他端口
docker run -d \
    --name nginx \
    -p 8080:80 \
    -v /data/nginx/html:/usr/share/nginx/html \
    --restart=always \
    nginx:latest
```

**问题 2：配置文件错误**

```bash
# 检查配置文件语法
docker exec nginx nginx -t

# 查看错误日志
cat /data/nginx/logs/error.log
```

**问题 3：无法访问网站**

```bash
# 检查防火墙
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --add-port=443/tcp --permanent
sudo firewall-cmd --reload

# 检查 SELinux
sudo setenforce 0

# 检查容器端口映射
docker ps | grep nginx
```

## 十二、在 Docker 中安装 Ollama

### 12.1 什么是 Ollama

Ollama 是一个用于在本地运行大语言模型（LLM）的工具，支持多种开源模型，如：
- Llama 3、Llama 2
- Mistral
- Qwen（通义千问）
- Gemma
- 等等

### 12.2 拉取 Ollama 镜像

```bash
# 拉取最新版本的 Ollama
docker pull ollama/ollama:latest

# 查看镜像
docker images | grep ollama
```

输出示例：

```
ollama/ollama   latest    4a6b6b4d7b8c   2 days ago     2.1GB
```

### 12.3 运行 Ollama 容器

**方式一：简单运行（测试用）**

```bash
docker run -d \
    --name ollama \
    -p 11434:11434 \
    ollama/ollama:latest
```

**方式二：持久化运行（生产环境推荐）**

```bash
# 创建数据目录
mkdir -p /data/ollama

# 设置权限
sudo chown -R 1000:1000 /data/ollama

# 运行容器
docker run -d \
    --name ollama \
    -p 11434:11434 \
    -v /data/ollama:/root/.ollama \
    --restart=always \
    ollama/ollama:latest
```

**参数说明：**

| 参数 | 说明 |
|------|------|
| `-d` | 后台运行容器 |
| `--name ollama` | 容器名称 |
| `-p 11434:11434` | 端口映射（Ollama 默认端口 11434） |
| `-v /data/ollama:/root/.ollama` | 数据卷挂载（持久化模型数据） |
| `--restart=always` | 容器重启策略（自动重启） |

### 12.4 查看 Ollama 容器状态

```bash
# 查看运行中的容器
docker ps | grep ollama

# 查看容器日志
docker logs ollama
```

输出示例：

```
CONTAINER ID   IMAGE              COMMAND                  CREATED         STATUS         PORTS                     NAMES
abc123def456   ollama/ollama      "/bin/ollama serve"      2 minutes ago   Up 2 minutes   0.0.0.0:11434->11434/tcp   ollama
```

### 12.5 下载和运行模型

**进入 Ollama 容器**

```bash
docker exec -it ollama bash
```

**下载模型**

```bash
# 下载 Llama 3.2（最新版本，推荐）
ollama pull llama3.2

# 下载 Llama 3
ollama pull llama3

# 下载 Qwen 2.5（通义千问）
ollama pull qwen2.5

# 下载 Mistral
ollama pull mistral

# 下载 Gemma 2
ollama pull gemma2

# 下载小模型（适合测试）
ollama pull phi3
```

**查看已下载的模型**

```bash
ollama list
```

输出示例：

```
NAME                    ID              SIZE    MODIFIED
llama3.2:latest         a80c4f17acd5    4.7 GB  2 minutes ago
qwen2.5:latest          8b5c6d3e9f7a    4.2 GB  5 minutes ago
```

**运行模型进行对话**

```bash
# 使用 Llama 3.2 进行对话
ollama run llama3.2

# 使用 Qwen 2.5 进行对话
ollama run qwen2.5

# 使用特定大小的模型
ollama run llama3.2:1b    # 1B 参数版本（小）
ollama run llama3.2:3b    # 3B 参数版本（中）
ollama run llama3.2:11b   # 11B 参数版本（大）
```

**对话示例**

```
>>> 你好，请介绍一下你自己
我是 Llama 3.2，一个由 Meta 训练的大型语言模型。我可以帮助你回答问题、生成文本、翻译语言等。

>>> 什么是 Docker？
Docker 是一个开源的容器化平台，它允许开发者将应用程序及其依赖项打包到一个轻量级、可移植的容器中...
```

### 12.6 通过 API 访问 Ollama

Ollama 提供了 REST API，可以通过 HTTP 请求访问。

**测试 API 连接**

```bash
# 在宿主机测试
curl http://localhost:11434/api/generate -d '{
  "model": "llama3.2",
  "prompt": "你好，请介绍一下你自己"
}'
```

**使用 Python 调用 Ollama API**

```python
import requests

# API 端点
url = "http://localhost:11434/api/generate"

# 请求数据
data = {
    "model": "llama3.2",
    "prompt": "你好，请介绍一下你自己",
    "stream": False  # 是否流式输出
}

# 发送请求
response = requests.post(url, json=data)

# 打印结果
print(response.json()['response'])
```

**使用 JavaScript/Node.js 调用 Ollama API**

```javascript
const fetch = require('node-fetch');

async function chatWithOllama() {
    const response = await fetch('http://localhost:11434/api/generate', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({
            model: 'llama3.2',
            prompt: '你好，请介绍一下你自己',
            stream: false
        })
    });

    const data = await response.json();
    console.log(data.response);
}

chatWithOllama();
```

### 12.7 常用 Ollama 命令

```bash
# 列出所有已下载的模型
ollama list

# 删除模型
ollama rm llama3.2

# 查看模型信息
ollama show llama3.2

# 复制模型
ollama cp llama3.2 my-model

# 查看运行中的模型
ollama ps

# 创建自定义模型
ollama create my-model -f Modelfile

# 查看模型版本
ollama version
```

### 12.8 常用 Ollama 容器操作

```bash
# 停止 Ollama 容器
docker stop ollama

# 启动 Ollama 容器
docker start ollama

# 重启 Ollama 容器
docker restart ollama

# 删除 Ollama 容器（模型数据会保留在 /data/ollama）
docker rm ollama

# 查看 Ollama 容器详情
docker inspect ollama

# 查看 Ollama 容器资源使用情况
docker stats ollama
```

### 12.9 Ollama 配置说明

**创建 Modelfile（自定义模型）**

```bash
# 在宿主机创建 Modelfile
mkdir -p /data/ollama/models

# 创建 Modelfile
sudo tee /data/ollama/models/Modelfile <<-'EOF'
FROM llama3.2

# 设置系统提示词
SYSTEM You are a helpful AI assistant that speaks Chinese.

# 设置参数
PARAMETER temperature 0.7
PARAMETER top_p 0.9
PARAMETER num_ctx 4096

# 设置模板
TEMPLATE """
{{- range .Messages }}
{{- if .System }}<|start_header_id|>system<|end_header_id|>

{{ .System }}<|eot_id|>{{ end }}
{{- if .User }}<|start_header_id|>user<|end_header_id|>

{{ .User }}<|eot_id|>{{ end }}
{{- if .Assistant }}<|start_header_id|>assistant<|end_header_id|>

{{ .Assistant }}<|eot_id|>{{ end }}
{{- end }}
<|start_header_id|>assistant<|end_header_id|>

"""
EOF

# 在容器内创建自定义模型
docker exec -it ollama ollama create my-model -f /root/.ollama/models/Modelfile
```

### 12.10 性能优化建议

**1. GPU 加速（如果有 NVIDIA GPU）**

```bash
# 安装 NVIDIA Container Toolkit
sudo yum install -y nvidia-container-toolkit

# 配置 Docker
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker

# 使用 GPU 运行 Ollama
docker run -d \
    --name ollama \
    --gpus all \
    -p 11434:11434 \
    -v /data/ollama:/root/.ollama \
    --restart=always \
    ollama/ollama:latest
```

**2. 内存限制**

```bash
# 限制容器内存使用
docker run -d \
    --name ollama \
    --memory="8g" \
    --memory-swap="8g" \
    -p 11434:11434 \
    -v /data/ollama:/root/.ollama \
    --restart=always \
    ollama/ollama:latest
```

**3. 模型选择建议**

| 用途 | 推荐模型 | 内存要求 | 速度 |
|------|----------|----------|------|
| 测试学习 | phi3:mini | 2GB | 快 |
| 日常对话 | llama3.2:3b | 4GB | 快 |
| 代码生成 | qwen2.5:7b | 8GB | 中 |
| 复杂任务 | llama3.2:11b | 16GB | 慢 |
| 高质量输出 | llama3.2:70b | 64GB | 很慢 |

### 12.11 验证 Ollama 安装

**方法一：查看容器日志**

```bash
docker logs ollama
```

输出示例：

```
time=2023-10-09T10:30:00.000Z level=INFO msg="Starting ollama server"
time=2023-10-09T10:30:00.100Z level=INFO msg="Listening on [::]:11434"
```

**方法二：测试 API 连接**

```bash
curl http://localhost:11434/api/tags
```

输出示例：

```json
{
  "models": [
    {
      "name": "llama3.2:latest",
      "modified_at": "2023-10-09T10:30:00Z",
      "size": 4724461392
    }
  ]
}
```

**方法三：运行对话测试**

```bash
docker exec -it ollama ollama run llama3.2 "你好"
```

**方法四：使用 Web 界面（可选）**

```bash
# 安装 Open WebUI（提供 Web 界面）
docker run -d \
    --name open-webui \
    -p 3000:8080 \
    --add-host=host.docker.internal:host-gateway \
    -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
    --restart=always \
    ghcr.io/open-webui/open-webui:main
```

访问 `http://虚拟机IP:3000` 即可使用 Web 界面与 Ollama 对话。

### 12.12 常见问题解决

**问题 1：模型下载速度慢**

```bash
# 使用国内镜像加速
docker run -d \
    --name ollama \
    -p 11434:11434 \
    -v /data/ollama:/root/.ollama \
    -e OLLAMA_HOST=0.0.0.0 \
    --restart=always \
    ollama/ollama:latest
```

**问题 2：内存不足**

```bash
# 选择更小的模型
ollama pull llama3.2:1b

# 或者限制并发
docker run -d \
    --name ollama \
    --memory="4g" \
    -p 11434:11434 \
    -v /data/ollama:/root/.ollama \
    --restart=always \
    ollama/ollama:latest
```

**问题 3：无法从外部访问**

```bash
# 检查防火墙
sudo firewall-cmd --add-port=11434/tcp --permanent
sudo firewall-cmd --reload

# 检查端口监听
netstat -tlnp | grep 11434
```

## 十三、Docker 常用命令

### 13.1 镜像管理

```bash
# 搜索镜像
docker search ubuntu

# 拉取镜像
docker pull ubuntu:latest

# 查看本地镜像
docker images

# 查看镜像详情
docker inspect IMAGE_ID

# 删除镜像
docker rmi IMAGE_ID

# 强制删除镜像（即使有容器使用）
docker rmi -f IMAGE_ID

# 删除所有无用镜像
docker image prune

# 删除所有未使用的镜像
docker image prune -a

# 保存镜像为 tar 文件
docker save -o ubuntu.tar ubuntu:latest

# 从 tar 文件加载镜像
docker load -i ubuntu.tar

# 给镜像打标签
docker tag ubuntu:latest myubuntu:v1.0

# 推送镜像到仓库
docker push myusername/myubuntu:v1.0
```

### 13.2 容器管理

```bash
# 运行容器（前台）
docker run ubuntu

# 运行容器（后台）
docker run -d ubuntu

# 运行容器（交互式）
docker run -it ubuntu bash

# 运行容器（指定名称）
docker run --name mycontainer ubuntu

# 运行容器（端口映射）
docker run -p 8080:80 nginx

# 运行容器（映射多个端口）
docker run -p 8080:80 -p 8443:443 nginx

# 运行容器（映射多个端口）(后台运行)
docker run -d -p 8080:80 -p 8443:443 nginx --name 别名

# 运行容器（数据卷挂载）
docker run -v /host/path:/container/path ubuntu 

# 运行容器（自动重启）
docker run --restart=always nginx

# 运行容器（限制资源）
docker run --cpus="0.5" --memory="512m" ubuntu

# 查看运行中的容器
docker ps

# 查看所有容器
docker ps -a

# 查看容器详情
docker inspect CONTAINER_ID

# 启动容器
docker start CONTAINER_ID

# 停止容器
docker stop CONTAINER_ID

# 强制停止容器
docker kill CONTAINER_ID

# 重启容器
docker restart CONTAINER_ID

# 删除容器
docker rm CONTAINER_ID

# 强制删除容器（即使正在运行）
docker rm -f CONTAINER_ID

# 删除所有已停止的容器
docker container prune

# 进入运行中的容器
docker exec -it CONTAINER_ID bash

# 复制文件到容器
docker cp /host/file CONTAINER_ID:/container/path

# 从容器复制文件
docker cp CONTAINER_ID:/container/path /host/file

# 查看容器资源使用情况
docker stats

# 暂停容器
docker pause CONTAINER_ID

# 恢复容器
docker unpause CONTAINER_ID
```

### 13.3 日志查看

```bash
# 查看容器日志
docker logs CONTAINER_ID

# 实时查看日志
docker logs -f CONTAINER_ID

# 查看最近 N 条日志
docker logs --tail=100 CONTAINER_ID

# 查看指定时间后的日志
docker logs --since="2023-10-01" CONTAINER_ID

# 查看日志并显示时间戳
docker logs -t CONTAINER_ID
```

### 13.4 数据卷管理

```bash
# 创建数据卷
docker volume create myvolume

# 查看数据卷
docker volume ls

# 查看数据卷详情
docker volume inspect myvolume

# 删除数据卷
docker volume rm myvolume

# 删除未使用的数据卷
docker volume prune

# 挂载数据卷到容器
docker run -v myvolume:/container/path ubuntu
```

### 13.5 网络管理

```bash
# 查看网络列表
docker network ls

# 创建网络（桥接模式）
docker network create mynetwork

# 创建网络（自定义子网）
docker network create --subnet=172.18.0.0/16 mynetwork

# 查看网络详情
docker network inspect mynetwork

# 连接容器到网络
docker network connect mynetwork CONTAINER_ID

# 断开容器与网络的连接
docker network disconnect mynetwork CONTAINER_ID

# 删除网络
docker network rm mynetwork

# 删除未使用的网络
docker network prune

# 运行容器时指定网络
docker run --network=mynetwork ubuntu
```

### 13.6 Dockerfile 相关

```bash
# 构建镜像
docker build -t myimage:v1.0 .

# 构建镜像（指定 Dockerfile 路径）
docker build -t myimage:v1.0 -f Dockerfile.prod .

# 构建镜像（不使用缓存）
docker build --no-cache -t myimage:v1.0 .

# 查看构建历史
docker history myimage:v1.0
```

> 更系统的 Dockerfile 编写教程见 **第十六章：Dockerfile 详解（重点）**。

### 13.7 Docker Compose 相关

```bash
# 启动服务
docker-compose up

# 启动服务（后台）
docker-compose up -d

# 停止服务
docker-compose down

# 查看服务日志
docker-compose logs

# 实时查看服务日志
docker-compose logs -f

# 查看运行中的容器
docker-compose ps

# 重启服务
docker-compose restart

# 重建并启动服务
docker-compose up -d --build

# 查看服务配置
docker-compose config

# 进入容器
docker-compose exec service_name bash
```

### 13.8 Docker 系统管理

```bash
# 查看 Docker 信息
docker info

# 查看 Docker 版本
docker --version

# 查看 Docker 命令帮助
docker --help

# 查看具体命令帮助
docker run --help

# 清理所有未使用的资源
docker system prune

# 清理所有资源（包括运行中的容器）
docker system prune -a -f

# 查看磁盘使用情况
docker system df

# 查看事件日志
docker events

# 查看容器进程
docker top CONTAINER_ID

# 更新 Docker 配置
docker update --restart=always CONTAINER_ID
```

## 十四、常见问题解决

### 14.1 网络问题

如果安装过程中遇到网络超时，可以尝试更换镜像源：

```bash
# 备份原 yum 配置
sudo cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak

# 使用阿里云镜像源
sudo curl -o /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo

# 清理缓存并更新
sudo yum clean all
sudo yum makecache
```

### 14.2 "没有可用软件包 docker-ce"问题

**问题描述：**
执行 `sudo yum install -y docker-ce docker-ce-cli containerd.io` 时，提示：
```
没有可用软件包 docker-ce。
没有可用软件包 docker-ce-cli。
没有可用软件包 containerd.io。
```

**原因分析：**
1. Docker 官方仓库配置不正确或未生效
2. CentOS 版本太旧（如 CentOS 6）或已停止维护（如 CentOS 8）
3. 系统架构不支持（非 x86_64）

**解决方案：**

**方案一：检查并修复 Docker 仓库配置**

```bash
# 1. 检查是否已添加 Docker 仓库
ls /etc/yum.repos.d/ | grep docker

# 2. 如果没有 docker-ce.repo 文件，重新添加
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo

# 3. 清理缓存并重新生成
sudo yum clean all
sudo yum makecache

# 4. 再次尝试安装
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

**方案二：使用阿里云镜像源（推荐）**

```bash
# 备份原配置
sudo cp /etc/yum.repos.d/docker-ce.repo /etc/yum.repos.d/docker-ce.repo.bak

# 使用阿里云的 Docker 镜像源
sudo tee /etc/yum.repos.d/docker-ce.repo <<-'EOF'
[docker-ce-stable]
name=Docker CE Stable - $basearch
baseurl=https://mirrors.aliyun.com/docker-ce/linux/centos/$releasever/$basearch/stable
enabled=1
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/docker-ce/linux/centos/gpg
EOF

# 清理缓存并安装
sudo yum clean all
sudo yum makecache
sudo yum install -y docker-ce docker-ce-cli containerd.io
```

**方案三：手动下载 RPM 包安装**

```bash
# 1. 访问 https://download.docker.com/linux/centos/7/x86_64/stable/Packages/
#    下载以下三个 RPM 包：
#    - containerd.io-xxx.rpm
#    - docker-ce-cli-xxx.rpm
#    - docker-ce-xxx.rpm

# 2. 使用 wget 下载（以具体版本为例）
cd /tmp
wget https://download.docker.com/linux/centos/7/x86_64/stable/Packages/containerd.io-1.6.21-3.1.el7.x86_64.rpm
wget https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-cli-24.0.6-1.el7.x86_64.rpm
wget https://download.docker.com/linux/centos/7/x86_64/stable/Packages/docker-ce-24.0.6-1.el7.x86_64.rpm

# 3. 安装 RPM 包
sudo rpm -ivh containerd.io-1.6.21-3.1.el7.x86_64.rpm
sudo rpm -ivh docker-ce-cli-24.0.6-1.el7.x86_64.rpm
sudo rpm -ivh docker-ce-24.0.6-1.el7.x86_64.rpm
```

**方案四：检查 CentOS 版本**

```bash
# 查看 CentOS 版本
cat /etc/centos-release

# 如果是 CentOS 8，需要使用替代方案：
# CentOS 8 已于 2021 年底停止维护，Docker 官方不再提供支持
# 可以升级到 CentOS Stream 8，或使用 Podman 替代 Docker
```

**方案五：检查系统架构**

```bash
# 查看系统架构
uname -m

# Docker 官方只支持 x86_64 架构
# 如果是其他架构（如 arm、aarch64），需要使用对应的镜像或编译版本
```

### 14.3 权限问题

如果运行 Docker 命令提示权限不足：

```bash
# 方案一：使用 sudo
sudo docker run hello-world

# 方案二：将用户加入 docker 组
sudo usermod -aG docker $USER
newgrp docker
```

### 14.4 SELinux 问题

如果 SELinux 阻止 Docker 访问某些目录：

```bash
# 临时关闭 SELinux
sudo setenforce 0

# 永久关闭 SELinux（需要重启）
sudo sed -i 's/^SELINUX=.*/SELINUX=disabled/' /etc/selinux/config
```

### 14.5 防火墙问题

如果需要从外部访问容器服务，需要开放端口：

```bash
# 查看防火墙状态
sudo firewall-cmd --state

# 开放端口（例如 8080）
sudo firewall-cmd --zone=public --add-port=8080/tcp --permanent

# 重新加载防火墙配置
sudo firewall-cmd --reload
```

## 十五、卸载 Docker 及其中安装的内容

### 15.1 先搞清楚：Docker 里"安装的内容"是什么？

在 Docker 里"装"的 MySQL、Nginx、Ollama 等，本质是**容器 + 镜像 + 数据**三样东西，卸载要按顺序清理：

```
容器（正在运行的服务）   → 第 1 步：停止并删除
镜像（应用的"安装包"）   → 第 2 步：删除
数据（挂载的数据目录）   → 第 3 步：按需删除（删了数据就真没了）
```

**重要提醒：** 删除容器和镜像**不会**删除挂载出去的数据目录（如 `/data/mysql`），必须最后手动删。这也是前面章节推荐挂载持久化的原因——误删了容器，数据还在。

### 15.2 卸载单个应用（以 MySQL 为例）

```bash
# 第 1 步：停止并删除容器
docker stop mysql
docker rm mysql

# 第 2 步：删除镜像
docker rmi mysql:8.0

# 第 3 步：删除持久化数据（确定不要了再删！）
sudo rm -rf /data/mysql
```

**验证卸载干净：**

```bash
docker ps -a | grep mysql    # 无输出 = 容器已删
docker images | grep mysql   # 无输出 = 镜像已删
```

其他应用同理，替换容器名、镜像名和数据目录即可：

| 应用 | 容器名 | 镜像 | 数据目录 |
|------|--------|------|----------|
| MySQL | mysql | mysql:8.0 | /data/mysql |
| Nginx | nginx | nginx:latest | /data/nginx |
| Ollama | ollama | ollama/ollama:latest | /data/ollama |

**卸载 Django + Vue3 项目（第十七章部署的）：**

```bash
# 1. 停止并删除三个容器和网络
cd /data/myproject
docker compose down

# 2. 删除构建出来的镜像（名字 = 项目目录名-服务名）
docker rmi myproject-backend myproject-frontend

# 3. 删除项目和数据（确定不要了再删）
sudo rm -rf /data/myproject
```

### 15.3 一键清空所有容器和镜像（重置 Docker）

如果不想一个个删，可以整体清空（**不可恢复，谨慎执行**）：

```bash
# 强制删除所有容器（包括正在运行的）
docker rm -f $(docker ps -aq)

# 删除所有镜像
docker rmi -f $(docker images -q)

# 删除所有未使用的数据卷
docker volume prune -f

# 验证：两条命令都应该是空的
docker ps -a
docker images
```

或者使用 Docker 官方清理命令：

```bash
# 清理所有未使用的资源（停止的容器、无用网络、悬空镜像、构建缓存）
docker system prune

# 彻底清理：连"没有被任何容器使用的正常镜像"也删掉，接近重置
docker system prune -a

# 查看 Docker 磁盘占用（清理前后对比）
docker system df
```

**两个命令的区别：**

- `docker system prune`：只删悬空镜像（无标签的废弃层），正常镜像保留
- `docker system prune -a`：所有没被容器使用的镜像都删，等于重新开始

### 15.4 关于数据卷（volume）

除了 `-v /data/xxx:...` 这种挂载目录，Docker 还有自己管理的数据卷（volume），单独清理：

```bash
docker volume ls        # 查看所有数据卷
docker volume rm 卷名   # 删除指定数据卷
docker volume prune -f  # 删除所有未使用的数据卷
```

### 15.5 卸载 Docker 本身

确认 Docker 里清空、且不再需要 Docker 后，再执行本节卸载：

**① 停止 Docker 服务**

```bash
sudo systemctl stop docker
```

**② 卸载 Docker 包**

```bash
sudo yum remove docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

**③ 删除相关数据**

```bash
sudo rm -rf /var/lib/docker
sudo rm -rf /var/lib/containerd
```

**④ 验证卸载完成**

```bash
docker --version
# 输出 -bash: docker: command not found 即卸载成功
```

## 十六、Dockerfile 详解（重点）

### 16.1 什么是 Dockerfile

Dockerfile 是一个纯文本文件（文件名必须叫 `Dockerfile`，无扩展名），里面按顺序写着构建镜像所需的每一步命令。Docker 读取它就能自动构建出镜像。

核心流程：

```
编写 Dockerfile  →  docker build 构建镜像  →  docker run 运行容器
```

为什么要用 Dockerfile：
- **固化环境**：把"装系统、装依赖、配环境"写成代码，一次编写到处运行
- **可复现**：任何人拿到 Dockerfile 都能构建出完全一样的环境
- **可版本化**：能放进 Git，随代码一起迭代

### 16.2 第一个 Dockerfile（完整操作步骤）

以下操作在 **CentOS 终端**中执行：

**第 1 步：创建练习目录**

```bash
mkdir -p /data/dockerfile-demo && cd /data/dockerfile-demo
```

**第 2 步：创建 Dockerfile**

```bash
tee Dockerfile <<-'EOF'
# 基础镜像（Dockerfile 的第一条指令必须是 FROM）
FROM centos:7

# 维护者信息（元数据）
LABEL maintainer="yourname@example.com"

# 构建镜像时执行的命令
RUN echo "hello dockerfile" > /tmp/hello.txt

# 容器启动时默认执行的命令
CMD ["cat", "/tmp/hello.txt"]
EOF
```

**逐行解释（3 个最容易困惑的点）：**

**① `tee` 是什么意思？**

`tee` 是 Linux 命令，作用是"把内容写入文件，同时在屏幕上显示一份"。它的固定搭配：

```bash
tee 文件名 <<-'EOF'
...要写入的内容...
EOF
```

效果 = 把 `EOF` 之间的所有内容**原样写入** `Dockerfile` 文件（文件不存在会自动创建，已存在会覆盖）。相当于"用记事本打开文件 → 粘贴内容 → 保存"，只是全部由命令自动完成。

> 为什么不用 vim 手动编辑？vim 需要会进入/退出编辑器，容易出错；`tee` 一条命令直接生成文件，最适合照着教程复制粘贴。本文档里所有 `tee ... <<-'EOF'` 都是这个用法。

**② `EOF` 是什么意思？**

`EOF`（End Of File）只是一个**结束标记**，不是命令，本身不会被写进文件。`<<-'EOF'` 的意思是"开始接收输入，直到遇到单独一行的 `EOF` 为止"：

```
tee Dockerfile <<-'EOF'   ← 开始接收输入
FROM centos:7             ← 这些行都会被写进文件
CMD ["cat", "/tmp/hello.txt"]
EOF                       ← 输入到此结束（这一行不写入文件）
```

标记词可以换成别的（如 `DONE`），只要开头结尾一致即可，约定俗成用 `EOF`。

**③ `CMD ["cat", "/tmp/hello.txt"]` 是什么意思？**

这是 Dockerfile 指令，意思是"**容器启动时，默认执行 `cat /tmp/hello.txt` 这条命令**"：

- `cat` 是 Linux 命令，作用是"读取并打印文件内容"
- `["cat", "/tmp/hello.txt"]` 是 JSON 数组写法（Exec 格式）：第一个元素是命令，后面元素是参数，合起来等价于执行 `cat /tmp/hello.txt`
- 整句效果：容器一启动就打印 `/tmp/hello.txt` 的内容——而前面 `RUN echo ... > /tmp/hello.txt` 已经在构建时把 `hello dockerfile` 写进了这个文件

所以第 5 步运行容器时看到输出 `hello dockerfile`，就是这条 CMD 的结果。

**第 3 步：构建镜像**（注意最后有一个 `.`，代表"当前目录"是构建上下文）

```bash
docker build -t mycentos:v1 .
```

**第 4 步：查看构建好的镜像**

```bash
docker images | grep mycentos
```

**第 5 步：运行容器验证**

```bash
docker run --rm mycentos:v1
# 输出：hello dockerfile 就说明成功了
```

### 16.3 Dockerfile 常用指令详解

| 指令 | 作用 | 示例 |
|------|------|------|
| `FROM` | 指定基础镜像，必须是第一条指令 | `FROM centos:7` |
| `RUN` | **构建镜像时**执行命令（安装软件等） | `RUN yum install -y python3` |
| `COPY` | 把宿主机文件复制进镜像 | `COPY app.py /app/` |
| `ADD` | 类似 COPY，额外支持自动解压 tar、URL 下载（推荐用 COPY） | `ADD app.tar.gz /app/` |
| `WORKDIR` | 设置工作目录（后续指令的"当前目录"） | `WORKDIR /app` |
| `ENV` | 设置环境变量（构建和运行时都有效） | `ENV TZ=Asia/Shanghai` |
| `ARG` | 构建时传入的变量（仅构建阶段有效） | `ARG VERSION=1.0` |
| `EXPOSE` | 声明容器监听的端口（仅声明，不映射） | `EXPOSE 8080` |
| `VOLUME` | 声明匿名数据卷 | `VOLUME /data` |
| `CMD` | 容器启动时的默认命令（可被 `docker run` 后面的命令覆盖） | `CMD ["python3", "app.py"]` |
| `ENTRYPOINT` | 容器入口命令（不易被覆盖，常与 CMD 配合） | `ENTRYPOINT ["python3"]` |
| `USER` | 指定运行容器的用户 | `USER appuser` |
| `LABEL` | 添加镜像元数据（版本、作者等） | `LABEL version="1.0"` |
| `HEALTHCHECK` | 容器健康检查 | `HEALTHCHECK CMD curl -f http://localhost:8080 \|\| exit 1` |

**两种书写格式的区别：**

```dockerfile
# Shell 格式：通过 /bin/sh -c 执行，可用环境变量、管道等 Shell 特性
RUN echo "hello"

# Exec 格式（推荐）：JSON 数组，直接执行不经过 Shell，参数必须用双引号
CMD ["python3", "app.py"]
```

### 16.4 核心指令深入理解

**FROM —— 选择基础镜像**

```dockerfile
FROM centos:7          # 官方操作系统镜像
FROM python:3.10-slim  # 自带 Python 3.10 的精简镜像（推荐，体积小）
FROM ubuntu:22.04      # Ubuntu 基础镜像
```

选型建议：
- 优先选 `slim`/`alpine` 等精简版，减小镜像体积
- 固定具体版本号，不要用 `latest`，避免基础镜像悄悄更新导致构建结果不可复现

**RUN —— 合并命令减少层数（重要优化）**

Dockerfile 里几乎每条指令都会生成一个镜像层，层越多镜像越大：

```dockerfile
# 不推荐：两条 RUN 产生两层
RUN yum install -y wget
RUN yum install -y curl

# 推荐：合并成一条，并顺手清理缓存
RUN yum install -y wget curl && yum clean all
```

**EXPOSE 与 -p 的关系**

```dockerfile
EXPOSE 8080
```

`EXPOSE` 只是"声明"容器打算用哪个端口，**并不会真正映射端口**。真正让外部访问要靠：

```bash
docker run -p 8080:8080 镜像名
```

**CMD 与 ENTRYPOINT 的区别（高频考点）**

| 对比项 | CMD | ENTRYPOINT |
|--------|-----|------------|
| 作用 | 提供默认命令 | 固定入口命令 |
| `docker run 镜像 追加参数` 时 | 整个 CMD 被覆盖 | 追加参数变成 ENTRYPOINT 的参数 |
| 典型用法 | 提供默认值 | 固定程序 + CMD 提供默认参数 |

```dockerfile
ENTRYPOINT ["python3"]
CMD ["app.py"]
# docker run myimage           → 执行 python3 app.py
# docker run myimage test.py   → 执行 python3 test.py
```

**COPY 与 ADD 的区别**

- `COPY`：单纯复制文件，语义清晰，**优先使用**
- `ADD`：额外支持自动解压 tar 包、从 URL 下载，行为"隐式"，容易踩坑，只在需要解压时使用

### 16.5 构建命令 docker build 详解

```bash
# 基本用法：-t 指定 镜像名:标签，最后的 . 表示当前目录是构建上下文
docker build -t myimage:v1 .

# 指定 Dockerfile 文件名（文件名不叫 Dockerfile 时）
docker build -t myimage:v1 -f Dockerfile.prod .

# 不使用缓存，完全重新构建
docker build --no-cache -t myimage:v1 .

# 给 Dockerfile 里的 ARG 传参
docker build --build-arg VERSION=2.0 -t myimage:v2 .

# 查看镜像分层（每一层对应 Dockerfile 里的一条指令）
docker history myimage:v1
```

**关于最后那个 `.`（构建上下文）：**

`docker build` 会把指定目录（这里是 `.` 即当前目录）下的所有文件发送给 Docker 引擎，Dockerfile 里的 `COPY` 只能复制这个目录里的文件。因此：
- Dockerfile 本身可以放别处（用 `-f` 指定），但 `COPY` 的源文件必须在上下文目录里
- 上下文目录不要放无关的大文件，否则每次构建都上传很慢（用 `.dockerignore` 排除）

### 16.6 .dockerignore 文件

放在 Dockerfile 同目录，作用类似 `.gitignore`，构建时排除不需要发送给引擎的文件：

```bash
tee .dockerignore <<-'EOF'
.git
__pycache__
*.pyc
.env
data/
logs/
*.md
EOF
```

### 16.7 分层缓存机制（为什么写法影响构建速度）

Docker 构建时，如果某条指令没变化、且它依赖的上层没变，就直接用缓存，跳过执行。

```dockerfile
# 推荐写法：先 COPY 依赖清单并安装，再 COPY 其余代码
COPY requirements.txt .
RUN pip install -r requirements.txt
COPY . .
```

效果：只改业务代码时，前两层（装依赖）直接走缓存，只有最后的 `COPY . .` 重新执行，构建秒级完成。如果反过来先 `COPY . .` 再安装依赖，改任何一行代码都会触发重新安装依赖，非常慢。

### 16.8 实战：Python Flask 应用完整示例

**第 1 步：创建项目目录**

```bash
mkdir -p /data/flask-demo && cd /data/flask-demo
```

**第 2 步：创建 app.py**

```bash
tee app.py <<-'EOF'
from flask import Flask
app = Flask(__name__)

@app.route("/")
def index():
    return "Hello, Docker + Flask!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8080)
EOF
```

**第 3 步：创建 requirements.txt**

```bash
echo "flask==3.0.0" > requirements.txt
```

**第 4 步：创建 Dockerfile（Python 项目标准写法）**

```bash
tee Dockerfile <<-'EOF'
FROM python:3.10-slim

ENV PYTHONUNBUFFERED=1 \
    TZ=Asia/Shanghai

WORKDIR /app

# 先复制依赖文件并安装（利用缓存，代码改动时不用重装依赖）
COPY requirements.txt .
RUN pip install --no-cache-dir -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt

# 再复制业务代码
COPY app.py .

EXPOSE 8080

CMD ["python", "app.py"]
EOF
```

**第 5 步：构建镜像**

```bash
docker build -t flask-demo:v1 .
```

**第 6 步：运行容器**

```bash
docker run -d --name flask-demo -p 8080:8080 --restart=always flask-demo:v1
```

**第 7 步：验证**

```bash
curl http://localhost:8080
# 输出：Hello, Docker + Flask! 就说明成功
```

### 16.9 多阶段构建（进阶：减小镜像体积）

适用于"编译环境和运行环境分离"的场景（如 Go、Java、前端打包）：

```dockerfile
# 第一阶段：构建阶段（包含编译工具，体积大）
FROM golang:1.21 AS builder
WORKDIR /build
COPY . .
RUN go build -o myapp .

# 第二阶段：运行阶段（只拷贝编译结果，体积小）
FROM alpine:3.19
WORKDIR /app
COPY --from=builder /build/myapp .
CMD ["./myapp"]
```

最终镜像只包含第二阶段内容，不带编译器，体积可缩小数倍。

### 16.10 Dockerfile 常用操作命令汇总

```bash
# 构建镜像
docker build -t 镜像名:标签 .

# 指定 Dockerfile 构建
docker build -t 镜像名:标签 -f Dockerfile.prod .

# 不使用缓存构建
docker build --no-cache -t 镜像名:标签 .

# 查看镜像分层历史
docker history 镜像名:标签

# 查看镜像详情
docker inspect 镜像名:标签

# 删除镜像
docker rmi 镜像名:标签

# 给镜像重新打标签
docker tag 镜像名:标签 新镜像名:新标签

# 导出/导入镜像（离线传输到其他机器）
docker save -o myimage.tar 镜像名:标签
docker load -i myimage.tar
```

### 16.11 常见问题

**问题 1：COPY 找不到文件**

```
COPY failed: file not found in build context
```

原因：`COPY` 的文件不在构建上下文目录（`docker build` 最后那个 `.` 指定的目录）里。

**问题 2：构建很慢**

- 检查是否先 COPY 全部代码再安装依赖，应改为先装依赖再 COPY 代码（见 16.7）
- 检查构建目录是否混入无关大文件，配置 `.dockerignore`（见 16.6）

**问题 3：容器内 pip/yum 安装慢或失败**

- pip 用国内源：`-i https://pypi.tuna.tsinghua.edu.cn/simple`
- yum 用阿里云源并清理缓存：`yum clean all`

**问题 4：容器启动就退出**

- 用 `docker logs 容器名` 查看报错，通常是 CMD 命令写错
- 容器内主程序必须**前台运行**，不能在命令里加 `&` 放到后台，否则主进程结束容器就退出

## 十七、项目部署实战：Djangoserver2 + DjangoQt 前后端分离（重点）

> 本章针对本机实际项目改写：**后端 = `D:\work\AI2617\djangoserver2`（Django 5 + DRF + Celery + Redis）**，**前端 = `D:\work\AI2617\djangoqt`（Vue3 + Vite + Element Plus + pnpm）**。CentOS 已按前 16 章装好 Docker，照下面 9 步顺序做完即可在 `http://服务器IP` 访问整套系统。

### 17.1 部署架构说明

本项目比一般教程多了 **Redis 和 Celery Worker**——因为后端用 Celery 异步导出 Excel（`apps.system.tasks` 等），所以容器编排共 **5 个容器**：

1. **frontend 容器（Nginx:80）**：托管 Vue3 打包产物 + 反向代理 `/api/`、`/images/`、`/exports/` 到后端
2. **web 容器（Django + Gunicorn:8001）**：提供 REST API，自动执行 `migrate` + `seed` 造数
3. **worker 容器（Celery）**：跑异步任务（导出 Excel 等），复用后端镜像
4. **mysql 容器（MySQL 8.0:3306）**：业务数据库 `web2315_ent`
5. **redis 容器（Redis 7:6379）**：缓存 + Celery broker + result backend

架构图：

```
浏览器访问 http://服务器IP
        │
        ▼
┌────────────────────────────────────────┐
│  frontend 容器 (Nginx:80) ← 唯一对外    │
│  /            → 返回 Vue3 静态文件        │
│  /api/        → 转发给 web 容器 (8001)   │
│  /images/     → 转发给 web 容器 (上传图片)│
│  /exports/    → 转发给 web 容器 (Excel)   │
└───────────┬────────────────────────────┘
            ▼
┌────────────────────────┐    ┌────────────────┐    ┌────────────────┐
│  web 容器              │    │ worker 容器     │    │  mysql 容器     │
│  Django + Gunicorn     │    │ Celery          │ →  │  web2315_ent    │
│  端口 8001（仅内部）    │    │ 复用后端镜像     │    │  3306（仅内部） │
└───────┬────────────────┘    └─────┬──────────┘    └────────────────┘
        │                           │
        └───────────┬───────────────┘
                    ▼
            ┌────────────────┐
            │ redis 容器      │
            │ 缓存 + Celery    │
            │ 6379（仅内部）  │
            └────────────────┘
```

| 容器 | 镜像来源 | 角色 | 对外端口 |
|------|---------|------|---------|
| frontend | 自建（nginx:stable-alpine + dist） | 托管 Vue3 + 反向代理 | **80（唯一对外）** |
| web | 自建（python:3.12-slim + 项目代码） | REST API、登录、JWT | 无 |
| worker | 复用 web 镜像 | Celery 异步任务（Excel 导出） | 无 |
| mysql | mysql:8.0 | 业务数据 | 无 |
| redis | redis:7-alpine | 缓存 + Celery 队列 | 无 |

**关键设计：只有 Nginx 对外开 80 端口**，Django(8001)、MySQL(3306)、Redis(6379) 全部只在容器内网互通，外部访问不到，更安全。

**跨域问题为什么消失了：** 前端 `vite.config.ts` 里 `baseApi = '/api'`，页面走 `/`、接口走 `/api/`，都通过同一个 `http://服务器IP` 访问，浏览器视为同源，没有跨域。

**部署总流程（共 8 步，照着顺序一步步做即可）：**

> **整体思路：前 5 步全部在本机 Windows 上用 VS Code 把文件改好，第 6 步才一次性打包上传到 CentOS。** 不要提前上传——因为前端 Dockerfile/nginx.conf、根目录 .env、docker-compose.yml 都还没建好。

| 步骤 | 做什么 | 在哪里操作 |
|------|--------|-----------|
| 第 1 步 | 安装 Docker Compose 插件 | CentOS 终端 |
| 第 2 步 | 检查后端 `djangoserver2/`（自带 Dockerfile 等，**基本不用改**，看说明即可） | 本机 |
| 第 3 步 | 在 `djangoqt/` 下新增 3 个文件：Dockerfile + nginx.conf + .dockerignore | 本机改好 |
| 第 4 步 | 在项目根目录 `D:\work\AI2617\` 创建 `.env`（密码、JWT 密钥集中配置） | 本机改好 |
| 第 5 步 | 在项目根目录创建 `docker-compose.yml`（5 容器合并编排） | 本机改好 |
| 第 6 步 | **打包整体上传**到 CentOS + 一键构建镜像并启动全部容器 | Windows 打包 → CentOS 启动 |
| 第 7 步 | 创建 Django 超级管理员账号 | CentOS 终端 |
| 第 8 步 | 浏览器验证部署结果 | Windows 浏览器 |

> **建议：前 5 步全部在本机用 VS Code 改好，第 6 步用 `tar` 打包（自动排除 `node_modules`/`dist` 等不该上传的目录）→ `scp` 上传 → 服务器解压 → `docker compose up -d --build` 一气呵成。比在服务器上用 vim/tee 效率高得多，改错也好恢复。**

### 17.2 第 1 步：安装 Docker Compose

编排 5 个容器需要 Docker Compose（新版是 `docker compose` 插件）：

```bash
# 安装 compose 插件（阿里云源里就有这个包）
sudo yum install -y docker-compose-plugin

# 验证（注意：v2 版本是 "docker compose"，中间是空格）
docker compose version
```

输出 `Docker Compose version v2.x.x` 即成功。本章统一用 `docker compose`（旧版 `docker-compose` 用法相同，把空格换成 `-` 即可）。

### 17.3 目录结构总览（先看一眼最终长什么样）

**本节不是操作步骤**，只是让你在动手前心里有张图：本机要准备哪些文件、最终在服务器上长什么样。**上传动作统一在第 6 步做，现在不要上传。**

**最终目标：服务器 `/data/ai2617/` 下的结构**（`/data` 目录的说明见下方注释）：

```
/data/ai2617/                      ← 项目根目录（第 6 步在这里执行 docker compose）
├── docker-compose.yml             ← 第 5 步本机创建，第 6 步打包上传
├── .env                           ← 第 4 步本机创建，第 6 步打包上传
├── djangoserver2/                 ← 后端项目（整体打包上传）
│   ├── Dockerfile                 ← 项目自带，无需修改（第 2 步讲解）
│   ├── entrypoint.sh              ← 项目自带，无需修改
│   ├── .dockerignore              ← 项目自带，无需修改
│   ├── requirements.txt           ← 项目自带
│   ├── manage.py
│   ├── config/                    ← Django 配置包（settings 分 base/dev/prod 三层）
│   │   ├── __init__.py            ← 已写好 pymysql.install_as_MySQLdb()
│   │   ├── settings/
│   │   │   ├── base.py            ← 数据库从 DB_HOST 等环境变量读
│   │   │   ├── dev.py
│   │   │   └── prod.py            ← 部署用：DEBUG=False + ALLOWED_HOSTS 收紧
│   │   ├── wsgi.py
│   │   ├── celery.py
│   │   └── urls.py
│   └── apps/                      ← 业务 app（system/approval/echarts_app/upload_app/logins）
│
└── djangoqt/                      ← 前端项目（整体打包上传）
    ├── Dockerfile                 ← 第 3 步本机新增
    ├── nginx.conf                 ← 第 3 步本机新增
    ├── .dockerignore              ← 第 3 步本机新增
    ├── package.json               ← 项目自带（pnpm 包管理）
    ├── vite.config.ts             ← 项目自带（baseApi='/api'）
    ├── .env.dev / .env.pro        ← 项目自带
    └── src/                       ← Vue3 源码
```

> **`/data` 是什么？在 CentOS 里看不到正常吗？**
>
> - `/data` **不是系统自带的目录**，是我们自己创建的普通文件夹，位置就在 CentOS 硬盘的根目录下。看不到是因为还没创建过——第 6 步解压前会执行 `mkdir -p /data/ai2617`，执行后 `ls /` 就能看到它了
> - 它**不属于 Nginx，也不属于 Docker**，就是 CentOS 里最普通的目录。MySQL 容器通过 named volume 把数据存在 Docker 管理区，日志通过 `-v ./djangoserver2/logs:/app/logs` 挂到项目目录里
> - 自查命令：`ls /` 查看根目录下所有文件夹（bin、etc、home、data……）；`ls /data` 查看 data 里有什么

> **重要：打包上传时自动排除大目录**：本机 `djangoqt/` 下的 `node_modules`（几百 MB）和 `dist`（旧打包产物）**不上传**，容器构建时会自己装依赖和打包；`djangoserver2/.env`（本地开发配置）也不上传，避免和部署用的根目录 `.env` 冲突。第 6 步的 `tar` 命令已带 `--exclude` 参数自动排除。

### 17.4 第 2 步：准备 Django 后端（项目自带，基本无需改动）

**好消息：** 你本机的 `djangoserver2/` 项目里已经写好了完整的部署文件——`Dockerfile`、`entrypoint.sh`、`.dockerignore`、`requirements.txt` 都齐了，**这一步基本不用动**。下面只是逐个文件说明"它在做什么、为什么这么写"，看完心里有底即可。

**先看总览：这 5 个文件/目录分别在做什么**

| 文件/目录 | 类比 | 作用 | 谁来读它 |
|----------|------|------|---------|
| `Dockerfile` | 菜谱/装配图 | 告诉 Docker 怎么把这个项目"打包"成镜像：从哪个基础镜像开始、装什么系统包、装哪些 Python 依赖、拷哪些代码、启动时跑什么命令 | Docker 引擎在 `docker compose build` 时读 |
| `entrypoint.sh` | 开机自检脚本 | 容器启动时**第一时间执行**的脚本：等 MySQL/Redis 起来 → 跑 migrate 建表 → 跑 seed 造数 → 把控制权交给 CMD（gunicorn/celery） | 容器启动时由 `ENTRYPOINT ["/app/entrypoint.sh"]` 调用 |
| `.dockerignore` | 打包黑名单 | 告诉 Docker 哪些文件**不要**打进镜像：`node_modules`、`.env`、`__pycache__`、`logs` 等，减小镜像体积、防止密码泄露 | Docker 在 `COPY . /app/` 时按这个清单过滤 |
| `requirements.txt` | 采购清单 | 列出项目依赖的所有 Python 包及版本：Django、DRF、PyMySQL、Celery、Redis、gunicorn…… | Dockerfile 里的 `RUN pip install -r requirements.txt` 读取 |
| `config/settings/` | 多套环境配置 | Django 配置分三层：`base.py`（公共）→ `dev.py`（开发覆盖）→ `prod.py`（生产覆盖）。部署时用 prod，关 DEBUG、收紧跨域 | Django 启动时按 `DJANGO_SETTINGS_MODULE` 环境变量选一套 |

**它们之间怎么协作（容器启动流程）：**

```
docker compose up
    ↓
1. Docker 读 Dockerfile → 构建 djangoserver2 镜像
   ├─ 从 python:3.12-slim 基础镜像开始
   ├─ 按 .dockerignore 过滤，把代码 COPY 进镜像
   ├─ 按 requirements.txt 装 Python 依赖
   └─ 设定 ENTRYPOINT=entrypoint.sh，CMD=gunicorn...
    ↓
2. 启动容器 → 先跑 entrypoint.sh
   ├─ nc -z 探测 MySQL/Redis 端口（等它们就绪）
   ├─ python manage.py migrate      （按 config/settings/prod.py 建表）
   ├─ python manage.py seed         （造初始数据）
   └─ exec "$@"  →  把控制权交给 CMD（gunicorn 启动 Web 服务）
    ↓
3. gunicorn 监听 8001，开始对外提供 API
```

> **一句话记忆：** `Dockerfile` 管"怎么打包"，`entrypoint.sh` 管"启动前先干啥"，`.dockerignore` 管"别带啥进镜像"，`requirements.txt` 管"装哪些 Python 包"，`config/settings/` 管"Django 用哪套配置"。

#### ① `djangoserver2/Dockerfile`（项目自带，无需改）

```dockerfile
FROM python:3.12-slim

ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1 \
    DJANGO_SETTINGS_MODULE=config.settings.prod

WORKDIR /app

# netcat 用于 entrypoint 等待 MySQL/Redis 就绪
RUN apt-get update \
    && apt-get install -y --no-install-recommends netcat-traditional \
    && rm -rf /var/lib/apt/lists/*

# 先装依赖（利用 Docker 层缓存：requirements 不变就不重装）
COPY requirements.txt /app/
RUN pip install --no-cache-dir -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

# 拷贝项目代码
COPY . /app/
RUN chmod +x /app/entrypoint.sh

EXPOSE 8001

ENTRYPOINT ["/app/entrypoint.sh"]
# 默认启动 Web 服务；worker 服务在 docker-compose 里用 command 覆盖
CMD ["gunicorn", "config.wsgi:application", "--bind", "0.0.0.0:8001", "--workers", "3", "--timeout", "60"]
```

**逐行解释关键点：**

| 行 | 作用 | 为什么这么写 |
|----|------|-------------|
| `DJANGO_SETTINGS_MODULE=config.settings.prod` | 指定用 prod 配置 | 部署用 prod（DEBUG=False），开发用 dev（DEBUG=True） |
| `apt-get install netcat-traditional` | 装 nc 命令 | entrypoint.sh 用 `nc -z` 探测 MySQL/Redis 是否就绪 |
| `COPY requirements.txt` 单独一层 | 先装依赖再拷代码 | 改代码时不会重装依赖，构建快 |
| `-i https://pypi.tuna.tsinghua.edu.cn/simple` | 用清华 pip 源 | 容器内默认走官方源会很慢/超时 |
| `ENTRYPOINT` + `CMD` 分离 | 入口脚本统一处理 | entrypoint.sh 等待依赖就绪 + migrate + seed，再交给 CMD 启动 |
| `EXPOSE 8001` | 声明端口 | gunicorn 监听 8001，与 settings 里的路由一致 |

#### ② `djangoserver2/entrypoint.sh`（项目自带，无需改）

```bash
#!/usr/bin/env bash
set -e

DB_HOST="${DB_HOST:-db}"
DB_PORT="${DB_PORT:-3306}"

echo ">> 等待 MySQL ${DB_HOST}:${DB_PORT} 就绪 ..."
until nc -z "${DB_HOST}" "${DB_PORT}"; do
  echo "   MySQL 尚未就绪，2 秒后重试 ..."
  sleep 2
done
echo ">> MySQL 已就绪。"

# 用了 Redis 时等它
if [ "${USE_REDIS}" = "True" ]; then
  REDIS_HOST="${REDIS_HOST:-redis}"
  REDIS_PORT="${REDIS_PORT:-6379}"
  echo ">> 等待 Redis ${REDIS_HOST}:${REDIS_PORT} 就绪 ..."
  until nc -z "${REDIS_HOST}" "${REDIS_PORT}"; do
    echo "   Redis 尚未就绪，2 秒后重试 ..."
    sleep 2
  done
  echo ">> Redis 已就绪。"
fi

# 只有 web 容器需要迁移/造数；worker 容器用 SKIP_MIGRATE=1 跳过
if [ "${SKIP_MIGRATE}" != "1" ]; then
  echo ">> 执行数据库迁移 migrate ..."
  python manage.py migrate --noinput
  echo ">> 执行 seed 造数（幂等）..."
  python manage.py seed || echo "   seed 跳过/已存在"
fi

echo ">> 启动服务：$*"
exec "$@"
```

**关键设计：**

- **`nc -z` 探活**：MySQL/Redis 容器"启动"≠"能连"，entrypoint 用 `nc -z` 真正探测端口能连通才往下走，避免一启动就 migrate 报错
- **`SKIP_MIGRATE` 开关**：web 容器跑迁移 + 造数，worker 容器设 `SKIP_MIGRATE=1` 跳过（防止两个容器同时写库冲突）
- **`exec "$@"`**：把控制权交给 CMD（gunicorn / celery），让主进程 PID=1，能正确接收 `docker stop` 信号

#### ③ `djangoserver2/.dockerignore`（项目自带，无需改）

```text
__pycache__/
*.py[cod]
*.sqlite3
.git/
.gitignore
.trae/
.env               # 防止本地 .env（密码）被打进镜像
logs/
media/
*.log
.venv/
venv/
tests/
Dockerfile
docker-compose.yml
.dockerignore
学习手册.md
```

**为什么要排除 `.env`？** 因为镜像可能被推到仓库分享，`.env` 含密码绝对不能进镜像；容器启动时用 `env_file: .env` 注入即可。

#### ④ `djangoserver2/requirements.txt`（项目自带，无需改）

```text
Django>=5.0,<5.3
djangorestframework>=3.15,<3.17
djangorestframework-simplejwt>=5.3,<5.5
django-cors-headers>=4.3,<5.0
drf-spectacular>=0.27,<0.29
PyMySQL>=1.1,<2.0
django-redis>=5.4,<6.0
redis>=5.0,<6.0
celery>=5.4,<6.0
openpyxl>=3.1,<4.0
python-dotenv>=1.0,<2.0
gunicorn>=22.0,<24.0
```

> 说明：用 PyMySQL（纯 Python）代替 mysqlclient（要 C 编译环境），容器内 `apt-get install` 不用装 `default-libmysqlclient-dev`，构建又快又稳。

#### ⑤ `config/settings/prod.py`（项目自带，无需改）

```python
from .base import *  # 继承 base 公共配置

DEBUG = False                                       # 生产必须关 DEBUG
ALLOWED_HOSTS = env_list('ALLOWED_HOSTS', '')       # 从 .env 读允许的 IP/域名
CORS_ALLOW_ALL_ORIGINS = False                      # 跨域收紧
CORS_ALLOWED_ORIGINS = env_list('CORS_ALLOWED_ORIGINS', '')

SECURE_CONTENT_TYPE_NOSNIFF = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
```

> **关键提醒：** `.env` 里的 `ALLOWED_HOSTS` 必须填你服务器的 IP（如 `192.168.1.100`），不能写 `*`——否则 Django 会拒绝请求。第 4 步创建 `.env` 时要注意这点。

#### ⑥ 小结：本步无需改任何文件

```
✅ Dockerfile         项目自带，直接复用
✅ entrypoint.sh      项目自带，自动 migrate + seed
✅ .dockerignore      项目自带，已排除 .env
✅ requirements.txt  项目自带，含 PyMySQL + Celery + Redis
✅ config/settings/  三层结构，prod 已配好

→ 后端无需改动，进入第 3 步给前端加文件（第 6 步才统一打包上传）
```

### 17.5 第 3 步：准备 Vue3 前端（新增 3 个文件）

本机 `djangoqt/` 项目里**没有** Docker 部署相关文件（`node_modules`、`dist` 也都不该进镜像），需要在本机 `djangoqt/` 目录下**新增 3 个文件**：`Dockerfile`、`nginx.conf`、`.dockerignore`。这些文件在第 6 步打包时一起上传，现在先在本机建好。

#### ① 在 `D:\work\AI2617\djangoqt\` 下新建 `Dockerfile`

```dockerfile
# ===== 第一阶段：Node + pnpm 构建 Vue3 项目 =====
FROM node:20-alpine AS builder

# 装 pnpm（项目用 pnpm，看 pnpm-lock.yaml 就知道）
RUN npm install -g pnpm

WORKDIR /app

# 先复制依赖清单，利用 Docker 层缓存
COPY package.json pnpm-lock.yaml pnpm-workspace.yaml ./

# 用国内镜像源装依赖，速度快
RUN pnpm config set registry https://registry.npmmirror.com \
    && pnpm install --frozen-lockfile

# 拷贝源码并打包（输出到 /app/dist）
COPY . .
RUN pnpm run build

# ===== 第二阶段：Nginx 运行环境（体积小） =====
FROM nginx:stable-alpine

# 把第一阶段打包好的 dist 复制到 Nginx 默认静态目录
COPY --from=builder /app/dist /usr/share/nginx/html

# 用我们写的 nginx.conf 替换默认配置
COPY nginx.conf /etc/nginx/conf.d/default.conf

EXPOSE 80
```

> **为什么用多阶段构建？**
> - 第一阶段（builder）：用 Node + pnpm 把 Vue3 源码打包成 `dist/`（一堆 html/css/js）
> - 第二阶段（runtime）：只保留 `dist/` 和 Nginx，**丢弃整个 Node 环境**
> - 最终镜像只有几十 MB（vs Node + 源码 + node_modules 动辄几百 MB）
>
> **为什么用 `--frozen-lockfile`？** 严格按 `pnpm-lock.yaml` 锁定的版本安装，不会因为版本漂移导致构建结果不一致。

#### ② 在 `D:\work\AI2617\djangoqt\` 下新建 `nginx.conf`

```nginx
server {
    listen 80;
    server_name localhost;

    # Vue3 打包后的静态文件目录（Dockerfile 里已放入）
    root /usr/share/nginx/html;
    index index.html;

    # 前端路由（history 模式必须配置，否则刷新页面 404）
    location / {
        try_files $uri $uri/ /index.html;
    }

    # /api/ 开头的请求转发给 Django 容器（web 是 compose 里的服务名）
    location /api/ {
        proxy_pass http://web:8001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # 上传图片访问（Django MEDIA_URL='/images/'）
    location /images/ {
        proxy_pass http://web:8001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }

    # Celery 导出 Excel 下载
    location /exports/ {
        proxy_pass http://web:8001;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

**逐段解释：**

| location | 作用 | 为什么这么写 |
|----------|------|-------------|
| `/` | 返回 Vue3 静态文件 | `try_files $uri $uri/ /index.html` 让前端路由（如 `/login`、`/user/list`）刷新时不报 404，统一回退到 index.html 由 Vue Router 处理 |
| `/api/` | 转发给后端 | 浏览器请求 `http://IP/api/auth/login` → Nginx 转给 `http://web:8001/api/auth/login`（`web` 是 compose 里的服务名，容器间用服务名互访） |
| `/images/` | 上传图片访问 | Django 用 `MEDIA_URL='/images/'` 暴露上传文件，前端 `<img src="/images/xxx.jpg">` 走这里 |
| `/exports/` | Excel 下载 | Celery 异步生成的 Excel 文件下载链接 |

> **注意 `proxy_pass http://web:8001` 里的 `web`** ——这是 17.7 节 `docker-compose.yml` 里后端服务的名字，**必须和 compose 里的服务名一致**，否则 Nginx 解析不到。

#### ③ 在 `D:\work\AI2617\djangoqt\` 下新建 `.dockerignore`

```text
node_modules/
dist/
.git/
.gitignore
.vscode/
*.log
stats.html
Dockerfile
nginx.conf
.dockerignore
```

> **为什么排除 `node_modules` 和 `dist`？** `node_modules` 几百 MB，进镜像后会被 builder 阶段重新 `pnpm install` 覆盖；`dist` 是旧的打包产物，构建时会重新生成。两者都不要进镜像，能大幅减小构建上下文体积（上传到 Docker daemon 的速度）。

#### ④ 验证前端 `vite.config.ts`（**无需改**）

你项目里 `vite.config.ts` 已配好 `baseApi = '/api'`（在 `src/config/index.ts` 里），这正是 Nginx 反向代理需要的：

```typescript
// src/config/index.ts（项目自带，无需改）
const ENVconfig: interConfig = {
    dev:        { baseApi: '/api', mockApi: '/examples' },
    production: { baseApi: '/api' }    // ← 生产环境用相对路径 /api
}
```

```typescript
// vite.config.ts（项目自带，无需改）
server: {
    proxy: {
        '/api':     { target: 'http://localhost:8000', changeOrigin: true },  // 开发期代理
        '/images':  { target: 'http://localhost:8000', changeOrigin: true },
        '/exports':  { target: 'http://localhost:8000', changeOrigin: true },
    }
}
```

> **开发期**（`pnpm dev`）：Vite dev server 把 `/api` 代理到 `localhost:8000`（本机 Django runserver）
> **生产期**（容器部署）：Nginx 把 `/api` 代理到 `web:8001`（容器内 gunicorn）
> 两套环境都用相对路径 `/api`，前端代码完全不用改。

#### ⑤ 小结：本步在本机新增 3 个文件后整体上传

```
✅ djangoqt/Dockerfile       多阶段构建（pnpm 打包 → nginx 运行）
✅ djangoqt/nginx.conf       反向代理 /api、/images、/exports 到 web 容器
✅ djangoqt/.dockerignore    排除 node_modules/dist

→ 3 个文件建好后，进入第 4 步创建 .env（第 6 步统一打包上传，现在不传）
```

### 17.6 第 4 步：创建 .env 配置文件（本机创建，第 6 步随项目上传）

**在 `D:\work\AI2617\` 下新建 `.env` 文件 —— 所有密码、密钥集中管理，会注入到容器**

#### ① 先生成两个随机密钥（在本机 PowerShell 执行）

```powershell
# 生成 Django SECRET_KEY（复制输出结果）
python -c "from django.core.management.utils import get_random_secret_key; print(get_random_secret_key())"

# 生成 JWT 密钥（复制输出结果）
python -c "import secrets; print(secrets.token_urlsafe(32))"
```

> 如果本机没装 Django，可用这条替代生成 SECRET_KEY：
> ```powershell
> python -c "import secrets; print('django-insecure-' + secrets.token_urlsafe(50))"
> ```

#### ② 在 `D:\work\AI2617\` 下新建文件 `.env`，内容如下

```bash
# ============================================================
# 部署 .env —— 含密码、密钥，不要提交到 Git！
# ============================================================

# --- Django ---
# 把上面生成的 SECRET_KEY 粘贴到这里（不要照抄示例值）
DJANGO_SECRET_KEY=django-insecure-粘贴你生成的随机串
DJANGO_SETTINGS_MODULE=config.settings.prod
# ⚠️ 必须改成你服务器的实际 IP（多个用逗号分隔），不能用 *
# 例：ALLOWED_HOSTS=192.168.1.100
ALLOWED_HOSTS=服务器IP

# --- MySQL 数据库 ---
DB_NAME=web2315_ent
DB_USER=root
DB_PASSWORD=123456
DB_HOST=db                 # ← compose 里 mysql 服务名，不要改
DB_PORT=3306

# --- JWT 密钥（签发 token 用，粘贴上面生成的 JWT_SECRET） ---
JWT_SECRET=粘贴你生成的JWT随机串
JWT_ACCESS_HOURS=10
JWT_REFRESH_HOURS=120

# --- Redis 缓存 / Celery ---
USE_REDIS=True
REDIS_HOST=redis            # ← compose 里 redis 服务名，不要改
REDIS_PORT=6379
REDIS_URL=redis://redis:6379/1
CELERY_BROKER_URL=redis://redis:6379/2
CELERY_RESULT_BACKEND=redis://redis:6379/3

# --- 跨域（生产环境显式放行前端域名，逗号分隔；同源可省略） ---
# 如果前端和后端都走 http://服务器IP（同源），可以留空
CORS_ALLOWED_ORIGINS=http://服务器IP

# --- worker 容器跳过 migrate（防止和 web 容器冲突） ---
SKIP_MIGRATE=0             # web 容器用 0；worker 在 compose 里覆盖为 1
```

#### ③ 必须改的 3 处（⚠️ 改错 Django 会拒绝访问）

| 配置项 | 改成什么 | 怎么查 |
|--------|---------|------|
| `DJANGO_SECRET_KEY` | 第 ① 步生成的随机串 | 复制 PowerShell 输出 |
| `JWT_SECRET` | 第 ① 步生成的 JWT 密钥 | 复制 PowerShell 输出 |
| `ALLOWED_HOSTS` | CentOS 服务器的实际 IP | 在 CentOS 终端执行 `ip addr` 看 `inet` 那行 |

#### ④ `.env` 放在哪里？要替换后端的 `.env` 吗？

```
D:\work\AI2617\
├── .env                  ← ★ 这一份（部署用，DB_HOST=db）
├── docker-compose.yml
├── djangoserver2\
│   └── .env              ← ✘ 这是本地开发用的（DB_HOST=127.0.0.1）
└── djangoqt\
```

- **本机根目录的 `.env`**（刚创建的）= 部署用，第 6 步会随 tar 包上传到服务器，compose 用 `env_file: .env` 注入容器
- **`djangoserver2/.env`** = 本地开发用（连本机 MySQL），**不要上传**（已在 `djangoserver2/.dockerignore` 里排除）

> **关键提醒：**
> - **`ALLOWED_HOSTS` 必须改成服务器实际 IP**，写 `*` 在 prod 模式下会被 Django 拒绝
> - **`DB_HOST=db`** 和 **`REDIS_HOST=redis`** 不要改——这是 compose 里的服务名，容器之间用服务名互访（不是 IP）
> - `.env` 含密码，**不要提交到 Git 仓库**（在根目录 `.gitignore` 里加 `.env`）

### 17.7 第 5 步：创建 docker-compose.yml（本机创建，第 6 步随项目上传）

**在 `D:\work\AI2617\` 下新建 `docker-compose.yml` 文件 —— 5 容器合并编排（MySQL + Redis + Web + Worker + Frontend）**

直接在 VS Code 里新建文件，把下面内容粘进去保存即可（注意 YAML 用空格缩进，不能用 Tab）：

```yaml
# ============================================================
# AI2617 全栈部署：MySQL + Redis + Django Web + Celery Worker + Vue3 前端
# 启动：  docker compose up -d --build
# 访问：  http://服务器IP/       接口文档：http://服务器IP/api/schema/swagger/
# ============================================================
services:
  # ---------- 数据库 ----------
  db:
    image: mysql:8.0
    container_name: ai2617-mysql
    restart: unless-stopped
    environment:
      MYSQL_ROOT_PASSWORD: "123456"
      MYSQL_DATABASE: web2315_ent
      TZ: Asia/Shanghai
    command:
      - --character-set-server=utf8mb4
      - --collation-server=utf8mb4_general_ci
    volumes:
      - mysql_data:/var/lib/mysql     # 数据持久化到 named volume
    networks:
      - webnet
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost", "-p123456"]
      interval: 5s
      timeout: 5s
      retries: 20

  # ---------- 缓存 + Celery 队列 ----------
  redis:
    image: redis:7-alpine
    container_name: ai2617-redis
    restart: unless-stopped
    networks:
      - webnet
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 5s
      timeout: 3s
      retries: 20

  # ---------- Django Web（gunicorn）----------
  web:
    build: ./djangoserver2
    container_name: ai2617-web
    restart: unless-stopped
    env_file: .env
    environment:
      # 覆盖 .env 里的部分值（确保容器内用 compose 服务名寻址）
      DB_HOST: db
      DB_PORT: "3306"
      DB_NAME: web2315_ent
      DB_USER: root
      DB_PASSWORD: "123456"
      REDIS_HOST: redis
      REDIS_PORT: "6379"
      REDIS_URL: redis://redis:6379/1
      CELERY_BROKER_URL: redis://redis:6379/2
      CELERY_RESULT_BACKEND: redis://redis:6379/3
      USE_REDIS: "True"
      ALLOWED_HOSTS: "*"
      SKIP_MIGRATE: "0"
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    volumes:
      - media_data:/app/media         # 上传图片持久化
      - ./djangoserver2/logs:/app/logs # 日志挂出来方便看
    networks:
      - webnet
    # 不写 ports：web 只在容器内网，外部通过 frontend 的 Nginx 反代访问

  # ---------- Celery Worker（复用 web 镜像）----------
  worker:
    build: ./djangoserver2
    container_name: ai2617-worker
    restart: unless-stopped
    env_file: .env
    environment:
      DB_HOST: db
      DB_PORT: "3306"
      DB_NAME: web2315_ent
      DB_USER: root
      DB_PASSWORD: "123456"
      REDIS_HOST: redis
      REDIS_PORT: "6379"
      REDIS_URL: redis://redis:6379/1
      CELERY_BROKER_URL: redis://redis:6379/2
      CELERY_RESULT_BACKEND: redis://redis:6379/3
      USE_REDIS: "True"
      SKIP_MIGRATE: "1"               # worker 不重复迁移
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    command: ["celery", "-A", "config", "worker", "-l", "info"]
    volumes:
      - media_data:/app/media
    networks:
      - webnet

  # ---------- Vue3 前端（Nginx 托管 + 反向代理）----------
  frontend:
    build: ./djangoqt
    container_name: ai2617-frontend
    restart: unless-stopped
    ports:
      - "80:80"                        # 唯一对外端口
    depends_on:
      - web
    networks:
      - webnet

networks:
  webnet:
    driver: bridge

volumes:
  mysql_data:
  media_data:
```

> **注意：**
> - **`web` 服务不写 `ports`** —— 外部不直接访问 Django，所有请求走 frontend 容器的 Nginx 反代到 `web:8001`
> - **`worker` 用 `command:` 覆盖** —— 复用 web 镜像，但启动时跑 celery 而不是 gunicorn
> - **`ALLOWED_HOSTS=*` 在 environment 里覆盖了 .env** —— 因为容器内实际通过同源（Nginx 反代）访问，Host 头是 localhost，写死具体 IP 反而访问不了；如要严格安全，可改 Nginx 的 `proxy_set_header Host $host` 并在 .env 里写具体域名
> - **端口冲突提示**：如果第十一章练习用的 nginx 容器还在运行（占用 80 端口），先执行 `docker stop nginx && docker rm nginx`，或把这里的 `"80:80"` 改成 `"8080:80"`

### 17.8 第 6 步：打包上传 + 一键构建并启动容器

本步分两段：**先在本机打包上传，再在 CentOS 上构建启动**。

#### ① 本机打包（在 Windows PowerShell 执行）

```powershell
# 进入项目根目录（djangoserver2 和 djangoqt 的上一级）
cd D:\work\AI2617

# 确认本机已准备好这 4 个文件（前 5 步的成果）
ls
# 预期看到：
# djangoserver2/   ← 后端（含自带 Dockerfile/entrypoint.sh/.dockerignore/requirements.txt）
# djangoqt/        ← 前端（已加 Dockerfile/nginx.conf/.dockerignore）
# .env             ← 第 4 步创建（已填 SECRET_KEY/JWT_SECRET/ALLOWED_HOSTS）
# docker-compose.yml  ← 第 5 步创建

# 打 tar 包（自动排除不该上传的大目录）
tar -czf ai2617-deploy.tar.gz `
  --exclude="djangoqt/node_modules" `
  --exclude="djangoqt/dist" `
  --exclude="djangoserver2/__pycache__" `
  --exclude="djangoserver2/logs" `
  --exclude="djangoserver2/media" `
  --exclude="djangoserver2/.env" `
  --exclude="djangoserver2/.trae" `
  --exclude="djangoqt/.vscode" `
  djangoserver2 djangoqt .env docker-compose.yml

# 查看包大小（应该只有几 MB，不含 node_modules）
ls ai2617-deploy.tar.gz
```

> **为什么要 `--exclude`？**
> - `node_modules`/`dist`：前端容器构建时会自己 `pnpm install` + `pnpm run build`，传上去也是浪费
> - `djangoserver2/.env`：本地开发配置（DB_HOST=127.0.0.1），会和部署用的根目录 `.env`（DB_HOST=db）冲突
> - `__pycache__`/`logs`/`media`：临时文件、日志、上传文件，部署时不该带

#### ② 上传到服务器（在 Windows PowerShell 执行）

```powershell
# 把 tar 包传到 CentOS 的 /data 目录下（IP 换成你的服务器 IP）
scp ai2617-deploy.tar.gz root@服务器IP:/data/

# 如果服务器 SSH 端口不是 22，用 -P 指定：
scp -P 2222 ai2617-deploy.tar.gz root@服务器IP:/data/
```

> 也可以用 FinalShell/XShell 的图形界面拖拽上传，效果一样。

#### ③ 服务器解压并启动（在 CentOS 终端执行）

```bash
# 1. 创建项目目录并解压
mkdir -p /data/ai2617
cd /data/ai2617
tar -xzf /data/ai2617-deploy.tar.gz

# 2. 确认解压后的结构
ls
# 预期看到：djangoserver2  djangoqt  .env  docker-compose.yml

# 3. 再次确认 .env 里的 ALLOWED_HOSTS 是本机 IP（如果没改，在这里改）
vi .env
# 按 i 进入编辑，改 ALLOWED_HOSTS=你的CentOS_IP，按 Esc → :wq 保存退出
# 如果不会 vi，可以用：sed -i 's/ALLOWED_HOSTS=.*/ALLOWED_HOSTS=192.168.1.100/' .env
# （把 192.168.1.100 换成你的实际 IP）

# 4. 构建镜像并启动全部 5 个容器（首次部署和以后更新代码都用这一条）
#    首次构建会拉镜像 + pip install + pnpm install，大约 5~10 分钟
docker compose up -d --build

# 5. 查看容器状态（5 个容器都是 Up，db 和 redis 是 healthy 即成功）
docker compose ps

# 预期输出：
# NAME                STATUS                      PORTS
# ai2617-frontend     Up                          0.0.0.0:80->80/tcp
# ai2617-web          Up
# ai2617-worker       Up
# ai2617-mysql        Up (healthy)
# ai2617-redis        Up (healthy)

# 6. 查看 web 容器启动日志，确认 migrate 执行完成、无报错（Ctrl+C 退出）
docker compose logs -f web
```

**首次构建会看到的输出过程：**

```
[+] Running 9/9
 ✔ Network ai2617_webnet   Created               # 创建内网
 ✔ Volume ai2617_mysql_data Created              # 创建数据卷
 ✔ Volume ai2617_media_data Created
 ✔ Container ai2617-mysql   Healthy              # 等 MySQL 起来
 ✔ Container ai2617-redis   Healthy              # 等 Redis 起来
 ✔ Container ai2617-web     Started              # 跑 migrate + seed
 ✔ Container ai2617-worker  Started              # 启动 Celery
 ✔ Container ai2617-frontend Started             # 启动 Nginx
```

> **首次构建慢是正常的**：要下载 python:3.12-slim、node:20-alpine、nginx:stable-alpine、mysql:8.0、redis:7-alpine 等 5 个基础镜像（每个几十 MB ~ 几百 MB），还要 `pip install` 和 `pnpm install`。后续改代码再 build 时，Docker 层缓存生效，会快很多。

> **以后改代码后的更新流程**（不用再打包整个项目，只传改动的部分）：
> ```powershell
> # 本机：只打改动的项目（比如后端改了）
> tar -czf update.tar.gz `
>   --exclude="djangoserver2/__pycache__" `
>   --exclude="djangoserver2/logs" `
>   --exclude="djangoserver2/.env" `
>   djangoserver2
> scp update.tar.gz root@服务器IP:/data/ai2617/
> ```
> ```bash
> # 服务器：解压覆盖 + 重建
> cd /data/ai2617
> tar -xzf /data/update.tar.gz              # 覆盖旧代码
> docker compose up -d --build web worker    # 只重建后端
> ```

### 17.9 第 7 步：创建管理员账号（仅首次部署需要）

```bash
# 进入 web 容器创建 Django 超管账号
docker compose exec web python manage.py createsuperuser
```

按提示输入用户名、邮箱、密码（密码不显示，输入即可）。完成后用这个账号登录后端。

> **注意：** 本项目用自定义用户模型 `AUTH_USER_MODEL = 'system.User'`（主键字段叫 `userId`，不是默认的 `id`），JWT 已配 `USER_ID_FIELD='userId'`。所以登录走 `/api/auth/login`（在 `apps/system/urls.py` 里），不是 Django 自带的 `/admin/`。
>
> 项目里还有 `python manage.py seed` 造数命令（在 entrypoint.sh 里自动跑了），会预置一些角色、菜单、初始用户，可以查看 `apps/system/management/commands/seed.py` 了解。

### 17.10 第 8 步：验证部署结果

在 Windows 浏览器打开（把 `服务器IP` 换成 CentOS 的实际 IP，可用 `ip addr` 在 CentOS 终端查）：

| 验证项 | 方法 | 预期结果 |
|--------|------|----------|
| 前端页面 | 浏览器打开 `http://服务器IP/` | 看到 Vue3 登录页 |
| 登录接口 | 浏览器开 F12 → Network → 登录 | `/api/auth/login` 返回 `{code:200, data:{token:...}}` |
| 接口文档 | 浏览器打开 `http://服务器IP/api/schema/swagger/` | Swagger UI 页面 |
| 健康检查 | `curl http://服务器IP/api/` | 返回 `{"code":200,"msg":"ok","data":{"service":"djangoserver2-ent","status":"running"}}` |
| 前端刷新 | 前端某个子路由页面（如 `/system/user`）按 F5 | 不出现 404 |
| 数据库 | `docker compose exec db mysql -uroot -p123456` | 能登录并看到 `web2315_ent` 库 |
| Celery 任务 | 触发一个导出 Excel 的接口 | `docker compose logs -f worker` 看到 celery 收到任务 |

**如果外部机器无法访问，开放防火墙 80 端口**（详见 14.5 节）：

```bash
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --reload
```

> 如果是云服务器（阿里云/腾讯云），还要在云控制台的安全组里放行 80 端口。

### 17.11 docker compose 命令逐词拆解（先看懂每条命令在干嘛）

前面反复出现的 `docker compose up -d --build`、`docker compose logs -f web` 这些命令，如果只照抄不回懂，出错时就会抓瞎。这一节把每条命令**拆成一个个词**讲清楚，看完就能自己组合。

**先搞懂 `docker compose` 这 2 个词是什么：**

| 词 | 含义 | 类比 |
|----|------|------|
| `docker` | Docker 本体，负责管理镜像和单个容器 | 就像"水管总阀门" |
| `compose` | Docker 的**编排插件**，一次管理一组容器 | 就像"总控面板"，一个命令同时管 5 个容器 |

> 没有 compose 之前，5 个容器要 5 条 `docker run` 分别启动，还要手动建网络、写一堆参数。有了 compose，把这些都写进 `docker-compose.yml`，一条 `docker compose up` 就全搞定。

**命令全集对照表（先有个整体印象）：**

| 命令 | 作用 | 会不会删数据 |
|------|------|-------------|
| `docker compose up -d --build` | 重新构建镜像 + 创建并后台启动所有容器 | 不会 |
| `docker compose up -d` | 用**现有镜像**创建并启动容器（不重新 build） | 不会 |
| `docker compose ps` | 查看所有容器状态（Up/healthy） | 不会（只读） |
| `docker compose logs -f 服务名` | 实时跟踪某个容器的日志 | 不会（只读） |
| `docker compose exec 服务名 命令` | 进到某个运行中的容器里执行一条命令 | 不会 |
| `docker compose restart 服务名` | 重启容器（不重建镜像、不重读 .env） | 不会 |
| `docker compose stop` / `start` | 停止 / 启动所有容器（容器还在，只是停了） | 不会 |
| `docker compose down` | 停止并**删除**所有容器 + 删网络 | 不会（数据卷保留） |
| `docker compose down -v` | 连**数据卷一起删除** | ⚠️ 会清空数据库 |

---

#### ① `docker compose up -d --build`（最常用，逐词拆解）

```bash
docker compose up -d --build
│      │       │  │   │
│      │       │  │   └── build：重新构建镜像（代码改了必须加，否则用旧镜像）
│      │       │  └────── d = detached「分离/后台」：启动后不占用终端，可以继续敲命令
│      │       └───────── up：创建并启动 compose.yml 里定义的所有容器
│      └──────────────── compose：用编排插件（读 docker-compose.yml）
└─────────────────────── docker：调起 Docker 本体
```

**逐个词的作用：**

| 词 | 去掉会怎样 |
|----|-----------|
| `up` | 不写就没有启动动作。`up` 的本职是"创建 + 启动"，如果容器不存在就新建，存在就复用 |
| `-d` | 不加 `-d`，容器会**占住当前终端**一直刷日志，你没法敲下一条命令，只能 Ctrl+C 退出（但 Ctrl+C 会把容器也停掉）。所以生产必须加 `-d` 后台跑 |
| `--build` | 不加 `--build`，如果镜像已经构建过，就直接用**旧镜像**，你新改的代码不生效。第一次部署和改代码后都要加 |

> **一句话记忆：** `up` 管"启动"，`-d` 管"别挡着我敲命令"，`--build` 管"代码变了重新打包"。

**只操作某一个容器**（在命令末尾加服务名）：

```bash
docker compose up -d --build web        # 只重建并启动 web 这一个容器
docker compose up -d --build frontend   # 只重建前端
docker compose up -d --build web worker # 一次重建 web 和 worker 两个
```

> 服务名（`web`、`worker`、`frontend`、`db`、`redis`）就是 `docker-compose.yml` 里 `services:` 下面那 5 个名字，照着填。

---

#### ② `docker compose logs -f web`（看日志）

```bash
docker compose logs -f web
│      │       │    │   │
│      │       │    │   └── web：只看 web 这个容器的日志（换成 worker/db/redis/frontend 看别的）
│      │       │    └────── -f = follow「跟随」：日志有新内容就实时刷新，像 tail -f
│      │       └─────────── logs：查看容器输出的日志
│      └─────────────────── compose：编排插件
└────────────────────────── docker：Docker 本体
```

- **不加 `-f`**：只打印当前已有的日志，打印完就结束，不实时跟踪
- **不看某个容器，看全部**：直接 `docker compose logs`（不带服务名）
- **退出**：按 `Ctrl+C`

---

#### ③ `docker compose exec web bash`（进容器执行命令）

```bash
docker compose exec web bash
│      │       │    │   │
│      │       │    │   └── bash：在容器里开一个 Linux 终端（进 MySQL 就换成 mysql -uroot -p123456）
│      │       │    └────── web：目标容器名
│      │       └─────────── exec = execute「执行」：在运行中的容器里执行一条命令
└────────────────────────── compose / docker（同上）
```

- 等价于"钻进容器内部操作"，比如进去跑 `python manage.py` 命令
- **常见用法**：
  ```bash
  docker compose exec web python manage.py migrate      # 在容器里跑 migrate
  docker compose exec web bash                          # 进容器开终端，然后手动 ls、ps 等
  docker compose exec db mysql -uroot -p123456          # 进 MySQL 客户端
  ```

---

#### ④ `restart` / `stop` / `start` / `down` 的区别（重点：会不会丢数据）

| 命令 | 干了什么 | 容器还在吗 | 数据还在吗 |
|------|---------|-----------|-----------|
| `restart web` | 重启 web 容器（进程重启） | 在 | 在 |
| `stop` | 停止所有容器（相当于关机） | **在**（只是停了） | 在 |
| `start` | 把 stop 的容器重新启动 | 在 | 在 |
| `down` | 停止并**删除**容器 + 删掉 compose 建的网络 | ❌ 删了 | ✅ 在（存 named volume 里） |
| `down -v` | 删容器 + 删网络 + **删数据卷** | ❌ 删了 | ⚠️ **丢了** |

> **关键区分：**
> - `restart` ≈ 电脑的"重启"——不重建镜像，也不重新读 `.env`
> - `stop` + `start` ≈ 电脑的"关机 + 再开机"——容器对象还在
> - `down` ≈ 把容器"拆了重买"——容器没了，但数据存在 `mysql_data`、`media_data` 两个卷里，下次 `up` 还能找回
> - `down -v` ≈ 连硬盘数据一起抹掉——**改密码后想重置数据库才用，平时别碰**

> **记住一条铁律：改了 `.env` 或 `docker-compose.yml`，`restart` 没用，必须 `down` 再 `up`：**
> ```bash
> docker compose down && docker compose up -d
> ```
> 因为 `restart` 只会重启进程，不会重新读取环境变量和编排配置。

---

#### ⑤ 什么时候用哪个命令（速查表）

| 场景 | 该敲的命令 |
|------|-----------|
| 第一次部署 / 改了代码 | `docker compose up -d --build` |
| 只是重启用现有镜像 | `docker compose up -d` |
| 改了 `.env` 或 compose 文件 | `docker compose down && docker compose up -d` |
| 看某个容器日志 | `docker compose logs -f web` |
| 进容器跑 Django 命令 | `docker compose exec web python manage.py xxx` |
| 查看容器状态 | `docker compose ps` |
| 紧急重启某容器 | `docker compose restart web` |
| 彻底停止（删容器保留数据） | `docker compose down` |
| 重置数据库（清空数据） | `docker compose down -v` |

### 17.12 日常更新与运维命令（改代码后看这里）

```bash
# 进入项目目录（每次操作前都要先 cd 进来）
cd /data/ai2617

# ========== 改代码后重新部署 ==========
# 后端代码改了（Django 视图/模型等）：
docker compose up -d --build web worker      # 重建 web + worker（共用镜像）
docker compose up -d --build web             # 只重建 web 也行，但 worker 用的还是旧镜像

# 前端代码改了（Vue 组件等）：
docker compose up -d --build frontend

# 一次性全部重建（最稳妥，慢一点）
docker compose up -d --build

# ========== 查看日志 ==========
docker compose logs -f web         # Django 运行日志
docker compose logs -f worker      # Celery 任务日志
docker compose logs -f frontend   # Nginx 访问日志
docker compose logs -f db          # MySQL 日志

# ========== 手动执行 Django 管理命令 ==========
docker compose exec web python manage.py makemigrations
docker compose exec web python manage.py migrate
docker compose exec web python manage.py createsuperuser
docker compose exec web python manage.py seed              # 重跑造数（幂等）
docker compose exec web python manage.py shell             # 进 Django shell

# ========== 容器管理 ==========
docker compose ps                  # 查看所有容器状态
docker compose restart web         # 重启 web 容器（不动数据）
docker compose stop                # 停止所有容器（数据保留）
docker compose start               # 启动已停止的容器
docker compose down                # 停止并删除容器（数据在 volume 里不丢）
docker compose down -v             # ⚠️ 停止 + 删除数据卷（清空 MySQL 数据，慎用！）

# ========== 进入容器调试 ==========
docker compose exec web bash       # 进 web 容器开 shell
docker compose exec db mysql -uroot -p123456    # 进 MySQL
docker compose exec redis redis-cli             # 进 Redis
```

> **重要：改 `.env` 或 `docker-compose.yml` 后必须重启容器才生效**
>
> ```bash
> docker compose down && docker compose up -d
> ```
> 仅 `restart` 不会重新读 `.env`，必须 `down` 再 `up`。

### 17.13 常见问题排查

**问题 1：前端页面能打开，但接口报 502 Bad Gateway**

- **原因**：Nginx 联系不上 web 容器（web 没起来，或启动比 frontend 晚）
- **排查**：
  ```bash
  docker compose ps                    # 看 web 状态
  docker compose logs web               # 看后端报错
  docker compose exec frontend ping web # 测试 Nginx 能否解析 web 容器
  ```
- **解决**：
  - web 没起来：看日志修复后 `docker compose up -d --build web`
  - web 起来了但 frontend 解析不到：`docker compose restart frontend`（让 Nginx 重新解析）

**问题 2：前端路由页面刷新后 404**

- **原因**：Vue Router 用了 history 模式，但 nginx.conf 缺少 `try_files $uri $uri/ /index.html;`
- **解决**：第 3 步的 `nginx.conf` 已经配好这行，确认上传后没改坏；改完要 `docker compose up -d --build frontend`

**问题 3：后端日志报 "Access denied for user 'root'@'xxx'"（数据库密码错）**

- **排查**：确认 `.env` 里 `DB_PASSWORD=123456` 和 compose 里 `MYSQL_ROOT_PASSWORD: "123456"` 一致
- **修改后必须重启**：
  ```bash
  docker compose down
  docker compose up -d
  ```
- **注意：** 如果 `mysql_data` volume 里已有旧数据，改 `MYSQL_ROOT_PASSWORD` 不会生效（密码只在 MySQL 首次初始化时写入）。需要彻底重置数据库：
  ```bash
  docker compose down -v          # ⚠️ 删除数据卷，会清空所有 MySQL 数据
  docker compose up -d --build
  ```

**问题 4：接口跨域报错 CORS**

- 本方案所有请求都走 Nginx 同域转发，正常不会跨域
- **报跨域说明**：前端把地址写成了 `http://IP:8001` 直连后端，应改成相对路径 `/api/xxx`（你项目的 `src/config/index.ts` 里 `baseApi='/api'` 是对的）
- 或者：在 `.env` 里 `CORS_ALLOWED_ORIGINS=http://服务器IP` 显式放行

**问题 5：修改了 .env 或 docker-compose.yml 不生效**

- **原因**：`docker compose restart` 不会重新读 `.env`
- **解决**：
  ```bash
  docker compose down && docker compose up -d
  ```
- 改 `docker-compose.yml` 同理

**问题 6：worker 容器一直重启（Celery 报错）**

- **原因**：通常是连不上 Redis，或代码里 import 出错
- **排查**：
  ```bash
  docker compose logs worker
  docker compose exec worker celery -A config inspect ping   # 测试 celery
  ```

**问题 7：上传图片访问 404（`/images/xxx.jpg` 打不开）**

- **原因**：nginx.conf 没代理 `/images/`，或 web 容器没启动
- **解决**：第 3 步的 `nginx.conf` 已配 `location /images/`，确认上传后 `docker compose restart frontend`

**问题 8：构建时 pnpm install 报错 "ERR_PNPM_PEER_DEP_ISSUES"**

- **原因**：`package.json` 里某些包 peer 依赖冲突
- **临时解决**：把 Dockerfile 里 `pnpm install --frozen-lockfile` 改成 `pnpm install --frozen-lockfile --strict-peer-dependencies=false`
- **根治**：本机 `pnpm install` 时按提示修复 peer 依赖，重新生成 `pnpm-lock.yaml`

**问题 9：构建时 `vue-tsc` 打印一大段 tsc 帮助信息后报错（`failed to solve ... exit code: 1`）**

- **表现**：构建日志里 `RUN npm run build` 后，`vue-tsc` 输出了 `COMMON COMMANDS`、`COMMAND LINE FLAGS`、`COMMON COMPILER OPTIONS` 等 tsc 帮助信息，然后报错退出
- **根本原因**：前端 Dockerfile 是**多阶段构建**，要在容器内重新 `pnpm run build` 打包，需要的是**源码**（`src/`、`tsconfig.json`、`vite.config.ts`、`index.html` 等）。但上传的是打包好的 `dist`，容器里没有 `tsconfig.json` 和源码，`vue-tsc` 找不到项目 → 打印帮助 → 报错
- **解决**：重新上传**完整的 `djangoqt` 目录（源码）**，不要只传 dist：
  ```powershell
  # 本机打包源码（排除 node_modules 和 dist）
  tar -czf update-frontend.tar.gz `
    --exclude="djangoqt/node_modules" `
    --exclude="djangoqt/dist" `
    djangoqt
  scp update-frontend.tar.gz root@服务器IP:/data/ai2617/
  ```
  ```bash
  # 服务器覆盖并重建
  cd /data/ai2617
  tar -xzf update-frontend.tar.gz
  docker compose up -d --build frontend
  ```
- **必查**：确认 `djangoqt/` 里一定有 `package.json`、`pnpm-lock.yaml`、`tsconfig.json`、`vite.config.ts`、`index.html`、`src/` 目录、`Dockerfile`、`nginx.conf`、`.dockerignore`

**问题 10：构建时 vue-tsc 报具体 TypeScript 类型错误**

- 你项目的 `package.json` 里 `"build": "vue-tsc && vite build"` 会先跑类型检查，报的是 `.vue`/`.ts` 里的类型错误
- **临时绕过**：把 `package.json` 改成 `"build": "vite build"`（跳过类型检查，紧急部署用）
- **根治**：按报错信息修复 `.vue` / `.ts` 里的类型错误

## 十八、Docker Desktop 安装与使用（Windows / Mac 桌面版）

> 前面第一章~第十七章讲的都是 **CentOS 服务器上装 Docker 引擎**（纯命令行、没有图形界面）。这一章专门讲 **Docker Desktop**——给 **Windows / Mac 个人电脑**用的图形化 Docker。如果你平时在自己 Windows 电脑上学 Docker、跑容器，就用这个，不用装虚拟机。

### 18.1 Docker Desktop 是什么？和前面的 CentOS Docker 有什么区别？

**一句话：** Docker Desktop 是一个**带图形界面的桌面软件**，装好后里面自动包含了 Docker 引擎 + 命令行 + Docker Compose +（可选）Kubernetes，你在 Windows 的 PowerShell 里敲 `docker` 命令和在 CentOS 里敲是一模一样的。

| 对比项 | 前面讲的 CentOS Docker（第一~十七章） | 本章 Docker Desktop（Windows/Mac） |
|--------|----------------------------------------|--------------------------------------|
| 装在哪 | Linux 服务器 / 虚拟机里 | Windows / Mac 个人电脑上 |
| 有没有图形界面 | 没有，纯命令行 | 有，带图形管理面板（Dashboard） |
| 底层原理 | 直接跑在 Linux 内核上 | 靠 WSL2（Windows）/ HyperKit（Mac）模拟一个 Linux 内核 |
| 命令是否一样 | `docker ps`、`docker run`... | 完全一样，`docker ps`、`docker run`... |
| 适用场景 | 生产服务器、长期部署 | 本地开发、学习、调试 |

> **简单记：** 服务器上装 Docker 引擎 = 自己动手搭厨房；Windows 上装 Docker Desktop = 买个带装修的样板房，拎包入住。命令行操作两者完全通用，所以前面学的 `docker` 命令这里照样能用。

### 18.2 安装前准备：检查系统要求（Windows）

Docker Desktop 在 Windows 上依赖 **WSL 2**（Windows Subsystem for Linux 第二版），先确认你的电脑满足以下条件：

| 要求 | 如何检查 |
|------|----------|
| Windows 10 64 位（版本 2004+，内部版本 19041+）或 Windows 11 | 设置 → 系统 → 关于，看"版本"和"系统类型" |
| CPU 支持虚拟化并在 BIOS 里开启 | 任务管理器 → 性能 → CPU，看右下角"虚拟化: 已启用" |
| 至少 4GB 内存 | 任务管理器 → 性能 → 内存 |
| WSL 2 已安装 | PowerShell 执行 `wsl -l -v`（下面 18.3 会讲怎么装） |

> **如果"虚拟化"显示禁用：** 重启电脑进 BIOS（开机按 Del/F2/F12，因主板而异），找到 `Intel VT-x` 或 `AMD-V` / `SVM Mode`，设为 `Enabled`，保存重启。

### 18.3 安装步骤（Windows，逐条照做）

**第 1 步：开启 WSL 2（Windows 子系统 for Linux）**

以**管理员身份**打开 PowerShell（开始菜单 → 右键 PowerShell → 以管理员身份运行），执行：

```powershell
# 安装 WSL（包含 WSL2 内核和默认的 Ubuntu 发行版）
wsl --install
```

装完**重启电脑**。重启后再打开 PowerShell，验证：

```powershell
# 查看已安装的发行版和版本号，VERSION 列显示 2 才对
wsl -l -v
```

> 说明：`wsl --install` 是 Windows 10 较新版本才支持的一键命令。如果你的系统较旧提示无法识别，去微软官网搜 "WSL2 手动安装步骤" 按教程装。

**第 2 步：下载 Docker Desktop 安装包**

去 Docker 官网下载页（国内访问可能慢，可挂代理或用阿里云等镜像站搜 "Docker Desktop Installer Windows"）：

- 官方地址：`https://docs.docker.com/desktop/install/windows-install/`
- 下载文件名类似：`Docker Desktop Installer.exe`（约 500MB+）

**第 3 步：运行安装程序**

1. 双击 `Docker Desktop Installer.exe`
2. 安装向导里，勾选 **"Use WSL 2 instead of Hyper-V"**（推荐，性能更好）
3. 点 **OK** 开始安装，等几分钟
4. 安装完成后点 **"Close and restart"**，电脑会重启

**第 4 步：首次启动**

1. 重启后，开始菜单里找 **Docker Desktop**，点开
2. 弹出"接受协议"窗口，勾选接受，点 **Accept**
3. 等右下角鲸鱼图标变绿 / 状态栏显示 **"Docker Desktop is running"**，说明启动完成

### 18.4 验证安装是否成功

打开 PowerShell（普通身份即可），执行：

```powershell
# 看 Docker 版本
docker --version

# 看详细版本（Client + Server 都有才算成功）
docker version

# 跑一个测试容器，看到 "Hello from Docker!" 即成功
docker run hello-world
```

> 如果 `docker version` 的 Server 部分报错 "Cannot connect to the Docker daemon"，说明 Docker Desktop 没启动完，等右下角鲸鱼图标变绿再试。

### 18.5 配置镜像加速（国内必做，否则拉镜像很慢）

这一步等价于前面第七章在 CentOS 里改 `/etc/docker/daemon.json`，只不过 Docker Desktop 改的地方在**图形界面里**：

1. 点右下角鲸鱼图标 → **Settings**（齿轮）
2. 左侧菜单选 **Docker Engine**
3. 右侧会出现一个 JSON 编辑框，改成：

```json
{
  "registry-mirrors": [
    "https://docker.1panel.live",
    "https://docker.m.daocloud.io",
    "https://dockerproxy.com",
    "https://docker.nju.edu.cn",
    "https://hub-mirror.c.163.com"
  ],
  "experimental": false
}
```

4. 点右上角 **Apply & restart**，等 Docker 重启完成

**验证加速是否生效：**

```powershell
# 看生效的镜像源，能看到上面配的地址就对了
docker info
```

### 18.6 基本使用：图形界面 + 命令行

Docker Desktop 同时给你**两种操作方式**，日常推荐：简单操作用图形界面，精细控制用命令行。

**① 图形界面（Dashboard 面板）**

打开 Docker Desktop 主窗口，左侧主要分这几块：

| 区域 | 作用 |
|------|------|
| **Containers** | 看所有容器（运行中/已停止），可一键启动、停止、重启、删除、进终端、看日志 |
| **Images** | 看本地已下载的镜像，可基于镜像一键创建容器、删除镜像 |
| **Volumes** | 数据卷（持久化存储），看哪些卷被容器使用 |
| **Builds** | 构建记录（用 Dockerfile 构建镜像的历史） |

> 举例：在 **Images** 里找到 `nginx`，点 **Run**，填上端口映射 `8080:80`，点 Run，就等于执行了 `docker run -p 8080:80 nginx`，浏览器访问 `http://localhost:8080` 就能看到 nginx 欢迎页。

**② 命令行（和 CentOS 完全一样）**

前面第六章 6.4 的命令速查在 Windows 上**原封不动照用**，例如：

```powershell
# 拉镜像
docker pull nginx

# 跑一个容器，名字 web，宿主 8080 映射容器 80
docker run -d --name web -p 8080:80 nginx

# 看运行中的容器
docker ps

# 进容器开终端
docker exec -it web bash

# 停止 / 启动 / 删除
docker stop web
docker start web
docker rm -f web
```

> **区别提示：** CentOS 里文件路径是 `/data/mysql` 这种 Linux 路径；Windows 上挂载目录要用 Windows 路径，如 `-v D:\docker\data\mysql:/var/lib/mysql`，注意盘符和反斜杠。

### 18.7 一个完整小例子：在 Windows 上跑 MySQL

```powershell
# 1. 在 D 盘建好数据目录（持久化用）
mkdir D:\docker\data\mysql

# 2. 拉镜像
docker pull mysql:8.0

# 3. 跑容器：端口 3306，root 密码 123456，数据持久化到 D 盘
docker run -d --name mysql `
  -p 3306:3306 `
  -e MYSQL_ROOT_PASSWORD=123456 `
  -v D:\docker\data\mysql:/var/lib/mysql `
  mysql:8.0

# 4. 看是否在跑
docker ps

# 5. 进入容器登录 MySQL
docker exec -it mysql mysql -uroot -p123456
```

> 说明：PowerShell 里的续行符是反引号 `` ` ``（对应 CentOS 里的反斜杠 `\`）。也可以把整个 `docker run` 写成一行不断开。

### 18.8 Docker Desktop 常见问题

**问题 1：启动报 "Docker Desktop requires a newer WSL kernel version"**

```powershell
# 以管理员身份运行 PowerShell，更新 WSL 内核
wsl --update
```

更新完重启 Docker Desktop。

**问题 2：右下角鲸鱼一直转圈不变绿**

- 检查 BIOS 虚拟化是否开启（见 18.2）
- 检查 WSL2 是否装好：`wsl --status`
- 杀掉残留进程后重启：任务管理器结束所有 `Docker`、`wsl` 相关进程，再重新打开 Docker Desktop

**问题 3：C 盘空间被 Docker 吃光了**

Docker Desktop 默认把镜像/容器存 C 盘。改到 D 盘：
- Settings → Resources → Advanced → Disk image location 改成 `D:\DockerData`
- 点 Apply & restart（注意：会迁移已有数据，耗时较长）

**问题 4：容器里访问宿主机（你的 Windows）服务**

容器里 `localhost` 指的是容器自己，不是你的 Windows。要用特殊域名：

- Docker Desktop 18.x+：容器内访问 `host.docker.internal` 即等于访问你的 Windows
- 例：你的 Windows 上跑了 MySQL 在 3306，容器里连接串写 `host.docker.internal:3306`

**问题 5：和 VMware/VirtualBox 冲突**

Docker Desktop 用了 Hyper-V/WSL2，可能和老版 VMware 冲突。建议：
- VMware 升级到 15.5.5+（支持和 Hyper-V 共存）
- 或者用 Docker Desktop 的 WSL2 模式（默认就是，比 Hyper-V 模式兼容性好）

> **何时该从 Docker Desktop 换回 CentOS 服务器 Docker？** 当你的项目要部署到真实服务器、要做 CI/CD、要 7×24 跑——那时候用前面第一~十七章的 CentOS 方案。本地开发学习，Docker Desktop 足够。

## 附录：Docker 版本说明

| 版本类型 | 说明 |
|----------|------|
| **docker-ce** | Docker Community Edition，社区版，免费使用 |
| **docker-ce-cli** | Docker 命令行工具 |
| **containerd.io** | 容器运行时 |

推荐使用最新稳定版（Stable），适合生产环境。