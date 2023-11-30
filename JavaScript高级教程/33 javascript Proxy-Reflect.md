# **Proxy - Reflect**



## 引入话题：监听对象属性的操作（存储属性描述符）

需求场景：当我们对某个属性进行操作的时候，y有时候会希望可以同时更改页面内部的内容：

```js
const obj = {
  name: "meiciko"
}

const nameEl = document.querySelector(".name")
nameEl.textContent = obj.name

obj.name = "kobe"
// 虽然修改了 obj.name ，但是页面内部的内容没有被修改
// 但是更多的时候，我们是想页面可以实时更新的！否则只能够手动的去修改：
nameEl.textContent = obj.name // 再进行一次操作
```

上述操作中我们都是手动去完成，==但是很多时候我们更希望上述中的赋值操作能在属性变化的时候自动去做页面的刷新、所以需要去监听这个属性的改动==。



可以使用对象的属性描述符去完成这个效果（get）（set）：

```js
Object.defineProperty(obj, "name", {
  set: function(newValue) {
    console.log(`${key}的值被修改为`, newValue)
  },
  get: function() {
   	return _name
  }
})
```

如果想要遍历所有的属性，然后对每个属性都进行类似的做法。

```js
const keys = Object.keys(obj)
for (const key of keys) {
  Object.defineProperty(obj, key, {
    set: function(newValue) {
      console.log(`${key}的值被修改为`, newValue)
    },
    get: function() {
      return value
    }
  })
}
```

上述就是 `vue2 响应式`的原理，但是在 `vue3响应式` 中，使用的是 `promise` .



但是，使用Object.defineProperty 是存在一些弊端的：

- 首先，Object.defineProperty 设计的初衷，不是为了去监听一个对象中的所有属性的。
- 我们在定义某些属性的时候，初衷其实是定义普通的属性，之色后面强项变成了数据的属性描述符。
- 其次，如果想监听更丰富的操作，比如新增属性，删除属性，那么 Object.defineProperty 是无能为力的。

所以，存储数据符的设计的初衷并不是为了去监听一个完整的对象。





## Proxy的基本使用

上述中对属性的描述是为了引出这样一个思想，想要监听一个对象的相关操作，现在进入正文：

在 ES6 中，新增了一个 Proxy 类，这个类从名字就可以看出来，是用于帮助我们创建一个代理的：



- 也就是说，如果我们希望==监听一个对象的相关操作==，那么我们可以先==创建一个代理对象（proxy）对象==；
- 之后对该对象的所有操作，都通过代理对象完成，==代理对象可以监听我们想要对原对象进行哪些操作==；



- 首先，我们需要使用 ==new Proxy== 对象，并且==传入监听的对象以及一个处理对象==，可以称之为 ==handler== ;
- 其次，==我们之后的操作都是直接对 Proxy 的操作，而不是对原有对象，因为我们需要在 handler 里面进行监听==。



```js
const obj = {
  name: "meiciko",
  age: 18,
  height: 1.88
}

// 1. 创建一个obj的代理（传入obj）
const objProxy = new Proxy(obj, {})

// 2. 对obj的所有操作，应该去操作objProxy
console.log(objProxy.name) // meiciko
objProxy.name = "kobe"
console.log(objProxy.name) // kobe
console.log(obj) // {name: 'kobe', age: 18, height: 1.88}
```

在上面的基础上，我们还希望它做出更多的事情，比如打印一句话，比如可能涉及的更多的操作。





## Proxy的set和get捕获器

如果我们想要监听某些具体的操作，那么就可以在 handle 中添加对应的 ==捕获器（trap)== ;

==set== 和 ==get== 分别对应的是函数类型：

==set== 函数有四个参数：

1. targrt：目标对象（监听的对象）；
2. property：将要被设置的属性key；
3. value：新属性值；
4. receiver：调用的代理对象（proxy对象）；



==get== 函数有三个参数：

1. target：目标对象（监听的对象）；
2. peoperty：备货区的属性key；
3. receiver：调用的代理对象（proxy对象）；



```js
const obj = {
  name: "meiciko",
  age: 18,
  height: 1.88
}

// 1. 创建一个obj的代理（传入obj）
const objProxy = new Proxy(obj, {
  set: function(target, peoperty, value) {
    console.log(`监听到${peoperty}的值修改为了${value}`)
  },
  get: function(target, peoperty) {
    console.log(`监听到${peoperty}的获取`)
    return target[peoperty]
  }
})

console.log(objProxy.name) // 输出
// 监听到name属性的获取
// meiciko
objProxy.name = "Meiciko" // 输出
// 监听到name属性的值修改为了Meiciko
objProxy.address = "CN" // 输出
// 监听到address的值修改为了CN
```





## Proxy所有捕获器

上述只是介绍了 set 和 get 捕获器，但是其实 Proxy 有着13个捕获器，那么这13个捕获器分别是做什么的呢？

- handler.getPrototypeOf()：Object.getPrototypeOf 方法的捕获器；
- handler.setPrototypeOf()：Object.setPrototypeOf 方法的捕获器；
- handler.isExtensible()：Object.isExtensible 方法的捕获器（判断是否可以新增属性）
- handler.preventExtensions()：Object.preventExtensions 方法的捕获器；
- handler.difineProperty()：Object.defineProperty方法的捕获器；
- handler.ownKeys()：Object.getOwnPropertyNames方法和Object.getOwnPropertySymbols方法的捕获器；
- ==handler.has()：in操作符的捕获器==；
- ==handler.get()：属性读取操作的捕获器==；
- ==handler.set()：属性设置操作的捕获器==；
- ==handler.deleteProperty()：delete操作符的捕获器==；
- handler.apply()：函数调用操作的捕获器；
- handler.construct()：new操作符的捕获器；



```js
// 示例：delete
const obj = {
  name: "meiciko",
  age: 18,
  height: 1.88
}

// 1. 创建一个obj的代理（传入obj）
const objProxy = new Proxy(obj, {
	deleteProperty: function(target, key) {
    console.log(`删除了${key}的属性`)
    delete obj.name
  }
})
delete objProxy.name
// 输出：删除了name的属性
```





## 监听函数（proxy的construct和apply）

比如：监听函数是否是通过 apply 调用的！

```js
function foo() {
  
}

const fooProxy = new Proxy(foo, {
  apply: function(target, thisArg, otherArgs) {
    console.log("函数的apply侦听")
    return target.apply(thisArg, otherArgs)
  },
  construct: function(target, otherArray {
    console.log(target, argArray, newTarget)
    return new target(...otherArray)
  }
})

fooProxy.apply("abc", [111, 222])
new fooProxy("aaa", "bbb")
```





由于 proxy 异常的好用，所以 vue3 的响应式原理用的就是 proxy。
