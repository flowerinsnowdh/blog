+++
title = 'Archlinux 下 Fcitx5-Rime 在 Electron 软件无法输入切换的问题'
description = 'Archlinux 在安装了 Fcitx5-Rime 虚拟键盘时无法在类似 QQ、VSCode 等 Electron 软件下输入中文'
date = 2025-05-16T16:52:42+08:00
draft = false
categories = [
    'GNU-Linux',
    'Archlinux',
    '问题解决经验'
]
tags = [
    'GNU-Linux',
    'Archlinux',
    '问题解决经验',
]
+++

在安装了 Archlinux 并安装了 PlasmaKDE/Wayland、Fcitx5-Rime 虚拟键盘（输入法）后，我又通过 AUR 安装了 QQ、微信、VSCode 等工具，出现了一个问题：不能在它们里面使用 Fcitx5-Rime 输入中文了，下面是解决经验，仅供参考

## 注意
本方法适用于 PlasmaKDE/Wayland，理论上来说所有 Electron 软件都支持用下方的方法解决

## 一、确定标签配置文件
以 QQ 为例，我们可以查看它的启动脚本

<details open="open">

<summary>/usr/bin/linuxqq</summary>

```shell
#!/bin/bash

if [ -d ~/.config/QQ/versions ]; then
        find ~/.config/QQ/versions -name sharp-lib -type d -exec rm -r {} \; 2>/dev/null
        find ~/.config/QQ/versions -name libssh2.so.1 -type f -exec rm {} \; 2>/dev/null
fi

rm -rf ~/.config/QQ/crash_files/*

XDG_CONFIG_HOME=${XDG_CONFIG_HOME:-~/.config}

if [[ -f "${XDG_CONFIG_HOME}/qq-flags.conf" ]]; then
        mapfile -t QQ_USER_FLAGS <<<"$(grep -v '^#' "${XDG_CONFIG_HOME}/qq-flags.conf")"
        echo "User flags:" ${QQ_USER_FLAGS[@]}
fi

exec /opt/QQ/qq ${QQ_USER_FLAGS[@]} "$@"
```

</details>

然后提取关键词 `${XDG_CONFIG_HOME}/qq-flags.conf`，这个就是该应用的标签配置文件

## 二、添加标签
编辑上面的获取的配置文件，如果不存在就创建一个，然后写入下面的内容

<details open="open">

<summary>~/.config/qq-flags.conf</summary>

```
--ozone-platform-hint=wayland
--enable-wayland-ime
```

</details>

## 三、重启应用
重启应用，问题得到解决