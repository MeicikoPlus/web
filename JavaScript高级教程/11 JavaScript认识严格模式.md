# 认识严格模式

## 什么是严格模式？

首先有历史方面的因素：

- 长久以来，JavaScript不断向前发展并未带来任何兼容性问题；
- 新的特性加入，旧的功能也没有改变，这样做有利于兼容旧代码；
- 但缺点就**JavaScript的创作者的任何错误或者不完善的决定也将永远保留到JavaScript语言中**；

就像他的开发者所说的：它的原创之处并不优秀，它的优秀之处并不原创。

所以在ECMAScript5标准中，JavaScript提出了==严格模式==的概念：

- 严格模式很好理解，是==一种具有限制性的JavaScript模式==，从而使代码隐式的脱离了”懒散模式“；

- 支持严格模式的浏览器在检测到代码有严格模式时，会==以更加严格的方式对代码进行检测和执行==；

  

## 严格模式对JavaScript语义进行的一些限制

- 严格模式通过==抛出错误==来消除一些原有的==静默==错误；
- 严格模式让JS引擎在执行代码时可以进行更多的优化（不需要对一些特殊语法进行处理）；
- 严格模式禁用了在==ECMAScript未来版本中可能会定义的一些语法==；

补充：静默错误是指·一些默认情况下并不报错的一些错误（比如： name = "meiciko" 这种没有使用关键字的声明）。

从某种角度来说，开启严格模式后的JavaScript执行效率是更高的，因为不需要对一些错误在进行处理。



## 开启严格模式

语法：

```javascript
// 给整个的文件开启严格模式
"use strict"
```



给某一个函数开启严格模式：

```JavaScript
function fun() {
  "use strict" // 必须写在开头！
}
```

总结一下：严格模式通过在文件或者函数的开头使用"use strict"来开启。

注1:  一般打包的时候会自动开启严格模式, 不需要手动开启.

注2：没有类似于"no use strict"这样的指令可以使程序返回默认模式，==且现代JavaScript支持class和module，它们会自动启用use strict==。



## 严格模式限制

JavaScript严格模式下的严格语法限制：

- JavaScript设计为新手开发者更容易上手，所以有时会本来语法错误，但是也会被认为可以正常被解析；
- 但是这种方式可能会留下安全隐患；
- 在严格模式下，这种失误就会被当做错误，以便于快速的发现和修正；

1.

```JavaScript
function fun() {
  message = "hellow world"
}
// 这是一个其实有错误语法的代码.
// 这样写会定义一个在全局作用域内的变量:message.
// 这是一种语法错误.
```

```JavaScript
// 如果开启源代码模式后, 再这样写会报错.
// 报错: 无法意外的创建全局变量.
```

2.

```JavaScript
// 严格模式也会引起静默失败, 即不会报错也没有任何效果的赋值操作异常.
var obj = {
  name: "meiciko"
}
obj.name = "Meiciko"
// 上述代码是正确的, 是可以正常修改的.
```

```JavaScript
Object.defineProperty(obj, "name", {
  writable: false // 默认为true, 代表name可以修改, false为不可以修改
})
// 将obj的name设置为不可以修改
obj.name = "Meiciko"
console.log(obj.name) // 非严格模式下是不会报错, 也不会改掉name
// 但是在严格模式下, 会报错, name是一个只读属性, 不能赋值.
```

3.

```JavaScript
// 严格模式下的函数的参数名称不能相同
function fun(num, num) {} 
// 这种情况是不被允许的
```

4.

```
严格模式下也不允许使用with.
以及严格模式下, eval不再为上层引用自身定义的变量.
```

```JavaScript
eval('var message = "meiciko"')
console.log(message) // 报错
```

5.

```javascript
// 严格模式下, this是不会转换成地下类型的.
function fun() {
	console.log(this)
}.
fun.apply("abc") // 输出abc而不是输出一个对象.

// 注意:在严格模式的情况下, 函数的this是不绑定全局对象的, 而是undefined.
```

