---
draft: true # 是否为草稿
title: CataClysm-DDA在archlinux下无中文的解决方法
date: 2020-04-08 21:29:43
status: public
tags: games cateclysm      #tag
categories: games          #文章分类
cover: https://tokaku-picture.oss-cn-shanghai.aliyuncs.com/20200408212002.png
---



安装

```bash
yay -S cataclysm-dda-git 
```



完成后打开

虽然设置了中文，但是显示的是英文



![](https://tokaku-picture.oss-cn-shanghai.aliyuncs.com/20200408211425.png)

![](https://tokaku-picture.oss-cn-shanghai.aliyuncs.com/20200408212002.png)

原因是没有本地化CataClysm-DDA的中文



解决方法为，下载git文件，

运行*Cataclysm-DDA-master/lang/*中的*compile_mo.sh*文件，会在当前目录下生成mo文件夹

（*/Cataclysm-DDA-master/lang/mo/zh_CN）

将**/Cataclysm-DDA-master/lang/mo/zh_CN/LC_MESSAGES/cataclysm-dda.mo* 复制到*/usr/share/locale/zh_CN/LC_MESSAGES/*目录下重启游戏就大功告成了！

![](https://tokaku-picture.oss-cn-shanghai.aliyuncs.com/20200408211928.png)