## 第四章：Shell 编程
> 🏷️ **本章定位：⭐ 开发必会 —— 脚本自动化是开发、测试、部署的通用技能，整章精读多练**

### 0. 为什么要学 Shell 编程？日常到底用在哪？（了解）

一句话回答：**只要服务器还在 Linux 上跑，Shell 脚本就是你和机器最省时间的沟通方式。** 你不用把它当"第二门编程语言"去精通，但工作中**"重复操作 3 次以上就该写脚本"**的活，都要靠它。下面 3 个真实场景，脚本每一行都加了注释，看完就知道这一章学完能做什么。

---

#### 场景 1：部署脚本——把"敲 20 条命令"压成 1 条（最高频）

**痛点**：每次部署 Django/Node/Java 项目，都要手动 `git pull` → 装依赖 → 备份旧版本 → 重启服务，敲错一条就出事。

**Shell 化之后**：写一个 `deploy.sh`，一条命令搞定，可以反复执行（术语叫"幂等"——跑一次和跑十次效果一样）：

```bash
#!/bin/bash                                          # shebang：指定用 bash 解释这个脚本
set -e                                    # 任何一条命令失败立刻退出，不会继续往下闯祸
APP_DIR="/opt/myapp"                      # 定义变量：项目目录
BACKUP_DIR="/backup/$(date +%Y%m%d_%H%M%S)"   # 备份目录，$(date ...) 会换成当前时间戳

echo "==> 1/4 拉取最新代码"               # 打印进度提示
cd "$APP_DIR" && git pull origin main     # 进项目目录 && 拉代码（前一条成功才执行后一条）

echo "==> 2/4 安装依赖"
pip install -r requirements.txt           # 按清单装 Python 依赖

echo "==> 3/4 备份旧版本"
mkdir -p "$BACKUP_DIR"                    # -p：目录不存在才创建，已存在不报错
cp -r "$APP_DIR"/* "$BACKUP_DIR"/         # -r：把旧版本整个复制到备份目录

echo "==> 4/4 重启服务"
systemctl restart gunicorn                # 重启 Web 服务
journalctl -u gunicorn -n 10 --no-pager   # 看服务最近 10 行日志，确认启动正常
```

用的时候只敲一条：`sudo ./deploy.sh`

---

#### 场景 2：定时备份——让机器自己干活

**痛点**：数据库每天凌晨 2 点要备份一次，你不能天天定闹钟爬起来敲命令。

**Shell 化之后**：写个 `backup_db.sh`，交给系统的 `cron` 定时调度：

```bash
#!/bin/bash
set -e
DATE=$(date +%Y%m%d)                      # 取当天日期，如 20260915
BACKUP="/backup/db/myapp_$DATE.sql.gz"    # 备份文件名里带上日期
mysqldump -u root -p"xxx" myapp | gzip > "$BACKUP"   # 导出数据库，经管道直接压缩存盘

find /backup/db -name "myapp_*.sql.gz" -mtime +7 -delete   # 删掉 7 天前的旧备份

echo "$(date) 备份完成：$BACKUP" >> /var/log/backup.log    # 往日志文件追加一条记录
```

再用 `crontab -e` 写入一行 `0 2 * * * /opt/scripts/backup_db.sh`，系统就会每天凌晨 2 点自动跑（14 章细讲）。

---

#### 场景 3：批量处理文件——少敲 100 次

**痛点**：服务器上 `logs/` 目录里 100 个 `.log` 文件，每个都要加一行表头、改一下后缀。

**Shell 化之后**：用 for 循环，3 行搞定：

```bash
#!/bin/bash
for f in logs/*.log; do                                  # 依次取出每个 .log 文件名放进变量 f
    out="${f%.log}.txt"                                  # ${f%.log} 去掉 .log 后缀，再加 .txt
    echo "=== 日志开始 ===" > "$out"                     # > 先写一行表头（覆盖）
    cat "$f" >> "$out"                                   # >> 再把原日志内容追加进去
done
echo "处理完成，共 $(ls logs/*.log | wc -l) 个文件"      # $(...) 统计文件个数
```

---

### 0.1 学到什么程度算够？（了解）

| 目标 | 标准 |
|------|------|
| ❌ 不需要达到 | 像运维大神一样用 shell 写上千行复杂系统管理工具 |
| ❌ 不需要追求 | bash 的冷门特性——复杂逻辑请直接用 Python |
| ✅ 必须做到 | 看懂别人写的脚本、能改 2~3 行适配自己项目、能从零写出上面这种 20~30 行的小脚本 |
| ✅ 进阶可选 | 写函数复用、用 `set -euo pipefail` 让脚本更健壮、用 shellcheck 检查脚本 |

**一句话总结**：**复杂业务逻辑用 Python/Go，"把 Linux 命令串起来自动化"用 Shell。** 判断标准很简单：**如果脚本超过 100 行还在长大，就该换 Python 了。**

### 0.2 学习路径建议

```
1 节 快速入门（怎么写、怎么跑）          ← 先看
2~5 节 变量、环境变量、$1 参数、$? 返回值  ← 核心基础
6~8 节 运算、if/case 条件判断             ← 核心
9~11 节 for/while 循环、read 读输入       ← 核心，到这里能写上面 3 个场景
12~13 节 常用命令、函数、脚本调试          ← 进阶复用
14 节 定时备份实战（crontab + rsync）     ← 综合实战
```

---

## 1. Shell 编程快速入门（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 1.1 什么是 Shell（了解）

**一句话定义**：Shell 是你和 Linux 内核之间的"翻译官"——你敲命令，Shell 解释给内核听；内核干完活，Shell 把结果返回给你。

```
你（用户）  →  Shell（解释器）  →  Linux 内核  →  硬件
                 ↑
            你写的 Shell 脚本也是它一行一行解释执行的
```

Shell 有两层身份：
- **交互式工具**：你敲一行 `ls`，它解释一行（这就是你天天在终端干的事）
- **编程语言**：把一堆命令写进 `.sh` 文件，它从头到尾解释执行（这就是 Shell 脚本）

Linux 上有多种 Shell，**它们是不同的独立程序**，你只需要认识两个：

| Shell | 路径 | 说明 |
|-------|------|------|
| **bash** | `/bin/bash` | ⭐ **CentOS 默认**，功能强、文档全，99% 的脚本用它，本书也只讲它 |
| **sh** | `/bin/sh` | 最基础的 POSIX 标准 Shell；CentOS 里它是指向 bash 的软链接 |

> zsh、fish 等是终端美化向的 Shell，服务器上基本不装，不用花时间。

### 1.2 查看当前 Shell 与临时切换（必会）

```bash
echo $0                  # 打印当前正在用的 Shell：输出 bash 或 -bash（- 表示登录时启动的）
echo $SHELL              # 打印你"档案上的默认 Shell"（存在 /etc/passwd 里，临时切换不会改它）
cat /etc/shells          # 列出系统认可的所有合法 Shell

bash                     # 临时进入一个 bash（执行脚本排错时偶尔用） echo $SHLVL 记录**当前嵌套的 shell 层级**。
exit                     # 退回到原来的 Shell
```

**`bash` 临时进入是什么意思？（必会）**

> 🏷️ ⭐ 开发必会 —— 理解"shell 嵌套"，排错时常用

你当前已经在 bash 里了，再敲 `bash` 会发生什么？**在当前 shell 里再启动一个新的 bash 子进程**，就像俄罗斯套娃。

```
你登录服务器
  └── bash（第 1 层，登录 shell）
       └── 你敲 bash
            └── bash（第 2 层，子 shell）← 现在你在这里
                 └── 敲 exit → 退回第 1 层
```

**敲 `bash` 后你会进入一个"全新的 bash 环境"**：

- 它继承了第 1 层的环境变量
- 但你在这层里 `export` 的新变量、`cd` 的目录，**只在这层有效**，`exit` 后回到第 1 层全部失效

**什么时候用**（3 个真实场景）：

| 场景 | 为什么敲 `bash` |
|------|----------------|
| 脚本排错 | 脚本里某行报错，你想"逐行敲一遍"复现。敲 `bash` 进一个干净环境，把脚本命令一行行敲进去，看哪行炸了 |
| 测环境变量 | 改了 `~/.bashrc` 想测试，但不想影响当前会话。敲 `bash` 进新 shell 会重新加载 `.bashrc`，测完 `exit` 退出，当前会话不受影响 |
| 当前不是 bash | 比如你在 zsh/csh 里，想临时用 bash 的语法（如数组 `${arr[@]}`）测试一行命令 |

**演示：改 .bashrc 后用嵌套 bash 测试，不影响当前会话（必会）**

> 🏷️ ⭐ 开发必会 —— 照着敲一遍就懂了。全程 6 步，最后第 6 步证明当前会话没被污染。

**第 1 步：当前会话里确认 `MY_TEST` 变量不存在**

```bash
echo "MY_TEST=[$MY_TEST]"
# 输出：MY_TEST=[]   ← 空的，说明当前会话没有这个变量
```

**第 2 步：往 `~/.bashrc` 追加一行测试变量**

```bash
echo 'export MY_TEST="hello_from_bashrc"' >> ~/.bashrc

# 确认追加成功
tail -1 ~/.bashrc
# 输出：export MY_TEST="hello_from_bashrc"
```

**第 3 步：当前会话里再看一眼，还是空的（因为没 source）**

```bash
echo "MY_TEST=[$MY_TEST]"
# 输出：MY_TEST=[]   ← 仍然是空的！.bashrc 改了但当前会话不会自动重新加载
```

> 💡 这就是为什么改完 `.bashrc` 要么 `source ~/.bashrc`，要么重新登录。下面演示用 `bash` 进新 shell 来测试，比 source 更安全（不污染当前会话）。

**第 4 步：敲 `bash` 进新 shell，验证变量被加载了**

```bash
bash                          # 进入第 2 层 bash，启动时会自动 source ~/.bashrc
echo $SHLVL                   # 输出 2，证明在第 2 层
echo "MY_TEST=[$MY_TEST]"
# 输出：MY_TEST=[hello_from_bashrc]   ← 新 shell 加载了 .bashrc，变量有值了！
```

**第 5 步：在第 2 层里临时改点东西（演示这层的修改不会带出去）**

```bash
export MY_TEST="changed_in_subshell"
echo "MY_TEST=[$MY_TEST]"
# 输出：MY_TEST=[changed_in_subshell]   ← 这层里改了
```

**第 6 步：`exit` 退回第 1 层，验证当前会话完全没被影响**

```bash
exit                          # 退回第 1 层
echo $SHLVL                   # 输出 1，回到第 1 层
echo "MY_TEST=[$MY_TEST]"
# 输出：MY_TEST=[]   ← 还是空的！第 2 层的修改完全没带回来
```

**整个过程发生了什么**：

```
第1层 bash（当前会话）
  │  MY_TEST=[]（空）
  │
  ├─ 改 ~/.bashrc，追加 export MY_TEST=...
  │  （第1层不会自动重读，MY_TEST 还是空）
  │
  └─ 敲 bash → 第2层 bash（自动 source ~/.bashrc）
        MY_TEST=[hello_from_bashrc]
        └─ 改 MY_TEST=changed_in_subshell（只在这层有效）
        └─ exit → 回到第1层
  │
第1层 MY_TEST 仍然是 []（空），完全没被污染
```

**为什么这比 `source ~/.bashrc` 好**：

| 方式 | 影响当前会话 | 适用场景 |
|------|------------|---------|
| `source ~/.bashrc` | ✅ 会立刻加载到当前会话 | 你确定配置没问题，想马上生效 |
| `bash` 进新 shell 测 | ❌ 不影响当前会话 | 不确定配置对不对，**先在子 shell 里测，没问题再 source** |

> 🔑 **一句话记忆**：**改 `.bashrc` 后不确定对不对？先敲 `bash` 进去测，测完 `exit` 退出，当前会话干干净净。** 测没问题了再 `source ~/.bashrc` 让当前会话生效。
>
> ⚠️ 演示完别忘了把测试行删掉：`sed -i '/MY_TEST/d' ~/.bashrc`

**怎么证明是嵌套的**：

```bash
echo $SHLVL       # Shell 嵌套层数：第1层是1，敲bash后变2，再敲bash变3
# 在第1层：输出 1
bash
echo $SHLVL       # 输出 2
exit
echo $SHLVL       # 又回到 1
```

> ⚠️ **别敲太多层忘了退出**：连续敲 5 次 `bash` 就嵌套了 5 层，要敲 5 次 `exit` 才能退出来。看到提示符没变别以为没反应，其实已经进去了。

> 💡 日常**不需要切换默认 Shell**。服务器统一用 bash，脚本统一写 `#!/bin/bash`，最省心。

### 1.3 第一个 Shell 脚本（必会）

```bash
vi hello.sh              # 新建脚本文件
```

写入 3 行内容：

```bash
#!/bin/bash              # shebang：告诉系统用 /bin/bash 解释这个脚本（每个脚本第一行固定写它）
# 我的第一个 Shell 脚本   # # 开头是注释，不会执行
echo "Hello, World!"     # echo：往屏幕打印一行文字
```

然后赋权、运行：

```bash
chmod +x hello.sh        # 加可执行权限（不加只能用 bash hello.sh 跑）
./hello.sh               # 方式一：直接运行（./ 表示当前目录，不能省略）
bash hello.sh            # 方式二：让 bash 解释执行（不需要可执行权限）
```

> 💡 **不用 vim 也能建脚本**：命令行里直接用 `cat` 写文件，适合短脚本：
> ```bash
> cat > hello.sh << 'EOF'
> #!/bin/bash
> echo "Hello, World!"
> EOF
> ```
> `<< 'EOF'` 是 heredoc 语法（13.5 节细讲）：把两行 EOF 之间的内容喂给 cat，写入 hello.sh。`'EOF'` 加单引号是为了防止 `$变量` 被提前展开。回车到 `EOF` 那行就结束了。

> ⚠️ **最容易犯的错：`echo $0 > hello.sh`**
>
> 很多人以为这是在"创建脚本"，其实完全错了：
>
> ```bash
> echo $0 > hello.sh     # ❌ 错误！
> ```
>
> `$0` 是一个**变量**，代表"当前运行的脚本名"。你在终端里直接敲时，`$0` 的值是你当前 shell 的名字 `-bash`（登录 shell 前面带个 `-`）。所以这条命令把字符串 `-bash` 写进了 `hello.sh`。
>
> 然后执行就会报错：
>
> | 命令 | 报错 | 原因 |
> |------|------|------|
> | `bash hello.sh` / `sh hello.sh` | `-bash: 未找到命令` | 文件内容是 `-bash`，被当成命令执行，找不到这个命令 |
> | `./hello.sh` | `权限不够` | 文件没有可执行权限 |
> | `sudo ./hello.sh` | `找不到命令` | sudo 的 PATH 不含当前目录，且脚本没 x 权限 |
>
> **正确做法**：用 `vim hello.sh` 或 `cat > hello.sh << 'EOF'` 写入真正的脚本内容（第一行必须是 `#!/bin/bash`）。


### 1.4 脚本执行方式（必会）

一共有三种执行方式，区别在"要不要新开一个进程"：

```bash
./hello.sh               # 方式一：直接运行，新开一个子 shell 执行（要求有 shebang 和 x 权限）
bash hello.sh            # 方式二：显式让 bash 解释，也新开子 shell（不需要 x 权限）
sh hello.sh              # 方式二的变体：让 sh 解释（CentOS 7 上等价于 bash）
source hello.sh          # 方式三：在【当前 shell】里逐行执行（. hello.sh 是简写）
```

### 1.5 脚本规范（必会）

一个**最小规范脚本**长这样（3 行核心，照着抄就行）：

```bash
#!/bin/bash                    # ① 第一行：指定解释器

# 功能：一句话说明这个脚本干什么    # ② 开头写注释，方便以后看懂
# 作者：your_name

echo "Hello, World!"           # ③ 你的代码
```

**进阶：加一行严格模式（推荐，但不强求）**：

```bash
#!/bin/bash
set -e          # 任何命令失败就立即退出（默认会忽略错误继续跑，容易埋下隐患）
echo "Hello"
```

> 💡 `set -e` 就够日常用了。`-u`（未定义变量报错）和 `-o pipefail`（管道失败检测）是更严格的选项，等脚本写复杂了再加。调试时用 `set -x` 打印每条命令（13.6 讲）。

---

## 2. Shell 变量（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 2.1 变量基础（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

```bash
#!/bin/bash

# 定义变量：变量名=值，等号两边【不能有空格】（有空格会被当成命令）
name="Tom"               # 字符串用引号包起来
age=25                   # 数字不用引号
score=95.5

# 使用变量：变量名前加 $
echo $name               # Tom
echo ${name}             # Tom（加 {} 是为了和后面的文字隔开，如 ${name}_log.txt）
echo "姓名: ${name}, 年龄: ${age}"   # 双引号里可以直接放变量

# 把命令的执行结果赋给变量：用 $(命令)
current_dir=$(pwd)       # pwd 的输出（当前目录路径）存进变量
today=$(date '+%F')      # 今天的日期，如 2026-09-15
file_count=$(ls | wc -l) # 当前目录文件个数

echo "当前目录: $current_dir"
echo "今天日期: $today"
echo "文件个数: $file_count"

# ── 查：变量定义好后，怎么看它是不是环境变量 ──
# env 列出所有环境变量，grep ^name= 只看 name 这一行
# ^name= 表示"行首就是 name="，避免误匹配到 username、filename 等
env | grep ^name=        # 如果输出 name=Tom，说明 name 是环境变量；没输出就是普通变量

# ── 删：用完后怎么删除变量 ──
unset name               # 删除变量 name（之后 echo $name 就是空的）
echo "删除后 name 的值: [$name]"   # 验证：方括号里应该是空的
```

> 💡 这段可以直接复制保存为 `var_test.sh`，然后 `bash var_test.sh` 运行，所有注释都会保留、命令都会执行，方便你边看边跑。

### 2.2 变量命名规则（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握
- 只能包含字母、数字、下划线
- 不能以数字开头
- 不能使用保留字（if, then, fi, etc.）
- 区分大小写，默认为大写

```bash
# 有效命名
my_var
_var
VAR_123
myName
MY_NAME

# 无效命名
2name         # 不能以数字开头
my-var        # 不能有连字符
my var        # 不能有空格
if            # 保留字 关键字
```

### 2.3 变量类型（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

Shell 变量默认都是**字符串**，通过上下文区分：

```bash
# 字符串
str="hello world"

# 整数（使用算术运算）
num1=10
num2=20
result=$((num1 + num2))    # $(( )) 是 bash 算术运算语法，里面直接写 + - * / %（6.1 节细讲）
echo $result    # 30

# 数组（bash 支持）shell中的数组 ，和python中列表类似，列表有逗号
my_array=("apple" "banana" "cherry")
           0         1       2
echo ${my_array[0]}    # apple
echo ${my_array[@]}    # 所有元素
echo ${#my_array[@]}   # 数组长度
```

> 💡 **`@` 和 `#` 怎么记？**
>
> | 写法 | 含义 | 例子（数组 apple/banana/cherry） |
> |------|------|------|
> | `${my_array[0]}` | 第 0 个元素 | `apple` |
> | `${my_array[1]}` | 第 1 个元素 | `banana` |
> | `${my_array[@]}` | **所有元素** | `apple banana cherry` |
> | `${#my_array[@]}` | 所有元素的**个数**（`#` 是数数） | `3` |
>
> 两个符号分开记：
> - **`@`** = "全部元素"（类比"全班同学"）
> - **`#`** = "数一数有几个"（类比"全班人数"）
>
> 所以 `${arr[@]}` 是把全部元素列出来，`${#arr[@]}` 是数一下有几个。
>
> 还有个写法 `${arr[*]}`，不加引号时和 `@` 效果一样；但加双引号时有区别：
> ```bash
> arr=("a b" "c")           # 数组有 2 个元素："a b" 和 "c"
>
> echo "${arr[@]}"          # "a b" c   → 两个元素分开，循环时算 2 项
> echo "${arr[*]}"          # a b c     → 所有元素拼成一整串，循环时算 1 项
> ```
> 日常用 `@`（带引号 `"${arr[@]}"`）最安全，元素不会被空格拆散。
>
> **一句话记住**：`@` = 全部，`#` = 数数。

### 2.4 只读变量（必会，相当于其他语言的常量）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

> 🎯 **作用说明**：写脚本时有些值一旦定下来就不该再改（端口、配置路径、版本号、PI 这种数学常量）。用 `readonly` 把变量锁定成"只读"，防止后续代码不小心覆盖。

```bash
#!/bin/bash

# 定义只读变量（两种写法等价）
readonly PI=3.14                # 写法一：readonly
declare -r MAX_CONN=100         # 写法二：declare -r（-r = readonly）

echo "PI = $PI"                 # 正常使用：3.14
echo "MAX_CONN = $MAX_CONN"     # 100

# 试图修改 → 报错
PI=3.15                        # ❌ bash: PI: 只读变量
# 试图删除 → 也报错
unset PI                       # ❌ bash: declare: PI: 只读变量（无法删除）

# 只读变量只能等脚本结束（或 shell 退出）时自动销毁，中途不能改不能删
```

> 💡 **关于"静态变量"的说法**：
>
> shell 里**没有"静态变量"这个词**，对应的概念叫**"只读变量"**（readonly）。
>
> | 其他语言 | shell 对应 |
> |---------|-----------|
> | C/Java 的 `static`（类级别/持久化） | shell 没这个概念（脚本结束变量就没了） |
> | C/Java 的 `const` / `final`（不可变常量） | shell 的 `readonly` / `declare -r` |
>
> | 变量类型 | 能改值 | 能 `unset` 删除 | 谁能用 |
> |---------|--------|----------------|---------|
> | 普通变量 `x=1` | ✅ 能改 | ✅ 能删 | 当前 shell |
> | 环境变量 `export x=1` | ✅ 能改 | ✅ 能删 | 当前 shell + 子进程 |
> | 只读变量 `readonly x=1` | ❌ **不能改** | ❌ **不能删** | 当前 shell（退出自动销毁） |
>
> **记住一句话**：普通变量能改能删，只读变量不能改不能删，只能等脚本结束自动消失。

### 2.5 环境变量（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

> 🎯 **作用说明**：普通变量只在当前脚本/终端可见，环境变量（全局变量）可以被子进程、其他脚本读取。比如装 JDK 时配的 `JAVA_HOME`，所有 Java 程序都能读到。

#### 2.5.1 查看环境变量

```bash
# 查看所有环境变量
env

# 查看单个环境变量的值
echo $HOME           # 当前用户的家目录，如 /root
echo $USER           # 当前用户名
echo $PATH           # 命令搜索路径（第 3 节细讲）
echo $JAVA_HOME      # Java 安装目录（装 JDK 时自己配的）
echo $LANG           # 系统语言编码，如 zh_CN.UTF-8
echo $HOSTNAME       # 主机名
```

#### 2.5.2 设置环境变量的三个基本语法

| 语法 | 作用 | 说明 |
|------|------|------|
| `export 变量名=变量值` | 将普通变量输出为环境变量（全局变量） | 加了 `export`，子进程、其他脚本才能读到 |
| `source 配置文件` | 让修改后的配置信息立即生效 | 也叫"重新加载"，否则要重新登录才生效 |
| `echo $变量名` | 查询环境变量的值 | 和查普通变量一样 |

#### 2.5.3 完整案例：设置 Python 项目的环境变量

> ⚠️ 下面的命令会修改系统配置文件 `/etc/profile`，建议先在测试环境操作。

假设你有一个 Django 项目在 `/www/myproject`，用 Python 虚拟环境跑，给它配置环境变量：

```bash
# 第 1 步：在 /etc/profile 末尾添加项目环境变量
# tee 命令：把输入同时输出到屏幕和文件（-a = append 追加，不覆盖原文件）
# > /dev/null：把屏幕输出丢进"黑洞"（/dev/null 是个特殊设备，写进去的内容直接丢弃）
# << 'EOF'：heredoc 多行输入，到 EOF 那行结束（单引号让里面的 $ 不被提前解析）
sudo tee -a /etc/profile > /dev/null << 'EOF'
export PROJECT_DIR=/www/myproject                 # 项目根目录
export VENV_PATH=/www/myproject/venv              # Python 虚拟环境路径
export PATH=$VENV_PATH/bin:$PATH                  # 把虚拟环境的 bin 加到 PATH，这样直接敲 python/pip 就是虚拟环境的
EOF

# 第 2 步：让配置立即生效（不执行这一步，要重新登录才生效）
source /etc/profile

# 第 3 步：查看环境变量的值
echo $PROJECT_DIR          # /www/myproject
echo $VENV_PATH            # /www/myproject/venv
which python               # 应该指向 /www/myproject/venv/bin/python

# 第 4 步：验证环境变量能被子进程读到（说明是全局的）
# 敲 bash 回车，在当前终端里套娃开一个"新的 bash"（子进程）
# 提示符可能没变，但你已经进入第二层 shell 了
bash
# 在这个新的 bash 里敲 echo $PROJECT_DIR，能输出值说明环境变量传进来了
echo $PROJECT_DIR          # /www/myproject
# 敲 exit 回车，关掉第二层，回到原来的那个 shell
exit
```

> ⚠️ **以上是教学示例，实际企业开发不这么做！**
>
> 把项目配置写进 `/etc/profile` 有两个问题：
> 1. **不安全**：所有用户、所有进程都能读到（包括数据库密码、密钥）
> 2. **不灵活**：多个项目的配置混在一起，改一个可能影响另一个
>
> **企业实际做法：用 `.env` 文件管理项目配置**（开发和生产都用这个）：

```bash
# 第 1 步：在项目根目录创建 .env 文件（注意：前面有个点，是隐藏文件）
vim /www/myproject/.env

# 写入项目配置（格式就是 变量名=值，不需要 export）：
DB_HOST=127.0.0.1
DB_PORT=3306
DB_USER=myproject
DB_PASSWORD=你的数据库密码
SECRET_KEY=你的Django密钥
```

```bash
# 第 2 步：把 .env 加进 .gitignore，绝对不能提交到代码库（防止密钥泄露）
echo ".env" >> /www/myproject/.gitignore
```

```python
# 第 3 步：Django 里用 python-dotenv 读取（先 pip install python-dotenv）
# settings.py
from dotenv import load_dotenv
import os

load_dotenv()  # 自动读取同目录的 .env 文件，把里面的变量加载到环境变量

DATABASES = {
    'default': {
        'HOST': os.getenv('DB_HOST'),        # 读 .env 里的 DB_HOST
        'PORT': os.getenv('DB_PORT'),
        'USER': os.getenv('DB_USER'),
        'PASSWORD': os.getenv('DB_PASSWORD'),
    }
}
```

> 💡 **生产部署时**：在 systemd 的 `.service` 文件里用 `EnvironmentFile=/www/myproject/.env` 加载，不用改 `/etc/profile`。

#### 2.5.4 export 的作用：普通变量 vs 环境变量

```bash
#!/bin/bash

# 普通变量：只在当前 shell 可见，子进程读不到
NORMAL_VAR="hello"

# 环境变量：加了 export，子进程能读到
export GLOBAL_VAR="world"

# 当前 shell 两个都能看到
echo "普通变量: $NORMAL_VAR"     # hello
echo "环境变量: $GLOBAL_VAR"     # world

# 开一个子 shell 验证：子进程能不能读到这两个变量？
# 开子 shell 有两种方式：
#   方式 1：bash -c '命令'   → 启动 bash 执行引号里的命令，执行完自动退出（推荐，省事）
#   方式 2：直接敲 bash       → 进入交互式子 shell，手动敲命令验证，敲 exit 退出
# 这里用方式 1：
bash -c 'echo "子进程看到的普通变量: $NORMAL_VAR"; echo "子进程看到的环境变量: $GLOBAL_VAR"'
# 输出：
# 子进程看到的普通变量:        （空！子进程读不到普通变量）
# 子进程看到的环境变量: world  （能读到）
```

> 💡 关于"临时设置 vs 永久设置"（改哪个文件、什么时候生效）已在第三章 7.3 节详细讲过，这里不重复。一句话：临时用 `export`，永久写 `~/.bashrc` 或 `/etc/profile` 后 `source`。


### 2.6 变量引用区别（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

```bash
# 单引号：原样输出，单引号里的 $ 不当变量解析
name="Tom"              # 先定义变量
echo '$name'            # 输出: $name（$ 被当成普通字符）

# 双引号：会解析变量和 $(命令)
echo "$name"            # 输出: Tom
echo "今天是 $(date +%F)"   # 输出: 今天是 2026-09-15

# 反引号：也表示执行命令（老写法，不推荐）
echo `date`             # 效果同 $(date)，但反引号不能嵌套、看着也容易混

# $()：命令替换（推荐写法，可以嵌套）
echo $(date)            # 输出当前日期时间

# 把命令结果【赋值给变量】（两种写法等价，推荐 $()）
A=`date`                # 反引号：把 date 的输出存进变量 A
B=$(date)               # $()：和反引号等价，推荐写法（可嵌套、不易看错）
echo "A = $A"
echo "B = $B"

# 带参数的命令也行，加不加空格效果一样
today=$(date '+%F')     # 今天日期，如 2026-09-21
count=$(ls | wc -l)     # 当前目录文件个数
```

> 🔑 记住：**双引号解析、单引号不解析、命令结果用 `$()`**。反引号是老写法，现在统一用 `$()`。

### 2.7 变量默认值（必会）

写脚本时经常要处理"参数没传、变量可能为空"的情况，只需要记住这两个写法：

```bash
# ${var:-默认值}：var 为空或没定义时，临时使用默认值（不改 var 本身）
name=""                          # name 是空的
echo "${name:-Unknown}"          # 输出: Unknown
echo "$name"                     # 还是空（:- 只在这一行临时顶替）

# ${var:=默认值}：var 为空时，把默认值【赋给】它（var 本身被改写）
: ${DB_HOST:=localhost}          # 注意开头的 : 是占位符，整行只赋值、不输出，如果**没有前面的冒号**，只   `${DB_HOST:=localhost}`：
                                 #   bash 会**把结果打印输出到屏幕**，很多脚本不希望输出多余文字，所以前面加 `:` 吃掉输出，只做赋值。
echo "$DB_HOST"                  # localhost：以后直接用 $DB_HOST 即可
- 情况①：**之前没有定义 DB_HOST** → 自动执行 `DB_HOST=localhost`
- 情况②：**DB_HOST="" 空字符串** → 自动执行 `DB_HOST=localhost`
- 情况③：**DB_HOST="127.0.0.1" / "mysql 容器 IP" 已经有值** → 啥也不改，保留原来的值
                                                                                                             # 场景1：DB_HOST没有定义
: ${DB_HOST:=localhost}
echo "$DB_HOST"   # 输出 localhost

# 场景2：提前设置DB_HOST=192.168.1.100
DB_HOST="192.168.1.100"
: ${DB_HOST:=localhost}
echo "$DB_HOST"   # 输出 192.168.1.100，不会被覆盖！

# 场景3：DB_HOST是空
DB_HOST=""
: ${DB_HOST:=localhost}
echo "$DB_HOST"   # 输出 localhost
                                                                                                                                                                                                                                                                            
# 最常见用法：脚本开头给参数兜底
# 拆解：${ 1 :- /data/backup }
#   $1            = 脚本收到的第 1 个位置参数
#   :-            = "为空就用后面的默认值"（冒号+横杠，固定语法）
#   /data/backup  = 默认值
# 整体意思：$1 有值就用 $1，$1 没值（没传参数）就用 /data/backup
BACKUP_DIR="${1:-/data/backup}"  #$1位置参数
# 举例：
#   ./backup.sh /tmp/logs   → BACKUP_DIR="/tmp/logs"（用了传的参数）
#   ./backup.sh             → BACKUP_DIR="/data/backup"（没传参数，用默认值）
```

> 💡 其余 `+`、`?` 等变体开发很少用，需要时再查手册。字符串截取、文件名取后缀等操作统一放在 6.2 节讲。

### 2.8 数组（必会）

#### 2.8.1 普通索引数组
```bash
# 定义数组：用括号，元素之间用空格
arr=("apple" "banana" "cherry")

# 读取
echo ${arr[0]}               # apple（下标从 0 开始）
echo ${arr[@]}               # apple banana cherry（@ 表示所有元素，遍历必用）
echo ${#arr[@]}              # 3（# 表示元素个数）

# 追加元素
arr+=("date")                # += 追加：在数组末尾再加一个元素

# 删除
unset arr[1]                 # 删除下标 1 的元素（其他元素下标不变）

# 遍历（最常用，"${arr[@]}" 加双引号才能正确处理带空格的元素）
for item in "${arr[@]}"; do
    echo "$item"
done
```

#### 2.8.2 关联数组（了解，"键→值"映射，bash 4+）

普通数组用 0、1、2 做下标，关联数组可以用自定义的名字做下标（相当于其他语言的 Map/字典）。开发用得少，认识这个写法即可：

```bash
declare -A server=(                          # declare -A：声明关联数组（必须先声明）
    [web1]="192.168.1.10"                    # 键 web1 → 值 IP
    [web2]="192.168.1.11"
)
echo "${server[web1]}"                       # 192.168.1.10：按键取值

for name in "${!server[@]}"; do              # ${!server[@]} 取所有键（! 表示取"键名"而非"值"），${server[@]} 取所有值
    echo "$name -> ${server[$name]}"
done
```

> 💡 如果脚本里真要做复杂的数据统计，直接用 Python 更合适，不要在 bash 里硬写。

### 2.9 变量作用域（local 关键字）（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

```bash
#!/bin/bash
# 全局变量（函数内未用 local 声明的都是全局！）
counter=0                    # 【全局变量】定义在脚本顶层，整个shell环境可见
increment() {
    counter=$((counter + 1))   # 没有 local，直接操作**全局counter**
}
safe_increment() {
    local counter=0           # local关键字：声明【函数局部变量】
    counter=$((counter + 1))  # 这里修改的是函数内部自己的counter，和外面全局counter完全是两个变量
    echo "函数内 counter: $counter"
}

increment   # 全局counter = 0+1 =1
increment   # 全局counter =1+1=2
echo "全局 counter: $counter"          # 输出 2

safe_increment # 创建局部counter，初始0→1，打印函数内counter=1；函数结束，局部变量销毁
safe_increment # 再次新建局部counter，初始0→1，打印函数内counter=1；函数结束销毁
echo "全局 counter 还是: $counter"     # 全局变量没有被修改，仍然是2
```

**坑：忘了 local 会污染全局变量**，写函数时养成 `local var=...` 习惯，就是说Bash 函数里，如果变量不加 `local`，这个变量就是全局的，会把脚本外面同名变量的值改掉，造成意外 bug；所以函数内部定义变量，一律加上 `local`。

### 2.10 字符串引号使用陷阱（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

```bash
# 变量赋值时空格必须加引号
name="Tom Hanks"              # ✓ 正确
name=Tom Hanks                # ✗ 错：会被解析为 name=Tom 后执行 Hanks 命令

# 引用变量必须加双引号（防止分词和通配）
files="*.txt"
echo $files                   # 会展开当前目录的 txt 文件（潜在 bug）
echo "$files"                 # 输出 *.txt（预期行为）


注：echo $HOME中，`$HOME`：变量里面存的是**普通文本，没有 `* ? []` 这类 glob 符号**，不加双引号也不会触发通配符，所以一般没事。
1. 变量替换 → `/home/zhangsan`
2. 字符串没有`*`，glob 无事可做，直接传给 echo
👉 结果和`echo "$HOME"`一样。

⚠️ 但是！**$HOME 不加引号也不是永远安全！**
如果你的用户名里**包含空格**（比如 `/home/my home`）：
HOME="/home/my home"
echo $HOME     # 无引号：单词分割！变成两个参数 /home/my 和 home
echo "$HOME"   # 双引号：完整保留空格，一个参数
这就是**单词分割**坑，和 glob 是两个坑。

# 路径含空格的坑（最常见）
path="/home/user/My Documents"

# 错误：分词
ls $path                      # ls 试图列 /home/user/My 和 Documents
cp $path /tmp/                # 复制两个不存在的文件

# 正确：加双引号
ls "$path"
cp "$path" /tmp/

# 在 bash 中比较字符串必须加双引号
str="hello"
[ $str = "hello" ]           # OK，但 $str 为空时报错 不推荐
[ "$str" = "hello" ]          # ✓ 安全写法
[[ $str == "hello" ]]         # ✓ [[ ]] 不需要双引号（推荐）
```

> 🔑 一句话防坑：**凡是引用变量，全部加双引号** `"$var"`，能避开 90% 的空格/分词问题。单双引号的区别在 2.5 节，这里不再重复。

---

## 3. 设置环境变量（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 3.1 临时环境变量（必会）

```bash
export MY_VAR="hello"                # export：导出变量，当前终端 + 它启动的子进程都能读到
echo $MY_VAR                         # 验证：输出 hello
unset MY_VAR                         # 取消

MY_VAR="hello" mycommand             # 只给【这一条命令】临时带一个变量，命令结束就没了
```

> 💡 直接 `MY_VAR="hello"` 不加 export，只在当前 shell 可见，子进程（比如脚本里启动的程序）读不到。

### 3.2 用户级永久变量（必会）

只对当前用户永久生效，写进 `~/.bashrc`（第 1 节讲过，非登录 shell 也会读它，最不容易踩坑）：

```bash
vi ~/.bashrc                         # 打开当前用户的 bashrc
# 在文件末尾加入：
export MY_VAR="hello"
export JAVA_HOME=/usr/local/java/jdk8          # 告诉系统 JDK 装在哪
export PATH=$JAVA_HOME/bin:$PATH               # 把 java 命令所在目录加到 PATH（3.4 讲）

source ~/.bashrc                     # 重新加载，立即生效（不用重新登录）
```

### 3.3 系统级永久变量（必会）

要让**所有用户**都生效，放在 `/etc/profile.d/` 目录下（系统启动时会自动加载这个目录里所有 `.sh`）：

```bash
sudo vi /etc/profile.d/myapp.sh      # 文件名随意，.sh 结尾即可
# 内容：
export MY_VAR="hello"
export JAVA_HOME=/usr/local/java/jdk8

source /etc/profile.d/myapp.sh       # 立即生效；新登录的用户会自动加载
```

> 💡 这是安装 JDK/Maven 等开发环境时的标准做法。

### 3.4 PATH 变量（必会）

`PATH` 是一串用冒号分隔的目录。你敲一个命令时，系统**从左到右**在这些目录里找同名程序，找不到就报 `command not found`。

```bash
echo $PATH                           # 查看，形如 /usr/local/bin:/usr/bin:/bin:/usr/local/sbin

# 把自己的程序目录加到 PATH【最前面】（前面的目录优先找到）
export PATH=/usr/local/myapp/bin:$PATH

# 永久生效：写进 ~/.bashrc 或 /etc/profile.d/myapp.sh
```

**为什么装了 java 还要配 PATH？** 因为 `java` 程序在 `/usr/local/java/jdk8/bin` 里，这个目录不在默认 PATH 中，系统找不到；把它加进 PATH 后，在任何目录下都能直接敲 `java`。

### 3.5 配置文件加载顺序（了解）

```
登录 shell（SSH 登录、su - 用户）：
  /etc/profile → /etc/profile.d/*.sh → ~/.bash_profile

非登录 shell（已登录后开终端、跑脚本）：
  ~/.bashrc
```

> 🔑 实战结论：个人配置统一写 `~/.bashrc`，全机配置统一放 `/etc/profile.d/xxx.sh`，其他文件不要动。

---

## 4. 位置参数变量（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 4.1 位置参数（必会）

运行脚本时在命令后面跟的内容，就是"位置参数"，按顺序用 `$1`、`$2`…… 读取：

```bash
#!/bin/bash
# 假设这样运行：./test.sh arg1 arg2 arg3
echo "脚本名: $0"          # ./test.sh        —— $0 永远是脚本自己的名字
echo "第一个参数: $1"      # arg1
echo "第二个参数: $2"      # arg2
echo "参数个数: $#"        # 3                —— 传进来的参数有几个
echo "所有参数: $@"        # arg1 arg2 arg3
```

> 💡 参数超过 9 个时要加花括号：`${10}`、`${11}`（`$10` 会被理解成 `$1` 后面跟个 0）。

### 4.2 shift：逐个吃掉参数（了解）

```bash
#!/bin/bash
# shift：把参数整体左移一格——原来的 $1 丢弃，$2 变成新的 $1
while [ $# -gt 0 ]; do    # $# 是参数个数；-gt 0 表示"大于 0"，还有参数就继续循环
    echo "处理参数: $1"
    shift                 # 吃掉当前的 $1，参数少一个
done
```

### 4.3 "$@" 的正确用法（了解）

循环处理全部参数时，**固定写 `for arg in "$@"`**（加双引号），即使参数里带空格也不会被拆错：

```bash
for arg in "$@"; do       # 每个参数循环一次，带空格也完整
    echo "处理: $arg"
done
```

> 不用纠结 `$*` 和 `$@` 的理论区别——记住"遍历参数永远 `"$@"`"就够了。

---

## 5. 预定义变量（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 5.1 常用预定义变量（必会）

| 变量 | 说明 |
|------|------|
| `$0` | 脚本自己的名字 |
| `$1`~`$9`、`${10}`… | 位置参数（第 4 节） |
| `$#` | 位置参数的个数 |
| `$@` | 所有位置参数（遍历用 `"$@"`） |
| `$$` | 当前脚本/Shell 的进程号 PID |
| `$!` | 最近一个后台进程的 PID |
| `$?` | **上一条命令的返回值：0 = 成功，非 0 = 失败**（脚本里用得最多） |

### 5.2 返回值示例（必会）

写一个脚本 `preVar.sh`，把常用特殊变量都打印出来：

```bash
#!/bin/bash

echo "当前执行的进程 id=$$"           # $$ ：本脚本的进程号

sleep 10 &                            # & 让命令在后台运行
echo "最后一个后台方式运行的进程 id=$!"   # $! ：最后一个后台进程的 PID

ls /tmp > /dev/null                   # 随便执行一条命令
echo "执行的结果是=$?"                 # $? ：上一条命令的返回值（0=成功）

echo "0=$0 1=$1 2=$2"                 # $0 脚本名，$1 $2 第1、2个参数
echo "所有的参数=$*"                   # $* ：所有参数
echo "参数的个数=$#"                   # $# ：参数个数
```

运行（不传参数）：

```bash
./preVar.sh
# 当前执行的进程 id=16392
# 最后一个后台方式运行的进程 id=16393
# 执行的结果是=0
# 0=/root/shcode/preVar.sh 1= 2=        ← $1 $2 为空（没传参数）
# 所有的参数=
# 参数的个数=0
```

运行（传参数）：

```bash
./preVar.sh hello world
# 0=/root/shcode/preVar.sh 1=hello 2=world
# 所有的参数=hello world
# 参数的个数=2
```

> 🔑 日常记住三个：**`$?` 判成败、`$#` 数参数、`$1` 取参数**。

---

## 6. 运算符（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 6.1 算术运算（开发岗只需 `$(( ))`）（必会）
> 🏷️ ⭐ 开发必会 —— 只记 `$((  ))` 一种，覆盖 95% 场景

```bash
a=10
b=3
echo $((a + b))       # 13  加
echo $((a - b))       # 7   减
echo $((a * b))       # 30  乘
echo $((a / b))       # 3   除（只取整数部分，10/3=3）
echo $((a % b))       # 1   取余
echo $((a ** b))      # 1000 幂
echo $(( (a + b) * 2 ))   # 26  支持括号
```

> 💡 **只记 `$((  ))`**：bash 内置、不需要外部命令、性能最好。其他方式（`expr`、`bc`、`awk`）需要时再查——`bc` 是 Linux 自带的命令行计算器（支持小数），但 bash 本身**只能算整数**，要算小数用 `echo "scale=2; 10/3" | bc`（scale=2 保留两位小数，结果 3.33）。

### 6.2 字符串运算（5 个必会操作）（必会）
> 🏷️ ⭐ 开发必会 —— 截取/长度/替换，开发常用来处理路径和文件名

```bash
str="HelloWorld"

# ① 求长度
echo ${#str}               # 10

# ② 截取（从第 0 位开始取 5 个字符）
# 语法：${变量:起始位置:长度}  ← 第二个数是【长度】，不是结束位置
echo ${str:0:5}            # Hello（从0开始取5个：H,e,l,l,o）
echo ${str:5}              # World（只有一个数字时=起始位置，从第5位开始取到结尾）

# ③ 替换（结合参数扩展，文件改名用得上）
# 以 file="report.tar.gz" 为例，字符串里有 2 个点：
#
#   r e p o r t . t a r . g z
#   0 1 2 3 4 5 6 7 8 9 10 11 12
#               ↑         ↑
#            第1个点    第2个点
#
# 【最短 vs 最长】核心区别：
#   最短 = 找到离"起始方向"最近的那个匹配就停
#   最长 = 一直找到离"起始方向"最远的那个匹配
#
# ── 从尾部删 % 和 %%（从右往左找）──
#
#   r e p o r t . t a r . g z
#   ←←←←←←←←←←←←←←←←←←←←←←
#        从尾部(右)往左找 "."
#
# %  最短：从右往左找，遇到第1个"."就停 → 删 ".gz"，剩 "report.tar"
echo ${file%.*}            # report.tar
#
# %% 最长：从右往左找，一直找到最远（最左）的"." → 删 ".tar.gz"，剩 "report"
echo ${file%%.*}           # report
#
# ── 从头部删 # 和 ##（从左往右找）──
#
#   r e p o r t . t a r . g z
#   →→→→→→→→→→→→→→→→→→→→→→
#        从头部(左)往右找 "."
#
# #  最短：从左往右找，遇到第1个"."就停 → 删 "report."，剩 "tar.gz"
echo ${file#*.}            # tar.gz
#
# ## 最长：从左往右找，一直找到最远（最右）的"." → 删 "report.tar."，剩 "gz"
echo ${file##*.}           # gz
```

**记忆口诀**：
- `#`：从头删（# 像左边）
- `%`：从尾删（% 像右边）
- 单个`# / %`：最短匹配（找到第一个就停）
- 双`## / %%`：最长匹配（一直找到最远的）

**最常用的两个**：

- `${file##*.}` 取文件扩展名（gz）
- `${file%.*}` 去掉扩展名（report.tar）

> 💡 这 4 个 `%`/`#` 操作是 Shell 处理文件后缀的核心，比 sed 简单。**`%` 在后 → 从尾部删；`#` 在前 → 从头部删**；一个最短、两个最长。

**处理路径（脚本里切文件/目录名极高频）：**

```bash
path="/data/logs/app.log"

echo "${path##*/}"        # app.log    —— ##*/ 删掉最后一个 / 及其前面的全部（取文件名）
echo "${path%/*}"         # /data/logs —— %/*  删掉最后一个 / 及其后面的全部（取目录）
basename "$path"          # app.log    —— 外部命令，效果同上
dirname "$path"           # /data/logs
```

### 6.3 比较运算符（条件判断的核心）（必会）
> 🏷️ ⭐ 开发必会 —— `[ ]` 里写条件判断

> 📌 **怎么跑这些例子？** 两种方式都行：
> - **方式 A（推荐，最快）**：直接在终端逐行敲，每敲一行回车就能看到结果
> - **方式 B**：新建 `compare.sh`，把代码贴进去，`bash compare.sh` 运行
>
> 下面例子里的 `&& echo "..." || echo "..."` 是"成功就打印前面，失败就打印后面"，方便你一眼看到判断结果。

**数值比较**（只记这 6 个，字母缩写 e=equal/g=greater/l=less）：
```bash
a=10
b=5
# -eq = equal（等于）
[ $a -eq $b ] && echo "相等" || echo "不等"       # 不等（10不等于5）

# -ne = not equal（不等于）
[ $a -ne $b ] && echo "不等" || echo "相等"       # 不等（10不等于5，成立）

# -gt = greater than（大于）
[ $a -gt $b ] && echo "a大于b" || echo "a不大于b"  # a大于b（10>5，成立）

# -lt = less than（小于）
[ $a -lt $b ] && echo "a小于b" || echo "a不小于b"  # a不小于b（10<5不成立）

# -ge = greater or equal（大于等于）
[ $a -ge 10 ] && echo "a>=10" || echo "a<10"      # a>=10（10>=10，成立）

# -le = less or equal（小于等于）
[ $a -le 10 ] && echo "a<=10" || echo "a>10"      # a<=10（10<=10，成立）
```

**字符串比较：**
```bash
str1="hello"
str2="world"
str3=""

# = 字符串相等（注意是单等号 =，不是 ==）
[ "$str1" = "$str2" ] && echo "相等" || echo "不等"     # 不等

# != 字符串不等
[ "$str1" != "$str2" ] && echo "不等" || echo "相等"    # 不等

# -z = zero length（字符串长度为0，即空）
[ -z "$str3" ] && echo "str3为空" || echo "str3非空"    # str3为空

# -n = non-zero length（字符串长度不为0，即非空）
[ -n "$str1" ] && echo "str1非空" || echo "str1为空"    # str1非空
```

**文件测试（最常用 4 个）：**
```bash
# -f = file（存在且是普通文件）
[ -f /etc/passwd ] && echo "是普通文件" || echo "不是"    # 是普通文件

# -d = directory（存在且是目录）
[ -d /etc ] && echo "/etc是目录" || echo "不是目录"       # /etc是目录

# -e = exists（存在，不区分文件还是目录）
[ -e /etc/passwd ] && echo "文件存在" || echo "不存在"    # 文件存在

# -r = readable（可读）
[ -r /etc/passwd ] && echo "可读" || echo "不可读"        # 可读

# -w = writable（可写）
[ -w /etc/passwd ] && echo "可写" || echo "不可写"        # 不可写（普通用户）

# -x = executable（可执行）
[ -x /usr/bin/ls ] && echo "可执行" || echo "不可执行"     # 可执行
```

**逻辑运算（与/或/非）：**
```bash
a=7

# && = and（与）：两个条件都成立才成立
[ $a -gt 5 ] && [ $a -lt 10 ] && echo "5<a<10" || echo "不在范围"   # 5<a<10

# || = or（或）：只要有一个成立就成立
[ $a -lt 3 ] || [ $a -gt 8 ] && echo "a<3或a>8" || echo "3<=a<=8"   # 3<=a<=8

# ! = not（非）：取反
! [ $a -eq 0 ] && echo "a不等于0" || echo "a等于0"                   # a不等于0
```

> ⚠️ **新手最容易踩的坑**：`[ ]` 内变量两边一定要加双引号 `[ "$var" = "x" ]`，否则变量为空时会报 `unary operator expected`。

---

## 7. 条件判断（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 7.1 test 与 [ ] 与 [[ ]] 的区别（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

**三者其实是一回事，只是写法不同：**

```bash
# ① test 命令：最原始的写法
test -f /etc/passwd && echo "文件存在"
# ↑ test 是一个命令，-f /etc/passwd 是它的参数
#   测试 /etc/passwd 是否存在且是普通文件，是就打印"文件存在"

# ② [ ] ：test 的简写（[ 其实就是 test 命令的别名）语法糖
[ -f /etc/passwd ] && echo "文件存在"
# ↑ [ -f /etc/passwd ] 完全等价于 test -f /etc/passwd
#   注意 [ 后面和 ] 前面都要有空格！

# ③ [[ ]] ：bash 增强版，推荐用这个
[[ -f /etc/passwd ]] && echo "文件存在"
# ↑ [[ ]] 比 [ ] 更强大，支持通配符匹配、正则匹配、不需要给变量加引号

# [[ ]] 的特殊能力：通配符匹配 ==
[[ "hello" == "h*" ]] && echo "匹配通配符"   
# ↑ "h*" 里的 * 是通配符，匹配 h 开头的任意字符串   注意变量不需要双引号（推荐）
#   "hello" 以 h 开头，所以匹配成功，打印"匹配通配符"
#   注意：[ ] 不支持这个写法，只有 [[ ]] 支持
```

#### 7.1.1 [ ] vs [[ ]] 全面对比
| 特性 | `[ ]` (test) | `[[ ]]` (bash 扩展) |
|------|------------|---------------------|
| 标准 | POSIX 标准，跨 shell 兼容 | bash/ksh/zsh 扩展 |
| 变量加引号 | **必须** `["$x" = "$y"]` | 不需要 `[[$x == $y]]` |
| 通配符匹配 `==` | ❌ 不支持 | ✅ `[[ $x == h* ]]` |
| 正则匹配 `=~` | ❌ 不支持 | ✅ `[[ $x =~ ^[0-9]+$ ]]` |
| 逻辑与/或 | `[ a ] && [ b ]` | `[[ a && b ]]` 一行搞定 |
| `<` `>` 比较 | 需转义 `\[ $a \< $b \]` | 直接 `[[ $a < $b ]]` |
| 算术比较 | `[ $a -lt $b ]` | `[[ $a -lt $b ]]` 或 `(( ))` |

**坑：`[ ]` 中变量未加引号且为空会报语法错误**
```bash
str=""
[ $str = "hello" ]         # 报错：unary operator expected
                              # 因展开成 [ = "hello" ]
[ "$str" = "hello" ]        # ✓ 正确：[ "" = "hello" ]
[[ $str = "hello" ]]         # ✓ [[ ]] 不需要加引号（推荐）

# 推荐结论：
# - 写 bash 脚本，判断用 [[ ]]
# - 写 POSIX 兼容脚本，用 [ ]，且变量必须加双引号
```

#### 7.1.2 [[ ]] 通配符匹配
```bash
# == 右边可以写通配符（* 匹配任意字符），不用写正则就能判断后缀
file="report.csv"
[[ $file == *.csv ]] && echo "是 CSV 文件"      # 匹配，打印

# 多条件组合：&& 表示"并且"，|| 表示"或者"
age=25
if [[ $age -ge 18 && $age -le 60 ]]; then       # -ge 18 且 -le 60
    echo "适龄劳动"
fi

status="pending"
if [[ $status == "active" || $status == "pending" ]]; then
    echo "处理中"
fi
```

#### 7.1.3 (( )) 算术判断（数字场景最佳）
```bash
# 数字比较用 (( ))，里面直接写 > < 等符号，不用 -gt/-lt，更直观
n=5
(( n > 0 )) && echo "正数"
(( n % 2 == 0 )) && echo "偶数"                  # % 取余，余数为 0 即偶数
(( n >= 1 && n <= 100 ))                        # 范围判断
(( n++ ))                                       # 自增 1（循环计数用）
```

### 7.1.4 退出码与 set -euo pipefail（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

#### 一、退出码：每条命令都有一个"执行结果"

```bash
# 退出码规则：0 = 成功，非 0 = 失败
ls /etc/passwd           # 文件存在，执行成功
echo $?                  # 0  （$? = 上一条命令的退出码）

ls /nonexistent          # 文件不存在
echo $?                  # 非 0（ls 失败返回 2）

# 常见退出码含义：
#   0   = 成功
#   1   = 通用失败（脚本里校验不通过时最常用）
#   2   = 用法错误（比如命令参数不对）
#   126 = 命令存在但不可执行
#   127 = 命令未找到
#   130 = 被 Ctrl+C 中断（128+2，2 是中断信号）
```

> 💡 **`$?` 是什么**：bash 内置变量，保存"上一条命令的退出码"。每条命令执行完，bash 都会记下它是成功（0）还是失败（非 0）。

#### 二、脚本里主动用 exit 返回退出码

```bash
#!/bin/bash
# 保存为 exit_demo.sh
if [ ! -f "/etc/passwd" ]; then
    echo "错误：文件不存在"
    exit 1                  # 主动退出，退出码 1（失败）
fi
echo "文件存在"
exit 0                      # 主动退出，退出码 0（成功）
```
```bash
chmod +x exit_demo.sh
./exit_demo.sh
echo $?                      # 0（脚本成功结束）
```

#### 三、set -euo pipefail：脚本的"严格模式"

**不加的问题**：默认情况下，脚本里某条命令失败了，bash 会**忽略错误继续往下跑**，导致后面的操作在错误的状态下执行，产生更严重的问题。

**加了的效果**：让脚本"遇错即停"，避免错误扩散。

逐个拆解：

##### 1. `set -e`：任何命令失败立刻退出

```bash
# ❌ 不加 -e：mkdir 失败了（目录已存在），脚本继续往下跑，可能误操作
#!/bin/bash
mkdir /data/backup           # 假设这个目录已存在，mkdir 失败，退出码非 0
rm -rf /data/backup/*        # 还是会执行！可能误删文件
echo "完成"

# ✅ 加 -e：mkdir 失败立刻退出，不会执行后面的 rm
#!/bin/bash
set -e
mkdir /data/backup           # 失败 → 脚本立刻退出，后面的 rm 不会执行
rm -rf /data/backup/*
echo "完成"
```

##### 2. `set -u`：引用未定义变量立即报错

```bash
# ❌ 不加 -u：变量名拼错了，bash 静默当成空串，不会报错
#!/bin/bash
BACKUP_DIR="/data/backup"
mkdir -p "$BACKUP_DIRR"      # 拼错了（多了个 R），$BACKUP_DIRR 是空，变成 mkdir -p "" → 报错或建错地方
echo "完成"

# ✅ 加 -u：变量未定义直接报错退出
#!/bin/bash
set -u
BACKUP_DIR="/data/backup"
mkdir -p "$BACKUP_DIRR"      # 报错：BACKUP_DIRR: unbound variable，脚本退出
echo "完成"
```

##### 3. `set -o pipefail`：管道中任一命令失败，整条管道算失败

```bash
# ❌ 不加 pipefail：管道只看最后一条命令的退出码
#!/bin/bash
cat /nonexistent | grep "hello"   # cat 失败了，但 grep 成功（没匹配退出码 1？其实 grep 没匹配也是非 0）
echo $?                             # 可能是 0，你以为成功了，其实 cat 失败了

# 举个更清楚的例子：
ls /nonexistent | wc -l             # ls 失败，但 wc -l 成功（输出 0）
echo $?                             # 0（只看 wc -l 的退出码），你以为成功了

# ✅ 加 pipefail：管道中任一命令失败，整条管道退出码非 0
#!/bin/bash
set -o pipefail
ls /nonexistent | wc -l
echo $?                             # 非 0（ls 失败被捕获到了）
```

##### 4. 推荐写法：三行合一

```bash
#!/bin/bash
set -euo pipefail   # 等价于：
                    #   set -e  （命令失败即退出）
                    #   set -u  （未定义变量报错）
                    #   set -o pipefail  （管道任一失败即失败）

# 调试时再加 -x，把每条执行的命令打印出来（13.6 讲）
# set -euxo pipefail
```

> ⚠️ `set -e` 有个常见例外：命令出现在 `if`、`while` 的条件里，或在 `&&`/`||` 链中时，失败不会导致退出（因为这些位置本来就是"故意拿来判断"的）。
> ```bash
> set -e
> if ls /nonexistent; then    # ls 失败不会退出脚本，因为它在 if 条件里
>     echo "存在"
> fi
> ```

### 7.2 if 语句（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

> 📌 **语法拆解**（以 `if [ -f /etc/passwd ]; then ... fi` 为例）：
>
> | 部分 | 含义 |
> |------|------|
> | `if` | 开始条件判断 |
> | `[ -f /etc/passwd ]` | **`[` 就是 `test` 命令**（7.1 讲过），`-f /etc/passwd` 是它的参数，测试文件是否存在。`[` 后面和 `]` 前面**必须有空格** |
> | `;` | 分隔符，把 `if` 和 `then` 写在同一行时必须加分号（换行可以不加分号） |
> | `then` | 条件成立（返回值为 0）时，执行下面的命令 |
> | `fi` | **if 的结束标志**（就是 `if` 倒过来写），必须有，否则语法错误 |
>
> **为什么要用 `[ ]`？** 因为 `if` 后面跟的是"一条命令"，`[ 条件 ]` 就是一条命令（等价于 `test 条件`），它的退出码是 0（成立）或非 0（不成立），`if` 根据这个退出码决定走哪条分支。

```bash
# 单分支：条件成立就做
if [ -f /etc/passwd ]; then               # -f：存在且是普通文件
    echo "文件存在"
fi

# 双分支：成立/不成立各做一件事
if [ $# -gt 0 ]; then                     # 传了参数吗
    echo "参数个数: $#"
else
    echo "没有参数"
fi

# 多分支：elif 可以有任意多个，else 兜底
if [ "$1" == "start" ]; then
    echo "启动服务"
elif [ "$1" == "stop" ]; then
    echo "停止服务"
elif [ "$1" == "restart" ]; then
    echo "重启服务"
else
    echo "用法: $0 {start|stop|restart}"  # 参数不对时提示正确用法
fi
```

**怎么运行？（必须传参数才能看到不同分支）**

```bash
# 保存为 if_demo.sh
#!/bin/bash
if [ $# -gt 0 ]; then
    echo "参数个数: $#"
else
    echo "没有参数"
fi

if [ "$1" == "start" ]; then
    echo "启动服务"
elif [ "$1" == "stop" ]; then
    echo "停止服务"
elif [ "$1" == "restart" ]; then
    echo "重启服务"
else
    echo "用法: $0 {start|stop|restart}"
fi
```
```bash
chmod +x if_demo.sh

# 不传参数（$# 是 0，$1 是空）
./if_demo.sh
# 输出：
# 没有参数
# 用法: ./if_demo.sh {start|stop|restart}

# 传 start
./if_demo.sh start
# 输出：
# 参数个数: 1
# 启动服务

# 传 stop
./if_demo.sh stop
# 输出：
# 参数个数: 1
# 停止服务

# 传 restart
./if_demo.sh restart
# 输出：
# 参数个数: 1
# 重启服务

# 传其他参数
./if_demo.sh xxx
# 输出：
# 参数个数: 1
# 用法: ./if_demo.sh {start|stop|restart}
```

> 💡 `$#` = 参数个数，`$1` = 第一个参数，`$0` = 脚本本身名字。

### 7.3 if 常见示例（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

```bash
# 检查当前是不是 root 用户
# 逐符号拆解 "$(id -u)" -ne 0：
#   id -u        → id 命令查用户信息，-u 只输出数字 UID（如 root 输出 0，qyz 输出 1000）
#   $(id -u)     → 命令替换：把 id -u 的输出取出来，变成一个值（如 "0" 或 "1000"）
#   "$(id -u)"   → 双引号包起来，防止 UID 为空时报错
#   -ne 0        → not equal 0，不等于 0
# 整句意思：当前用户的 UID 不等于 0（= 不是 root）
# （Linux 规定 root 的 UID 是 0，其他用户 UID >= 1000）
if [ "$(id -u)" -ne 0 ]; then
    echo "请使用 root 运行此脚本"
    exit 1
fi

# 目录不存在就创建（-d 判断目录，! 取反）
if [ ! -d "/data/backup" ]; then
    mkdir -p /data/backup                  # -p：父目录一起建，已存在不报错
fi
```

### 7.4 条件判断组合（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

```bash
# && ：左边成功才执行右边；|| ：左边失败才执行右边
mkdir -p /data/logs || exit 1              # 目录建不了（没权限）就别往下跑了
cp -r /src/* /dst/ && echo "复制成功" || echo "复制失败"

# if 里组合条件（用 [ ] 时每个条件分开写，中间用 && / ||）
# 逐符号拆解：
#   [ -f "$FILE" ]   → -f = file，判断 $FILE 存在且是普通文件（不是目录）
#   &&               → 逻辑"并且"，左边成立才会判断右边
#   [ -r "$FILE" ]   → -r = readable，判断 $FILE 当前用户有读权限
#   ; then           → if 语句固定结尾：条件写完了，开始执行 then 后面的命令
# 整句意思：$FILE 是文件 并且 可读 → 就 cat 显示它
if [ -f "$FILE" ] && [ -r "$FILE" ]; then
    cat "$FILE"
fi
```

> ⚠️ `$FILE` 是你自己定义的变量，**不是 bash 自带的**。直接运行没输出是因为 `$FILE` 是空的，`[ -f "" ]` 不成立。

**怎么运行测试？**

```bash
# 方式 1：先给 FILE 赋值，再跑
FILE="/etc/passwd"                    # 给变量赋值（等号两边不能有空格）
if [ -f "$FILE" ] && [ -r "$FILE" ]; then
    cat "$FILE"
fi
# 输出：/etc/passwd 的内容

# 方式 2：换成不存在的文件，看条件不成立的情况
FILE="/etc/nonexistent"
if [ -f "$FILE" ] && [ -r "$FILE" ]; then
    cat "$FILE"
else
    echo "文件不存在或不可读"
fi
# 输出：文件不存在或不可读

# 方式 3：存成脚本，通过参数传文件名
#!/bin/bash
# 保存为 cat_file.sh
FILE="$1"                              # $1 = 你传的第一个参数
if [ -f "$FILE" ] && [ -r "$FILE" ]; then
    cat "$FILE"
else
    echo "用法: $0 <文件名>"
fi
```
```bash
chmod +x cat_file.sh
./cat_file.sh /etc/passwd    # 输出 passwd 内容
./cat_file.sh /etc/nonexist  # 输出用法提示
```

---

## 8. case 语句（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 8.1 case 语法（必会）

`case` 适合"一个变量有好几个固定取值、每个取值做不同事"的场景，比一长串 `if/elif` 清爽：

```bash
case $variable in
    pattern1)
        命令序列1                # 变量等于 pattern1 时执行
        ;;                       # ;; 表示这个分支结束（必须写）
    pattern2)
        命令序列2
        ;;
    pattern3|pattern4)
        命令序列3                # | 表示 pattern3 或 pattern4 都走这里
        ;;
    *)
        默认命令                 # * 匹配一切，相当于 else 兜底
        ;;
esac                             # case 反着拼写结束
```

### 8.2 case 示例（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

脚本参数分发是 case 最高频的用法（服务启停脚本都是这个套路）：

```bash
#!/bin/bash
# 保存为 myapp.sh，运行方式：./myapp.sh start
case "$1" in
    start)
        echo "启动服务..."
        systemctl start myapp        # 实际用：把 myapp 换成你的服务名（如 nginx）
        ;;
    stop)
        echo "停止服务..."
        systemctl stop myapp
        ;;
    restart)
        echo "重启服务..."
        systemctl restart myapp
        ;;
    status)
        systemctl status myapp
        ;;
    *)
        echo "用法: $0 {start|stop|restart|status}"   # 传错参数时提示
        exit 1
        ;;
esac
```

**怎么运行测试？**

```bash
# 1. 保存为脚本并加执行权限
chmod +x myapp.sh

# 2. 传不同参数测试（$1 就是你传的第一个参数）
./myapp.sh start       # 输出: 启动服务...（然后 systemctl 会报错，因为没有 myapp 服务）
./myapp.sh stop        # 输出: 停止服务...
./myapp.sh restart     # 输出: 重启服务...
./myapp.sh xxx         # 输出: 用法: ./myapp.sh {start|stop|restart|status}

# 3. 想看到完整效果，把 myapp 改成真实存在的服务（如 nginx）：
#    把脚本里所有 systemctl xxx myapp 改成 systemctl xxx nginx
#    然后 ./myapp.sh status 就能看到 nginx 的状态
```

> 💡 `$1` 是脚本的第一个位置参数。`./myapp.sh start` 里，`start` 就是 `$1`。`$0` 是脚本本身的名字（`./myapp.sh`）。

### 8.3 case 模式匹配（了解）

分支模式也支持通配符，按文件后缀分类很方便：

```bash
case "$file" in
    *.txt)                        # * 匹配任意字符，*.txt 即所有 txt 文件
        echo "文本文件"
        ;;
    *.jpg|*.png|*.gif)            # 多种后缀共用一个分支
        echo "图片文件"
        ;;
    *.sh)
        echo "Shell 脚本"
        ;;
    *)
        echo "其他文件"
        ;;
esac
```

---

## 9. for 循环（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握
### 9.1 for 语法（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

> 📌 **语法拆解**：
>
> **语法一：`for 变量 in 列表; do ... done`**
>
> | 部分 | 含义 |
> |------|------|
> | `for 变量 in 列表` | 从列表里**逐个**取值，每次循环把当前值赋给变量 |
> | `;` | 分隔符（写同一行才需要，换行不用） |
> | `do` | 循环体开始标志 |
> | `done` | 循环体结束标志（= do + ne，和 if/fi 一样倒过来写） |
>
> **执行流程**：
>
> ```
> for fruit in apple banana orange; do
>  echo $fruit
> done
> ```
> 第 1 轮：fruit=apple → 打印 apple
> 第 2 轮：fruit=banana → 打印 banana
> 第 3 轮：fruit=orange → 打印 orange
> 列表取完了 → 循环结束
>
> **语法二：C 风格 `for (( 初始化; 条件; 步进 )); do ... done`**
>
> 和 C/Java 的 for 循环完全一样：
> - `初始化`：循环开始前执行一次（如 `i=0`）
> - `条件`：每轮开始前判断，成立才继续（如 `i<10`）
> - `步进`：每轮结束后执行（如 `i++`）

```bash
# 语法一：遍历列表（最常用）
for 变量 in 列表; do
    命令序列
done

# 语法二：C 风格（明确知道循环次数/步长时用）
for (( 初始化; 条件; 步进 )); do
    命令序列
done
```

### 9.2 for 循环示例（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

```bash
# ① 遍历文件（最常用，通配符直接展开成文件名列表）
# 语法拆解：for 变量 in 列表; do 循环体 done
#   *.txt        → bash 自动把它展开成当前目录所有 .txt 文件名（如 a.txt b.txt）
#   file         → 每次循环，file 变量等于其中一个文件名
#   do...done    → 循环体，对每个文件执行一遍
for file in *.txt; do
    echo "处理文件: $file"
    wc -l "$file"                          # "$file" 加引号，文件名带空格也不出错
done

# ② 遍历数字序列：{1..5} 是 bash 内置写法，直接生成 1 2 3 4 5
for i in {1..5}; do
    echo "数字: $i"
done
```

**怎么运行？**

方式 A（终端直接跑，最快）：
```bash
# 例子 ② 可以直接在终端逐行敲
for i in {1..5}; do echo "数字: $i"; done
# 输出：数字: 1  数字: 2  数字: 3  数字: 4  数字: 5

# 例子 ① 依赖当前目录有 .txt 文件，先建两个测试文件
echo "hello" > a.txt
echo "world" > b.txt
for file in *.txt; do echo "处理文件: $file"; wc -l "$file"; done
# 输出：处理文件: a.txt  1 a.txt  处理文件: b.txt  1 b.txt
```

方式 B（存成脚本）：
```bash
# 保存为 for_demo.sh
#!/bin/bash
for i in {1..5}; do
    echo "数字: $i"
done
```
```bash
chmod +x for_demo.sh
./for_demo.sh
```

> ⚠️ 如果当前目录没有 .txt 文件，`*.txt` 不会展开，会原样变成字符串 "*.txt"，导致 `wc -l *.txt` 报错。生产脚本里会加 `shopt -s nullglob` 让没匹配到时展开成空。

### 9.3 for 循环实战：批量压缩旧日志（必会）（自己试）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

这是开发/运维最常见的批量任务：把 7 天前的旧日志压缩成 .gz 省空间。逐行拆解：

```bash
#!/bin/bash
LOG_DIR="/var/log/app"                              # 日志目录（改成你自己的）

# for 循环：逐个取出 $LOG_DIR 下的 .log 文件
# "$LOG_DIR"/*.log  → 展开成 /var/log/app/a.log /var/log/app/b.log ...
# logfile           → 每次循环等于其中一个文件路径
for logfile in "$LOG_DIR"/*.log; do

    # if 判断：文件存在 且 文件修改时间 早于 7天前
    # 拆解：
    #   [ -f "$logfile" ]                         → 是普通文件（防 *.log 没匹配到的情况）
    #   &&                                         → 并且
    #   "$(stat -c %Y "$logfile")"                 → 取文件的最后修改时间（Unix 时间戳，秒数）
    #   -lt                                        → less than，小于
    #   "$(date -d '7 days ago' +%s)"             → 7 天前的时间戳（秒数）
    # 整句：文件的修改时间 < 7天前的时间戳 = 文件超过 7 天没改了
    if [ -f "$logfile" ] && [ "$(stat -c %Y "$logfile")" -lt "$(date -d '7 days ago' +%s)" ]; then
        gzip "$logfile"                             # gzip 压缩（原文件变成 .gz，原文件消失）
        echo "已压缩: $logfile"
    fi
done
```

**在哪里创建、怎么运行？**

```bash
# 第 1 步：在家目录创建脚本
cd ~
vi compress_old_logs.sh
# 按 i 进入编辑，粘贴上面的代码，按 Esc，输入 :wq 保存退出

# 第 2 步：加执行权限
chmod +x compress_old_logs.sh

# 第 3 步：准备测试数据（/var/log/app 可能不存在，先建目录和测试文件）
sudo mkdir -p /var/log/app
sudo touch /var/log/app/old.log
sudo touch /var/log/app/new.log
# 把 old.log 的修改时间改成 10 天前（模拟旧日志）
sudo touch -d '10 days ago' /var/log/app/old.log

# 第 4 步：运行脚本
sudo ./compress_old_logs.sh
# 输出：已压缩: /var/log/app/old.log
# new.log 因为是今天的，不会被压缩

# 第 5 步：验证
ls -l /var/log/app/
# 应该看到：old.log.gz（被压缩了）  new.log（没变）
```

> 💡 **推荐方案：用系统真实日志测试**（不动原始日志，复制一份出来）
>
> CentOS 系统的真实日志在 `/var/log/`，比如 `/var/log/messages`。复制一份出来测：
>
> ```bash
> # ① 复制一份 messages 出来（不动原始文件）
> sudo cp /var/log/messages /var/log/messages.test
>
> # ② 把副本的修改时间改成 10 天前（模拟旧日志）
> sudo touch -d '10 days ago' /var/log/messages.test
>
> # ③ 改脚本里的 LOG_DIR（vi compress_old_logs.sh，把这行改掉）
> # LOG_DIR="/var/log/app"  →  LOG_DIR="/var/log"
>
> # ④ 运行脚本
> sudo ./compress_old_logs.sh
> # 输出：已压缩: /var/log/messages.test
>
> # ⑤ 验证
> ls -l /var/log/messages.test*
> # 应该看到：messages.test.gz（被压缩了），原 messages 没动
>
> # ⑥ 清理测试文件（压缩包可以留着，也可以删）
> sudo rm /var/log/messages.test.gz
> ```

> 💡 **时间戳比较的原理**：Unix 时间戳是从 1970-01-01 到现在的秒数，数字越大时间越晚。
> - 旧文件时间戳小（比如 1727000000）
> - 7 天前的时间戳 = 当前时间 - 7×24×3600（比如 1727500000）
> - `旧文件时间戳 < 7天前时间戳` = 文件比 7 天还老 = 该压缩了

---

## 10. while 循环（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 10.1 while 语法（必会）

> 📌 **语法拆解**：
>
> | 部分 | 含义 |
> |------|------|
> | `while 条件` | 条件成立（退出码 0）就执行循环体，不成立就退出 |
> | `;` | 分隔符（写同一行才需要） |
> | `do` | 循环体开始标志 |
> | `done` | 循环体结束标志（= do 倒过来写，和 if/fi、for/done 一样） |
>
> **和 for 的区别**：
> - `for`：知道要循环几次（遍历列表）
> - `while`：不知道循环几次，只要条件成立就一直循环
>
> **执行流程举例**：
> ```
> count=0
> while [ $count -lt 3 ]; do
>     echo $count
>     count=$((count + 1))
> done
> ```
> 第 1 轮：count=0，0<3 成立 → 打印 0，count 变 1
> 第 2 轮：count=1，1<3 成立 → 打印 1，count 变 2
> 第 3 轮：count=2，2<3 成立 → 打印 2，count 变 3
> 第 4 轮：count=3，3<3 不成立 → 退出循环

```bash
# 语法一：条件为真就一直循环（和其他语言的 while 一样）
while 条件; do
    命令序列
done

# 语法二：逐行读取文件（处理文件的标准写法，10.4 细讲）
while read 变量; do
    命令序列
done < 文件名
```

### 10.2 while 循环示例（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

```bash
# ① 简单计数：count 小于 5 就继续
count=0
while [ $count -lt 5 ]; do
    echo "计数: $count"
    count=$((count + 1))                  # 每次加 1，不加会死循环
done

# ② 逐行读取文件：read 每读一行放进 line，-r 不转义反斜杠
while read -r line; do
    echo "行内容: $line"
done < /etc/passwd                        # < 文件：把文件内容喂给循环
```

**怎么运行？**

方式 A（终端直接跑）：
```bash
# 例子 ① 直接在终端敲
count=0
while [ $count -lt 5 ]; do echo "计数: $count"; count=$((count + 1)); done
# 输出：计数: 0  计数: 1  计数: 2  计数: 3  计数: 4

# 例子 ② 读 /etc/passwd（系统自带文件）
while read -r line; do echo "行: $line"; done < /etc/passwd | head -3
# 输出前 3 行：
# 行: root:x:0:0:root:/root:/bin/bash
# 行: bin:x:1:1:bin:/bin:/sbin/nologin
# 行: daemon:x:2:2:daemon:/sbin:/sbin/nologin
```

方式 B（存成脚本）：
```bash
# while_demo.sh
#!/bin/bash
count=0
while [ $count -lt 5 ]; do
    echo "计数: $count"
    count=$((count + 1))
done
```
```bash
chmod +x while_demo.sh
./while_demo.sh
```

> ⚠️ while 循环最容易犯的错：**忘记改条件变量**，导致死循环。
> 比如 `while [ $count -lt 5 ]; do echo $count; done` 里没有 `count=$((count+1))`，count 永远是 0，条件永远成立，会一直打印 0。
> 死循环按 `Ctrl+C` 终止。

### 10.3 break 与 continue（必会）

```bash
# break：立刻结束【整个循环】
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break                            # 数到 5 直接退出，后面 6~10 不再执行
    fi
    echo $i                              # 打印 1 2 3 4
done

# continue：跳过【本次】循环剩余部分，直接进下一次
for i in {1..10}; do
    if [ $((i % 2)) -eq 0 ]; then
        continue                         # 偶数直接跳过
    fi
    echo $i                              # 只打印奇数：1 3 5 7 9
done
```

**怎么运行？**

```bash
# 直接在终端跑（一行版）
for i in {1..10}; do if [ $i -eq 5 ]; then break; fi; echo $i; done
# 输出：1 2 3 4

for i in {1..10}; do if [ $((i % 2)) -eq 0 ]; then continue; fi; echo $i; done
# 输出：1 3 5 7 9
```

> 💡 `$((i % 2))` 是取余运算，`%` = mod。i 除以 2 的余数：偶数余 0，奇数余 1。

### 10.4 while read 安全读取（必会）

从文件或命令输出逐行取数据时，**固定用这个模板**，能避开两个经典坑：

```bash
# ❌ 坑一：for + $(cat)，含空格的行会被拆成多段
for line in $(cat file.txt); do echo "$line"; done

# ❌ 坑二：管道 | 后的 while 运行在"子 shell"里，里面改的变量循环结束就没了
count=0
cat file.txt | while read line; do
    count=$((count + 1))                 # 改的是子 shell 里的 count
done
echo "行数: $count"                      # 0！外面的 count 根本没变

# ✅ 正确写法：输入重定向 <，循环就在当前 shell 里跑
count=0
while IFS= read -r line; do              # IFS=（Internal Field Separator 内部字段分隔符）设为空，保留行首行尾空格；-r 不吞反斜杠
    count=$((count + 1))
    echo "$count: $line"
done < file.txt                          # 用 < 喂文件，而不是 |
echo "总行数: $count"                    # 正确输出行数

# 处理 CSV：IFS=, 指定逗号分隔，一行拆成多列
while IFS=, read -r id name email; do
    echo "$id | $name | $email"
done < users.csv
```

> 🔑 记住模板 `while IFS= read -r line; do ...; done < 文件` 即可，原理不用深究。

**怎么运行测试？**

```bash
# 第 1 步：创建测试文件 file.txt
cat > file.txt << 'EOF'
hello world
foo bar
EOF

# 第 2 步：创建测试文件 users.csv
cat > users.csv << 'EOF'
1,张三,zhangsan@example.com
2,李四,lisi@example.com
EOF

# 第 3 步：运行正确写法
count=0
while IFS= read -r line; do
    count=$((count + 1))
    echo "$count: $line"
done < file.txt
echo "总行数: $count"
# 输出：
# 1: hello world
# 2: foo bar
# 总行数: 2

# 第 4 步：运行 CSV 例子
while IFS=, read -r id name email; do
    echo "$id | $name | $email"
done < users.csv
# 输出：
# 1 | 张三 | zhangsan@example.com
# 2 | 李四 | lisi@example.com
```

> ⚠️ 坑二的演示：`cat file.txt | while read` 里改的 count 循环结束后会变回 0，因为管道右边的 while 跑在子 shell 里。这就是为什么要用 `< file.txt` 而不是 `cat file.txt |`。

---

## 11. read 获取输入（了解）
> 🏷️ 📖 概念了解 —— 后端脚本大多非交互（部署、备份都是自动跑），read 用得少，知道有这个东西即可

### 11.1 read 基本用法（了解）

```bash
# 最基本：等用户输入一行，回车后内容存进变量
read name
echo "你好, $name"

read -p "请输入用户名: " username        # -p：在同一行显示提示语
read -s -p "请输入密码: " password        # -s：输入时不显示内容（输密码用），输完变量里照常有值
echo                                      # -s 不换行，补一个换行
read -t 5 -p "5秒内不输入就跳过: " input  # -t 5：等待 5 秒，超时就继续往下走
```

### 11.2 实战：执行危险操作前确认（了解）

`read` 最实用的场景：删除、覆盖等操作前让用户输 y/n 确认：

```bash
#!/bin/bash
read -p "确定要清空 /tmp/app_cache 吗？(y/n): " ans    # 弹出确认
case "$ans" in
    y|Y|yes|YES)                          # 用户输入 y / Y / yes 都算同意
        rm -rf /tmp/app_cache
        echo "已清空"
        ;;
    *)                                    # 其他任何输入都取消（默认安全）
        echo "已取消"
        ;;
esac
```

---

## 12. 系统函数（必会）（自己试）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 12.1 脚本里最常用的命令（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

下面 4 个命令写脚本时天天用。**学习顺序**：先把每个命令**单独在终端跑一遍**搞懂输出，再看最后的综合例子理解怎么组合。

#### ① date —— 取时间（拼备份文件名必用）

```bash
# 格式符：%Y 年 %m 月 %d 日 %H 时 %M 分 %S 秒（都是两位，不足补 0）
date '+%Y%m%d_%H%M%S'                # 输出: 20260915_143022

# 脚本里通常把时间存到变量，再拼进文件名：
TIMESTAMP=$(date '+%Y%m%d_%H%M%S')   # $(...) 把命令输出存到变量
echo "备份文件名: backup_${TIMESTAMP}.tar.gz"
# 输出: 备份文件名: backup_20260915_143022.tar.gz
```

#### ② basename / dirname —— 从路径里切文件名、目录名

```bash
# basename：只留文件名（去掉前面的目录）
basename /data/logs/app.log          # 输出: app.log

# dirname：只留目录（去掉后面的文件名）
dirname  /data/logs/app.log          # 输出: /data/logs

# 脚本里典型用法：已知一个文件的完整路径，分别取目录和文件名
FILE="/www/myproject/manage.py"
DIR=$(dirname "$FILE")               # /www/myproject
NAME=$(basename "$FILE")             # manage.py
echo "目录: $DIR, 文件名: $NAME"
```

#### ③ cut —— 按分隔符切列

```bash
# -d 指定分隔符，-f 取第几列（从 1 开始数）
#`-d` ：**delimiter，分隔符**。`-d:` 意思：**用冒号 `:` 当做分割线**，把一行切开
# 注意：`-d` 后面紧跟分隔字符，`-d:` 等价于 `-d ":"`
#`-f` ：**field，字段 / 列**。`-f1` = **取分割后的第 1 列（从 1 开始计数，不是 0！这点和编程语言不一样）**
root:x:0:0:root:/root:/bin/bash
1 :  2 :3:4: 5 :  6  :    7
# 用冒号`:`切开之后，分成 7 段：

echo "root:x:0:0" | cut -d: -f1      # 输出: root（按冒号切，取第 1 列）
echo "root:x:0:0" | cut -d: -f3      # 输出: 0（取第 3 列）
  
# 实战：从 /etc/passwd 里取出所有用户名（第 1 列）
cut -d: -f1 /etc/passwd              #   /etc/passwd里面就是root:x:0:0:root:/root:/bin/bash
```

#### ④ $RANDOM —— bash 内置随机数

```bash
echo $RANDOM                         # 输出: 0~32767 之间的随机整数
echo $((RANDOM % 100))               # 取余后变成 0~99
echo $((RANDOM % 50 + 1))            # 1~50 之间的随机数（+1 是为了从 1 开始）
```

#### 实战：备份单个文件的脚本（改配置前必用）

**用在哪里？** 改任何重要文件之前（Nginx 配置、Django settings.py、.env 等），先跑这个脚本备份一份，改坏了能回滚。这是开发运维的基本习惯。

把下面内容保存成 `backup_file.sh`：

```bash
#!/bin/bash
# 用法：./backup_file.sh /path/to/要备份的文件
# 功能：给指定文件做个带时间戳的备份副本，放在同目录下

# $1 是要备份的文件路径；${1:-} 意思是没传参数就是空的
SRC_FILE="${1:-}"

# 没传参数就提示用法并退出
if [ -z "$SRC_FILE" ]; then               # -z：字符串为空
    echo "用法: $0 /path/to/file"          # $0 是脚本自己的名字
    exit 1
fi

# 文件不存在就报错退出
if [ ! -f "$SRC_FILE" ]; then             # ! -f：不是普通文件（不存在或是目录）
    echo "错误: 文件不存在 $SRC_FILE"
    exit 1
fi

# 1. date 生成时间戳
TIMESTAMP=$(date '+%Y%m%d_%H%M%S')

# 2. $RANDOM 生成随机数（防同一秒内多次备份重名）
RAND=$((RANDOM % 1000))

# 3. basename 从路径里取出原文件名
FILENAME=$(basename "$SRC_FILE")          # 例如 app.log

# 4. 拼出备份文件名：原文件名_时间戳_随机数.bak
BACKUP_NAME="${FILENAME}_${TIMESTAMP}_${RAND}.bak"

# 5. dirname 取出源文件所在目录，备份放在同目录
SRC_DIR=$(dirname "$SRC_FILE")            # 例如 /data/logs

# 6. cp 复制一份
cp "$SRC_FILE" "${SRC_DIR}/${BACKUP_NAME}"

echo "备份完成: ${SRC_DIR}/${BACKUP_NAME}"
echo "原文件:   $SRC_FILE"
```

**运行方法**：

```bash
chmod +x backup_file.sh

# 备份 Nginx 配置（改之前先备份！）
./backup_file.sh /www/server/nginx/conf/nginx.conf

# 备份 Django 配置
./backup_file.sh /www/myproject/.env

# 输出类似：
# 备份完成: /www/server/nginx/conf/nginx.conf_20260915_143022_523.bak
# 原文件:   /www/server/nginx/conf/nginx.conf
```

> 💡 **实际开发中的用法**：每次改 Nginx 配置、Django settings.py、.env 之前，先跑一下这个脚本。改坏了直接 `cp 备份文件 原文件` 就能恢复，不用手忙脚乱。

### 12.2 两个实用小工具（了解）

**① 执行前先检查命令是否安装（`command -v`）：**

```bash
# command -v：-v = verbose，输出命令的完整路径（找到就成功，找不到就失败）
# >/dev/null：丢掉屏幕输出，只看退出码
command -v nginx >/dev/null && echo "nginx 已安装" || echo "请先安装 nginx"
```

**② retry：失败自动重试 3 次**（网络抖动场景很好用，顺便复习函数和 `"$@"`）：

```bash
retry() {
    local attempt=1                          # local：函数内部变量（2.8 节）
    while [ $attempt -le 3 ]; do             # 最多试 3 次
        echo "第 $attempt 次尝试..."
        if "$@"; then                        # "$@" 把传进来的整条命令原样执行
            return 0                         # 成功就返回 0，不再重试
        fi
        sleep 2                              # 失败等 2 秒再试
        attempt=$((attempt + 1))
    done
    return 1                                 # 3 次都失败，返回失败
}

retry git pull                               # 用法：git pull 失败会自动重试
```

---

## 13. 自定义函数（必会）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 13.1 函数定义（必会）

**语法**：`函数名() { 命令... }`（不用写 `function` 关键字，也不用写参数列表）

**调用**：直接写函数名 + 空格 + 参数（**不加括号**，这是和 Python/Java 最大的区别）

```bash
# 定义
say_hello() {
    echo "Hello, $1!"               # $1 是函数的第一个参数（和脚本参数一个套路）
}

# 调用
say_hello "World"                    # 输出: Hello, World!
say_hello "Shell"                    # 输出: Hello, Shell!
```

> ⚠️ 调用函数是 `say_hello "World"`，**不是** `say_hello("World")`，这是新手最常犯的错。

### 13.2 函数参数与返回值（必会）

函数里用 `$1 $2 ...` 接收参数，和脚本参数完全一样。返回值有两种方式：

| 方式 | 用途 | 怎么接住 |
|------|------|---------|
| `echo` 输出 | 返回数据（字符串、数字） | 外面用 `$(函数名 参数)` 接住 |
| `return` 状态码 | 返回成功/失败（0=成功，非0=失败） | 外面用 `if 函数名; then` 判断 |

```bash
#!/bin/bash
# ① 带参数的函数
greet() {
    local name="$1"                  # local：只在函数内部有效，防止污染外面的同名变量
    local age="$2"
    echo "你好, $name! 你 $age 岁。"
}

# ② 返回数据：函数里 echo，外面用 $() 接
add() {
    local a=$1
    local b=$2
    echo $((a + b))                  # 用 echo 把结果输出
}

# ③ 返回成败：用 return 状态码，if 判断
is_even() {
    if [ $(( $1 % 2 )) -eq 0 ]; then
        return 0                     # 是偶数 → 成功(0)
    else
        return 1                     # 是奇数 → 失败(1)
    fi
}

# ===== 调用演示 =====
greet "张三" 25                       # 输出: 你好, 张三! 你 25 岁。

result=$(add 10 20)                   # $() 接住函数的 echo 输出
echo "10 + 20 = $result"              # 输出: 10 + 20 = 30

if is_even 4; then                    # 把函数直接放进 if
    echo "4 是偶数"
else
    echo "4 是奇数"
fi
```

**运行方法**：保存成 `func_demo.sh`，然后：
```bash
chmod +x func_demo.sh
./func_demo.sh
# 输出：
# 你好, 张三! 你 25 岁。
# 10 + 20 = 30
# 4 是偶数
```

> 🔑 记住：**要数据用 `echo` + `$()`，要成败用 `return` + `if`**。

### 13.3 函数实战：日志函数（必会）

写长脚本时，先定义一个统一的日志函数，后面所有输出都走它（带时间戳、分级）：

```bash
#!/bin/bash
log() {
    local level="$1"                          # 第 1 个参数：级别 INFO/ERROR
    local message="$2"                        # 第 2 个参数：日志内容
    local timestamp=$(date '+%F %T')          # 当前时间（%F = 年-月-日，%T = 时:分:秒）
    echo "[$timestamp] [$level] $message"
}

# 检查命令是否存在，不存在就退出（12.2 的 command -v 封装成函数）
check_command() {
    # >/dev/null 2>&1：把正常输出(1)丢黑洞，把错误输出(2)也合并到 1 的去向（一起丢黑洞）
    # 2 = 标准错误(stderr)，1 = 标准输出(stdout)，&1 表示"指向 1 的目标"
    if ! command -v "$1" >/dev/null 2>&1; then
        log "ERROR" "$1 未安装"
        exit 1
    fi
}

# ===== 使用演示 =====
log "INFO"  "脚本开始执行"                     # [2026-09-15 14:30:22] [INFO] 脚本开始执行
check_command python3                          # 没有 python3 脚本就退出
log "INFO"  "python3 已安装，继续..."
log "ERROR" "模拟一条错误日志"
```

**运行方法**：保存成 `log_demo.sh`，然后：
```bash
chmod +x log_demo.sh
./log_demo.sh
```

### 13.4 实战：Django 项目备份脚本（必会）

实际开发中，函数最常见的用途就是**把重复操作封装起来**。比如备份一个 Django 项目，要备份 3 样东西：

| 备份内容 | 为什么要备份 | 备份方式 |
|---------|------------|---------|
| MySQL 数据库 | 业务数据（用户、订单等），丢了就完了 | `mysqldump` 导出成 SQL |
| 项目配置文件 | `.env`（密钥、数据库密码）、`settings.py` | `tar` 打包 |
| 用户上传文件 | `media/` 目录（头像、附件等） | `tar` 打包 |

把每个备份步骤封装成函数，主流程只负责"调用"：

```bash
#!/bin/bash
# Django 项目备份脚本
# 用法: ./backup.sh [项目名]   （不传项目名默认 myproject）

# ===== 配置区：根据你的实际情况改这几个变量 =====
PROJECT_NAME="${1:-myproject}"           # 项目名（不传参数默认 myproject）
PROJECT_DIR="/www/${PROJECT_NAME}"       # 项目根目录
BACKUP_ROOT="/data/backup"               # 备份存放的根目录
DB_NAME="${PROJECT_NAME}_db"             # 数据库名
DB_USER="root"                           # 数据库用户
DB_PASS="your_db_password"               # 数据库密码（生产环境放 .env 里，别写死）
RETAIN_DAYS=7                            # 保留最近 7 天的备份

# ===== 函数区：每个函数干一件事 =====

# 日志函数（带时间戳和级别）
log() {
    local level="$1"
    local message="$2"
    echo "[$(date '+%F %T')] [$level] $message"
}

# 函数 1：备份 MySQL 数据库
backup_database() {
    local timestamp=$(date '+%Y%m%d_%H%M%S')
    local backup_file="${BACKUP_DIR}/${DB_NAME}_${timestamp}.sql.gz"

    log "INFO" "开始备份数据库 ${DB_NAME}..."
    # mysqldump：MySQL 自带的导出工具；| gzip：边导出边压缩
    mysqldump -u "$DB_USER" -p"$DB_PASS" "$DB_NAME" | gzip > "$backup_file"

    if [ $? -eq 0 ]; then                 # $? 是上一条命令的退出码，0 表示成功
        log "INFO" "数据库备份成功: $backup_file"
    else
        log "ERROR" "数据库备份失败！"
        return 1                           # 返回 1 表示失败
    fi
}

# 函数 2：备份项目配置文件和用户上传文件
backup_files() {
    local timestamp=$(date '+%Y%m%d_%H%M%S')
    local backup_file="${BACKUP_DIR}/${PROJECT_NAME}_files_${timestamp}.tar.gz"

    log "INFO" "开始备份项目文件..."
    # tar 打包：-czf = create(创建) + gzip(压缩) + file(输出到文件)
    # 只打包 .env 和 media 目录，不打包代码（代码在 git 里）
    tar -czf "$backup_file" \
        -C "$PROJECT_DIR" \                # -C：先切到项目目录再打包（避免路径里带 /www/...）
        .env media/                        # 要打包的内容：配置文件 + 用户上传文件

    if [ $? -eq 0 ]; then
        log "INFO" "文件备份成功: $backup_file"
    else
        log "ERROR" "文件备份失败！"
        return 1
    fi
}

# 函数 3：清理过期备份
cleanup_old_backups() {
    log "INFO" "清理 ${RETAIN_DAYS} 天前的备份..."
    # find -mtime +N：找修改时间超过 N 天的文件；-delete 直接删
    find "$BACKUP_DIR" -name "*.sql.gz" -mtime +$RETAIN_DAYS -delete
    find "$BACKUP_DIR" -name "*_files_*.tar.gz" -mtime +$RETAIN_DAYS -delete
    log "INFO" "清理完成"
}

# ===== 主流程：按顺序调用函数 =====
main() {
    mkdir -p "$BACKUP_DIR"                 # 确保备份目录存在

    backup_database || exit 1              # 数据库备份失败就整体退出
    backup_files    || exit 1              # 文件备份失败就整体退出
    cleanup_old_backups

    log "INFO" "全部备份完成！"
}

main                                     # 调用主函数，脚本从这里开始跑
```

**运行方法**：

```bash
chmod +x backup.sh

# 备份默认项目（myproject）
./backup.sh

# 备份指定项目
./backup.sh shop

# 输出类似：
# [2026-09-15 14:30:00] [INFO] 开始备份数据库 myproject_db...
# [2026-09-15 14:30:05] [INFO] 数据库备份成功: /data/backup/myproject_db_20260915_143000.sql.gz
# [2026-09-15 14:30:05] [INFO] 开始备份项目文件...
# [2026-09-15 14:30:08] [INFO] 文件备份成功: /data/backup/myproject_files_20260915_143005.tar.gz
# [2026-09-15 14:30:08] [INFO] 清理 7 天前的备份...
# [2026-09-15 14:30:08] [INFO] 清理完成
# [2026-09-15 14:30:08] [INFO] 全部备份完成！
```

> 💡 **这就是函数的价值**：`main()` 里只看得到"备份数据库、备份文件、清理旧备份"三件事，具体怎么备份的细节都封装在各自的函数里。脚本长了之后，这种结构比把所有命令堆在一起好维护得多。

### 13.5 函数库：source 复用（了解）

多个脚本共用同一批函数时，把函数放进一个公共文件，用 `source` 加载（和第 1 节加载 `~/.bashrc` 是同一个命令）：

```bash
# 1. 建一个函数库文件 common.sh
cat > common.sh <<'EOF'
log() {
    echo "[$(date '+%F %T')] $*"    # $* 把所有参数拼成一个字符串（日志场景用 $* 更合适）
}
EOF

# 2. 在别的脚本里加载，之后就能直接调用里面的函数
source ./common.sh
log "脚本启动"
```

### 13.5 Here Document：一次写多行内容（了解）

`<<EOF`（heredoc）可以把多行文本一次性喂给命令，写配置文件、给 mysql 喂 SQL 时常用：

```bash
# ① 把多行文本写进文件（cat > 文件 <<EOF ... EOF）
cat > /tmp/test.txt <<EOF
第一行
第二行
当前用户是 $USER                 # 不加引号的 EOF：里面的 $变量、$(命令) 会被解析
EOF

# ② EOF 加单引号：内容原样写入，$ 和 $( ) 不解析（写配置模板时用它）
cat > /tmp/template.sh <<'EOF'
echo "$HOME"                     # 这行的 $HOME 会原样写进文件，不会被替换
EOF

# ③ 给 mysql 喂多条 SQL（开发初始化数据库时常见）
mysql -u root -p mydb <<EOF
SELECT * FROM users LIMIT 5;
UPDATE config SET value='1' WHERE name='debug';
EOF
```

> 规则就一条：**结束标记 EOF 必须顶格写、两边一致；想让 `$` 原样保留就写 `<<'EOF'`**。

### 13.6 命令分组与整组重定向（了解）

想把好几条命令的输出一起送进同一个文件，用 `{ }` 包起来：

```bash
# { 命令1; 命令2; } > 文件 2>&1：所有正常输出和报错都进同一个文件
{ echo "=== 报告头 ==="; date; df -h; } > /tmp/report.txt 2>&1
# 注意语法：{ 后面要有空格，每条命令后要有 ; 或换行
```

### 13.7 xargs 批量处理（必会，与第二章 find 部分配合）

`xargs` 把管道传来的内容变成后面命令的参数。**`find -exec`、`xargs` 的系统讲解在第二章 4.6 节**，这里只放脚本里最常用的两个写法：

```bash
# ① 把 find 找到的文件交给 rm/grep 等批量处理
find /tmp -name "*.log" | xargs rm -f        # 等价于 find /tmp -name "*.log" -delete

# ② 文件名可能带空格时的安全写法：-print0 和 -0 配对，用 \0 分隔
find / -name "*.txt" -print0 | xargs -0 grep "pattern"

# ③ -I {} 占位符：控制参数插在命令的哪个位置
find /var/log -name "*.log" | xargs -I {} cp {} /backup/    # 每个文件复制到 /backup/
```

### 13.8 调试技巧（必会）

脚本不按预期工作时，按下面三步排查，不要肉眼硬看：

```bash
# ① bash -n：只检查语法不执行（少写 fi、done 时一查就出来）
bash -n script.sh

# ② bash -x：执行并把每条命令（变量展开后的样子）打印出来，前面带 +
bash -x script.sh

# ③ 只跟踪脚本中的某一段：set -x 开始，set +x 结束
set -x
...怀疑有问题的代码...
set +x
```

有网络的话强烈建议装 **shellcheck**，它像脚本的"代码检查器"，能直接指出引号、变量未定义等问题：

```bash
# shellcheck 不在 CentOS 7 base 源里，需要先装 EPEL 扩展源
# ⚠️ 前提：先按第三章 7.1.0 节修好 CentOS base 源，再装 EPEL（EPEL 7 也已归档，需按 7.1.0 末尾的方法改指向阿里云 epel-archive）
yum install -y epel-release
yum install -y shellcheck
shellcheck script.sh                 # 按行号给出问题和修改建议
```

---

## 14. 定时备份数据库实战（必会）（自己试）
> 🏷️ ⭐ 开发必会 —— 软件开发日常高频使用，务必掌握

### 14.1 备份脚本（必会）

下面这个脚本就是开头场景 2 的完整版，逐行看：

```bash
#!/bin/bash
# 功能：备份 MySQL 数据库并自动清理 7 天前的旧备份
set -euo pipefail                         # 严格模式：出错即停、变量未定义即报错（1.5 节）

# ===== 配置区（换环境只改这里）=====
DB_NAME="myapp"                           # 要备份的库名
DB_USER="root"
DB_PASS="your_password"
BACKUP_DIR="/data/backup/mysql"           # 备份存放目录
RETAIN_DAYS=7                             # 备份保留天数
LOG_FILE="/var/log/mysql_backup.log"
TIMESTAMP=$(date '+%Y%m%d_%H%M%S')        # 当前时间戳，用来给备份文件命名

log() { echo "[$(date '+%F %T')] $*" >> "$LOG_FILE"; }   # 小日志函数：输出追加到日志文件

# ===== 开始备份 =====
mkdir -p "$BACKUP_DIR"                                      # 确保备份目录存在
log "开始备份 $DB_NAME"

# mysqldump：MySQL 自带的导出工具，把整个库导出成 SQL 文本
# -u 用户名 -p密码（-p 和密码之间没空格）
# --single-transaction：在一个事务里导出，不锁表（InnoDB 引擎推荐，不影响线上写入）
# \ 是行续接符：一行太长时换行写，bash 会把下一行拼上来当作同一行
mysqldump -u "$DB_USER" -p"$DB_PASS" --single-transaction "$DB_NAME" \
    | gzip > "$BACKUP_DIR/${DB_NAME}_$TIMESTAMP.sql.gz"     # 管道 | 把导出内容交给 gzip 压缩，> 写入文件

# ===== 清理过期备份 =====
# find 的 -mtime +7：按修改时间找文件，+7 表示超过 7 天的；-delete 直接删掉
find "$BACKUP_DIR" -name "${DB_NAME}_*.sql.gz" -mtime +$RETAIN_DAYS -delete

log "备份完成: ${DB_NAME}_$TIMESTAMP.sql.gz"
```

脚本写好后赋权并手动跑一遍，确认没问题再上定时任务：

```bash
chmod +x /usr/local/bin/mysql_backup.sh    # 加执行权限
/usr/local/bin/mysql_backup.sh             # 先手动执行验证
cat /var/log/mysql_backup.log              # 看日志确认
```

### 14.2 设置定时任务（必会）

```bash
crontab -e                    # 编辑当前用户的定时任务（类似 vi，保存即生效）
crontab -l                    # 查看当前已有的定时任务
```

在打开的文件里加一行：

```bash
# 分 时 日 月 周   要执行的命令（命令务必写绝对路径）
0 2 * * * /usr/local/bin/mysql_backup.sh      # 每天凌晨 2:00 执行备份
0 3 1 * * /usr/local/bin/mysql_archive.sh     # 每月 1 号 03:00 归档
```

### 14.3 时间格式说明（必会）

```
# ┌───── 分钟 (0-59)
# │ ┌──── 小时 (0-23)
# │ │ ┌─── 日 (1-31)
# │ │ │ ┌── 月 (1-12)
# │ │ │ │ ┌─ 星期 (0-7，0 和 7 都是周日)
# │ │ │ │ │
  * * * * * 命令
```

```bash
*/5 * * * *  命令      # 每 5 分钟
0 */1 * * *  命令      # 每小时整点
0 2 * * *    命令      # 每天 02:00
0 0 1 * *    命令      # 每月 1 号 00:00
0 0 * * 0    命令      # 每周日 00:00
@daily       命令      # 每天一次（等价于 0 0 * * *，记不住星号时可用）
@reboot      命令      # 每次开机后执行一次
```

> 四个符号：`*` 任意值、`,` 列举多个（`1,3,5`）、`-` 范围（`1-5`）、`/` 步长（`*/10`）。

### 14.4 crond 服务与排坑（必会）

cron 的后台服务叫 `crond`，定时任务不执行时先确认它在运行、是否开机自启：

```bash
systemctl status crond          # 看运行状态
systemctl enable crond          # 设为开机自启
tail -f /var/log/cron           # 看 cron 自己的运行日志（任务有没有被触发）
```

#### 14.4.1 cron 环境的坑（最高频踩雷点）

> 🏷️ ⭐ **必看** —— 手动能跑、cron 一跑就报 command not found，99% 是这个原因。

cron 执行脚本时的环境和你登录后的 shell **完全不同**：
- **PATH 极简**（通常只有 `/usr/bin:/bin`），找不到 `java`、`mysql`、`pip`
- 不加载 `~/.bashrc` 等配置文件
- 没有终端，不能交互输入
- 工作目录是家目录，不是脚本所在目录

四个对策：

```bash
# ① 最稳：命令一律写绝对路径（yum 装的命令默认在 /usr/bin/ 下）
/usr/bin/mysqldump -u root -p mydb > backup.sql

# ② 或在脚本开头显式补 PATH（yum 装的 MySQL/JDK 命令本就在 /usr/bin，无需额外加；
#    只有手动装在自建目录的才需要补，例如第三章 8.2 手动装的 JDK 在 /usr/local/java/jdk8/bin）
export PATH=/usr/bin:/bin:/usr/local/java/jdk8/bin

# ③ 脚本开头先 cd 到脚本自己所在目录（避免相对路径错位）
# dirname "$0"：取出脚本所在目录（$0 是脚本名）；|| exit 1：cd 失败就立刻退出
cd "$(dirname "$0")" || exit 1

# ④ crontab 里把输出和报错重定向到日志，失败了才有东西可查
# > /tmp/backup.log 2>&1：正常输出写日志，错误输出也合并进去（2>&1 见 13.3 节）
0 2 * * * /usr/local/bin/backup.sh > /tmp/backup.log 2>&1
```

排查顺序：先 `grep CRON /var/log/cron` 确认任务被触发 → 再看重定向日志里的报错 → 最后把命令原样粘到终端手动跑一遍。

#### 14.4.2 生产级备份：本地备份 + rsync 同步到备份机（了解）
> 🏷️ 📖 概念了解 —— 多机备份是运维的活，后端开发单机备份够用，rsync 知道是干什么的就行

单机备份只防"误删"，防不了机器损坏。企业里通常再用 **rsync** 把备份同步到另一台备份服务器：

```bash
#!/bin/bash
# 备份后同步到远程备份机
set -euo pipefail
BACKUP_DIR="/data/backup/mysql"
TIMESTAMP=$(date '+%Y%m%d_%H%M%S')
REMOTE="backup@192.168.1.100"                          # 备份机的 用户@IP

mysqldump -u root -p"xxx" --single-transaction myapp | gzip \
    > "$BACKUP_DIR/myapp_$TIMESTAMP.sql.gz"            # ① 本地备份

rsync -avz --partial --timeout=300 \                   # ② 同步到备份机
    "$BACKUP_DIR/myapp_$TIMESTAMP.sql.gz" \
    "$REMOTE:/backup/mysql/"

find "$BACKUP_DIR" -name "myapp_*.sql.gz" -mtime +7 -delete   # ③ 清理本地 7 天前备份
```

**rsync 是开发应该认识的同步工具**（部署代码、同步目录都可能用它）：

```bash
# 本地目录同步（结尾的 / 有讲究：/src/ 表示同步目录【内容】到 /dst/）
rsync -avz /src/ /dst/

# 远程同步（走 ssh 加密）
rsync -avz -e ssh /data/ backup@192.168.1.100:/backup/data/

# --delete：让目标和源完全一致（目标里多余的文件会被删，危险！先试运行）
rsync -avz --delete /src/ /dst/

# --exclude：排除不想传的东西
rsync -avz --exclude='.git' --exclude='*.tmp' /src/ /dst/

# -n 模拟运行（dry-run）：只打印会做什么、不真传，加 --delete 前必跑一次
rsync -avzn --delete /src/ /dst/
```

常用选项：`-a` 保持权限/时间等属性并递归、`-v` 显示过程、`-z` 传输时压缩、`--partial` 断点续传、`--timeout=300` 超时 300 秒放弃、`-e ssh` 走 ssh 加密传输。rsync 的核心优势是**增量传输**——只传变化的部分，第二次同步会非常快。

### 14.5 其他备份方式对比（了解）

shell + crontab 是**传统做法**，小项目和裸机部署还在用。但现代开发有更多选择：

| 方式 | 适用场景 | 优点 | 缺点 |
|------|---------|------|------|
| **shell + crontab**（14 节讲的） | 裸机/虚拟机部署 | 简单可控、不依赖额外工具 | 要自己写清理、传输、告警 |
| **Docker exec** | MySQL 跑在 Docker 容器里 | 不用在宿主机装 mysqldump | 要进容器执行 |
| **云数据库自带备份** | 用阿里云 RDS、腾讯云 CDB 等 | 一键开启、自动定时、支持按时间点恢复 | 付费、被云厂商绑定 |
| **K8s CronJob** | 用 K8s 编排的集群 | 容器化、声明式配置 | 学习成本高 |
| **Python 脚本** | 需要复杂逻辑（多库、传 OSS、钉钉告警） | 灵活、易维护 | 要装 Python 环境 |

#### 14.5.1 Docker 里备份 MySQL

如果 MySQL 是用 Docker 跑的，宿主机上没有 mysqldump，用 `docker exec` 进容器执行：

```bash
#!/bin/bash
# backup_docker_mysql.sh
# 功能：备份 Docker 容器里的 MySQL
set -euo pipefail

CONTAINER="mysql"                              # 你的 MySQL 容器名（docker ps 看）
DB_NAME="myapp"
DB_USER="root"
DB_PASS="your_password"
BACKUP_DIR="/data/backup"
TIMESTAMP=$(date '+%Y%m%d_%H%M%S')

# docker exec：在容器里执行 mysqldump
# 输出通过管道 | 重定向到宿主机的文件（不用进容器）
docker exec "$CONTAINER" \
    mysqldump -u"$DB_USER" -p"$DB_PASS" --single-transaction "$DB_NAME" \
    | gzip > "$BACKUP_DIR/${DB_NAME}_$TIMESTAMP.sql.gz"

echo "备份完成: ${DB_NAME}_$TIMESTAMP.sql.gz"
```

```bash
# 运行方式和普通脚本一样
chmod +x backup_docker_mysql.sh
./backup_docker_mysql.sh

# 加到 crontab
0 2 * * * /usr/local/bin/backup_docker_mysql.sh > /tmp/docker_backup.log 2>&1
```

> 💡 **为什么不用 `docker exec ... > 文件` 而是 `| gzip > 文件`？**
> `docker exec` 的输出是 SQL 文本，直接 `>` 保存体积大，用 `| gzip` 管道压缩后再存，省空间。

#### 14.5.2 云数据库备份（了解）

用阿里云 RDS、腾讯云 CDB 等云数据库时，**不用自己写备份脚本**：
- 控制台一键开启自动备份（默认每天一次）
- 支持按时间点恢复（Point-in-Time Recovery）
- 支持手动下载备份文件
- 费用包含在数据库实例里，不用额外买备份机

> 建议：个人项目/学习用 shell + crontab（练手），上生产用云数据库自带备份（省心）。

---

## 本章小结

学完本章，应该能独立完成这些事：

1. ✅ 写出规范的 bash 脚本（shebang、`set -euo pipefail`、`chmod +x`、三种执行方式）
2. ✅ 用变量、环境变量、`$1`/`$@` 参数、`$?` 返回值
3. ✅ 用 `$(( ))` 做整数运算，用 `${var:-默认值}` 兜底，用 `%`/`#` 处理文件名
4. ✅ 用 `if`/`case` 做条件判断，用 `for`/`while` 写循环
5. ✅ 用函数（`echo`+`$()` 返回数据，`return`+`if` 返回成败）封装重复逻辑
6. ✅ 会用 `bash -n`/`bash -x`/shellcheck 调试脚本
7. ✅ 能写出"备份 + 清理 + crontab 定时"的完整小方案
8. 📖 了解：`read` 交互输入、`rsync` 远程同步（运维向，知道有这东西即可）

一条红线：**脚本超过 100 行还在长大、或者要处理复杂数据结构，就换 Python。**

**下一章预告：** 《Linux 学习课件 — 第五章：进阶实战》将涵盖日志管理、Web 服务器部署、宝塔面板、Django+MySQL+Vue3 全栈项目部署等开发实战内容。

---

