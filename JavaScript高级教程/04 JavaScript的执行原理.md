# **JavaScript执行原理**

## 执行上下文的创建

```JavaScript
var message = "Global Message"

function fun() {
  var message = "Fun message"
}

var num1 = 10
var num2 = 20
var result = num1 + num2
console.log(result)
```

上述是一段简单的代码, 那么这些代码是如何执行的呢?

### 执行前 -- 初始化全局对象

JavaScript引擎会在==执行代码之前==, 会先在==堆内存==中创建一个==全局对象==: `Global Object (GO)`

(补充: 全局对象其实就是window对象)

- 该对象==所有作用域都可以访问==.
- 对象内包含Date, Array, String, Number, setTimeout, setInterval等等.
- 其中还有一个==window属性指向自己==.

(补充: GO对象创建时也会初始化一些对象, 例如String, Number ... ... document, history ... ...)

### 执行前 -- 执行上下文

JavaScript引擎中有一个==执行上下文栈==(ECS), 它是用于管理执行上下文的栈结构, 而这个引擎要执行的是全局的代码块 (注: 这个栈指的是一个栈结构(数据结构栈)).

**任何代码的执行都会创建一个叫执行上下文的东西, 而当要执行全局代码块的时候, 会创建一个Global Execution Context (GEC), 即全局执行上下文.**

代码会==被放入执行上下文当中==, 然后执行具体的代码.  从逻辑上来说, 多个执行上下文会形成一个栈结构, 而最顶部的执行上下文一定正在执行的执行上下文.



> 栈结构: 简单描述以下就是, 一个栈(比喻成一个容器), 当有一个内容存放进去的时候, 会直接来到栈底, 再放一个东西后, 会紧接着叠加在上面, 可以想象为一个瓶子.



### 执行前 -- 认识VO对象 Variable Object (变量对象)

**每一个执行上下文会关联一个VO对象, 变量和函数声明会添加到这个VO对象中, 作为属性**.

```javascript
var message = "Global Message"

function fun() {
 var message = "Fun message"
}

var num1 = 10
var num2 = 20
var result = num1 + num2
console.log(result)
```

在执行上述代码的过程==前==, 会将message, fun, num1... ...这样的变量保留添加到GO对象(作为属性, 如果函数有参数也会被添加)中, 但是==并未赋值(undefined)==.

当全局代码被执行的时候, VO就是GO对象,    ==同时this也会指向GO==.

如果检测到是一个函数, 会被赋值给一个内存地址, 也就是为什么函数可以被提前访问的, 而变量提前访问会是undefined.



### 执行中 -- 代码执行过程

这时候拿出来我们的代码:

```JavaScript
var message = "Global Message"

function fun() {
  var message = "Fun message"
}

var num1 = 10
var num2 = 20
var result = num1 + num2
console.log(result)
```

结束上述所说的几个步骤后, JavaScript代码开始从上到下依次执行, 

- 第一步: 会把Global Message赋值给message.
- 第二步: 将10赋值给num1.
- 第三步: 将20赋值给num2.
- 第四步: 将num1与num2相加的结果赋值给result.
- 第五步: 打印result.

在代码执行到函数定义之前，函数的声明已经被提升了，但是函数体内部的代码并没有执行，只有在实际调用函数时，函数体内的代码才会执行。

如果这个时候，在原本的代码基础上添加一句：

```JavaScript
var message = "Global Message"

function fun() { // 这里
  var message = "Fun message"
  var age = 18
  var height = 1.88
  console.log("fun function")
}

fun() // 这里

var num1 = 10
var num2 = 20
var result = num1 + num2
console.log(result)
```

这时候JavaScript的执行情况如下所示：

- 将已经创建好的message赋值：Global Message。

- 执行fun函数前，创建一个新的执行上下文，已知一个执行上下文创建成功的时候会关联一个VO对象， 所以这个新的上下文会关联一个新的VO，这个VO对象里面会存放一些新的属性。

  值得注意的是，在执行的过程中遇到==执行一个函数==，会根据==函数体==创建一个==函数执行上下文==，简称FEC， 并且压入到ECS中。

  所以可以总结出：==每个执行上下文都会关联一个VO，而函数在执行前也会关联一个上下文关联的VO在函数执行时变量对象（VO）会转变为活动对象AO（Activation Object）并用于存储函数的参数、局部变量以及函数内部声明的变量和函数。==
  
  ==可以说变量对象（VO）是在进入函数上下文之前的概念，而活动对象（AO）是在函数实际执行时的概念。它们在功能上相似，但术语上有所不同，表示了不同的状态。==

​		这个AO对象会使用arguments作为初始化，并且初始值是传入的参数。

​		这个AO会作为函数执行上下文的所关联的VO来存放变量的初始化。

- 执行fun函数，将函数内部创建的message赋值为Fun Message，将18赋值给age，将1.88赋值给height，打印fun funcation。
- ... ...

当再次添加一些改动：
```javascript
var message = "Global Message"

function fun(num) { 
  var message = "Fun message"
  var age = 18
  var height = 1.88
  console.log("fun function")
}

fun(1) // 这里
fun(12) 
fun(123)
fun(1234)

var num1 = 10
var num2 = 20
var result = num1 + num2
console.log(result)
```

当上述代码执行的时候：

- 将已经初始化好的message赋值。

- 当执行到第10行的时候，给fun函数创建一个AO对象，AO对象有argument对象，用来存放初始化的边变量。

- 将argument中的num赋值为1，将... ...1.88赋值给height。

- 执行完第10行的fun(1)后，==fun(1)的执行上下文会被弹出清理==，此时执行上下文栈中最顶层的执行上下文重新变成全局执行上下文（GEC）。

- 然后开始执行fun(12)，函数第二次将要被执行，此时会新建一个AO，**注意是新建，因为之前第十行执行的函数的虽然和第11行执行的是一个函数，但是上个AO关联的是第十行的函数的执行上下文，而第十行函数的执行上下文已经被弹出清理，所以第十行函数的执行上下文大概率会垃圾回收**。

  而第11行的函数会新建一个执行上下文并且新建关联的AO，步骤同第十行的函数执行步骤。

- 执行完第11行函数后，这个函数的执行上下文弹出，执行下一行，而因为下一行虽然也是这个函数，但是也会创建一个新的AO。



==注意：函数声明（Function Declaration）会被提升，而函数表达式（Function Expression）不会被完全提升，但是函数名在当前作用域内会被提升，具体行为还与函数表达式的具体形式有关。==



## 变量查找的作用域

当要寻找一个变量的时候，会**优先从自己的VO里寻找**.

```javascript
console.log(message) // undefinded
var message = "meiciko"
console.log(message) // meiciko
```

在上述代码中，代码先创建了自己的GO并且在执行上下文栈中创建了执行上下文，当开始运行的时候，先关联了自己的VO也就是GO，然后执行第一行，需要寻找message，此时在自己的VO中发现为undefined。

然后是赋值操作，赋值结束后会再次寻找message，此时优先从自己的VO中寻找，发现为meiciko。

### 作用域和作用域链

==当进入到一个执行上下文时，执行上下文也会关联一个作用域链==。

==作用域链是一个对象列表==，用于变量标识符的求值。

==当进入一个执行上下文时==，这个作用域链被创建，==并且根据代码类型，添加一系列的对象==。

在ES5之前，常见代码类型有两种，一种是全局代码，一种是函数代码。

当是全局代码的时后，作用域链就是GO（可以简单理解为，每个执行上下文都会关联一个列表，当这个执行上下文是全局的时候，列表中只有GO）。

如果是函数的话，==函数被创建的时候，作用域链也会确定下来==，==作用域链只与函数创建的位置有关==，当函数进入执行上下文的时候，会将函数内的作用域链赋值给上下文关联的作用域链。

下面用一个函数嵌套多层来理解：

```JavaScript
var message = "Global Message"

function foo() {
  var name = "meiciko"
  function bar() {
    console.log(name)
    function test() {
      console.log(message)
    }
    return test
  }
  return bar
}

var bar = foo()
var test = bar()
test()
```

多层嵌套的查找顺序如下所示：

1. 在JavaScript代码执行之间，会先初始化一个GO对象，这个GO对象中已经有message但是未赋值，同时，还有一个函数对象，这个函数对象对应的是foo函数，需要注意的是，**函数内部的函数并不会被初始化一个函数对象**！**只会先创建最外层的函数对象**。

2. 函数对象中有一个叫[[scopes]]的列表来保存函数的作用域链，这个作用域链的第一位就是Global，因为列表在JavaScript中也是一个特殊的对象，所以scopes相当于在内存中又创建了一个对象。

3. 当代码运行到第十三行的时候，会创建一个新的执行上下文，这个执行上下文其实是foo的执行上下文，他会关联一个自己的VO，因为foo是函数，所以关联的VO也是AO，在foo函数关联的AO中，初始化了一个bar函数对象，而这个bar的自己的scopes（作用域链）如下所示：

   scopes[ ]：[

   ​	0：Closure（foo）：foo所对应的AO对象。

   ​	1：Global：全局作用域。

   ]

4. 当执行到第16行的时候，也会创建一个bar函数关联的AO，此时需要打印name，会现在自己的VO中查找，发现没有后会在上述提过的scopes中查找，即查找foo的AO对象，此时foo的AO对象中有name，就会打印此name。
5. 执行完打印后，会初始化一个test的函数，这个函数也有自己的scopes，可以推算出它的作用域链为bar函数的AO，foo函数的AO，Global。
6. 当test函数要查找message的时候，会现在自己的AO中查找，发现没有后会前往作用域链中的第一项，bar函数的AO，发现没有后会前往foo函数的AO，最后查找Global，发现message并且打印。  



###  作用域测试题

```JavaScript
// 面试题一
var n = 100
function fun() {
  n = 200
}
fun()
console.log(n) 
// 问n会打印出来什么？答案：200
```

上述中的n不是在函数体内部定义的，而是直接访问的，会根据作用域去寻找。

 ```JavaScript
 var n = 100
 function fun() {
   console.log(n)
   var n = 200
   console.log(n)
 }
 fun()
 ```

第一次会打印undefined，第二次会打印200。

```JavaScript
var n = 100
function foo() {
  console.log(n)
}
function fun() {
  var n = 200
  console.log(n)
  foo()
}

fun()
```

会打印一次200，一次100。

需要注意的是，作用域与定义的位置有关，与调用的位置无关。

```JavaScript
var a = 100
function fun() {
  console.log(a)
  return
  var a = 100
}

fun()
```

结果为undefined。

这一题取决于函数会不会在return后面创建a，结果是会创建，因为创建操作是VO在代码==执行之前==开始的，而return是在==代码执行==的时候生效的，所以只要定义过就会存在。

```JavaScript
// 注：JavaScript中有一种错误的写法：
message = "meiciko"
// 但是JavaScript引擎也可以解析，但是会把他变为全局作用域，所以有时会出现下面这种情况：
function fun() {
  message = "meiciko"
}
fun()
console.log(message) // 依然能拿到，但是这什一种错误的写法！
```

```JavaScript
function fun() {
  var a = b = 100
}
fun()
console.log(a) // 访问出错，a并没有被定义。
console.log(b) // 访问成功，因为b被JavaScript被解析为了全局变量。
```



### 问题一：创建那么多对象，会不会把内存耗尽？

如果一直创建不销毁，内存必然会耗尽。

### 问题二：为什么要设计的这么麻烦？为什么这样设计？

这样设计最主要的目的就是为了==闭包==。

上述两个问题开一个新栏说明。
