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

