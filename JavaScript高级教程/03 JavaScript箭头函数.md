# **JavaScript箭头函数(arrow function)**

## 什么是箭头函数

箭头函数是ES6之后新增的一种编写函数的方法, 并且它比函数表达式更加简洁.

## 需要注意的是: 

1. 箭头函数**不会绑定this, arguments属性**.
2. 箭头函数**不能作为构造函数来使用** ( 不能和new一起来使用, 会报错 ).

```javascript
let fun = function(name, age) {
  ... ...
}
// 上下对比
let fun = (name, age) => {
 	... ...
} // 箭头函数的完整写法(非简写)
```

```javascript
let arr = ["meiciko1", "meiciko2", "meiciko3"]
arr.forEach(function(i) {
  console.log(i)
})
// 箭头函数写法:
arr.forEach((i) => {
  console.log(i)
})

// 也可以用在定时器中
setTimeout(() => {
  ... ...
}, 5000)
```



## 箭头函数的编写优化

1. 当括号中的参数只有一个的时候, 括号可以省略.

```JavaScript
// 箭头函数的简写
let arr = ["meiciko1", "meiciko2", "meiciko3"]
arr.forEach((item) => {
  console.log(item)
})
// 上述只用到了一个参数(只有一个参数可以理解为只用到了一个参数)
arr.forEach(item => {
  console.log(item)
})

// 再看一个例子:
let arr = [100, 24, 34, 45, 93, 74, 9, 55]
let numbers = arr.filter(item => {
  return item % 2 === 0
})
console.log(numbers)
```

2. 如果函数体只有一行代码, 那么{}也可以省略 (如果有return的话不能省, 如果省略需要带上return一起省略).

```JavaScript
let arr = ["meiciko1", "meiciko2", "meiciko3"]
// 还是上述列表
arr.forEach(item => {
  console.log(item)
})
// 上述函数其实还可以再变为:
arr.forEach(item => console.log(item))
```

3. 如果函数内容只有一行代码, 那么这行代码的表达式结果将自动作为整个函数的返回值.

```JavaScript
let arr = [100, 24, 34, 45, 93, 74, 9, 55]
let newarr = arr.filter(item => item % 2 === 0)
console.log(newarr)
// 输出: (4) [100, 24, 34, 74]
```

4. **如果我们的默认返回值是一个对象, 那么这个对象必须加小括号.**

```javascript
let arrfun = () => 1
console.log(arrfun())
// 输出: 1
let arrfun = () => [1, 2]
console.log(arrfun())
// 输出: [1, 2]
let arrfun = () => ({name: "meiciko"})
console.log(arrfun())
// 必须加小括号才能正常输出, 
```



## 箭头函数练习

```javascript
// 实现数组中所有偶数的平方的和
let arr = [20, 30, 11, 15, 111]
let numbs = arr.filter(item => item % 2 === 0).map(item => item ** 2).reduce((before, after) => before + after, 0)
console.log(numbs)
```



   

## 箭头函数的应用(利用箭头函数的this原理, 可从[02 this指向原则]中查看)

箭头函数可以用于对网页请求的数据的接收赋值.

```javascript
function request(url, callback) {
	let result = ["Meiciko", 18, 114514]
	// 假设上述是得到的返回值
	callback(result)
}

let obj = {
	name: "",
	age: 0,
	id: 0,
	network: function() {
		request("目标请求的地址", (result) => {
			// 因为箭头函数没有this, 所以this会直接前往上层作用域寻找, 而上层作用域就是obj对象.
      this.name = result[0]
			this.age = result[1]
			this.id = result[2]
		} )
	}
}

obj.network()
console.log(obj)
```

