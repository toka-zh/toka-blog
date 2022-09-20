---
draft: true # 是否为草稿
title: 泰拉瑞亚-搭建tMod服务器
date: 2020-04-208 21:29:43
status: public
tags: games Terraria      #tag
categories: games          #文章分类
cover: 

---

所需文件：

​	tmod windows端[tModLoader.Windows.v0.11.6.2.zip](https://github.com/tModLoader/tModLoader/releases/download/v0.11.6.2/tModLoader.Windows.v0.11.6.2.zip)

​	tmod linux端[tModLoader.Linux.v0.11.6.2.tar.gz](https://github.com/tModLoader/tModLoader/releases/download/v0.11.6.2/tModLoader.Linux.v0.11.6.2.tar.gz)  

​	Xftp

​	Xshell

解压到服务器中对应的服务器文件目录下，并且给tModLoaderServer 744权限

```bash
cd /opt/terraria/# 或者 cd /opt/terraria/1353/Linux/  泰拉的服务器文件根目录
chmod +x tModLoaderServer
```

或者使用xftp进入文件夹右键权限设置为744也可以







把config文件中的**TerrariaServer.bin.x86_64** 改为**tModLoaderServer**就可以成功运行了

（当然，也可以把**TerrariaServer.bin.x86_64**删除，然后将**tModLoaderServer**改名为**TerrariaServer.bin.x86_64**）这种办法最方便

进入到服务器文件夹中，使用命令尝试启动

```bash
cd /opt/terraria/ #进入服务器文件根目录，普遍是放在/opt/terraria/1353/Linux/
./tModLoaderServer #启动服务
```



然后就会出现小界面，输入***m***，进入mod菜单，使之自动生成mod文件



进入到***/root/.local/share/Terraria/tModLoader/***目录下

