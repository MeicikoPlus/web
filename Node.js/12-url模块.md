# URL（统一资源定位符）总结

URL（统一资源定位符）是用于定位网络上资源的地址。它包含协议名、域名、端口号、路径和参数等信息。下面是对URL的一些总结：

## 协议

- URL的协议名包括http和https，其中https相比http更安全。

## 域名和端口号

- 域名是指IP地址的别名，用于标识网站的地址。
- 端口号是指协议通信时使用的端口，http协议默认端口号是80，https默认端口号是443。默认端口号可以省略。

## 路径

- 路径指明了服务器上资源的位置。URL的路径分为两种：
  - 以"/"开头的路径是相对于当前URL的根路径。例如：`/abc.jpg`实际访问的是`www.baidu.com/abc.jpg`。
  - 非"/"开头的路径是相对于当前页面的路径。假设当前页面地址是`www.baidu.com/aaa/bbb/ccc`，使用`abc.jpg`实际访问的是`www.baidu.com/aaa/bbb/ccc/abc.jpg`。

## 参数

- 参数是通过URL传递给服务器的附加信息，以"?"开始。多个参数使用"&"分隔。
- 例如：`www.baidu.com/abc?key=why&status=1`中的参数为`key=why`和`status=1`。

## 相对地址和绝对地址

- 绝对地址是一个完整的URL，以协议名开头，例如`https://www.baidu.com`。
- 相对地址是不以协议名开头的URL，可以是域名、IP或路径开头。

## 示例代码

下面是一个使用Node.js的`url`模块解析URL的示例代码：

```
javascriptCopy codeconst url = require("url")

const urlStr = "www.baidu.com/aaa/bbb/ccc/abc?key=why&status=1"
const urlObj = url.parse(urlStr) // 不建议使用

console.log(urlObj)
```

以上是对URL的一些基本概念和示例代码的总结。URL在Web开发中具有重要的作用，能够准确定位和访问网络上的资源。