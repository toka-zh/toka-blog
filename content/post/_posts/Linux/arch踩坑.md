---
draft: true # 是否为草稿
title: Arch-Linux安装踩坑指南
data: 2020-3-5 22:10:12

---



## 显卡部分

##查看显卡信息（不需要输入，用于查看显卡信息）





## 安装与配置`zsh`

安装`zsh`

```bash
yay -S zsh
```

将终端设置为`zsh`

```bash
sudo chsh -s /bin/zsh username #如果使用konsole则右键配置把/bin/bash改为/bin/zsh
```



安装`oh-my-zsh`

```bash
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh
chmod +x install.sh
./install.sh
```



修改`zsh`主题

```bash
vim  ~/.zshrc 
```

找到`ZSH_THEME`

修改为

```bash
ZSH_THEME="agnoster"
```



安装语法高亮

```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git 
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```



最后使设置生效

```bash
source ~/.zshrc
```





# 一些必要软件的安装

汇总

```bash
yay -S i3-gaps polybar picom feh rofi networkmanager_dmenu dotdrop alacritty clight fcitx5-im bluez bluez-utils ranger htop autorandr chromium wps-office-cn wps-office-mui-zh-cn ttf-wps-fonts mpd ncmpcpp 
```



## 

# 

## 窗口管理器及环境

 i3-gaps 窗口管理器本器

 polybar 状态栏

 picom 窗口透明特效

feh 壁纸

rofi 快捷打开应用，类似于`开始`

```bash
yay -S i3-gaps polybar picom feh rofi
```



## 必备软件

### dotfiles管理工具 - dotdrop

```bash
yay -S dotdrop
```

从github获取dotfiles仓库

导入配置文件

### 虚拟终端 - alacritty

```bash
yay -S alacritty
```

### 图形化网络管理 - networknamager_dmenu

```bash
yay -S networknamager_dmenu
```



### 亮度管理 - clight

```bash
yay -S clight
systemctl start clight
systemctl enable clight
```



### 输入法 - fcitx5

```bash
pacman -S fcitx5-im
```



### 蓝牙驱动 - bluez

```bash
sudo pacman -S bluez bluez-utils 
systemctl start bluetooth.service
systemctl enable bluetooth.service
```

### 文件管理器 - ranger

```bash
yay -S ranger
```

### 任务管理器 - htop 

```bash
yay -S  htop 
```





### 自动切换显示器 - autorandr

```bash
yay -S autorandr  
```



## 常用软件

Chrome

````bash
sudo pacman -S chromium
````



网易云音乐

```bash
yay -S netease-cloud-music
```



WPS中文版

```bash
yay -S wps-office-cn wps-office-mui-zh-cn ttf-wps-fonts
```



mpd + ncmpcpp

```bash
yay -S mpd ncmpcpp
```



百度网盘

```bash
yay -S baidunetdisk
```



aira2下载器

```bash
yay -S motrix
```



# 双显卡切换方案

查看显卡信息（不需要输入，用于查看显卡信息）

```bash
lspci -k | grep -A 2 -E "(VGA|3D)"
```





安装依赖

```bash
sudo pacman -S nvidia opencl-nvidia lib32-nvidia-utils lib32-opencl-nvidia mesa lib32-mesa-libgl xf86-video-intel
```



安装`bumblebee`

```bash
sudo pacman -S bumblebee bbswitch
```



把用户添加到bumblebee组里

```bash
sudo gpasswd -a username bumblebee    //把username换成自己的用户名
```



启动`bumblebeed`服务

```bash
sudo systemctl enable bumblebeed.service
```



禁用`nouveau`

```bash
blacklist nouveau
blacklist lbm-nouveau
```





# VMware使用

安装`vmware`虚拟机

```bash
yay -S --noconfirm --needed  vmware-workstation
```



启用相关服务：

```bash
sudo systemctl enable vmware-networks.service  vmware-usbarbitrator.service vmware-hostd.service
sudo systemctl start vmware-networks.service  vmware-usbarbitrator.service vmware-hostd.service
```

最后，加载vmware模块：

sudo modprobe -a vmw_vmci vmmon



-----------

无法加载模块

```bash
git clone https://github.com/mkubecek/vmware-host-modules.git
cd vmware-host-modules
git checkout workstation-15.5.1
git fetch
make
sudo make install
```



无法启动服务

```bash
 yay -S vmware-systemd-services
 systemctl start vmware
 systemctl enable vmware
```





这是由于两台计算机上.config / kwinrc中的这些行引起的：

[Compositing]
OpenGLIsUnsafe=true
我将其设置为false，现在一切又可以正常工作。

我查看了备份。确实是在昨天进行更新时添加的。



# pacman常用命令

清理孤包

```
sudo pacman -R $(pacman -Qdtq)
```



清除未安装软件包的缓存：

```
# pacman -Sc
```

安装一个**本地**包(不从源里下载）：

```
# pacman -U /path/to/package/package_name-version.pkg.tar.xz
```

安装一个**远程**包（不在 *pacman* 配置的源里面）：

```
# pacman -U http://www.example.com/repo/example.pkg.tar.xz
```

`pacman -Scc` 可以清理所有缓存，但这样 pacman 在重装软件包时就只能重新下载了。除非空间不足，否则不应这么做。

