**Docker 入门与项目部署**

这里只是快速上手docker,如果想看完整操作,请看这个笔记:[[Docker]]

---

> **这份笔记的写法**：先是概念（一~三），然后是我**自己动手做的完整记录**（四~六，附真实输出和踩过的坑），最后是还没实践到的知识点备查（七以后）。

---

## 一、Docker 到底解决什么问题

上一份笔记里那套流程，自己也走了一遍：传 JDK、传 Tomcat、解压、挪目录、改 `/etc/profile`、`source`、启动、放网页、关防火墙……

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

按昨天笔记里的安装步骤一步步执行即可（装 docker-ce、启动服务、设置开机自启）。

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
> ```bash
> docker rm eloquent_morse
> ```

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

## 八、下一步：用 Docker 跑一个 Tomcat（重点）

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

### 8.2 运行之后检查

```bash
docker ps
```

看到 `STATUS` 是 `Up`、`PORTS` 是 `0.0.0.0:8080->8080/tcp`，就成功了。

如果 `STATUS` 是 `Exited`，说明启动就退出了，看日志：`docker logs tomcat-01`。

---

## 九、浏览器访问出现 404——这不是错误

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
docker cp ./mi <容器ID>:/usr/local/tomcat/webapps/
```

课上把宿主机 `/opt/tomcat8/webapps/mi`（那个"小米"网页项目）拷进了容器的 `webapps` 下。

拷完再进去验证：

```bash
docker exec -it <容器ID> bash
cd webapps
ls              # 能看到 mi 了
exit
```

然后浏览器访问 `http://虚拟机IP:8080/mi/index.html`，页面就出来了。

---

## 十一、把容器"打包"成新镜像（commit）

每次都 `docker cp` 拷一遍太麻烦，尤其是要部署到多台服务器时。

**思路**：把"已经发布好项目的容器"重新打包成一个**新镜像**，以后直接运行这个新镜像，项目就在里面了。

```bash
docker commit -m "描述信息" -a "作者" <容器ID或名字> <新镜像名:tag>
```

课上实际执行（把 `tomcat-01` 打包成 `tomcat-mi`）：

```bash
docker commit -m "mi project" -a "2617" tomcat-01 tomcat-mi
```

- `-m`：提交说明（message）
- `-a`：作者（author）
- 最后两个：**源容器** 和 **新镜像名**

然后：

```bash
docker images          # 多了一个 tomcat-mi
docker run -d -p 8080:8080 --name tomcat-mi-01 tomcat-mi
```

直接访问 `IP:8080/mi/index.html` 就能看到项目，**不用再 cp**。

### 11.1 commit 的局限（重要）

| 场景 | commit 合适吗 |
| :--- | :--- |
| 项目已经开发完成、不再改动 | ✅ 合适，打包一次到处用 |
| 项目还在调试，今天改完明天还要改 | ❌ 不合适，改一次就得重新 commit 一次 |

所以后面还有更好的办法——**数据卷挂载**（第十三节）。

---

## 十二、镜像的导出与导入（save / load）

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

## 十三、数据卷挂载（-v）——实际最好用的方式

### 13.1 commit 方式的问题

- 项目还在改 → 每次改完都要 commit 一次，太蠢；
- `docker cp` → 每发一次版拷一次，也累。

### 13.2 挂载的思路

**把宿主机的某个目录，和容器里的 `webapps` 目录，直接"绑"在一起。**

```
宿主机 /root/webapps  ←——同一个目录——→  容器 /usr/local/tomcat/webapps
```

以后只需要往宿主机的目录里放文件，容器里立刻就有。**改宿主机 = 改容器**。

### 13.3 命令

```bash
docker run -d -p 8080:8080 --name tomcat-01 \
  -v /root/webapps:/usr/local/tomcat/webapps \
  tomcat:8
```

- `-v` = volume（数据卷）
- 格式：`-v 宿主机目录:容器目录`
- 注意方向：**前面是宿主机的，后面是容器的**

### 13.4 课上验证过程

1. 在宿主机上准备一个目录，把项目拷进去：

```bash
mkdir -p /root/webapps
cp -r /opt/tomcat8/webapps/mi /root/webapps/
ls /root/webapps        # mi
```

2. 用 `-v` 参数启动容器：

```bash
docker run -d -p 8080:8080 --name tomcat-01 \
  -v /root/webapps:/usr/local/tomcat/webapps \
  tomcat:8
```

3. 浏览器访问 `IP:8080/mi/index.html` → 正常显示。

4. **验证"热更新"**：在宿主机上改文件

```bash
cd /root/webapps/mi
vi index.html
# 把 <title> 里的内容改一下，:wq 保存
```

5. 浏览器直接刷新（**没有重启容器、没有重新拷贝**）→ 页面标题变了。

**证明两边真的是同一个目录。**

### 13.5 挂载方式的好处

- 改代码 / 换版本，只往宿主机目录里丢文件就行，容器不用动；
- 多个容器可以挂载**同一个宿主机目录**，一次修改全部生效；
- 容器删了，宿主机的文件还在，数据不会丢。

> （补充）这种叫"绑定挂载"（bind mount）。Docker 还有另一种"命名卷"（`-v 卷名:容器路径`），由 Docker 管理存储位置，更适合放数据库数据。日常发布项目用绑定挂载最直观。

> ⚠️ **CentOS 上的坑**：SELinux 默认 enforcing，挂载后访问会 403/404。先 `getenforce` 看状态，如果是 `Enforcing`，临时 `setenforce 0`，或者给目录打标签 `chcon -Rt svirt_sandbox_file_t /root/webapps`。

---

## 十四、命令总表

### 镜像相关

| 命令 | 作用 |
| :--- | :--- |
| `docker images` | 查看本地镜像 |
| `docker pull 镜像名:tag` | 从仓库拉取镜像 |
| `docker rmi 镜像ID/名` | 删除镜像（有容器在用则删不掉） |
| `docker save 镜像 -o xx.tar` | 把镜像导出成本地文件 |
| `docker load -i xx.tar` | 从本地文件导入镜像 |

### 容器相关

| 命令 | 作用 |
| :--- | :--- |
| `docker run ...` | 用镜像启动容器（核心命令） |
| `docker ps` | 查看运行中的容器 |
| `docker ps -a` | 查看所有容器 |
| `docker ps -q` / `-aq` | 只输出 ID |
| `docker stop <ID>` | 停止 |
| `docker start <ID>` | 启动 |
| `docker restart <ID>` | 重启 |
| `docker rm <ID>` | 删除容器 |
| `docker logs <ID>` | 看容器输出（排错第一招） |
| `docker exec -it <ID> bash` | 进入容器内部 |
| `docker cp 源 容器ID:目标` | 宿主机 ↔ 容器传文件 |
| `docker commit -m "..." -a "..." 容器 新镜像` | 把容器打包成镜像 |

### `docker run` 常用参数

| 参数 | 作用 |
| :--- | :--- |
| `-d` | 后台运行 |
| `-p 宿主端口:容器端口` | 端口映射 |
| `--name 名字` | 指定容器名 |
| `-v 宿主目录:容器目录` | 数据卷挂载 |
| `--rm` | 容器停止后自动删除 |

---

## 十五、完整流程速查

```bash
# 0. 装好 Docker + 配镜像加速器

# 1. 拉镜像（老师机器上已有，我的没有，必须做）
docker pull tomcat:8

# 2. 准备项目目录并放项目
mkdir -p /root/webapps
cp -r /opt/tomcat8/webapps/mi /root/webapps/

# 3. 启动容器（挂载方式）
docker run -d -p 8080:8080 --name tomcat-01 \
  -v /root/webapps:/usr/local/tomcat/webapps \
  tomcat:8

# 4. 检查
docker ps

# 5. 浏览器访问
#    http://虚拟机IP:8080/mi/index.html

# 6. 以后改项目：直接改 /root/webapps 里的文件，刷新页面即可
```

**做集群（跑第二个容器）：**

```bash
docker run -d -p 8081:8080 --name tomcat-02 \
  -v /root/webapps:/usr/local/tomcat/webapps \
  tomcat:8
```

访问 `IP:8080` 和 `IP:8081` 拿到的是同一个项目——这就相当于一个最简单的集群。**容器内部都是 8080，靠宿主机端口错开来区分**，这就是容器隔离最直观的体现。

---

## 十六、坑与技巧（补充）

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
| 想开机自动拉起容器（补充） | 默认不会 | `docker run` 加 `--restart=always` |
| 想看实时日志（补充） | — | `docker logs -f <容器ID>` |

---

## 十七、一句话总结

> **传统方式**：装 JDK → 配环境变量 → 装 Tomcat → 传项目 → 关防火墙（十几步，换机器重来）
> **Docker 方式**：`docker pull` → `docker run -d -p 8080:8080 -v 目录 tomcat:8`（两步）

Docker 的价值不在"少敲几条命令"，而在于**环境是可复制、可传输的**：把自己调试好的环境打包成镜像，别人拉下来就能跑，不会出现"在我电脑上是好的"。后面做大型项目、做集群、做 CI/CD，都是建立在这个基础上的。