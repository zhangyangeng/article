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