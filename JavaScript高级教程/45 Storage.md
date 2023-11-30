# Storage

## 认识 Storage

==WebStorage== 主要提供了一种机制，这种机制可以让浏览器提供一种比 ==cookie== 更直观的key，value存储方式。

- ==localstorage==：本地存储，提供一种==永久性的存储方法==，在关掉网页重新打开的时候，存储的内容依然保留；
- ==sessionStorage==：回话存储，提供的是==本次回话的存储==，在关掉会话后，存储的内容会被清除；



## Storage 基本操作

正常情况下，类似与下述的操作是把数据存在了内存中，这并不是一种持久化存储的方案，当浏览器的页面关闭掉或者刷新了以后，这条数据已经消失了（不过重新运行了！）

```js
const message = "hellow storage"
```



如果我们想要存储一些持久化的数据，比如用户的身份令牌（token）我们需要向服务器发送一次网络请求，从服务器拿到用户的身份令牌，此时如果用户关闭掉了这个页面，下次进入这个页面还是需要再次请求，但是这样会浪费时间也会浪费资源，也会给服务器带来很多压力，那么如果下次再进入浏览器的时候这个身份令牌还可以用，那么可以将这个token存储在本地的某个位置。

那么下次进入网页的时候，可以先试着从服务器中拿到token，如果没有的话再从服务器请求过来token。

保存在本地的方法：保存在Stroge中；

```js
const token = "user-token"
const username = "meiciko"
const password = "123456"

// 将token，用户名，密码保存在storage中
localStorage.setItem("token", token)
localStorage.setItem("username", username)
localStorage.setItem("password", password)
```

获取token等数据：

```js
let token = localStorage.getItem("token")
if (!token) {
  console.log("从服务器中获取token")
  localStorage.setItem("token", token)
}
// 用户和密码同理
```





# localStorage 和 sessionStorage 的区别

- 验证一：在关闭网页后重新打开，localStorage会保留，sessionStorage会被删除；
- 验证二：在页面内部实现跳转，locakStorage会保留，sessionStorage也会保留；
- 验证三：在页面外实现跳转（打开新的网页），locaStorage会保留，sessionStorage不会被保留；



```js
locakStorage,setItem("localkey", "value")
sessionStorage.setItem("sessionKey", "value")
```





# Storage常见属性和方法

## 属性

==Storage.length==：这个属性用来获取存储数据的个数

```js
console.log(localStorage.length)
```



## 方法

| 方法                            | 作用                   |
| ------------------------------- | ---------------------- |
| ==Storage.setItem(key, value)== | 设置数据的键和值       |
| ==Storage.getItem(key, value)== | 获取数据的键和值       |
| Storage.key(1)                  | 根据索引来获取某个位置 |
| ==Storage.removeItem(key)==     | 根据key来删除          |
| Storage.clear()                 | 将存储的内容全部删除   |





# Storage工具封装

实际开发中，会封装一个工具（类）

==storage本身是无法直接存储对象类型的，它只能存储基本数据类型==。

```js
class Cache {
  setCache(key, value) {
    if (!value) {
      throw new Error("value值不能为空")
    } 
		if (value) {
      local.Storage.setItem(key, JSON.stringify(value))
    }
  }
  getCache(key) {
    const res = localStorage.getItem(key)
    if (result) {
      return JSON.parse(result)
    }
  }
  removeCache(key) {
    localStorage.removeItem(key)
  }
  clear() {
    localStorage.clear()
  }
}

const localCache = new Cache()
```

在另一个文件：

```js
const userInfo = {
  name: "meiciko",
  nickname: "Meiciko",
  level: 100,
  imgURL: "http://github.com/coderwhy.png"
}
// 正常情况下存储一个对象类型使用的办法：
localStorage.setItem("userInfo", JSON.stringify(userInfo))
// 取出
console.log(JSON.parse(localStroage.getItem("userInfo")))

// 现在使用封装的类来存：
localCache.setCache("userinfo", userinfo)
// 拿取
localCache.getCache("userinfo")
```

可以看到使用封装好的类来做这些事情是非常方便的。



上述只是对localStroage的操作，但是还可以再添加新的效果

```js
class Cache {
  constructor (isLocal = true) { // 默认是local
    this.storage = isLocal ? localStorage : sessionStorage
  }
  setCache(key, value) {
    if (!value) {
      throw new Error("value值不能为空")
    } 
		if (value) {
      this.storage.setItem(key, JSON.stringify(value))
    }
  }
  getCache(key) {
    const res = this.storage.getItem(key)
    if (result) {
      return JSON.parse(result)
    }
  }
  removeCache(key) {
    this.storage.removeItem(key)
  }
  clear() {
    this.storage.clear()
  }
}
const localCache = new Cache()
const sessionCache = new Cache(false)
```

此时这个工具使用起来会非常的方便！