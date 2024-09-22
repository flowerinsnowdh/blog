+++
title = '非 root 用户管理 Docker'
date = 2024-09-22T18:39:58+08:00
draft = false
categories = [
    'GNU/Linux',
    'Docker',
]
tags = [
    'GNU/Linux',
    'Docker',
]
+++

https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user

# 非 root 用户管理 Docker
## 问题描述
Docker 守护进程会绑定一个 Unix socket，默认只允许 root 用户操作

## 注意事项
## 版权与声明
本文部分技术核心取自以下文章，感谢这些文章的作者

1. Docker 官方文档：[Manage Docker as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)

如果可能，请尽量阅读上方的原文章来学习

### Debian 12
本文章是基于 Debian 12 的，其他的 GNU/Linux 操作系统可能会失效

### 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行
以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

### 危险
<p style="color:red">下文中会创建一个 docker 用户组，此用户拥有 root 级别的权限，您可能因此受到攻击，请三思而后行。如果您没有这个需求，请在每次调用 docker 时使用 sudo</p>

<p style="color:red">跟着此文章执行产生的一切后果由您亲自承担</p>

## 步骤
### 创建用户组

<details open="open">

<summary># bash</summary>

```shell
groupadd docker
```

</details>

### 将您的用户添加到 docker 用户组

<details open="open">

<summary># bash</summary>

```shell
sudo usermod -aG docker $USER
```

</details>

### 重新登录
登出并重新登录您的用户