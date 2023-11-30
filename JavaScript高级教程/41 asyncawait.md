# async/await

## 异步函数 async function

### 创建一个异步函数

创建一个异步函数需要使用关键字：==async==。

```js
async function fn() {
  console.log("this is a async function")
}
fn() // 输出: this is a async function
```

==async== 是 ==asynchronous== 的缩写，代表异步，非同步，创建一个异步函数还可以使用下面的写法，与普通函数的创建方法相同：

```js
const fn = async function() {} 
const fn = async () => {}
class 类名 {
  async fn() {}
}
```



### 异步函数的执行流程

异步函数的内部代码执行过程和普通的函数是一致的，==默认情况下也是会被同步执行的==。

异步函数有返回值的时候，和普通函数会有区别：

- ==情况一==：异步函数也可以有返回值，但是异步函数的返回值行党羽被包裹到了Promise.reslove中；
- ==情况二==：如果我们的异步函数的返回值是Promise，状态会由Promise决定；
- ==情况三==：如果我们的异步函数的返回值是一个对象并且实现了thenable，那么会有对象的then方法来决定；



### 异步函数返回值



#### 情况一：返回一个正常值或者undefined

首先定义一个异步函数：

```js
async function fn() {
	
}
console.log(fn())
```

==情况一==：如果内部==没有返回任何任何值==，它会返回一个Promise:

```
Promise
[[Prototype]]: Promise
[[PromiseState]]: "fulfilled"
[[PromiseResult]]: undefined
```

==情况一==：如果内部==返回了一个普通的值==，相当于进行了Promise.reslove操作

```js
async function fn() {
	return "hellow world"
}
console.log(fn())
```

```
Promise {<fulfilled>: 'hellow world'}
```

如果进行正常的Promis操作：

```js
async function fn() {
	return "hellow world"
}
fn().then(res => {
  console.log("进行了then操作：", res)
})
// 输出：进行了then操作： hellow world
```

也就是说，它依然把值包装为了一个Promise。

（==注：异步函数与普通函数最大的区别就在于返回值的区别==）



#### 情况二：返回一个Promise

==情况二==：如果内部返回的值是Promise，那么状态会由Promise来决定。

先定义一个异步函数：

```js
async function fn() {
	return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("return a Promise")
    }, 3000)
  })
}
fn().then(res => {
  console.log(res)
})
// 三秒后，控制台输出：
// return a Promise
```



#### 情况三：返回了一个thenable

先定义一个异步函数：

```js
async function fn() {
	return {
    then: function(resolve, reject) {
      setTimeout(() => {
        resolve("return a thenable")
      }, 3000)
    }
  }
}
fn().then(res => {
  console.log(res)
})
// 三秒后，输出：
// return a thenable
```



## 异步函数的异常处理

在异步函数的执行流程中，它的执行顺序如下所示：在调用 fn 函数的时候，会拿到一个 Promise，来调用 then 或者 catch。

但是在这个代码的执行过程中，Promise 是有三种状态的，就==需要考虑什么时候会变成 reject 的状态==：

如果在异步函数的执行的过程中，突然出现了某些错误，使得代码抛出了一个异常，出现报错，会进行如下操作：

- 拿到报错，会进行 ==Promise.reject("err message")== 的操作，可以通过在外面使用 catch来捕获

```js
async function fn() {
	return new Promise((resolve, reject) => {
		throw new Error("err message")
  })
}
fn().then(res => {
  console.log(res)
}).catch(err => {
  console.log("Error:", err)
})
// 输出：
Error: err message
    at 1.html:12:9
    at new Promise (<anonymous>)
    at fn (1.html:11:9)
    at 1.html:15:1
```

所以，==如果在 async 中抛出了异常，那么程序它并不会像普通函数一样报错，而是作为 Promise 的 reject 来传递==；

在浏览器中出现报错是一个非常危险的情况，因为会阻塞后续代码的运行，但是这种方式可以解决因为错误而无法使得后续代码运行的问题。



# await

## await 的使用条件

在异步函数中，是可以使用关键字：==await== 的，==await 只可以在异步函数中使用，而不能再普通函数中使用==。



## await 的作用

==await== 关键字有什么特点呢？

- 通常情况下==使用 await 是后面跟上一个表达式==，==这个表达式会返回一个 Promise==；
- 那么 await 会==等回到 Promise 的状态变为 fulfilled 状态==，==才会继续执行异步函数==；

所以结合上述两种特性，可以知道 await 经常用于去管理一个函数，在它后面直接写一个值是没有意义的：

```js
function foo() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve("success message")
    }, 3000)
  })
}
async function fn() {
  console.log("--------------")
  const res = await foo()
  console.log(res)
  console.log("--------------")
}
fn()
```

```js
-------------- // 在这里等待了三秒，相当于做了一次yield操作，将程序暂时暂停掉了，（其实可以理解为是 yield 演化过来的）
success message
--------------
```

总之，在异步函数中使用 await 后，程序会进入等待的状态，所以其实可以把这种代码理解为，生成器和 Promise 一起使用的写法。



## 使用案例

==await 遇到异常的时候，会把这个异常抛出去，作为整个异步函数的 Promise 的异常==。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <script>
    const fist_url = "http:\\ ....."

    function requestData(msg) {
      return new Promise((resolve, reject) => {
        setTimeout(() => {
          resolve({ message: "success message", data: msg })
        })
      })
    }

    async function getData() {
      const first_res = await requestData(fist_url)
      const second_res = await requestData(`url2?param=${first_res.data}`)
      const res = await requestData(`url3?param=${second_res.data}`)
      return res
    }

    getData().then(res => {
      console.log("res:", res)
    }).catch(err => {
      console.log("error:", err)
    })
  </script>
</body>
</html>

```

await 后面也可以写上异步函数，并且可以等待 Primise 的结果。



 

