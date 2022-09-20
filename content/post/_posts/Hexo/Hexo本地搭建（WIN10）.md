---
draft: true # 是否为草稿
title: Hexo博客的搭建   #文章名
date: 2019‎-12‎-‎26‎ ‏‎21:14
status: public          #公开
tags: Hexo      #tag
categories: Hexo           #文章分类
---

### 1.本地环境搭建

[node.js ](http://nodejs.cn/)

[git](https://git-scm.com/)

右键点击Git Bash Here 输入

```bash
$ node -v
$ npm -v
```

确认node是否安装成功



### 2.安装HEXO

下载并安装hexo

```bash
$ npm install -g hexo-cli	#安装HEXO框架
```



```bash
$ hexo init blog  			#blog为相对路径下的安装路径
$ cd blog
$ npm install
```

注 ：执行 install 命令出现下列警告属于window下安装的正常现象，无视他。

```bash
$ npm install
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@2.1.2 (node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@2.1.2: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.11 (node_modules\nunjucks\node_modules\fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.11: wanted {"os":"darwin","arch":"any"} (current: {"os":"win32","arch":"x64"})

```



### 3.HEXO的常用命令

```bash
$ hexo g 	#hexo generate 	生成静态文件
$ hexo s	#hexo server   	启动本地服务器，使用hexo s -p 5000更改端口
$ hexo n	#hexo new      	新建文章
$ hexo c	#hexo clean		清理缓存文件（网页显示异常时用）
```

### 4.基本配置

安装好之后打开是英文界面，并且啥都没有，这首就需要自己去配置

配置文件位于 **根目录** 下的 **_config.yml**文件，用文本编辑器打开：

【注】编辑时注意属性与值之间留一个空格符

```ba
#Site 网站相关配置
title           网站标题
subtitle        网站副标题
description     网站描述
keywords        网站关键词
author          网站作者名字
language        网站使用的语言，这里填zh-Hans
timezone        网站使用的时区，这里默认使用电脑的时区
```

具体配置说明，查看 [HEXO官方文档](https://hexo.io/zh-cn/docs/configuration.html)



### 5.官方文档

[开始使用](https://hexo.io/zh-cn/docs/)

[建站](https://hexo.io/zh-cn/docs/setup)

[配置](https://hexo.io/zh-cn/docs/configuration)

[命令](https://hexo.io/zh-cn/docs/commands)

[迁移](https://hexo.io/zh-cn/docs/migration)