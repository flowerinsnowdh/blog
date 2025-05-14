+++
title = 'GNU/Linux 手动安装字体'
date = 2025-05-14T14:16:20+08:00
draft = false
categories = [
    'GNU-Linux'
]
tags = [
    'GNU-Linux',
]
+++

## 一、安装字体到指定目录
### 1) 要为单个用户安装
安装到 `~/.local/share/fonts/`

可能需要先创建目录

<details open="open">

<summary>bash</summary>

```shell
mkdir ~/.local/share/fonts/
```

</details>

### 2) 要为系统（所有用户）安装
安装到 `/usr/local/share/fonts/`

> 是否要创建子目录由用户决定，各发行版也有不同。为清晰起见，可将每个字体族放在单独的目录里。Fontconfig 将递归搜索默认路径，确保可以发现子目录下的文件。
> 以下是一个目录结构示例： 
> 请确保所有用户都有读取字体文件的权限，即至少将文件 chmod 为 444，目录为 555。 

```plain
 /usr/local/share/fonts/
 ├── otf
 │   └── SourceCodeVariable
 │       ├── SourceCodeVariable-Italic.otf
 │       └── SourceCodeVariable-Roman.otf
 └── ttf
     ├── AnonymousPro
     │   ├── Anonymous-Pro-B.ttf
     │   ├── Anonymous-Pro-I.ttf
     │   └── Anonymous-Pro.ttf
     └── CascadiaCode
         ├── CascadiaCode-Bold.ttf
         ├── CascadiaCode-Light.ttf
         └── CascadiaCode-Regular.ttf
```

## 二、更新 Fontconfig 的缓存

<details open="open">

<summary>bash</summary>

```shell
fc-cache
```

</details>

## 参考资料
[1] Arch Linux. 字体[EB/OL]. (2025-05-02)[2025-05-14]. [https://wiki.archlinuxcn.org/wiki/字体](https://wiki.archlinuxcn.org/wiki/%E5%AD%97%E4%BD%93)
