+++
title = '为 Google Pixel 6 刷入 KernelSU'
date = 2024-09-30T10:06:15+08:00
draft = false
categories = [
    'Google Pixel 6',
    'KernelSU',
]
tags = [
    'Google Pixel 6',
    'KernelSU',
]
+++

# 为 Google Pixel 6 刷入KernelSU
Google Pixel 稍微有点不一样，它的压缩方式是 legacy_lz4，GKI 模式不能通过官方提供的 boot 镜像，需要通过手动修补 boot.img 的方式刷入 KernelSU，其他设备其实也可以这样执行

下文会使用 KernelSU 文档的先进入临时 GKI 映像，再刷入 LKM 的方式

## 注意事项
### 版权与声明
本文部分技术核心取自以下文章，感谢这些文章的作者

1. KernelSU 的官方安装文档：[安装 | KernelSU](https://www.flowerinsnow.cn/redirect?to=https://kernelsu.org/guide/installation.html)

如果可能，请尽量阅读上方的原文章来学习

### bootloader 锁
只有欧版的 Google Pixel 才能解除 bootloader 锁

### 防火长城
由于防火长城的存在，您不能直接访问`android.com`、`google.com`等域名（包括其子域名），所以请您自己想办法

### 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行

以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

### 危险
<p style="color:red">数据无价，请确保您已经备份好了您的重要数据，根据此文章执行产生的一切后果由您亲自承担</p>

## 准备工具
### 一台电脑
您需要一台电脑才能完成该操作

### Android Debug Bridge
简称 adb，请从 [Android 调试桥 (adb)](https://www.flowerinsnow.cn/redirect?to=https://developer.android.com/studio/command-line/adb) 下载，并添加到环境变量中

不多做解释和教程

### 驱动程序
#### Windows
若使用 Windows，则需要下载安装 USB 驱动程序：[Get the Google USB Driver](https://www.flowerinsnow.cn/redirect?to=https://developer.android.com/studio/run/win-usb)

请根据官方文档来完成安装操作

#### Unix
若使用 Linux 或 MacOS，则不需要也不应该下载安装 USB 驱动程序，请参阅[在硬件设备上运行应用](https://www.flowerinsnow.cn/redirect?to=https://developer.android.com/studio/run/device)

## 解除 bootloader 锁
### 解锁 OEM
进入`设置`-`关于本机`-狂点`Build 号`7次以上-输入锁屏密码（如果有）-返回上一级-`系统`-`开发者选项`-开启`OEM 解锁`

### 进入 bootloader

<details open="open">

<summary>$ bash</summary>

```shell
adb reboot bootloader
```

</details>

### 解除 bootloader 锁
<p style="color:red"><b>警告：</b>此操作将清除手机上的所有数据</p>

<details open="open">

<summary>$ bash</summary>

```shell
fastboot flashing unlock
```

</details>

随后在手机上通过上下音量键和锁屏键选择解除bootloader锁

## 下载并安装 KernelSU 管理器
### 下载 KernelSU 管理器
[https://github.com/tiann/KernelSU/releases](https://www.flowerinsnow.cn/redirect?to=https://github.com/tiann/KernelSU/releases)

下载并安装`KernelSU_<版本>.apk`

### 安装 KernelSU 管理器

<details open="open">

<summary>$ bash</summary>

```shell
adb install KernelSU_<版本>.apk
```

</details>

### 下载 AnyKernel3
打开 KernelSU 管理器，查看内核版本，并从 releases 下载对应的 AnyKernel3

### 获取 Image 文件
解压出其中的`Image`文件

<details open="open">

<summary>$ bash</summary>

```shell
unzip AnyKernel3-<内核版本>_<构建日期>.zip 
```

</details>

### 确认文件
将其传入手机内，至任意位置，下面以`/data/local/tmp/`为例

<details open="open">

<summary>$ bash</summary>

```shell
adb push Image /data/local/tmp/
```

</details>

## 获取出厂映像
### 下载出厂映像
从 [Nexus 和 Pixel 设备的出厂映像](https://www.flowerinsnow.cn/redirect?to=https://developers.google.com/android/images) 将出厂映像下载下来，以 Google Pixel 6 的 Android 14 为例

<p style="color:blue"><b>提示：</b>如果您是第一次访问该下载站，您可能需要将页面滑到最底端，同意“条款及条件”（即点击“确认”按钮）</p>

<p style="background:yellow"><b>注意：</b>必须使用和 OTA 一样的版本的映像，否则将会导致手机无法开机，这需要您找到对应版本的映像重新刷入或将 OTA 升级为该版本。这种情况可能会因为需要您 Wipe data/factory reset 而丢失数据</p>

### 解压出厂映像
下载应该会得到一个`<设备代号>-<构建版本>-factory-<提交哈希值>.zip`压缩包，将其解压

<details open="open">

<summary>$ bash</summary>

```shell
unzip <设备代号>-<构建版本>-factory-<提交哈希值>.zip
```

</details>

### 获取映像文件
其内部应该还有一个`image-<设备代号>-<构建版本>.zip`，将其也解压

<details open="open">

<summary>$ bash</summary>

```shell
unzip image-<设备代号>-<构建版本>.zip
```

</details>

### 确认文件
此时应该会有一个`boot.img`，将其传入手机内，至任意位置，下面以`/data/local/tmp/`为例

<details open="open">

<summary>$ bash</summary>

```shell
adb push boot.img /data/local/tmp/
```

</details>

## 获取 magiskboot
我们需要`magiskboot`可执行程序来帮助我们修补`boot.img`

### 下载 Magisk
可以从 [Magisk/releases](https://www.flowerinsnow.cn/redirect?to=https://github.com/topjohnwu/Magisk/releases) 下载 .apk 文件

可以下载正式版（一般命名为`Magisk-<版本>.apk`），也可以下载 Pre release 版（一般命名为`app-release.apk`）

下文以正式版示范

### 解压 .apk 文件

<details open="open">

<summary>$ bash</summary>

```shell
unzip Magisk-<版本>.apk
```

</details>

### 提取 magiskboot
找到`lib/arm64-v8a/libmagiskboot.so`，将其重命名为`magiskboot`

<details open="open">

<summary>$ bash</summary>

```shell
mv libmagiskboot.so magiskboot
```

</details>

### 确认文件
将其传入手机内，至一个可执行的位置，下面以`/data/local/tmp/`为例

<details open="open">

<summary>$ bash</summary>

```shell
adb push magiskboot /data/local/tmp/
```

</details>

## 修补 boot.img
### 进入 shell

<details open="open">

<summary>$ bash</summary>

```shell
adb shell
```

</details>

### 进入 magiskboot 所在目录

<details open="open">

<summary>$ bash:adb shell</summary>

```shell
cd /data/local/tmp/
```

</details>

### 赋予 magiskboot 执行权限

<details open="open">

<summary>$ bash:adb shell</summary>

```shell
chmod +x magiskboot
```

</details>

### 解包 boot.img

<details open="open">

<summary>$ bash:adb shell</summary>

```shell
./magiskboot unpack boot.img
```

</details>

### 使用 Image 覆盖 kernel
此时应该会有一个名为`kernel`的文件，用我们刚刚的`Image`文件将其覆盖

<details open="open">

<summary>$ bash:adb shell</summary>

```shell
mv Image kernel
```

</details>

### 重新打包 boot.img
<details open="open">

<summary>$ bash:adb shell</summary>

```shell
./magisk repack boot.img
```

</details>

会生成一个`new-boot.img`文件

### 退出 adb shell
<details open="open">

<summary>$ bash:adb shell</summary>

```shell
exit
```

</details>

## 准备刷写
### 拉取 boot.img
<details open="open">

<summary>$ bash</summary>

```shell
adb pull /data/local/tmp/new-boot.img
```

</details>

这就是修补后的`boot.img`

### 进入 bootloader
<details open="open">

<summary>$ bash</summary>

```shell
adb reboot bootloader
```

</details>

## 选择
现在你已经拥有了我们需要的文件，接下来你有两个选择，具体如何选择，请参阅 KernelSU 的文档[安装介绍](https://www.flowerinsnow.cn/redirect?to=https://kernelsu.org/guide/installation.html#introduction)

1. 直接使用 GKI 模式使用 KernelSU，请跳转[GKI 模式](#gki-模式)
2. 使用 LKM 模式使用 KernelSU，请跳转[LKM 模式](#lkm-模式)

## GKI 模式
### 刷写映像
将刚刚修补的`boot.img`刷入 boot 分区，就是 GKI 模式

<details open="open">

<summary>$ bash</summary>

```shell
fastboot flash boot new-boot.img
```

</details>

### 重启手机
重启手机，完成安装

## LKM 模式
### 临时 GKI 模式
将刚刚修补的`boot.img`作为临时启动映像，进入临时 GKI 模式，获取临时 root 权限

<details open="open">

<summary>$ bash</summary>

```shell
fastboot boot new-boot.img
```

</details>

### 刷写 LKM 模式
进入系统后，打开 KernelSU 管理器，进入安装页面，选择`直接安装（推荐）`

### 重启手机
重启手机，完成安装