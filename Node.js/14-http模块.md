# http

## 什么是http模块？

在了解 `http` 模块之前，需要先了解什么是客户端，什么是服务器：

- 在网络节点中，负责消费资源的电脑叫做客户端；
- ==负责对外提供网络资源的电脑==，叫做服务器；
- 服务器和普通电脑的区别，就在与服务器上安装了==web 服务器软件==，例如 IIS，Apache 等等，通过安装这些服务器软件，就能够把一台普通的电脑变成一台 web 服务器；
- 所以本质上服务器和普通电脑的区别就在于有没有安装 web 服务器软件；

而在 `node` 模块中的 `http` 模块是官方提供的，用来==创建 web 服务器== 的模块，可以方便的把一台普通的电脑变成一台 web 服务器。

导入 http 模块的方法也和之前导入内置模块的办法相同：

```js
const http = require("http")
```



## 创建 web 服务器

### 步骤一：导入http模块

### 步骤二：创建一个web服务器实例

创建服务器使用的是 http 模块的 createServer 方法：

```js
const server = http.createServer()
```

### 步骤三：为服务器绑定 request 事件

绑定事件需要使用服务器实例的 on 方法：

```js
const http = require("http")

const server = http.createServer()
server.on("request", (req, res) => {
  // 当有请求发送到服务器的时候，就会触发 request 事件，从而调用这个事件处理函数
  console.log("hello world")
})
```

### 步骤四：启动服务器

调用服务器实例的 listen 方法，就可以启动当前的 web 服务器实例，listen 方法有两个参数，第一个参数是端口号，第二个参数是回调函数。

经历上述四个步骤后，一个最基本的服务器就已经出现了：
```js
const http = require("http")

const server = http.createServer()
server.on("request", (req, res) => {
  // 当有请求发送到服务器的时候，就会触发 request 事件，从而调用这个事件处理函数
  console.log("hello world")
})

server.listen(80, () => {
  console.log("http server running at http://127.0.0.1:80")
})
```





## req 请求对象

在服务器接收到了客户端的请求的时候，就会通过调用 on 方法为服务器绑定的 request 事件处理函数，如果想在事件处理函数中，==访问与客户端相关的数据或者属性==，可以使用如下的方式：

- req 是请求对象，它包含了客户端相关的数据和属性，例如：

- req.url 就是客户端请求的 url 地址；
- req.method 是客户端请求的方案类型；

```js
const http = require("http")

const server = http.createServer()

server.on("request", (req, res) => {
  console.log("req:", req)
  console.log("req.url:", req.url)
  console.log("req.method:", req.method)
})

server.listen(8080, () => {
  console.log("http server running at http://127.0.0.1:8080")
})
```



## res 响应对象

在服务器的 request 时间处理函数中，如果想要访问==与服务器先关的数据或者属性==，可以使用以下方式：

==res.end 方法==：向客户端发送制指定的内容，并结束这次请求的处理过程；



## 解决中文乱码的问题

当调用 res.end 方法，向客户端发送中文内容的时候，会出先乱码的问题，此时需要==手动设置内容的编码方式==。

设置响应头

```js
const http = require("http")

const server = http.createServer()

server.on("request", (req, res) => {
  // 利用 end 方法，向客户端响应一些内容：
  res.setHeader("Content-Type", "text/html; charset=utf-8")
  res.end("请求成功")
})

server.listen(8080, () => {
  console.log("http server running at http://127.0.0.1:8080")
})
```





