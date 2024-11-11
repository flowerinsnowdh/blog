+++
title = '使用 APT 安装 Docker Engine'
date = 2024-11-06T11:02:50+08:00
draft = false
categories = [
    'GNU-Linux/Debian/APT软件安装',
    'Docker',
]
tags = [
    'GNU-Linux',
    'Debian',
    'APT软件安装',
    'Docker',
]
+++

Debian 官方 APT 源追求稳定，而 Docker APT 追求更新快，所以 Docker 软件包后来不会再出现在 Debian 官方 APT 源上了，所以我们要添加 Docker 源

## 注意事项
## 版权与声明
本文部分技术核心取自以下文章，感谢这些文章的作者

1. Docker 的官方文档：[Install Docker Engine on Debian](https://www.flowerinsnow.cn/redirect?to=https://docs.docker.com/engine/install/debian/)

如果可能，请尽量阅读上方的原文章来学习

### Debian 12
本文章是基于 Debian 12 的，理论上对于 Debian 的所有版本以及基于 Debian 开发的 GNU/Linux 发行版都有效，例如 Ubuntu，但是注意源文件需要正确编辑成与操作系统对应的内容，而不是一味地照抄

### 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行

以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

### 危险
<p style="color:red">更改 APT 环境可能导致您的服务器宕机、系统崩溃甚至数据丢失，请您提前关闭相应服务，并做好备份，跟着此文章执行产生的一切后果由您亲自承担</p>

## 一、卸载旧版本
如果您之前安装过、系统预装等方式已经安装了 Docker Engine，那首先需要将它们卸载

如果您不确定您是否安装了 Docker Engine，执行就完了

## Debian
<details open="open">

<summary># bash</summary>

```shell
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do apt remove $pkg; done
```

</details>

## Ubuntu
<details open="open">

<summary># bash</summary>

```shell
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do apt remove $pkg; done
```

</details>

## 二、添加 Docker APT 仓库
### 2.1. 安装 HTTP/HTTPS 客户端
为了访问 WEB，我们需要一个命令行工具 `curl`

为了访问 HTTPS，我们需要 `ca-certificates`，也就是证书颁发机构的根证书

<details open="open">

<summary># bash</summary>

```shell
apt install ca-certificates curl
```

</details>

### 2.2. 添加仓库公钥
为了安全地验证下载文件的完整性和抗抵赖性，我们需要安装 Docker 仓库的 PGP 公钥

#### 2.2.1. 创建公钥存储目录
<details open="open">

<summary># bash</summary>

```shell
install -m 0755 -d /etc/apt/keyrings/
```

</details>

#### 2.2.2. 安装 Docker 仓库公钥
请展开对应方案阅读

##### Debian
<details>

<summary>Debian 方案</summary>

<details open="open">

<summary># bash</summary>

```shell
curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
```

</details>

* 您可能需要在这一步上使用代理服务器，以访问`docker.com`这个域名
* 您也可以使用镜像服务器，例如[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/) 提供了 Docker 社区版的镜像服务器

以下使用[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/)镜像服务器

<details open="open">

<summary># bash</summary>

```shell
curl -fsSL https://mirrors.ustc.edu.cn/docker-ce/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
```

</details> <!-- END # bash -->

</details> <!-- END Debian 方案 -->

##### Ubuntu
<details>

<summary>Ubuntu 方案</summary>

<details open="open">

<summary># bash</summary>

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

</details>

* 您可能需要在这一步上使用代理服务器，以访问`docker.com`这个域名
* 您也可以使用镜像服务器，例如[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/) 提供了 Docker 社区版的镜像服务器

以下使用[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/)镜像服务器

<details open="open">

<summary># bash</summary>

```shell
curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
```

</details> <!-- END # bash -->

</details> <!-- END Ubuntu 方案 -->

### 2.3. 添加仓库源
请展开对应方案阅读

#### Debian
<details>

<summary>Debian 方案</summary>

<details open="open">

<summary># bash</summary>

```shell
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

</details> <!-- END # bash -->

* 如果您正在使用 Debian 衍生发行版（如 Kali），您可能需要将上文中的`$VERSION_CODENAME`替换成您发行版的代号
* 您可能需要在更新 APT 源时使用代理服务器，以访问`docker.com`这个域名
* 您也可以使用镜像服务器，例如[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/) 提供了 Docker 社区版的镜像服务器

以下使用[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/)镜像服务器

<details open="open">

<summary># bash</summary>

```shell
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://mirrors.ustc.edu.cn/docker-ce/linux/debian/ \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

</details> <!-- END # bash -->

</details> <!-- END Debian 方案 -->

#### Ubuntu 方案
<details>

<summary>Ubuntu 方案</summary>

<details open="open">

<summary># bash</summary>

```shell
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

</details> <!-- END # bash -->

* 如果您正在使用 Ubuntu 衍生发行版（如 Linux Mint），您可能需要将上文中的`$VERSION_CODENAME`替换成`$UBUNTU_CODENAME`
* 您可能需要在更新 APT 源时使用代理服务器，以访问`docker.com`这个域名
* 您也可以使用镜像服务器，例如[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/) 提供了 Docker 社区版的镜像服务器

以下使用[中国科学技术大学开源软件镜像](https://mirrors.ustc.edu.cn/)镜像服务器

<details open="open">

<summary># bash</summary>

```shell
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://mirrors.ustc.edu.cn/docker-ce/linux/ubuntu/ \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

</details> <!-- END # bash -->

</details> <!-- END Ubuntu 方案 -->

### 2.4. 更新 APT 源
<summary># bash</summary>

```shell
apt update
```

</details>

## 三、安装 Docker Engine
<details open="open">

<summary># bash</summary>

```shell
apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

</details>

## 四、测试安装结果
<details open="open">

<summary># bash</summary>

```shell
docker run hello-world
```

</details>

这条命令将会下载 Docker 镜像，并创建一个 Docker 容器，输出一些确认信息最后退出

* 您可能需要在这一步上使用代理服务器，以访问 Docker 镜像仓库，方案可见[为 Docker 配置代理服务器](/p/%E4%B8%BA-docker-%E9%85%8D%E7%BD%AE%E4%BB%A3%E7%90%86%E6%9C%8D%E5%8A%A1%E5%99%A8/)、
