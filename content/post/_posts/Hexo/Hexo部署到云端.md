---
draft: true # 是否为草稿
title: Hexo部署到云端
date: 2020-1‎-24 ‏‎14:35:13
status: public
tags: Hexo
categories: Hexo
---

# hexo部署到centos服务器上

***本文介绍将hexo部署到腾讯云的centos服务器上。\***

*准备一台centos云服务器。*

## 搭建本地hexo服务

**搭建本地hexo服务可以参见我之前的博客，这里不再赘述。**

[搭建hexo本地环境](https://blog.hotchpotch.club/hexo+github搭建个人博客.html)

------

## 登录centos云服务器

*这里采用ssh远程登录*

打开终端输入以下命令

```
ssh username@your_server_ip
```

*然后输入你的用户密码按下回车键就可以登进去了*

这里是以默认端口号22进行访问，如果你的端口号改动了则可以加上 *-p* 参数。

```
ssh username@your_server_ip -p 端口号
```

------

## 安装和配置nginx

- *ssh 登录到服务器然后安装nginx*

```undefined
yum update
yum install -y nginx
```

- **启动nginx并设置开机自启**



```bash
service nginx start
systemctl enable nginx
```



- **配置nginx环境**

*这里主要配置域名以及网站根目录，如果没有域名则可以暂时不配置，等购买域名并完成域名备案域名解析后再做配置。*

```
vim /etc/nginx/nginx.conf
```

*以下列出主要配置:*

*新建博客目录:*

```
mkdir /usr/share/nginx/blog
```

将nginx网页的根目录改为博客目录

```bash
server {
    listen  80;
    server_name     example.com; #改为自己的域名
    root            /usr/share/nginx/blog; #改为自己博客的目录
}
```

nginx配置基本完成。

------

## 安装Node.js

下载Nodejs**

```bash
yum install -y nodejs
```

*测试是否配置正确*

```
node -v
```

到此为止我们的Node.js就配置好了

------

## 安装Git并进行配置

- **安装Git**

```
yum install -y git
```

- **创建git用户并为其设置密码**



```bash
adduser git #创建用户
passwd git  #设置密码，之后要输入两次git用户的密码
```

- **切换至git用户，添加SSH Key**

*在客户端查看并复制客户端的SSH Keys*



```ruby
#注意这是在客户端执行，将内容复制下，下面要用。
cat ~/.ssh/id_rsa.pub
```



```bash
#这是在服务器端执行的
su git  #切换用户
mkdir ~/.ssh    #创建目录
vim ~/.ssh/authorized_keys  #将刚刚复制的内容写进去 
```

- **为刚刚的文件和目录设置权限**



```undefined
chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

- **在客户端测试是否能连接服务器**

*终端输入以下命令，如果成功登录则配置成功。*

```
ssh -v git@your_server_ip #注意更改你自己的服务器ip
```

- **将博客发布目录的属主属组改为git**

```
chown -R git:git /usr/share/nginx/blog
```

- **创建空的Git仓库**



```bash
su git
cd ~
git init --bare blog.git    #使用--bare参数，Git就会创建一个裸库。
```

- **配置git hooks（资源勾子）**

```
vim ~/blog.git/hooks/post-receive
```

*在post-receive中插入以下内容:*

```bash
#!/bin/bash
git --work-tree=/usr/share/nginx/blog --git-dir=/home/git/blog.git checkout -f
```

*赋予其执行权限*

```bash
chmod +x ~/blog.git/hooks/post-rceive #-x或755
```

------

## 配置博客根目录的主配置文件

*编辑博客根目录下的_config.yml*



```ruby
deploy:
  type: git
  repo: git@your_server_ip:/home/git/blog.git #注意改成自己的服务器ip
  branch: master
  message:
```

*在博客主目录执行以下命令*

```undefined
hexo clean
hexo g
hexo d
```