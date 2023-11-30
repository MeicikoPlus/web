# JS函数this指向

------



## this的一些绑定现象

### 方式一: 普通调用(默认调用)

```JavaScript
function fun() {
  console.log("当一共函数以最普通的方式调用的时候, this指向:", this)
}

fun()
// 输出: 当一共函数以最普通的方式调用的时候, this指向: Window
```

### 方式二: 通过对象调用

```JavaScript
// 承接上文
let obj = {
	name: "meiciko",
	fun: fun
}

obj.fun()
// 输出: 当函数以对象调用的时候, this指向: {name: 'meiciko', fun: ƒ}
```

### 方式三: 通过call()或者apply()去调用

```JavaScript
// 承接上文
let newobj = {
  name: "Meiciko"
}

obj.fun.call(newobj)
// 输出: 当函数以对象调用的时候, this指向: {name: 'Meiciko'}
```

可以总结:

1. 函数在被调用的时候, JavaScript会**默认给this绑定一个值**.

2. **this的绑定和定义的位置(编写的位置)没有关系**.

3. this的绑定和**调用方式**以及**调用的位置**有关系.

4. this是**在运行时**被绑定的.

   

------

## this的绑定规则

### 第一种: 默认绑定

默认绑定可以理解为**函数独立调用** (独立函数可以理解为没有被绑定到某个对象上进行调用的函数)

```JavaScript
// H2种的函数的一些调用现象的第一种就是函数独立调用
// 这里再举出一共例子

// 函数定义在对象种, 但是独立调用
let obj = {
  name: "meiciko"
  fun: function() {
    console.log("obj对象内的this指向:", this)
  }
}

let fun2 = obj.fun
fun2() // 输出: 指向window对象
// fun2()函数是独立调用, 没有由别的对象产生相对其的调用.
// ---------------------------------------------------

// 函数内调用函数也是默认绑定
function test1() {
  console.log(this)
  test2()
}
function test2() {
  console.log(this)
  test3()
}
function test3() {
  console.log(this)
  test1()
} // 这种也是默认调用
// ----------------------------------------------------

// 高阶函数
let obj = {
  name: "meiciko"
  fun: function() {
    console.log("obj对象内的this指向:", this)
  }
}
function test(fun){
  fun() // 在这里任然是独立调用, 根据js的内存原理, 其实test函数内部又创建了一个fun()函数被赋值给obj.fun的地址, 相当于一个副本.
}
test(obj.fun)
```

如果进入严格模式, 默认情况下的调用会指向undifined.

```JavaScript
"use strict" // 在js代码第一行加入这句话, 代表js代码进入严格模式, 可以避免很多低级错误
// 除了"use strict"内容同上个代码块
// H2种的函数的一些调用现象的第一种就是函数独立调用
// 这里再举出一共例子

// 函数定义在对象种, 但是独立调用
let obj = {
  name: "meiciko"
  fun: function() {
    console.log("obj对象内的this指向:", this)
  }
}

let fun2 = obj.fun
fun2() // 输出: undifined
```



### 第二种: 隐式绑定

>  理解什么是隐式绑定什么是显示绑定的时候可以参考笔记[JavaScript基础教程]中的JavaScript基本数据类型转换中的显示转换和隐式转换, 两者的区别就在于一个是JavaScript引擎自动进行对this的绑定, 一个是自身控制this的绑定.

隐式绑定常见的调用方式是**通过某个对象进行调用**的, 也就是它的调用位置种, 是**通过某个对象发起的函数调用**.

```JavaScript
function fun() {
  console.log("this指向:", this)
}

var obj = {
  fun: fun
}

console.log(obj.fun)
/*
输出结果:
ƒ fun() {
  console.log("this指向:", this)
}
*/

// 在此基础上进行测试
obj.fun()
// 输出: this指向: {fun: ƒ} [object]
```

```JavaScript
function fun() {
  console.log("this指向:", this);
}

let obj1 = {
  name: "meiciko",
  fun: fun
}

let obj2 = {
  name: "Meiciko",
  obj1: obj1
}

obj2.obj1.fun()
// 输出: this指向: {name: 'meiciko', fun: ƒ}
// this绑定于调用自己的对象相关.
```



### 第三种: 显示绑定

#### apply()和call()

对于显示绑定, 可以先从一个问题引入:

```JavaScript
function fun() {
  console.log("this指向:", this)
}

let obj = {
  name: "meiciko"
}
// 问题: 我想要让fun()的this绑定到obj上
// 这时候独立调用是不能的, 一般来说可能会给obj创建一个新的属性并赋值为fun, 再使用obj.fun()
// 但是为了给fun的this进行定向的绑定, 做了这么多操作会很麻烦, 而且还改变了对象的内部结构(多了一个fun)
// 新的方法:
fun.call(obj) //不能使用fun().call(obj), 否则相当于使用函数的返回值进行call操作
// 或者使用apply(), 两者的区别很小
fun.apply(obj)
// 输出: this指向: {name: 'meiciko'}
```

当使用 方法.call("对象") 时, call函数的左右就是将**调用其的函数的this强制指向参数内部的对象**, **当call()参数不是一个对象的时候, 会进行一次包装**, 把它包装成包装类对象, 如果对象无法包装(比如传入undifined或者null), 就会指向window对象.

```JavaScript
function fun() {
  console.log("this指向:", this)
}
fun.call(123)
// this指向: Number {123}
// 如果想要更加深刻理解这个包装:
function test() {
  console.log(this.length)
}
test.call("meiciko")
// 输出: 7
```

**JavaScript中所有函数都可以使用call和apply方法**, 两个方法的第一个参数是相同的, 都**要求传入一个对象** , 之后的参数, apply是数组, call为参数列表. 

```JavaScript
function fun(name, age, id) {
	console.log("fun函数this指向:", this)
}
let obj = {}
// 正常情况下的调用
fun("meiciko", 18, 114514)

// 当我们使用obj调用call函数的时候, fun()内部的参数可以写在call中
fun.call(obj, "meiciko", 18, 114514)
// apply()的第二个参数也可以传参, 但是是以数组的方式.
fun.apply(obj, ["meiciko", 18, 114514])
```

 

#### bind()

```javascript
// 先看案例
function fun() {
	console.log("this指向:", this)
}

let obj = {}
fun() // 输出: this指向: window
let test = fun.bind(obj) // 本质上是生成了一个新函数, 效果是这个新函数绑定了obj
test() // 输出: this指向: {}
```

使用bind(), 会创建一个**新的绑定函数**.

绑定函数是一个**怪异函数对象**. (ECMAScript 2015 中的术语)

bind()的参数问题:

```JavaScript
function fun(name, age, id, address) {
	console.log("this指向:", this)
  console.log(name, age, id, address)
}

let obj = {}
let test = fun.bind(obj, "meiciko", 18, 114514)
test("henan")
/*
this指向: {}
meiciko 18 114514 henan
*/
```



 

### 第四种: new绑定

JavaScript中函数也可以当作一共类的构造函数去使用, 也就是使用new关键字.

```JavaScript
function createstu(name, age, id) {
  this.name = name
  this.age = age
  this.id = id
  this.fun = function() {
    console.log("this指向:", this)
  }
  console.log("构造函数this指向:", this)
}

let stu1 = new createstu("meiciko", 18, 114514)
// 输出: 构造函数this指向: createstu {name: 'meiciko', age: 18, id: 114514, fun: ƒ}
stu1.fun()
// 输出:this指向: createstu {name: 'meiciko', age: 18, id: 114514, fun: ƒ}
```

使用new关键词来调用函数的时候会进行以下操作:

1. 创建一个全新的对象.

2. 这个新对象会被执行prototype连接;

3. 这个新对象会被绑定到函数调用的this上(this的绑定在这个步骤完成).

4. 如果函数没有返回其它对象, 表达式就会返回这个对象.

   

------

## this的绑定规则优先级

1. 默认规则的优先级是最低的, 因为存在其它规则的时候, 就会通过其它规则来绑定this.

2. 显示绑定高于隐式绑定

```javascript
let obj = {
	fun: function() {
		console.log("this指向:", this)
	} 
}

obj.fun.call("String")
// 输出: this指向: String
```

3. new绑定优先级高于隐式绑定

```JavaScript
let obj = {
  fun: function() {
    console.log("this的指向:", this)
  }
}

new obj.fun()
// 输出: this的指向: {}
// 指向了一个新对象.
```

4. (new和call, apply不会一起使用, 两者没有可比性), new和bind的优先级相比, new的优先级要更高

```JavaScript
function fun() {
  console.log("this指向:", this)
}

var newfun = fun.bind("String")
new newfun()
// 输出: this指向: {}
// this指向了一个空对象, 可以得出new优先级是高于bind的
```





------

## this规则之外 -- 忽略显示绑定

如果在显示绑定中, 我们传入一共null或者undefined, 那么显示绑定会被忽略, 使用默认规则.

```JavaScript
function fun() {
  console.log("this指向:", this)
}

let obj = {
  fun: fun
}

fun.call(undefined) // 输出: this指向: window
obj.fun.call(undefuned) // 输出: this指向: window
```

## this规则之外 -- 间接函数引用

创建一个函数的间接引用, 这种情况使用默认绑定规则.

```JavaScript
let obj1 = {
  name: "meiciko",
  fun: function() {
    console.log("this指向:", this)
  }
}

let obj2 = {
  name: "Meiciko"
}

obj2.fun = obj1.fun
obj2.fun() // 输出: this指向: {name: 'Meiciko', fun: ƒ}
// 此时指向obj2

// 如果这样写: (实际编程不要这样写(垃圾代码), 仅仅是为了测试)
let result = (obj2.fun = obj1.fun)
result()
// 输出: this指向: window
```

(obj2.fun = obj1.fun)这类赋值的结构是fun函数, fun函数被直接调用那么是默认绑定.



------

## this规则之外 -- 箭头函数

箭头函数是没有this的, 箭头函数也没有有自己的作用域, 但是会继承它的定义上下文作用域。.

```javascript
let fun = () => {
	console.log(this)
}
// 调用这个方法后, 不能打印 this, 因为箭头函数没有独立的作用域，它会继承它被定义的上下文作用域中的 this
```

无论如何调用这个函数，箭头函数中的 `this` 值都是继承自父级作用域，而不是在上层作用域中找到的。

