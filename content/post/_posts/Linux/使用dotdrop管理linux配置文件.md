---
draft: true # 是否为草稿
title: 使用dotdrop管理dotfiles（配置文件）
data: 2020-8-18 18:10:12
---



官方wiki：https://github.com/deadc0de6/dotdrop/wiki

[w](https://github.com/deadc0de6/dotdrop/wiki/usage#install-dotfiles)

# 安装dotdrop

## 使用包管理安装

```bash
yay -S dotdrop
```

## 作为子模块安装

```bash
## create the repository
$ mkdir dotfiles; cd dotfiles
$ git init

## install dotdrop as a submodule
$ git submodule add https://github.com/deadc0de6/dotdrop.git
$ pip3 install -r dotdrop/requirements.txt --user
$ ./dotdrop/bootstrap.sh

## use dotdrop
$ ./dotdrop.sh --help
```



# 配置dotdrop

在`～/`目录新建dotdrop配置文件`config.yaml`

```yaml
config:
   dotpath: dotfiles
   keepdot: true
dotfiles:
profiles:
```

- `dotpath`：包含要`dotfiles`由dotdrop 管理的点文件的目录的路径（默认）（绝对路径或相对于配置文件位置）

* `keepdot`：导入隐藏文件时保留前导点`dotpath`（默认为*false*）

还有一些其他的选项（不太用的到）见官方文档[#config](https://github.com/deadc0de6/dotdrop/wiki/config)词条

关于配置文件的位置问题

# 导入dotfiles

导入到配置文件中的dotpath中（用空格隔开多个dotfile）

```bash
dotdrop import ~/.vimrc ~/.xinitrc ~/.config/polybar
```

然后在dotfiles目录下使用git命令同步到github即可

# 将dotfiles同步到dotpath

```bash
git clone
cd dotfiles

```

# 更新dotfiles

将本地dotfiles同步到dotpath

强制更新dotfiles（全部）

```bash
dotdrop update -f
```

更新某些dotfile

```bash
dotdrop update ~/.vimrc ~/.xinitrc 
```

# 

# 删除某个dotfile

```bash
dopdrop remove ~/.vimrc ~/.xinitrc 
```



该命令`remove`允许停止使用dotdrop管理特定的点文件。它会：

- 删除配置文件中的条目（在`dotfiles`和下`profile`）
- 从中删除文件 `dotpath`