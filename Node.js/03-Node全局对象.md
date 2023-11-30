# 全局对象

在之前的 JavaScript 使用中，浏览器有一个全局对象叫 window（浏览器的全局对象），而如果使用 node 来运行JavaScript 文件，node 是找不到 window（显示未定义） 的。

在 node 中，如果想要使用全局对象，是使用的 ==global==。

不同的是，在 node 中，全局对象并不常用。



## 特殊的全局对象

在 node 中，有一些特殊的全局对象，之所以叫特殊的全局对象，是因为这些全局对象实际上是模块中的变量，只是每个模块都有，所以看起来是全局变量。

这些特殊的全局变量在命令行的交互中是不可以使用的。

包括：_dirname，__filename， ==exports==，==module==，==require()==

后三个全局对象会在模块化中详细介绍。

### __dirname（两个下划线）

这个表示当前文件的目录结构。

```js
console.log(__dirname) // 使用node运行后输出文件的目录名字
```

上述返回的只有目录不带上自身的名字，如果想要获得自身的目录加上自身的文件名，可以使用__filename，

```js
console.log(__filename) // 会带上自身的文件名称
```

在开发中，会使用 dirname 来寻找这个文件的相对目录内的其他文件。



## 常见的全局对象

### process

代表着代码的进程，可以从中读取进程的信息。（内容量很大）

一般用于通过这个对象来获取传入的参数或者 node 的版本。

### console

提供了简单的调试控制台。

### 定时器函数

- setTimeout：在指定毫秒后执行一次；
- setInterval：每间隔指定毫秒后执行一次；
- setImmediate：回调函数 I/O事件后立即执行；
- process.nextTick：添加到下一次的 tick 队列中；

```js
// 立即执行回调函数，不需要传入时间
setImmediate(() => {
	console.log("setImmediate")
})
```

### global

global 对象类似于之前学到的 window 对象，在使用 var 定义的时候，会默认添加到 window 中，但是在 node 中并不会添加到 global 中。

但是 node 中还有一个 globalthis，在浏览器运行环境中，globalthis 是一个标识符，指向的是window，在 node 中 globalthis 指向的也是 global （===）。



