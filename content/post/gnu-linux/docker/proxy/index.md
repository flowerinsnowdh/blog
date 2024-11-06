+++
title = '为 Docker 配置代理服务器'
date = 2024-09-18T19:33:02+08:00
draft = false
categories = [
    'GNU-Linux/Docker',
]
tags = [
    'GNU-Linux',
    'Docker',
    '配置代理'
]
+++

Docker 会读取环境变量`http_proxy`与`https_proxy`，前提是在它的环境中而不是在一个 bash 中

本文章需要您的 Docker 运行于 systemd，否则，请寻找别的方法

如果 Docker 运行于 systemd，则我们通过 export 设置的环境变量不能被它的守护进程读取到

## 注意事项
### 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行
以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

## 编辑环境
找到`docker.service`，它应该位于`/usr/lib/systemd/system/docker.service`，如果不在这里，您可以使用`systemctl status docker`来找到它，输出大概如下

<details open="open">

<summary>console</summary>

```console
Loaded: loaded (/lib/systemd/system/docker.service; enabled; preset: enabled)
```

</details>

编辑这个 service 文件，在`Service`表下添加

<details open="open">

<summary>console</summary>

```ini
Environment="HTTP_PROXY=http://......"
Environment="HTTPS_PROXY=http://......"
```

</details>

## 重载守护进程配置
<details open="open">

<summary># bash</summary>

```ini
systemctl daemon-reload
```

</details>
