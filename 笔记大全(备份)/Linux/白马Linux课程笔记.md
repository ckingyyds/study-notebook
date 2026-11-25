# Linux基础知识

**Linux 之父:林纳斯·托瓦兹**



## 网络连接三种方式

① 桥接模式(B)：直接连接物理网络,独立IP地址(独立门户)

② NAT 模式(N)：用于共享主机的 IP 地址（默认选中，✅ 推荐）(亲戚住你家,正常活动,但是门牌号ip是你家)

③ 仅主机模式(H)：与主机共享的专用网络,只能和主机交流(亲戚住你家,但不让出去,只和你交流)

## 配置VMware固定IP







## 关机与重启

### 1 关机命令

```bash
# 关机
shutdown -h now              # 立即关机
shutdown -h +10              # 10分钟后关机
shutdown -h 1                # 1分钟后关机
shutdown -h 20:00            # 指定时间关机
poweroff                     # 立即关机
halt                         # 立即关机（需要root权限）
init 0                       # 关机

# 取消关机
shutdown -c
```

### 2 重启命令

```bash
reboot                       # 立即重启
shutdown -r now              # 立即重启
shutdown -r +10              # 10分钟后重启
init 6                       # 重启
```

### 3 登录与注销

```bash
# 登录
# 本地登录：直接输入用户名和密码
# 远程登录：ssh 用户名@IP地址

# 注销
exit                         # 退出当前shell
logout                       # 退出当前shell
Ctrl+d                       # 退出当前shell
```

# Linux快捷键和其他知识

## 快捷键

| 快捷键           | 作用                          | 典型场景                                                     |
| ---------------- | ----------------------------- | ------------------------------------------------------------ |
| **Tab**          | 补全命令 / 路径               | 敲两三个字母按 Tab 自动补全；连按两下列出候选——**最重要的习惯**，还防敲错 |
| **Ctrl + C**     | 中止当前命令                  | ping 刷屏停不下来、程序卡死、输错想重来的**标准逃生键**      |
| **Ctrl + D**     | 结束输入 / 退出会话           | 等价敲 `exit`，登出终端就按它                                |
| **↑ / ↓**        | 逐条翻上/下一条历史命令       | 改个小参数重跑上一条命令，不用重敲                           |
| Ctrl + L         | 清屏                          | 等价 `clear`，屏幕太乱时一键清爽                             |
| Ctrl + A / E     | 光标跳到**行首 / 行尾**       | 敲了长命令才发现开头忘加 `sudo`：Ctrl+A 回行首补             |
| Ctrl + U / K     | 删掉光标**前 / 后**的所有内容 | 密码敲错整行重来                                             |
| Ctrl + W         | 删掉光标前**一个单词**        | 长命令只错一个参数                                           |
| Ctrl + R（了解） | **搜索**历史命令              | 敲几个字母就带出以前用过的长命令，越用越省                   |
| Ctrl+S           | 终端会"假死"                  | **Ctrl+Q** 解锁,好玩                                         |



历史命令 history,上一条命令 !!

```bash
history               # 列出本次登录前+至今的所有命令（带编号，默认存 1000 条）
history | grep tar    # 只看用过的 tar 命令（配合 grep，找回上周敲过的长命令）

!1002                 # 重新执行编号 1002 的那条命令

!!                    # 重复上一条命令
# sudo !! 完整演示：
cat /etc/shadow       # 报错 Permission denied（普通用户看不了）
sudo !!               # 自动变成 sudo cat /etc/shadow，密码验证后正常输出

```



## 其他知识

### 通配符与特殊字符

三个基本通配符

| 符号    | 匹配                      | 例子                                           |
| ------- | ------------------------- | ---------------------------------------------- |
| `*`     | 任意个任意字符（含 0 个） | `*.log` 所有日志；`a*` 以 a 开头               |
| `?`     | **恰好一个**任意字符      | `data?.log` 匹配 data1.log 但不匹配 data10.log |
| `[...]` | 括号里任意一个字符        | `file[abc].txt` 匹配 filea/fileb/filec         |

```bash
ls *.txt                    # 所有 .txt 文件
ls data?.log                # data1.log, data2.log（不含 data10.log）
ls [0-9]*                   # 以数字开头的文件（[] 里可以写范围 a-z、0-9）
ls [!a]*                    # 不以 a 开头（! 表示非，bash 里 ^ 也可以）
ls file[123].conf           # file1.conf、file2.conf、file3.conf
rm *.log.[1-5]              # 删除 .log.1 到 .log.5
rm -r test*                 # 删除所有以test开头的文件或文件夹
```

其他高频特殊符号

| 符号      | 作用                           | 例子                                             |
| --------- | ------------------------------ | ------------------------------------------------ |
| `~`       | 当前用户家目录                 | `cd ~` 回家；`ls ~lisi` lisi 的家目录            |
| `{a,b,c}` | 花括号展开：生成多个参数       | `mkdir -p /data/{logs,conf,bin}` 一次建 3 个目录 |
| `{1..5}`  | 生成序列                       | `touch file{1..5}.txt` 建 file1~file5            |
| `;`       | 顺序执行多条命令（不管成败）   | `cd /tmp; ls`                                    |
| `&&`      | 前一条成功才执行后一条         | `make && make install`                           |
| `\|\|`    | 前一条失败才执行后一条         | `ping -c1 host \|\| echo "不通"`                 |
| `\`       | 转义符：让特殊字符变回普通字符 | `echo "价格是 \$100"`                            |
| !!        | 上一次的命令                   | sudo  !!   以管理员身份执行上一条命令,注意空格   |



### **文件类型**

天天见的 3 种（占你遇到的 99%）,ls -l出现的第一位

ls列出的深色表示文件夹,浅色白色表示文件

| 字符 | 类型               | 例子                             | 一句话说明                                |
| ---- | ------------------ | -------------------------------- | ----------------------------------------- |
| `-`  | 普通文件           | `a.txt`、`run.sh`、`/usr/bin/ls` | 文本、脚本、二进制程序、压缩包……统统是它  |
| `d`  | 目录               | `/etc`、`project/`               | 目录本质是"存着子文件名单的特殊文件"      |
| `l`  | 软链接（符号链接） | `/tmp/p -> /etc/passwd`          | 相当于 Windows 快捷方式，箭头后是真实目标 |

还有bcsp,认识即可的 4 种（在 /dev、/run 下会撞见）

# Linux的目录结构

![image-20221027214128453](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027214128.png)

- `/`，根目录是最顶级的目录了
- Linux只有一个顶级目录,没有什么盘符的概念：`/`  **命令:   cd  /    切到根目录**
- 路径描述的层次关系同样适用`/`来表示
- 在Linux系统中，路径之间的层级关系，使用：/来表示(除号)
- /home/bob/a.txt，表示根目录下的home文件夹内有bob文件夹，内有a.txt
- 注意：
  ·开头的/表示根目录,描述目录都从/开始
  ·后面的/表示层级关系

 Linux 核心哲学 ——"一切皆文件" 

## 核心目录一览 

```
/   →根目录，一切从这里开始
├──    一、系统核心区（管启动和系统本身）
├── /boot/     存放系统启动相关的文件,内核 vmlinuz、GRUB、initramfs
├── /etc/      ⭐ 系统级配置文件中心（改得最多）,相当于 Windows 的注册表
├── /lib,/lib64(软链接)  存放系统最基本的共享库,很多命令（比如 ls、cp）运行时要依赖这里的库文件
├── /root/     root用户的家目录（注意不是 /home/root）(需要管理员权限进入)
├──    二、用户与软件区（管人和程序）
├── /home/     ⭐ 普通用户的家目录 /home/用户名/,如/home/zyc/
├── /usr/      ⭐ 存放用户安装的软件和系统资源
	├── /usr/local：用户自己手动编译安装的软件通常放在这里（比如源码编译的 Nginx）
├── ⭐/bin(软链接) 存放普通用户能用的命令（如 ls、cp、vim）
├── /sbin(软链接)  存放管理员才能用的命令（如 useradd、systemctl)
├── /opt/      额外装的第三方软件，一个软件一个文件夹（如 /opt/google/chrome）习惯上，/usr/local 放手动编译的，/opt 放第三方整包解压的
├──     三、数据与运行时区（管动态变化的数据）
├── /var/      ⭐ 可变数据：日志、数据库、缓存
├── /tmp/      临时文件，所有人可写，30 天自动清理
├── /run/      运行时数据（PID、Socket），重启清空（内存文件系统）
├──     四、设备与挂载区（管硬件）
├── /mnt/      临时挂载点（自己 mount 用）
├── /media/    可移动介质自动挂载点（U 盘、光盘）
├── /dev/      设备文件（硬盘、终端、null），由 udev 动态生成
├── /sys/      虚拟文件系统：硬件与驱动信息
├── /proc/     虚拟文件系统：进程与系统信息（不占磁盘）
├── 其他,了解即可
├── /srv/      服务数据目录
└── /lost+found/  文件系统修复时的"抢救区"
```

```
bin/        ← /usr/bin 的软链接（CentOS 7+）
sbin/       ← /usr/sbin 的软链接
lib/        ← /usr/lib 的软链接,存放系统最基本的共享库,很多命令（比如 ls、cp）运行时要依赖这里的库文件
lib64/      ← /usr/lib64 的软链接,同lib/
```

最常用的几个目录

1. **`/etc`**：改配置去这里。
2. **`/home`**：你自己的文件在这里。
3. **`/var/log`**：系统出问题了，来看日志。
4. **`/usr/bin`**：系统命令都在这里。

# Linux基础命令



## Linux命令基础

无论是什么命令，用于什么用途，在Linux中，命令有其通用的格式：

**command [-options] [parameter]**

- command：命令本身
- -options：[可选,非必填]命令的一些选项,可以通过选项控制命令的行为细节,一个-后面可以跟多个属性组合,顺序不影响,如ls -lha 和 ls -alh和 ls -a -s -h是一样的
- parameter：[可选,非必填]命令的参数,多数用于命令的指向目标等

示例：

- `ls -l /home/itheima`，ls是命令本身，-l是选项，/home/itheima是参数
  - 意思是以列表的形式，显示/home/itheima目录内的内容
- `cp -r test1 test2`，cp是命令本身，-r是选项，test1和test2是参数
  - 意思是复制文件夹test1 成为 test2



## ls命令(文件夹三剑客1)

**功能：**列出文件夹信息

**直接输入ls命令,**表示列出当前工作目录下的内容,当前工作目录是?

Linux系统的命令行终端,在启动的时候,默认会加载HOME家目录

家目录我觉得可以理解成/home/用户的软连接 , **cd ~** 出来的是家目录实际路径在/home/zyc下

**语法**：`ls [-l -h -a] [Linux路径]`

- 参数：被查看的文件夹，不提供参数，表示查看当前工作目录
- -l，以列表形式查看文件
- -h，以易于阅读的形式（必须与l一起使用 -lh，会显示文件大小单位KB之类的）
- -a，显示所有文件all（含隐藏） 例如在家目录下ls -a

**ls -lh 列表形式输出格式解读（共 7 列，空格隔开）：**

```
drwxr-xr-x  2 root root 4.0K May 10 08:30 test
    ①       ②   ③   ④     ⑤       ⑥        ⑦
```

| 列         | 例子里的值     | 含义                                                         |
| ---------- | -------------- | ------------------------------------------------------------ |
| ① 权限     | `drwxr-xr-x`   | 文件类型 + 三组权限（10 个字符怎么读见 [6.1](#)：第 1 位是文件类型 d=目录，后面属主 rwx、属组 r-x、其他人 r-x） |
| ② 硬链接数 | `2`            | 有多少个硬链接指向它（目录默认 2，知道即可）                 |
| ③ 属主     | `root`         | 文件所有者                                                   |
| ④ 属组     | `root`         | 文件所属的组                                                 |
| ⑤ 大小     | `4096`         | 字节数（普通文件才准；目录固定显示 4096，不代表里面内容大小）。加 `-h` 显示成 4.0K |
| ⑥ 修改时间 | `May 10 08:30` | 最后一次修改的日期时间                                       |
| ⑦ 文件名   | `test`         | 名字；`l` 开头的软链接这列会显示 `链接名 -> 真实路径`        |

**补充:**

ls -lt                   # 按时间排序
ls -lS                    # 按大小排序
ls -lR                    # 递归列出,根目录你试试呢

***ls列出的深色表示文件夹,浅色白色表示文件***

### 隐藏文件、文件夹

在Linux中以`.`开头的文件名，均是隐藏的。

默认不显示出来，需要`-a`选项才可查看到。



## cd命令(文件夹三剑客2)

功能：切换工作目录

语法：`cd [目标目录]`,没有属性

参数：目标目录，要切换去的地方，不提供默认切换到`当前登录用户HOME目录`

```
cd                        # 回到家目录（等价于 cd ~）
cd /					# 回到根目录
cd /etc                   # 绝对路径,从根目录开始
cd ../home                # 相对路径,比如在mnt下面时
cd ~user                  # 切换到指定用户家目录
cd -                      # 回到上一次的目录
cd ..                     # 回到上一级目录
```



### 相对路径、绝对路径

- 相对路径，==非==`/`开头的称之为相对路径

  相对路径表示以`当前目录`作为起点，去描述路径，如`test/a.txt`，表示当前工作目录内的test文件夹内的a.txt文件

- 绝对路径，==以==`/`开头的称之为绝对路径

  绝对路径从`根`开始描述路径
  
  
  
  示例:在/var/games下

[zyc@mycentos games]$ cd /home/zyc/桌面      #绝对路径

[zyc@mycentos 桌面]$ cd -
/var/games

[zyc@mycentos games]$ cd ../../home/zyc/桌面     #相对路径

### 特殊路径符

- `.`，表示当前，比如./a.txt，表示当前文件夹内的`a.txt`文件,  ./也可以省略
- `..`，表示上级目录，比如`../`表示上级目录，`../../`表示上级的上级目录
- `~`，表示用户的HOME目录，比如`cd ~`，即可切回用户HOME目录,可以省略



## pwd命令(文件夹三剑客3)

功能：以绝对路径展示当前工作目录

语法：`pwd`



## mkdir/ rmdir命令

功能：创建文件夹/删除文件夹

语法：`mkdir [-p -m -v ] 参数`

- 参数：被创建文件夹的路径
- 选项：-p，可选，表示创建前置路径(递归创建)

  ​           -m,  指定权限创建

  ​		   -v,  显示创建信息,就是给你一个提示

[zyc@mycentos mnt]$ sudo mkdir -pv test/test1/test2/test3 
mkdir: 已创建目录 "test"
mkdir: 已创建目录 "test/test1"
mkdir: 已创建目录 "test/test1/test2"
mkdir: 已创建目录 "test/test1/test2/test3"

语法：`rmdir [-p  -v ] 参数`

`rmdir` 有一个**非常严格的限制**：**它只能删除“空目录”。** 只要目录里有一个文件或子目录，它就会报错拒绝执行

递归删除空目录

```
rmdir -p a/b/c
```

显示删除过程信息

```
rmdir -v test
```



```bash
mkdir ../test        # 创建单个目录
mkdir -p a/b/c       # 递归创建(自动创建不存在的父目录)
mkdir -m 755 test    # 指定权限创建,755你懂的
mkdir -v test        # 显示详细过程
mkdir {q,w,e}        # 创建多个同级目录

rmdir test           # 删除空目录
后面学的rm命令强制删除
rm -rf test          # 强制删除非空目录 -r递归删除 f强制删除
```



## touch命令

功能：创建文件

语法：`touch 参数` 

- 参数：被创建的文件路径

```
touch file.txt       # 创建空文件（不存在时）,存在时则更新时间戳,内容不变 
touch file1.txt file2.txt    # 一次创建多个更新时间
```



## cat命令

功能：查看显示文件内容(也可以写入创建文件)

语法：`cat 参数`

- 参数：被查看的文件路径

```bash
cat ~/zyc/demo1/aaa.txt    # 查看文件全部内容
cat -n /etc/passwd         # 显示行号（配置文件找第几行时好用）
cat -b file.txt            # 只给非空行编号

# cat 可以连接多个文件，依次输出（这才是它名字的由来）
cat file1 file2            # 先输出 file1 全部，再输出 file2 全部(不换行)
cat header.txt body.txt > all.txt   # 拼接两个文件成一个新文件(创建一个新文件)
```

**用 cat 创建/写入文件（不用进编辑器就能写，脚本里常用）：**

cat写入文件,如果该文件不存在,和vim打开不存在的文件一样,会创建该文件,cat查看文件不行

```bash
cat > note.txt       # 回车后开始输入内容，输入完按 Ctrl+d 结束（> 是覆盖）
cat >> note.txt      # >> 是追加到文件末尾，不会覆盖原内容
hello
Ctrl+d               # 按住 Ctrl 再按 d，结束输入

# 多行一次写入（heredoc，写配置文件时最常用，注意 EOF 顶格写）
<<'EOF'：这是 Here Document 的起始标志。
<<：告诉 Shell，接下来的输入不是从键盘敲，而是从下面的代码块里读。
'EOF'：这是一个自定义的结束分隔符（名字叫什么都行，通常用 EOF，即 End Of File）。
最后的 EOF：单独占一行，表示“输入到此结束”
cat > ~/zyc/demo1/try.txt <<'EOF'
server {
    listen 80;
}
EOF
```

>  **cat 的局限：只适合小文件**。大文件（日志几十万行）用 cat 会刷屏刷半天，应该用 `less`（分页看）、`head/tail`（看头尾）、`grep`（筛内容）。这是新手最常见的"把终端刷爆"事故。







## more/less命令

功能：查看文件，可以支持翻页查看

语法：`more 参数`

- 参数：被查看的文件路径
- 在查看过程中：
  - `空格`键翻页
  - `q`退出查看

语法：`less 参数`

less -N 文件   ------>带行号打开

**用这个,这个吊,**cat和more的结果真正会显示在命令行,less相当于打开看,不会使得终端冗余,而且不管多大都秒开,是个秒开仙人

在查看过程中：

- j` / `k` 或 方向键       下/上移动一行
- `空格` / `f`                向下翻一页（f = forward）
- `b`                            向上翻一页（b = backward）
- `g` / `G`                     跳到文件**开头 / 结尾**
- `Num g`                     跳到第 N 行（如 `100g` 跳第 100 行）
- `/关键字` 回车          向下搜索
- `?关键字` 回车          向上搜索
- `n` / `N `                    下一个 / 上一个匹配,和上下搜索一起用
-  v                             启用编辑器
- `q`                            退出查看











## cp命令(copy)

功能：复制粘贴文件、文件夹

语法：`cp [-r] 参数1 参数2`

- 参数1，被复制的文件或文件夹的路径
- 参数2，要复制去的地方的路径
- **选项：-r，可选，递归复制文件夹使用**
- ​           -i :覆盖前询问
- ​           -v:显示复制过程
- ​           -p:保留修改时间和权限,只复制文件但想保留原属性

简单示例：

- cp a.txt b.txt，复制当前目录下a.txt到当前目录下为b.txt
- cp a.txt test/，复制当前目录a.txt到test文件夹内
- cp -r test test2，复制当前目录下的文件夹test到当前目录下为test2文件夹



#### 复制单个文件

先造测试文件：cat  > a..txt   输入后ctrl +d

```bash
# 情况1：目标不存在 → 复制并改名（相当于"另存为"）
cp a.txt b.txt         # 当前目录没有 b.txt → 新建 b.txt，内容和 a.txt 一样

# 情况2：目标是一个已存在的文件 → 覆盖它（危险，默认不提示！）
cp a.txt b.txt         # b.txt 已存在 → b.txt 原内容被覆盖，无确认提示
cp -i a.txt b.txt      #覆盖前询问

# 情况3：目标是一个已存在的目录 → 复制进这个目录，文件名不变
mkdir backup           #创建在当前文件夹下
cp a.txt backup/       # backup/ 是目录 → 得到 backup/a.txt（注意末尾的 / 是好习惯）

# 情况4：目标是一个不存在的目录（末尾带 / 但目录不存在）→ 报错
cp a.txt notexist/     # cp: cannot create regular file 

# 情况5：一次复制多个文件到目录（最后一个参数必须是已存在的目录）
cp a.txt b.txt c.txt qq/        # 三个文件都进 qq/
cp *.conf /etc/nginx/conf.d/    # 通配符：所有 .conf 复制过去
```



#### 复制整个文件夹(必须加-r)

复制文件夹时，目标目录存不存在又是两种结果：

```bash
# dir2 不存在 → 整个 dir1 复制一份并改名为 dir2
cp -r dir1 dir2           # 得到 dir2/，里面内容和 dir1 一样

# dir2 已存在（是目录）→ dir1 整个被放进 dir2 里面
cp -r dir1 dir2           # 得到 dir2/dir1/（多套了一层！这是最常见的意外）
```

#### 





## mv命令(move)

**功能：移动/ 重命名 文件和文件夹**  , mv = move，但它身兼两职：**在同一个目录里 = 重命名；跨目录 = 移动**。一条命令搞定，没有单独的 rename 命令（Windows 则是改名和剪切分开的）。

语法：`mv 参数1 参数2`

- 参数1：被移动的文件或文件夹
- 参数2：要移动去的地方，参数2如果不存在，则会进行改名,通常参数2是路径
- 属性 -i   覆盖前询问
- ​        -v  显示过程

```bash
# ① 在同一个目录 → 重命名（目标不存在时）
mv oldname.txt newname.txt     # 文件改名
mv olddir newdir               # 目录改名（不需要 -r！）

# ② 跨目录 → 移动（目标是已存在的目录时）
mv file.txt /aaa     这个分两种情况: 1.如果aaa存在，那么就会把file.txt放到aaa文件夹。 2.如果原来aaa文件夹不存在，就相当于给file.txt重命名为aaa,并移动到/根目录下面。含义不同。
其实情况2就相当于移动的同时改名
mv file.txt /tmp/newname.txt   # 移动的同时改名

mv file.txt /home/111/aaa/  把文件移动到/home/111/aaa/，不是根目录你要写好绝对路径，mv file.txt /aaa/的意思是移动到根目录下面的aaa，不是/home/111/aaa/下面的aaa文件夹,  第二个/明确告诉命令： 这东西必须是【目录】,复制路径最后最好加一个这个


mv a.txt b.txt c.txt /tmp/     # 多个文件移动到目录（最后一个参数是目录）
mv *.log /var/log/archive/     # 通配符批量移动
```



**目标已存在时的行为（和 cp 一样容易踩坑）：**

```bash
mv a.txt b.txt        # 目标存在时 → b.txt 已存在 → b.txt 被覆盖，无提示！
mv dir1 dir2          # dir2 已存在且是目录 → dir1 整个移进 dir2/ 里面（变 dir2/dir1）
                      # dir2 不存在 → dir1 直接改名为 dir2
```





## rm命令(remove)

功能：删除文件、文件夹

语法：`rm [-r -f] 参数...参数`

- 参数：支持多个，每一个表示被删除的，空格进行分隔
- 选项：-r，递归删除,删除文件夹使用
- 选项：-f，强制删除，不会给出确认提示，root权限才能用(非自己用户目录时)
- 选项:  -i,    每个文件删除前都询问确认（interactive）
- 选项: -v,    显示删除过程
- 选项:`-rf`  强制文件夹（最常用，也最危险）

基本用法

```bash
rm file.txt               # 删除单个文件（默认直接删，不提示）
rm a.txt b.txt c.txt      # 一次删多个
rm *.log                  # 通配符：删除当前目录所有 .log
```

删除目录

```bash
rm -r mydir               # ✅ 递归删除：目录 + 里面所有子目录和文件
rm -rf mydir              # 强制删目录，不问不报错
rm -i important.txt       # 谨慎删除：rm: remove regular file 'important.txt'?  按 y 确认
```

注意事项

**Linux 的 rm 不进回收站，删了就是删了，没有"撤销"。** rm命令很危险，一定要注意，特别是切换到root用户的时候。

以下几条务必记住：

```bash
# 1 永远不要在没看清当前路径时执行带通配符的 rm -rf
cd /opt/myapp/logs        # 先 cd 进去、pwd 确认、ls 看一眼，再删
rm -rf *.log              # 确认无误才执行

# 2 这些写法极度危险，看到就要警惕：
rm -rf /                  # 自杀命令：删除整个系统所有文件
rm -rf $VAR/              # 变量为空时变成 rm -rf /，会删全盘！
rm -rf /*                 # 同理

# 3 路径有空格会要命
rm -rf / tmp/x            # 看起来想删 /tmp/x，实际系统解析成删 / 和 tmp/x 两个目标！
```





## which命令(查找文件命令1)

功能：查看命令的程序本体文件路径,只针对命令的程序文件

语法：`which 参数`

- 参数：被查看的命令
- 选项: -a 显示所有匹配的文件路径

```shell
which -a ls
# 输出示例：/bin/ls
           /usr/bin/ls
which cd
# 输出示例：/bin/cd
```



## find命令(查找文件命令2)

功能：搜索文件

语法1按文件名搜索：`find 起始路径 选项 参数`

- 路径，搜索的起始路径

- 参数，搜索的关键字，支持通配符*， 比如：`*`test表示搜索任意以test结尾的文件

- 选项说明

  | 选项             | 功能                                                         | 开发高频度  |
  | ---------------- | ------------------------------------------------------------ | ----------- |
  | `-name <文件名>` | 按文件名查找(支持通配符 `*` `?`,要加引号)                    | ⭐⭐⭐ 最高频  |
  | `-type <类型>`   | 按文件类型找(`f`=普通文件,`d`=目录,`l`=软链接)               | ⭐⭐ 常用     |
  | `-size <大小>`   | 按大小找(`+`大于,`-`小于,无符号=等于;单位:`k`/`M`/`G`) k小写,其余两个大写 | ⭐⭐ 清磁盘   |
  | `-mtime <天数>`  | 按修改时间找(`+`N天前,`-`N天内)                              | ⭐⭐ 清旧日志 |

简单使用

```shell
# 案例1:按文件名 - 找根目录下的test文件
find / -name "test"   
find /mnt -name *.txt   #找mnt文件夹下的所有txt文件

# 案例2:按类型 - 只找目录
find /var -type d

# 案例3:按大小 - 找整个系统下大于 200M 的文件(+n 大于,-n 小于,n 等于)
find / -type f -size +200M 
ls -lh 复制出现的路径   #验证

# 案例4:按时间 - 清理 7 天前的旧日志
find /var/log -name "*.log" -mtime +7 -delete
```

练习

请使用find命令找出：名称中带有centos的文件

```shell
find / -name "*centos*"
```

请使用find命令找出：/usr 目录内大于100M的文件

```shell
find /usr -size +100M
```



## grep命令(联用管道符)

**功能：**从文件中通过关键字过滤文件行

find 负责找文件，grep 负责找内容

语法：`grep [选项] "关键字" 文件路径`

- 参数，关键字，必填，表示过滤的关键字，带有空格或其它特殊符号，建议使用””将关键字包围起来

- 参数，文件路径，必填，表示要过滤内容的文件路径，可作为内容输入端口(管道符的输入)

- | 选项                     |                      作用                      | 助记                     |
  | ------------------------ | :--------------------------------------------: | ------------------------ |
  | `-i`                     |                 忽略大小写匹配                 | ignore-ca se             |
  | `-n`                     |            显示行号（定位日志必备）            | number                   |
  | `-r`                     |                递归搜索整个目录                | recursive                |
  | `-v`                     |        反向匹配：显示**不含**关键词的行        | invert                   |
  | `-c`                     |                 统计匹配的行数                 | count                    |
  | `-l`                     |          只列出包含关键词的**文件名**          | list                     |
  | `-w`                     |       整词匹配（error 不会匹配 errors）        | word                     |
  | `-A n` / `-B n` / `-C n` |         显示匹配行**后/前/前后** n 行          | After / Before / Context |
  | `-o`                     |         只输出匹配到的片段，不输出整行         | only-matching            |
  | `-E`                     |     扩展正则（`\|` `()` `{}` 不用加转义）      | extended                 |
  | `-q`                     | 不输出任何内容，只返回"找到没有"（脚本判断用） | quiet                    |

  

**基本用法（拿 a.txt 走一遍就懂）**

先准备一个测试文件：

```bash
cat > a.txt 
2026-09-12 10:00:01 INFO  service started
2026-09-12 10:00:05 ERROR db connect failed
2026-09-12 10:00:09 WARN  retrying
2026-09-12 10:01:00 ERROR timeout after 30s
2026-09-12 10:02:00 INFO  error count 2

```

最基本的用法：把含 ERROR 的行筛出来

```bash
grep "ERROR" a.txt
```

输出（只显示包含 ERROR 的行，其他行全部丢掉）：

```
2026-09-12 10:00:05 ERROR db connect failed
2026-09-12 10:01:00 ERROR timeout after 30s
```

再配合最常用的 4 个选项，各跑一遍看区别：

```bash
grep -n "ERROR" a.txt    # 过滤出来的结果加行号，方便回日志里定位：输出前面多出 2: 4:
grep -i "error" a.txt    # 忽略大小写：连第 5 行的小写 error 也能匹配到
grep -v "INFO" a.txt     # 反向过滤：不含 INFO 的行才显示（剩 2、3、4 行）
grep -c "ERROR" a.txt    # 只输出一个数字 2：统计有 ERROR 的行数
```

高级用法还没看----老李笔记---上下文与正则,实际场景几乎都配合管道符用（调试日志最常用）



## wc命令(联用管道符)

功能：统计行数/单词/字节   word count

语法：`wc [-c -m -l -w] 文件路径`

- 选项，-c，统计bytes数量
- 选项，-m，统计字符数量
- 选项，-l，统计行数
- 选项，-w，统计单词数量
- 参数，文件路径，被统计的文件，可作为内容输入端口(管道符的输入)

```bash
wc 离骚.txt               # 输出：行数  单词数  字节数  文件名
					    #       120   450    3200  file.txt
wc -l 离骚.txt            # 只统计行数（line）——最高频
wc -w 离骚.txt            # 只统计单词数（word，按空格分）
wc -c 离骚.txt            # 只统计字节数（byte）除1024是内存的那个
wc -m 离骚.txt            # 只统计字符数（中文一个字算 1，-c 会算成 3 个字节）
wc -L 离骚.txt            # 最长那一行有多少字符
```

实际场景几乎都配合管道符用：

```bash
ps aux | wc -l                    # 当前有多少个进程
cat access.log | grep " 500 " | wc -l    # 日志里 500 错误有多少条
grep -c "ERROR" app.log           # 其实 grep 自带 -c 统计匹配行数，更省事
ls | wc -l                        #查看文件夹下有多少文件或文件夹
ls *.log | wc -l                  # 当前目录有多少个 .log 文件
```





## 管道符|  (结果输入)只是简单掌握,考完需要还愿

写法：`|`

功能：将符号左边的**结果**，作为符号右边的输入

为什么叫"管道"?你可以把它理解成一根**连接两个命令的水管** —— 前一个命令生产的数据顺着这根水管直接流进后一个命令,**中间不落地、不存文件**,速度极快。

语法:

```
命令1 | 命令2 | 命令3 ... 
       前一个的 stdout(输出) → 后一个的 stdin(输入)
```

简单示例：

```bash
cat bbb.txt | grep -nv "itheima"  #将cat bbb.txt的结果，作为grep命令的输入，用来过滤`itheima`关键字  (/home/zyc/demo1)
ls -l | grep ".txt"           # 只看含 .txt 的行
ps aux | grep java            # 找 java 进程,ps是进程相关命令
cat access.log | wc -l        # 统计日志行数（cat 可省略，直接 wc -l < file）
history | grep ssh            # 从历史命令里找用过的 ssh
ifconfig | grep inet          # 只看 IP 那几行
cat a.txt | grep itheima | grep itcast    #可以支持嵌套
```

例题:

对创建的test.txt进行统计

请使用cat、grep、管道符、wc命令组合，进行统计：

- 统计文件中带有itcast关键字的有几行

```
cat test.txt | grep itcast | wc -l
```

- 统计文件中带有itheima关键字的结果中有多少个单词

```
cat test.txt | grep itheima | wc -w
```



## echo命令(print来了)

功能：把后面的文字打印到屏幕，**写脚本时输出提示信息、配合重定向写文件、查看变量**都靠它

语法：`echo  "参数" `

- 参数：被输出的内容,引号是习惯

**简单用法以及两个常用选项:**直接打印

```bash
echo "Hello World"               # 最基本：打印一行文字
echo Hello World                 # 不带引号也行，但含空格/特殊字符时建议加双引号
echo -n "请输入："                # -n：结尾不换行（做交互提示，光标停在后面）
echo -e "第一行\n第二行\t缩进"     # -e：识别转义字符（默认不识别）
```

### `反引号

功能：被两个反引号包围的内容，会作为命令执行

示例：

- echo \`pwd\`，会先执行pwd,将pwd结果通过echo输出

你放这pwd在这举例子真的不想笑吗,直接输pwd有什么区别



### **高级用法:配合变量和命令替换,**输出变量:等我回来还愿





## head/tail命令

**功能：**查看文件头部内容

**语法：**`head [-n] 参数`

- 参数：被查看的文件
- 选项：-n，查看的行数,不写默认10行,可以直接写-数字,如-20

**功能：**查看文件尾部内容

**语法：**`tail [-f] 参数`

- 参数：被查看的文件
- 选项：-f，持续跟踪文件修改
- 选项：-n，查看的行数,不写默认10行,可以直接写-数字,如-20

**基础用法**

```bash
head file.txt             # 默认显示前 10 行
head -n 20 file.txt       # 显示前 20 行（n 可省略：head -20 file.txt）
head -c 100 file.txt      # 显示前 100 个字节（不是行）

tail file.txt             # 默认显示最后 10 行
tail -n 20 file.txt       # 显示最后 20 行
tail -c 100 file.txt      # 显示最后 100 个字节
```

**tail -f   /  tail -F高级用法**

```bash
tail -f ~/demo1/aa.txt               # txt写入文件保存时,自动显示,常用来监视日志 , Ctrl+c 退出  
tail -f /var/log/nginx/access.log    # 打开后"挂着"，有新日志写入就实时显示
tail -f -n 100 app.log               # 先显示最后 100 行，再继续跟踪
```

**-f和-F的区别**

-f是死的,监视的是文件,如果文件改名字了,就监视不到了,改回来也不行

-F是活的,监视的是文件名,如果文件名改了,他会说找不到这个文件名,如果这个文件名又出现了,他会继续自动追踪

**和管道符组合用法：**

```bash
tail -f app.log | grep ERROR          # 实时只盯报错行
head -n 50 file.txt | tail -n 20      # 看第 31~50 行（先取前50行，再取其中最后20行）
ps aux | head -n 6                    # 配合管道：只看 ps 输出的前 6 行（含表头）
```





## 重定向符(结果写入)  只是简单掌握,考完需要还愿

功能：将符号左边的**结果**，输出到右边指定的文件中去

- `>`，表示覆盖输出
- `>>`，表示追加输出

```
echo `pwd` >> /home/zyc/demo1/a.txt
```

**管道 + 重定向组合（经典写法）：**

```bash
# 查日志里的 500 错误，去重统计，取前 10，存到文件
grep " 500 " access.log | awk '{print $7}' | sort | uniq -c | sort -rn | head -10 > top500.txt
```

> ⚠️ **管道默认只传 stdout，不传 stderr**。想让报错也进管道：`command 2>&1 | grep xxx`。管道不会自动把结果写文件，要存盘就在整条链最后加 `> 文件`。
>



## vim编辑器

![img](https://i-blog.csdnimg.cn/blog_migrate/5e5ff7f41235dd9021ae45c9f6449f5b.png)

**语法:**vim 文件路径

如果文件路径表示的文件**不存在**，那么此命令会用于**新建并打开编辑**

如果文件路径表示的文件**存在**，那么此命令用于**编辑已有文件**



先记住两条规律，所有快捷键都能举一反三：

> **规律 1：任何移动命令前都可以加数字，表示重复几次。**
> 如 `5j` = 向下 5 行，`3w` = 向后跳 3 个单词，`10dd` = 删 10 行。
>
> **规律 2：vim 快捷键 = 操作符 + 范围（动词+宾语）。**(建议少用,或者等熟练后使用)
> 操作符有 `d`(删)、`y`(复制)、`c`(删并进入编辑)；范围有 `w`(单词)、`$`(到行尾)、`G`(到文件末尾) 等。
> 如 `dw` = 删除一个单词，`d$` = 从光标删到行尾，`yG` = 复制到文件末尾。**操作符按两下 = 作用于整行**：`dd`、`yy`、`cc`。

### 命令模式快捷键

oO是新开一行

![image-20221027215841573](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027215841.png)

![image-20221027215846581](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027215846.png)

**补充**

| 按键      | 作用                   | 例子/说明          |
| --------- | ---------------------- | ------------------ |
| `x(必会)` | 删除光标处**一个字符** | `3x` 连删 3 个字符 |
| `X`(必会) | 删除光标**前一个**字符 | 类似键盘 Backspace |

![image-20221027215849668](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027215849.png)

*G和gg是文件开头和结尾切换*

### 底线命令快捷键

![image-20221027215858967](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027215858.png)



### **Vim配置文件永久高亮**

```shell
第 1 步：在终端（黑窗口命令行，不是 vim 里面）执行
vim ~/.vimrc
# 文件不存在也没关系，vim 打开空白页面，保存时会自动创建这个文件
第 2 步：按 i                    → 左下角出现 -- INSERT --，进入插入模式
第 3 步：把下面的配置内容复制/敲进去
    set number
    syntax on
    set ruler
    set showmatch
    set autoindent
    set smartindent
    set tabstop=4
    set shiftwidth=4
    set expandtab
    set hlsearch
    set incsearch
    set ignorecase
    set smartcase
    set encoding=utf-8
    set fileencodings=utf-8,gbk
	set pastetoggle=<F2>
第 4 步：按 Esc                  → 退出插入模式
第 5 步：输入 :wq 再按回车        → w=保存 q=退出（:q! 是不保存退出）
第 6 步：cat ~/.vimrc          # 能看到你写进去的配置，说明保存成功了
验证:vim /etc/passwd       # 随便打开个文件，行号和语法高亮应该自动出现了
```







## help命令

查看命令的帮助

可以通过：`命令 --help`查看命令的选项的帮助手册

绝大多数命令都支持 `--help`（或 `-h`），在屏幕上直接打印一份**简明帮助**：命令语法、参数列表、常用示例，看完即走，不用翻整本手册。

```bash
ls --help           # 忘了 -l -a -h 各是什么意思时
tar --help          # 最常用！tar 参数多如牛毛，谁都记不住，用前查一下
cp --help
grep --help
useradd --help
```

输出通常很长，配合管道看：

```bash
tar --help | less        # 内容太多时用 less 分页看（上下箭头翻，q 退出）
tar --help | grep -i compress   # 只看跟"压缩"有关的参数行
```

  

## man命令

`man`（manual）是系统自带的**官方手册**，内容比 `--help` 全得多：不仅有每个参数的详细解释，还有使用示例、相关命令、配置文件说明。

```bash
man ls              # 打开 ls 的手册
man tar             # 学 tar 必看，里面有最权威的参数组合示例
man useradd         # 管理员命令也有手册
man find            # 查看 find 命令的官方完整手册
```

**man 手册的内部结构（打开后知道看哪几段）：**

```
NAME         一句话说明这命令是干嘛的
SYNOPSIS     语法格式（方括号 [] 里的是可选项）
DESCRIPTION  详细说明
OPTIONS      所有参数的解释 ← 你最常看的部分
EXAMPLES     使用示例（部分手册有）
FILES        相关配置文件
SEE ALSO     相关命令
```



# Linux常用操作

## 文件打包压缩和解压

压缩解压

压缩

`tar -zcvf 压缩包 被压缩1...被压缩2...被压缩N`

- -z表示使用gzip，可以不写



`zip [-r] 参数1 参数2 参数N`

![image-20221027221906247](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221906.png)



解压

`tar -zxvf 被解压的文件 -C 要解压去的地方`

- -z表示使用gzip，可以省略
- -C，可以省略，指定要解压去的地方，不写解压到当前目录







`unzip [-d] 参数`

![image-20221027221939899](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221939.png)





## 文件软链接操作

功能：创建文件、文件夹软链接（快捷方式）

语法：`ln -s 参数1 参数2`

- 参数1：被链接的
- 参数2：要链接去的地方（快捷方式的名称和存放位置）



## 文件权限管理

**chmod命令**

修改文件、文件夹权限



语法：`chmod [-R] 权限 参数`

- 权限，要设置的权限，比如755，表示：`rwxr-xr-x`

  ![image-20221027222157276](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222157.png)

- 参数，被修改的文件、文件夹

- 选项-R，设置文件夹和其内部全部内容一样生效



**chown命令**

修改文件、文件夹所属用户、组



语法：`chown [-R] [用户][:][用户组] 文件或文件夹`

![image-20221027222326192](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222326.png)









## 用户和用户组管理

用户组管理

![image-20221027222354498](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222354.png)



## 用户管理

![image-20221027222407618](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222407.png)



































## 软件安装

- CentOS系统使用：
  - yum [install remove search] [-y] 软件名称
    - install 安装
    - remove 卸载
    - search 搜索
    - -y，自动确认
- Ubuntu系统使用
  - apt [install remove search] [-y] 软件名称
    - install 安装
    - remove 卸载
    - search 搜索
    - -y，自动确认

> yum 和 apt 均需要root权限



## systemctl

功能：控制系统服务的启动关闭等

语法：`systemctl start | stop | restart | disable | enable | status 服务名`

- start，启动
- stop，停止
- status，查看状态
- disable，关闭开机自启
- enable，开启开机自启
- restart，重启



- 



## 日期

语法：`date [-d] [+格式化字符串]`

- -d 按照给定的字符串显示日期，一般用于日期计算

- 格式化字符串：通过特定的字符串标记，来控制显示的日期格式
  - %Y   年%y   年份后两位数字 (00..99)
  - %m   月份 (01..12)
  - %d   日 (01..31)
  - %H   小时 (00..23)
  - %M   分钟 (00..59)
  - %S   秒 (00..60)
  - %s   自 1970-01-01 00:00:00 UTC 到现在的秒数



示例：

- 按照2022-01-01的格式显示日期

  ![image-20221027220514640](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027220514.png)

- 按照2022-01-01 10:00:00的格式显示日期

  ![image-20221027220525625](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027220525.png)

- -d选项日期计算

  ![image-20221027220429831](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027220429.png)

  - 支持的时间标记为：

    ![image-20221027220449312](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027220449.png)





## 时区

修改时区为中国时区

![image-20221027220554654](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027220554.png)



## ntp

功能：同步时间

安装：`yum install -y ntp`

启动管理：`systemctl start | stop | restart | status | disable | enable ntpd`



手动校准时间：`ntpdate -u ntp.aliyun.com`



## ip地址

格式：a.b.c.d

- abcd为0~255的数字



特殊IP：

- 127.0.0.1，表示本机
- 0.0.0.0
  - 可以表示本机
  - 也可以表示任意IP（看使用场景）



查看ip：`ifconfig`



## 主机名

功能：Linux系统的名称

查看：`hostname`

设置：`hostnamectl set-hostname 主机名`





## ps命令

功能：查看进程信息

语法：`ps -ef`，查看全部进程信息，可以搭配grep做过滤：`ps -ef | grep xxx`



## kill命令

![image-20221027221303037](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221303.png)



## nmap命令

![image-20221027221241123](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221241.png)



## netstat命令

功能：查看端口占用

用法：`netstat -anp | grep xxx`



## ping命令

测试网络是否联通

语法：`ping [-c num] 参数`

![image-20221027221129782](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221129.png)



## wget命令

![image-20221027221148964](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221149.png)

## curl命令

![image-20221027221201079](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221201.png)

![image-20221027221210518](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221210.png)



## top命令

功能：查看主机运行状态

语法：`top`，查看基础信息



可用选项：

![image-20221027221340729](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221340.png)



交互式模式中，可用快捷键：

![image-20221027221354137](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221354.png)



## df命令

查看磁盘占用

![image-20221027221413787](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221413.png)



## iostat命令

查看CPU、磁盘的相关信息

![image-20221027221439990](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221440.png)

![image-20221027221514237](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221514.png)



## sar命令

查看网络统计

![image-20221027221545822](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027221545.png)



## 环境变量

- 临时设置：export 变量名=变量值
- 永久设置：
  - 针对用户，设置用户HOME目录内：`.bashrc`文件
  - 针对全局，设置`/etc/profile`



### PATH变量

记录了执行程序的搜索路径

可以将自定义路径加入PATH内，实现自定义命令在任意地方均可执行的效果



## $符号

可以取出指定的环境变量的值

语法：`$变量名`

示例：

`echo $PATH`，输出PATH环境变量的值

`echo ${PATH}ABC`，输出PATH环境变量的值以及ABC

如果变量名和其它内容混淆在一起，可以使用${}









## su命令

切换用户

语法：`su [-] [用户]`

![image-20221027222021619](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222021.png)



## sudo命令

![image-20221027222035337](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222035.png)



比如：

```shell
itheima ALL=(ALL)       NOPASSWD: ALL
```

在visudo内配置如上内容，可以让itheima用户，无需密码直接使用`sudo`







## genenv命令

- `getenv group`，查看系统全部的用户组

  ![image-20221027222446514](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222446.png)

- `getenv passwd`，查看系统全部的用户

  ![image-20221027222512274](https://image-set.oss-cn-zhangjiakou.aliyuncs.com/img-out/2022/10/27/20221027222512.png)



## env命令

查看系统全部的环境变量

语法：`env`





























