# 模块化开发

## 介绍

什么是模块化开发？

- 模块化开发的最终目的是将程序==划分==为一个小小的结构；
- 这个结构中编写属于自己的逻辑代码，与自己的作用域，定义变量名词时不会影响到其他的结构；
- 这个结构可以将自己希望暴漏的==变量，函数，对象等导出==给其结构使用；
- 也可以通过某种方式，==导入另外解构中的变量，函数，对象==等；

也就是说，模块化开发是把一个大型的项目分为一个个不同的模块，每个模块独立开发，==互不影响==。

当两个不同的开发人员在两个不同的文件中命名了一个相同名称的变量，并不会影响到另外一个的文件，因为独立的模块有独立的作用域。 

如果有一个模块需要使用其他模块内部的内容的时候，可以进行==导出==和==导入==两个操作。

按照上述所说的情况的开发程序的过程，就是模块化开发的过程（注：早期的 JavaScript 是没有模块化开发的）。



## 使用模块化（上）

在之前的学习中，两份不同的 JavaScript 代码引入文件，会采用下面的方法：

```html
<script src="1.js"></script>
<script src="2.js"></script>
```

但是本质上，两份文件的代码内容还是在同一个作用域下面，为了避免这样的操作产生的命名冲突，通常每个文件会将代码内容写入一个立即执行函数中：

```js
// 假设这是1.js的文件
(function() {
  // 正常内容
})()
```

这样做，是可以避免命名冲突的问题的，因为每个立即执行函数都会产生自己的作用域，但是这样会带来另外一个问题：即另外一个文件是拿不到第一个文件中的某些变量的。

为了解决这个问题，通常会在立即执行函数中，把内部的变量以一个对象的形式返回出去：

```js
// 假设这是1.js的文件
const moduleA = (function() {
  // 正常内容
  let name = "meiciko"
  let age = 18
  let height = 1.88
  return { // 采用属性增强的写法
    name,
    age,
    height
  }
})()
```

这样当 `2.js` 想要拿到 `1.js` 中的 name 的时候，可以这样做：

```js
// 假设这是2.js文件
console.log(moduleA.name)
```



## 使用模块化（下）

上述的模块化方案是个人制定的，没有统一的标准，社区使用的模块化方案为：`CommonJS`，ECMA 后来退出了自己模块化的方案：`ESModule`。

### 模块化的历史

- 在早期的网页开发时期，Brendan Eich 开发 JavaScript 仅仅作为一种==脚本语言==，作为一些简单的表单验证或者动画实现等等，这个时期仅仅需要把 JavaScript 代码写到 script 标签中即可，并没有必要放到多个文件中来编写。
- 但是随着 JavaScript 的快速发展，JavaScript 代码变的越来越复杂：
  - ajax 的出现，前后端分离，意味着数据返回后端后，开发人员需要使用 JavaScript 来实现对页面的渲染；
  - SPA 的出现，前端页面变的更加复杂，包括==前端路由==，==状态管理==等等一些列复杂的需求需要通过 JavaScript来实现；
  - 包括 Node 的实现，JavaScript 编写复杂的后端程序，没有模块化是致命的硬伤；

- 所以，模块化开发是 JavaScript 一个非常迫切的需求，但是 JavaScript 本身知道 ES6 之后才推出了自己的模块化方案，在此之前，为了让 JavaScript 支持模块化，涌现了很多不同模块化规范：AMD，CMD，CommonJS 等等；

### CommonJS 规范和 Node 的关系

CommonJS ==是一个规范==，最初提出来是在浏览器以外的地方使用，并且当时被命名为 ServerJS ，后来为了体现它的广泛性，修改为了 CommonJS 。

- ==Node== 是 CommonJS 在服务器端一个具有代表性的实现；
- ==Browserify== 是 CommonJS 在浏览器中的一种实现；
- ==Webpack== 打包工具具备对 CommonJS 的支持和转换；

CommonJS 规定：模块要导出内容应该使用：==exports==，模块中要导入内容应该使用：==require==。

（注：浏览器默认没有对 CommonJS 进行实现，也就是说默认情况下是无法使用 CommonJS 的。）

（node 已经实现了 CommonJS 的规范）



# CommonJS

假设现在有两个文件（a.js，b.js）

```js
a.js
const user_name = "meiciko"
const password = "123456"

function formatUser(user_name, password) {
  return {
    user_name,
    password
  }
}

exports.user_name = user_name
exports.password = password
exports.formatUser = formatUser
```

```js
b.js
const {user_name, password, formatUser} = require("./a.js") // 对象的解构

console.log(res.user_name)
console.log(res.password)
console.log(res.formatUser(res.user_name, res.password))
```

上述是最基础的使用方法，可以在 node 中直接使用，浏览器因为没有实现 CommonJS 所以不能这样使用。

所以，==Node 中对 CommonJS 进行了支持和实现==，让我们在开发 node 的过程中可以方便的进行模块化的开发：

- 在 Node 中，每一个 js 文件都是一个独立的模块；
- 这个模块中包括 CommonJS 规范的核心变量：==exports，module.exports，require==；
- 我们可以使用这些变量来方便进行模块化开发；

（注：exports 是一个对象，我们可以在这个对象中添加很多个属性，添加的属性会导出，另一个文件就可以导入。）



## CommonJS 本质

在内存中，默认有一个空对象（exports），我们可以在这个对象中添加一些属性。

在名为 require 函数中，本质上是把内存中 exports 这个对象的地址给返回，并且可以通过==引用赋值==，来拿到内部的数据。

所以，CommonJS 的==本质就是一个引用赋值==。

与此同时，require 通过各种查找方式，最终找到了这个对象。



## CommonJS 用法

在开发中，很多时候导出并不会使用上述的语法，而是使用：

```js
module.exports.name = name
```

这种写法和上一种写法本质上是一样的，都是通过内存中的一个对象来完成值的传出，module 对象中的exports 属性指向的就是 exports 对象（是同一个对象），也就是说，==Node 本质上是在导出 module.exports对象==。

所以，上述情况很多时候可以优化为下面的代码（直接改了内存中的exports的指向）：

```js
module.exports = {
  name: name
}
```

- CommonJS 中是没有 module.exports 的概念的；
- 但是为了实现模块的导出， Node 中使用的是 Module 类，每一个模块都是 Module 的一个实例，也就是 module。
- 所以在 Node 中真正用于导出的其实不是 exports ，而是module.exports 。
- 因为 module 才是导出真正的实现者。
- 但因为 CommonJS 规范中没有 module.exports 所以将两者相等。



## require 细节

假设现在有一个 util 文件

```js
const sayHellow = function() {
  console.log("hellow world")
}

const name = "meiciko"
const age = 18

console.log(module.exports === exports) // true

module.exports = {
  name,
  age,
  sayHellow
}
```

已知，require 是一个函数，可以传入一个字符串参数，作为一个路径，可以用于帮我们引入一个模块中导出的对象。

现在，在另一个文件中，想要导入 util.js 内的数据，可以这样写：

```js
let { name, age, sayHellow } = require("./util.js")
```

但是，上述代码中的文件后缀其实是可以去掉的：

```js
let { name, age, sayHellow } = require("./util")
```

之所以可以这样修改，是因为 require 拥有自己的查找规则

- ==导入格式：require(X)==

  - **情况一**：X 是一个 Node 核心模块（也就是 node 的内置模块），比如 path，http，此时函数会直接返回核心模块，并且停止查找。
  - **情况二**：如果 X 是一个以 `./` ，`../`，`/` ... ... 这样开头的字符串，函数回去寻找这个目录的这个文件，如果文件有后缀名的话，会直接找到这个文件，如果不写后缀名，会自动按照下列顺序查找：
    - 第一步：
      - 查找 X.js 文件；
      - 查找 X.json 文件；
      - 查找 X.node 文件；
    - 第二步：如果没有找到对应的文件，会将 X 作为一个目录，寻找这个目录下的 index 文件：
      - 查找 X / index.js 文件；
      - 剩余步骤类似第一步；

  - **情况三**：如果放入的参数不是一个路径，也不是一个核心模块：

    ```js
    const res = require("meiciko")
    ```

    此时会抛出一个错误：找不到。

    但是其实可以让它找到，如果在当前文件夹下面有一个文件夹 `node_modules` （名字是固定的），在这个文件夹中有一个 meiciko 的文件或者文件夹，就会进行情况二的步骤。

  在开发中，会使用 `const axios = require("axios")` 这句代码，实际上是因为当使用 `npm install axios` 命令后，会将一个远程库中的 axios 源文件下载到 `node_modules` 文件夹中，此时函数会进行情况三的查找方式，从而拿到 axios 。



## 模块加载过程

结论一：==模块在第一次引入的时候，模块中的 js 代码会运行一次==。

结论二：==模块被多次引入的时候，会缓存，最终只会加载一次==。

之所以只会加载一次是因为每个模块对象 module 都有一个属性：loaded，为 false 的时候表示还没有加载，为 true 表示已经加载。

