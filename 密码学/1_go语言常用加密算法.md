## 常见的加密算法

+ Base64
+ MD5
+ AES
+ DES

## Base64

请参考这篇[文章](https://blog.csdn.net/wo541075754/article/details/81734770)

Base64是一种任意二进制到文本字符串的编码方法，常用于在URL、Cookie、网页中传输少量二进制数据。

首先使用Base64编码需要一个含有64个字符的表，这个表由大小写字母、数字、+和/组成。采用Base64编码处理数据时，会把每三个字节共24位作为一个处理单元，再分为四组，每组6位，查表后获得相应的字符即编码后的字符串。编码后的字符串长32位，这样，经Base64编码后，原字符串增长1/3。如果要编码的数据不是3的倍数，最后会剩下一到两个字节，Base64编码中会采用\x00在处理单元后补全，编码后的字符串最后会加上一到两个 = 表示补了几个字节。

```
package main

import (
	"encoding/base64"
	"fmt"
	"reflect"
	"unsafe"
)

var BASE64Table = "IJjkKLMNO567PQX12RVW3YZaDEFGbcdefghiABCHlSTUmnopqrxyz04stuvw89+/"

func Encode(data string) string {
	content := *(*[]byte)(unsafe.Pointer((*reflect.SliceHeader)(unsafe.Pointer(&data))))
	coder := base64.NewEncoding(BASE64Table)
	return coder.EncodeToString(content)
}

func Decode(data string) string {
	coder := base64.NewEncoding(BASE64Table)
	result, _ := coder.DecodeString(data)
	//return *(*string)(unsafe.Pointer(&result))
	return string(result)
}

func main() {
	strTest := "test"
	strEncrypt := Encode(strTest)
	strDecrypt := Decode(strEncrypt)
	fmt.Println(strEncrypt)
	fmt.Println(strDecrypt)
}
```

## MD5

MD5的全称是Message-DigestAlgorithm 5，它可以把一个任意长度的字节数组转换成一个定长的整数，并且这种转换是不可逆的。对于任意长度的数据，转换后的MD5值长度是固定的，而且MD5的转换操作很容易，只要原数据有一点点改动，转换后结果就会有很大的差异。正是由于MD5算法的这些特性，它经常用于对于一段信息产生信息摘要，以防止其被篡改。其还广泛就于操作系统的登录过程中的安全验证，比如Unix操作系统的密码就是经过MD5加密后存储到文件系统中，当用户登录时输入密码后， 对用户输入的数据经过MD5加密后与原来存储的密文信息比对，如果相同说明密码正确，否则输入的密码就是错误的。

> MD5已经被破解，所以应用比较少

## DES

DES是一种对称加密算法，又称为美国数据加密标准。DES加密时以64位分组对数据进行加密，加密和解密都使用的是同一个长度为64位的密钥，实际上只用到了其中的56位，密钥中的第8、16...64位用来作奇偶校验。DES有ECB（电子密码本）和CBC（加密块）等加密模式。

DES算法的安全性很高，目前除了穷举搜索破解外， 尚无更好的的办法来破解。其密钥长度越长，破解难度就越大。


## AES

AES，即高级加密标准（Advanced Encryption Standard），是一个对称分组密码算法，旨在取代DES成为广泛使用的标准。AES中常见的有三种解决方案，分别为AES-128、AES-192和AES-256。

AES加密过程涉及到4种操作：字节替代（SubBytes）、行移位（ShiftRows）、列混淆（MixColumns）和轮密钥加（AddRoundKey）。解密过程分别为对应的逆操作。由于每一步操作都是可逆的，按照相反的顺序进行解密即可恢复明文。加解密中每轮的密钥分别由初始密钥扩展得到。算法中16字节的明文、密文和轮密钥都以一个4x4的矩阵表示。

AES 有五种加密模式：电码本模式（Electronic Codebook Book (ECB)）、密码分组链接模式（Cipher Block Chaining (CBC)）、计算器模式（Counter (CTR)）、密码反馈模式（Cipher FeedBack (CFB)）和输出反馈模式（Output FeedBack (OFB)）。下面以CFB为例。

加密。iv即初始向量，这里取密钥的前16位作为初始向量。


# 明文补码和去补码操作
加密前明文补码规则 （cipher.NewCBCEncrypter 调用前操作）：

加密前：数据字节长度对8取余，余数为m，若m>0,则补足8-m个字节，字节数值为8-m，即差几个字节就补几个字节，字节数值即为补充的字节数，若为0则补充8个字节的8

解密后去除补码规则（cipher.NewCBCDecrypter调用解密完成后）：

解密后：取最后一个字节，值为m，则从数据尾部删除m个字节，剩余数据即为加密前的原文。



> 什么是对称加密？什么是非对称加密？

## ADS

AES，即高级加密标准（Advanced Encryption Standard），是一个对称分组密码算法，旨在取代DES成为广泛使用的标准。AES中常见的有三种解决方案，分别为AES-128、AES-192和AES-256。

AES加密过程涉及到4种操作：字节替代（SubBytes）、行移位（ShiftRows）、列混淆（MixColumns）和轮密钥加（AddRoundKey）。解密过程分别为对应的逆操作。由于每一步操作都是可逆的，按照相反的顺序进行解密即可恢复明文。加解密中每轮的密钥分别由初始密钥扩展得到。算法中16字节的明文、密文和轮密钥都以一个4x4的矩阵表示。

AES 有五种加密模式：电码本模式（Electronic Codebook Book (ECB)）、密码分组链接模式（Cipher Block Chaining (CBC)）、计算器模式（Counter (CTR)）、密码反馈模式（Cipher FeedBack (CFB)）和输出反馈模式（Output FeedBack (OFB)）。下面以CFB为例。

加密。iv即初始向量，这里取密钥的前16位作为初始向量。

