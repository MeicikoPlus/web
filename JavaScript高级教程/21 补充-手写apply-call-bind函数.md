# 手写apply-call-bind函数

```js
function fn() {
  console.log(this)
}

fn.apply("abc")
```

其实仔细观察机会发现，fn函数对apply方法的调用是（.）也就是说是把fn当成了一个对象去调用一个方法，也就是说**`apply这样的函数是写在Function的显示原型中的`**`。 

```js
console.log(Function.prototype.apply === fn.apply) // true
```



# 手写Apply

## 第一步：

这是一个简单的办法: 

```js
var obj = {name: "meiciko", age: 18}

// 现在有一个函数
function foo() {
  console.log(this.name, this.age)
}

// 手写一个apply的方法
Function.prototype.myApply = function(thisArg) {
  // 将这个参数对象的fn属性赋值给this（也就是foo函数）
  thisArg.fn = this
  // 在使用这个参数对象去调用fn函数，这样这个fn函数中的this指向的是thisArg这个对象
  thisArg.fn()
  
  delete thisArg.fn
}

// 调用函数
foo.myApply(obj)
```

但是上述的情况并没有考虑到下面的这些效果：

```js
foo.myApply(123)
foo.myApply(null)
```

所以还是需要再次改进！



## 第二步：

相比于第一步，多了一个对null和undefined的判断。

```js
var obj = {name: "meiciko", age: 18}

function foo(name, age) {
  console.log(this.name, this.age)
}

Function.prototype.myApply = function(thisArg) {
  // 在这里多了一个对于空和未定义的判断
  thisArg = (thisArg === null || thisArg === undefined) ? window : Object(thisArg)
  
	Obejct.defineProperty(thisArg, "fn", {
    enumerable: false,
    configurabel: true,
    value: this
  })
  thisArg.fn()
  
  delete thisArg.fn
}

foo.myApply(obj)
```





## 第三步，处理参数

之前的步骤再优化，就是对于参数的处理：

```js
var obj = {name: "meiciko", age: 18}

function foo(name, age) {
  console.log(this.name, this.age)
}

Function.prototype.myApply = function(thisArg, otherArgs) {
  thisArg = (thisArg === null || thisArg === undefined) ? window : Object(thisArg)
  
	Obejct.defineProperty(thisArg, "fn", {
    enumerable: false,
    configurabel: true,
    value: this
  })
  // 使用展开运算符
  thisArg.fn(...otherArgs)
  
  delete thisArg.fn
}

foo.myApply(obj)
```





# 手写Call

思路同上，只需要注意一下展开运算符运用的位置：

```js
var obj = {name: "meiciko", age: 18}

function foo(name, age) {
  console.log(this.name, this.age)
}

// 展开运算符
Function.prototype.mycall = function(thisArg, ...otherArgs) {
  thisArg = (thisArg === null || thisArg === undefined) ? window : Object(thisArg)
  
	Obejct.defineProperty(thisArg, "fn", {
    enumerable: false,
    configurabel: true,
    value: this
  })
	// 展开运算符
  thisArg.fn(...otherArgs)
  
  delete thisArg.fn
}

foo.mycall(obj)
```





# 对重复逻辑的封装

对于相同的东西进行封装，对于不同的东西以参数的形式传入。

注意：对相同的逻辑要养成封装的好习惯，但是不需要过度抽取。

```js
function execFn(thisArg, otherArgs) {
    thisArg = (thisArg === null || thisArg === undefined) ? window : Object(thisArg)

    Obejct.defineProperty(thisArg, "fn", {
      enumerable: false,
      configurabel: true,
      value: fn
    })
    // 展开运算符
    thisArg.fn(...otherArgs)

    delete thisArg.fn
	}
}
```

```js
Function.pototype.myapply = function(thisArgs, otherrgs) {
  this.exeFn(thisArg, otherArgs)
}
```

