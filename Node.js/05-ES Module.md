# 认识 ES Module

目前的前端开发中已经不在通过 `script` 标签进行频繁的引入，而是使用模块化的方案，当需要使用到某个文件中的变量的时候，便会从目标文件导出，并在本文件引入。

之前学的 `CommonJS` 是一种社区规范，并且在 `node` 中得到了实现，但是并没有在浏览器中实现，那么在浏览器中进行模块化开发的话，就需要使用 ECMAScrpit2015 中提供的模块化：`ES Module` 。

当然还有另外一种使用方式，ES Module 对某写浏览器并不支持，但是一个打包工具可以做到这些事情，就是 `Webpack` ，因为 Webpack 是一个模块化的打包方式。

注：两种方式都可以做到模块化开发，并且可以混合使用。

注：使用 ES Module 会自动开启严格模式。



## export 

export 关键字是将一个模块中的变量，函数，类导出。

如果希望它的内容全部导出。可以使用如下的方式：

- 在语句声明前直接加上 export 关键字；
- 将所有要导出的标识符，放在 export 后面的{}中；
- 导出时候给标识符起一个别名；



## export import 的使用

```js
// util.js
const user_name = "meiciko"
const password = "123456"
function sayHello() {
  console.log("hello world")
}

// 导出
export {
  user_name, 
  password,
  sayHello
}
```

```js
// main.js
import { user_name, password, sayHello } from "./util.js";
console.log(user_name)
console.log(password)
console.log(sayHello())
```

```html
// html
<!DOCTYPE html>
<html>
<head>
    <title>ES Module Tutorial</title>
</head>
<body>
    <script type="module" src="main.js"></script>
</body>
</html>
```

==常用==



## 产生冲突

如果，在 `main.js` 文件中，也想要一个叫 `user_name` 的变量名称：

```js
// main.js
import { user_name, password, sayHello } from "./util.js";

const user_name = "main"
console.log(user_name)
console.log(password)
console.log(sayHello())
```

此时就会产生错误，为了解决这类问题，可以使用 ==`as`==，它的作用就是给导出的标识符取一个==别名==。

```js
// util.js
const user_name = "meiciko"
const password = "123456"
function sayHello() {
  console.log("hello world")
}

// 导出
export {
  user_name as username, 
  password,
  sayHello
}
```

当然，也可以在导入的文件中取一个别名：

```js
// main.js
import { user_name as username, password, sayHello } from "./util.js";
```

一般来说，别名也是在导入的文件中取。



## 其他导出方式

当遇到需要导出某个变量的时候，直接在变量的命名之前进行导出操作：

```js
export const name = "meiciko"
```

这样就可以在定义的时候就将它导出出去。

注意：==使用这种导出方式的时候是不能使用 as 的==。

==常用==



### 其他导入方式：给模块取别名

在导入的时候，可以使用 `*` 来表示将目标模块所有的内容全部导入：

```js
import * form "./util.js"
```

但是只这样写是无法使用的，所以需要给模块取一个别名：

```js
import * as obj fom "./util.js"
console.log(obj.user_name)
```





# 整理代码的思路（入口文件）

当需要导入多个文件的内容的时候，可以创建一个==入口文件==。

```js
import {"变量1", "变量2"} form "./util1.js"
import {"变量3", "变量4"} form "./util2.js"
import {"变量5} form "./util3.js"
import {"变量6"form "./util4.js"
        
export {
	变量1，
  变量2，
  ...
}
```



