+++
title = 'Debian 12 安装 Intel 显卡驱动'
date = 2024-11-03T13:35:26+08:00
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

## 注意事项
## 版权与声明
本文部分技术核心取自以下文章，感谢这些文章的作者

1. Debian官方Wiki：[NvidiaGraphicsDrivers - Debian Wiki](https://www.flowerinsnow.cn/redirect?to=https://wiki.debian.org/GraphicsCard#Intel)

如果可能，请尽量阅读上方的原文章来学习

### Debian 12
本文章是基于 Debian 12 的，如果您是其他操作系统，可能无法通过此文安装

### 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行
以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

### 危险
<p style="color:red">打驱动有风险，请先备份好您的密钥以及重要文件，跟着此文章执行产生的一切后果由您亲自承担</p>

## 一、通过 APT 安装
<details open="open">

<summary># bash</summary>

```shell
apt install xserver-xorg-core
```

</details>
