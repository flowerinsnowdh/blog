+++
title = 'Debian 12 安装 Nvidia显卡驱动'
date = 2024-09-17T23:10:56+08:00
draft = false
categories = [
    'GNU-Linux/Debian/Debian12'
]
tags = [
    'GNU-Linux',
    'Debian',
    'Debian 12',
    '驱动安装',
]
+++
# Debian 12 安装 Nvidia显卡驱动
## 版权与声明
本文部分技术核心取自以下文章，感谢这些文章的作者

1. Debian官方Wiki：[NvidiaGraphicsDrivers - Debian Wiki](https://www.flowerinsnow.cn/redirect?to=https://wiki.debian.org/NvidiaGraphicsDrivers)
2. [@Dissolve](https://www.flowerinsnow.cn/redirect?to=https://im.csdn.net/chat/m0_53930581)在CSDN发布的文章：[Debian 12 安装Nvidia驱动及黑屏故障排除（纯保姆级教程）_debian安装nvidia驱动后黑屏-CSDN博客](https://www.flowerinsnow.cn/redirect?to=https://blog.csdn.net/m0_53930581/article/details/139692003)，转载时进行了增删改，感谢大佬的授权

如果可能，请尽量阅读上方的原文章来学习

## 注意事项
### “非自由”
> Nvidia Graphics Drivers are partly secret source (closed source/proprietary) software, owned by a for profit corporation and not supported by Debian. For those interested in stronger security and stronger privacy, it is suggested to consider using an alternative to Nvidia Graphics Drivers. Which is both fully libre source (open source) and supported by Debian. Such as [Mesa](https://www.flowerinsnow.cn/redirect?to=https://wiki.debian.org/Mesa).
> —— [NvidiaGraphicsDrivers - Debian Wiki](www.flowerinsnow.cn/redirect?to=https://wiki.debian.org/NvidiaGraphicsDrivers)

也就是说Nvidia的驱动程序并非自由和开源，并不受Debian支持的，需要更安全、更强隐私的驱动程序~又爱折腾~的用户可以尝试 [Mesa](https://www.flowerinsnow.cn/redirect?to=https://wiki.debian.org/Mesa) 等替代方法

### Debian 12
本文章是基于 Debian 12 的，请勿在其它版本和操作系统中安装，请根据对应文档（例如Debian其他版本请根据上方的Debian Wiki）操作

### 支持的设备
当前文章的安装方法支持[这些Nvidia设备](https://www.flowerinsnow.cn/redirect?to=https://us.download.nvidia.com/XFree86/Linux-x86_64/525.105.17/README/supportedchips.html)（GeForce 600系列及以上），其它设备请根据Wiki文档使用[nouveau](https://www.flowerinsnow.cn/redirect?to=https://nouveau.freedesktop.org/)

### 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行
以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

### 危险
<p style="color:red">打驱动有风险，请先备份好您的密钥以及重要文件，跟着此文章执行产生的一切后果由您亲自承担</p>

## 安装内核头文件
首先对于一个新的 Debian 系统来说，我们需要安装其`linux-headers`文件

<details open="open">

<summary># bash</summary>

```shell
apt install linux-headers-$(uname -r)
```

</details>

## 安装dkms
<details open="open">

<summary># bash</summary>

```shell
apt install dkms
```

</details>

## 添加apt源
添加 debian 官方的 'contrib'、'non-free'、'non-free-firmware' 仓库源

使用你自己的方式编辑 `/etc/apt/sources.list`

## 安装dkms
<details open="open">

<summary>/etc/apt/sources.list</summary>

```plain
# Debian Bookworm
deb http://deb.debian.org/debian/ bookworm main contrib non-free non-free-firmware
```

</details>

或者可以尝试其他镜像源，例如中科大镜像

~强烈建议在安装了openssl与ca-certificates的系统下使用HTTPS访问，用HTTPS啊！为什么不用！~

<details open="open">

<summary>/etc/apt/sources.list</summary>

```plain
# Debian Bookworm
deb https://mirrors.ustc.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
```

</details>

随后，更新软件源

<details open="open">

<summary># bash</summary>

```shell
apt update
```

</details>

## 安装驱动程序
<details open="open">

<summary># bash</summary>

```shell
apt install nvidia-driver firmware-misc-nonfree
```

</details>

如果出现一个页面，说类似”闭源的驱动程序与当前 nouveau 不兼容，最好的方法就是完成当前安装“，那就听它的，继续安装

操作结束后，DKMS 就会在在系统中构建 nvidia 驱动程序

## 使用dkms将驱动程序安装到系统内核
查看安装的驱动程序版本

<details open="open">

<summary>$ bash</summary>

```shell
ls /usr/src/ | grep nvidia
```

</details>

可能会出现

<details open="open">

<summary>console</summary>

```console
nvidia-current-535.183.01
```

</details>

记下这个版本号（`nvidia-<版本号>`）

随后使用dkms安装

<details open="open">

<summary># bash</summary>

```shell
dkms install -m nvidia -v current-535.183.01
```

</details>

请将上方的`current-535.183.01`替换为自己的版本号

## Wayland
如果不需要，就可以到此结束了，如果需要Wayland，请继续下面的操作

### 启用内核模式设置
<details open="open">

<summary># bash</summary>

```shell
echo 'GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX nvidia-drm.modeset=1"' > /etc/default/grub.d/nvidia-modeset.cfg
```

</details>

<details open="open">

<summary># bash</summary>

```shell
update-grub
```

</details>

### 安装helper脚本
<details open="open">

<summary># bash</summary>

```shell
apt install nvidia-suspend-common
```

</details>

然后启动它们

<details open="open">

<summary># bash</summary>

```shell
systemctl enable nvidia-{suspend,hibernate,resume}.service
```

</details>

### 验证启用
此外，我们还需要验证`PreserveVideoMemoryAllocationsNVIDIA`模块参数是否已启用

如果不启用该参数，`/usr/lib/udev/rules.d/61-gdm.rules`中的`udev`规则将强制回退到 X11

检查参数：

<details open="open">

<summary>$ bash</summary>

```shell
cat /proc/driver/nvidia/params | grep PreserveVideoMemoryAllocations
```

</details>

如果返回1说明已启用

<details open="open">

<summary>console</summary>

```console
PreserveVideoMemoryAllocations: 0
```

</details>

如果返回0说明还未启用，请使用下面的命令覆写配置

<details open="open">

<summary># bash</summary>

```shell
echo 'options nvidia NVreg_PreserveVideoMemoryAllocations=1' > /etc/modprobe.d/nvidia-power-management.conf
```

</details>

## 重启电脑
重启电脑，完成安装