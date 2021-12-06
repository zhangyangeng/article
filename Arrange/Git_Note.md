# 基础

- 所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，而对于二进制文件是无法跟踪的
- [廖雪峰](https://liaoxuefeng.gitee.io/resource.liaoxuefeng.com/git/git-cheat-sheet.pdf)命令总结

# 命令

- `git diff`：查看某文件有什么改动
- `git reset --hard 哈希值`：回退到上一版本
- `git reflog`：查看命令历史
- 撤销修改：
  - `git checkout -- file`：把文件在工作区的修改全部撤销
    - 修改后文件未放入暂存区：回到版本库一模一样的状态
    - 已入暂存区又进行了修改：回到添加到暂存区中的状态
  - `git reset HEAD <file>`：可以把暂存区的修改撤销掉（unstage），重新放回工作区

# 杀手功能

## 远程仓库

### (1).创建

- 创建 SSH Key
```
ssh-keygen -t rsa -C "youremail@example.com"
```

- 创建好后可以去用户主目录中查看是否含有 `.ssh` 文件夹，在该文件夹里存放着两个文件
  - `id_rsa` 为私钥，只能自己使用
  - `id_rsa.pub` 为公钥，可以给其他人使用
- 登录 GitHub 或者 GitLab 中，在设置中找到 `SSH Keys` 字段添加一个 key，在其中输入公钥

### (2).拉取远程仓库项目

- `git clone git@192.168.129.110:zhangyangeng/test.git` 命令可以拉取指定仓库
- `cd test` 进入到项目目录中
- `touch README.md` 创建一个文件
- `git add README.md` 将修改提交到暂存区
- `git commit -m "add README"` 将暂存区中的文件提交到版本库
- `git push -u origin master` 将当前版本库中的内容推送到远程仓库

### (3).推送已有项目到远程仓库

- `git remote add origin git@192.168.129.110:zhangyangeng/test.git` 将远程仓库与当前项目目录进行关联
  - origin 为远程仓库的名字，默认为其
  - 后面的内容是自己仓库的链接，自行修改
- `git push -u origin master` 将当前版本库中的内容推送到远程仓库
- `git push origin master` 之后只要本地做了提交，就可以通过命令把本地 `master` 分支的最新修改推送至远程仓库了

### (4).查看远程仓库

- `git remote -v` 可以查看到当前所有的远程仓库
- 一般会含有两行内容，分别代表抓取和推送的 `origin` 地址

### (5).删除远程仓库

- `git remote rm origin` 注意：这里并不是直接删除了远程仓库，而是解除了本地与远程仓库的连接，删除还需要去网站后台进行删除

## 分支部分

- 撤销修改与切换分支的命令很类似，所以最新版本的 Git 中提供了以下命令来切换到该分区
```
git switch -c dev	// 新建并切换分区
git switch master	// 切换到现有分区
```

- `git log --graph --pretty=oneline --abbrev-commit` 查看项目分支历史

- 禁用Fast Forward：
  - 可以禁用 “快速合并模式” 来在合并分支时生成新的 commit，这样从分支历史上就可以看出分支信息
  - 只要在合并时使用 `git merge --no-ff -m "merge with no-ff" dev` 即可禁用该模式
- 分支策略：
  - `master` 分支是非常稳定的，仅用来发布新版本
  - `dev` 分支是不稳定的，等版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本
  - 每个人都在 `dev` 分支上干活且都有自己的分支，时不时地往 `dev` 分支上合并就可
  - 团队图：[这里](https://www.liaoxuefeng.com/wiki/896043488029600/900005860592480)

### (0).删除远程分支

- 先查看所有的分支，使用 `git branch -a` 可以看到本地和远程的所有的分支
- 然后使用 `git push origin --delete 远程分支名` 即可删除远程分支

### (1).临时处理Bug

- 可以使用 `git stash` 将当前的工作现场临时存储起来
- 然后处理相应 Bug
- 然后合并 Bug 分支到主分支上
- 切换到当前工作分支
- 使用 `git stash list` 查看所有被存储的工作区
- 恢复工作区：
  - 先使用 `git stash apply` 恢复，然后使用 `git stash drop` 删除 stash 内容
  - 直接使用 `git stash pop` 恢复，且同时把 stash 呢日荣删除

### (2).在子分支修复主分支同样的bug

- 可以使用 `git stash` 将当前的工作现场临时存储起来
- 然后处理相应 Bug
- 然后合并 Bug 分支到主分支上
- 切换到当前工作分支
- 使用 `git stash list` 查看所有被存储的工作区
- 使用 `git cherry-pick <bug修复的哈希值>` 命令把 bug 提交的修改“复制”到当前分支
- 恢复工作区：
  - 先使用 `git stash apply` 恢复，然后使用 `git stash drop` 删除 stash 内容
  - 直接使用 `git stash pop` 恢复，且同时把 stash 呢日荣删除

### (3).多人协作

- 首先，可以试图用`git push origin <branch-name>`推送自己的修改
- 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并
  - 如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`
- 如果合并有冲突，则解决冲突，并在本地提交
- 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功

# Rebase

- 看不懂

# 标签管理

- 发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本
- 所以，标签也是版本库的一个快照

## 创建标签

- `git tag <name>` 切换到需要打标签的分支上执行该命令来创建标签
- `git tag <name> 哈希值` 给对应的 commit 打标签
- `git tag -a <name> -m "提示文字"` 创建带有说明的标签

## 查看标签

- `git tag` 查看标签（标签默认是按字母顺序排序的）
- `git show <name>` 查看标签的具体信息

## 推送标签

- `git push origin <name>` 推送某个标签到远程
- `git push origin --tags` 一次性推送全部尚未推送到远程的本地标签

## 删除标签

- `git tag -d <name>` 删除本地标签
- `git tag -d <name>` 然后 `git push origin :refs/tags/v0.9` 删除已经推送到远程的标签

# 自定义Git

## 忽略特殊文件

- 在 Git 工作区的根目录下创建一个特殊的 `.gitignore` 文件，然后把要忽略的文件名填进去，Git 就会自动忽略这些文件
- 不需要从头写`.gitignore`文件，GitHub 已经为我们准备了各种配置文件，只需要组合一下就可以使用了。所有配置文件可以直接在线浏览：https://github.com/github/gitignore
- 忽略文件的原则：
  - 忽略操作系统自动生成的文件，比如缩略图等
  - 忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没必要放进版本库，比如Java编译产生的`.class`文件
  - 忽略你自己的带有敏感信息的配置文件，比如存放口令的配置文件
- 当某个自己想要上传的文件被忽略时我们可以使用 `-f` 参数来强制添加到 Git
- `git check-ignore -v 文件名` 来检查哪个规则有问题
- 忽略文件中的例外规则：可以不排除指定文件，如下：
```
# 排除所有.开头的隐藏文件:
.*
# 排除所有.class文件:
*.class
# 不排除.gitignore和App.class:
!.gitignore
!App.class
```

## 配置别名

- 将命令配别名拉过来

## 搭建Git服务器

- 点击[查看](https://www.liaoxuefeng.com/wiki/896043488029600/899998870925664)

# Git Fork流操作

1. 从远程仓库A fork到自己仓库B
2. 从自己仓库clone到本地 
3. git remote add upstream https://gitLab.XXXX.com/A/project（添加一个upstream指向远程仓库）
4. git fetch upstream（获取A上的所有分支到你本地)
5. git merge upstream/master（将远程的master分支的内容同步到本地的master上），此时你的项目与远程一致了，这是时候在本地进行修改后正常 add ，commit，push之前要确定你这次提交之前与你最后一次拉代码的时候没有人提交，这时候需要再拉一次代码
6. git git fetch upstream （将远程分支同步到本地）
7. git merge upstream/master （合并分支，每次将远程仓库项目同步到本地project需要6，7操作，/后可以是你想同步的任意分支）
8. git pull（同步到本地）
9. git push origin master(将内容推到你的仓库B)
10. 然后请求合并到远程仓库A

# Git配置问题

## 执行 git push 出错

- 错误代码：`git@github.com: Permission denied (publickey). fatal: Could not read from remote repository.`
- 解决方法1：检查是否成功在远程仓库中添加了本机对应的 SSH Key，如果没有，添加后再次执行
- 解决方法2：检查本地Git仓库是否和该SSH key关联，即执行 `ssh-add "你的 id-rsa 文件地址"`

## 执行 ssh-add 出错

- 错误代码：`Could not open a connection to your authentication agent`
- 解决方法：执行 `ssh-agent bash` ，然后再执行 `ssh-add `

# 如何在一台电脑上提交到不同的远程仓库中？

- 本人在公司练习项目用的公司的GitLab，而自己在做一些小 demo 时是想要将其提交到自己的 GitHub 中
- 所以进行如下操作

## 1.配置私人账户的ssh

- 根据配置命令，重命名为另一个 SSH key
- 将生成的公钥配置到自己的账户下

## 2.修改私人仓库的用户名和邮箱

- 因为在公司电脑上配置了全局的用户名和邮箱，就会导致在上传时是以“别人”传到了自己的仓库中
- 所以需要在自己的项目目录中配置用户名和邮箱（此时无需添加 --global）

## 3.使用ssh-agent来管理私钥

>当我们的主机有多个密钥对（例如：root用户生成了一对公钥私钥对，hmk用户生成了另外一对公钥私钥对），我们连接到其他不同的多台主机时，可能用的认证用户不一样，这时候需要我们手动指定使用哪个密钥，一旦机器过多输入会非常繁琐，ssh-agent能帮我们管理这些密钥对
当我们给私钥加了密码，而我们的认证方式又选择了密钥认证，ssh-agent可以帮助我们免去输入密码的繁琐操作

- 使用 `ssh-agent bash` 命令来启动
- 给当前目录添加私人账户的公钥进行管理，使用 `ssh-add "你的 id-rsa 文件地址"` 进行添加

## 4.成功连接私人账户

- 此时成功连接，就可以执行相关操作了

## 5.关闭进程

- 因为 `ssh-agent` 是独立进程，所以即使我们退出当前 shell 连接它依然存在，所以我们最好在退出前用命令`ssh-agent k`关闭它



























# 后续任务

- 检查 Snipaste 软件配置
- 检查 BitDock 的配置
- 合并当天笔记
- 试试 [pull request](https://www.liaoxuefeng.com/wiki/896043488029600/900937935629664)