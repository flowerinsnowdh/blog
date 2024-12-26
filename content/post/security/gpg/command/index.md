+++
title = 'GPG 命令行操作'
date = 2024-12-26T11:26:11+08:00
draft = false
categories = [
    '信息安全/GnuPG',
]
tags = [
    '信息安全',
    'PGP',
    'GPG',
    'GnuPG',
    'OpenPGP',
]
+++

# 简介
## OpenPGP
[OpenPGP](https://www.openpgp.org/) 是一个由 <a href="https://www.ietf.org/"><span title="互联网工程任务组">IETF</span></a> 制定的一个标准，定义了用于加密、解密、签名和密钥管理的规范。截至 24/12/26，它的最新标准的标准号为 [RFC9580](https://www.rfc-editor.org/rfc/rfc9580.html)

## GnuPG
[GnuPG](https://www.gnupg.org/) 是 [RFC4880](https://www.rfc-editor.org/rfc/rfc4880.html) 定义的 OpenPGP 标准的完整、自由、免费实现。GnuPG 允许你对数据和通信进行加密核签名；它拥有一个多功能密钥管理系统，以及各种公共密钥目录的访问模块

GnuPG 简称 GPG，是一种命令行工具，具有与其他应用程序轻松集成的功能。有大量的前端应用程序和库可供使用。

# 注意
## 参考
本文部分技术核心取自以下文章，感谢这些文章的作者

1. [GnuPG](https://www.gnupg.org/) 的[手册](https://www.gnupg.org/documentation/manuals/gnupg/)

如果可能，请尽量阅读上方的原文来学习

## 下文注释
在下文代码块中，左上角写着`#`的为需要超级用户身份执行的权限；左上角写着`$`的只需要普通用户既可执行

以下代码只使用`#`和`$`来表示权限等级，不再明文写`sudo`

## 私钥的保密
<span style="color: red">所有的私钥、私钥备份一般只有个人或企业拥有，一切向你所有私钥的人**都是诈骗**</span>

<span style="color: red">你应该确保私钥只有全天下只有你拥有</span>

<span style="color: red">如果私钥有已被泄露的可能性，请及时吊销密钥</span>

## 私钥的重要性
续签、吊销密钥都必须拥有私钥才能生成相应证书，所以请你<span style="color: red">妥善保管好私钥</span>

<span style="color: red">私钥应该及时备份，并加密保存或离线保存</span>

<span style="color: red">如果你失去私钥，你将永远无法续签、吊销以及未来进行签名</span>

## 有效期
生成私钥时，建议将有效期设置短一些，因为它是可以续签的，为避免夜长梦多，建议将有效期设置短一些，以免遭遇不测

## 可信与抵赖
虽然数字签名的签名时间可以由签名者伪造，内容也可以随意写。所以签名内容并不一定可信，<span style="color: red">但是一旦签名者将内容发布出去，他将永远**无法抵赖**他曾经签名过这段内容</span>

这也就是为什么数字签名可以具有与纸质签名同等法律效力的原因，它更安全，在需要更高可信度的情况下

## 命令行
<span style="color: red">在任何时候，你都不应该把口令、密钥等敏感信息直接输入在命令上，包括 echo</span>

类似这样

<details open="open">

<summary>$ bash</summary>

```shell
echo '-----BEGIN PGP PRIVATE KEY BLOCK-----

lFgEZ20g+RYJKwYBBAHaRw8BAQdAJLAcrQ6ur3MP8GzRnKiY5MgrWIMXkBlUY0Kd
xr+p1FoAAQDp2EuDDq7C9vhGetLozdrKvFBe8MrpB32VhOGL4X9iTBQLtB1leGFt
cGxlIDxleGFtcGxlQGV4YW1wbGUubmV0PoiZBBMWCgBBFiEEBgWMZOeTa25Px4Kr
gRsnziPzz4gFAmdtIPkCGwMFCQABBUcFCwkIBwICIgIGFQoJCAsCBBYCAwECHgcC
F4AACgkQgRsnziPzz4hyZQD/SGJ6rmri0C1GbUWXpdDAi246z/aI+b7XJHz7YzZf
mYEBAIxwVyA3vm9GtNgI3bmCuSCDuJLoOlqmxiraE5Q56IoJnF0EZ20g+RIKKwYB
BAGXVQEFAQEHQGmBKiRDu5E2b4+EdnqUHAHeakV9X+VaganOE1Z5lqtqAwEIBwAA
/1n96Wz4iI5pn3sFO83isA5KJ6e+D347G73kXu2CBkrQES+IfgQYFgoAJhYhBAYF
jGTnk2tuT8eCq4EbJ84j88+IBQJnbSD5AhsMBQkAAQVHAAoJEIEbJ84j88+IROAA
/1EP80PwDBcAFATyYyrmq7E2zyM5/HTKQuMml1cSOMIVAQDkbLpAVXCY7psH6uXw
vYB985To6u86W9Yp0mWH35ohCg==
=wUWt
-----END PGP PRIVATE KEY BLOCK-----' | gpg --import
```

</details>

### 为什么？
1. 你在 bash 上执行的内容，会被记录，可以在 `history` 命令中查看，也可以在 `~/.bash_history` 上查看
2. 你执行的命令语句在运行中可以在 `ps`、`htop` 等进程查看器中看到

### 怎么办
应该尽可能从文件读取，或使用标准输入流输入

例如，执行单独一条 `gpg --import` 或 `gpg -c` 等命令后，gpg 就会持续等待你将内容输入在控制台（作为标准输入流）上，期间可以换行，直到读取到 EOF 信号为止

等待你粘贴或输入完信息后，向标准输入流发送一个 EOF 信号即可结束，在 Linux 的 Shell 中，它是 Ctrl+D 快捷键

# 安装 GnuPG
正常情况下，GnuPG 会默认安装在 GNU/Linux 操作系统上，因为它们的软件包管理器通常都会校验签名，没有 GnuPG 作为接口的话，是无法工作的。如果没有安装，可以尝试使用包管理器安装，例如 `apt install gnupg`

在 Windows 下，也有类似 [Gpg4win](https://gpg4win.org/) 的支持，Gpg4win 会同时安装一个图形页面程序，可以让你方便地管理和操作各种内容（例如密钥创建、删除、吊销、加密、解密、签名、验证等）

# 密码算法
以下是 GnuPG 截至 2.2.40 支持的密码算法

## 非对称密码
<table>
    <tr>
        <th>算法</th>
        <th>加密</th>
        <th>数字签名</th>
        <th>密钥交换</th>
        <th>基于</th>
        <th>问题</th>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/RSA_(cryptosystem)"><span title="Rivest–Shamir–Adleman">RSA</span></a></td>
        <td>√</td>
        <td>√</td>
        <td>√</td>
        <td>大整数的因数分解</td>
        <td><span style="color: orangered">效率低</span>。应至少使用 2048 位确保安全</td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/ElGamal_encryption"><span title="ElGamal">ELG</span></a></td>
        <td>√</td>
        <td>√</td>
        <td>√</td>
        <td>离散对数</td>
        <td><span style="color: orangered">效率低</span></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Digital_Signature_Algorithm"><span title="Digital Signature Algorithm">DSA</span></a></td>
        <td></td>
        <td>√</td>
        <td></td>
        <td>离散对数</td>
        <td><span style="color: red">不安全</span>、<span style="color: orangered">效率低</span></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Elliptic-curve_Diffie%E2%80%93Hellman"><span title="Elliptic Curve Diffie-Hellman">ECDH</span></a></td>
        <td></td>
        <td></td>
        <td>√</td>
        <td>椭圆曲线离散对数</td>
        <td></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Elliptic_Curve_Digital_Signature_Algorithm"><span title="Elliptic Curve Digital Signature Algorithm">ECDSA</span></a></td>
        <td></td>
        <td>√</td>
        <td></td>
        <td>椭圆曲线离散对数</td>
        <td></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/EdDSA"><span title="Edwards-curve Digital Signature Algorithm">EDDSA</span></a></td>
        <td></td>
        <td>√</td>
        <td></td>
        <td>爱德华曲线离散对数</td>
        <td></td>
    </tr>
</table>

## 对称密码
<table>
    <tr>
        <th>算法</th>
        <th>基于</th>
        <th>问题</th>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/International_Data_Encryption_Algorithm"><span title="International Data Encryption Algorithm">IDEA</span></a></td>
        <td>线性代数</td>
        <td><span style="color: orangered">效率低</span></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Triple_DES"><span title="Triple Data Encryption Algorithm">3DES</span></a></td>
        <td>3 次 DES 运算</td>
        <td><span style="color: orangered">效率低</span></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/CAST-128"><span title="CAST-128">CAST5</span></a></td>
        <td>Feistel网络</td>
        <td><span style="color: orangered">效率适中</span></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Blowfish_(cipher)"><span title="Blowfish">BLOWFISH</span></a></td>
        <td>Feistel网络</td>
        <td><span style="color: red">不安全</span></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Advanced_Encryption_Standard"><span title="Advanced Encryption Standard">AES</span></a></td>
        <td rowspan="3">有限域上的代数运算</td>
        <td rowspan="3"></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Advanced_Encryption_Standard"><span title="Advanced Encryption Standard">AES192</span></a></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Advanced_Encryption_Standard"><span title="Advanced Encryption Standard">AES256</span></a></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Twofish"><span title="Twofish">TWOFISH</span></a></td>
        <td>Feistel网络</td>
        <td></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Twofish"><span title="Camellia">CAMELLIA128</span></a></td>
        <td rowspan="3">Feistel网络</td>
        <td rowspan="3"></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Twofish"><span title="Camellia">CAMELLIA192</span></a></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/Twofish"><span title="Camellia">CAMELLIA256</span></a></td>
    </tr>
</table>

## 散列
<table>
    <tr>
        <th>算法</th>
        <th>长度</th>
        <th>问题</th>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/SHA-1"><span title="Secure Hash Algorithm 1">SHA1</span></a></td>
        <td>160</td>
        <td><span style="color: red">不安全</span></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/RIPEMD"><span title="RIPEMD">RIPEMD160</span></a></td>
        <td>160</td>
        <td></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/SHA-2"><span title="SHA-2">SHA256</span></a></td>
        <td>256</td>
        <td></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/SHA-2"><span title="SHA-2">SHA384</span></a></td>
        <td>384</td>
        <td></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/SHA-2"><span title="SHA-2">SHA512</span></a></td>
        <td>512</td>
        <td></td>
    </tr>
    <tr>
        <td><a href="https://en.wikipedia.org/wiki/SHA-2"><span title="SHA-2">SHA224</span></a></td>
        <td>224</td>
        <td></td>
    </tr>
</table>

# 帮助文档
使用 `--help` 标记，可以列出 gpg 常用选项帮助文档

* 可以简写为 `-h`

# 查看版本
使用 `--version` 标记，可以查看当前 gpg 版本

# 列出所有选项
使用 `--dump-options` 标记，可以列出 gpg 所有可用的选项

# 列出所有公钥
使用 `--list-public-keys` 标记，可以列出当前存储的所有公钥

* 可以简写为 `-k`/`--list-keys`

# 列出所有私钥
使用 `--list-secret-keys` 标记，可以列出当前存储的所有私钥

* 可以简写为 `-K`

# 生成新的非对称密钥对
使用 `--generate-key` 标记，可以通过当前参数随机生成一个新的非对称密钥对

* 参数位于 `openpgp-revocs.d` 目录下
* 可以简写为 `--gen-key`

# 生成新的非对称密钥对：向导模式
使用 `--full-generate-key` 标记，可以随机生成一个新的非对称密钥对，并使用向导模式

* 可以简写为 `--full-gen-key`

# 导出公钥
使用 `--export` 标记，可以导出公钥

* 可以在后面指定多个密钥名，若不指定密钥名，默认会导出所有的公钥

# 备份私钥
使用 `--export-secret-subkeys` 标记，可以备份私钥

* 可以简写为 `--export-secret-keys`
* 可以在后面指定多个密钥名，若不指定密钥名，默认会导出所有的私钥
* <span></span>

# 导入密钥
使用 `--import` 标记，可以导入密钥，或更新密钥的状态（例如续签、吊销等）

* 可以写为 `--fast-import`

# 删除公钥
使用 `--delete-keys` 标记，可以删除公钥

* 可以在后面指定多个密钥名
* <span style="color: red">注意：该操作不会吊销密钥，如果你已经把公钥公布出去，此举不会吊销你的密钥，它仍然在互联网、密钥服务器和保存过你公钥的电脑上存在</span>
* 如果想要**吊销密钥**，请参阅 [吊销密钥](#生成吊销密钥证书)

# 删除私钥
使用 `--delete-secret-keys` 标记，可以删除公钥

* 可以在后面指定多个密钥名
* <span style="color: red">注意：该操作不会吊销密钥，如果你已经把公钥公布出去，此举不会吊销你的密钥，它仍然在互联网、密钥服务器和保存过你公钥的电脑上存在</span>
* 如果想要**吊销密钥**，请参阅 [吊销密钥](#生成吊销密钥证书)
* <span style="color: red">注意：该操作不可撤销，请确认你已经完全不需要该密钥，或已经做好备份的情况下再继续。如果它已被公布，你在失去私钥后永远也无法吊销它</span>

# 生成吊销密钥证书
使用 `--delete-keys` 标记，可以删除公钥

* <span style="color: red">注意：该操作不会吊销密钥，如果你已经把公钥公布出去，此举不会吊销你的密钥，它仍然在互联网、密钥服务器和保存过你公钥的电脑上存在</span>
* 如果想要**吊销密钥**，请将生成出来的证书公布到互联网、密钥服务器和保存过你公钥的电脑那里，让他们使用 [导入密钥](#导入密钥) 来更新该公钥

# 删除私钥
使用 `--export-secret-subkeys` 标记，可以导出私钥

* 可以简写为 `--export-secret-keys`
* 可以在后面指定多个密钥名，若不指定密钥名，默认会导出所有的私钥

# 对称加密
使用 `--symmetric` 标记，可以使用口令对称加密数据

当你运行命令后，软件会向你询问口令

* 可以简写为 `-c`
* 可以在后面指定一个文件，若不指定文件，默认从标准输入流中读取
* 可以配合 [额外标记/装甲](#装甲) 创建 ASCII 字符封装的输出。但该标记必须写在语句的前方。若不使用，默认会输出二进制数据
* 可以配合 [额外标记/输出文件](#输出文件)，以将输出写到文件。但该标记必须写在语句的前方。若不使用，默认会在相同位置生成一个 `.gpg` 文件（或在同时签名时生成 `.sig` 文件，或在使用 armor 标记时生成 `asc` 文件）
* 可以配合 `--cipher-algo` 后面加一个算法，以指定加密算法。若不使用，默认使用 AES256
* 可以配合 [非对称加密](#非对称加密)
* 可以配合 [签名](#签名)
* 可以配合 [分离式签名](#分离式签名)，以生成一个加密的签名
* 默认情况下，gpg 会缓存对称加密的口令，因此解密操作可能不需要用户输入口令。可以配合 `--no-symkey-cache` 禁用这一功能。

<details>

<summary>你知道吗</summary>

* gpg 会使用[密钥派生函数](https://zh.wikipedia.org/zh-hans/%E5%AF%86%E9%92%A5%E6%B4%BE%E7%94%9F%E5%87%BD%E6%95%B0) 将输入的口令转换为固定长度的密钥
* gpg 会将密钥派生函数明文地写在密文头部，以方便用户解密文件时相同的密钥派生函数获取相同的密钥而不是让用户手动指定
* gpg 会在加密时使用随机生成的[盐](https://zh.wikipedia.org/wiki/%E7%9B%90_(%E5%AF%86%E7%A0%81%E5%AD%A6))（64 位）来提高安全性
* gpg 会将加密时使用的随机生成的盐明文地写在密文头部
* gpg 会将**加密前文件的文件名**、**加密时间**、**接收者的邮箱及名称**写在头部或尾部，但请放心，它们是加密存储的，没有口令无法查看。若是从标准输入流读取的，则文件名会是一个空字符串。不过注意，它们都是可以伪造的

</details>

# 非对称加密
使用 `--encrypt` 标记，可以使用公钥加密数据

* 可以简写为 `-e`
* 可以在后面指定一个文件，若不指定文件，默认从标准输入流中读取
* 可以使用 `--recipient`/`-r` 指定收件人。。但该标记必须写在语句的前方。若不使用，程序会向你询问收件人
* 可以配合 [额外标记/装甲](#装甲) 创建 ASCII 字符封装的输出。但该标记必须写在语句的前方。若不使用，默认会输出二进制数据
* 可以配合 [额外标记/输出文件](#输出文件)，以将输出写到文件。但该标记必须写在语句的前方。若不使用，默认会在相同位置生成一个 `.gpg` 文件（或在同时签名时生成 `.sig` 文件，或在使用 armor 标记时生成 `asc` 文件）
* 可以配合 [对称加密](#对称加密)
* 可以配合 [签名](#签名)
* 可以配合 [分离式签名](#分离式签名)，以生成一个加密的签名

<details>

<summary>你知道吗</summary>

椭圆曲线密码**只能用于签名与密钥交换**，为什么我能为一个椭圆曲线公钥加密文件？

要明白这一点，你需要知道[迪菲-赫尔曼密钥交换](https://zh.wikipedia.org/wiki/%E8%BF%AA%E8%8F%B2-%E8%B5%AB%E7%88%BE%E6%9B%BC%E5%AF%86%E9%91%B0%E4%BA%A4%E6%8F%9B)（简称 DH）是怎样工作的，请自行了解

当你为一个椭圆曲线公钥加密文件时，gpg 会随机生成一个[椭圆曲线迪菲-赫尔曼密钥交换](https://zh.wikipedia.org/wiki/%E6%A9%A2%E5%9C%93%E6%9B%B2%E7%B7%9A%E8%BF%AA%E8%8F%B2-%E8%B5%AB%E7%88%BE%E6%9B%BC%E9%87%91%E9%91%B0%E4%BA%A4%E6%8F%9B)（简称 ECDH）算法的密钥对（称为**临时密钥对**），它是椭圆曲线版的 DH 密钥交换算法，具有更高效、安全的算法和更短的密钥

gpg 此时拥有**临时密钥对**和**收件人的公钥**

也就是说，gpg 可以使用**临时私钥**和**收件人的公钥**合成[共享密钥](https://zh.wikipedia.org/wiki/%E5%85%B1%E4%BA%AB%E5%AF%86%E9%91%B0)

这个**共享密钥**就是一个**对称密钥**，用于接下来的加密

gpg 会将**临时公钥**明文地放在消息头部，并将加密的数据和一些其他数据放在后面

当收件人收到数据后，它会结合**临时公钥**和**自己的私钥**合成一个相同**共享密钥**，并解密剩下的内容

由于合成共享安全密钥必须拥有一方的公钥和另一方的私钥，而非发送者既不知道临时私钥，也不知道收件人的私钥，所以无法解密信息

这也就是为什么每次非对称加密的结果都不相同，甚至天翻地覆的原因，因为每次加密都会完全随机地生成临时密钥对

也许这就是密钥交换的魅力所在吧

但是请注意，虽然它是密钥交换的结果，但它不像 TLS、SSH 那样，它<span style="color: red">并不向前保密</span>，因为 TLS 与 SSH 双方都使用临时密钥对，而 gpg 加密数据只有一方使用临时密钥对，<span color="red">所以一旦接收者的密钥泄露，以前被保存下来的内容都能被轻松解密</span>

与对称加密相同，非对称加密时 gpg 同样会将**加密前文件的文件名**、**加密时间**、**收件人的邮箱及名称**写在头部或尾部，但请放心，它们是加密存储的，没有口令无法查看。若是从标准输入流读取的，则文件名会是一个空字符串。不过注意，它们都是可以伪造的

</details>

# 签名
使用 `--sign` 标记，可以对数据进行签名

* 可以简写为 `-s`
* 此种签名方式包含整个文件，也就是说它会生成一个新的文件，包含原文件和签名信息。这个文件是 gpg 的格式，所以无法直接运行，必须先 [删除签名](#解密) 后才能使用
* 可以在后面指定一个文件，若不指定文件，默认从标准输入流中读取
* 可以配合 [额外标记/装甲](#装甲) 创建 ASCII 字符封装的输出。但该标记必须写在语句的前方。若不使用，默认会输出二进制数据
* 可以配合 [额外标记/输出文件](#输出文件)，以将输出写到文件。但该标记必须写在语句的前方。若不使用，默认会在相同位置生成一个 `.gpg` 文件（或在使用 armor 标记时生成 `asc` 文件）
* 可以配合 [对称加密](#对称加密)
* 可以配合 [非对称加密](#非对称加密)

# 分离式签名
使用 `--detach-sign` 标记，可以对数据进行分离式签名

* 可以简写为 `-b`
* 与 [签名](#签名) 不同，分离式签名会产生一个额外的文件，这个文件不包含原文件，只包含签名信息。通常将其和原文件一起发布，这使原文件可以直接运行而不是必须 [删除签名](#解密) 后才能使用
* 可以在后面指定一个文件，若不指定文件，默认从标准输入流中读取
* 可以配合 [额外标记/装甲](#装甲) 创建 ASCII 字符封装的输出。但该标记必须写在语句的前方。若不使用，默认会输出二进制数据
* 可以配合 [额外标记/输出文件](#输出文件)，以将输出写到文件。但该标记必须写在语句的前方。若不使用，默认会在相同位置生成一个 `.sig` 文件（或在使用 armor 标记时生成 `asc` 文件）
* 可以配合 [对称加密](#对称加密)，以生成一个加密的签名文件
* 可以配合 [非对称加密](#非对称加密)，以生成一个加密的签名

# 验证签名
使用 `--verify` 标记，可以对签名进行验证

* 可以在后面指定 0~2 个文件。
* 如果该签名是一个含文件信息的签名，则后面只能指定 0~1 个文件。若不指定文件，默认从标准输入流中读取
* 如果该签名是一个分离式的签名，则后面只能指定 1~2 个文件。第 1 个参数必须是签名文件，第二个参数必须是源文件。如果只指定第 1 个参数，则 gpg 会在当前目录下寻找与签名文件同名的源文件（去掉 `.sig` 或 `.asc`），如果没有找到，程序会以错误的方式退出

# 解密
使用 `--decrypt` 标记，可以解密数据，或是删除文件上的签名使其恢复成原文件

* 可以简写为 `-d`
* 后面可以加一个文件，若不加文件，则默认从标准输入流中读取
* 可以配合 [额外标记/输出文件](#输出文件)，以将输出写到文件。但该标记必须写在语句的前方。若不使用，默认会输出到标准输出流

# 列出所有数据
使用 `--list-packets` 标记，可以列出 gpg 格式文件中的所有元数据（例如加密/签名时间、原文件名、密钥 ID、接收者等等）

* 后面可以加一个文件，若不加文件，则默认从标准输入流中读取
* 你需要能够 [解密](#解密) 这个文件作为前提才能看到被加密的元数据
* 可以配合 [额外标记/详细输出](#详细输出) 看到更详细的信息

# 额外标记
## 详细输出
使用 `--verbose` 标记，可以输出详细信息，以方便在出现问题时分析

* 可以简写为 `-v`

## 输出文件
使用 `--output` 标记，后面跟一个文件，可以将输出重定向到此文件

* 可以简写为 `-o`

## 装甲
使用 `--armor` 标记，可以创建 ASCII 字符封装的输出，以方便在无法使用不可打印字符时

* 可以简写为 `-a`

### 你知道吗
它使用头部 + [Base64](https://zh.wikipedia.org/wiki/Base64) + 尾部组成

# 日常使用
## 去中心化使用
如果你只是想与小伙伴建立加密通信，你可以与小伙伴们互相交换公钥，这样你就可以加密只有小伙伴的私钥才能解密的消息（包括你自己也无法解密它）。为了防止消息被篡改或防止抵赖，你可以为消息生成签名信息，全天下只有你能够生成这段签名信息

OpenPGP 为电子邮件而生，所以部分邮箱客户端拥有 OpenPGP 加密和签名功能，例如 [Thunderbird](https://www.thunderbird.net/)，用好它们，即使是邮箱服务器管理员也无法解密或伪造消息

## 商业化使用
更多时候，OpenPGP 是用于签名软件的。软件开发者（特别是自由软件开发者）通常会在分发软件时附上一份分离式签名文件，以供用户验证软件未被篡改，也供用户审查软件内容，一旦签名，签名者就无法抵赖自己签名过这些内容

如果你是软件用户，在遇到可验证签名的软件时，你可以先导入该开发者的公钥（通常位于他们的主页上），然后通过 GnuPG 来验证它，以确保它未被篡改

如果你是开发者，想要签名自己的软件，可以把你的公钥放在你的主页上，然后每次发布软件时带上你的签名
