## func (enc *Encoding) Decode(dst, src []byte) (n int, err error)

参数列表：

- dst 解码缓冲区
- src 要解码的字符串切片

返回值：

- n 解码到缓冲区的字节数
- err 可能的错误

功能说明：

对输入的字符串进行 base64 解码

代码实例：

```go
package examples

import (
    "fmt"
    "encoding/base64"
)

func ExampleDecode1() {

    // 原始字符串
    origin := "this is a test string."

    // 要解码的字符串
    src := "dGhpcyBpcyBhIHRlc3Qgc3RyaW5nLg=="
    // 计算解码缓冲区需要的长度
    maxlen := base64.StdEncoding.DecodedLen(len(src))

    // 创建足够长度的缓冲区
    dst := make([]byte, maxlen)

    // 解码
    n, err := base64.StdEncoding.Decode(dst, []byte(src))
    fmt.Println(err == nil)

    // n 小于等于缓冲区长度
    fmt.Println(n <= maxlen)

    // n 等于原始字符串的长度
    fmt.Println(n == len(origin))

    // 原始字符串和解码后字符串相同
    fmt.Println(string(dst[0:n]) == origin)

    // Output:
    // true
    // true
    // true
    // true

}

// 测试待解码内容中含有 "\r" "\n" 的情况(会被忽略)
func ExampleDecode2() {

    origin := "this is a test string."
    src := "dGhpcyBpcyBhI\rHRl\nc3Qgc\n\r3RyaW5nLg=="
    maxlen := base64.StdEncoding.DecodedLen(len(src))
    dst := make([]byte, maxlen)
    n, err := base64.StdEncoding.Decode(dst, []byte(src))
    fmt.Println(err == nil)
    fmt.Println(n <= maxlen)
    fmt.Println(n == len(origin))
    fmt.Println(string(dst[0:n]) == origin)

    // Output:
    // true
    // true
    // true
    // true

}

// 测试解码失败的情况
func ExampleDecode3() {

    origin := "this is a test string."
    src := "dGhpcyBpcyBhI@HRlc3Qgc3RyaW5nLg=="
    maxlen := base64.StdEncoding.DecodedLen(len(src))
    dst := make([]byte, maxlen)
    n, err := base64.StdEncoding.Decode(dst, []byte(src))
    fmt.Println(err != nil)
    fmt.Println(n <= maxlen)
    fmt.Println(n != len(origin))
    fmt.Println(string(dst[0:n]) == "this is a")

    // Output:
    // true
    // true
    // true
    // true

}
```
