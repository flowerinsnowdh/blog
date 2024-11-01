+++
title = '使用 APT 安装 Eclipse Temurin'
description = 'Eclipse Temurin 是预编译 OpenJDK'
date = 2024-11-02T02:44:46+08:00
draft = false
categories = [
    'GNU-Linux/Debian',
    '软件安装/APT',
    'Adoptium Eclipse Temurin',
]
tags = [
    'GNU-Linux',
    'Debian',
    '软件安装',
    'APT 软件安装',
    'Adoptium Eclipse Temurin',
]
+++

本文章介绍的是使用 Adoptium 官方仓库利用 APT 安装 Eclipse Temurin，如果您正在寻找下载 Eclipse Temurin 的方法，请参阅[下载 Eclipse Temurin](/p/下载-eclipse-temurin/)

Eclipse Temurin 是 Adoptium OpenJDK 发行版的名称

## 注意事项
## 版权与声明
本文部分技术核心取自以下文章，感谢这些文章的作者

1. Aadoptium 的官方文档：[Linux (RPM/DEB/APK) installer packages | Adoptium](https://www.flowerinsnow.cn/redirect?to=https://adoptium.net/installation/linux/)

如果可能，请尽量阅读上方的原文章来学习

### Debian 12
本文章是基于 Debian 12 的，理论上对于 Debian 的所有版本以及基于 Debian 开发的 GNU/Linux 发行版都有效，例如 Ubuntu，但是注意源文件需要正确编辑成与操作系统对应的内容，而不是一味地照抄

### 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行

以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

### 危险
<p style="color:red">更改 APT 环境可能导致您的服务器宕机、系统崩溃甚至数据丢失，请您提前关闭相应服务，并做好备份，跟着此文章执行产生的一切后果由您亲自承担</p>

## 确保存在必要的软件包

<details open="open">

<summary># bash</summary>

```shell
apt install wget apt-transport-https gpg
```

</details>

## 下载 Eclipse Adoptium GPG 密钥
<details open="open">

<summary># bash</summary>

```shell
wget -qO - https://packages.adoptium.net/artifactory/api/gpg/key/public | gpg --dearmor | tee /etc/apt/trusted.gpg.d/adoptium.gpg > /dev/null
```

</details>

## 配置 Eclipse Adoptium apt 仓库源
要检查支持的所有版本，请查看 [Index of deb/dists](https://packages.adoptium.net/ui/native/deb/dists/) 列表。

### 对于 Debian
<details open="open">

<summary># bash</summary>

```shell
echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
```

</details>

### 对于Linux Mint（基于 Ubuntu）
对于 Linux Mint (基于 Ubuntu) 您必须将 `VERSION_CODENAME` 替换为 `UBUNTU_CODENAME`. 

<details open="open">

<summary># bash</summary>

```shell
echo "deb https://packages.adoptium.net/artifactory/deb $(awk -F= '/^UBUNTU_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
```

</details>

### 镜像源
如果您想使用国内的 Adoptium 镜像 apt 源，[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/)是个不错的选择

您只需要将命令中的 `packages.adoptium.net/artifactory/deb` 改为 `mirrors.ustc.edu.cn/adoptium/deb/` 即可，例如

<details open="open">

<summary># bash</summary>

```shell
echo "deb https://mirrors.ustc.edu.cn/adoptium/deb/ $(awk -F= '/^UBUNTU_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
```

</details>

Ubuntu 同理

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
