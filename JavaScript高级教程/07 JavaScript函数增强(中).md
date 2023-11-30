# JavaScript函数柯里化

## 柯里化概念理解

柯里化也是属于函数式编程中一个很重要的概念，是一种关于函数的高阶技术，它不仅被用于JavaScript，也被用于其他的编程语言。

较抽象的解释如下：

- 在计算机科学中，柯里化（Currying）又译为卡瑞化或者加里化；
- 是把接收==多个参数的函数==，变成==接收一个单一参数==（最初函数的第一个参数）的函数，并且==返回接收余下的参数==，而且==返回结果的新函数==的技术；
- 柯里化声称：“==如果你固定某些参数，你将得到接受余下参数的一个函数==”；

举例说明：

```JavaScript
// 现在有这样一个函数, 它接收三个参数
function fun(x, y, z) {
  // ...
}

// 现在我们把这个函数变为
function foo1(x) {
  return foo2(y){
    return foo3(z) {
      // 对xyz进行操作
    }
  }
}
// 格式可能不对，但大概就是这个意思。
foo1(x)(y)(z)
```

上述这样的过程就是柯里化，用不抽象的方法去解释的话，就是：

- ==只传递给函数一部分参数来调用它，让他返回一个函数去处理剩余的函数==；
- ==这个过程就叫做柯里化==。

柯里不会调用函数，它只是对函数进行转换。



## 一个案例

```JavaScript
function foo(x, y, z) {
	console.log(x + y + z)
}
foo(10, 20, 30) // out: 60
```

上述是一个正常的普通的函数，但是如果想以==foo(10)(20)(30)==的形式调用，此时会报错，因为foo不是一个柯里化的函数所以目前不能这样调用，但是如果想要变成这样的调用方式：

```JavaScript
function foo(x) {
  return function(y) {
    return function(z) {
      console.log(x + y + z)
    }
  }
}
foo(10)(20)(30) // out: 60
```

上述就是对函数进行了转化的过程柯里化的过程，而这个过程中生产的函数就是柯里化函数。

使用箭头函数优化一下上述代码：

```JavaScript
// 第一次优化
function foo(x) {
  return y => {
    return z => {
      console.log(x + y + z)
    }
  }
}
// 第二次优化
function foo(x) {
  return y => {
    return z => console.log(x + y + z)
  }
}
// 第三次优化
function foo(x) {
  return y => z => console.log(x + y + z)
}
// 第三次优化
var foo = x => {
  return y => z => console.log(x + y + z)
}
// 第四次优化
var foo = x => y => z => console.log(x + y + z)
// 当然，想要方便辨识可以这样：
var foo = x => y => z => {
  console.log(x + y + z)
}

// 调用
foo(10)(20)(30)
```



