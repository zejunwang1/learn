-   [Learn git the easy way](#learn-git-the-easy-way)
    -   [Git 配置](#git-配置)
    -   [Git 工作流程](#git-工作流程)
        -   [克隆仓库](#克隆仓库)
        -   [2. 创建新分支](#创建新分支)
        -   [3. 暂存文件](#暂存文件)
        -   [4. 提交更改](#提交更改)
        -   [5. 推送更改](#推送更改)
        -   [6. 创建 Pull Request（PR）](#创建-pull-requestpr)
        -   [7. 删除分支](#删除分支)
    -   [Git 工作区、暂存区和版本库](#git-工作区暂存区和版本库)
        -   [1. 工作区（Working Directory）](#工作区working-directory)
        -   [2. 暂存区（Staging Area）](#暂存区staging-area)
            -   [git ls-files](#git-ls-files)
            -   [git add](#git-add)
            -   [git mv](#git-mv)
            -   [git rm](#git-rm)
            -   [git status](#git-status)
        -   [3. 版本库（Repository）](#版本库repository)
            -   [git commit](#git-commit)
            -   [git log](#git-log)
            -   [git reset](#git-reset)
            -   [git diff](#git-diff)
    -   [Git 远程操作](#git-远程操作)
        -   [git remote](#git-remote)
        -   [git fetch](#git-fetch)
        -   [git pull](#git-pull)
        -   [git push](#git-push)
        -   [git submodule](#git-submodule)
    -   [Git 分支管理](#git-分支管理)
        -   [1. 创建分支](#创建分支)
        -   [2. 查看分支](#查看分支)
        -   [3. 合并分支](#合并分支)
        -   [4. 删除分支](#删除分支-1)
        -   [5. 拉取分支](#拉取分支)

Learn git the easy way
======================

Git
是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

Git 配置
--------

Git 提供了 `git config` 命令，用来配置或读取相应的工作环境变量。

这些变量可以存放在以下三个不同的地方：

-   `/etc/gitconfig` 文件：系统中对所有用户都普遍适用的配置。若使用 `git config` 时用 `--system` 选项，读写的就是这个文件。

-   `~/.gitconfig` 文件：用户目录下的配置文件，只适用于该用户。若使用 `git config` 时用 `--global` 选项，读写的就是这个文件。

-   `.git/config` 文件：这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 `.git/config` 里的配置会覆盖 `/etc/gitconfig` 中的同名变量。

配置个人用户名和电子邮件地址，这是为了在每次提交代码时记录提交者的信息：

``` {.shell}
git config --global user.name "runoob"
git config --global user.email test@runoob.com
```

Git 设置全局 socks 代理：

``` {.shell}
git config --global http.proxy 'socks5://用户名:密码@代理地址:端口'
git config --global https.proxy 'socks5://用户名:密码@代理地址:端口'
```

可以使用 `git config --list` 命令查看已有的配置信息。

Git 工作流程
------------

### 克隆仓库

首先需要将远程仓库克隆到本地：

``` {.shell}
git clone https://github.com/username/repo.git
cd repo
```

### 2. 创建新分支

为了避免直接在 main 或 master 分支上进行开发，通常会创建一个新的分支：

``` {.shell}
git checkout -b new-feature
```

### 3. 暂存文件

将修改过的文件添加到暂存区，以便进行下一步的提交操作：

``` {.shell}
# 添加特定文件的改动
git add filename
# 添加暂存区文件的改动
git add -u
# 添加当前目录下的所有文件的改动到暂存区，但不包括全新的文件
git add .
# 添加所有已跟踪文件的改动到暂存区，同时将新文件添加到暂存区
git add -A
```

### 4. 提交更改

将暂存区的更改提交到本地仓库，并添加提交信息：

``` {.shell}
git commit -m "Add new feature"
```

### 5. 推送更改

在推送本地更改之前，最好从远程仓库拉取最新的更改，以避免冲突：

``` {.shell}
# 在主分支上工作
git pull origin main
# 在新的分支上工作
git pull origin new-feature
```

将本地的提交推送到远程仓库：

``` {.shell}
git push origin new-feature
```

### 6. 创建 Pull Request（PR）

在 GitHub 或其他托管平台上创建 Pull
Request，邀请团队成员进行代码审查。PR 合并后，你的更改就会合并到主分支。

在 PR 审核通过并合并后，可以将远程仓库的主分支合并到本地分支：

``` {.shell}
git checkout main
git pull origin main
git merge new-feature
```

### 7. 删除分支

如果不再需要新功能分支，可以将其删除：

``` {.shell}
git branch -d new-feature
```

或者从远程仓库删除分支：

``` {.shell}
git push origin --delete new-feature
```

Git 工作区、暂存区和版本库
--------------------------

![](D:\nlp\doc\markdown\git1.png)

我们先来理解下 Git 工作区、暂存区和版本库概念：

-   **工作区：** 就是你在电脑里能看到的目录。

-   **暂存区：** 英文叫 stage 或 index。一般存放在 .git 目录下的 index
    文件（.git/index）中，所以我们把暂存区有时也叫作索引（index）。

-   **版本库：** 工作区有一个隐藏目录 .git，这个不算工作区，而是 Git
    的版本库。

### 1. 工作区（Working Directory）

工作区是本地计算机上的项目目录，在这里可以进行文件的创建、修改和删除操作。工作区包含了当前项目的所有文件和子目录。

### 2. 暂存区（Staging Area）

暂存区是一个临时存储区域，它包含了即将被提交到版本库中的文件快照，在提交之前，你可以选择性地将工作区中的修改添加到暂存区。**常用命令：**

#### git ls-files

查看暂存区的文件：

``` {.shell}
git ls-files
# 只列出已跟踪的文件
git ls-files -t
# 只列出未跟踪的文件
git ls-files -o
# 显示文件的状态信息，包括文件的模式和SHA-1哈希值
git ls-files -s
```

#### git add

添加文件到暂存区：

``` {.shell}
# 添加特定文件的改动
git add filename
# 添加暂存区文件的改动
git add -u
# 添加当前目录下的所有文件的改动到暂存区，但不包括全新的文件
git add .
# 添加所有已跟踪文件的改动到暂存区，同时将新文件添加到暂存区
git add -A
```

#### git mv

用于在 Git
仓库中重命名或移动文件。该命令会创建一个新的文件（目录），并将其添加到暂存区，同时删除旧的文件（目录）。使用
`git mv`
可以保持文件的历史记录，即使文件名变了，也能追踪到之前的历史记录‌。

``` {.shell}
git mv [file] [newfile] 
```

#### git rm

从暂存区中删除文件：

``` {.shell}
# 从工作区和暂存区删除文件，本地文件被删除
git rm -r filename
# 从暂存区删除文件，本地文件还在，只是不希望该文件被版本控制
git rm -r --cached filename
```

#### git status

查看上次提交之后是否有对文件进行再次修改：

``` {.shell}
$ git status
On branch master

Initial commit

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

    new file:   README
    new file:   hello.php
```

git status 命令会显示以下信息：

-   当前分支的名称。

-   未提交的修改：显示已暂存但未使用 `git commit` 提交的文件列表。

-   未暂存的修改：显示已修改但尚未使用 `git add` 添加到暂存区的文件列表。

-   未跟踪的文件：显示尚未纳入版本控制的新文件列表。

### 3. 版本库（Repository）

版本库包含项目的所有版本历史记录。

每次提交都会在版本库中创建一个新的快照，这些快照是不可变的，确保了项目的完整历史记录。**常用命令：**

#### git commit

将暂存区的更改提交到本地版本库：

``` {.shell}
git commit -m "Commit message"
```

#### git log

查看提交历史：

``` {.shell}
git log
# 以简洁的一行格式显示提交信息
git log --oneline
# 显示最近的5次提交
git log -n 5
# 显示自指定日期之后的提交
git log --since="2024-01-01"
# 显示指定日期之前的提交
git log --until="2024-07-01"
# 只显示某个作者的提交
git log --author="Author Name"
# 不显示合并提交
git log --no-merges
# 显示简略统计信息，包括修改的文件和行数
git log --stat
```

#### git reset

回退版本，可以指定退回某一次提交的版本。它可以通过改变 HEAD
指针来修改历史提交记录，影响暂存区和工作区。命令语法格式如下：

``` {.shell}
git reset [--soft | --mixed | --hard] [HEAD]
```

-   `--soft`：仅重置 HEAD
    指针，保留暂存区和工作区的更改。适用于需要保留修改但重新提交的情况。

-   `--mixed`：默认选项，重置 HEAD
    指针，将暂存区的内容恢复到指定提交状态，但保留工作区的更改。适用于撤销暂存操作但保留工作区更改的情况。

-   `--hard`：重置 HEAD
    指针，暂存区和工作区丢弃所有未提交的更改。适用于确定不再需要本地更改的情况。

``` {.shell}
# 回退上上上一个版本
git reset --hard HEAD~3
# 回退到某个版本
git reset --hard bae128
# 将本地的状态回退到和远程的一样
git reset --hard origin/main
```

**HEAD 说明：**

-   HEAD 表示当前版本

-   HEAD\^ 上一个版本

-   HEAD\^\^ 上上一个版本

-   HEAD\^\^\^ 上上上一个版本

-   以此类推 ...

可以使用 \~数字表示：

-   HEAD\~0 表示当前版本

-   HEAD\~1 上一个版本

-   HEAD\~2 上上一个版本

-   HEAD\~3 上上上一个版本

-   以此类推 ...

#### git diff

用于比较不同状态下的文件差异。

(1) 查看工作区和上次提交的差异：

``` {.shell}
git diff
```

这将显示自上次提交以来对所有文件所做的更改。

(2) 查看暂存区和上次提交的差异：

``` {.shell}
git diff --staged
```

这将显示已经添加到暂存区但尚未提交的更改。

(3) 查看特定提交/分支与当前工作区的差异：

``` {.shell}
git diff commitA
git diff branch
```

(4) 只显示发生变化的文件名：

``` {.shell}
git diff --name-only
```

(5) 显示文件名和变化状态：

``` {.shell}
git diff --name-status
```

(6) 显示简要的统计信息：

``` {.shell}
git diff --stat
```

该命令会列出所有修改过的文件及其状态（如新增、修改、删除），但不会显示具体的改动内容。

(7) 比较两次提交的差异：

``` {.shell}
git diff commitA commitB
```

这里 `commitA` 和 `commitB` 可以是提交哈希值、分支名或标签名等。

`git diff`的输出可以被重定向到其他工具进行进一步处理。

``` {.shell}
git diff commitA commitB > diff_result.txt
```

可以将差异结果输出到一个文件，然后使用文本编辑器来查看。或者使用专门的差异分析工具，如
`meld`、`kdiff3` 等，结合 `git difftool`
命令来进行更直观的图形化差异比较。

(8) 比较两个分支的差异：

``` {.shell}
git diff branch1 branch2
```

这里将显示 `branch1` 和 `branch2` 分支之间的差异。

(9) 比较两个提交之间特定文件的差异：

``` {.shell}
git diff commitA commitB path/to/file
```

这将显示在提交 `commitA` 和 `commitB` 之间，文件 `path/to/file` 的差异。

(10) 比较两个分支之间特定目录的差异：

``` {.shell}
git diff branch1 branch2 path/to/directory/
```

这将显示分支 `branch1` 和 `branch2` 之间，目录 `path/to/directory/`
的差异。

Git 远程操作
------------

**常用命令：**

#### git remote

用于管理 Git 仓库中的远程仓库。常见用法：

-   `git remote`：列出当前仓库中已配置的远程仓库。

-   `git remote -v`：列出当前仓库中已配置的远程仓库，并显示它们的 URL。

-   `git remote add <remote_name> <remote_url>`：添加一个新的远程仓库。指定远程仓库的名称和
    URL，将其添加到当前仓库中。

-   `git remote rename <old_name> <new_name>`：将已配置的远程仓库重命名。

-   `git remote remove <remote_name>`：从当前仓库中删除指定的远程仓库。

-   `git remote set-url <remote_name> <new_url>`：修改指定远程仓库的
    URL。

-   `git remote show <remote_name>`：显示指定远程仓库的详细信息，包括
    URL 和跟踪分支。

#### git fetch

用于从远程仓库获取最新的历史记录和数据，但不会自动合并或更改项目当前的工作目录和暂存区。这个命令将远程仓库的更新信息下载存储在本地仓库的
`.git` 目录中，但不会影响工作目录或暂存区。

``` {.shell}
# 获取所有远程分支的更新
git fetch
# 获取特定远程仓库的所有分支的更新
git fetch <remote>
# 获取特定远程仓库的特定分支的更新
git fetch <remote> <branch>
```

在获取远程分支数据后，可以执行以下命令合并到当前分支：

``` {.shell}
git merge <remote>/<branch>
```

如果想从远程仓库的特定分支拉取代码，并更新至本地分支：

``` {.shell}
git fetch origin main:brantest
```

这个命令会将远程仓库的 `main` 分支的内容更新到本地的 `brantest` 分支。如果本地还没有 `brantest` 分支，Git
会自动创建一个新的分支。

#### git pull

用于从远程仓库获取代码并合并到本地分支。这个命令其实是
`git fetch` 和 `git merge` 的简写，先从远程仓库获取最新的提交记录，然后将这些提交记录合并到项目当前的分支中。

``` {.shell}
git pull <remote> <branch>
```

-   `remote` 是远程存储库的名称，默认为 `origin`。

-   `branch` 是远程分支的名称，默认为 `main` 或 `master`。

``` {.shell}
git pull origin main
```

这会拉取远程 `main` 分支的最新更改并合并到当前分支中。

``` {.shell}
git pull origin main:brantest
```

这会拉取远程 `main` 分支的最新更改，并合并到本地的 `brantest`
分支，如果本地不存在 `brantest` 分支，Git 会自动创建它。

#### git push

用于将本地仓库的更改（commit）上传到远程仓库。

``` {.shell}
git push <remote> <local_branch>:<remote_branch>
```

-   `remote` 是远程存储库的名称，默认为 `origin`。

-   `local_branch` 是希望推送的本地分支的名称。

-   `remote_branch` 是想要更新的远程分支的名称。

将本地的 `main` 分支推送到 origin 远程存储库的 `main` 分支：

``` {.shell}
git push origin main
```

相等于：

``` {.shell}
git push origin main:main
```

如果本地版本与远程版本有差异，但又要强制推送可以使用 `--force` 参数：

``` {.shell}
git push --force origin main
```

删除远程分支可以使用 `--delete` 参数，以下命令表示删除远程的 `main`
分支：

``` {.shell}
git push --delete origin main
```

推送所有本地分支到远程仓库，可以使用 `--all` 选项：

``` {.shell}
git push --all origin
```

如果想要推送当前分支到远程仓库并跟踪它，可以使用 `-u` 选项：

``` {.shell}
git push -u origin main
```

这样会将本地的 `main` 分支与远程仓库 `origin` 关联，之后只需
`git push`。

#### git submodule

用于管理包含其他 Git
仓库的项目。这个命令对于大型项目或需要将外部库集成到项目中的情况非常有用。通过使用子模块，你可以将外部库作为你的项目的一部分来管理，而不必将其直接合并到主仓库中。

**1. 初始化子模块**

``` {.shell}
git submodule init
```

这个命令会初始化配置文件中的所有子模块。它会根据 `.gitmodules`
文件中的信息设置子模块的 URL 和路径，但不会下载子模块的内容。

常见用法：在克隆了一个包含子模块的仓库后，运行子命令来初始化子模块。

``` {.shell}
git clone <repo-url>
cd <repo-dir>
git submodule init
git submodule update
```

**2. 更新子模块**

``` {.shell}
git submodule update
```

这个命令会从子模块的远程仓库中拉取子模块的内容，并将其更新到
`.gitmodules` 文件中指定的提交。

**3. 添加子模块**

``` {.shell}
git submodule add <repo-url> [<path>]
```

这个命令会将指定的 Git 仓库作为子模块添加到当前仓库中。

`<repo-url>` 是子模块的仓库地址，`<path>` 是子模块在主仓库中的路径（可选，如果不指定，默认使用子模块仓库的名称作为路径）。

常见用法：将外部库作为子模块添加到项目中。

``` {.shell}
git submodule add https://github.com/example/libfoo.git libfoo
```

**4. 移除子模块**

``` {.shell}
git submodule deinit <path>
git rm <path>
```

-   `git submodule deinit <path>`：将子模块从 `.git/config` 文件中移除，并删除子模块目录中的文件。
-   `git rm <path>`：将子模块的引用从主仓库中删除，并提交更改。

常见用法：从主仓库中移除一个子模块。

``` {.shell}
git submodule deinit libfoo
git rm libfoo
rm -rf .git/modules/libfoo
```

**5. 列出子模块**

``` {.shell}
git submodule
```

列出当前仓库中的所有子模块，以及它们的提交哈希和路径。

**6. 更新所有子模块**

``` {.shell}
git submodule update --recursive --remote
```

-   `--recursive`：递归地更新所有子模块（包括子模块的子模块）。
-   `--remote`：从子模块的远程仓库拉取最新的更改。

Git 分支管理
------------

Git 分支管理是 Git
强大功能之一，能够让多个开发人员并行工作，开发新功能、修复 bug
或进行实验，而不会影响主代码库。

`<img src="file:///D:/nlp/doc/markdown/git_branch.png" title="" alt="" width="527">`{=html}

#### 1. 创建分支

创建新分支并切换到该分支：

``` {.shell}
git checkout -b <branchname>
```

切换分支：

``` {.shell}
git checkout <branchname>
```

当切换分支的时候，Git
会用该分支最后提交的快照替换项目工作目录的内容，所以多个分支不需要多个目录。

在切换分支前，最好确保当前分支所做的修改已经提交，否则会报错。

但我们有时可能会遇到这样的情况，正在 dev
分支开发新功能，做到一半时团队成员反馈一个
bug，需要马上解决，但新功能又暂时不想提交，这时就可以使用 `git stash`
命令将 dev
分支的工作区和暂存区保存起来，然后切换到另一个分支去修改bug，修改完提交后再切回
dev 分支，使用 `git stash pop`
来恢复之前的进度继续开发新功能。下面是使用 `git stash` 时要遵循的顺序：

-   将修改保存到分支 A 工作区
-   运行 `git stash`
-   签出分支 B
-   修正 B 分支的 bug
-   提交并推送 (可选) 至远程
-   切回分支 A
-   运行 `git stash pop` 来恢复工作区暂存的改动

#### 2. 查看分支

查看所有本地分支：

``` {.shell}
git branch
```

查看所有远程分支：

``` {.shell}
git branch -r
```

查看所有本地和远程分支：

``` {.shell}
git branch -a
```

#### 3. 合并分支

将其他分支合并到当前分支：

``` {.shell}
git merge <branchname>
```

例如，切换到 `main` 分支并合并 `deploy` 分支：

``` {.shell}
git checkout main
git merge deploy
```

当合并过程中出现冲突时，Git 会标记冲突文件，需要手动解决冲突。

打开冲突文件，按照标记解决冲突。完成后：

``` {.shell}
git add <conflict-file>
git commit
```

#### 4. 删除分支

删除本地分支：

``` {.shell}
git branch -d <branchname>
```

强制删除未合并的分支：

``` {.shell}
git branch -D <branchname>
```

删除远程分支：

``` {.shell}
git push origin --delete <branchname>
```

#### 5. 拉取分支

拉取远程分支 `origin/main` 的最新提交并合并至本地当前分支：

``` {.shell}
git pull origin main
```

**5.1 本地提交前报错**

有时候在本地提交代码前想使用 `git pull` 拉取远程仓库更新时会报错：

``` {.context}
error: Your local changes to the following files would be overwritten by merge:
    README.md
Please commit your changes or stash them before you merge.
Aborting
```

说明本地当前修改的代码和别人修改提交到远程仓库的代码有冲突，可以使用
`git stash` 来解决：

(1) 存储本地工作区修改的代码：

``` {.shell}
git stash
```

(2) 拉取远程分支代码：

``` {.shell}
git pull origin main
```

``` {.context}
 * branch            main       -> FETCH_HEAD
Updating e021282..42c2045
Fast-forward
 README.md | 2 +-
 1 file changed, 1 insertion(+), 1 deletion(-)
```

这时已经不报错了，能正常 `pull` 下来。

(3) 把本地存储的代码释放出来：

``` {.shell}
git stash pop
```

``` {.context}
Auto-merging README.md
CONFLICT (content): Merge conflict in README.md
```

发现在 `README.md`
文件里会有冲突，我们手动解决下冲突，就可以正常添加和提交了。

**5.2 本地提交后报错**

有时候在本地工作区修改代码提交后，拉取远程分支最新提交时会报错：

``` {.shell}
git pull origin main
......
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge (the default strategy)
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```

这说明本地分支和远程分支的提交历史发生了分叉（Divergent
Branches），即双方都有对方没有的新提交（Commit）。Git
无法自动决定如何合并两者的差异，需要用户明确指定合并策略。

可以通过生成一个新的合并提交（Merge Commit）来整合本地和远程的提交历史：

``` {.shell}
git pull --no-rebase origin main
```

若存在代码合并冲突，手动修改冲突文件后进行提交即可。

或者通过配置设置全局默认行为（合并提交）：

``` {.shell}
git config --global pull.rebase false
```
