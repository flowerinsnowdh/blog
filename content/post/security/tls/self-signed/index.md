+++
title = '自签名 TLS 证书'
date = 2024-11-14T13:39:22+08:00
draft = false
categories = [
    '信息安全/TLS',
    'OpenSSL',
]
tags = [
    '信息安全',
    'TLS',
    'SSL',
    'OpenSSL',
]
+++

## 版权与声明
本文部分技术核心取自以下文章，感谢这些文章的作者

1. [欧文斯](https://vircloud.net/author/1/) 在 [VirCloud](https://vircloud.net/) 上发的文章 [正确使用 OpenSSL 自签发代码|邮件|域名|IP 证书（附免费可信任 IP 证书申请） - VirCloud's Blog - Learning&Sharing](https://vircloud.net/operations/sign-ip-crt.html)
2. [个人开发记录](https://www.getce.cn/)上的文章：[个人开发记录 -- openssl生成代码签名证书(中间证书篇)](https://www.getce.cn/show/172.html)

如果可能，请尽量阅读上方的原文章来学习

### 推荐
强烈推荐使用 ECC 代替 RSA，ECC 现在的兼容度已经，它的长度比 RSA 低、运算比 RSA 快、安全性比 RSA 高，完全可以是 RSA 的上位替代

### 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行

以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

## 介绍
签发SSL证书是几乎零成本的，但是想让浏览器等程序信任该证书是很难的，自签证书不被浏览器信任，会有警告，甚至可能阻止访问

### 特点
- 免费
- 随时签发：无需审核，自己可以随时签发证书
- 与正规证书颁发机构签发的证书使用相同的加密方式
- 超长有效期：想签多久就签多久

## 一、创建 CA 私钥和证书
### 1.1. 创建私钥
二选一

#### ECC 私钥
<details open="open">
<summary>$ bash</summary>

```shell
openssl ecparam -genkey -name prime256v1 -noout -out ca.key
```
</details>

#### RSA 私钥
<details open="open">
<summary>$ bash</summary>

```shell
openssl genrsa -out ca.key 4096
```
</details>

### 1.2. 创建 CA 证书
<details open="open">
<summary>$ bash</summary>

```shell
openssl req -new -x509 -key ca.key -out ca.crt -days 7300
```
</details>

* 默认哈希算法使用sha256，若想要别的算法可以加上指定参数。例如 sha384 就加上`-sha384`
* 上方命令签发了有效期20年的证书，如有其他需要可以将`-days`后面的数字改为需要的天数

若想让命令不询问任何内容，可以直接使用`-subj`指定内容

<details open="open">
<summary>$ bash</summary>

```shell
openssl req -new -x509 -key ca.key -out ca.crt -days 7300 -subj "/C=AU/ST=full name/L=city/O=company/OU=section/CN=server FQDN or YOUR name"
```
</details>

### 1.3. 确认文件
刚刚的操作完成后，会存在以下文件

- `ca.key` <span style="color:red">该文件请确保绝对的私密，确保天底下只有你拥有</span>，否则别人可以拿你的证书来签名
- `ca.crt` 这是 CA 根证书，可以拿去给客户端安装，安装后客户端即信任该证书颁发机构

## 二、申请证书
以下为`example.net`签发证书，需要您将变量替换，例如 DNS
### 2.1. 创建私钥
二选一

#### ECC 私钥
<details open="open">
<summary>$ bash</summary>

```shell
openssl ecparam -genkey -name prime256v1 -noout -out example.net.key
```
</details>

#### RSA 私钥
<details open="open">
<summary>$ bash</summary>

```shell
openssl genrsa -out example.net.key 4096
```
</details>

以上生成了一个 4096 位的 RSA 密码

### 2.2. 创建证书申请文件
<details open="open">
<summary>$ bash</summary>

```shell
openssl req -new -key example.net.key -out example.net.csr
```
</details>

同样，如果不想要被询问，而是直接写在行内，可以使用`-subj`参数

<details open="open">
<summary>$ bash</summary>

```shell
openssl req -new -key example.net.key -out example.net.csr -subj "/C=CN/ST=Jiangsu/L=Suzhou/O=Example company/OU=Example unit/CN=Example"
```
</details>

甚至很多内容根本不需要填写，例如一个 DV 证书可以直接这么写

<details open="open">
<summary>$ bash</summary>

```shell
openssl req -new -key example.net.key -out example.net.csr -subj "/CN=example.net"
```
</details>

### 2.3. 确认文件
刚刚的操作完成后，会存在以下文件

- `example.net.key` <span style="color:red">该文件请确保绝对的私密，确保天底下只有你拥有</span>，连 CA 都不要递交，别被骗了，CA 不需要私钥就可以进行签名，否则别人可以通过该私钥解密 SSL/TLS 信息
- `example.net.csr` 该文件包含申请证书的信息和公钥，将该文件提交给 CA 让 CA 签名

## 三、签发证书
现在浏览器增加了对证书的检查，必须是 SAN 证书才能被信任，你必须添加一个**可选名称**（或者叫**备用名称**），它应该被声明为附加信息，否则你的证书仍然会被提示**不安全**

### 3.1. 创建 ext 文件
<details open="open">
<summary>example.net.ext</summary>

```plain
keyUsage=nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage=serverAuth, clientAuth
subjectAltName=@SubjectAlternativeName

[SubjectAlternativeName]
DNS.1=example.net
DNS.2=*.example.net
```
</details>

这样就可以为 `example.net` 和 `*.example.net` 签发证书

* 必须添加至少一个DNS，如果有多个，可以以此类推：`DNS.2`、`DNS.3`...
* 假如你用的是**数字IP**，应该把`DNS.1`换成`IP.1`

### 3.2. 签发证书
<details open="open">
<summary>$ bash</summary>

```shell
openssl x509 -req -days 90 -in example.net.csr -out example.net.crt -extfile example.net.ext -CA ca.crt -CAkey ca.key -CAcreateserial
```
</details>

* 有效期为90天，可换成任意天数
* 默认签名哈希算法使用sha256，若想要别的算法可以加上指定参数。例如sha384就加上`-sha384`

### 3.3. 确认文件
刚刚的操作完成后，会存在以下文件

- `example.net.crt` 该文件就是 TLS/SSL 证书，部署到服务器后即可开启 HTTPS
- `example.net.key` 该文件就是私钥，部署到服务器后即可开启 HTTPS

其他文件（除了 CA 证书和私钥外）都可以删了

## 安装根证书
由于自签证书不被信任，要想信任它，需要安装 CA 证书

这里的 CA 证书就是上方生成的`ca.crt`

注意`ca.key`不能给别人，别被骗了，只需要`ca.crt`就可以安装了

* 不要随意安装别人给你的 CA 证书

### Windows
#### 安装
1. 双击打开`.crt`文件
2. 单击`安装证书(I)...`
3. 选择`当前用户(C)`或`本地计算机(L)`单选框，前者是为当前用户安装，后者是为所有用户安装
4. 单击`浏览(R)`
5. 在列表中选择`受信任的根证书颁发机构`
6. 单击`确定`
7. 单击`下一页(N)`
8. 单击`完成(F)`

#### 卸载
使用快捷键`Windows+R`打开运行窗口，输入`mmc`打开管理控制台

单击工具栏中的`文件(F)`，单击`打开(O)`，选择其一

1. 本地计算机：`C:\Windows\System32\certlm.msc`
2. 当前用户：`C:\Windows\System32\certmgr.msc`

打开`受信任的根证书颁发机构`路径，找到安装的证书，右击，单击`删除(D)`，单击`是(Y)`

### Linux
#### 安装
将`.crt`文件复制到`/usr/local/share/ca-certificates/`目录下，然后执行命令

<details open="open">
<summary># bash</summary>

```shell
update-ca-certificates
```
</details>

#### 卸载
将`.crt`从`/usr/local/share/ca-certificates/`目录下删除，然后执行命令

<details open="open">
<summary># bash</summary>

```shell
update-ca-certificates
```
</details>

### Mac
不知道，别问我（生气）
