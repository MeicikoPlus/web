# import 函数

通过 import 加载一个模块，是不可以放在逻辑代码中的。

假设现在有一个 util.js 文件

```js
export const user_name = "why"
export const password = "123"
export function sayHello() {
  console.log("hello world")
}
```

现在去 main.js 文件中导入这个文件： 

```js
import { user_name, password, sayHello } from "./util.js";

console.log(user_name)
console.log(password)
sayHello()
```

现在是可以正常导入的，但是如果将导入的操作写在逻辑代码中：

```js
if (true) {
  import { user_name, password, sayHello } from "./util.js";

  console.log(user_name)
  console.log(password)
  sayHello()
}
```

会出现报错：导入的功能只能在顶部导入。

这是因为，浏览器在扫描代码之前，就会在顶部的代码中扫描 import，并将对应的文件下载下来，然后开始执行代码，所以不允许在逻辑代码中导入，==只能写在 JavaScript 代码的顶层==。

但是开发中也有，在某种情况下才会去导入某个文件的操作，这个时候可以使用 ==import 函数==；

## import 函数使用

```js
if (true) {
  // 这一步操作是异步的，是不会阻塞代码的执行的
  // 而这个import中返回的是一个promise
  const importPromise = import( "./util.js")
  importPromise.then(res => {
    console.log(res.user_name)
    console.log(res.password)
    res.sayHello()
  })
}
```

所以当我们必须要将 import 和 逻辑关系联系起来使用的时候，需要使用 import 函数；

·

所以，通过 import 加载一个模块，是不可以放在其逻辑代码中的，不然会报错：

- 这是因为，ES Module 在被 JS 引擎解析的时候，就必须知道它的依赖关系；
- 由于这个时候 js 代码没有任何的运行，所以无法再进行类似于 if 的判断中根据代码的执行情况

- 甚至拼接路径的写法也是错误的，因为我们必须到运行到时候才能确定 path 的值；

```js
import { user_name, password, sayHello } from "./util" + ".js";
// 就像是上述的代码，也是会报错的，因为相加是在js代码执行的时候才会去拼接，但是模块应该在js代码没有运行的时候就被解析，所以这个写法是不被允许的。
```



# import meta

import meta 是一个给 ==JavaScript 模块暴露特定上下文的元数据属性的对象==；

- 它包含了这个模块的信息，比如说这个模块的URL；
- 它是在ES11中新增的特性。



