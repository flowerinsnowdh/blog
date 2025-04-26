+++
title = 'Debian 12 安装 Nvidia 显卡驱动'
date = '2024-09-17T23:10:56+08:00'
publishDate = '2024-09-17T23:10:56+08:00'
lastmod = '2025-04-25T21:07:52+08:00'
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

Nvidia 官方不会为 GNU/Linux 系统制作显卡驱动，所以 Debian 官方为其编写了显卡驱动，只是安装起来稍复杂

## 注意事项
### 1. “非自由”
> Nvidia Graphics Drivers are partly secret source (closed source/proprietary) software, owned by a for profit corporation and not supported by Debian. For those interested in stronger security and stronger privacy, it is suggested to consider using an alternative to Nvidia Graphics Drivers. Which is both fully libre source (open source) and supported by Debian. Such as [Mesa](https://www.flowerinsnow.cn/redirect?to=https://wiki.debian.org/Mesa).
> —— [NvidiaGraphicsDrivers - Debian Wiki](https://wiki.debian.org/NvidiaGraphicsDrivers)

也就是说 Nvidia 的专有驱动程序并非自由和开源，并不受 Debian 支持的，需要更安全、更强隐私的驱动程序~又爱折腾~的用户可以尝试 [Mesa](https://wiki.debian.org/Mesa) 等替代方法

### 2. Debian 12
本文章是基于 Debian 12 的，请勿在其它版本和操作系统中安装，请根据对应文档（例如 Debian 其他版本请根据上方的 Debian Wiki）操作

### 3. 支持的设备
当前文章的安装方法支持[这些Nvidia设备](https://us.download.nvidia.com/XFree86/Linux-x86_64/525.105.17/README/supportedchips.html)（GeForce 600系列及以上），其它设备请根据 Wiki 文档使用[nouveau](https://www.flowerinsnow.cn/redirect?to=https://nouveau.freedesktop.org/)

### 4. 危险
<span style="color:red">打驱动有风险，请先备份好您的密钥以及重要文件，跟着此文章执行产生的一切后果由您亲自承担</span>

## 步骤
### 1. 安装内核头文件
首先对于一个新的 Debian 系统来说，我们需要安装其`linux-headers`文件

<details open="open">

<summary>bash</summary>

```shell
sudo apt install linux-headers-$(uname -r)
```

</details>

### 2. 安装 DKMS
<details open="open">

<summary>bash</summary>

```shell
sudo apt install dkms
```

</details>

### 3. 添加 APT 源组件
添加 Debian 官方的 `contrib`、`non-free`、`non-free-firmware` 仓库组件

使用你自己的方式编辑 `/etc/apt/sources.list`，在官方源上添加 `contrib non-free non-free-firmware` 三个组件

~强烈建议在安装了 openssl 与 ca-certificates 的系统下使用 HTTPS 访问，用 HTTPS 啊！为什么不用！~

<details open="open">

<summary>/etc/apt/sources.list</summary>

```plain
deb https://deb.debian.org/debian/ bookworm main contrib non-free non-free-firmware
deb https://deb.debian.org/debian/ bookworm-updates main contrib non-free non-free-firmware
deb https://deb.debian.org/debian/ bookworm-backports main contrib non-free non-free-firmware
deb https://security.debian.org/debian-security/ bookworm-security main contrib non-free non-free-firmware
```

</details>

#### 3.1. 镜像源（可选）
可以尝试其他镜像源，例如 [南京大学开源镜像站](https://mirrors.nju.edu.cn/)，这能让下面的步骤变快不少

<details open="open">

<summary>/etc/apt/sources.list</summary>

```plain
deb https://mirrors.nju.edu.cn/debian/ bookworm main contrib non-free non-free-firmware
deb https://mirrors.nju.edu.cn/debian/ bookworm-updates main contrib non-free non-free-firmware
deb https://mirrors.nju.edu.cn/debian/ bookworm-backports main contrib non-free non-free-firmware
deb https://mirrors.nju.edu.cn/debian-security/ bookworm-security main contrib non-free non-free-firmware
```

</details>

#### 3.2. 更新软件源
<details open="open">

<summary>bash</summary>

```shell
sudo apt update
```

</details>

### 4. 安装驱动程序
<details open="open">

<summary>bash</summary>

```shell
sudo apt install nvidia-driver firmware-misc-nonfree
```

</details>

#### 4.1. 与 noveau 不兼容

如果出现一个页面，说类似”开源驱动 noveau 内核模块与闭源的 nvidia 模块冲突，解决这个问题的最简单方法是安装完成后重新启动计算机“，直接点击确定

操作结束后，DKMS 就会在在系统中构建 nvidia 驱动程序，此期间可能会消耗些许资源，计算机风扇可能会加速转动

### 5. 使用 DKMS 将驱动程序安装到系统内核
#### 5.1 查看安装的驱动程序版本

<details open="open">

<summary>bash</summary>

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

#### 5.2 使用 DKMS 安装

<details open="open">

<summary>bash</summary>

```shell
sudo dkms install -m nvidia -v current-535.216.01
```

</details>

请将上方的`current-535.216.01`替换为自己的版本号

### 6. Wayland（可选）
如果不需要，就可以到此结束了，如果需要Wayland，请继续下面的操作

#### 6.1. 启用内核模式设置
<details open="open">

<summary>bash</summary>

```shell
sudo echo 'GRUB_CMDLINE_LINUX="$GRUB_CMDLINE_LINUX nvidia-drm.modeset=1"' > /etc/default/grub.d/nvidia-modeset.cfg
```

</details>

<details open="open">

<summary>bash</summary>

```shell
sudo update-grub
```

</details>

#### 6.2. 安装 helper 脚本
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

#### 6.3. 验证启用
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

### 7. 重启计算机
重启计算机，完成安装

## 参考文献
[1] Debian. NvidiaGraphicsDrivers[EB/OL]. (2025-03-22)[2024-09-17]. https://wiki.debian.org/NvidiaGraphicsDrivers

[2] Dissolve. Debian 12 安装Nvidia驱动及黑屏故障排除（纯保姆级教程）[Z/OL]. (2024-06-14)[2024-09-17]. https://blog.csdn.net/m0_53930581/article/details/139692003

[3] MirrorZ Project. Debian 软件仓库镜像使用帮助[EB/OL]. (2025-04-25)[2024-09-17]. https://mirror.nju.edu.cn/mirrorz-help/debian/?mirror=NJU