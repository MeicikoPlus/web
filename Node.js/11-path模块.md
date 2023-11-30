文件路径有两种表示形式，即绝对路径和相对路径。

- 绝对路径：完整的路径，以盘符开头，并完整描述了文件所在的上层目录。例如：`C:\Users\Lenovo\Desktop\ZhiYou\02-JavaScript\0914\node模块化\02node内置模块.js`。
- 相对路径：不完整的路径，仅记录结尾部分，忽略了盘符和顶层目录。例如：`node模块化\02node内置模块.js`。

`__dirname`表示当前文件所在目录的绝对路径。

# **path模块**

`path`是一个内置模块，主要用于处理文件路径。

- `join()`函数用于将两个路径拼接为一个路径。例如：`abc/123`和`qwe/456`通过`path.join()`拼接后得到结果`abc/123/qwe/456`。
- `resolve()`函数将相对路径转换为绝对路径。例如，将`./02node内置模块.js`转换为绝对路径。
- `extname()`函数用来获取文件路径中的扩展名。例如，对路径`./02node内置模块.js`应用`path.extname()`函数将返回`.js`。
- `basename()`函数可获取文件的文件名，即文件路径中的最后一部分。例如，对路径`./02node内置模块.js`应用`path.basename()`函数将返回`02node内置模块.js`。

```js
const path = require("path");

let p1 = "abc/123";
let p2 = "qwe/456";
let p3 = path.join(p1, p2);
console.log(p3); // 输出：abc/123/qwe/456

let p4 = "./02node内置模块.js";
let absolutePath = path.resolve(p4);
console.log(absolutePath); // 输出：C:\Users\Lenovo\Desktop\ZhiYou\02-JavaScript\0914\node模块化\02node内置模块.js

let extension = path.extname(p4);
console.log(extension); // 输出：.js

let filename = path.basename(p4);
console.log(filename); // 输出：02node内置模块.js

```

