# JavaScript函数增强！（上）

## 函数属性

已知：我们知道JavaScript中的函数也是一个对象，对象可以拥有自己的属性和方法。

```JavaScript
// 对象的使用方法：
var obj = {
  // ...
}
obj.message = "Meicko"
console.log(obj.message)
// out: Meiciko
```

```javascript
// 其实函数也可以
function fun() {
  // ...
}
fun.message = "Meiciko"
console.log(fun.message)
// out: Meiciko
```

不过不同的是，**函数创建时已经默认拥有了一些属性**，比如：

| 属性   | 值                                                           |
| ------ | ------------------------------------------------------------ |
| name   | 函数的名称；                                                 |
| length | 参数的个数，只获取形参的个数，也就是本来要接收的参数的个数，不会将剩余参数计算在内， （注：如果参数被设置了默认值（比如fun（name，age = 18）那么像age这样的被设置了默认值的参数也不会被计算在内）；==重要== |

### 关于剩余参数的补充

在ES6新增语法中，新填了剩余参数的写法（三个点加名称）：

实例：==function fun(name, age, ...other)==

```JavaScript
function fun(name, age, ...msg) {
  console.log("name:", name, "age:", age)
  console.log("msg:", msg, typeof msg)
}
fun("meiciko", 18, "额外参数1", "额外参数二", {name: "额外参数3"})
```

```JavaScript
输出结果如下：
name: meiciko age: 18
msg: (3) ['额外参数1', '额外参数二', {…}] length: 3[[Prototype]]: Array(0) object
```

传入函数的额外参数会放在以传入的名称命名的数组中。



## 函数的arguments

```javascript
function fun(x, y) {
	console.log(arguments)
}
fun(1, 2, 3, 4)
```

```
输出：
Arguments(4) [1, 2, 3, 4, callee: ƒ, Symbol(Symbol.iterator): ƒ]
```

虽然argument很想一个数组，但是**argument是一个类数组对象**！但是argument可任意通过索引获取对应的内容（比如：argument[0]），可以通过循环遍历，同时argument还是一个可迭代对象，可以使用迭代器遍历。

```JavaScript
function fun(x, y) {
	for (var arg of arguments) {
    console.log(arg)
  }
}
fun(1, 2, 3, 4)
```

```
out：
1
2
3
4
```

**再次提醒**：argument是一个类数组对象类型，意味着它不是一个数组类型，而是一个对象类型，但是它有一些数组的特性，比如length，比如可以通过索引访问，但是它没有一些数组的方法，比如filter，比如map等等。

### arguments转换为数组

**有很多应用场景需要将argument转成数组，目的是为了利用数组的一些特性完成某些操作或需求。**

下面是一些常见的转换方法：

| 方法                                                         | 解释                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 创建一个新的数组，并遍历arguments，将arguments中的每个元素添加到新的数组中。 | 通过遍历来转化。                                             |
| ==var newArray = Array.form(arguments)==                     | Array.form(arr)是ES6的语法，可以将一个**可迭代**的arr中的每一个元素同样赋值给新的数组。 |
| ==var newArray = [...arguments]==                            | 这种写法也是ES6的新语法，叫==展开运算符==。<br />写法是[...arr]，效果是对arr进行遍历，并且将遍历得到的元素放在新的数组里面。 |
| 调用slice方法：<br />==var newArray = [].slice.apply(arguments)== | slice方法可以对数组进行截取，比如slice(1,2)是从以1的索引所在元素开始到以2为索引的元素为止，不包含以2为索引的元素。<br />如果slice()没有传入参数，会默认从开始截取到结尾，相当于对数组进行了拷贝。<br />而当一个数组调用slice()的时候，相当于slice函数中的this指向了这个数组并进行了操作，所以可以使用apply函数去改变slice的this指向，将slice中的this指向arguments。<br />但因为是在方法中使用slice方法，而slice是数组对象的方法，函数体内是找不到slice的，所以需要使用一个空数组去调用slice。 |

### 箭头函数不绑定arguments

箭头函数并不绑定arguments。

```JavaScript
var fun = () => {
  console.log(arguments)
}
out: not defined // 没有找到，就像箭头函数没有this一样
```

正常来说，函数中没有找到某个变量会在上层作用域中寻找，如果我们做一个嵌套：

```javascript
function foo() {
  var bar = () => {
    console.log(arguments)
  }
  bar()
}
foo(1, 2)
```

上述代码可以拿到1和2，所以箭头函数中使用arguments会去上层作用域中查找。



## 函数的剩余（rest）参数

**ES6中引入了rest parameter，可以将不定数量的参数放入到一个数组中**。

（注：上文中已经对剩余参数进行了基本的介绍，接下来为详细介绍）

**如果最后一个参数是==...==为前缀的，那么它会将剩余的参数放在该参数中，并且作为一个数组**。

**==剩余参数必须放到最后一个位置，否则会报错！==**

使用reset的参数的原因，很多时候想要拿到函数的参数，会对arguments进行操作，但是arguments操作有些繁琐，而且会把形参页放进去，所以reset既可以拿到额外的不带形参的参数，本身还是一个数组，使用起来会非常方便。

```JavaScript
function fun(num1, num2, ...otherNums) {
  console.log(otherNums, typeof otherNums)
}

fun(20, 30, 40, 50, 60, 70)

out: (4) [40, 50, 60, 70] 'object'
```

同时，因为箭头函数不绑定arguments了，那么一定有一个东西替换掉了arguments的位置，这个东西就是rest parameters。

**当然，也可以将一个函数写成只有剩余参数的写法**。

```JavaScript
function fun(...args) {
  // ...
}
```

### 剩余参数和arguments的区别

1. 剩余参数==只包含那些没有对应形参的实参==，而==arguments对象包含了传给函数的所有实参==。
2. ==arguments对象不是一个真正的数组，而rest参数是一个真正的数组==，可以进行数组的所有操作。
3. arguments是早期ECMASciript中方便去获取所有参数提供的一个数据结构，而reset参数是ES6中提供并且希望以此来代替arguments的。



## 理解JavaScript纯函数

函数式编程中有一个非常重要的概念叫做**纯函数**，JavaScript符合函数式编程的规范，所以也有纯函数的概念。

在react开发中纯函数被多次提及。

### 什么是纯函数？

较抽象定义：在程序设计中，若一个函数符合以下条件，那么这个函数被称为纯函数。

- 此函数在==相同的输入值==时，需要产生==相同的输出==。
- 函数的输出和输入值以外的其他隐藏信息或状态无关，也和由I/O设备产生的外部输出无关。
- 该函数不能有语义上可观察的函数副作用，诸如“触发事件”，使输出设备输出，或更改输出值以外物件的内容等。

先理解第一个条件，简单来说就是无论外界环境怎么变化，一个在不同的外界环境下传入了相同的值，得到的结果是相同的：

```JavaScript
// 举一个反例
var num = 100
function sum(num1, num2) {
  return num1 + num2 + num
}

sum(10, 20) // out: 130

// 这时候改变了外界的环境
num = 500
sum(10, 20) // 此时函数还是传入了相同的值，但是结果不言而喻。
```

==相当于纯函数不能使用外层作用域的东西，不能使用闭包！==

再理解第二条，函数输出输入值以外的其他隐藏信息或状态无关，这一条和第一条很类似，不能和外界有关系。也和I/O设备产生的输出无关也是为了这一条件，比如一个键盘点击事件出发后输出被点击的按键等等都不是纯函数。

然后理解第三条，该函数不能有语义上可观察的函数副作用，可以这样说，==函数在执行过程中，不能有副作用==。

**==副作用本身是一个医学上的概念，但是计算机科学中的副作用表示在执行一个函数时，除了返回函数值以外，还对调用函数产生了附加的影响，比如修改了全局变量，修改参数或者改变了外部的存储==**。

 如下所示：

```JavaScript
function sum(x, y) {
  return x + y
} // 这个函数没有副作用

var obj = {
  name: "meiciko",
  age: 18,
  message: "..."
}

function printinfo(info) {
	if (!info.flag) {
      console.log(info.name, info.age, info.message)
  		info.flag = "已被打印"
  }
}
// 这个函数就产生了副作用，经过了这个函数，对象多了一样东西。
// 这样设计的目的是别人使用的时候不去改变别人的内容，是框架的一种设计哲学！
```

（注：副作用往往是产生bug的温床）



### 纯函数的案例

- slice：slice截取数组时不会对原数组进行任何操作，而是生成一个新的数组。

- splice：splice截取数组，会返回一个新的数组，也会对原数组进行修改。

上述的slice就是一个纯函数，不会改变数组的本身，但是splice函数不是一个纯函数。

使用纯函数的好处：

1. 可以放心的使用，因为不需要关心外层作用域中的值是什么状态会不会被改变，纯函数保持了函数的纯度，单纯的实现自己的业务逻辑。
2. 确定的输入得到确定的输出，且输入的内容不会被任意篡改。 
