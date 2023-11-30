# let / const

在ES5中声明变量都是使用的 `var` 关键字，但是从ES6开始新增了两个关键字可以用来声明变量：`let`、`const`。



# let、const基本的使用

## let

从直观角度（用法方面）来说，`let` 和 `var` 是没有什么区别的，都是用于声明变量的。

```js
var message = "Hellow World" // 这是之前使用var来声明变量的情况。
message = "你好 世界"
```

```js
let message = "Hellow World" // 这是ES6之后使用let来声明的。
message = "你好 世界"
```



## const

- const关键字是constant单词的缩写，表示**`常量`**，衡量的意思；
- 它表示保存的**数据一旦被赋值，就不能被修改**；
- **但是如果赋值的是引用类型，那么就可以通过引用来找到对应的对象，修改对象的内容**；

```js
const message = "Hellow World"
message = "你好 世界" // 程序报错：赋值给了一个恒定的变量！
```

```js
const obj = {name: "meiciko", age: 18} 
obj.name = "Meiciko" // 修改成功，不会报错
obj = {} //报错：赋值给了一个恒定的变量！
```



# let、const的注意事项

## 注意事项一：let、const不允许变量的重复声明！

需要注意的是：==不同于var，let、const并不允许重复声明变量==！

```js
var message = "Hellow World"
var message = "123" // 程序不报错，只是被复写
```

```js
let message = "Hellow world"
let message = "123" // 程序报错：定义了重复的变量
```

重复声明变量是早期JavaScript中存在的设计缺陷，这两个新的关键字相当于对JavaScript的优化。



## 注意事项二：let、const有没有作用域提升?（重要）

我们知道，var声明的变量是会进行作用域的提升的。

但是**如果我们使用let声明的变量，在声明之前访问会报错**。

```js
console.log(message) // undefined
var message = "123"
```

```js
console.log(message) // 报错！在它被初始化之前不能访问！
let message = "123"
```

那么是不是说明被let、const声明的变量只有在代码执行阶段才会创建呢？

事实上，**这些变量会被创建在包含他们的词法环境被实例化时。但是不可以访问它们，直到词法绑定被求值**。

所以，只是==被赋值之前无法被提前访问而不是没有被提前声明==。

var声明的变量能够被作用域提升，也是JavaScript语言之前糟粕的地方！



### 什么是作用域提升？

从上面我们可以看出，在执行上下文的词法环境创建出来的时候，变量事实上已经被创建了，只是这个变量是不能被访问的。

那么已经有了变量，但是不能被访问，是不是一种作用域的提升呢？

作用域提升：==在声明变量的作用域中，如果这个变量可以在声明之前被访问，那么我们就可以称之为作用域提升==。

==在上述过程中，虽然变量被提前创建了出来，但是不能被访问，所以严格意义上我们**不能称之为作用域提升**==。



## 暂时性死区（TDZ）

我们知道，在let、const定义的标识符真正执行到声明的代码之前，是不能被访问的！

**从块级作用域的顶部一直到变量声明完成之前，这个变量处在暂时性死区**！

暂时性死区取决于代码执行顺序，与代码定义的位置无关。

```js
function foo() {
  console.log(message)
}
let message = "Hellow World"
foo() // 可以访问，因为暂时性死区取决于代码执行顺序，与定义位置无关！
console.log(message)
```

暂时行死区形成之后，在该区域内这个标识符不能访问！

```javascript
let message = "Hellow World"
function foo() {
  console.log(message) // 报错：在赋值之前不能够被访问！
  let message = "你好 世界"
}
foo()
```





## 注意事项三：不会添加到Window上面

```js
var message = "Hellow World"
var address = "CN"
console.log(window.message) // 会输出Hellow World
```

从上述代码可以看出，全局中var声明的变量会被添加到window上面。

但是，==使用let和const定义的变量不会添加到window上面==！

```js
let message = "Hellow World"
console.log(window.message) // 输出undefined
```

那么这些变量会保存在哪里呢？虽然使用这两个关键字声明的变量被放在全局的词法环境的环境记录，但是这个环境记录 不等于 window对象。

官方的解释是：==全局的环境记录从逻辑上来说是一个简单的记录，但是它是由一个对象的环境记录（window）和声明环境记录（Script）合成（组合）的==。

所以var声明的变量被添加到了对象环境记录，而let和const声明的变量放在了声明环境记录中。





# var、let、const的块级作用域

## 什么叫做块级作用域

什么叫做块？块其实就是代码块，当吧代码块封装为一个函数的时候，会形成自己的作用域，形成了作用域后，代表着内部定义的变量外部是无法访问的。

在之前的学习中，JavaScript只会形成两个作用域：==全局作用域和函数作用域==。

==从ES6开始，我们使用let和const声明的我们的变量是有块级作用域的==。

```js
{
  let age = 18
  let message = "Hellow World"
}

console.log(age, message)

```

会发现上述代码会报错：age没有被找到（未定义）。

所以，let和const形成了自己的块级作用域，也包括了class等等也会有这样的效果：

```JavaScript
{
   class Person{}
}
const p = new Person() // 报错，没有定义Person
```

不过，函数拥有块级作用域，但是在外面依然是可以访问的！这是因为引擎会对函数的声明进行特殊的处理，允许像var那样在外界直接访问！

```js
{
  fucntion foo() {}
}
console.log(foo) // 可以访问到
```

但是这是浏览器做的特殊处理，实际上是不支持这样的！



## 代码块的词法环境

```js
// 先看下面这段代码：
var message = "Hellow World"
var age = 18
function foo() {}
let address = "CN"
// 代码块
{
  var height = 1.88
  let title = "123"
  let info = "456"
}
```

==当执行上下文执行到一个代码块的时候，执行上下文的词法环境指向一个被创建的代码块的词法环境==（之前的全局执行的词法环境不会被销毁），在这里，这个代码块的词法环境也有自己的环境记录和outer，outer指向的是全局关联的词法环境，而在环境记录中记录着title和info，当然，这段代码中代码块中使用var声明的变量会存在全局的词法环境中的环境记录中的对象式环境记录中！

当执行这个代码块结束的时候，因为之前的代码块词法环境没有被引用，所以会被销毁掉！

![](..\images\词法环境\代码块的词法环境.png)





# var、let、const的选择

## 对于var的使用

- 我们要明白一个事实，var标签出来的特殊性比如作用域提升，window的全局对象，没有块级作用域等都是一些历史遗留的问题；
- 其实是JavaScript在设计之初的一种语言缺陷；
- 在实际编程中，可以==使用最新规范来写，而不是再使用var来定义变量==！



## 对于let和const

- 对于let和const是推荐使用的！
- 我们会==优先推荐使用const，这样可以保证数据的安全性不会被随意的篡改==；
- 当==知道一个变量后续需要被重新赋值的时候，才会使用let==；
- 这种在很多其他于语言中也是一种约定俗成的规范；