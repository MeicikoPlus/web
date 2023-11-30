# Fetch/Fetch API

Fetch可以看做早期的XMLHttpRequest的替换方案，它提供了一种更加现代的处理方案：

- 比如==返回值是一个Promise==，提供了一种更优雅的处理方式；
  - 在请求发送成功的时候，调用resolve回调then；
  - 在请求发送失败的时候，调用reject回调catch；
- 比如不像XMLHttpRequest一样，所有的操作都在一个对象上；



## Fetch 使用

> Promise<response>  fetch ( input [ , init ] )

- ==input==：定义要获取的资源地址，可以是一个URL字符串，也可以使用一个Request对象（实验性特性）类型；
- ==init==：其他初始化参数：
  - ==method==：请求使用的方法，如GET，POST；
  - ==headers==：请求头的信息；
  - ==body==：请求的body信息；



## 案例（Fetch）

### 基本使用

**1.1**：未优化的代码

```js
// 利用Fetch发起一个GET请求
fetch("http:...").then(res => {
  // 获取到对应的response（res）
  // 获取具体的结果
  res.json().then(res => {
    console.log("res:", res)
  }) // 如果是文本就使用.text
}).catch(err => {
  console.log("err:", err)
})
```

**1.2**：优化方式：返回值

```js
fetch("http:...").then(res => {
  return res.json()
}).then(res => {
  console.log("res:", res)
}).catch(err => {
  console.log("err:", err)
})
```

**1.3**：优化方式：异步函数

```js
async function getData() {
  const response = await fetch("http:...")
  const res = await response.json()
}
getData()
```

### 发送请求并且携带参数

```js
async function getData() {
  const response = await fetch("http:...", {
 		method: "post",
    headers: {
      "Content-type": "application/json"
    }, 
    body: JSON.stringfy({
      username: "meiciko",
      password: "1234567"
    })
  })
  const res = await response.json()
}
getData()
```

