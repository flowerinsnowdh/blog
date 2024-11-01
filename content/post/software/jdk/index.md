+++
title = '下载 Eclipse Temurin'
description = 'Eclipse Temurin 是预编译 OpenJDK'
date = 2024-09-03T21:35:25+08:00
draft = false
categories = [
    '软件安装'
]
tags = [
    '软件安装'
]
+++

本文章介绍的是从 Adoptium 官方网站下载安装版或便携版的 Eclipse Temurin，如果您正在寻找使用 APT 源安装的方法，请参阅[使用 APT 安装 Eclipse Temurin](/p/使用-apt-安装-eclipse-temurin/)

由于[OracleJDK](https://www.flowerinsnow.cn/redirect?to=https://www.oracle.com/java/technologies/)的不再免费，[OpenJDK](https://www.flowerinsnow.cn/redirect?to=https://jdk.java.net/)只提供6个月内的支持，在此不再推荐。

> 在咨询律师之前，请勿使用 Oracle Java SE 开发工具包 (JDK)。 —— Which Version of JDK Should I Use?
>
> 
如果你是个人开发者而非企业开发者，这里强烈推荐 Adoptium 的 Eclipse Temurin，自由软件精神！

推荐使用[LTS](https://www.flowerinsnow.cn/redirect?to=https://zh.wikipedia.org/zh-cn/%E9%95%B7%E6%9C%9F%E6%94%AF%E6%8F%B4)版本，目前的4个是 Java8、Java11、Java17 和 Java21

# 版权与声明
本文部分技术核心取自以下文章，感谢这些文章的作者

1. whichjdk.com：[https://whichjdk.com/](https://www.flowerinsnow.cn/redirect?to=https://whichjdk.com/)

如果可能，请尽量阅读上方的原文章来学习

# 下载
[Latest Releases | Adoptium](https://www.flowerinsnow.cn/redirect?to=https://adoptium.net/temurin/releases/)

## 8
### Windows
#### x64
##### JDK
###### zip
* 版本：8.0.422+5
* 官方下载：https://github.com/adoptium/temurin8-binaries/releases/download/jdk8u422-b05/OpenJDK8U-jdk_x64_windows_hotspot_8u422b05.zip
* sha256sum: `3dfe7750f599c22cfcd37c98dbec0b88a2e5f0e5cc7e89ba76af2a7d47856dbf`

###### msi
* 版本：8.0.422+5
* 官方下载：https://github.com/adoptium/temurin8-binaries/releases/download/jdk8u422-b05/OpenJDK8U-jdk_x64_windows_hotspot_8u422b05.msi
* sha256sum: `9944b308061827c8ad26bedd573eac334c12eaa72c8b7f5ee73a5795e7710204`

##### JRE
###### zip
* 版本：8.0.422+5
* 官方下载：https://github.com/adoptium/temurin8-binaries/releases/download/jdk8u422-b05/OpenJDK8U-jre_x64_windows_hotspot_8u422b05.zip
* sha256sum: `7ac37757292c85ed00a2cc7a38cc0b82d48b337eddea9c7f71414bc7bf439af0`

###### msi
* 版本：8.0.422+5
* 官方下载：https://github.com/adoptium/temurin8-binaries/releases/download/jdk8u422-b05/OpenJDK8U-jre_x64_windows_hotspot_8u422b05.msi
* sha256sum: `6a53b2e2e0eee6b238d79999e4de2fac70efc03922d48ea6d1007f50e7c11307`

### Linux
#### x64
##### JDK
* 版本：8.0.422+5
* 官方下载：https://github.com/adoptium/temurin8-binaries/releases/download/jdk8u422-b05/OpenJDK8U-jdk_x64_linux_hotspot_8u422b05.tar.gz
* sha256sum: `4c6056f6167fae73ace7c3080b78940be5c87d54f5b08894b3517eed1cbb2c06`

##### JRE
* 版本：8.0.422+5
* 官方下载：https://github.com/adoptium/temurin8-binaries/releases/download/jdk8u422-b05/OpenJDK8U-jre_x64_linux_hotspot_8u422b05.tar.gz
* sha256sum: `0ac516cc1eadffb4cd3cfc9736a33d58ea6a396bf85729036c973482f7c063d9`

## 11
### Windows
#### x64
##### JDK
###### zip
* 版本：11.0.24+8
* 官方下载：https://github.com/adoptium/temurin11-binaries/releases/download/jdk-11.0.24%2B8/OpenJDK11U-jdk_x64_windows_hotspot_11.0.24_8.zip
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin11-binaries/jdk-11.0.24%2B8/OpenJDK11U-jdk_x64_windows_hotspot_11.0.24_8.zip
* sha256sum: `e0181952006f9779551511d1f449ca33269a58b7b8802f001fd4ceeff2fd01f3`

###### msi
* 版本：11.0.24+8
* 官方下载：https://github.com/adoptium/temurin11-binaries/releases/download/jdk-11.0.24%2B8/OpenJDK11U-jdk_x64_windows_hotspot_11.0.24_8.msi
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin11-binaries/jdk-11.0.24%2B8/OpenJDK11U-jdk_x64_windows_hotspot_11.0.24_8.msi
* sha256sum: `c6d15bff637a78d2033cd42c592e47c09fe87e7d028ae7d1fbf591c547848917`

##### JRE
###### zip
* 版本：11.0.24+8
* 官方下载：https://github.com/adoptium/temurin11-binaries/releases/download/jdk-11.0.24%2B8/OpenJDK11U-jre_x64_windows_hotspot_11.0.24_8.zip
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin11-binaries/jdk-11.0.24%2B8/OpenJDK11U-jre_x64_windows_hotspot_11.0.24_8.zip
* sha256sum: `78e10f7d025898b7dc7436b2bb986570283cca3cb4a654991a4f9671231da536`

###### msi
* 版本：11.0.24+8
* 官方下载：https://github.com/adoptium/temurin11-binaries/releases/download/jdk-11.0.24%2B8/OpenJDK11U-jre_x64_windows_hotspot_11.0.24_8.msi
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin11-binaries/jdk-11.0.24%2B8/OpenJDK11U-jre_x64_windows_hotspot_11.0.24_8.msi
* sha256sum: `da8e0016ee777b9eb4536991ba5e1ca38be049db13239c2f3924f759730fe329`

### Linux
#### x64
##### JDK
* 版本：11.0.24+8
* 官方下载：https://github.com/adoptium/temurin11-binaries/releases/download/jdk-11.0.24%2B8/OpenJDK11U-jdk_x64_linux_hotspot_11.0.24_8.tar.gz
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin11-binaries/jdk-11.0.24%2B8/OpenJDK11U-jdk_x64_linux_hotspot_11.0.24_8.tar.gz
* sha256sum: `0e71a01563a5c7b9988a168b0c4ce720a6dff966b3c27bb29d1ded461ff71d0e`

##### JRE
* 版本：11.0.24+8
* 官方下载：https://github.com/adoptium/temurin11-binaries/releases/download/jdk-11.0.24%2B8/OpenJDK11U-jre_x64_linux_hotspot_11.0.24_8.tar.gz
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin11-binaries/jdk-11.0.24%2B8/OpenJDK11U-jre_x64_linux_hotspot_11.0.24_8.tar.gz
* sha256sum: `e0c1938093da3780e4494d366a4e6b75584dde8d46a19acea6691ae11df4cda5`

## 17
### Windows
#### x64
##### JDK
###### zip
* 版本：17.0.12+7
* 官方下载：https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.12%2B7/OpenJDK17U-jdk_x64_windows_hotspot_17.0.12_7.zip
* sha256sum: `052049d687ebfda6a4032d54afcd0da6549a23bc2ed04cfaa509746eeacbae71`

###### msi
* 版本：17.0.12+7
* 官方下载：https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.12%2B7/OpenJDK17U-jdk_x64_windows_hotspot_17.0.12_7.msi
* sha256sum: `1e6df6b445d9e995e86fd8225c658df1411d3abab86b540ce4d2063c8a889835`

##### JRE
###### zip
* 版本：17.0.12+7
* 官方下载：https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.12%2B7/OpenJDK17U-jre_x64_windows_hotspot_17.0.12_7.zip
* sha256sum: `646f1f60286670da309b586d0905f1df1a5c2f674d24006823d688bca65388f4`

###### msi
* 版本：17.0.12+7
* 官方下载：https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.12%2B7/OpenJDK17U-jre_x64_windows_hotspot_17.0.12_7.msi
* sha256sum: `62caaa23b88545099612ae77455fe2ac888ad3731ac0758f5cbedad406fd3c6c`

### Linux
#### x64
##### JDK
* 版本：17.0.12+7
* 官方下载：https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.12%2B7/OpenJDK17U-jdk_x64_linux_hotspot_17.0.12_7.tar.gz
* sha256sum: `9d4dd339bf7e6a9dcba8347661603b74c61ab2a5083ae67bf76da6285da8a778`

##### JRE
* 版本：17.0.12+7
* 官方下载：https://github.com/adoptium/temurin17-binaries/releases/download/jdk-17.0.12%2B7/OpenJDK17U-jre_x64_linux_hotspot_17.0.12_7.tar.gz
* sha256sum: `0e8088d7a3a7496faba7ac8787db09dc0264c2bc6f568ea8024fd775a783e13c`

## 21
### Windows
#### x64
##### JDK
###### zip
* 版本：21.0.4+7-LTS
* 官方下载：https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.4%2B7/OpenJDK21U-jdk_x64_windows_hotspot_21.0.4_7.zip
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin21-binaries/jdk-21.0.4%2B7/OpenJDK21U-jdk_x64_windows_hotspot_21.0.4_7.zip
* sha256sum: `c725540d911531c366b985e5919efc8a73dd4030965cd9a740c3d2cd92c72c74`

###### msi
* 版本：21.0.4+7-LTS
* 官方下载：https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.4%2B7/OpenJDK21U-jdk_x64_windows_hotspot_21.0.4_7.msi
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin21-binaries/jdk-21.0.4%2B7/OpenJDK21U-jdk_x64_windows_hotspot_21.0.4_7.msi
* sha256sum: `5eadbdeabdca1a7abf6416a6b35bf7afd86e7edade7b5d44059fbcecacaef372`

##### JRE
###### zip
* 版本：21.0.4+7-LTS
* 官方下载：https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.4%2B7/OpenJDK21U-jre_x64_windows_hotspot_21.0.4_7.zip
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin21-binaries/jdk-21.0.4%2B7/OpenJDK21U-jre_x64_windows_hotspot_21.0.4_7.zip
* sha256sum: `b58f6117d26a138da4cb962b974efc4be4b88b65093366146965d16ad3c45e75`

###### msi
* 版本：21.0.4+7-LTS
* 官方下载：https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.4%2B7/OpenJDK21U-jre_x64_windows_hotspot_21.0.4_7.msi
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin21-binaries/jdk-21.0.4%2B7/OpenJDK21U-jre_x64_windows_hotspot_21.0.4_7.msi
* sha256sum: `cf5b9440680994f1571eb1b83fe017eafbec9e6e8a9cd033b3c099e967c1a553`

### Linux
#### x64
##### JDK
###### tar.gz
* 版本：21.0.4+7-LTS
* 官方下载：https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.4%2B7/OpenJDK21U-jdk_x64_linux_hotspot_21.0.4_7.tar.gz
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin21-binaries/jdk-21.0.4%2B7/OpenJDK21U-jdk_x64_linux_hotspot_21.0.4_7.tar.gz
* sha256sum: `51fb4d03a4429c39d397d3a03a779077159317616550e4e71624c9843083e7b9`

##### JRE
###### tar.gz
* 版本：21.0.4+7-LTS
* 官方下载：https://github.com/adoptium/temurin21-binaries/releases/download/jdk-21.0.4%2B7/OpenJDK21U-jre_x64_linux_hotspot_21.0.4_7.tar.gz
* 中国科学技术大学开源软件镜像：https://mirrors.ustc.edu.cn/adoptium/releases/temurin21-binaries/jdk-21.0.4%2B7/OpenJDK21U-jre_x64_linux_hotspot_21.0.4_7.tar.gz
* sha256sum: `d3affbb011ca6c722948f6345d15eba09bded33f9947d4d67e09723e2518c12a`

# 安装
大部分以zip和tar.gz结尾的均为绿色软件，解压后可以直接使用

msi 则为安装软件，双击安装即可

# 配置
## Windows
如果您下载的是 .msi 版本，那么就是安装版本，可以根据向导来，否则是便携版本，请继续往下参阅

### 便携版本
找一个地方解压缩，并设置环境变量

1. 添加一个环境变量 `JAVA_HOME` 到安装目录下
2. 编辑 `Path` 环境变量，添加一个条目，内容填写安装目录下的 bin 目录，可以指定为 `JAVA_HOME\bin\`

例如

新建 `JAVA_HOME` 环境变量，值为`C:\Program Files\Java\jdk-11.0.12.8+0\`

编辑 `Path` 环境变量，加一条 `%JAVA_HOME%\bin\`

## Linux

找个地方解压缩，并设置环境变量

1. 添加一个环境变量 `JAVA_HOME` 到安装目录下
2. 追加 `PATH` 环境变量，添加一个条目，内容填写安装目录下的 bin 目录，可以指定为 `JAVA_HOME\bin\`

可以添加到 `~/.bashrc`（用户脚本）或 `/etc/profile`（系统脚本），每次进入 bash 会自动执行，也可以使用 `source` 命令手动执行

<details open="open">

<summary>~/.bashrc 或 /etc/profile</summary>

```shell
export JAVA_HOME=/usr/local/java/jdk-11.0.12.8+0/
export PATH=$PATH:$JAVA_HOME/bin/
```

</details>
