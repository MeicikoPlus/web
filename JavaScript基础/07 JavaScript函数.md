# **JavaScript函数**

## 函数的声明和调用

**函数其实就是对某段代码的封装**, **这段代码通常用于完成我们需要的功能**.

默认情况下, **JavaScript会给我们提供一些已经实现好的函数**, 当然, 我们也可以编写自己的函数.

函数的使用通常分为声明函数(封装), 和调用函数(享受).

声明函数也可以叫做**定义函数**, 我们可以根据自身的需求定义很多自己的函数.

## 函数的声明

函数的声明使用function关键字, 这种写法称之为函数的定义:

```javascript
function 函数名() {
    // 函数封装的代码... ...
}
```

注: **函数的命名规则跟变量的命名规则相同**, 且最好做到**见名知意**.

函数声明结束后, 并不会执行内部的代码, 如果想要执行内部的代码需要再次调用函数.

```JavaScript
function fun() {
    // 函数封装的代码... ...
}
// 调用函数
fun()
```

## 函数的参数

函数的参数可以增加函数的通用性, 针对相同数据处理的逻辑, 以及对更多数据的适应性.

函数的参数分为形参和实参, **形参值定义函数的时候小括号内部的参数, 可以在函数内部作为变量使用, 是形式上的参数**.

**实参是指调用函数时小括号内部的参数, 用来将数据传递到函数的内部**.

```JavaScript
// 使用传入参数的方法将播放量数据转换为指定形式的字符串
// 现有播放量的数据
let datalist = [
  {
    data: 123456
  },
  {
    data: 678901
  }
]
for (let i = 0; i < datalist.length; i++) {
  // 将没有处理的数据以实参的形式传入math.floor()函数
  let str = Math.floor(Number(datalist[i].data)/10000) + "万"
	datalist[i].data = str
}
console.log(datalist)
```



### ES6为函数添加默认值

```javascript
function fn(a = 2, b = 3) { console.log(a, b) }
fn() // 输出2和3
```

上述写法可以给函数配置默认值。



## 函数的返回值

函数不仅仅可以有参数, 还可以有返回值, 一般使用**return**关键字来返回结果, 需要注意的时, **一旦执行了return操作, 那么当前的函数就会被终**止.

如果函数没有返回语句, 则会默认返回undefined.

## argument

在函数中, 都存在一个变量叫argument, 这个变量不是数组, 是一个对象类型(但是类数组), 类数组使得它是索引从零开始的有长度, 并且可以通过索引去访问内部的元素.

```javascript
function fun(name, age, score) {
	console.log("name:" + name + "age:" + age + "score:" + score)
  console.log(typeof agruments)
 	console.log("agruments[0]:" + arguments[0])
  console.log("agruments[1]:" + arguments[1])
  console.log("agruments[2]:" + arguments[2])
}
fun("meiciko", 18, 200)
// object
// agruments[0]:ckw
// agruments[1]:18
// agruments[2]:200
```

## 函数中调用函数

函数可以在内部去调用另一个函数, 当然也可以调用自己, 但是调用自己最好有一个结束条件, 否则会出现无限调用的情况, 自己调用自己的这种情况叫做递归.

```javascript
// 递归求幂函数的一般方法
function fun(x, n) {
	let result = 1
  for(var i = 1; i < n; i++){
  	result *= x    
  }
  return result
}

// 递归函数思路
function fun(x, n) {
  // 设置结束条件
  if(n == 1){
    return x
  }
  return x * fun(x, n-1)
}
```

## 函数表达式

在JavaScript中, 函数还可以使用表达式的方法去声明.

```JavaScript
var str = function(){
    // 代码块
}
str()
```

相比较于之前的声明方法, **函数表达式只有在代码执行到达的时候才会被创建, 并且仅从那一刻开始才可用**.

而**正常方法声明的函数会在JavaScript准备运行时, 首先把全局函数声明找到并创建**, 所以这也是为什么函数的声明在被定义之前就可以被调用.

## 回调函数

回调函数通俗可以理解为: **通过一个函数的参数去调用一个函数的过程**, 可以理解为函数的回调.

```JavaScript
// 现在有
function sum(a, b) {
  return a + b
}
function drop(a, b) {
  return a - b
}
function multiply(a, b) {
  return a * b
}
function divide(a, b) {
  return a / b
}
// 回调
function fun(a, b, f) {
  return f(a, b)
}
console.log(fun(1, 2, sum))
console.log(fun(1, 2, drop))
// ... ...
```

## 立即执行函数

立即执行函数表达的含义是一个函数定义完成后被立即执行, 实现方法如下:

```JavaScript
(function(){
	// 代码...
})()
```

立即执行函数和普通代码最大的区别是, 在立即执行函数中定义的变量是由自己的作用域的.

立即执行函数通常用于, 防止过多的变量命名冲突, 或者使用闭包在循环内部出啊关键一个新的作用域去避免外界访问.

## 函数的存储方法

函数的存储方法与对象相同. 函数内部的内容被存储在堆内存种, 并把地址赋值给栈内存种的函数的名称.

## ES6箭头函数

箭头函数是ES6引入的一种简洁的函数写法，可以更简便地定义匿名函数。

箭头函数的基本语法是：`(参数) => {函数体}`。

箭头函数有以下特点：

1. 省略了`function`关键字，使得语法更加简洁。

   ```JavaScript
   const greet = (name) => {
     console.log(`Hello, ${name}!`);
   };
   
   greet("Alice"); // 输出：Hello, Alice!
   ```

   

2. 当只有一个参数时，可以省略参数的括号。

   ```JavaScript
   const square = x => x * x;
   
   console.log(square(5)); // 输出：25
   ```

   

3. 当函数体只有一条语句时，可以省略花括号和`return`关键字。

注: 箭头函数与传统的函数表达式在某些情况下有一些细微的差异，例如箭头函数没有自己的`this`、`arguments`、`super`和`new.target`绑定。因此，在使用箭头函数时需要注意其上下文和特殊行为。
