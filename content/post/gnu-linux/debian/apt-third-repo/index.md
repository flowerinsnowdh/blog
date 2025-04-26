+++
title = 'APT 添加第三方仓库'
date = 2025-01-10T11:38:38+08:00
draft = false
categories = [
    'GNU-Linux/Debian/APT软件安装',
]
tags = [
    'GNU-Linux',
    'Debian',
    'APT软件安装',
]
+++

APT（**A**dvanced **P**ackage **T**ool）是 Debian 其派生的 Linux 软件包管理器。APT可以自动下载，配置，安装二进制或者源代码格式的软件包，因此简化了Unix系统上管理软件的过程。

Debian 追求稳定，所以更新通常较慢。为了弥补这一点，部分软件的仓库提供了它们自己的仓库服务器来辅助或者直接用来代替官方仓库，使用它们，可以第一时间安装最新的软件

## 注意事项

### 1. Debian
本文章是基于 Debian 的，理论上对于 Debian 的所有版本以及基于 Debian 开发的 GNU/Linux 发行版都有效，例如 Ubuntu，但是注意源文件需要正确编辑成与操作系统对应的内容，而不是一味地照抄

### 2. 镜像选择
下方文档中可能使用不同的镜像服务器，原因就是有些服务器没有相关内容或不是最新

所以我选择的优先级是

1. 是否可用
2. 是否最新
3. 非营利
4. 以我的家庭网络最快

## 添加方法
### 1. 导入仓库 GPG 公钥
APT 仓库会提供一个 GPG 公钥，以允许用户使用 APT 工具时自动验证下载内容的完整性，为安全性做最重要的保障。

第三方 APT 仓库的 GPG 公钥通常保存在 `/etc/apt/keyrings/` 下，装甲格式（.asc）和非装甲格式（.gpg）都可以使用

例如，如果你想导入 Mozilla APT 密钥环，你可以使用如下命令

<details open="open">

<summary>bash</summary>

```shell
sudo curl -o /etc/apt/keyrings/packages.mozilla.org.asc https://packages.mozilla.org/apt/repo-signing-key.gpg
```

</details>

上面是一个装甲的 GPG 公钥，由于编码成了特殊格式使其都是 ASCII 可打印字符，可以由人类阅读。对于计算机来说，想要使用它必须解装甲，而每次解装甲都会消耗时间和资源，虽然非常少。如果你在乎这点时间和资源，可以像下面这样使用命令行工具+管道的方法直接将其解装甲保存

<details open="open">

<summary>bash</summary>

```shell
curl https://packages.mozilla.org/apt/repo-signing-key.gpg | gpg --dearmor | sudo tee /etc/apt/keyrings/packages.mozilla.org.gpg > /dev/null
```

</details>

这样就可以直接保存二进制的公钥文件

### 2. 添加源文件
源文件中保存了仓库 URL 等内容，它们通常使用 `.list` 文件格式来保存

它们的位置位于 `/etc/apt/sources.list.d/`，每一行代表一个源

#### 2.1. 格式

格式分为 4 个部分

##### 2.1.1 档案类型（Archive type）
第一个条目为档案类型

`deb` 表示二进制包，也就是预编译包

`deb-src` 表示源包

通常情况下使用 `deb` 即可，使用 `deb-src` 可能会让速度变慢

##### 2.1.2 仓库 URL
第二个条目是仓库 URL

##### 2.1.3. 分发版本（Distribution）
第三个条目是软件包分发版本，用于区分不同的操作系统的发行版

官方仓库通常将发行版代号作为分发版本，第三方仓库也有可能会将它们自己的代号作为分发版本

##### 2.1.4. 组件类型（Component）
第四个条目是组件类型

官方仓库通常以自由度作为组件类型，例如

- `main` 由符合 [Debian 自由软件指导方针（以下简称DFSG）](https://www.debian.org/social_contract#guidelines) 的软件包组成
- `contrib` 由符合 DFSG 的软件包组成，但是其依赖项可能包含非 `main` 组件的软件包
- `non-free` 会包含不符合 DFSG 的软件包
- `non-free-firmware` 提供硬件所需固件，但不符合 DFSG

第三方仓库也有可能会有它们自己的分类方式

#### 2.2. 例

<details open="open">

<summary>/etc/apt/sources.list.d/mozilla.list</summary>

```
deb [arch=amd64 signed-by=/etc/apt/keyrings/packages.mozilla.org.asc] https://packages.mozilla.org/apt mozilla main
```

</details>

具体文件名不重要，但必须使用 `.list` 作为后缀名

### 3. 配置优先级
如果第三方源的包可能与 Debian 官方源冲突，你可能会更倾向于使用软件官方的仓库，这时你就需要为软件官方的仓库源设置更高的优先级。如果不会冲突（如打包到 Debian 官方）则可以跳过

它们通常以文件的形式保存在 `/etc/apt/preferences.d/`

例如

<details open="open">

<summary>/etc/apt/preferences.d/mozilla</summary>

```
Package: *
Pin: release a=mozilla
Pin-Priority: 1000
```

</details>

官方源的优先级通常为 500，一般来说只要设置为比 500 更大的数字即可优先使用该仓库。当优先级 ≥1000 时会强制使用该仓库

具体文件名不重要

### 4. 更新源
<details open="open">

<summary>bash</summary>

```shell
sudo apt update
```

</details>

## 常用源
### Mozilla
公钥（装甲）：https://packages.mozilla.org/apt/repo-signing-key.gpg

仓库 URL：https://packages.mozilla.org/apt

* [南京大学开源镜像站](https://mirrors.nju.edu.cn/) 可用：https://mirrors.nju.edu.cn/mozilla/apt

硬件架构：`all`、`amd64`、`arm64`、`i386`

分发版本：`mozilla`

组件类型：`main`

#### 一键部署命令
<details open="open">

<summary>bash</summary>

```
curl https://packages.mozilla.org/apt/repo-signing-key.gpg | gpg --dearmor | sudo tee /etc/apt/keyrings/packages.mozilla.org.gpg > /dev/null
echo "deb [arch=`dpkg --print-architecture` signed-by=/etc/apt/keyrings/packages.mozilla.org.gpg] https://packages.mozilla.org/apt mozilla main" | sudo tee /etc/apt/sources.list.d/mozilla.list > /dev/null
echo -e 'Package: *\nPin: release a=mozilla\nPin-Priority: 1000' | sudo tee /etc/apt/preferences.d/mozilla > /dev/null
```

</details>

### Nginx
公钥（装甲）：https://nginx.org/keys/nginx_signing.key

仓库 URL：https://nginx.org/packages/debian

* [南京大学开源镜像站](https://mirrors.nju.edu.cn/) 可用：https://mirrors.nju.edu.cn/nginx/debian/

硬件架构：`amd64`、`arm64`

分发版本：使用 lsb_release

组件类型：`nginx`

#### 一键部署命令
<details open="open">

<summary>bash</summary>

```
curl https://nginx.org/keys/nginx_signing.key | gpg --dearmor | sudo tee /etc/apt/keyrings/nginx-archive-keyring.gpg > /dev/null
echo "deb [arch=`dpkg --print-architecture` signed-by=/etc/apt/keyrings/nginx-archive-keyring.gpg] https://nginx.org/packages/debian `lsb_release -cs` nginx" | sudo tee /etc/apt/sources.list.d/nginx.list > /dev/null
echo -e 'Package: *\nPin: release o=nginx\nPin-Priority: 1000' | sudo tee /etc/apt/preferences.d/nginx > /dev/null
```

</details>

### Docker
公钥（装甲）：https://download.docker.com/linux/debian/gpg

仓库 URL：https://download.docker.com/linux/debian

* [南京大学开源镜像站](https://mirrors.nju.edu.cn/) 可用：https://mirrors.nju.edu.cn/docker-ce/linux/debian/

硬件架构：`amd64`、`arm64`、`armhf`、`s390x`、`ppc64el`

分发版本：使用 lsb_release

组件类型：`stable`、`edge`、`test`、`nightly`

#### 一键部署命令
<details open="open">

<summary>bash</summary>

```
curl https://download.docker.com/linux/debian/gpg | gpg --dearmor | sudo tee /etc/apt/keyrings/docker.gpg > /dev/null
echo "deb [arch=`dpkg --print-architecture` signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian `lsb_release -cs` stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

</details>

### NodeSource
公钥（装甲）：https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key

仓库 URL：https://deb.nodesource.com/node_23.x

硬件架构：`amd64`、`arm64`、`armhf`、`x86_64`

分发版本：`nodistro`

组件类型：`main`

#### 一键部署命令
<details open="open">

<summary>bash</summary>

```
curl https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/nodesource.gpg > /dev/null
echo "deb [arch=`dpkg --print-architecture` signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_23.x `lsb_release -cs` stable" | sudo tee /etc/apt/sources.list.d/nodesource.list > /dev/null
echo -e 'Package: *\nPin: release o=nodistro\nPin-Priority: 1000' | sudo tee /etc/apt/preferences.d/nodesource > /dev/null
```

</details>

如果需要使用 sudo，请在 tee 命令上使用

### Eclipse Temurin
公钥（装甲）：https://packages.adoptium.net/artifactory/api/gpg/key/public

仓库 URL：https://packages.adoptium.net/artifactory/deb

* [南京大学开源镜像站](https://mirrors.nju.edu.cn/) 可用：https://mirrors.nju.edu.cn/adoptium/deb/

硬件架构：`amd64`、`arm64`、`armhf`、`i386`、`ppc64el`、`riscv64`、`s390x`

分发版本：使用 lsb_release

组件类型：`main`

#### 一键部署命令
<details open="open">

<summary>bash</summary>

```
curl https://packages.adoptium.net/artifactory/api/gpg/key/public | gpg --dearmor | sudo tee /etc/apt/keyrings/adoptium.gpg > /dev/null
echo "deb [arch=`dpkg --print-architecture` signed-by=/etc/apt/keyrings/adoptium.gpg] https://packages.adoptium.net/artifactory/deb `lsb_release -cs` main" | sudo tee /etc/apt/sources.list.d/adoptium.list > /dev/null
```

</details>

### Caddy
公钥（装甲）：https://dl.cloudsmith.io/public/caddy/stable/gpg.key

仓库 URL：https://dl.cloudsmith.io/public/caddy/stable/deb/debian

硬件架构：`amd64`、`arm64`、`armel`、`armhf`、`armv7l`、`i386`、`ppc64el`、`riscv64`、`s390x`

分发版本：`any-version`

组件类型：`main`

#### 一键部署命令
<details open="open">

<summary>bash</summary>

```
curl https://dl.cloudsmith.io/public/caddy/stable/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/caddy-stable-archive-keyring.gpg > /dev/null
echo 'deb [signed-by=/etc/apt/keyrings/caddy-stable-archive-keyring.gpg] https://dl.cloudsmith.io/public/caddy/stable/deb/debian any-version main' | sudo tee /etc/apt/sources.list.d/caddy-stable.list > /dev/null
```

</details>

## 参考文献
[1] Debian. SourcesList[EB/OL]. (2025-04-20)[2025-01-10]. https://wiki.debian.org/SourcesList

[2] Debian. AptConfiguration[EB/OL]. (2024-03-14)[2025-01-10]. https://wiki.debian.org/AptConfiguration

[3] Debian. Debian 社群契约[EB/OL]. (2022-10-01)[2025-01-10]. https://www.debian.org/social_contract