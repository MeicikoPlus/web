# Promise

在 `JavaScript` 中，`Promise` 是一种用于处理异步操作的机制，它可以更优雅地处理回调地狱`（Callback Hell）`问题，使异步编程更加清晰和可读。

回调地狱`（Callback Hell）`是指在异步编程中，多个嵌套的回调函数形成的一种不可维护和难以理解的代码结构。这种情况通常在处理多个异步操作，特别是在它们之间有依赖关系或顺序关系时出现。

在 `JavaScript` 早期，处理异步操作通常通过回调函数来实现，但当多个异步操作需要嵌套时，代码会变得非常复杂，难以阅读和维护。这种情况下，每个回调函数都依赖前一个回调函数的结果，导致代码嵌套层级不断增加，形成一个“金字塔”状的结构，很难理解和调试。



# 异步 困境

在没有使用 `Promise` 之前，我们去处理异步函数，通常会利用**回调函数**去告诉自己异步函数的运行情况：

```js
// 这是一个简单的利用回调函数去观察异步函数状态的解构
function execCode(counter, callback) {
  setTimeout(() => {
    console.log("异步开始!")
    let total = 0
    for (let i = 0; i < counter; i++) {
      total += i
    }
    callback(total)
  }, 3000)
}

execCode(100, (value) => {
  console.log("异步函数执行结束", value)
})
```

当然，异步任务也有失败的时候，比如` ajax` 请求，当这些异步任务失败的时候，我们也需要将失败的结果告诉外部，所以可以根据上述代码进行进一步的改动：

```js
function execCode(counter, successCallBack, failCallBack) {
  setTimeout(() => {
    console.log("异步开始!")
    if (counter <= 0) {
      // counter不能计算的情况
      failCallBack(`${counter}值有问题`) // 调用异步失败的回调函数
    } else {
      // counter可以计算的情况
      let total = 0
      for (let i = 0; i < counter; i++) {
        total += i
      }
      successCallBack(total) // 调用异步成功的回调函数
    }
  }, 3000)
}

execCode(20,
  (value) => {
    console.log("异步函数执行结束", value)
  },
  (err) => {
    console.log("异步函数失败", err)
  }
)
```

在 `ES5` 之间处理异步使用的就是类似于上述的代码，但是这样的代码有一些弊端，首先是设计上的问题，对使用者很不方便，对调用者也很不方便，其次就前面所提到的回调地狱，产生的代码难以阅读，难以维护。



# Promise 使用

在 `ES6` 中，新增了 `promise` 来解决这类问题，`Promise` 是一种用于处理异步操作的机制，它可以更优雅地处理回调地狱`（Callback Hell）`问题，使异步编程更加清晰和可读。

`Promise` 表示一个可能会在未来某个时间点完成或失败的操作，并提供了一种结构化的方式来处理这些操作。它具有三种状态：

1. Pending（进行中）：Promise初始状态，表示操作还在进行中。

2. Fulfilled（已完成）：表示操作已成功完成，可以获取结果。

3. Rejected（已失败）：表示操作因某些原因失败，可以获取失败的原因。

   

`Promise` 有两个主要的方法来处理异步操作的结果：`.then()` 和 `.catch()`。

- `.then(onFulfilled, onRejected)`：当Promise状态变为已完成时，会调用 `onFulfilled` 函数来处理操作的结果。如果操作失败，不会触发 `.then()`，而是会继续到接下来的 `.catch()`。
- `.catch(onRejected)`：当Promise状态变为已失败时，会调用 `onRejected` 函数来处理失败的原因。

```js
// 使用promise作为一个凭证，用这个对象内部的方法来进行对异步函数的回调，
// 这样可以使得函数的参数不需要再传递回调函数。
function execCode(counter) {
  const promise = new Promise((resolve, reject) => {
    setTimeout(() => {
      if (counter <= 0) { // 进行失败的回调
        reject("失败")
      } else { // 进行成功的回调
        let total = 0
        for (let i = 0; i < counter; i++) {
          total += i
        }
        resolve(total)
      }
    }, 3000)
  })
  return promise
}

const promise = execCode(100)
promise.then(value => {
	console.log(value) // 输出4950
})
promise.catch(err => {
	console.log(err)
})
```

```js
const promise2 = execCode(-20)
promise2.then(value => {
  console.log(value)
})
promise2.catch(err => {
  console.log(err) // 失败
})
// 之后再使用的时候，可以进行上述操作，减少了很多代码量
```



## 上述代码的简化版

```js
// 简化
execCode(-20).then(value => {
	// 异步成功的操作
}).catch(err => {
  // 异步失败的操作
})
```

没有 `promise` 代码也可以实现，但是有 `promise` 后代码会更像是一种规范，也更加容易处理。



# Promise API

- `Promise` 是一个**类**，可以翻译成 承诺，契约；
- `new` 调用创建 `Promise` 对象时，需要传入一个回调函数，称之为 `executor` ；
  - 这个回调函数会被立即执行，并且给传入另外两个回调函数 `resolve`， `reject`；
  - ==当我们调用 `resolve` 回调函数的时候，会执行 `Promise` 对象的 `then` 方法传入的回调函数==；
  - ==当我们调用 `regect` 回调函数的时候，会执行 `promise` 对象的 `catch· 方法传入的回调函数==；





# Promise 状态区分（详细使用）

```js
// 第一步：创建一个promise对象
const promise = new Promise((resolve, reject) => {
  resolve() // 可以传递参数也可以不传递参数
  reject() // 可以传参也可以不传参
})
```

```js
// 对promise的监听
promise.then(value => {
  console.log("成功的回调！")
}).catch(err => {
  console.log("失败的回调！")
})
```

上述是 `promise` 正常使用的写法，下面是其他情况：

## 同一个回调函数多次调用

```js
const promise = new Promise((resolve, reject) => {
  resolve()
  resolve()
  resolve()
  ... // 连续调用多次一个回调函数
})
promise.then(value => {
  console.log("成功的回调！")
}) // 依然是输出一次成功的回调，也就是说，函数内部即使调用了多次的回调函数，最终也只会变为调用了一次。
```

## 调用了成功的回调和失败的回调

```js
const promise = new Promise((resolve, reject) => {
  resolve() // 调用了异步成功的回调函数
	reject() // 调用了异步失败的回调函数
})
promise.then(value => {
  console.log("成功的回调！")
}).catch(err => {
  console.log("失败的回调！")
}) // 只会回掉第一个回调函数，输出：成功的回调！
// 如果把上述promise中的成功回调函数和失败的回调函数互换位置，则会输出：失败的回调！
```

## 为什么会出现上述情况

想知道为什么出现上述的情况，需要从 `Promise` 的代码结构来入手：

在上述的代码中 `promise` 的过程可以分为三个状态：

1. ==待定状态（pending）==：初始状态，既没有被兑现，也没有被拒绝，可以理解为在执行 `executor` 中的代码时，处于该状态；
2. ==已兑现（fulfilled）==：意味着操作成功完成：执行了 `resolve` 时候，处于该状态，`promise` 已经被兑现；
3. ==已拒绝（rejected）==：意味着操作失败，类似于已兑现状态；

在 `promise` 从待定状态变为已兑现或者已拒绝中任意一种状态后，就不会再变成任意一种状态。

==也就是说 `promise` 的状态一旦确定，就不会改变！==



# Executor

在Promise中，Executor是一个函数，它作为参数传递给Promise的构造函数，用于初始化Promise实例。Executor函数在Promise被创建时立即执行，它接受两个参数，这两个参数分别是resolve和reject函数。这两个函数用于改变Promise的状态，从而表示操作的结果是成功还是失败。

Executor函数的作用是执行异步操作，通常在异步操作完成后，根据操作的结果调用resolve函数或reject函数来决定Promise的状态。如果操作成功，调用resolve函数，并传递成功的结果作为参数；如果操作失败，调用reject函数，并传递失败的原因作为参数。

Executor函数的基本用法如下：

```js
const myPromise = new Promise((resolve, reject) => {
  // 异步操作
  // 如果操作成功，调用 resolve() 并传递结果
  // 如果操作失败，调用 reject() 并传递错误原因
});
```

实际上，Executor函数就是Promise的正常使用。当你使用Promise构造函数创建一个Promise实例时，传递给构造函数的函数就是Executor函数。Executor函数的作用是在Promise实例创建时立即执行，它用于执行异步操作，并根据操作的结果调用`resolve`或`reject`函数，从而决定Promise的最终状态。



# Promise resolve 的值

## 普通值

如果 `resolve` 传入的是一个普通值或者独享，那么这个值会作为 `then` 回调的参数。

下述这种就是常见的普通值：

```js
const promise = new Promise((resolve, reject) => {
  resolve( // 返回了用户的信息
  	[
      {username: "meiciko", password: 123},
      {username: "kobe", password: 456}
    ]
  )
})
promise.then(res => {
  console.log("在then中拿到结果:", res)
}).catch(err => {
})
```

## Promise值

```js
const p = new Promise((resolve, reject) =>{
  
})
const promise = new Promise((resolve, reject) => {
  resolve(p) // new了一个promise
})
promise.then(res => {
  console.log("在then中拿到结果:", res)
}).catch(err => {
})
```

==上述代码没有打印，当 `resolve` 的值是一个 `promise` 对象的时候，当前 `promise` 的状态会由传入的 `promise` 来决定！==

所以对上述代码进行一些修改

```js
const p = new Promise((resolve, reject) =>{
  setTimeout(() => {
    resolve("p的resolve")
  }, 2000)
})
const promise = new Promise((resolve, reject) => {
  resolve(p) // new了一个promise
})
promise.then(res => {
  console.log("在then中拿到结果:", res)
}).catch(err => {
})
```

最终输出：在then中拿到结果: p的resolve

所以得知，在修改前的代码中，没有打印的原因是，因为成功的回调函数中的是一个 `promise`，那么当前的 `promise` 的值会由内部来决定，而没有修改前的代码中的 `promise` 的状态一直是待定状态，所以没有输出。

而修改后的代码中，因为 p 的状态变成了已兑现，所以外部的 `Promise` 的状态也随着内部的代码变为了已兑现。

## thenable对象

什么叫做 thenable 对象呢？ 就是返回值中的对象中有一个 then 实现的方法：

```js
const promise = new Promise((resolve, reject) => {
  resolve({
    name: "meiciko",
    then: function(resolve) {
      resolve(111)
    }
  })
})
promise.then(res => {
  console.log("在then中拿到结果:", res)
}).catch(err => {
})
```

会按照最新实现的 then 来执行，并根据 then 方法的结果来决定 Promise 的状态。



# Promise then 方法的参数

```js
const promise = new Promise((resolve, reject) => {
  resolve()
})
promise.then(res => {
  console.log("成功的回调")
})
promise.then(res => {
  console.log("成功的回调")
})
promise.then(res => {
  console.log("成功的回调")
})
promise.then(res => {
  console.log("成功的回调")
})
```

上述这种情况可以执行四次！输出了四次成功的回调，但是一般不推荐这样写。

相同的，catch也可以被执行多次。



# then 返回值

Promise 中的 then 函数其实是有返回值的。

```js
const promise = new Promise((resolve, reject) => {
  resolve()
})

promise.then(res => {
  
}).catch(err => {
  
})
```

可以发现一件事情，==Promise 相当于直接进行了 Promise.then().catch() 是相当于 then 函数本身又返回了一个 promise==。

所以既然 then 返回的是一个 Promise，所以可以这样写：

```js
promise.then().then().then().catch() // 这种写法也叫做链式调用，而promise就是支持链式调用的。
```



## then 返回的 promise 的状态

我们知道 promise 是有三种状态的，那么它的 then 函数返回的 promise 是什么状态呢？

```js
const promise = new Promise((resolve, reject) => {
  resolve()
})
promise.then(res => {
  console.log("成功的回调1")
}).then(res => {
  console.log("成功的回调2")
}).then(res => {
  console.log("成功的回调3")
}).then(res => {
  console.log("成功的回调4")
}).then(res => {
  console.log("成功的回调5")
}).catch(err => {
	console.log("失败的回调1")
})
```

第二个 then ，查看的是返回的 promise ，但其实返回出来的 promise 没有决议，但是为什么后面的 then 还是执行了呢？这是因为返回的 promise 是一个新的 promise， 而后面调用的 then 依靠的是新的 promise 决议后来决定的，新的 promise 的状态是由前面的 then 的返回值来确定的：

```js
// 其实上述的第一个then的返回值可以理解为下述代码：
return new Promise((resolve) => {
  const result = fn()
  resolve(result)
})
```



```js
const promise = new Promise((resolve, reject) => {
  resolve(1)
})
promise.then(res => {
  console.log("成功的回调", res)
}).then(res => {
  console.log("成功的回调", res)
}).then(res => {
  console.log("成功的回调", res)
}).then(res => {
  console.log("成功的回调", res)
}).then(res => {
  console.log("成功的回调", res)
}).catch(err => {

})
```

```js
// 输出：
1.html:27 成功的回调 1
1.html:29 成功的回调 undefined
1.html:31 成功的回调 undefined
1.html:33 成功的回调 undefined
1.html:35 成功的回调 undefined
```

如果进行下述的修改：
```js
const promise = new Promise((resolve, reject) => {
  resolve(1)
})
promise.then(res => {
  console.log("成功的回调", res)
  return res
}).then(res => {
  console.log("成功的回调", res)
  return res
}).then(res => {
  console.log("成功的回调", res)
  return res
}).then(res => {
  console.log("成功的回调", res)
  return res
}).then(res => {
  console.log("成功的回调", res)
}).catch(err => {

})
```

```js
1.html:27 成功的回调 1
1.html:30 成功的回调 1
1.html:33 成功的回调 1
1.html:36 成功的回调 1
1.html:39 成功的回调 1
```





# Catch 返回值

事实上catch方法也是会返回一个promise对象的，所以catch方法后面我们可以继续调用then方法或者catch方法：

比如下面的代码：

```js
const promise = new Promise((resolve, reject) => {
  reject("meiciko")
})
promise.catch(err => {
  console.log("err1:", err)
}).catch(err => {
  console.log("err2:", err)
}).then(res => {
	console.log("res:", res)
})
```

输出：
```js
err1: meiciko
res: undefined
```

之所以会输出上述结果，是因为carch传入的回调在执行完后，返回的promise依然是已兑现的状态。

如果几万后续继续执行catch，需要抛出一个异常：

```js
// 对上述代码进行修改
const promise = new Promise((resolve, reject) => {
  reject("meiciko")
})
promise.catch(err => {
  console.log("err1:", err)
  throw new Error("error message")
}).catch(err => {
  console.log("err2:", err)
}).then(res => {
	console.log("res:", res)
})
```

```js
err1: meiciko
err2: Error: error message
res: undefined
```



# finally

finally是ES9之中新增的一个特性，表示==无论promise是什么状态，都会执行的代码。==

finally是不接受参数的，因为无论promise是什么状态他都会执行。

```js
const promise = new Promise((resolve, reject) => {
  // 无论这个promise会进入什么状态
  resolve()
})
promise.finall(() => {
  // 无论怎么样都会执行
	console.log("异步处理进行")
})
```

注意，想要一个可以无论什么状态都可以执行的操作，是不可以写在外面的，因为外面的函数会先执行的！达不到想要的效果。



# resolve 类方法

Promise 有一个 类方法 resolve：

```js
Promise.resolve("hellow world")
```

上述代码相当于直接new了一个promise，并且在内部写了一个resolve("hellow world")，它相当于：

```js
new Promise(resolve => {
  resolve("hellow world")
})
```

所以可以这样做：

```js
Promise.resolve("hellow world").then(res => {
  console.log(res)
}) // 输出hellow world
```

这个方法通常用于下述情况：

有时候我们已经有一个现成的内容了，但是希望将其转为promise来使用，这个时候我们可以使用Promise.resolve方法来完成！

# reject 类方法

有点类似于上述的resolve类方法。

用法也相同:

```js
const promise = Promise.reject("error message")
promise.catch(err => {
  console.log("err:", err)
})
```



# all 类方法

```js
const p1 = new Promise(...)
const p2 = new Promise(...)
const p3 = new Promise(...)

// 管理多个promise，比如希望等待上面三个promise都有结果后再执行
const promise = Promise.all([p1, p2, p3]).then(res => {
  console.log(res)
})
```

Promise.all的作用：

- 将多个promise包裹在一起形成一个新的Promise；
- 新的promise状态由包裹的所有promise来共同决定：
  - ==当所有的promise状态都变成已兑现的时候，新的promise状态为fulfilled，并且将所有promise的返回值组成一个数组==；
  - ==当有一个Promise状态为reject的时候，新的promise状态为reject，并将第一个reject的返回值作为参数==；

上述的代码中，前面三个promise都有结果后才会执行后面的这个promise。

来用一个案例来证明：

```JS
const startTime = new Date().getTime()

const p1 = new Promise((resolve, reject) => {
  // 三秒后执行
  setTimeout(() => {
    resolve("p1 resolve")
  }, 3000)
})

const p2 = new Promise((resolve, reject) => {
  // 二秒后执行
  setTimeout(() => {
    resolve("p2 resolve")
  }, 2000)
})

const p3 = new Promise((resolve, reject) => {
  // 五秒后执行
  setTimeout(() => {
    resolve("p3 resolve")
  }, 5000)
})

Promise.all([p1, p2, p3]).then(res => {
  const endTime = new Date().getTime()
  console.log(`执行时经过了${Math.round((endTime - startTime) / 1000)}秒`)
  console.log(res[0], res[1], res[2])

})
```

输出：

```js
执行时经过了5秒
p1 resolve p2 resolve p3 resolve
```

可以看到，Promise.all会等到所有的promise得到结果后，才会回调。



# allSettled

all方法有一个缺陷，当其中一个promise变成reject状态的时候，新的promise会立即变成对应的reject状态，那么对于resolved，以及仍然处于pending状态的promise，我们是获取不到对应的结果的！

为了解决这类情况，在ES11中，添加了新的API Promise.allSettled

- 该方法会在多有的Promise都有结果，无论是fulfilled，还是rejected，才会有最终的状态
- 并且这个promise的结果一定是fulfilled的



将上述代码进行修改

```js
const startTime = new Date().getTime()

const p1 = new Promise((resolve, reject) => {
  // 三秒后执行
  setTimeout(() => {
    reject("p1 reject")
  }, 3000)
})

const p2 = new Promise((resolve, reject) => {
  // 二秒后执行
  setTimeout(() => {
    resolve("p2 resolve")
  }, 2000)
})

const p3 = new Promise((resolve, reject) => {
  // 五秒后执行
  setTimeout(() => {
    resolve("p3 resolve")
  }, 5000)
})

Promise.allSettled([p1, p2, p3]).then(res => {
  const endTime = new Date().getTime()
  console.log(`执行时经过了${Math.round((endTime - startTime) / 1000)}秒`)
  console.log(res)

})
```

输出：

```js
执行时经过了5秒
(3) [{…}, {…}, {…}]
0: {status: 'rejected', reason: 'p1 reject'}
1: {status: 'fulfilled', value: 'p2 resolve'}
2: {status: 'fulfilled', value: 'p3 resolve'}
length: 3
[[Prototype]]: Array(0)
```

如果失败的话，数组中该对象的返回叫做reason！



# race 类方法

如果一个promise有了记过，我们就希望决定最终新Promise的状态，那么可以使用race方法：

- race是竞技，竞赛的意思，表示多个promise相互竞争，==谁先有结果，就先试用谁！==

```js
const p1 = new Promise((resolve, reject) => {
  // 三秒后执行
  setTimeout(() => {
    resolve("p1 resolve")
  }, 3000)
})

const p2 = new Promise((resolve, reject) => {
  // 二秒后执行
  setTimeout(() => {
    resolve("p2 resolve")
  }, 2000)
})

const p3 = new Promise((resolve, reject) => {
  // 五秒后执行
  setTimeout(() => {
    resolve("p3 resolve")
  }, 5000)
})

Promise.race([p1, p2, p3]).then(res => {
  console.log(res)
}).catch(err => {
  
})
```

输出：

```js
p2 resolve
```





# any方法

但是race有一个缺陷，在一个promise有结果（无论是已兑现还是已拒绝）都会成功race的最终结果.

any方法会在得到第一个已兑现后，返回结果，如果所有的promise都是已拒绝，就会创建一个错误信息并返回。

