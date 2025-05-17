+++
title = 'Archlinux 下 Fcitx5 无法在部分应用输入中文的问题'
date = 2025-05-16T16:52:42+08:00
lastmod = 2025-05-17T05:32:17+08:00
draft = false
categories = [
    'GNU-Linux',
    'Archlinux',
    '问题解决经验',
    'Fcitx5',
]
tags = [
    'GNU-Linux',
    'Archlinux',
    '问题解决经验',
    'Fcitx5',
]
+++

在安装了 Archlinux 并安装了 PlasmaKDE/Wayland、Fcitx5 虚拟键盘（输入法）后，我又通过安装了 QQ、微信、VSCode、IntelliJ IDEA、Minecraft 等应用，出现了一个问题：不能在它们里面使用 Fcitx5 输入中文了，下面是解决经验，仅供参考

## 注意
本方法适用于 Archlinux/Fcitx5/KDE Plasma/Wayland，并确定您已经配置好了输入法（如配置了虚拟键盘等）

## 通用解决方法：添加环境变量
最好添加三个环境变量，你可以不去理解它们，如果想要理解，请参阅相关文档（可以在[#参考资料](#参考资料)中参阅）

```
XMODIFIERS=@im=fcitx
GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
```

您可以通过修改 `/etc/environment`，或软件启动脚本，或软件内部设置实现它

### 例：启动命令行
您可以在命令前添加它们以添加应用程序的环境变量

<details open="open">

<summary>bash</summary>

```shell
XMODIFIERS=@im=fcitx GTK_IM_MODULE=fcitx QT_IM_MODULE=fcitx your_command_here
```

</details>

## 对于 Electron 应用的解决方法
可以为 Electron 应用添加标记来修复这个问题，这个方法同样对 chromium 内核的应用有效

```
--enable-features=UseOzonePlatform
--ozone-platform-hint=wayland
--enable-wayland-ime
```

如果你是 Electron 应用遇到了这个问题，那么可以按照如下步骤添加启动 flags

### 一、确定标签配置文件
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

### 二、添加标签
编辑上面的获取的配置文件，如果不存在就创建一个，然后写入下面的内容

<details open="open">

<summary>~/.config/qq-flags.conf</summary>

```
--enable-features=UseOzonePlatform
--ozone-platform-hint=wayland
--enable-wayland-ime
```

</details>

### 三、重启应用
重启应用，问题得到解决

## 参考资料
[1] Fcitx. FAQ[EB/OL]. (2024-11-22)[2025-05-18]. [https://fcitx-im.org/wiki/FAQ](https://fcitx-im.org/wiki/FAQ)

[2] Fcitx. Using Fcitx 5 on Wayland[EB/OL]. (2025-03-04)[2025-05-18]. [https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland](https://fcitx-im.org/wiki/Using_Fcitx_5_on_Wayland)

[3] Fcitx. Input method related environment variables[EB/OL]. (2016-02-02)[2025-05-18]. [https://fcitx-im.org/wiki/Input_method_related_environment_variables](https://fcitx-im.org/wiki/Input_method_related_environment_variables)

[4] Lejia Chen. Fcitx input method doesn't work in JetBrains IDEs on Linux[Z/OL]. (2024-11-13)[2025-05-18]. [https://youtrack.jetbrains.com/articles/SUPPORT-A-718/Fcitx-input-method-doesnt-work-in-JetBrains-IDEs-on-Linux](https://www.flowerinsnow.cn/redirect?dest=https://youtrack.jetbrains.com/articles/SUPPORT-A-718/Fcitx-input-method-doesnt-work-in-JetBrains-IDEs-on-Linux)