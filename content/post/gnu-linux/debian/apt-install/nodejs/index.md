+++
title = '使用 APT 安装 Node.js'
date = 2024-11-11T15:16:14+08:00
draft = false
categories = [
    'GNU-Linux/Debian/APT软件安装',
    'Node.js',
]
tags = [
    'GNU-Linux',
    'Debian',
    'APT软件安装',
    'Node.js',
]
+++

Debian 官方 APT 源仍然停留在 18.x 版本上，根据官方文档，我们可以通过添加 NodeSource 源安装

## 注意事项
### 版权与声明
本文部分技术核心取自以下文章，感谢这些文章的作者

1. NodeSource 的官方 GitHub 仓库：[nodesource/distributions](https://www.flowerinsnow.cn/redirect?to=https://github.com/nodesource/distributions)

如果可能，请尽量阅读上方的原文章来学习

### Debian 12
本文章是基于 Debian 12 的，理论上对于 Debian 的所有版本以及基于 Debian 开发的 GNU/Linux 发行版都有效，例如 Ubuntu，但是注意源文件需要正确编辑成与操作系统对应的内容，而不是一味地照抄

### 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行

以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

## 一、确保存在必要的软件包
<details open="open">

<summary># bash</summary>

```shell
apt install apt-transport-https ca-certificates curl gnupg
```

</details>

## 二、下载安装 NodeSource GPG 公钥
为了安全地验证下载文件的完整性和抗抵赖性，我们需要安装 Docker 仓库的 PGP 公钥

<details open="open">

<summary># bash</summary>

```shell
curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /usr/share/keyrings/nodesource.gpg
```

</details>

确保所有用户拥有读权限

<details open="open">

<summary># bash</summary>

```shell
chmod 644 /usr/share/keyrings/nodesource.gpg
```

</details>

## 三、添加 NodeSource APT 源
<details open="open">

<summary># bash</summary>

```shell
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/nodesource.gpg] https://deb.nodesource.com/node_22.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list > /dev/null
```

</details>

## 四、配置源
### 4.1. 配置 N|solid
<details open="open">

<summary># bash</summary>

```shell
echo "Package: nsolid" | tee /etc/apt/preferences.d/nsolid > /dev/null
echo "Pin: origin deb.nodesource.com" | tee -a /etc/apt/preferences.d/nsolid > /dev/null
echo "Pin-Priority: 600" | tee -a /etc/apt/preferences.d/nsolid > /dev/null
```

</details>

### 4.2. 配置 Node.js
<details open="open">

<summary># bash</summary>

```shell
echo "Package: nodejs" | tee /etc/apt/preferences.d/nodejs > /dev/null
echo "Pin: origin deb.nodesource.com" | tee -a /etc/apt/preferences.d/nodejs > /dev/null
echo "Pin-Priority: 600" | tee -a /etc/apt/preferences.d/nodejs > /dev/null
```

</details>

## 五、更新 APT 源
<details open="open">

<summary># bash</summary>

```shell
apt update
```

</details>

## 六、安装 Node.js
<details open="open">

<summary># bash</summary>

```shell
apt install nodejs
```

</details>