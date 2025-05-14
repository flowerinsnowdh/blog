+++
title = 'Debian 上双显卡亮度调节问题'
description = 'Debian 在有双显卡的情况下，无法调节屏幕亮度'
date = 2024-11-03T13:13:48+08:00
lastmod = 2025-05-14T14:15:42+08:00
draft = false
categories = [
    'GNU-Linux/Debian/Debian12'
]
tags = [
    'GNU-Linux',
    'Debian',
    'Debian 12',
    '驱动安装',
    '问题解决经验',
]
+++

问题：即使显卡驱动安装完成，也无法调节电脑的亮度（亮度滑块无效）

## 注意事项
### Debian 12
本文章是基于 Debian 12 的，如果您是其他操作系统，可能无法通过此文解决

### 危险
<p style="color:red">修改系统关键设置有风险，请先备份好您的密钥以及重要文件，根据此文章执行产生的一切后果由您亲自承担</p>

## 一、确认问题是否一致

<details open="open">

<summary>bash</summary>

```shell
ls /sys/class/backlight/
```

</details>

如果你和我一样，显示了 Nvidia 的亮度调节

<details open="open">

<summary>console</summary>

```console
nvidia_wmi_ec_backlight
```

</details>

那么恭喜你，请按照下面的方法解决

如果不是，这篇文章可能无法帮助你

## 二、修改亮度调节显卡
一般来说问题就出在，显示桌面环境的显卡和调节亮度的显卡不一致，所以我们要把调节亮度的显卡改为核显

用你自己的方式编辑文件`/etc/default/grub`

找到`GRUB_CMDLINE_LINUX`这一行

<details open="open">

<summary>/etc/default/grub</summary>

```plain
GRUB_CMDLINE_LINUX=""
```

</details>

在后面追加一段内容`acpi_backlight=native`

我的这一项是空值，所以只要添加这一段内容既可，效果如下

<details open="open">

<summary>/etc/default/grub</summary>

```plain
GRUB_CMDLINE_LINUX="acpi_backlight=native"
```

</details>

如果你的这段内容本身有`quiet`等内容，请在原本的内容后追加，例如

<details open="open">

<summary>/etc/default/grub</summary>

```plain
GRUB_CMDLINE_LINUX="quiet acpi_backlight=native"
```

</details>

## 三、更新 grub

<details open="open">

<summary>bash</summary>

```shell
sudo update-grub
```

</details>

## 四、重启
重启计算机，问题应该就解决了

## 参考资料
[1] 啵啵粆. Linux双显卡亮度调节失效解决办法[Z/OL]. (2023-07-07)[2024-11-03]. [https://www.bilibili.com/read/cv24842015/](https://www.flowerinsnow.cn/redirect?dest=https://www.bilibili.com/read/cv24842015/)
