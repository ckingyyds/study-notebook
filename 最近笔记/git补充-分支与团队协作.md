# git 补充笔记（2026-11-24 课堂）

本篇是《git简单使用.md》的**增补**，只写昨天没讲的内容，重复的一律剔除。

每块开头都标了**插入位置**，照位置贴进原文档即可；文末附「命令速查表要补的行」。

**已剔除的重复内容**（昨天笔记里都有，本篇不再写）：SVN/Git 集中式与分布式区别、四区模型、`init`/`status`/`add`/`commit`/`checkout 文件`/`log`/`reset`/`reflog` 的用法、Gitee 注册绑邮箱、网页新建仓库、`remote add origin`、`push -u`、`clone` 报错、仓库成员三种权限、PyCharm 装 Gitee 插件/配 git 路径/登录/克隆/Update Project。今天课堂前半段（约 0:00–9:30）就是把这些重演了一遍，只当复习。

---

## 补充 A：分支（branch）——今天最核心的新概念

> **插入位置**：原文档 `### 2.11 找回"消失"的提交：git reflog` 这一小节**结束之后**、`## 三、远程仓库 Gitee/Github…` 这一行**之前**。
> 建议单独编成新的一章 `## 三、分支：给不确定的开发开一条副本`，原「三、四、五、六」顺延为「四、五、六、七」；不想改编号就写成「二·五」。

### A.1 为什么需要分支

到昨天为止，所有提交都落在 `master`（主分支）上。单人开发没问题，**多人协作就出问题了**：

- 一旦 `commit`，版本记录就**持久化**了，**没法消除**。
- 假设现在做的是 v1.0，已开发完成；接着在 v1.0 基础上做 v2.0 的新功能，而这个功能**能不能做成是不确定的**。
- 如果照样往 `master` 上提交：做成了——皆大欢喜；**做失败了——这一堆无效提交、垃圾数据就永久留在 master 上了，把主分支"污染"了**。
- 一个团队里每个成员都这么干，master 上的提交记录会越来越乱。

**分支就是解决这个问题的机制**：以某个位置为**基点**"拷贝"一份出来，在副本上折腾，成功了才合并回去，失败了直接丢掉——主分支毫发无损。

### A.2 分支的基本理念（用图画出来更容易懂）

```text
master:  ●───●───●  (v1.0 完成)
                 │
                 └──●───●───●   dev 分支（开发 v2.0）
```

- `dev` 分支创建的那一刻，它的状态和 `master` **完全一样**——就是这个"基点"。
- 之后切到 `dev` 上写代码、提交，`master` 那边**一点不受影响**。
- v2.0 做成了 → 把 `dev` **合并**回 `master`；做失败了 → **不合并**，`dev` 直接删掉，`master` 还停在 v1.0。

**常见用法**：

- `master` 当**生产环境分支**：只放发布用的、可用的代码，提交记录永远干净。
- 开发新功能就开一个开发分支（如 `dev`），开发完再合并。
- 团队里**每个成员一个分支**：负责人给你开一条分支，你改到面目全非也影响不到别人的代码。
- **修 bug 也能开分支**：线上出问题，从出问题的位置开一条分支调通，再合并回去。
- 分支**可以再分叉**：`dev` 上还能继续开分支，结构想多复杂都行，按实际需要开。

> 求职角度的一句实话：公司给你派任务时敢把项目交给你乱改，靠的就是这种"分支隔离"。担心"改坏了怎么办"是多余的——只要对方用 git，你就是改坏了也在自己的分支里。

### A.3 查看分支：`git branch`

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

### A.4 创建分支：`git branch 分支名`

```bash
$ git branch dev        # 无输出即成功
$ git branch
  dev
* master
```

注意两点：

1. **创建 ≠ 切换**。`git branch dev` 只是造了一条新分支，`*` 还挂在 `master` 上——你人还在原地。
2. 新分支的**基点是当前提交**：你在 master 的哪个位置敲这条命令，dev 就从哪里长出来。

### A.5 切换分支：`git checkout 分支名`

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

### A.6 分支隔离到底有多"隔离"——亲手验证一次

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

### A.7 合并分支：`git merge 被合并的分支名`

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
- **`Fast-forward`（快进合并）**：因为 master 在分叉后**一步都没动过**，git 不需要真正"合并"内容，直接把 master 指针往前挪到 dev 的位置就行——历史是一条直线，最省事的情况。如果两边都各自提交过，就不会是快进，git 会生成一次新的合并提交（后面讲冲突时再说）。
- `hi.txt | 1 +`、`create mode 100644 hi.txt`：把 dev 上新增的文件带过来了。

合并后 `git log` 在 master 上也能看到 4 条记录了，`hi.txt` 也躺在 master 的工作区里。

### A.8 删除分支：`git branch -d 分支名`

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

### A.9 完整循环（记这一套就够了）

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

---

## 补充 B：团队协作第一条铁律——推送前必须先更新

> **插入位置**：原文档 `### 3.7 日常节奏：add → commit → push` 这一小节**结束之后**、`### 3.8 拉取代码：git clone` 之前。

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

---

## 补充 C：团队协作的完整闭环（负责人视角 + 成员视角）

> **插入位置**：原文档 `### 3.9 仓库成员与协作权限` 这一小节**结束之后**、`### 3.10 仓库页其他常用功能` 之前。

3.9 讲了怎么把人加进来，这里讲加进来之后**怎么真正协作起来**。

**为什么要有"负责人"**：团队不能三个人各建各的项目——包名不一样、命名规范不一样、目录结构不一样，最后根本合不到一起。所以流程固定为：

**① 负责人（仓库所有者）**

1. 本地先把项目骨架搭好（目录结构、包名、规范都定下来），写好初始代码。
2. 把这个项目推送到远程仓库（命令行 `remote add` + `push`，或 PyCharm 一键分享，见补充 D）。
3. 仓库 → 管理 → 仓库成员管理 → **开发者** → 邀请用户 → 复制链接发群里。
4. 成员自己点链接接受邀请，**账号下的仓库列表里就会多出这个仓库**（不是他的也变成"能看的"了）。

**② 团队成员**

1. 拿到仓库地址（「克隆/下载」里的 HTTPS 地址），用 `git clone` 或 PyCharm 的 Project from Version Control 拉到本地。
2. 在项目里写自己的代码（比如建个 `张三.py`，写点内容）。
3. `commit` → **`pull`** → `push`（顺序见补充 B）。**能推上去，就说明你真的是成员了**——公开仓库的路人只能拉不能推。

**③ 负责人收代码**

成员推上来之后，负责人本地**不会自动变**，必须自己点一次 **Update Project（Ctrl+T）** 或 `git pull`，才能把别人的代码拉到本地。

**④ 之后就是循环**：谁改了代码 → commit → pull → push；其他人 Update。仓库就此变成团队共享的最新代码。

---

## 补充 D：PyCharm 一键把新项目分享到 Gitee（Share Project on Gitee）

> **插入位置**：原文档 `### 4.5 克隆仓库为项目` 这一小节**结束之后**、`### 4.6 更新项目：Update Project` 之前。
> 新小节可编为 `### 4.6 分享新项目`，原 4.6、4.7 顺延为 4.7、4.8。

昨天讲的是"**从远程拉下来**"（克隆），这里补"**把本地项目推上去**"。

命令行要三步：远程网页建仓库 → `git remote add origin` → `git push -u`。
PyCharm 里**一步搞定**：本地初始化 + 远程建同名仓库 + 首次推送，全自动。

**入口**：顶部菜单 **VCS → Share Project on Gitee**（VCS = 版本控制，即昨天提的"渦身扛住/Version Control"）。

弹出的对话框要填：

- **Repository name**：远程仓库名（默认取项目名，可改）。
- **Remote name**：默认 `origin`——就是远程地址的别名，和命令行里那个 origin 是同一个东西。
- **仓库描述（Description）**：填一句简介，会写进远程仓库的介绍里（相当于 3.3 建库时填的"仓库介绍"）。
- **Commit message**：本次提交的说明，等价于 `git commit -m "..."`（比如填"项目初始化"）。
- **文件勾选列表**：默认会把项目里所有文件都选上，**要把 `.idea` 那个勾去掉**——那是 PyCharm 自己的工程配置（解释器路径、窗口布局之类），属于"本机设置"，不该推给别人，别人拉下来也用不上。

点 **Add**（或 Share）之后，PyCharm 自动完成：`git init` → `git add` → 首次 commit → 在 Gitee 上建同名仓库 → `remote add` → `push`。完成后菜单里那一项会变成灰色的 "Successfully shared project"。

**一个坑：分享出来的仓库默认是私有的。** 打开仓库链接会返回 **403（没权限）**。解决：

> 仓库页 → **管理** → 可见性改成 **开源** → 点 **保存**。回到仓库页，名字前面那把锁消失，就是公开仓库了，把链接发给谁都能访问。

（私有仓库有人数上限（约 5 人），团队人多就必须开源，或者去买扩容。）

> **GitHub 对照**：菜单项是 **Git → GitHub → Share Project on GitHub**（插件内置，无需安装）。对话框多一项 **Private/Public** 选择，可以直接建成公开仓库，不用像 Gitee 这样建完再改。其余流程（去勾 `.idea`、填 commit message）完全一致。

---

## 补充 E：PyCharm 里更细的操作——提交、看历史、回退、分支

> **插入位置**：原文档 `### 4.7 PyCharm 内的提交与推送` 这一小节**结束之后**、`## 五、四区流程图` 之前。
> 原 4.7 只有一句"Ctrl+K 提交、Ctrl+Shift+K 推送"，可以把它并入下面的 4.8，全部合并成一节。

### E.1 文件颜色 = 状态（和命令行的红绿完全对应）

- **红色**：新建但**未被 git 管理**（对应`Untracked`），或已修改但没进暂存区。PyCharm 会弹窗问你"要不要加入 git"——**勾上"不再询问"再点 Add**，以后新建文件就自动加入暂存区，不用手动 add 了（这是比命令行省事的地方）。
- **蓝色/绿色**：已进暂存区（对应命令行的绿色）。
- **正常颜色（无高亮）**：已提交、工作区和本地库一致（对应 `nothing to commit, working tree clean`）。

### E.2 Commit 面板：commit 还是 commit and push

点提交按钮（或 Ctrl+K）打开 Commit 面板：左边勾选要提交的文件，中间填提交信息，然后有两个按钮：

- **Commit**：只保存到**本地库**（= `git commit`），不推送。
- **Commit and Push**：保存到本地库**同时推到远程**（= `git commit` + `git push`）。**日常点这个**——代码改完不就是为了分享出去吗。

点 Commit and Push 后还会再弹一次确认"要 push 到哪个远程/分支"，点 **Push** 即可。推送成功后那些文件的颜色会恢复正常（因为工作区和远程一致了）。

### E.3 Log 面板：看提交历史 + 版本穿梭

**入口**：左下角 **Git / Version Control** 工具窗口 → **Log** 标签页，能看到全部提交记录（谁、什么时候、改了什么）。

双击某个提交可以看它改动的文件；**右键某个提交 → `Reset Current Branch to Here...`**，就对应命令行的 `git reset --hard 版本号`：

- 弹窗里选 **Hard**（硬重置：本地库指针、暂存区、工作区文件**全部**退回去，慎用，未提交的改动会没了）。
- 面板里那三个标识要知道：**`master`** = 分支指针，**`HEAD`** = 当前所在的提交。**HEAD 在哪，工作区的文件就是哪个版本**——这就是"版本穿梭"的原理。

### E.4 分支操作全在右下角

**入口**：窗口**右下角状态栏**的 `Git: master`（显示当前分支名），点它：

- **New Branch**：新建分支。填名字（如 `dev`）→ 点 **Create**，PyCharm 会**建好并自动切过去**（等价于 `git checkout -b dev`）。切成功后右下角会变成 `Git: dev`。
- **Checkout**（列表里点其他分支）：切换分支（= `git checkout 分支名`）。切到 master，dev 上新增的文件立刻从工作区消失——和命令行表现一模一样。

所以昨天命令行学的每条指令，在 PyCharm 里都能找到对应按钮：**按钮替你把命令敲了，但面试和排错时你还得知道底下执行的是哪条命令。**

> **GitHub 对照**：Log 面板、Reset、分支切换、Commit 面板全是 IDE 的通用 Git 功能，与平台无关，Gitee/GitHub 用户操作完全一样。

---

## 补充 F：命令速查表要补的行

> **插入位置**：原文档 `## 六、命令速查` 的表格**末尾**，直接加下面这些行。

| 命令 | 作用 | 要点 |
|---|---|---|
| `git branch` | 查看所有本地分支 | `*` 标出当前所在分支 |
| `git branch dev` | 创建分支（不切换） | 以当前位置为基点；创建后 `*` 仍在原分支 |
| `git checkout dev` / `git switch dev` | 切换分支 | 提示符括号里的分支名会变；新版本推荐 `switch` |
| `git checkout -b dev` | 创建并切换 | 上面两步合一，最常用 |
| `git merge dev` | 把 dev 合并到**当前**分支 | 先 checkout 到接收方再 merge；Fast-forward = 快进合并 |
| `git branch -d dev` | 删除已合并的分支 | 不能在分支自身上删；`-D` = 强制删（未合并会丢提交） |
| `git pull` | 拉取远程更新并合并到本地 | **推送前必做**，否则队友先推过就会被拒 |

## 补充 G：典型流程要补的片段

> **插入位置**：原文档末尾「典型完整流程」代码块里，`# —— 远程（Gitee/GitHub）——` 那一段**之后**追加。

```bash
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