+++
title = '使用 APT 安装 Eclipse Temurin'
description = 'Eclipse Temurin 是预编译 OpenJDK'
date = 2024-11-02T02:44:46+08:00
draft = false
categories = [
    'GNU-Linux/Debian/APT软件安装',
    'Adoptium Eclipse Temurin',
]
tags = [
    'GNU-Linux',
    'Debian',
    'APT软件安装',
    'Adoptium Eclipse Temurin',
]
+++

本文章介绍的是使用 Adoptium 官方仓库利用 APT 安装 Eclipse Temurin，如果您正在寻找下载 Eclipse Temurin 的方法，请参阅[下载 Eclipse Temurin](/p/下载-eclipse-temurin/)

Eclipse Temurin 是 Adoptium OpenJDK 发行版的名称

## 注意事项
### 版权与声明
本文部分技术核心取自以下文章，感谢这些文章的作者

1. Adoptium 的官方文档：[Linux (RPM/DEB/APK) installer packages | Adoptium](https://www.flowerinsnow.cn/redirect?to=https://adoptium.net/installation/linux/)

如果可能，请尽量阅读上方的原文章来学习

### Debian 12
本文章是基于 Debian 12 的，理论上对于 Debian 的所有版本以及基于 Debian 开发的 GNU/Linux 发行版都有效，例如 Ubuntu，但是注意源文件需要正确编辑成与操作系统对应的内容，而不是一味地照抄

### 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行

以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

### 危险
<p style="color:red">更改 APT 环境可能导致您的服务器宕机、系统崩溃甚至数据丢失，请您提前关闭相应服务，并做好备份，跟着此文章执行产生的一切后果由您亲自承担</p>

## 一、确保存在必要的软件包

<details open="open">

<summary># bash</summary>

```shell
apt install wget apt-transport-https gpg
```

</details>

## 二、下载安装 Eclipse Adoptium GPG 密钥
<details open="open">

<summary># bash</summary>

```shell
wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | gpg --dearmor | tee /etc/apt/trusted.gpg.d/adoptium.gpg > /dev/null
```

</details>

## 三、配置 Eclipse Adoptium apt 仓库源
要检查支持的所有 GNU/Linux 发行版，请查看 [Index of deb/dists](https://packages.adoptium.net/ui/native/deb/dists/) 列表。

请展开对应方案阅读

### Debian
<details>

<summary>Debian 方案</summary>

<details open="open">

<summary># bash</summary>

```shell
echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
```

</details> <!-- END # bash -->

* 如果您正在使用 Debian 衍生发行版（如 Kali），您可能需要将上文中的`$VERSION_CODENAME`替换成您发行版的代号
* 您也可以使用镜像服务器，例如[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/) 提供了 adoptium 仓库的镜像服务器

以下使用[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/)镜像服务器

<details open="open">

<summary># bash</summary>

```shell
echo "deb https://mirrors.ustc.edu.cn/adoptium/deb/ $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
```

</details> <!-- END # bash -->

</details> <!-- END Debian 方案 -->

### Ubuntu
<details>

<summary>Ubuntu 方案</summary>

<details open="open">

<summary># bash</summary>

```shell
echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
```

</details> <!-- END # bash -->

* 如果您正在使用 Ubuntu 衍生发行版（如 Linux Mint），您可能需要将上文中的`$VERSION_CODENAME`替换成`$UBUNTU_CODENAME`
* 您也可以使用镜像服务器，例如[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/) 提供了 adoptium 仓库的镜像服务器

以下使用[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/)镜像服务器

<details open="open">

<summary># bash</summary>

```shell
echo "deb https://mirrors.ustc.edu.cn/adoptium/deb/ $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
```

</details> <!-- END # bash -->

</details> <!-- END Debian 方案 -->

<details open="open">

<summary># bash</summary>

```shell
echo "deb https://mirrors.ustc.edu.cn/adoptium/deb/ $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
```

</details>

或者，您也可以手动编辑 `/etc/apt/sources.list.d/adoptium.list`

## 更新 apt 源
<details open="open">

<summary># bash</summary>

```shell
apt update
```

</details>

## 安装 Eclipse Temurin
Eclipse Temurin 在 apt 软件包中使用的名称是 `temurin-<version>-jdk`，例如 `temurin-21-jdk`

您可以按需安装

<details open="open">

<summary># bash</summary>

```shell
apt install temurin-21-jdk
```

</details>

## 测试是否安装完成
<details open="open">

<summary>$ bash</summary>

```shell
java -version
```

输出结果

</details>

<details open="open">

<summary>console</summary>

```console
openjdk version "21.0.4" 2024-07-16 LTS
OpenJDK Runtime Environment Temurin-21.0.4+7 (build 21.0.4+7-LTS)
OpenJDK 64-Bit Server VM Temurin-21.0.4+7 (build 21.0.4+7-LTS, mixed mode, sharing)
```

</details>
