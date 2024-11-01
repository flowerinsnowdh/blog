+++
title = 'GNU/Linux 下压缩操作'
date = 2024-11-01T12:19:42+08:00
draft = false
categories = [
    'GNU-Linux',
]
tags = [
    'GNU-Linux'
]
+++
## 常见的压缩类型有
- .zip
- .gz
- .xz
- .7z
- .rar

由于 .rar 格式是专有软件，不适合在 Linux 上运行，所以不讲

## 注意事项
### 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行
以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

## zip
.zip 文件格式是一种数据压缩和文档储存的文件格式

优势：压缩效率、兼容性

几乎每一个操作系统都能完美地支持zip类型

您可能需要安装相关程序包

<details open="open">

<summary># bash</summary>

```shell
apt install zip unzip
```

</details>

### 创建压缩文件

<details open="open">

<summary>$ bash</summary>

```shell
zip <压缩文件>.zip <文件1> [文件2] [文件3]...
```

</details>

注：如果要压缩的文件中含有目录，需要添加`-r`标记

### 解压缩文件
<details open="open">

<summary>$ bash</summary>

```shell
unzip <压缩文件>.zip
```

</details>

## Tape archiver
tar 是 Unix 和类 Unix 操作系统上的归档打包格式，可以将多个文件合并为一个文件，通常在 Unix 和类 Unix 操作系统上自带

tar 准确来说不是一种压缩格式，它只是一个归档，可以将很多文件打包成一个文件，但不会进行压缩，通常配合 gz、bz、xz 使用

### 创建归档文件
<details open="open">

<summary>$ bash</summary>

```shell
tar -cvf <归档文件>.tar <文件1> [文件2] [文件3]...
```

</details>

其中

- `-c` 标记代表创建归档文件
- `-v` 标记代表详细模式，会输出打包信息，如果不需要可以删掉
- `-f` 标记代表指定文件，如果不加则会在**标准输入流**中读取内容

### 解包归档文件
<details open="open">

<summary>$ bash</summary>

```shell
tar -xvf <归档文件>.tar
```

</details>

同样的

- `-x` 标记代表解包归档文件
- `-v` 标记代表详细模式，会输出解包信息，如果不需要可以删掉
- `-f` 标记代表指定文件，如果不加则会在**标准输入流**中读取内容

## GNU Zip
gz 格式是 GNU 计划实现的压缩单个文件的压缩格式，通常在 Unix 和类 Unix 操作系统上自带

gz 格式只能压缩一个文件，所以通常配合 tar 使用

### 注意
gz 命令压缩或解压后会将原本的文件删掉

### 创建压缩文件
<details open="open">

<summary>$ bash</summary>

```shell
gzip <文件>
```

</details>


### 解压文件
<details open="open">

<summary>$ bash</summary>

```shell
gzip -d <压缩文件>.gz
```

</details>

### 配合 tar 使用
只需要在 tar 命令的基础上添加 `-z` 标记
#### 创建压缩文件
<details open="open">

<summary>$ bash</summary>

```shell
tar -zcvf <压缩文件>.tar.gz <文件1> [文件2] [文件3]...
```

</details>


#### 解压文件
<details open="open">

<summary>$ bash</summary>

```shell
tar -zxvf <压缩文件>.tar.gz
```

</details>

## 7-zip
优势：压缩率

需要安装第三方库 p7zip-full

<details open="open">

<summary># bash</summary>

```shell
apt install p7zip-full
```

</details>

### 创建压缩文件
<details open="open">

<summary>$ bash</summary>

```shell
7z a <压缩文件>.7z <文件1> [文件2] [文件3]...
```

</details>

### 解压文件
<details open="open">

<summary>$ bash</summary>

```shell
7z x <压缩文件>.7z
```

</details>

## bzip2
与 gzip、xz 一样，同样支持多文件压缩，但是约定不能将多于一个的目标文件压缩进同一个归档文件
### 创建压缩文件
<details open="open">

<summary>$ bash</summary>

```shell
bzip2 <文件>
```

</details>

### 解压文件
<details open="open">

<summary>$ bash</summary>

```shell
bzip2 -d <压缩文件>.bz2
```

</details>

### 配合 tar 使用
只需要在 tar 命令的基础上添加 `-j` 标记
#### 创建压缩文件
<details open="open">

<summary>$ bash</summary>

```shell
tar -jcvf <压缩文件>.tar.bz2 <文件1> [文件2] [文件3]...
```

</details>


#### 解压文件
<details open="open">

<summary>$ bash</summary>

```shell
tar -jxvf <压缩文件>.tar.bz2
```

</details>

## xz
和 gzip 与 bzip2 一样，同样支持多文件压缩，但是约定不能将多于一个的目标文件压缩进同一个归档文件

### 注意
xz 命令压缩或解压后会将原本的文件删掉

### 创建压缩文件
<details open="open">

<summary>$ bash</summary>

```shell
xzip <文件>
```

</details>


### 解压文件
<details open="open">

<summary>$ bash</summary>

```shell
xzip -d <压缩文件>.xz
```

</details>

### 配合 tar 使用
只需要在 tar 命令的基础上添加 `-J` 标记
#### 创建压缩文件
<details open="open">

<summary>$ bash</summary>

```shell
tar -Jcvf <压缩文件>.tar.xz <文件1> [文件2] [文件3]...
```

</details>


#### 解压文件
<details open="open">

<summary>$ bash</summary>

```shell
tar -Jxvf <压缩文件>.tar.xz
```

</details>