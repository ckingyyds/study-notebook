**git简单使用**

Git 学习笔记：从本地仓库、分支，到 Gitee 远程仓库与团队协作，再到 PyCharm 集成。各平台操作后附简短 GitHub 对照，两边用法相通。

## 一、Git 是什么

版本控制工具本质上是**团队协作工具**，解决两件事：**版本管理**和**代码分享**。

### 集中式 vs 分布式

- **SVN 是集中式版本管理**：所有版本数据集中放在一台中心服务器上，版本管理、代码分享全都依托它；这台服务器一出问题就是**单点故障**，整个团队协作瘫痪。
- **Git 是分布式版本管理**：每个成员电脑上都有一个完整的本地仓库（相当于人人自带一台 git 服务器），版本管理在本地完成，不依赖某台中心机器，单点故障问题随之解决。
- 代码的共享借助**第三方代码托管平台**：Gitee（码云，国内）、GitHub、GitLab（国外）。注意名词区分：**Git 是工具，这三个是平台**，代码托管到哪个都行。

### 安装与全局签名

安装 Git 一路"下一步"即可，装完**记住安装位置**（后面 PyCharm 集成要用）。

装好后第一件事是做全局签名设置，在 Git Bash 里、任何目录下执行都行：

```bash
git config --global user.name 你的名字
git config --global user.email 你的邮箱
```

两条命令只要不报错就是设置成功。签名就是你的**身份**：git 每次保存版本都要记录"操作是谁做的"，会把用户名和邮箱打在每次提交上，也就是 `git log` 里的 **Author** 字段。

## 二、本地仓库：从 init 到版本回退

### 2.1 四区模型（全课核心）

![git4区模型](D:\笔记之神\最近笔记\assets\git4区模型.png)

使用 git 时可以把空间分成四个区：**工作区 → 暂存区 → 本地库 → 远程库**。**所有 git 指令本质上都是让代码在这几个区之间移动**，把移动流程搞清楚，git 也就会用了。

- **工作区**：新建、修改文件的地方。平常用的 PyCharm 编辑器就可以理解为工作区。
- **暂存区**：中间空间，把"需要保存版本的代码"**暂时存储**在这里。
- **本地库**：正式保存版本记录的地方。
- **远程库**：Gitee / GitHub / GitLab 平台，用于代码分享。

前三个区都在自己电脑上，由本机 git 管理；远程库在平台上。

**暂存区为什么存在？**两个原因：

1. **挑文件、让多个文件一起提交，避免版本记录过多。**改完 a.py 先 add 进暂存区，再改 b.py 也 add 进去……改完一圈后 A、B、C、D 一次 commit 一起保存。没有暂存区的话，每改一点就得直接写进本地库记一次版本，状态记录会越来越多。
2. **给一个"反悔"的缓冲。**文件放进暂存区后又不想记录这个版本了，可以 `git rm --cached` 撤回来；而一旦 commit 进本地库，那条记录就没法抹除了。

**区间移动不能跳跃**：想保存版本必须先 工作区→暂存区（`git add`），再 暂存区→本地库（`git commit`），不能从工作区直接进本地库。

完整工作流程：**修改 → 提交暂存 → 保存版本（commit）→ 推送远程（push）→ 其他成员拉取**，每个成员都在重复这个过程。对应指令：`git add`（工作区→暂存区）、`git commit`（暂存区→本地库）、`git push`（本地库→远程库）、`git checkout`（放弃工作区修改）、`git reset`（版本回退）。

### 2.2 打开 Git Bash 与准备项目

- 桌面空白处右键 → **显示更多选项**（Shift+F10）→ **Git Bash Here**，终端就定位在所点的目录。
- 提示符格式：`用户名@主机名 MINGW64 当前目录`，`~` 表示用户主目录。
- Git Bash 是类 Linux 环境，常用 Linux 命令都能直接用：`cd`（切目录）、`ls`（列文件）、`ls -a`（含隐藏文件）、`ls -al`（更详细）、`touch`、`cat`、`echo`、`clear`（清屏）等。

新建一个文件夹 `git-2617` 当作演示项目，进入并查看：

```bash
cd git-2617
ls          # 空目录，无输出
```

想让 git 管理哪个项目，就先 `cd` 进那个项目目录再操作。

### 2.3 初始化仓库：git init

```bash
git init
# Initialized empty Git repository in C:/Users/.../git-2617/.git/
```

`git init`（init = 初始化）在当前目录创建一个空的 Git 仓库，实际做法是生成一个隐藏的 **`.git` 目录**。从此这个目录下的文件都归 git 版本管理——**之后任何新建、修改、删除文件的操作，都会被 git "监视"记录**。初始化成功后提示符出现分支名 `(master)`，表示当前在默认主分支上。

验证（Linux 下以 `.` 开头的是隐藏文件，普通 `ls` 看不到）：

```bash
$ ls -a
./  ../  .git/
```

Windows 资源管理器里需 查看 → 显示 → 勾选"显示隐藏的项目"才能看到。`.git` 目录里是 hooks、info、objects、refs 子目录和 config、description、HEAD 文件：

- 它是仓库的"数据库"：`objects` 存所有提交对象与文件内容，`refs` 存分支/标签指针，`HEAD` 指向当前分支，`config` 是本仓库配置。
- **`.git` 里的内容不需要关注，更不要手动修改**——把它隐藏起来就是让你"当它不存在"，那是 git 运行时所需的环境文件。就像用数据库只需会操作，不必深究底层怎么存储；git 也一样，会在应用层用就够了。

### 2.4 诊断命令：git status

`git status` 随时报告工作区/暂存区的状态：哪些文件被管理了、哪些被修改了、哪些需要提交。刚初始化的空仓库：

```text
On branch master
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```

`On branch master` 表示当前状态在主分支上（分支是 git 的另一个重要概念，默认只有 master 一个主分支，详见第三章）；`nothing to commit` 表示没有任何需要提交的内容。

### 2.5 跟踪文件：add 与 rm --cached

新建文件并查看状态：

```bash
touch hello.txt
git status
```

```text
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        hello.txt
```

新文件默认是**未跟踪（Untracked）**：git 已监视到你新增了它，但你还没告诉 git 要不要管理它。**颜色是关键信号：红色 = 未跟踪，或已修改但没进暂存区；绿色 = 已在暂存区。**红色不代表文件有错，git 只是用颜色表示状态。命令行输出自带命令提示（如 `use "git add" to track`），照着敲即可——这是命令行操作 git 的一大好处。

添加到暂存区（工作区 → 暂存区）：

```bash
git add hello.txt      # 成功时静默无输出
git status
```

```text
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   hello.txt      # 绿色
```

反悔、撤出暂存区：

```bash
git rm --cached hello.txt
# rm 'hello.txt'

//如果是整个目录
git rm -r --cached .
```

`git rm --cached` **只把文件从暂存区撤下、取消跟踪，不会删除工作区的文件本身**；撤完状态又回到红色 Untracked。`add` / `rm --cached` 可以在工作区与暂存区之间来回操作——这就是暂存区给的"反悔空间"。

### 2.6 提交版本：git commit

```bash
git commit -m "create new file hello.txt"
```

```text
[master (root-commit) 3bd711d] create new file hello.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 hello.txt
```

- **执行一次 commit 就记录一次版本**，把暂存区当前状态正式存入本地库。
- `-m` 就是 message（提交信息）。git 会帮你管理很多版本，不写信息后面自己都分不清哪个是什么状态；信息内容没有硬性要求，自己能看懂就行。
- 第一次提交标注 `(root-commit)`（根提交）；`3bd711d` 是提交哈希（版本号）。`1 file changed, 0 insertions(+)` 因为 hello.txt 是空文件。
- commit 之后**暂存区被清空**，工作区与本地库状态一致，再查：

```text
On branch master
nothing to commit, working tree clean
```

**只要看到 working tree clean，就证明工作区和本地库状态一致。**

顺带：命令拼错时（如 `git stauts`），git 会报 `is not a git command`，并智能提示最相似的命令（`The most similar command is status`）。

### 2.7 修改文件与放弃修改：git checkout

给文件追加一行内容（`echo` 相当于 Python 的 `print`；`>>` 是追加重定向，`>` 是覆盖写入）：

```bash
echo "hello git01" >> hello.txt
cat hello.txt        # 输出：hello git01
git status
```

```text
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)
        modified:   hello.txt      # 红色
```

这次的红色和上次的不同：这是 **modified（已修改、未暂存）**——已被跟踪的文件被改动了，工作区和本地库记录的状态出现了差异。此时有两条路：`git add` 把修改放进暂存区等待提交，或 `git checkout` 放弃修改。

**放弃工作区的修改：**

```bash
git checkout hello.txt
# Updated 1 path from the index
```

checkout 用暂存区/本地库中记录的内容**覆盖**工作区文件，文件直接回到上次提交时的样子（这里是空文件）。典型场景：打开一个 `.py` 改了很多处，发现没解决问题、想恢复原状——靠手工回忆"改了哪里"不现实，`git checkout` 一条命令就回去了。恢复后 `git status` 回到 working tree clean。

### 2.8 git add . 与后续提交

**不是每改一行都要 add。**现在还不想记录状态就继续改（第 2 版、第 3版……），觉得可以了再 add；add 进暂存区的东西会随下一次 commit 被记录。

项目里文件多了以后，逐个写文件名太麻烦，用：

```bash
git add .
```

把当前目录下**所有**改动一并加入暂存区——**这是实际使用频率最高的写法**。Windows 上可能看到：

```text
warning: LF will be replaced by CRLF in hello.txt.
```

这是换行符转换警告（Git Bash 写出的文件用 Unix 换行符 LF，git 入库时会转换），不是错误，可忽略。

接着提交，完成第二次版本记录：

```bash
git commit -m "add one line in hello.txt"
# [master d9461b2] add one line in hello.txt
#  1 file changed, 1 insertion(+)
```

再新建 hi.txt，重复同样流程：

```bash
touch hi.txt
git add .
git commit -m "create new file hi.txt"
# [master a3b894d] create new file hi.txt
```

至此仓库里有 3 次提交。平时用 git 基本就是这个循环：**add → commit；想放弃修改就 checkout。**

### 2.9 查看历史：git log

回退版本前得知道要回到哪个版本，先查提交历史：

```bash
git log
```

```text
commit a3b894d7cef5338d5e977d13345011e607f573dc8 (HEAD -> master)
Author: mengshun <985732654@qq.com>
Date:   Mon Nov 23 15:05:20 2026 +0800

    create new file hi.txt

commit d9461b2f56458fd26a7a204ed1e0140620b56824
    ...（从新到旧列出全部提交）
```

每条提交的固定结构：

- **commit + 40 位哈希**：这条提交的**唯一标识**，类似数据库表的主键 id——后面写它就代表这个版本。**复制时不必抄全 40 位，取能在仓库中唯一区分的前缀（一般前 7 位以上）即可。**
- **Author**：提交人及邮箱，来自最初的全局签名设置——这就是"签名 = 身份"的落点。
- **Date**：提交时间。
- 缩进的**提交信息**：`commit -m` 写的内容。
- 最新一条后面的 `(HEAD -> master)`：**HEAD 是"你当前在哪个提交"的指针**，现在指向 master 分支的最新提交。看当前是什么状态，就看 HEAD 指向谁。

### 2.10 版本回退：git reset --hard

类似写论文：论文有 1.0、8.0、10.0 各版，想从 10.0 回到 8.0，用 `git reset` 加上从 log 里找到的版本号：

```bash
git reset --hard 3bd711d48622d
# HEAD is now at 3bd711d create new file hello.txt
```

`--hard` 表示回退时**把磁盘上的文件也"硬"回到目标版本**——工作区、暂存区、本地库一起变。回退到第一次提交后：hi.txt 直接消失（它是 a3b894d 才创建的），hello.txt 变回空文件（追加的那行属于 d9461b2，一并被撤销）。

注意两点：

- `--hard` 是强制的，回退点之后的内容会从工作区消失，操作前确认好版本号。
- 回退后再 `git log`，"未来"的提交从列表里消失了，只剩当前及更早的。

💡 核心问题：这些“未来”的提交真的被删除了吗？

**没有！Git 几乎是不会主动删除数据的。**

`提交B` 和 `提交C` 其实并没有被真正销毁，它们只是**成了“孤岛”**（没有被任何分支指针引用），被 Git 藏起来了。  
如果你后悔了，想回到“未来”，只要你还记得 `提交C` 的版本号（比如 `a1b2c3d`），或者你通过 `git reflog`（记录了你所有的操作历史）找到了 `提交C`，你依然可以通过 `git reset --hard a1b2c3d` 再穿越回去。

### 2.11 找回"消失"的提交：git reflog

回退之后想再回到原来的新版本怎么办？log 里版本号没了，但**记录并没有丢**：`git log` 只能看到"从当前版本往前"的历史，而 **`git reflog` 记录 HEAD 的每一次移动**，包括被 reset 掉的提交和 reset 动作本身：

```bash
$ git reflog
3bd711d (HEAD -> master) HEAD@{0}: reset: moving to 3bd711d48622d
a3b894d HEAD@{1}: commit: create new file hi.txt
d9461b2 HEAD@{2}: commit: add one line in hello.txt
3bd711d (HEAD -> master) HEAD@{3}: commit (initial): create new file hello.txt
```

从 reflog 里复制目标版本号，再 reset 一次——**`git reset --hard` 既能回退也能"前进"**：

```bash
git reset --hard a3b894d
# HEAD is now at a3b894d create new file hi.txt
$ cat hello.txt
hello git01
```

hi.txt 回来了，hello.txt 的内容也回来了。**结论：只要在 git 里提交过的记录就不会丢，用 `reset --hard` 可以在这些版本之间任意穿梭，想回到哪个版本就回到哪个版本。**

小提醒：`reflog` 是一个单词，写成 `git ref log` 会报错（git 会提示最相似命令是 reflog）。

## 三、分支：给不确定的开发开一条副本

### 3.1 为什么需要分支

到昨天为止，所有提交都落在 `master`（主分支）上。单人开发没问题，**多人协作就出问题了**：

- 一旦 `commit`，版本记录就**持久化**了，**没法消除**。
- 假设现在做的是 v1.0，已开发完成；接着在 v1.0 基础上做 v2.0 的新功能，而这个功能**能不能做成是不确定的**。
- 如果照样往 `master` 上提交：做成了——皆大欢喜；**做失败了——这一堆无效提交、垃圾数据就永久留在 master 上了，把主分支"污染"了**。
- 一个团队里每个成员都这么干，master 上的提交记录会越来越乱。

**分支就是解决这个问题的机制**：以某个位置为**基点**"拷贝"一份出来，在副本上折腾，成功了才合并回去，失败了直接丢掉——主分支毫发无损。

### 3.2 分支的基本理念（用图画出来更容易懂）

```text
master:  ●───●───●  (v1.0 完成) ──●───●───●
                 │
                 └──●───●───●   dev 分支（开发 v2.0）
```

- `dev` 分支创建的那一刻，它的状态和 `master(v1.0)` **完全一样**——就是这个"基点"。
- 之后切到 `dev` 上写代码、提交，`master` 那边**一点不受影响**。
- v2.0 做成了 → 把 `dev` **合并**回 `master`；做失败了 → **不合并**，`dev` 直接删掉，`master` 还停在 v1.0。

**常见用法**：

- `master` 当**生产环境分支**：只放发布用的、可用的代码，提交记录永远干净。
- 开发新功能就开一个开发分支（如 `dev`），开发完再合并。
- 团队里**每个成员一个分支**：负责人给你开一条分支，你改到面目全非也影响不到别人的代码。
- **修 bug 也能开分支**：线上出问题，从出问题的位置开一条分支调通，再合并回去。
- 分支**可以再分叉**：`dev` 上还能继续开分支，结构想多复杂都行，按实际需要开。

> 求职角度的一句实话：公司给你派任务时敢把项目交给你乱改，靠的就是这种"分支隔离"。担心"改坏了怎么办"是多余的——只要对方用 git，你就是改坏了也在自己的分支里。

### 3.3 查看分支：`git branch`

```bash
$ git branch
* master
```

`git branch` 列出本地所有分支，**`*` 号打在你当前所在的分支上**。刚 `init` 的仓库默认只有一个 `master`。

`git log` 里也能看出当前在哪：

```text
commit df364ccb811189de8ce8bf99833adfb76a6f595a (HEAD -> master)
```

`(HEAD -> master)` = 当前提交由 HEAD 指向、且位于 master 分支。

### 3.4 创建分支：`git branch 分支名`

```bash
$ git branch dev        # 无输出即成功
$ git branch
  dev
* master
```

注意两点：

1. **创建 ≠ 切换**。`git branch dev` 只是造了一条新分支，`*` 还挂在 `master` 上——你人还在原地。
2. 新分支的**基点是当前提交**：你在 master 的哪个位置敲这条命令，dev 就从哪里长出来。

### 3.5 切换分支：`git checkout 分支名`

```bash
$ git checkout dev
Switched to branch 'dev'
```

切换成功的标志是**提示符括号里的分支名变了**：

```text
mengshun@mosin MINGW64 ~/Desktop/git_2617 (dev)
```

切过去之后 `git log` 能看到继承来的历史记录（因为基点是 master，master 那两次提交自然是 dev 的祖先）。

> 小贴士：新版本 git 推荐用 `git switch dev` 代替 `git checkout dev`（`checkout` 一个词兼管"切分支"和"还原文件"两件事，容易混）。课堂演示用的是 `checkout`，两者效果一样。

### 3.6 分支隔离到底有多"隔离"——亲手验证一次

在 `dev` 分支上新建文件并提交：

```bash
$ touch hi.txt
$ git status
On branch dev
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        hi.txt

$ git add .
$ git commit -m "add dev branch"
[dev d27e629] add dev branch
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 hi.txt
```

现在**切回 master**：

```bash
$ git checkout master
Switched to branch 'master'
```

打开文件夹一看——**`hi.txt` 不见了**，只剩原来的 `hello.txt`；`git log` 也还是 2 条记录。这就是隔离：**dev 上的操作完全没影响到 master**。

再切回 `dev`：

```bash
$ git checkout dev
```

`hi.txt` 又回来了，`git log` 变成 4 条。**代码没丢，它只是"住"在 dev 分支里。**

再在 dev 上改一次、提交（让 dev 比 master 多走几步）：

```bash
$ echo "hi dev01" >> hi.txt
$ git add .
$ git commit -m "add one line in hi.txt"
```

### 3.7 合并分支：`git merge 被合并的分支名`

v2.0 开发完成，想把 dev 的成果搬到 master 上。**合并的方向极容易搞反，记死这句话：**

> **先 `git checkout` 到"要接收代码"的那个分支（这里是 master），再 `git merge` "提供代码"的分支（这里是 dev）。**
> 站在谁身上，就合并谁——`merge` 后面跟的是**来源**，不是去处。

```bash
$ git checkout master          # 先站到 master 上
$ git merge dev
Updating df364cc..d27e629
Fast-forward
 hi.txt | 1 +
 1 file changed, 1 insertion(+)
 create mode 100644 hi.txt
```

输出怎么读：

- `Updating df364cc..d27e629`：master 从旧提交 `df364cc` **前进到** `d27e629`。
- **`Fast-forward`（快进合并）**：因为 master 在分叉后**一步都没动过**，git 不需要真正"合并"内容，直接把 master 指针往前挪到 dev 的位置就行——历史是一条直线，最省事的情况。如果两边都各自提交过，就不会是快进，git 会生成一次新的合并提交（两边改到同一处才会冲突，课上未展开）。
- `hi.txt | 1 +`、`create mode 100644 hi.txt`：把 dev 上新增的文件带过来了。

合并后 `git log` 在 master 上也能看到 4 条记录了，`hi.txt` 也躺在 master 的工作区里。

### 3.8 删除分支：`git branch -d 分支名`

代码已经合并进 master，dev 就没必要留着了：

```bash
$ git branch -d dev
Deleted branch dev (was d27e629).
$ git branch
* master
```

- `-d` = delete。`was d27e629` 告诉你这条分支最后指向哪个提交——**分支是一个指针，删分支只是删指针，已经合并进 master 的代码一个字节都不会少**。
- 用 `-D`（大写）是**强制删除**：分支上还有没合并的提交时，`-d` 会拒绝（保护你），`-D` 会硬删。**没合并就 `-D`，那些提交就真的找不回来了**（只能靠 `git reflog` 抢救，且有时效）。
- 删之前**不能站在被删的分支上**：想删 dev，得先切到 master。

### 3.9 完整循环（记这一套就够了）

```bash
git checkout master       # 1. 从干净的主分支出发
git checkout -b dev       # 2. 建并切到新分支（= git branch dev + git checkout dev 两步合一）
# ……写代码、add、commit……
git checkout master       # 3. 切回主分支
git merge dev             # 4. 合并（站在 master 上合并 dev）
git branch -d dev         # 5. 合并完删掉临时分支
```

`git checkout -b 分支名` 是建分支+切分支的合体写法，日常用得最多。

> **GitHub 对照**：分支机制是 git 内核功能，与平台无关，命令完全一样。只有两点差异：① GitHub 默认主分支叫 `main`（Gitee 是 `master`），所以命令里换成 `main`；② GitHub 网页上合并通常走 **Pull Request** 流程（提 PR → 评审 → 点 Merge），Gitee 对应叫 **Pull Requests** 页面，本质都是"把某条分支合并到主分支"。

## 四、远程仓库 Gitee/Github：建库、推送、克隆与协作

### 4.1 为什么需要远程仓库

之前的操作全在本地完成。代码怎么分享给别人？笨办法是打压缩包发过去——上 20 天课就得发 20 个包，接收方往往只需要最后一个。用 git 则是把代码像传网盘一样每天推到远程平台，别人沿链接来取，而且 **git 做的是增量更新**，每次只传变化的部分。

至此 git 的两大作用齐了：**版本管理在本地完成**（装了 git 就能管）；**代码分享靠远程库**（push 上去、别人拉下来）。平台三选一：Gitee（码云，国内）、GitHub、GitLab（国外，需要相应网络条件）。本部分以 Gitee 为例。

### 4.2 Gitee 课前准备：绑定账号 + 添加邮箱

注册并登录 `gitee.com` 后，先进个人设置（头像 → 个人设置）做两件事：

1. **帐号管理里绑定至少一个第三方账号**（QQ、微信等）：便于账号安全找回；而且**不绑定的话推送代码可能失败**（会提示未绑定账号）。
2. **邮箱管理里添加自己的邮箱**：网页支持手机号登录，但 **PyCharm 登录只认邮箱**——这就是要提前绑邮箱的原因。添加后建议把"不公开我的邮箱地址"取消勾选。

> **GitHub 对照**：登录 github.com 后头像菜单 → Settings；邮箱在 Settings → Emails，对应开关是 "Keep my email addresses private"（勾选后网页操作使用 `<ID>+<username>@users.noreply.github.com`）。GitHub 没有 Gitee 式第三方账号绑定列表，等价能力是 Sign in with Google/Apple 或 OAuth 应用授权。

### 4.3 网站上新建仓库

仓库可以理解为**在平台里创建的一个文件夹**，用来存放你要推上去的项目代码——就像本地不会把所有文件堆在硬盘根目录，平台上的代码也按仓库分门别类管理。

入口：头像下拉 → 我的仓库，或任意页面右上角 **「+」→ 新建仓库**。表单要点：

- **仓库名称**：一般与项目名保持一致（项目叫 git-2617，仓库就叫 git-2617）；**同一账号下必须唯一**。仓库地址 = `个人空间/仓库名`（如 `https://gitee.com/classmeng/git-2617`）；账号名唯一 + 仓库名唯一 ⇒ 全世界的项目链接不会重复。
- **仓库介绍**：简单写清这个"文件夹"存的是什么即可。
- **可见性**：**开源（所有人可见）**——链接发给谁都能看，无人数限制；**私有（仅仓库成员可见）**——适合不想泄露代码、含私有数据的公司项目，但有人数限制（约 5 人，超出需付费扩容）。
- **初始化选项**（语言、.gitignore、开源许可证、模板、分支模型）：**选不选都行**。勾选后平台会替你初始化对应文件，比如 .gitignore 用来声明哪些文件不提交、不管理；分支模型不用管，默认就在 master 分支上。**本地已经有提交的情况下不要勾选**，避免远程生成的初始提交与本地历史冲突。

填好名称、选开源，点「创建」即可。

> **GitHub 对照**：+ → New repository（github.com/new），字段一一对应：Repository name / Owner / Description / Public or Private；初始化选项对应 "Add a README file / Add .gitignore / Choose a license"。注意默认分支差异：Gitee 用 `master`，GitHub 现在默认 `main`。

### 4.4 空仓库引导页：真正要做的只有两条命令

创建成功后是空仓库引导页，给出三段命令行入门：

```bash
# ① Git 全局设置（已配置过，跳过）
git config --global user.name "meng"
git config --global user.email "985732654@qq.com"

# ② 创建 git 仓库（从零开始的项目才需要；mkdir/init/add/commit 本地都做过了，跳过）
mkdir git-2617
cd git-2617
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin https://gitee.com/classmeng/git-2617.git
git push -u origin "master"

# ③ 已有仓库?（本地已有提交，用这一段）
cd existing_git_repo
git remote add origin https://gitee.com/classmeng/git-2617.git
git push -u origin "master"
```

本地仓库早就建好、提交过了，所以**真正需要做的只有最后两条：`git remote add origin` 和 `git push`**。

> **GitHub 对照**：新建空仓库后的 Quick setup 页结构几乎一样，URL 换成 `https://github.com/<user>/<repo>.git`，推送用 main 分支：`git branch -M main`、`git push -u origin main`。

### 4.5 关联远程：git remote add origin

回到 Git Bash（本地 git-2617 目录下）执行：
origin是别名,如果想同步到另一个仓库,不能再这这个别名

```bash
git remote add origin https://gitee.com/classmeng/git-2617.git
```

**无任何输出即成功。**这条命令让本地仓库和远程仓库**建立关联**（一条连接通道），之后本地的代码和版本信息才能推上去。`origin` 只是**给这一长串地址起的别名**（惯例名）——后面写 origin 就代表这个仓库地址，push/pull 不必每次敲完整 URL。关联只需做一次，推送则会经常做。

> **GitHub 对照**：命令完全相同，URL 换成 `https://github.com/<user>/<repo>.git` 即可。

### 4.6 第一次推送：git push

```bash
git push -u origin "master"
```

```text
Enumerating objects: 8, done.
...
Writing objects: 100% (8/8), 675 bytes, done.
To https://gitee.com/classmeng/git-2617.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

- `git push` 就是流程图里"本地库 → 远程库"那条箭头：把本地提交推送到 origin 的 master 分支。
- `* [new branch] master -> master`：第一次推送在远程**新建了 master 分支**——仓库一旦有提交推送，就会多出默认主分支，目前只有这一个分支。
- `-u`（= `--set-upstream`）把本地 master 与远程 origin/master 建立**跟踪关系**：此后 `git status` 能对比本地与远程（显示 up to date / ahead），同分支再推送直接 `git push` 就行。

回浏览器刷新仓库页，文件和提交记录都已上去（首次访问可能弹出 Gitee 风控滑块验证，拖完即可）。至此推送全流程跑通。

> **GitHub 对照**：命令与输出格式相同，仅 URL 和 `remote:` 行不同；默认分支为 main 时首推用 `git push -u origin main`。

### 4.7 日常节奏：add → commit → push

之后不断重复：**写了代码就 add，add 完就 commit，commit 之后再 push。步骤不能跳，也跳不了**——没有 add，commit 就没有暂存区数据可提交；没有 commit，push 就没有内容可推。分享节奏也很简单：**只要写了新代码就 push**，把仓库链接发给团队，别人沿链接点进去看到的永远是最新代码。

演示：本地给 hi.txt 追加一行并推送第二次：

```bash
$ echo "hi git01" >> hi.txt
$ git add .
$ git commit -m "add one line in hi.txt"
[master 90e3cb8] add one line in hi.txt
 1 file changed, 1 insertion(+)

$ git status
On branch master
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
nothing to commit, working tree clean
```

- `Your branch is up to date with 'origin/master'` / `ahead ... by 1 commit`：推送建立跟踪关系后，git 会自动对比本地与远程；这里提示本地领先 1 个提交，"本地有、还没推"。

再推一次：

```text
$ git push
To https://gitee.com/classmeng/git-2617.git
   a3b894d..90e3cb8  master -> master
```

`a3b894d..90e3cb8` 表示远程 master 从旧提交**前进到**新提交。刷新网页，hi.txt 内容已更新。


补充:一文件夹想上传两个仓库
```
git remote add gitee https://gitee.com/ckingyyds/buy-list.git
git remote add origin https://github.com/ckingyyds/buy-list.git

git remote 




```

### 4.8 推送前必须先更新（团队协作铁律）

昨天的顺序是 `add → commit → push`，**单人开发够了，多人协作还差一步**：

```text
正确顺序： add → commit → **pull（先更新）** → push
```

**为什么**：你改代码的时候，队友可能已经改完推上去了。这时你本地和远程已经**不一致**，直接 push 会被拒。

用一个具体例子看清（成员 A/B/C，提交编号用 1.0、2.0 表示）：

```text
① A 推送了一次                    → 远程是 1.0
② B 拉取一次                      → B 本地是 1.0
③ C 也拉取一次                    → C 本地是 1.0
④ B、C 各自开发自己的任务
⑤ C 先开发完，推送成功            → 远程变成 2.0
⑥ B 也开发完了，直接推送 —— 失败！因为 B 本地还停在 1.0，远程已经是 2.0
```

B 要做的：**先 `git pull` 把本地的 1.0 变成 2.0 的状态，再 push**。

补充两点容易误解的：

- **没更新也能推成功，说明这期间远程根本没别人改过**（你是第一个）。所以不是"必须更新"，而是"**更新是唯一能保证推得上去的做法**"。
- 你每次开始干活前拉的那一次，其实就相当于"推送前的那次更新"——但**改完到推送这段时间里别人仍可能推**，所以推送前再拉一次最稳。

```bash
git add .
git commit -m "描述"
git pull              # 先更新，把远程的新提交合并到本地
git push              # 再推送
```

### 4.9 拉取代码：git clone

别人要拿你的代码，最好拉到本地。仓库页右上「克隆/下载」按钮里有 HTTPS 地址和现成命令（弹窗还提示：使用 HTTPS 时建议配置**私人令牌**替代登录密码）：

```bash
git clone https://gitee.com/classmeng/git-2617.git
```

**希望代码放在哪个目录，就在哪个目录下执行**：想放桌面就在桌面右键 Git Bash Here 再 clone，仓库会下载到当前目录。clone 的另一个用途：**本地代码误删了，从远程重新 clone 一份即可恢复**。

常见报错：

```text
fatal: destination path 'git-2617' already exists and is not an empty directory.
```

目标目录已存在且非空（比如桌面上已有同名 git-2617）。解决：删掉/移走旧目录再克隆；或 `git clone <url> <新目录名>` 指定不同目标路径。

> **GitHub 对照**：`git clone` 是 git 本身的行为，与平台无关，URL 换成 `https://github.com/<user>/<repo>.git` 即可。注意 GitHub 自 2021-08-13 起 HTTPS 操作不再接受账号密码：Username 填 GitHub 用户名，Password 粘贴 **Personal Access Token**（Settings → Developer settings → Personal access tokens 生成），或由 Git Credential Manager 首次弹浏览器授权——与 Gitee"私人令牌替代密码"同理。

### 4.10 仓库成员与协作权限

开源仓库**任何人拿到链接都能拉取代码（只读），但推送不了（没有写权限）**——能看不能写。团队协作靠给仓库**添加成员**：仓库页 → 管理 → 仓库成员管理。

- **默认谁创建仓库，谁就是所有者和管理员。**
- 成员权限分**管理员 / 开发者 / 观察者**：希望别人也能提交代码，添加为**开发者**（团队成员一般都加开发者，大家都能拉取、都能提交）；只希望他看，就是**观察者**。
- 邀请方式三种：**链接邀请**（可选权限、是否需管理员审核，链接有效期 3 天）、**二维码**、**直接添加**（按对方 Gitee 账号搜索）。只要团队成员有 Gitee 账号就能邀请；被添加后拉取代码、编辑、提交推送都有对应权限。

> **GitHub 对照**：个人仓库成员管理在 Settings → **Collaborators** → Add people（按 username / 邮箱搜索发邀请，7 天未接受自动过期）；公开邀请链接只有**组织**仓库才有（Org → People → Create invitation link）。角色差异：Gitee 个人仓库分管理员/开发者/观察者；GitHub 个人仓库的 collaborator 一律 write 权限，五级角色 Read / Triage / Write / Maintain / Admin 仅用于组织仓库。

### 4.11 团队协作的完整闭环（负责人视角 + 成员视角）

4.10 讲了怎么把人加进来，这里讲加进来之后**怎么真正协作起来**。

**为什么要有"负责人"**：团队不能三个人各建各的项目——包名不一样、命名规范不一样、目录结构不一样，最后根本合不到一起。所以流程固定为：

**① 负责人（仓库所有者）**

1. 本地先把项目骨架搭好（目录结构、包名、规范都定下来），写好初始代码。
2. 把这个项目推送到远程仓库（命令行 `remote add` + `push`，或 PyCharm 一键分享，见 5.6）。
3. 仓库 → 管理 → 仓库成员管理 → **开发者** → 邀请用户 → 复制链接发群里。
4. 成员自己点链接接受邀请，**账号下的仓库列表里就会多出这个仓库**（不是他的也变成"能看的"了）。

**② 团队成员**

1. 拿到仓库地址（「克隆/下载」里的 HTTPS 地址），用 `git clone` 或 PyCharm 的 Project from Version Control 拉到本地。
2. 在项目里写自己的代码（比如建个 `张三.py`，写点内容）。
3. `commit` → **`pull`** → `push`（顺序见 4.8）。**能推上去，就说明你真的是成员了**——公开仓库的路人只能拉不能推。

**③ 负责人收代码**

成员推上来之后，负责人本地**不会自动变**，必须自己点一次 **Update Project（Ctrl+T）** 或 `git pull`，才能把别人的代码拉到本地。

**④ 之后就是循环**：谁改了代码 → commit → pull → push；其他人 Update。仓库就此变成团队共享的最新代码。

### 4.12 仓库页其他常用功能

- 导航标签：代码 / Issues / Pull Requests / Wiki / 统计 / 流水线 / 管理。
- 文件页支持：在线编辑、原始数据、按行查看（Blame）、提交历史。
- 管理（仓库设置）：改路径（**改后原克隆地址失效**）、介绍、默认分支、开源/私有切换、成员管理、WebHooks、保护分支等。

> **GitHub 对照**：导航对应 Code / Issues / Pull requests / Actions / Projects / Wiki / Security / Settings；文件页按钮对应 Edit / Raw / Blame / History；删仓库在 Settings → Danger Zone，分支保护在 Settings → Branches。

## 五、在 PyCharm 里用 Git（Gitee 插件）

日常开发在 PyCharm 里进行，把 PyCharm 与 git/Gitee 集成后，拉取、更新、推送都能在工具里点几下完成。集成共四步设置（File → Settings，Ctrl+Alt+S），外加克隆与更新两个日常操作。

### 5.1 文件编码

**Editor → File Encodings**：Global Encoding、Project Encoding、Default encoding for properties files 全设 **UTF-8**，勾选 Transparent native-to-ascii conversion，Create UTF-8 files 选 **with NO BOM**。可避免中文文件与提交信息乱码。（此项与平台无关，用 GitHub 也应同样设置。）

### 5.2 安装 Gitee 插件

**Settings → Plugins → Marketplace** 搜 `gitee`，安装 **Gitee** 插件（功能与 GitHub 插件类似）。装完后 **Version Control 设置里会多出一个 Gitee 选项**——装之前是没有的。

> **GitHub 对照**：GitHub 插件是 JetBrains IDE **内置**的，无需安装，直接在 Settings → Version Control → GitHub 使用；GitLab 同样不必额外集成。

### 5.3 配置 Git 可执行文件路径

**Settings → Version Control → Git**：Path to Git executable 填自己的 git 安装位置，如 `D:\Git\bin\git.exe`（常见为 `C:\Program Files\Git\bin\git.exe`），点 **Test** 能显示版本号即成功。

这一步让 PyCharm 识别本地安装的 git 工具——之后 PyCharm 就是**利用你本地的 git 环境**帮你做版本管理。这也是安装 git 时要记住安装位置的原因：按 安装盘 → Git → bin → git.exe 的层级找到它。

### 5.4 登录 Gitee 账号

**Settings → Version Control → Gitee → +**，两种登录方式任选其一：

- **Log In via Gitee...（OAuth 授权，最方便）**：自动打开浏览器到 Gitee 授权页，登录授权后回到 PyCharm，账号已添加进列表。
- **Log In with Password**：Login 填**邮箱**——网页可以用手机号登录，但 PyCharm 老版本识别不了手机号，必须用邮箱（这就是 4.2 提前绑邮箱的原因）；Password 填 **Gitee 网站设置的密码**，不是邮箱密码。

账号已存在时重复登录会报 `Account 'classmeng' already added`，关闭对话框即可。

> **GitHub 对照**：Settings → Version Control → GitHub → + 提供 Log In via GitHub...（OAuth，浏览器授权）与 Log In with Token...（粘贴 Personal Access Token）。

### 5.5 克隆仓库为项目

**File → Project from Version Control...**（欢迎页上是 "Get from Version Control" 按钮），两种方式：

- **拉自己的代码**：点左栏已登录的 **Gitee 账号**，会自动列出账号下全部仓库（列表加载需稍等），点选目标仓库即可，不用手填 URL。
- **拉别人的代码**：左栏选 **Repository URL**，把仓库页「克隆/下载」里的 HTTPS 地址贴进 URL 框，Directory 自动变为 `目录\仓库名`。

Directory 按自己的存放习惯选；建议**自己写的代码和拉取的别人代码分开放**（比如单独建一个目录存课程代码）。点 **Clone** 开始克隆——**工具背后执行的就是 `git clone` 命令**，这就是用工具和敲命令的区别：工具不用记命令，点一下就行。

克隆完成弹出 Open Project 对话框，问项目怎么打开：**This Window**（关掉当前项目、在本窗口打开）/ **New Window**（新窗口）/ **Attach**（挂到已打开的项目上）。打开后项目树里就是拉下来的文件，状态栏显示当前分支 master。

> **GitHub 对照**：同一个对话框，左栏点 GitHub（已登录）同样直接列出自己的仓库点选克隆，或用 Repository URL 填 GitHub 地址；Open Project 三选项完全一致。

### 5.6 分享新项目：Share Project on Gitee（一键推上去）

昨天讲的是"**从远程拉下来**"（克隆），这里补"**把本地项目推上去**"。

命令行要三步：远程网页建仓库 → `git remote add origin` → `git push -u`。
PyCharm 里**一步搞定**：本地初始化 + 远程建同名仓库 + 首次推送，全自动。

**入口**：顶部菜单 **VCS → Share Project on Gitee**（VCS = 版本控制，即昨天提的"渦身扛住/Version Control"）。

弹出的对话框要填：

- **Repository name**：远程仓库名（默认取项目名，可改）。
- **Remote name**：默认 `origin`——就是远程地址的别名，和命令行里那个 origin 是同一个东西。
- **仓库描述（Description）**：填一句简介，会写进远程仓库的介绍里（相当于 4.3 建库时填的"仓库介绍"）。
- **Commit message**：本次提交的说明，等价于 `git commit -m "..."`（比如填"项目初始化"）。
- **文件勾选列表**：默认会把项目里所有文件都选上，**要把 `.idea` 那个勾去掉**——那是 PyCharm 自己的工程配置（解释器路径、窗口布局之类），属于"本机设置"，不该推给别人，别人拉下来也用不上。

点 **Add**（或 Share）之后，PyCharm 自动完成：`git init` → `git add` → 首次 commit → 在 Gitee 上建同名仓库 → `remote add` → `push`。完成后菜单里那一项会变成灰色的 "Successfully shared project"。

**一个坑：分享出来的仓库默认是私有的。** 打开仓库链接会返回 **403（没权限）**。解决：

> 仓库页 → **管理** → 可见性改成 **开源** → 点 **保存**。回到仓库页，名字前面那把锁消失，就是公开仓库了，把链接发给谁都能访问。

（私有仓库有人数上限（约 5 人），团队人多就必须开源，或者去买扩容。）

> **GitHub 对照**：菜单项是 **Git → GitHub → Share Project on GitHub**（插件内置，无需安装）。对话框多一项 **Private/Public** 选择，可以直接建成公开仓库，不用像 Gitee 这样建完再改。其余流程（去勾 `.idea`、填 commit message）完全一致。

### 5.7 更新项目：Update Project

远程（比如老师）push 了新提交后，本地**不用重新 clone，打开项目点一次更新**即可：

- 入口：工具栏 Git 组第一个蓝色下拉箭头按钮，**Update Project（Ctrl+T）**。
- 对话框两个选项：**Merge incoming changes into the current branch**（默认，合并，对应 `git pull`，保留分支历史）/ **Rebase the current branch on top of incoming changes**（对应 `git pull --rebase`，把本地提交垫到远程提交之后，历史更线性）。初学直接 OK 用默认 Merge 即可；分支概念见 3.1–3.2，合并后历史的两种形态（快进 / 合并提交）见 3.7。
- 完成后右下角通知 **"1 file updated in 1 commit"**，打开文件就能看到远程新内容。

使用节奏：第一次拿链接 clone 下来，之后每次远程有更新，打开项目点一下 Update 就是最新代码。

> **GitHub 对照**：Update Project 是 IDE 的通用 Git 功能，与平台无关，GitHub 用户用法、快捷键完全相同。

### 5.8 PyCharm 内的提交与推送

提交和推送也可以直接在 PyCharm 里完成：**Ctrl+K 提交（Commit）、Ctrl+Shift+K 推送（Push）**，操作与平台无关（Gitee/GitHub 通用）。具体操作见下面 5.9–5.12。

下面把 5.8 一句话带过的提交与推送展开，并补上看历史、回退分支等日常操作。

### 5.9 文件颜色 = 状态（和命令行的红绿完全对应）

- **红色**：新建但**未被 git 管理**（对应 `Untracked`），或已修改但没进暂存区。PyCharm 会弹窗问你"要不要加入 git"——**勾上"不再询问"再点 Add**，以后新建文件就自动加入暂存区，不用手动 add 了（这是比命令行省事的地方）。
- **蓝色/绿色**：已进暂存区（对应命令行的绿色）。
- **正常颜色（无高亮）**：已提交、工作区和本地库一致（对应 `nothing to commit, working tree clean`）。

### 5.10 Commit 面板：commit 还是 commit and push

点提交按钮（或 Ctrl+K）打开 Commit 面板：左边勾选要提交的文件，中间填提交信息，然后有两个按钮：

- **Commit**：只保存到**本地库**（= `git commit`），不推送。
- **Commit and Push**：保存到本地库**同时推到远程**（= `git commit` + `git push`）。**日常点这个**——代码改完不就是为了分享出去吗。

点 Commit and Push 后还会再弹一次确认"要 push 到哪个远程/分支"，点 **Push** 即可。推送成功后那些文件的颜色会恢复正常（因为工作区和远程一致了）。

### 5.11 Log 面板：看提交历史 + 版本穿梭

**入口**：左下角 **Git / Version Control** 工具窗口 → **Log** 标签页，能看到全部提交记录（谁、什么时候、改了什么）。

双击某个提交可以看它改动的文件；**右键某个提交 → `Reset Current Branch to Here...`**，就对应命令行的 `git reset --hard 版本号`：

- 弹窗里选 **Hard**（硬重置：本地库指针、暂存区、工作区文件**全部**退回去，慎用，未提交的改动会没了）。
- 面板里那三个标识要知道：**`master`** = 分支指针，**`HEAD`** = 当前所在的提交。**HEAD 在哪，工作区的文件就是哪个版本**——这就是"版本穿梭"的原理。

### 5.12 分支操作全在右下角

**入口**：窗口**右下角状态栏**的 `Git: master`（显示当前分支名），点它：

- **New Branch**：新建分支。填名字（如 `dev`）→ 点 **Create**，PyCharm 会**建好并自动切过去**（等价于 `git checkout -b dev`）。切成功后右下角会变成 `Git: dev`。
- **Checkout**（列表里点其他分支）：切换分支（= `git checkout 分支名`）。切到 master，dev 上新增的文件立刻从工作区消失——和命令行表现一模一样。

所以昨天命令行学的每条指令，在 PyCharm 里都能找到对应按钮：**按钮替你把命令敲了，但面试和排错时你还得知道底下执行的是哪条命令。**

> **GitHub 对照**：Log 面板、Reset、分支切换、Commit 面板全是 IDE 的通用 Git 功能，与平台无关，Gitee/GitHub 用户操作完全一样。

## 六、四区流程图

贯穿全部内容的核心流程（建议背下来）：

```text
[工作区]  --git add .-->  [暂存区]  --git commit -m '版本信息'-->  [本地库]  --git push-->  [远程库 Gitee/GitHub]
 新增/修改                 暂存区文件                   版本记录                     代码分享

反向操作：
  暂存区 --git rm --cached 文件--> 工作区        （撤销暂存）
  工作区 <--git checkout 文件--                  （放弃修改）
  工作区 <--git reset --hard 版本号-- 本地库      （版本回退，长箭头）
```

| 方向 | 命令 | 说明 |
|---|---|---|
| 工作区 → 暂存区 | `git add .` / `git add 文件` | 把修改放入暂存区 |
| 暂存区 → 本地库 | `git commit -m '版本信息'` | 记录一次版本 |
| 本地库 → 远程库 | `git push` | 推送代码到远程仓库 |
| 放弃工作区修改 | `git checkout 文件` | 用暂存/库中内容覆盖工作区 |
| 撤销暂存 | `git rm --cached 文件` | 从暂存区退回工作区，不删文件 |
| 版本回退 | `git reset --hard 版本号` | 强制回到历史版本（也能"前进"） |
| 远程 → 本地 | `git clone url` / `git pull` | 克隆仓库 / 拉取更新 |

远程库一列既可以是 Gitee 也可以是 GitHub：四区模型与全部命令完全一致，换个 URL 而已。

## 七、命令速查

| 命令 | 作用 | 要点 |
|---|---|---|
| `git config --global user.name "名字"` | 全局签名（身份） | 记入每次提交的 Author 字段 |
| `git config --global user.email "邮箱"` | 全局邮箱 | PyCharm 登录 Gitee 也用它 |
| `git init` | 初始化本地仓库 | 生成隐藏 `.git` 目录；提示符出现 `(master)` |
| `git status` | 查看工作区/暂存区状态 | 红 = 未跟踪或修改未暂存；绿 = 已暂存；clean = 工作区与本地库一致 |
| `git add 文件` / `git add .` | 工作区 → 暂存区 | `.` = 全部改动，实际使用频率最高 |
| `git rm --cached 文件` | 撤销暂存（不删文件） | 暂存区的"反悔空间" |
| `git commit -m "信息"` | 暂存区 → 本地库 | 执行一次记录一次版本，生成唯一哈希 |
| `git checkout 文件` | 放弃工作区修改 | 输出 `Updated 1 path from the index` |
| `git log` | 查看提交历史 | 新→旧；哈希可只写能唯一区分的前缀；HEAD 指向当前提交 |
| `git reflog` | 查看 HEAD 全部移动记录 | 找回被 reset"丢掉"的提交哈希 |
| `git reset --hard 哈希` | 版本回退/前进 | `--hard` 连磁盘文件一起改，慎用 |
| `git remote add origin url` | 关联远程仓库 | origin 只是 URL 别名，做一次即可 |
| `git push -u origin master` | 推送到远程 | `-u` 建立跟踪关系，之后直接 `git push` |
| `git clone url` | 克隆远程仓库 | 在目标目录执行；同名非空目录会报 fatal |
| `touch` / `cat` / `echo "x" >> 文件` / `clear` | 建空文件 / 看内容 / 追加内容 / 清屏 | Git Bash 里的常用 Linux 命令 |
| `git branch` | 查看所有本地分支 | `*` 标出当前所在分支 |
| `git branch dev` | 创建分支（不切换） | 以当前位置为基点；创建后 `*` 仍在原分支 |
| `git checkout dev` / `git switch dev` | 切换分支 | 提示符括号里的分支名会变；新版本推荐 `switch` |
| `git checkout -b dev` | 创建并切换 | 上面两步合一，最常用 |
| `git merge dev` | 把 dev 合并到**当前**分支 | 先 checkout 到接收方再 merge；Fast-forward = 快进合并 |
| `git branch -d dev` | 删除已合并的分支 | 不能在分支自身上删；`-D` = 强制删（未合并会丢提交） |
| `git pull` | 拉取远程更新并合并到本地 | **推送前必做**，否则队友先推过就会被拒 |

**典型完整流程：**

```bash
# —— 本地（一次性准备）——
git config --global user.name "名字"
git config --global user.email "邮箱"
cd 项目目录
git init

# —— 日常循环 ——
# 新增/修改文件……
git status                 # 随时看状态
git add .                  # 工作区 → 暂存区
git commit -m "描述"        # 暂存区 → 本地库
git log / git reflog       # 查版本号
git reset --hard 版本号     # 版本穿梭
git checkout 文件           # 放弃某文件的修改

# —— 远程（Gitee/GitHub）——
git remote add origin url  # 关联远程，一次性
git push -u origin master  # 推送（GitHub 默认分支为 main）
git clone url              # 别人拉取你的代码
git pull                   # 拉取远程更新（PyCharm: Update Project, Ctrl+T）
# —— 分支（开发新功能时）——
git checkout -b dev        # 建并切到开发分支
# ……写代码、add、commit……
git checkout master        # 回到主分支
git merge dev              # 合并（站在 master 上合并 dev）
git branch -d dev          # 合并完删除临时分支

# —— 团队协作（每次推送前）——
git pull                   # 先更新（铁律）
git push                   # 再推送
```
