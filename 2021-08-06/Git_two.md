# 介绍
- 社畜正式开始在公司上班，一开始还是以学习为主
- 但公司使用的是GitLab，而我在公司电脑上个人练习使用的是GitHub
- 所以就萌生了在一台电脑上同时使用GitHub和GitLab的念头
# 生成密钥
- 密钥一般存在 `c/Users/用户名/.ssh` 目录下，如无该目录，自行创建
- 给两个网站生成不同的密钥
```cmd
ssh-keygen -t rsa -C "公司Gitlab邮箱地址"		// 生成对应的密钥：id_ras与id_ras.pub
ssh-keygen -t rsa -C "个人GitHub邮箱地址” -f ~/.ssh/id_rsa_github	// 生成对应的密钥：id_rsa_github与id_rsa_github.pub
```

- 将这两个密钥分别配置到GitLab和GitHub上的设置中
# 配置文件
- 在生成密钥的地方创建一个 `config` 文件
```cmd
# self(126）
Host github.com
        port 22
        User git
        HostName github.com
        PreferredAuthentications publickey
        IdentityFile ~/.ssh/id_rsa_github
# company(FoxMail)
Host gitlab.com
        port 22
        User git
        HostName 你公司gitlab的地址，注意不需要带http://
        PreferredAuthentications publickey
        IdentityFile ~/.ssh/id_rsa
```

# 测试连接
- 在命令行中分别运行以下两个命令，得出相同语句时说明连接成功
```cmd
$ ssh -T git@github.com
Hi zhangyangeng! You've successfully authenticated, but GitHub does not provide shell access.
$ ssh -T git@gitlab.com
Welcome to GitLab, @zhangyangeng!
```

# Git配置
- 这里因为公司电脑经常用到Git，所以我将公司的用户名和邮箱设置为了全局配置
```cmd
git config --global user.name "用户名"
git config --global user.email "邮箱"
```

- 将个人配置到单个文件夹里
```cmd
git config user.name "用户名"
git config user.email "邮箱"
```

- 之后就可以成功的远程连接仓库了

----------------------

# 介绍
- 自己近期在公司学习的时候练习的项目也比较多，但存放于公司电脑并不是一个合适的选择（不方便随处查看）
- 再加上近期对 Git 使用比较多，就萌生了将项目传到 GitHub 仓库中进行管理
- 起初想法是在一个仓库中建立文件夹来区分项目，后来感觉并不合适（每次从别处看的时候整个分支上的内容全clone下来了）
- 最后发现 orphan 分支（将N个完全不同的项目作为N个分支放在同一个仓库中, 并且分支之间互不影响）完全可以解决该问题

# 方法
## 1.创建仓库
- 本地创建仓库，并连接到远程仓库
## 2.创建orphan分支
- 按需创建，我这里需要创建一个 Android 分支来单独存放 Android 代码，执行如下：
```
 git checkout --orphan Android
```
- 分支虽然创建，但如果不进行提交的话远程仓库是没有该分支的
## 3.提交代码
- 执行如下命令进行提交：
```
git add ./
git commit -m "test"
```

## 4.推送到远程仓库
- 因为我们是在 orphan 分支上进行的操作，所以在推送的时候也要推送到相应的远程分支上
```
git push origin Android	// 注意：一定要推送到对应分支上
```

## 5.创建其他orphan分支
- 操作同上，但建议切换回主分支以后再新建 orphan 分支

# 克隆问题
- 当其他电脑或者其他人想要克隆该项目时该怎么办呢？
- 操作方法如下：
## 1.克隆仓库并进入项目
- 先将该远程仓库克隆到本地
```
git clone git@github.com:~~~.git
cd xxx
```

## 2.查看当前所有分支
- 使用如下命令可以看到远程的所有分支
```
git branch -a
```

## 3.创建本地分支
- 为了在某分支上工作，我们需要在本地创建一个和远程分支同名的分支
```
git checkout -b Android origin/Android
// 或
git checkout -t origin/Android		// 默认会在本地建立一个和远程分支名字一样的分支
```

## 4.拉取远程仓库最新内容

- 使用如下命令将该分支上的所有内容全都拉到本地
```
git pull
```

## 5.修改代码并推送
- 注意推送到推送到对应分支上
```
git push origin Android
```