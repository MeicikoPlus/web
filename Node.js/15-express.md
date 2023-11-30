# Express

## Express

什么是 Express ？官方给出的概念是：==Express 是基于 Node.js 平台，快速，开发，极简的 Web 开发平台==；

可以通俗理解为，Express 的作用和 Node.js 内置的 http 模块类似，是==专门用于创建 Web 服务器的==，只不过相比较于 http ，Express ==是基于 http 模块进一步封装出来的==，使用起来更加简洁，并且功能更加的强大；

Express 的==本质就是一个 npm 上面的第三方包==，提供了快速创建 Web 服务器的便捷方法；

对于前端开发程序员来说，最常见的两种服务器分别是：==Web 网站服务器==（专门对外提供 Web 网页资源的服务器），==API 接口服务器==（专门对外提供 API 接口的服务器）

而通过使用 Express，可以方便，快速的创建 Web 网站的服务器或者 API 接口的服务器；



## 安装

可以通过 npm 命令，直接在终端中进行安装：

```js
npm i express
```



## 创建基本的 Web 服务器

```js
// 导入express
const express = require("express")

// 创建 web 服务器
const app = express()

// 调用app.listen(端口号，启动成功后的回调函数)，启动服务器
app.listen(80, () => {
  console.log("Express Server running at http://127.0.0.1")
})
```



## 监听 GET 请求

通过 ==app.get== 方法，可以监听客户端的 GET 请求，具体的语法格式如下所示：

`app.get("请求URL", function(req, res) { /* 处理函数 */})`

- 参数一：客户端请求的 URL 地址；
- 参数二：请求对应的处理函数：
  - req：请求对象，包含了请求相关的属性和方法；
  - res：响应对象，包含了与响应相关的属性和方法；





## 监听 POST 请求

监听 POST 请求使用的是 ==app.post== 方法，具体的语法格式如下所示：

`app.post("请求URL", function(req, res) { /* 处理函数 */})`

- 参数一：客户端请求的 URL 地址；
- 参数二：请求对应的处理函数：
  - req：请求对象，包含了请求相关的属性和方法；
  - res：响应对象，包含了与响应相关的属性和方法；



## 把内容响应给客户端

通过 ==res.send== 方法，可以把处理好的内容发送给客户端；

```js
// 导入express
const express = require("express")

// 创建 web 服务器
const app = express()

app.get("/user", (req, res) => {
  res.send({
    code: 1000,
    msg: "success",
    data: {
      username: "meiciko",
      password: "1234567"
    }
  })
})

app.get("/post", (req, res) => {
  res.send("请求成功!")
})

// 调用app.listen(端口号，启动成功后的回调函数)，启动服务器
app.listen(80, () => {
  console.log("Express Server running at http://127.0.0.1")
})


```





## 获取 URL 中携带的查询参数

通过 ==req.query== 对象，可以访问到客户端通过==查询字符串==的形式，发送到服务器的参数；

注：默认情况下，req.query 是一个空对象；

```js
app.get("/user", (req, res) => {
	// 默认情况下 res.query 是一个空对象
  // 当客户端采用 ？name=meiciko&password=1234567 这种查询字符串的形式，发送到服务器的参数
 	// 可以通过 req.query 对象访问到，例如：
  // req,query.name   req.query.password
  console.log(req,query.name, req.query.password)
})

```



## 获取 URL 中的动态参数

通过 ==req.params== 对象，可以访问到 URL 中，通过 `：` 匹配到的==动态参数==。

```js
app.get("/user/:id", (req, res) => {
	// 和 res.query 相同，req.params 默认是一个空对象
  // req.params 里面粗放着通过：动态匹配到的参数值
  console.log(req.params)
})
```

通过上述形式后，当客户端发送了一个 ==http:127.0.0.1/user/1== 的请求后，在服务器这边的体现就是：

```js
// 输出：
{
  "id": "1"
}
```

同理，如果发送的是  ==http:127.0.0.1/user/12== 服务器这边对象中的 id 就是12

### 多个动态参数

```js
app.get("/user/:id/:name", (req, res) => {
	 console.log(req.params)
})
// 假设客户端发送的请求是 http:127.0.0.1/user/1/meiciko
```

```js
// 输出：
{
  "id": "1",
  "name": "meiciko"
}
```





## 托管静态资源

express 提供了一个非常好用的函数，叫做 `express.static()` ，通过它，我们可以非常方便的==创建一个静态资源服务器==。

通过这个函数，我们可以将目录下的图片，css文件，JavaScript文件对外开放访问。

使用方法：在 ==app.uer== 方法中使用：

```js
app.use(express.static("指定的一个目录"))
```

需要注意的是，express 在指定的静态目录中查找文件，并且对外提供资源的访问路径，因此，存放静态文件的目录名不会存在在url中。





## 托管多个静态资源目录

如果需要托管多个静态资源目录，只需要多次使用`express.static()` 方法即可。

```js
app.use(express.static("指定的一个目录"))
app.use(express.static("指定的另一个目录"))
```

值得注意的是，客户端打开的页面是第一次托管中的页面。





## 挂在路径前缀

如果希望托管的静态资源访问路径之前怪路径前缀，可以使用如下的方式

```js
app.use("/public", express.static("指定的一个目录"))
```



## nodemon

在服务端的代码发生改变后，我们需要不断的关掉之前的服务然后重启新的服务，但是频繁的执行这个步骤会很繁琐，所以可以下载 ==nodemon==工具，这个工具可以自动监听服务器的代码变动，从而自动的更新服务，使用这个工具也很简单，写法如下：

```js
npm install -g nodemon
```

### 使用方法

在下载好这个工具的时候，可以将node命令替换为nodemon来启动服务器，这样的好处是，代码被修改以后，会被nodemon监听到，从而实现自启动的效果。

