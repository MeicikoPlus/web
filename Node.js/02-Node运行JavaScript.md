# 运行 JavaScript

已知 Node.js 是借助的 V8 引擎来实现的，所以可以直接借用 Node.js 来运行 JavaScript 代码：

```js
这是一个已经编写好的文件（abc.js）：
console.log("abc")
```

可以在命令窗口中使用下面的命令来运行：

```
node abc.js (必须在这个目录下)
```



## Node 输入输出

### 输出

Node 输出内容与 JavaScript 的写法一一致，使用的是 `console.log()`。

```js
console.log("Hellow World")
console.clear() // 清空控制台
console.trace() // 打印函数的调用栈
```

### 输入

Node 有一个对应的全局对象，叫做 ==process（进程）==，它有一个 ==argv（argument vector（向量，矢量））== 的容器。

我们可以通过`console.log(process.argv)` 来获取输入的变量值。

```js
这是在终端的输入：
node abc.js num3=300 num4=400
```

```js
输出：
abc
[
  'node程序的路径',
  '目标文件的路径',
  'num3=300',
  'num4=400'
]
```

`argv` 是一个==数组==，在终端输入的参数会存在这里面。

在后面传入的参数，可以使用 `process.argv[2]` 和 `process.argv[3]` 来获得，得到的结果为 `String` 类型。

也可以通过 `forEach` 去遍历信息：

```js
process.argv.forEach(item => {
  console.log(item)
})
```



## Node REPL

什么是 `REPL` ？`REPL` 是 `Read-Eval-Print-Loop` 的简称，翻译为：读取-求值-输出-循环；

它是一个==简单的，交互式的编程环境==；

事实上，浏览器的输出窗口就可以看做是一个 REPL；

在终端中打开 `REPL` 可以直接在终端中输入==node== ，就可以打开了！



