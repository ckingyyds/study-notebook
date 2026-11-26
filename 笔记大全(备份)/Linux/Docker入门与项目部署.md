**Docker 入门与项目部署**

这里只是快速上手docker,如果想看完整操作,请看这个笔记:[[Docker]]

---

> **这份笔记的写法**：先是概念（一~三），然后是我**自己动手做的完整记录**（四~六，附真实输出和踩过的坑），中间是通用操作（七~十三）；**第十四章单独讲 Nginx**（统一入口、反向代理、负载均衡）；十五章以后是进阶和速查表。

---

## 一、Docker 到底解决什么问题

上一份笔记[[Tomcat部署网页-虚拟机]]里那套流程，自己也走了一遍：传 JDK、传 Tomcat、解压、挪目录、改 `/etc/profile`、`source`、启动、放网页、关防火墙……

**麻烦在哪：**

1. 换一台服务器，上面所有步骤**重来一遍**；
2. 项目还要装 MySQL、Redis、Nginx……每个都要手动装、手动配；
3. 用户量大了要做**集群**，那就得在第二台、第三台机器上再重复一遍；
4. 装错了、版本不对了，环境就废了。

**Docker 的思路**：别人已经把一个"装好 JDK+Tomcat 的精简 Linux"打包成了一个文件（镜像），你**下载下来、一条命令跑起来**就行。

```bash
# 传统方式：装 JDK + 配环境变量 + 装 Tomcat + 传项目，十几步
# Docker 方式：一条命令
docker run -d -p 8080:8080 --name tomcat-01 tomcat:8
```

用老师的话说：**做同样的发布，两种方式对比，你就体会到 Docker 的优势了。**

---

## 二、安装 Docker

按完整笔记里的安装步骤一步步执行即可（装 docker-ce、启动服务、设置开机自启）。

### 2.1 装完之后一定要配"镜像加速器"

Docker 拉取镜像默认走国外仓库，速度很慢。这和 pip 换清华源是同一件事。

**配置文件位置**：`/etc/docker/daemon.json`

```bash
mkdir -p /etc/docker
vi /etc/docker/daemon.json
```

写入（多个地址用逗号隔开，Docker 会按顺序尝试）：

```json
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://dockerproxy.com",
    "https://docker.1panel.live"
  ]
}
```

**重启 Docker 让它生效：**

```bash
systemctl daemon-reload
systemctl restart docker
```

验证生效：

```bash
docker info | grep -A 5 "Registry Mirrors"
```

> **说明**：以前阿里云、腾讯云的加速器可以直接用，现在基本只对"买了他们云服务器"的用户开放了，个人本地虚拟机用不了。所以现在只能配第三方地址，而且第三方不如官方稳定，**多配几个**，一个不通会自动试下一个。

---

## 三、三个核心概念（必须分清）

| 概念 | 英文 | 通俗理解 | 类比 |
| :--- | :--- | :--- | :--- |
| **仓库** | Registry | 存放镜像的"网盘"，配的加速器就是仓库地址 | Python 的 PyPI、手机的 App Store |
| **镜像** | Image | 一个装好环境的"安装包/模板"，**它自己不能提供服务** | 一个 `.exe` 安装程序、一个系统 ISO |
| **容器** | Container | 镜像**跑起来**之后的一个实例，一个能用的服务 | 装好并正在运行的系统 |

**三者的关系：**

```
仓库(Registry)  --docker pull-->  镜像(Image)  --docker run-->  容器(Container)
```

几个关键认识：

- **镜像是"精简版 Linux + 装好的软件"**。Tomcat 镜像 = 一个极简 Linux 内核 + JDK + Tomcat，只保留能跑 Tomcat 需要的东西，几 MB 到几百 MB，不是完整的 4G 的 CentOS。
- **一个镜像可以跑出无数个容器**。就像同一个安装包可以在多台电脑上装，只要你的机器扛得住。
- **容器之间互相隔离**。两个 Tomcat 容器内部都用 8080 端口，谁也不冲突，因为它们看不见对方。

> 老师用 Windows 打比方：纯净版 Windows 装完什么都没有，得自己一个个装软件；Ghost 版装完 PS、Office 都自带了。**镜像就是"把环境预先装好的那个系统盘"**。

---

## 四、第一步：先确认我的 Docker 到底能不能用

Docker 是之前装的，隔了一段时间没动，先别急着用，**七条命令把家底摸清楚**。一次整段贴进虚拟机：

```bash
echo "===== 1. Docker 版本 ====="
docker -v

echo "===== 2. 服务是否运行 ====="
systemctl is-active docker

echo "===== 3. 是否开机自启 ====="
systemctl is-enabled docker

echo "===== 4. 镜像加速器配置 ====="
docker info 2>/dev/null | grep -A 5 "Registry Mirrors"

echo "===== 5. 本地已有镜像 ====="
docker images

echo "===== 6. 本地已有容器 ====="
docker ps -a

echo "===== 7. Docker 数据目录剩余空间 ====="
df -h /var/lib/docker 2>/dev/null || df -h /
```

### 4.1 我机器上的实际输出

```
===== 1. Docker 版本 =====
Docker version 26.1.4, build 5650f9b

===== 2. 服务是否运行 =====
active

===== 3. 是否开机自启 =====
enabled

===== 4. 镜像加速器配置 =====
Registry Mirrors:
 https://docker.m.daocloud.io/
 https://dockerproxy.com/
 https://mirror.baidubce.com/
Live Restore Enabled: false

===== 5. 本地已有镜像 =====
REPOSITORY   TAG   IMAGE ID       CREATED        SIZE
mysql        8.0   6cd09145362d   6 months ago   799MB

===== 6. 本地已有容器 =====
CONTAINER ID   IMAGE          COMMAND                  STATUS              PORTS                    NAMES
a4e5d88db5c0   mysql:8.0      "docker-entrypoint.s…"   Up 7 hours          0.0.0.0:3306->3306/tcp   mysql
cf2340dcce4b   e2ac70e7319a   "/hello"                 Exited (0) 8 hours   ago                      eloquent_morse

===== 7. Docker 数据目录剩余空间 =====
文件系统               容量  已用  可用 已用% 挂载点
/dev/mapper/centos-root   17G  7.8G  8.0G   50% /
```

### 4.2 逐条判断

| # | 输出 | 判断 |
| :-- | :--- | :--- |
| 1 | `Docker version 26.1.4` | ✅ 装好了，版本还比较新 |
| 2 | `active` | ✅ 服务在跑 |
| 3 | `enabled` | ✅ 开机自启已配，不用每次手动起 |
| 4 | 三个加速器地址 | ✅ 配过了 |
| 5 | `mysql 8.0` 799MB | ✅ 拉过镜像，说明加速器真的能用 |
| 6 | 一个 mysql 容器 + 一个 `eloquent_morse` | ✅ 跑过容器 |
| 7 | 17G 总量 / **8.0G 可用** | ⚠️ 够用，但要注意 |

### 4.3 三个发现

**发现一：那个 `eloquent_morse` 是我之前跑 hello-world 留下的**

它的 `COMMAND` 是 `"/hello"`，`STATUS` 是 `Exited (0)`——**退出码 0 表示程序正常执行完毕**，不是报错。也就是说"拉取→运行"这条路当时就走通了。

> 但它的 `IMAGE` 显示的是 `e2ac70e7319a` 这样一串 ID，而不是 `hello-world:latest`。说明 hello-world 镜像后来被删了，容器记录却还留着。这种情况删容器就行：
>
```bash
docker rm eloquent_morse
```

**发现二：三个加速器地址，只有第一个还活着**

```
docker.m.daocloud.io    ← 还在，拉 mysql 就是靠它
dockerproxy.com         ← 早就停了
mirror.baidubce.com     ← 已不再对外服务
```

Docker 是"按顺序逐个尝试"的，第一个能通就用它，所以现在没出问题。但万一哪天第一个也挂了就彻底拉不动了，**建议换一组多个备用**：

```json
{
  "registry-mirrors": [
    "https://docker.m.daocloud.io",
    "https://docker.1panel.live",
    "https://dockerpull.org"
  ]
}
```

改完 `systemctl daemon-reload && systemctl restart docker`。⚠️ 这一步会重启 docker 服务，**正在运行的容器会受影响**（配了自动重启策略的会自己拉起来）。

**发现三：磁盘只剩 8G**

拉一个 `tomcat:8` 占 456MB，足够。但镜像会一直堆着不自动清，别乱拉。

### 4.4 真正的连通性测试

配置看得见摸不着，跑一个最轻量的镜像才知道路通不通：

```bash
docker run hello-world
```

成功的标志是这段：

```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
```

看到 `Hello from Docker!`，**拉取、存储、运行三个环节全通**。

失败的话对照：

| 报错 | 原因 |
| :--- | :--- |
| `context deadline exceeded` | 加速器没生效 / 地址全挂了 |
| `Cannot connect to the Docker daemon` | 服务没启动，`systemctl start docker` |
| `permission denied ...docker.sock` | 没加 sudo，或用户不在 docker 组 |
| 卡在 `Pulling from` 不动 | 网络问题，换加速器地址 |

---

## 五、第二步：把 tomcat 镜像拉下来

### 5.1 老师为什么没演示这一步

课上老师直接 `docker run` 就起来了，因为**他虚拟机里 2 年前就有 `tomcat:8` 这个镜像**（`CREATED: 2 years ago`）。他机器上还躺着 `nginx:latest` 和 `mysql:8`，都是之前做实验时拉下来的。

我的机器上只有一个 `mysql:8.0`（之前学 MySQL 时拉的），tomcat 这边是空的，不先拉，`docker run` 会直接报：

```
Unable to find image 'tomcat:8' locally
docker: Error response from daemon: ...
```

所以这一步**自己做的时候必须先执行**（课上没讲到，属于补充）。

### 5.2 实际操作

```bash
cd
docker pull tomcat:8
```

输出（八层依次下载）：

```
8: Pulling from library/tomcat
23828d760c7b: Pull complete
dfc0d9a6855e: Pull complete
3026e8edc115: Pull complete
cd47cb4ec28d: Pull complete
6789efe2b2c4: Pull complete
4a8163f4748d: Pull complete
065654ac7cb2: Pull complete
744365692396: Pull complete
Digest: sha256:06982e168b3d83bd903c0c9a54f9e14fe5ee4a1a2cbcc602be27f6748229735e
Status: Downloaded newer image for tomcat:8
docker.io/library/tomcat:8
```

### 5.3 确认拉到了

```bash
docker images
```

```
REPOSITORY   TAG   IMAGE ID       CREATED        SIZE
mysql        8.0   6cd09145362d   6 months ago   799MB
tomcat       8     0a2249be3d31   2 years ago    456MB
```

五列的含义：

| 列 | 含义 |
| :--- | :--- |
| `REPOSITORY` | 镜像名（tomcat / nginx / mysql） |
| `TAG` | 版本标签（8 / latest） |
| `IMAGE ID` | 镜像唯一编号（类似数据库主键） |
| `CREATED` | 创建时间 |
| `SIZE` | 大小 |

> `CREATED` 显示 `2 years ago` 不是"下载了两年前的旧东西"——这是**镜像被构建出来的时间**，跟你什么时候下载的无关。

### 5.4 为什么必须是 `tomcat:8` 而不是 `tomcat`

```bash
docker pull tomcat        # 等价于 docker pull tomcat:latest
```

不写 tag 默认拉 `latest`，也就是 Tomcat 10/11。而 **Tomcat 10 之后 Servlet 规范从 `javax.*` 换成了 `jakarta.*`**，课上的代码、老项目全部不兼容。所以统一锁 `tomcat:8`，和老师环境保持一致。

**镜像名 + 冒号 + 版本号（tag）**，这是定位一个镜像的标准写法。也可以用 IMAGE ID 代替名字，效果一样。

其他常用镜像同理：

```bash
docker pull nginx           # 不写版本，默认 latest
docker pull mysql:8
```

---

## 六、第三步：练手——用 hello-world 走一遍完整闭环

正式跑 Tomcat 之前，先用 hello-world 把"拉取 → 运行 → 查看 → 删除"整套动作走一遍。它只有 13KB，秒拉秒跑，**而且刚好能演示"镜像被容器占用就删不掉"这个坑**。

### 6.1 第一次运行

```bash
docker pull hello-world
docker run hello-world
```

第一次 `run` 时如果出现 `Unable to find image 'hello-world:latest' locally` 再自动 Pull，那是正常的（说明本地还没有，Docker 自动去拉了）。

### 6.2 「跑完就退出」不是失败

执行完 hello-world 之后：

```bash
docker ps
```

**空的。** 别慌——hello-world 不是服务，是个"打印一行字就结束"的小程序，跑完自然就退出了。

```bash
docker ps -a
```

```
CONTAINER ID   IMAGE         COMMAND    CREATED          STATUS                      PORTS   NAMES
1a2b3c4d5e6f   hello-world   "/hello"   10 seconds ago   Exited (0) 9 seconds ago            keen_hopper
```

三个关键信息：

| 列 | 值 | 含义 |
| :-- | :--- | :--- |
| `STATUS` | `Exited (0)` | **退出码 0 = 正常执行完毕**，不是报错 |
| `COMMAND` | `"/hello"` | 容器里执行的就是这么个程序 |
| `NAMES` | `keen_hopper` | 没写 `--name`，Docker 随机起的名字 |

> **和 mysql 对比着记**：mysql 是 `Up 7 hours`，因为它是个**长期服务**，起来就不走了；hello-world 是**一次性任务**，干完就退。后面学的 tomcat 属于前面那种。

**`docker ps` 输出的列**：`CONTAINER ID`、`IMAGE`（用哪个镜像跑的）、`COMMAND`、`CREATED`、`STATUS`（`Up` 运行中 / `Exited` 已退出）、`PORTS`（端口映射）、`NAMES`（起的名）。

> `ps` 和 `ps -a` 的关系，跟 Linux 的 `ls` 和 `ls -a` 一模一样。

### 6.3 再跑一次，感受本地缓存

```bash
docker run hello-world
```

这次**瞬间出结果**，不会再有 Pulling 的过程——镜像只在第一次下载，之后都从本地直接起容器。

再 `docker ps -a` 看一眼，现在有**两个** `Exited` 容器。这就是"一个镜像可以跑出无数个容器"。

### 6.4 看已退出的容器说过什么

```bash
docker ps -a               # 拿到某个容器 ID 的前几位
docker logs 1a2b           # 用它看那个容器的输出
```

容器已经退出了，但它的输出还在。**这个命令是排查"容器为什么起不来"的第一招**，容器起不来时先 `docker logs 容器名` 看它报了什么错。

### 6.5 删除的正确顺序：先删容器，再删镜像

```bash
docker rm 1a2b             # ID 前几位就够，能唯一识别即可
```

⚠️ **`docker rm` 只能删已经停止的容器**。容器还在 `Up` 时会报 `You cannot remove a running container`，得先 `docker stop`，或者用 `docker rm -f` 强删。

### 6.6 故意踩坑：镜像还被容器引用着（重点）

这是笔记里"删除镜像"那条警告的**活体验证**。留一个 `Exited` 的 hello-world 容器别删，然后直接删镜像：

```bash
docker rmi hello-world
```

**会失败**：

```
Error response from daemon: conflict: unable to remove repository reference "hello-world"
(must force) - container 9f8e7d6c5b4a is using its referenced image a1b2c3d4e5f6
```

翻译：**还有个容器在用这个镜像，删不了。**

为什么 Docker 这么设计？因为镜像一删，那个容器就再也起不来了（`docker start` 会失败）。它是在保护你。

按顺序解开：

```bash
docker ps -a              # 找到还引用 hello-world 的那个容器
docker rm <那个容器ID>     # 先把容器删掉
docker rmi hello-world    # 再删镜像，这次成功
```

```
Untagged: hello-world:latest
Untagged: hello-world@sha256:...
Deleted: sha256:a1b2c3d4e5f6...
```

```bash
docker images             # hello-world 没了
```

> `-f` 强删（`docker rmi -f hello-world`）能绕过限制，但**不建议**——会留下一个"引用了不存在的镜像"的僵尸容器，后面清理更麻烦。养成"先删容器再删镜像"的习惯。

### 6.7 `--rm`：跑完自动清理

如果压根不想留容器记录，跑的时候加 `--rm`：

```bash
docker run --rm hello-world
docker ps -a              # 没有新容器留下来
```

适合临时测试、跑个命令看看结果。

### 6.8 对比：长期运行的容器怎么停

hello-world 是"自己结束"，mysql 是"你让它停才停"：

```bash
docker ps                 # mysql 在 Up
docker stop mysql         # 主动停
docker ps                 # 看不到了
docker ps -a              # 还在，STATUS 变成 Exited (0)
docker start mysql        # 再启动
docker ps                 # 回到 Up
```

⚠️ **mysql 可以随便停启，数据不会丢**（数据存在容器自己的存储层里，不是内存）。但**不要 `docker rm mysql`**——那会连同数据一起删掉，除非当初用 `-v` 挂载到了宿主机。

---

## 七、命令参考（还没亲手练到的）

上面练到的：`pull` / `run` / `ps` / `ps -a` / `logs` / `rm` / `rmi` / `stop` / `start`。剩下这些先记着，用到再说。

### 7.1 只看容器 ID

```bash
docker ps -q       # 只输出运行中容器的 ID
docker ps -aq      # 所有容器的 ID
```

### 7.2 批量操作（很实用）

```bash
# 停掉所有正在运行的容器
docker stop $(docker ps -q)

# 删除所有容器（不管运行还是停止的，前面得先停）
docker rm $(docker ps -aq)
```

> `$(命令)` 是 Linux 的命令替换：先执行括号里的命令，把输出结果填到这里。

### 7.3 重启

```bash
docker restart <容器ID或名字>
```

### 7.4 删除容器的铁律

⚠️ 顺序永远是：**先 `stop`，再 `rm`**；删镜像同理，**先 `rm` 容器，再 `rmi` 镜像**。

---

## 八、跑起来：第一个 Tomcat 容器

> **这一章做什么**：把 Tomcat 真正跑起来，并确认它没跑着跑着退出。
> **做完会看到**：`docker ps` 里有一个 `Up` 状态的容器，但浏览器打开始终是 404 —— 这不是失败，下一章专门讲它。

### 8.1 运行容器的完整命令

```bash
docker run -d -p 8080:8080 --name tomcat-01 tomcat:8
```

**一个参数一个参数看：**

| 参数 | 全称 | 作用 | 不写会怎样 |
| :--- | :--- | :--- | :--- |
| `-d` | detach | **后台运行**（守护进程方式） | 容器在前台跑，会把当前终端占住，什么命令都敲不了 |
| `-p 8080:8080` | publish | **端口映射**：`宿主机端口:容器端口` | 容器外面访问不到，浏览器打不开 |
| `--name tomcat-01` | — | 给容器起名字，方便区分和管理 | Docker 随机起个名字（比如 `vibrant_morse`） |
| `tomcat:8` | — | 用哪个镜像 | — |

**关于 `-d` 阻塞的直观理解：**

老师用 `tail -f hello.txt` 演示过：这个命令会一直挂着监控文件新增内容，终端就卡在那儿，`Ctrl+C` 才能退出。`docker run` 不加 `-d` 是一模一样的道理。

（补充：Python 里的守护线程 `daemon=True` 也是这个意思——主线程结束，守护线程立刻被杀掉，不在前台挡着。）

**关于 `-p 8080:8080` 的原理：**

```
浏览器 → 虚拟机IP:8080 → 宿主机的 8080 端口
                              ↓ (端口映射)
                         容器内部的 8080 端口 → 容器里的 Tomcat
```

Tomcat 在容器里跑，也是监听 8080。把宿主机的 8080 和容器的 8080 绑在一起，访问宿主机 8080 就等于访问容器里的 Tomcat。

**冒号前面是宿主机的，后面是容器的。**

**如果报 `Bind for 0.0.0.0:8080 failed: port is already allocated`：**

说明宿主机的 8080 已经被别的程序占了（最常见是之前手工装过 Tomcat）。两个选择：

- **图快**：把 `-p 8080:8080` 改成 `-p 8081:8080`，后面访问一律用 8081；
- **图彻底**：翻到第十五章，把宿主机 Tomcat 和容器各分一个端口，两个都留着用。

### 8.2 运行之后检查

```bash
docker ps
```

看到 `STATUS` 是 `Up`、`PORTS` 是 `0.0.0.0:8080->8080/tcp`，就成功了。

如果 `STATUS` 是 `Exited`，说明启动就退出了，看日志：`docker logs tomcat-01`。

---

## 九、浏览器访问出现 404——这不是错误

> **这一章做什么**：搞清楚那个 404 到底是什么意思，以及打不开时按什么顺序排查。

浏览器打开 `http://虚拟机IP:8080`，看到的是：

```
HTTP 状态 404 - 未找到
类型 状态报告
描述 源服务器未能找到目标资源的表示或者是不愿公开一个已经存在的资源表示。
Apache Tomcat/8.5.100
```

**关键点：这个页面底部写着 `Apache Tomcat/8.5.100`。**

这说明：**已经成功访问到 Tomcat 了**，只是这个 Tomcat 里什么都没有。

原因：**官方 `tomcat:8` 镜像里，`webapps` 目录是空的**。上一份笔记里本机装的 Tomcat 自带一个 `ROOT` 项目，所以访问 `IP:8080` 能看到那只猫的欢迎页；但 Docker 镜像为了精简，把 `ROOT`、`docs`、`examples` 全删了，所以返回 404。

> 对比记忆：
> - 本机 Tomcat → `webapps` 里有 `ROOT` → `IP:8080` 能出页面
> - Docker 的 Tomcat → `webapps` 是空的 → `IP:8080` 出 404
>
> 404 页面能显示出 Tomcat 的版本号，就证明端口通了、服务在跑。

**打不开的三种情况：**

| 现象 | 检查 | 解决 |
| :--- | :--- | :--- |
| 浏览器转圈然后超时 | `systemctl status firewalld` | `systemctl stop firewalld`（或 `firewall-cmd --add-port=8080/tcp --permanent && firewall-cmd --reload`） |
| `Connection refused` | `ss -lntp \| grep 8080` | 没进程就是容器没起来，`docker logs tomcat-01` |
| 页面显示的是别的服务 | `ss -lntp \| grep 8080` 是谁占的 | 本机有旧 Tomcat/旧容器占着 8080，先停掉 |

---

## 十、把项目放进容器

> **这一章做什么**：把宿主机上的网页放进去，让浏览器能看到内容。
> **做完会看到**：`http://虚拟机IP:8080/dict/` 出页面。

项目在**宿主机**（Linux）上，Tomcat 在**容器**里，这两者之间原来没有通路。有两条路：`docker cp` 拷进去，或者 `docker exec` 进去看看再拷。

### 10.1 先进入容器内部看看

```bash
docker ps                          # 拿到 CONTAINER ID
docker exec -it <容器ID> bash      # 以交互模式进入容器
```

- `exec`：在运行的容器里执行命令
- `-i`：保持标准输入打开（交互）
- `-t`：分配一个伪终端
- `bash`：要执行的程序（进一个 bash 命令行）

进去之后，**提示符会变成容器 ID 前缀**（比如 `root@5097...:/usr/local/tomcat#`），说明已经在容器内部了。

```bash
pwd                    # /usr/local/tomcat
ls                     # 这就是 Tomcat 的目录结构
cd webapps
ls                     # 空的！这就是 404 的原因
pwd                    # /usr/local/tomcat/webapps
exit                   # 退出容器，回到宿主机
```

> 容器内部的路径是 `/usr/local/tomcat`，不是宿主机上的 `/opt/tomcat8`。两边是**隔离的两套文件系统**。

**一个小发现（课上演示过）**：容器里虽然是 Linux，但 `vi`、`vim` 都没有：

```bash
touch a.txt     # 可以用
ls              # 可以用
cd              # 可以用
vi a.txt        # bash: vi: command not found
```

因为它是个"精简版"，只保留了能跑服务必需的指令，编辑器这种用不上的就没装。这也说明：**在容器里改代码是不现实的做法**。

### 10.2 从宿主机拷进去

```bash
# 语法：docker cp 宿主机路径 容器ID:容器内路径
docker cp ./dict <容器ID>:/usr/local/tomcat/webapps/
```

老师课上把宿主机 `/opt/tomcat8/webapps/mi`（那个"小米"网页项目）拷进了容器的 `webapps` 下。

我自己的项目叫 `dict`，就在同一个 `webapps` 目录里（`/opt/tomcat8/webapps/dict`）。下面统一拿它演示，操作完全一样。

拷完再进去验证：

```bash
docker exec -it <容器ID> bash
cd webapps
ls              # 能看到 dict 了
exit
```

然后浏览器访问 `http://虚拟机IP:8080/dict/`，页面就出来了。

---

## 十一、把容器"打包"成新镜像（commit）

> **这一章做什么**：把"已经装好项目的容器"固化成一个新镜像。
> **解决什么问题**：不想每换一台机器就 `docker cp` 一遍。

每次都 `docker cp` 拷一遍太麻烦，尤其是要部署到多台服务器时。

**思路**：把"已经发布好项目的容器"重新打包成一个**新镜像**，以后直接运行这个新镜像，项目就在里面了。

`docker commit` 只打包**容器的可写层**。如果项目是通过 `-v` 挂载进去的，文件实际还在宿主机上，不属于可写层——提交出来的镜像里 `webapps/ROOT` 会是空的。

### 11.1 完整操作

核心是：**再开一个不挂载的容器，用 `cp` 把文件放进去，然后对它执行 commit**。

```bash
# 1. 起一个干净的临时容器（不挂载），端口用 8082
docker run -d -p 8082:8080 --name tomcat-cp tomcat:8

# 2. 把 dist 拷进去，注意结尾的 /. 表示"拷目录里的内容"
docker cp /home/zyc/dist/. tomcat-cp:/usr/local/tomcat/webapps/ROOT

# 3. 验证一下（浏览器打开也该是购物车）
curl -s http://localhost:8082/ | head -5

# 4. 打包成新镜像
docker commit -m "cart project" -a "zyc" tomcat-cp tomcat-cart:v1

# 5. 临时容器用完就删，把 8082 腾出来
docker rm -f tomcat-cp

# 6. 用新镜像直接跑，注意：没有 -v，也没有 cp
docker run -d -p 8082:8080 --name tomcat-cart tomcat-cart:v1

# 7. 访问 IP:8082，应该直接是购物车
```

第 4 步的三个参数：

| 参数 | 作用 |
| :--- | :--- |
| `-m "说明"` | 提交说明（message） |
| `-a "作者"` | 作者（author） |
| 最后两个 | **源容器** 和 **新镜像名:tag** |

第 6 步就是这一节想说明的：**一条 `docker run` 就把项目带走了，不用再 `cp`。**

```bash
docker images
# 会多出 tomcat-cart  v1  xxxxx  刚刚  4xxMB（比原来的大几MB，项目进去了）
```

### 11.2 commit 的局限（重要）

| 场景 | commit 合适吗 |
| :--- | :--- |
| 项目已经开发完成、不再改动 | ✅ 合适，打包一次到处用 |
| 项目还在调试，今天改完明天还要改 | ❌ 不合适，改一次就得重新 commit 一次 |

所以后面还有更好的办法——**数据卷挂载**（第十三章）。

---

## 十二、镜像的导出与导入（save / load）

> **这一章做什么**：把镜像导出成文件，搬到另一台没网的服务器上。

**场景**：A 服务器有网，已经拉好了 nginx、mysql、tomcat 镜像；B 服务器没网，但也装了 Docker。怎么把镜像弄过去？

**思路**：把 A 上的镜像**导出成一个本地文件**（tar 包），拷贝到 B，再**从文件导入**成镜像。

### 12.1 导出（save）

```bash
docker save <镜像ID或名字> -o <文件名.tar>
```

课上实际执行：

```bash
docker save 0a2249be3d31 -o tomcat8.tar
```

- `-o` 就是 output
- 生成的文件在当前目录下（相对路径）

### 12.2 导入（load）

```bash
docker load -i <文件名.tar>
```

```bash
docker load -i tomcat8.tar
docker images          # tomcat:8 又回来了
```

### 12.3 commit 和 save 别搞混

| 命令 | 方向 | 结果 |
| :--- | :--- | :--- |
| `docker commit` | **容器 → 镜像** | 把跑起来的容器固化成一个新镜像，存在 Docker 里 |
| `docker save` | **镜像 → 本地文件** | 把镜像导出成 `.tar` 文件，可以拷 U 盘、拷到别的服务器 |
| `docker load` | **本地文件 → 镜像** | 把 `.tar` 文件还原成镜像 |

> 有了本地 tar 包做备份，以后就不用每次 `docker pull` 了，从本地 load 更快，也不怕仓库地址失效。

---

## 十三、数据卷挂载（-v）——最好用、也最保命的方式

> **这一章做什么**：让宿主机目录和容器目录"变成同一个"，改文件刷新就生效。
> **实际做项目时最常用的是这个**，前面的 `cp` 和 `commit` 更多是理解原理。

### 13.1 commit 方式的问题

- 项目还在改 → 每次改完都要 commit 一次，太蠢；
- `docker cp` → 每发一次版拷一次，也累。

### 13.2 挂载的思路

**把宿主机的某个目录，和容器里的 `webapps` 目录，直接"绑"在一起。**

```
宿主机 /home/zyc/web  ←——同一个目录——→  容器 /usr/local/tomcat/webapps
```

> 老师演示时用的是 `/root/webapps`（他是 root）。我是 `zyc` 用户，写自己家目录下的 `/home/zyc/web` 就行，效果一样，还不用每次 `sudo`。下面的命令都按后者写。

以后只需要往宿主机的目录里放文件，容器里立刻就有。**改宿主机 = 改容器**。

### 13.3 命令

```bash
docker run -d -p 8080:8080 --name tomcat-01 \
  -v /home/zyc/web:/usr/local/tomcat/webapps \
  tomcat:8
```

- `-v` = volume（数据卷）
- 格式：`-v 宿主机目录:容器目录`，方向别搞反（前宿主、后容器）
- 宿主机的目录内容会**覆盖**容器里同一路径原有的内容

### 13.4 完整操作与验证

**第 1 步：在宿主机准备目录，把网页放进去**

```bash
mkdir -p /home/zyc/web
cp -r /opt/tomcat8/webapps/dict /home/zyc/web/
ls /home/zyc/web
# dict
```

（`dict` 是我自己放在宿主机 Tomcat `webapps` 里的项目；换成任何一个静态网页目录都一样。Vue 这类需要编译的项目看第十六章。）

**第 2 步：删掉旧容器，用挂载方式重新起**

端口和挂载都是 `docker run` 那一刻定死的，改不了，所以必须重建：

```bash
docker rm -f tomcat-01

docker run -d -p 8080:8080 --name tomcat-01 \
  -v /home/zyc/web:/usr/local/tomcat/webapps \
  tomcat:8
```

> 宿主机 8080 如果已经被别的程序占着，把 `-p 8080:8080` 换成 `-p 8081:8080`，访问时端口跟着换。这个问题第十五章细讲。

**第 3 步：浏览器访问**

`http://虚拟机IP:8080/dict/`

这里挂的是整个 `webapps` 目录，所以地址里带 `dict/` 这一层；如果把目录直接挂到 `ROOT`，访问就是 `IP:8080/`。

**第 4 步：在宿主机上改文件**

```bash
cd /home/zyc/web/dict

grep -o '<title>[^<]*</title>' index.html        # 看看现在的标题
sed -i 's#<title>.*</title>#<title>热更新成功</title>#' index.html
grep -o '<title>[^<]*</title>' index.html        # 确认改上了
```

**第 5 步：浏览器直接刷新**

没有重启容器、没有重新拷贝，标题就变了——**这就证明了，两边真的是同一个目录。**

> 我自己的 Vue 购物车挂的是网站根目录：
> ```bash
> docker run -d -p 8081:8080 --name tomcat-01 \
>   -v /home/zyc/dist:/usr/local/tomcat/webapps/ROOT \
>   tomcat:8
> ```
> 访问 `IP:8081/` 就是首页；改完代码重新 `npm run build`，刷新即生效。

### 13.5 挂载方式的好处

- 改代码 / 换版本，只往宿主机目录里丢文件就行，容器不用动；
- 多个容器可以挂载**同一个宿主机目录**，一次修改全部生效；
- 容器删了，宿主机的文件还在，数据不会丢。

> （补充）这种叫"绑定挂载"（bind mount）。Docker 还有另一种"命名卷"（`-v 卷名:容器路径`），由 Docker 管理存储位置，更适合放数据库数据。日常发布项目用绑定挂载最直观。

> ⚠️ **CentOS 上的坑**：SELinux 默认 enforcing，挂载后访问会 403/404。先 `getenforce` 看状态，如果是 `Enforcing`，临时 `setenforce 0`，或者给目录打标签 `chcon -Rt svirt_sandbox_file_t /home/zyc/web`。

---

---

## 十四、Nginx——统一入口、反向代理和负载均衡

> **这一章做什么**：到目前为止，一个项目一个端口：`8080/dict`、`8081/`……项目一多，端口就记不住了。这一章用 Nginx 把这些端口收到一个入口后面，做到"只敲 `IP/dict` 就能进"；再往上一步，用 Nginx 把请求轮流分给三台容器，做**负载均衡**。
> 全章都在我自己的虚拟机上跑，项目用我已经有的 `dict`，端口按我机器的实际情况排（我的 8080 被宿主机 Tomcat 占着，所以容器从 8081 开始）。

### 14.1 Nginx 是干什么的

Nginx 和 Tomcat 一样，可以当**Web 容器**用，但它比 Tomcat 轻得多，也不认识 Java。它的三个典型用途：

| 用途 | 说明 | 这一章会不会做 |
| :--- | :--- | :--- |
| **Web Server**（静态网页服务器） | 纯 html/css/js 的项目，用它比 Tomcat 更合适 | 会（14.2~14.4） |
| **反向代理** | 一个入口，按路径把请求转给后面不同的服务 | 会（14.5~14.9） |
| **负载均衡** | 后面三台跑同一个项目，请求轮流分给它们 | 会（14.10~14.11） |

> （补充）还有缓存、限流、HTTPS 终结这些能力，等真正用到再回来看。日常后端要配 Nginx，八成就是在配这三件事里的某一件。

**先记几个默认端口**，以后见到就知道是谁：

| 服务 | 默认端口 |
| :--- | :--- |
| Nginx | **80** |
| Tomcat | 8080 |
| MySQL | 3306 |
| Redis | 6379 |
| Oracle | 1521 |

> 平时上网不用敲端口，是因为浏览器默认走 80（https 走 443）。真实生产里对外几乎都只开 Nginx 的 80/443，后面的 Tomcat 都在内网，用户永远看不到 8080。

### 14.2 先用最朴素的方式跑起来

```bash
# 1. 拉镜像（我本地没有，老师机器上早就有）
docker pull nginx

# 2. 先确认 80 端口有没有被占
sudo ss -lntp | grep ':80 '
```

如果 `80` 那条**什么都没有**，直接用它；如果被占了（老师机器上就被别的服务占了，只好改用 81），就换个宿主端口，比如 `-p 8090:80`。

我这台机器 80 是空的，所以：

```bash
docker run -d -p 80:80 --name nginx01 nginx
```

```text
# 预期输出：容器 ID
1c2f3a9e5b7d0e4a6c8b2f1d3e5a7c9b0d2f4e6a8c0b1d3f5e7a9c2b4d6f8e0a
```

```bash
docker ps
```

```text
CONTAINER ID   IMAGE   COMMAND                  PORTS                               NAMES
1c2f3a9e5b7d   nginx   "/docker-entrypoint.…"   0.0.0.0:80->80/tcp, :::80->80/tcp   nginx01
```

浏览器打开 `http://虚拟机IP`（不用写端口），看到：

> **Welcome to nginx!**
> If you see this page, the nginx web server is successfully installed and working…

就看到这个页面，说明 **Nginx 已经装好、已经在跑**。

#### `-p 80:80` 的两个 80 是谁

- 前一个 `80`：**宿主机的 80**，外面访问 `IP`（不写端口默认 80）打到这里；
- 后一个 `80`：**容器里的 80**，Nginx 在容器内部监听的就是它。

这两个 80 可以不一样，比如 `-p 8090:80`，外面访问 `IP:8090`，容器里还是 80。**这一点在 14.8 配 `nginx.conf` 时会要命**，先记住。

> ⚠️ 老师在这一步踩过坑：他的 80 被机器上另一个服务占着，`docker run` 起不来，重启一次也没用，最后干脆改成 `-p 81:80`，访问 `IP:81`。

### 14.3 页面藏在容器里的哪个位置

要换成自己的项目，得先知道这页 HTML 在哪。

```bash
docker exec -it nginx01 bash
```

注意：**进 Nginx 容器后落脚点不是 `/`**（和 Tomcat 不一样，Tomcat 一进去就在 `/usr/local/tomcat`），所以直接 `whereis nginx` 找：

```bash
whereis nginx
```

```text
nginx: /usr/sbin/nginx /etc/nginx /usr/share/nginx /usr/share/man/man8/nginx.8.gz
```

两个关键目录：

| 目录 | 放什么 |
| :--- | :--- |
| `/etc/nginx` | **配置文件**，反向代理、负载均衡全在这儿配 |
| `/usr/share/nginx` | 网站文件，网页就在 `html/` 里 |

```bash
cd /usr/share/nginx/html
ls
cat index.html
```

```text
50x.html  index.html
```

`index.html` 的内容就是刚才那个 "Welcome to nginx!" 页面。**把它换成我的页面，就等于发布成功了。**

### 14.4 把项目放进 Nginx：先 `cp`，再挂载

我的项目是宿主机上的 `/home/zyc/web/dict`。

**方式一：`docker cp`（和第十章一样）**

```bash
exit                              # 先退出容器
cp -r /home/zyc/web/dict .        # 确保宿主机当前目录下有 dict
docker cp ./dict nginx01:/usr/share/nginx/html/
```

访问 `http://虚拟机IP/dict/` 就能看到项目首页。

想让它**只敲 IP 就出来**，就把 `dict` 里的 `index.html` 挪到 `html` 根目录（会覆盖 Nginx 的欢迎页）：

```bash
docker exec -it nginx01 bash -c "cp -r /usr/share/nginx/html/dict/* /usr/share/nginx/html/"
```

**方式二：挂载（推荐）**

```bash
docker rm -f nginx01

docker run -d -p 80:80 --name nginx01 \
  -v /home/zyc/web/dict:/usr/share/nginx/html \
  nginx
```

`/usr/share/nginx/html` 整个换成宿主机目录，**以后改页面不用再 cp、不用重启**，刷新就是新的。这就是第十三章讲的挂载，换成 Nginx 一样好用。

#### 所以 Tomcat 和 Nginx 该怎么选

| 场景 | 用谁 |
| :--- | :--- |
| 后端是 Java（Spring、JSP、Servlet） | **Tomcat**（Nginx 跑不了 Java） |
| 后端是 Python/Node/Go，或者纯静态页面 | 后端自己跑，前面用 **Nginx** 当入口 |
| 纯静态页面（html/css/js），就一个 | 两个都行，**Nginx 更轻** |

### 14.5 反向代理：一个入口，多个项目

先看问题。假设我机器上有三个项目：

```text
http://虚拟机IP:8081/dict/     词典
http://虚拟机IP:8082/jd/       京东
http://虚拟机IP:8083/tb/       淘宝
```

用户得记三个端口，而且端口一改（比如跟别的服务撞了）他就打不开了。

**反向代理**就是：对外只暴露 Nginx 一个端口，Nginx 看路径决定给谁：

```text
http://虚拟机IP/dict/   →  Nginx  →  转给 8081 那台
http://虚拟机IP/jd/     →  Nginx  →  转给 8082 那台
http://虚拟机IP/tb/     →  Nginx  →  转给 8083 那台
```

用户眼里只有一个服务器、一个端口，后面的结构他完全不知道。

> **"反向"是相对谁而言**：正向代理（比如翻墙工具）替**客户端**去访问外面；反向代理替**服务端**接待请求。方向相反，所以叫反向。

### 14.6 前置条件：让容器之间能互相找到

这里有个坑必须提前解决：**容器之间默认是互相隔离的**，Nginx 要转发给 Tomcat，首先得能"找到"那台 Tomcat。

Docker 的解法是**自定义网络**：把几个容器放进同一个网络，它们就能用**容器名**互相访问。

```bash
# 1. 看现有的网络（默认会有 bridge / host / none 三个）
docker network ls

# 2. 建一个自己的
docker network create tomcat_net

# 3. 看它里面有什么（现在是空的）
docker network inspect tomcat_net
```

```text
# 3 的输出里有这么一段，现在 Containers 是空的：
"Containers": {},
```

以后启动容器时加 `--network tomcat_net`，容器就进这个网络了。**同一个网络里，可以直接用容器名当主机名**，比如后面 `proxy_pass http://tomcat01:8080`。

### 14.7 搭一个"三台服务器 + 一个入口"的局

我的端口规划（和我机器的实际情况对齐）：

| 谁 | 宿主机端口 | 容器内端口 | 说明 |
| :--- | :--- | :--- | :--- |
| 宿主机上的 Tomcat | 8080 | — | 之前手装的那个，不动它 |
| 容器 tomcat01 | 8081 | 8080 | |
| 容器 tomcat02 | 8082 | 8080 | |
| 容器 tomcat03 | 8083 | 8080 | |
| **Nginx** | **80** | 80 | 统一入口 |

**第一步：清掉旧容器**（项目是挂载的，删容器不会丢文件）

```bash
docker rm -f tomcat-01
```

**第二步：起三台 Tomcat，都加入同一个网络**

```bash
docker run -d -p 8081:8080 --name tomcat01 \
  --network tomcat_net --restart=always \
  -v /home/zyc/web:/usr/local/tomcat/webapps \
  tomcat:8

docker run -d -p 8082:8080 --name tomcat02 \
  --network tomcat_net --restart=always \
  -v /home/zyc/web:/usr/local/tomcat/webapps \
  tomcat:8

docker run -d -p 8083:8080 --name tomcat03 \
  --network tomcat_net --restart=always \
  -v /home/zyc/web:/usr/local/tomcat/webapps \
  tomcat:8
```

三台挂的是**同一个宿主机目录** `/home/zyc/web`（里面就一个 `dict`），所以三台项目一模一样 —— 这正是"集群"的形态。

**第三步：确认三台都在网络里**

```bash
docker network inspect tomcat_net
```

```text
"Containers": {
    "…": { "Name": "tomcat01", … },
    "…": { "Name": "tomcat02", … },
    "…": { "Name": "tomcat03", … }
}
```

**第四步：起 Nginx，也加入这个网络**

先把配置文件从容器里拷出来（容器里没 `vi`，必须在宿主机改好再挂进去）：

```bash
# 用一次性容器把默认配置拷出来
docker cp nginx01:/etc/nginx/nginx.conf ./nginx.conf   2>/dev/null || \
docker run --rm nginx cat /etc/nginx/nginx.conf > nginx.conf

mkdir -p /home/zyc/nginx
mv nginx.conf /home/zyc/nginx/
```

然后带着挂载启动：

```bash
docker rm -f nginx01

docker run -d -p 80:80 --name nginx01 \
  --network tomcat_net --restart=always \
  -v /home/zyc/nginx/nginx.conf:/etc/nginx/nginx.conf \
  nginx
```

> 这里**只挂 `nginx.conf` 一个文件**。想连网页一起挂，再加一条 `-v /home/zyc/web/dict:/usr/share/nginx/html`。

**第五步：四台都在跑**

```bash
docker ps
```

```text
CONTAINER ID   IMAGE     PORTS                     NAMES
…              nginx     0.0.0.0:80->80/tcp        nginx01
…              tomcat:8  0.0.0.0:8081->8080/tcp    tomcat01
…              tomcat:8  0.0.0.0:8082->8080/tcp    tomcat02
…              tomcat:8  0.0.0.0:8083->8080/tcp    tomcat03
```

先单独验证三个都能打开：`IP:8081/dict/`、`IP:8082/dict/`、`IP:8083/dict/`，**都是同一个页面就是对的**。

### 14.8 配置 `nginx.conf`：把路径转发出去

打开 `/home/zyc/nginx/nginx.conf`，找到 `http { ... }` 块，在里面加一个 `server`：

```nginx
http {
    # …… 中间原来那一堆 include、log_format 都不用动 ……

    server {
        listen       80;
        server_name  _;

        location /dict {
            proxy_pass http://tomcat01:8080;
        }

        location /jd {
            proxy_pass http://tomcat02:8080;
        }

        location /tb {
            proxy_pass http://tomcat03:8080;
        }
    }
}
```

逐行解释：

| 写法 | 意思 |
| :--- | :--- |
| `listen 80` | Nginx **自己在容器里**监听 80 |
| `location /dict` | 用户访问的路径以 `/dict` 开头 |
| `proxy_pass http://tomcat01:8080` | 转给 `tomcat01` 这个容器的 8080 |

**为什么能写 `tomcat01` 这个名字？** 因为 Nginx 容器和 tomcat01 在同一个 `tomcat_net` 网络里，Docker 内置了 DNS，容器名就能直接解析成 IP。没加 `--network` 的话这里会报 `host not found in upstream`。

> ⚠️ **老师踩的坑（重点，14.12 细讲）**：`listen` 写的是**容器里的端口**。老师的外部端口改成了 81，就把 `listen` 也写成了 81 —— 这是错的，正因为这个，他的反向代理一直 404。

### 14.9 重启生效，然后验证

配置改完必须让 Nginx 重新加载：

```bash
docker restart nginx01
```

```bash
docker ps                                  # 四个都是 Up
docker exec nginx01 nginx -t               # 测配置语法
```

```text
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

然后浏览器：

```text
http://虚拟机IP/dict/     →  8081 那台的项目
http://虚拟机IP/jd/       →  8082 那台
http://虚拟机IP/tb/       →  8083 那台
```

**一个入口、三个项目**，反向代理就跑通了。

> （补充）如果报 `502 Bad Gateway`，九成是 `proxy_pass` 里的主机名/端口写错了，或者容器不在同一个网络；如果报 `404`，是路径没对上（`proxy_pass` 后面带不带斜杠会影响路径拼接方式，见下一节的说明）。

#### `proxy_pass` 后面带不带斜杠的区别

```nginx
location /dict {
    proxy_pass http://tomcat01:8080;      # 转发时保留 /dict
}
location /jd {
    proxy_pass http://tomcat02:8080/;     # 转发时把 /jd 去掉
}
```
- 不带斜杠：`/jd/a.html` → 后端收到 `/jd/a.html`
- 带斜杠：`/jd/a.html` → 后端收到 `/a.html`

我的 `dict` 是个目录项目，访问 `/dict/` 要能对上 `webapps/dict/`，所以**不带斜杠**。

### 14.10 负载均衡：三台跑同一个项目

反向代理解决的是"**不同**项目走同一个入口"，负载均衡解决的是"**同一个**项目扛不住，分给多台"。

> 注意区分：三台跑小米、京东、淘宝，那是反向代理；三台都跑小米，才叫**集群**，才谈得上负载均衡。

现在我的三台 Tomcat 已经挂着同一个 `dict` 了，就差让 Nginx 轮流分。

在 `http {}` 里，**在 `server` 之前**声明一个 `upstream`：

```nginx
http {
    upstream tomcat_cluster {
        server tomcat01:8080 weight=1;
        server tomcat02:8080 weight=1;
        server tomcat03:8080 weight=1;
    }

    server {
        listen       80;
        server_name  _;

        location / {
            proxy_pass http://tomcat_cluster;
        }
    }
}
```

| 写法 | 意思 |
| :--- | :--- |
| `upstream 名字 { ... }` | 声明一个集群，名字随便起，下面 `proxy_pass` 引用它 |
| `server IP:端口 weight=N` | 集群里的一台，`weight` 是权重 |
| `proxy_pass http://集群名` | 把请求交给这个集群，而不是某一台 |

**权重都是 1 的时候，就是轮询**：第一个请求给 01，第二个给 02，第三个给 03，第四个又回 01。

#### 怎么"看"出请求到底给了哪台

三台内容一模一样，光看页面分不出来。所以要**做个小标记**。

我懒得改三份文件，直接给三台的 `dict/index.html` 加一行字（挂载的好处，改宿主机就三台同时变），但那样三台还是一样……所以换个办法：**分别改容器里的**：

```bash
docker exec tomcat01 sh -c 'echo "<p>我是 tomcat01 (8081)</p>" >> /usr/local/tomcat/webapps/ROOT/index.html'
```

> 更简单的做法：三台**各自挂不同目录**，里面放内容略有差异的同一份项目。我这里图快，直接进容器改（注意这是临时改，容器删了就没了）。

（补充）其实最省事的验证方式是不改页面，而是看 Nginx 的访问日志：

```bash
docker logs -f nginx01
```
每刷新一次，日志里就多一行，配合 `upstream` 的 `access_log` 能看到转给了哪台。

### 14.11 权重与 ip_hash：两种分发策略

#### 权重（weight）：按能力分活

```nginx
upstream tomcat_cluster {
    server tomcat01:8080 weight=2;    # 干两份
    server tomcat02:8080 weight=1;
    server tomcat03:8080 weight=1;
}
```

四次请求里，大约两次给 01、一次给 02、一次给 03。

**什么时候用**：三台机器配置不一样。性能强的权重大，不然轮询会让强的那台闲着、弱的那台扛不住。

#### ip_hash：同一个用户锁死一台

权重策略有个**致命问题**：带登录的项目会乱。

用户第一次访问到了 tomcat01，在那里登录成功，Session 存在 tomcat01 里；下一次请求被轮到 tomcat02，tomcat02 上没有他的登录信息，直接把人踢回登录页 —— 用户会觉得"我明明刚登过"。

解法是**让同一个用户永远落在同一台**：

```nginx
upstream tomcat_cluster {
    ip_hash;
    server tomcat01:8080;
    server tomcat02:8080;
    server tomcat03:8080;
}
```

`ip_hash` 按客户端 IP 算哈希，IP 不变，算出来的结果就不变，永远挑同一台。个人用户 IP 基本固定，问题就解决了。

> 注意 `ip_hash` 和 `weight` 一般不同时用。还有第三种 `least_conn`（连接数少的多分点），单机演示看不出效果，知道有这回事就行。

| 策略 | 适合 | 不适合 |
| :--- | :--- | :--- |
| 轮询（`weight` 全 1） | 三台性能相当、无状态接口 | 有登录 Session 的项目 |
| `weight` | 三台性能差异大 | 有登录 Session 的项目 |
| `ip_hash` | **有登录的普通项目** | 用户都在同一个 NAT 后面（会全落到一台） |

> （补充）生产上更常见的做法是**把 Session 外置**（存 Redis），这样三台就无状态了，什么策略都能用。这是后面学 Redis 时的正经解法，`ip_hash` 只是最省事的一种。

### 14.12 ⚠️ 老师卡住的地方：`listen` 到底写 80 还是 81

这一节单独拎出来，因为老师在这上面耗了大半节课，反向代理一直 404，最后是靠做负载均衡时才偶然发现的。

**经过**（按实际时间顺序）：

1. 老师机器 80 被占，Nginx 起在 `-p 81:80`，访问 `IP:81` 正常看到欢迎页；
2. 配反向代理时，`server { listen 81; ... }` —— 这里写错了；
3. 结果 `IP:81/dict` 能开（因为 nginx 没挂？其实是首页兜底），`IP:81/jd`、`IP:81/tb` 全 404；
4. 中间试过关防火墙（`systemctl stop firewalld`）、`reboot` 重启机器，都没用；
5. 一节课上完还没解决，**老师先跳过去演示负载均衡**；
6. 演示负载均衡时同样的 404 又出现，老师这才把 `listen 81` 改成 `listen 80`，一改就通了；
7. 回头确认：**反向代理的失败也是同一个原因**。

**为什么是 80**：

```text
用户浏览器
   │  访问 IP:81
   ▼
宿主机 81 端口            ← -p 81:80 里的第一个 81（Linux 防火墙这一层）
   │  端口映射进来
   ▼
容器内的 80 端口          ← nginx.conf 的 listen 必须写这个
   │
   ▼
Nginx 进程
```

`listen` 是**Nginx 在容器内部**监听谁，Nginx 镜像里它天然就是 80。宿主机的 81 是 Docker 做的转发，Nginx 自己不知道也不关心。

**一句话记法**：

> `-p 宿主:容器` —— `listen` 永远写**冒号后面那个**（容器端口）。

同理，之前 `-p 8081:8080` 的 Tomcat，访问 8081 打到的其实是容器里的 8080；Nginx 转发时也必须是 `proxy_pass http://tomcat01:8080`，**不能写 8081**。这两个坑是同一个坑。

#### 老师另外两处没跑完的地方

- **反向代理没当场验证成功**：他改成 `listen 80` 之后先验证的是负载均衡，反向代理只是口头说"应该也是这个问题，一会儿再测"，并没有再完整演示一遍。所以反向代理的正确流程（14.5~14.9）是我自己按原理补的，配上负载均衡通了的现象互为印证。
- **80 端口的占用者**：老师提到是他机器上另一个服务占着（后面会讲到的运维面板），他自己也没深究。我这边 80 是空的，所以直接用了 80。

#### 本章命令回顾

```bash
# 镜像 / 容器
docker pull nginx
docker run -d -p 80:80 --name nginx01 nginx
docker exec -it nginx01 bash
whereis nginx

# 网络（反向代理 / 负载均衡的前提）
docker network ls
docker network create tomcat_net
docker network inspect tomcat_net

# 带网络启动
docker run -d -p 8081:8080 --name tomcat01 \
  --network tomcat_net --restart=always \
  -v /home/zyc/web:/usr/local/tomcat/webapps tomcat:8

# 挂配置文件
docker run -d -p 80:80 --name nginx01 \
  --network tomcat_net --restart=always \
  -v /home/zyc/nginx/nginx.conf:/etc/nginx/nginx.conf nginx

# 生效 & 检查
docker restart nginx01
docker exec nginx01 nginx -t
docker logs -f nginx01
```

---

## 十五、进阶：容器到底是什么，以及我踩过的坑

> ⚠️ **这一章是进阶，可以选择跳过**：只想先把流程跑通的，直接翻到第十八章的「完整流程速查」。
> 这里回答三个问题：为什么 `ps` 能看到容器里的进程、为什么一个端口只能有一个服务、以及**我机器上已经装过 Tomcat，怎么让两个都活着**。

### 15.1 跑起来之后：这个 Tomcat 到底是哪个 Tomcat

容器起来之后我顺手 `ps -ef | grep [t]omcat`，扫出来这么一条：

```
root  25079  25057  0 09:50 ?  00:00:02 /opt/java/openjdk/bin/java
  -Djava.util.logging.config.file=/usr/local/tomcat/conf/logging.properties
  -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager
  -Dcatalina.base=/usr/local/tomcat
  -Dcatalina.home=/usr/local/tomcat
  ... org.apache.catalina.startup.Bootstrap start
```

我当时愣了一下：**我 `/opt` 下装的明明是 `tomcat8`，怎么跑起来的是 `/usr/local/tomcat`？**

三个证据摆出来就清楚了：

| 线索 | 我装在 `/opt` 的 | 截图里这个 |
| :--- | :--- | :--- |
| Tomcat 路径 | `/opt/tomcat8` | `/usr/local/tomcat` |
| JDK 路径 | `/opt/jdk8` | `/opt/java/openjdk` |
| 运行用户 | `zyc` | `root` |

`/usr/local/tomcat` 和 `/opt/java/openjdk` 是 **`tomcat:8` 镜像内部的路径**，是官方 Dockerfile 里 `ENV CATALINA_HOME=/usr/local/tomcat` 规定的。

而我在宿主机上试：

```bash
ls /usr/local/tomcat
# ls: cannot access /usr/local/tomcat: No such file or directory
```

**进程是真的，路径是假的。** 这就是容器的本质。

### 15.2 为什么宿主机的 `ps` 能看到容器里的进程

因为**容器不是虚拟机**。

| | 虚拟机 | 容器 |
| :--- | :--- | :--- |
| 内核 | 自己独立的一套 | 共用宿主机的内核 |
| 启动过程 | 完整的开机流程（BIOS→内核→systemd） | 就是 `fork` 一个进程 |
| 隔离方式 | 硬件虚拟化 | Linux namespace |
| 宿主机 `ps` 能看到里面吗 | 看不到 | **看得到** |

容器只是宿主机上的一组普通进程，套了几层"命名空间"的视图限制：

- **文件系统（mount namespace）**：容器里看是 `/usr/local/tomcat`，宿主机上实际是 `/var/lib/docker/overlay2/xxx/diff/usr/local/tomcat`
- **网络（net namespace）**：容器里以为自己有独立的 `172.17.0.2` 网卡
- **进程（pid namespace）**：容器里只看得到自己那几个进程，**反过来宿主机不受这个限制，全部可见**

所以宿主机 `ps -ef` 一路扫下来，容器里的 java 进程也露出来了，但那条路径只在容器自己的逻辑里成立。

想把视角切到容器内再看：

```bash
docker top tomcat-01
```

这条命令只列容器内部视角的进程，就干净了。

### 15.3 一个端口只能有一个主人

`ss -lntp | grep 8080` 最开始输出是这样的：

```
LISTEN 0 128 *:8080 *:*
LISTEN 0 128 [::]:8080 [::]:*
```

后面本该有的 `users:(("xxx",pid=...))` 是空的。因为**占用 8080 的进程是 root 起的，我 `zyc` 号看不到别人的进程信息**。加 `sudo`：

```bash
sudo ss -lntp | grep 8080
# LISTEN 0 128 *:8080 *:*  users:(("docker-proxy",pid=41301,fd=4))
```

真凶是 `docker-proxy`——Docker 替容器占着这个端口。

那我宿主机上装的 `/opt/tomcat8` 现在是什么状态？它就算执行 `startup.sh` 也起不来：

```
SEVERE: Failed to initialize end point associated with ProtocolHandler ["http-bio-8080"]
java.net.BindException: Address already in use
```

**一个端口同一时间只能被一个进程绑。** Docker 先占了，本机的 Tomcat 就靠边站。

> 课上老师撞的是同一个现象的另一面：他 docker 里有 3 个 tomcat 容器占着 8080，本机 Tomcat 装好、`startup.sh` 也跑了，但浏览器访问到的却是容器的 404（脚注 `Apache Tomcat/8.5.100`）。后来 `docker stop` 掉那 3 个容器，本机 Tomcat 才接管 8080，页面才出来。

### 15.4 想两个都要：8081 给容器，8080 给宿主机

**关键规则：端口映射在 `docker run` 那一刻就定死了，之后改不了。** 没有 `docker set-port` 这种命令，`docker update` 也改不了 `-p`。

想换端口，只能**删掉容器重建**。

但删之前有个坑：`docker cp` 进去的项目存在于**容器的可写层**里，`docker rm` 一执行就跟着没了。所以先确认宿主机上还留着产物：

```bash
ls /home/zyc/dist
# index.html  css  js  favicon.ico
```

然后重建，顺手改成**挂载**方式，以后重建容器项目也不会丢：

```bash
docker rm -f tomcat-01

docker run -d -p 8081:8080 --name tomcat-01 \
  -v /home/zyc/dist:/usr/local/tomcat/webapps/ROOT \
  tomcat:8
```

`-p 8081:8080` = 宿主机 8081 → 容器 8080。
`-v` 之后，容器里的 `webapps/ROOT` 就是宿主机的 `/home/zyc/dist`，**改宿主机上的文件，刷新浏览器就生效**。

> 如果访问 8081 出来 **403 Forbidden**，是 CentOS 的 SELinux 拦的（它不允许容器访问宿主机上的普通目录）：
> ```bash
> sudo setenforce 0        # 临时关闭，重启后恢复
> ```
> 永久关闭要改 `/etc/selinux/config` 里的 `SELINUX=disabled`，然后重启系统。

最后验证两个端口各归其主：

```bash
sudo ss -lntp | grep -E ':8080|:8081'
```

期望看到：

```
LISTEN 0 128  *:8081     users:(("docker-proxy",pid=41301))   ← 容器
LISTEN 0 100 [::]:8080   users:(("java",pid=41756))           ← 宿主机 Tomcat
```

`8081` 归 `docker-proxy`、`8080` 归 `java`，分家成功。

顺带把宿主机的 Tomcat 也拉起来（记得先 `source /etc/profile`，不然 `JAVA_HOME` 没有）：

```bash
source /etc/profile
/opt/tomcat8/bin/startup.sh
tail -f /opt/tomcat8/logs/catalina.out     # 看到 Server startup in xxx ms 才算真起来
```

### 15.5 `--restart` 是少数能"后改"的配置

我想给容器加开机自启，又跑了一次 `docker run ... --restart=always`，结果报：

```
docker: Error response from daemon: Conflict. The container name "/tomcat-01"
is already in use by container "885c5b2b3b53...". You have to remove (or rename)
that container to be able to reuse that name.
```

占着这个名字的 `885c5b2b3b53`，就是**正在运行的那个容器**——它已经建好了，不需要再建。

而且 `--restart` 根本不用重建：

```bash
docker update --restart=always tomcat-01
```

验证：

```bash
docker inspect -f '{{.HostConfig.RestartPolicy.Name}}' tomcat-01
# always
```

这条**立即生效，容器不用停不用删**。`docker update` 能改的是资源限制（CPU/内存）和重启策略，改不了端口映射和挂载。

### 15.6 那到底该留哪个

| | 宿主机手工装 | Docker |
| :--- | :--- | :--- |
| 装一次 | 传包 → 解压 → 挪 `/opt` → 改 `profile` → `source` → 启动，十几分钟 | `docker pull` + `docker run`，两三分钟 |
| 换台机器 | 全部重来 | 一条命令 |
| 装两个版本 | 改端口、改配置、环境变量打架 | 跑两个容器，各占一个端口 |
| 弄坏了 | 手动清理目录、环境变量残留 | `docker rm` 干净利落，重建一分钟 |
| 删干净 | 一大堆残留 | `docker rmi` 就没了 |
| 开机自启 | 得自己写 systemd 服务 | `--restart=always` 一行搞定 |

课上先用宿主机装，是为了让我知道**里面到底装了什么、为什么 Web 容器需要 JDK、环境变量为什么必须配**——这些不懂，用 Docker 就是纯黑盒。

但**实际干活用 Docker**。

所以 `/opt` 里那两个（`jdk8`、`tomcat8`）现在删不删都行：不删只是占 300MB 磁盘，不冲突；想删就 `rm -rf /opt/tomcat8 /opt/jdk8`，再把 `/etc/profile` 里那两行 `export` 注释掉。

---

## 十六、进阶：前端项目（Vue）怎么放进容器

> ⚠️ **这一章是专项，只跟前端项目有关**：只讲 Vue / React 这类**需要编译**的项目怎么放进容器。
> 如果你的项目就是普通网页（html + css + js），第十章的做法已经够用了，可以跳过。

### 16.1 拷之前先搞清楚：Vue 项目要先 build

前面拷的 `dict` 是现成的静态网页（已经是 html + 图片这类成品），拷进去就能用。

但我自己的购物车项目（`buy-list`，vue 2.6.14 + vuex + axios + echarts，没用 vue-router）不一样——它是**源码**。

我第一次想直接把 `/home/zyc/buy-list` 整个拷进去。这么干，浏览器打开看到的是 `.vue` 文件的内容文本，不是网页。

因为 Tomcat 本质是"静态文件服务器 + JSP 容器"，它只认 `.html`、`.css`、`.js`。`.vue` 是给 `vue-cli-service build` 编译的，浏览器不认识这个后缀。

**类比一下就清楚了：**

| Java 那边 | Vue 这边 |
| :--- | :--- |
| `.java` 源码 | `.vue` 源码 |
| `javac` 编译 | `npm run build` |
| `.class` 字节码 | `dist/` 里的 html + js + css |
| 丢进 `webapps` 能跑 | 丢进 `webapps` 能跑 |

`npm run build` 就是前端世界的 `javac`。

所以那句话的准确说法是：**不是 Vue 项目跑不了，是源码跑不了；编译产物跑得很好，跟老师的 `mi` 网页没有任何区别。**

### 16.2 一个必踩的坑：publicPath

我的 `vue.config.js` 里什么都没配：

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true
})
```

**没写 `publicPath`，默认值是 `/`。**

这导致 build 出来的 `index.html` 里，所有资源都是 `/` 开头的**绝对路径**：

```html
<link rel="icon" href="/favicon.ico">
<script src="/js/chunk-vendors.22b5fde0.js"></script>
<script src="/js/app.9005c110.js"></script>
<link href="/css/app.87f8783a.css" rel="stylesheet">
```

后果：

| 放到哪 | 访问地址 | 结果 |
| :--- | :--- | :--- |
| `webapps/ROOT/` | `IP:8081/` | ✅ 正常，`/js/...` 正好对得上 |
| `webapps/buy-list/` | `IP:8081/buy-list/` | ❌ **白屏**，浏览器去 `IP:8081/js/...` 找文件，404 |

要用第二种路径，就得改配置重新 build：

```js
module.exports = defineConfig({
  transpileDependencies: true,
  publicPath: '/buy-list/'      // 或者写 './' 表示相对路径
})
```

**省事的办法是直接放 `ROOT`**，代码一个字都不用改。

### 16.3 实战：build → 传 → cp → 访问

**第一步：在哪儿 build**

我虚拟机里没装 node：

```
$ npm run build
bash: npm: 未找到命令...
```

但这**正说明了一个分工**：

```
开发机（Windows / 我的电脑）      服务器（虚拟机）
  写代码                            只放文件
  npm run build        →            dist/
```

服务器是拿来**跑**的，不是拿来**编译**的。公司里也一样：前端在自己电脑上 build，产物丢服务器；或者 CI 机器 build 完传到生产机。生产机上一个 node 都没有，很正常。

所以回到 Windows，在项目目录里：

```bash
cd D:\桌面\BDQN学习\前端\作业\buy-list\buy-list
npm run build
```

7.5 秒出结果：

```
File                                 Size          Gzipped
dist\js\chunk-vendors.22b5fde0.js    118.36 KiB    40.31 KiB
dist\js\app.9005c110.js              6.71 KiB      2.67 KiB
dist\css\app.87f8783a.css            3.30 KiB      1.00 KiB

DONE  Build complete. The dist directory is ready to be deployed.
```

`dist` 里一共 7 个文件、0.82MB：

```
dist\index.html
dist\favicon.ico
dist\css\app.87f8783a.css
dist\js\app.9005c110.js
dist\js\chunk-vendors.22b5fde0.js
dist\js\*.map            ← 调试用的，生产环境可以删掉
```

**第二步：传上去**

打成 zip（242KB）更好传，单个文件在 FinalShell 里拖一下就到 `/home/zyc/` 了：

```bash
cd /home/zyc
unzip -o dist.zip
ls dist
```

（没装 unzip 就 `yum install -y unzip`）

**第三步：拷进容器**

```bash
docker cp /home/zyc/dist tomcat-01:/usr/local/tomcat/webapps/ROOT
docker restart tomcat-01
```

`dist` 后面**不加斜杠**、目标是 `ROOT`——镜像里没有这个目录，`docker cp` 会自动创建，正好把 dist 里的内容整个倒进去。

**第四步：访问**

```
http://虚拟机IP:8080/
```

购物车页面就出来了。

### 16.4 踩坑：pnpm 装的 node_modules 是假的

第一次 build 直接失败：

```
Error: Cannot find module
'D:\桌面\BDQN学习\前端\作业\buy-list\buy-list\node_modules\@vue\cli-service\bin\vue-cli-service.js'
```

去翻 `node_modules\@vue\cli-service`，发现它**不是目录，是个 Junction（符号链接）**：

```
node_modules\@vue\cli-service
   → D:\桌面\BDQN学习\前端\作业\钟永超11.21周测\vuedemo1\node_modules\.pnpm\@vue+cli-service@5.0.9_...\node_modules\@vue\cli-service
```

指向的那个 `钟永超11.21周测\vuedemo1` **已经被我删掉了**，所以链接全断——`@vue/cli-service`、`@vue/cli-plugin-babel`、`@vue/cli-plugin-eslint` 三个包一个都读不到。

这是 pnpm 的老毛病：为了省磁盘，包本体只存一份，各个项目的 `node_modules` 里只放链接。**原目录一删，所有链接全废。**

修的办法就是重装一遍（走国内源快得多）：

```bash
npm install --registry=https://registry.npmmirror.com
# added 26 packages, and changed 47 packages in 16s
```

装完再 `npm run build` 就成功了。

**教训**：一个项目别混用包管理器。我那个目录里 `package-lock.json`（npm 的）和 `pnpm-lock.yaml`（pnpm 的）两个都在——两种锁文件打架，出这种事只是迟早的。选定一个，把另一个删掉。

### 16.5 整条链路串起来

```
写代码（.vue / .js / .css）
      ↓  npm run build（在开发机上）
静态文件（dist：index.html + js/ + css/）
      ↓  docker cp  或者  -v 挂载
容器里的 /usr/local/tomcat/webapps/ROOT
      ↓
浏览器访问 http://虚拟机IP:8081/
```

课上老师打包的是"米"网页，我打包的是购物车——**流程一模一样**。

这就是第十一章 `docker commit` 的用途：把"已经放好项目的容器"直接固化成镜像，换台机器 `docker run` 就起来了，连 `docker cp` 都省了。

---

## 十七、命令总表

### 镜像相关

| 命令 | 作用 |
| :--- | :--- |
| `docker images` | 查看本地镜像 |
| `docker pull 镜像名:tag` | 从仓库拉取镜像 |
| `docker rmi 镜像ID/名` | 删除镜像（有容器在用则删不掉） |
| `docker save 镜像 -o xx.tar` | 把镜像导出成本地文件 |
| `docker load -i xx.tar` | 从本地文件导入镜像 |

### 容器相关

| 命令                                       | 作用                                           |
| :--------------------------------------- | :------------------------------------------- |
| `docker run ...`                         | 用镜像启动容器（核心命令）                                |
| `docker ps`                              | 查看运行中的容器                                     |
| `docker ps -a`                           | 查看所有容器                                       |
| `docker ps -q` / `-aq`                   | 只输出 ID                                       |
| `docker stop <ID>`                       | 停止                                           |
| `docker start <ID>`                      | 启动                                           |
| `docker restart <ID>`                    | 重启                                           |
| `docker rm <ID>`                         | 删除容器                                         |
| `docker logs <ID>`                       | 看容器输出（排错第一招）                                 |
| `docker exec -it <ID> bash`              | 进入容器内部                                       |
| `docker cp 源 容器ID:目标`                    | 宿主机 ↔ 容器传文件                                  |
| `docker commit -m "..." -a "..." 容器 新镜像` | 把容器打包成镜像                                     |
| `docker top <ID>`                        | 看容器内部的进程（容器视角，不受宿主机 pid namespace 干扰）        |
| `docker inspect <ID>`                    | 查看容器全部详情（挂载、端口、重启策略、IP）                      |
| `docker update --restart=always <ID>`    | 改重启策略，设置需要开机自启的容器，**不用重建容器**，设置 restart 为 no 则关闭 |

### 网络相关（反向代理 / 负载均衡的前提）

| 命令 | 作用 |
| :--- | :--- |
| `docker network ls` | 查看所有网络（默认有 bridge / host / none） |
| `docker network create 名字` | 创建自定义网络 |
| `docker network inspect 名字` | 查看网络详情，**`Containers` 里就是加入了这个网络的容器** |
| `docker network rm 名字` | 删除网络 |

> 同一个自定义网络里的容器，**可以直接用容器名互相访问**（Docker 内置 DNS）。`proxy_pass http://tomcat01:8080` 能生效就是因为这个；没加 `--network` 就会报 `host not found in upstream`。

### `docker run` 常用参数

| 参数 | 作用 |
| :--- | :--- |
| `-d` | 后台运行 |
| `-p 宿主端口:容器端口` | 端口映射 |
| `--name 名字` | 指定容器名 |
| `-v 宿主目录:容器目录` | 数据卷挂载 |
| `--rm` | 容器停止后自动删除 |
| `--restart=always` | 开机 / 异常退出自动重启（建好后也能用 `docker update` 改） |
| `--network 网络名` | 加入自定义网络，容器之间可以用容器名互访（第十四章） |

---

## 十八、完整流程速查

```bash
# 0. 装好 Docker + 配镜像加速器

# 1. 拉镜像（老师机器上已有，我的没有，必须做）
docker pull tomcat:8

# 2. 准备项目目录并放项目
mkdir -p /home/zyc/web
cp -r /opt/tomcat8/webapps/dict /home/zyc/web/

# 3. 启动容器（挂载方式）
docker run -d -p 8080:8080 --name tomcat-01 \
  -v /home/zyc/web:/usr/local/tomcat/webapps \
  tomcat:8

# 4. 检查
docker ps

# 5. 浏览器访问
#    http://虚拟机IP:8080/dict/

# 6. 以后改项目：直接改 /home/zyc/web 里的文件，刷新页面即可
```

**做集群（跑第二个容器）：**

```bash
docker run -d -p 8081:8080 --name tomcat-02 \
  -v /home/zyc/web:/usr/local/tomcat/webapps \
  tomcat:8
```

访问 `IP:8080` 和 `IP:8081` 拿到的是同一个项目——这就相当于一个最简单的集群。**容器内部都是 8080，靠宿主机端口错开来区分**，这就是容器隔离最直观的体现。

**我自己的实践：Vue 购物车 + 端口分家**

（因为宿主机 `/opt` 里还装着一个 Tomcat，它要 8080，所以容器让到 8081）

```bash
# 1. 开发机上 build（Windows）
cd D:\桌面\BDQN学习\前端\作业\buy-list\buy-list
npm run build                      # 产物在 dist\

# 2. 传进虚拟机并解压
cd /home/zyc
unzip -o dist.zip                  # 得到 /home/zyc/dist

# 3. 容器起在 8081，把 dist 挂载成 ROOT
docker run -d -p 8081:8080 --name tomcat-01 \
  -v /home/zyc/dist:/usr/local/tomcat/webapps/ROOT \
  tomcat:8

# 4. 访问
#    http://虚拟机IP:8081/
```

以后改了代码，重新 `npm run build`，刷新浏览器就是新版——挂载是实时的，不用再 `docker cp`、也不用重启容器。

三种把项目放进容器的方式对比：

| 方式 | 命令 | 改代码之后怎么办 | 容器删了项目还在吗 |
| :--- | :--- | :--- | :--- |
| 进容器里改 | `docker exec -it ... bash` + `vi` | 镜像里没装 vi，不现实 | 没了 |
| 拷贝 | `docker cp` | 每次都要重新 cp + restart | 没了 |
| **挂载** | `-v 宿主机目录:容器目录` | `npm run build` 完刷新就行 | **在** |

**Nginx 统一入口 + 负载均衡（第十四章）**

```bash
# 1. 建一个网络，让容器之间能用容器名互访
docker network create tomcat_net

# 2. 三台 Tomcat 都挂同一个项目、进同一个网络
docker run -d -p 8081:8080 --name tomcat01 --network tomcat_net --restart=always \
  -v /home/zyc/web:/usr/local/tomcat/webapps tomcat:8
docker run -d -p 8082:8080 --name tomcat02 --network tomcat_net --restart=always \
  -v /home/zyc/web:/usr/local/tomcat/webapps tomcat:8
docker run -d -p 8083:8080 --name tomcat03 --network tomcat_net --restart=always \
  -v /home/zyc/web:/usr/local/tomcat/webapps tomcat:8

# 3. 把 Nginx 的默认配置拷到宿主机（容器里没 vi，必须在宿主机改）
mkdir -p /home/zyc/nginx
docker run --rm nginx cat /etc/nginx/nginx.conf > /home/zyc/nginx/nginx.conf

# 4. 起 Nginx，挂配置文件、进同一个网络
docker run -d -p 80:80 --name nginx01 --network tomcat_net --restart=always \
  -v /home/zyc/nginx/nginx.conf:/etc/nginx/nginx.conf nginx

# 5. 改完 nginx.conf 后生效
docker restart nginx01
docker exec nginx01 nginx -t
```

`nginx.conf` 里的两块关键配置：

```nginx
http {
    # 反向代理：不同项目走同一个入口的不同路径
    server {
        listen 80;                                    # 容器内端口，不是宿主机端口！
        location /dict { proxy_pass http://tomcat01:8080; }
        location /jd   { proxy_pass http://tomcat02:8080; }
        location /tb   { proxy_pass http://tomcat03:8080; }
    }

    # 负载均衡：同一个项目分给三台
    upstream tomcat_cluster {
        server tomcat01:8080 weight=1;
        server tomcat02:8080 weight=1;
        server tomcat03:8080 weight=1;
    }
}
```

> 权重全 1 = 轮询；有登录的项目改成 `ip_hash;`（写在 `upstream` 第一行），同一 IP 永远落到同一台。

---

## 十九、坑与技巧（补充）

| 现象 | 原因 | 解决 |
| :--- | :--- | :--- |
| `docker: command not found` | 没装 / 没启动 | 装 docker-ce，`systemctl start docker` |
| 拉取镜像超时 | 没配镜像加速器 / 加速器失效 | 配 `/etc/docker/daemon.json`，多配几个地址 |
| `Unable to find image 'xxx' locally` | 本地没有这个镜像 | 先 `docker pull xxx` |
| `conflict: unable to remove repository reference` | 还有容器在用这个镜像 | `docker ps -a` → `docker rm 容器` → 再 `docker rmi` |
| `You cannot remove a running container` | 容器还在跑 | 先 `docker stop`，或 `docker rm -f` |
| `Error response from daemon: Conflict. The container name "/xxx" is already in use` | 这个名字的容器还存在（哪怕启动失败） | `docker ps -a` 找到它 `docker rm`，或者换个 `--name` |
| `port is already allocated` / 访问到别的服务 | 宿主机 8080 被别的容器/程序占了 | 换宿主机端口（`-p 8081:8080`），或 `ss -lntp \| grep 8080` 查占用者 |
| 删完容器马上再 run 报 IP 冲突 | 网络 IP 没立即释放 | `reboot` 重启机器（课上就这么解决的），或等一下再试 |
| 进容器后 `vi: command not found` | 镜像精简，没装编辑器 | 不要在容器里改文件，用挂载或 cp |
| 容器数据丢了 | 用了 `docker rm`，容器内数据随容器销毁 | 用 `-v` 挂载到宿主机 |
| 挂载后访问 403/404 | SELinux enforcing 拦截 | `setenforce 0` 或 `chcon -Rt svirt_sandbox_file_t 目录` |
| **想改端口映射**（8080 换 8081） | `-p` 在 `docker run` 那一刻就定死了，`docker update` 也改不了 | 只能 `docker rm` 后重建；项目用 `-v` 挂载的不会丢，用 `docker cp` 的要先备份 |
| 想开机自动拉起容器（补充） | 默认不会 | `docker run` 加 `--restart=always`；已经建好的容器用 `docker update --restart=always <容器>`，不用重建 |
| 想看实时日志（补充） | — | `docker logs -f <容器ID>` |
| Nginx 起不来，报 `port is already allocated`（80 被占） | 宿主机 80 有别的服务在用 | 换宿主机端口 `-p 8090:80`，**`nginx.conf` 里的 `listen` 仍然写 80** |
| 反向代理全部 404，但直接访问后端端口是好的 | `nginx.conf` 的 `listen` 写成了宿主机端口（如 81） | `listen` 永远写 `-p` 冒号**后面**那个容器端口（80） |
| 反向代理报 502 Bad Gateway | `proxy_pass` 的主机名/端口写错，或容器不在同一网络 | 确认三个容器都加了 `--network tomcat_net`，`docker network inspect` 能看到它们 |
| `host not found in upstream "tomcat01"` | 容器没加入同一个自定义网络 | 加 `--network` 重建容器；容器名拼写也要对 |

---

## 二十、一句话总结

> **传统方式**：装 JDK → 配环境变量 → 装 Tomcat → 传项目 → 关防火墙（十几步，换机器重来）
> **Docker 方式**：`docker pull` → `docker run -d -p 8080:8080 -v 目录 tomcat:8`（两步）
> **Nginx 方式**：`docker network create` → 多台容器进同一网络 → Nginx 按路径（反向代理）或权重（负载均衡）统一分发（一个入口、多台后端）

Docker 的价值不在"少敲几条命令"，而在于**环境是可复制、可传输的**：把自己调试好的环境打包成镜像，别人拉下来就能跑，不会出现"在我电脑上是好的"。后面做大型项目、做集群、做 CI/CD，都是建立在这个基础上的。
