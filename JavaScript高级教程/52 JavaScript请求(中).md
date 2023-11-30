# JavaScript发送请求

AJAX是**异步的JavaScript和XML**.

如何来完成AJAX请求:

1. 创建网络请求的AJAX对象.
2. 监听XMLHttpRequest对象状态的变化, 或者监听onload事件(请求完成时触发).
3. 配置网路请求(通过open方法).
4. 发送send网络请求.

# XHR-Fetch请求

这是一个简单的请求:
```JavaScript
// 1. 创建一个xml对象
const xhr = new XMLHttpRequest()
// 2. 监听状态的变化
xhr.onreadystatechange = function() {
  console.log(xhr.responseText)
}
// 3. 配置请求
xhr.open("get", "https://restapi.amap.com/v3/weather/weatherInfo?key=31f308e92b05f0e77489371d2052e1bc&city=410100&output=JSON")
// 4. 发送请求
xhr.send()
```

## XMLHttpRequest的state(状态)

下面是上端代码的稍加改装:

```JavaScript
<script>
  const xhr = new XMLHttpRequest()
  xhr.onreadystatechange = function() {
    console.log("statechange:", xhr.readyState)
    // 修改了这里, xhr.readyState的效果是读取对应的状态
  }
  xhr.open("get", "https://restapi.amap.com/v3/weather/weatherInfo?key=31f308e92b05f0e77489371d2052e1bc&city=410100&output=JSON")
  xhr.send()
</script>
```

```JavaScript
// 打印结果如下:
statechange: 1
statechange: 2
statechange: 3
statechange: 4
```

事实上, 我们可以在**一次网络请求中看到状态发生了很多次变化**, 这是因为对于一次请求来说包括如下的状态:

| 值   | 状态    | 描述                                     |
| ---- | ------- | ---------------------------------------- |
| 1    | UNSENT  | 代理已被创建, 但是尚未调用open()方法     |
| 2    | OPENED  | open()方法已被调用                       |
| 3    | LOADING | 下载中, responseText属性已经包含部分数据 |
| 4    | DONE    | 下载操作已完成                           |

所有可以在进行一次优化:
```JavaScript
const xhr = new XMLHttpRequest()
xhr.onreadystatechange = function() {
  // 当状态不是DONE的时候, 直接返回
  if(xhr.readyState !== XMLHttpRequest.DONE) return

  // 确定拿到了数据(DONE == 4)
  console.log(xhr.response)
}
xhr.open("get", "https://restapi.amap.com/v3/weather/weatherInfo?key=31f308e92b05f0e77489371d2052e1bc&city=410100&output=JSON")
xhr.send()
```

## 异步的网络请求以及如何进行同步

先看代码:

```JavaScript
const xhr = new XMLHttpRequest()
xhr.onreadystatechange = function() {
  // 当状态不是DONE的时候, 直接返回
  if(xhr.readyState !== xhr.DONE) return

  // 确定拿到了数据(DONE == 4)
  console.log(xhr.response)
}
xhr.open("get", "https://restapi.amap.com/v3/weather/weatherInfo?key=31f308e92b05f0e77489371d2052e1bc&city=410100&output=JSON")
xhr.send()

console.log("这是写在请求后的输出")
```

运行结果:

```JavaScript
index.html:21 这是写在请求后的输出
index.html:16 {"status":"1","count":"1","info":"OK","infocode":"10000","lives":[{"province":"河南","city":"郑州市","adcode":"410100","weather":"晴","temperature":"33","winddirection":"西","windpower":"≤3","humidity":"32","reporttime":"2023-07-04 20:00:17","temperature_float":"33.0","humidity_float":"32.0"}]}
```

当代码执行请求这一步骤的时候, **因为发起请求收到响应需要时间, JavaScript的代码会继续执行下去**, 也就是所谓的异步.

如果想要代码同步进行, 则只需对open()内部的参数进行一部分的设置即可:
```JavaScript
const xhr = new XMLHttpRequest()
xhr.onreadystatechange = function() {
  // 当状态不是DONE的时候, 直接返回
  if(xhr.readyState !== xhr.DONE) return

  // 确定拿到了数据(DONE == 4)
  console.log(xhr.response)
}
// 在open函数内部有第三个参数, 默认位true(异步), 当使用false后, 可以进行同步操作
xhr.open("get", "https://restapi.amap.com/v3/weather/weatherInfo?key=31f308e92b05f0e77489371d2052e1bc&city=410100&output=JSON", false)
xhr.send()

console.log("这是写在请求后的输出")
```

当进行同步的时候, 代码会先等到获得响应后才会继续执行.

在代码开发中一般使用异步, 因为异步不会阻塞代码运行.

## XMLHttpRequest其他事件监听

除了onreadystatechange还有其他事件可以监听(注: onreadystatechange事件的效果位当state的状态改变的时候被调用)

| loadstart | 请求开始                                                  |
| --------- | --------------------------------------------------------- |
| progress  | 一个响应数据包到达, 此时整个response body都在response.    |
| abort     | 调用xhr.abort()取消了请求                                 |
| error     | 发生连接错误, 例如, 域错误, 不会发生诸如404这类的http错误 |
| load      | 请求成功完成                                              |
| timeout   | 由于请求超时而取消了请求(仅发生在设置了timeout的情况下)   |
| loaded    | 在load, error, timeout. 或者abort后触发                   |

 ```javascript
 let url = "... ..."
 const xhr = new XMLHttpRequest()
 xhr.onload = function() {
 	console.log("onload")
 }
 xhr.open("get", url)
 xhr.send()
 ```

比如我们可以使用onload(请求成功完成), 这样可以省去一些判断语句(当然请求失败的时候缺少了一些应对的灵活性).

## 响应数据和响应类型

当我们发送了请求后, 我们需要获取的对应结果: response属性.

XMLHttpRequest的 **response属性返回响应的正文内容**, 返回的类型取决于responseType的属性设置(一般来说常见返回的数据为json类型).

xhr有一个属性为responseType, 它默认为一个空的字符串, 默认值为text, 即普通的文本, 这就会导致我们拿到响应的数据为文本类型.

可以在接收到数据的时候将它转化为JSON类型:

```JavaScript
xhr.onload() = function() {
  const resJSON = JSON.parse(xhr.response)
}
```

当然也可以设置responseType:

```javascript
let url = "... ..."
const xhr = new XMLHttpRequest()
xhr.onload = function() {
	console.log(xhr.response)
}
xhr.responseType = "json" // 修改响应的类型
xhr.open("get", url)
xhr.send()
```





# 请求本地json文件渲染表格案例

```HTML
<header>
  <button class="control">动态请求数据转换为表格</button>
</header>
```

```JavaScript
const headerEl = document.querySelector("header")
const btnEl = document.querySelector(".control")
// 监听鼠标点击事件
btnEl.addEventListener("click", addtable)

function addtable() {
  let user_list
  let url = "./data.json"
  // 请求本地的json文件
  let xhr = new XMLHttpRequest()
  xhr.onreadystatechange = function() {
    if (xhr.status == 200) {
      if (xhr.readyState !== xhr.DONE) return
      // 将响应的文本转换为json格式
      user_list = JSON.parse(xhr.response)

      const tableEl = document.createElement("table")
      tableEl.classList.add("mytable")
      // 创建表头
      const theadEl = document.createElement("thead")
      let user_keys = Object.keys(user_list[0])
      const thEl = document.createElement("tr")
      for (let i = 0; i < user_keys.length; i++) {
        const th_tdEl = document.createElement("td")
        th_tdEl.classList.add("mytd")
        th_tdEl.innerHTML = user_keys[i]
        thEl.appendChild(th_tdEl)
      }
      theadEl.appendChild(thEl)
      tableEl.appendChild(theadEl)                              
      // 创建表格
      for (let i = 0; i < user_list.length; i++) {
        const trEl = document.createElement("tr")
        for (let j = 0; j < user_keys.length; j++) {
          const tdEl = document.createElement("td")
          tdEl.classList.add("mytd")
          tdEl.innerHTML = user_list[i][user_keys[j]]
          trEl.appendChild(tdEl)
        }
        tableEl.appendChild(trEl)
      }
      // 将表格添加到header中
      headerEl.appendChild(tableEl)
    }
  }
  // 配置|发送请求
  xhr.open("get", url)
  xhr.send()
}
```



# 请求HTML文件案例

```javascript
// 接上个案例演示
// 因为返回的内容直接作为字符串被插入进去, 所以内部的js没有被运行
// 为了解决这个办法, 需要使用eval()
```

```HTML
<-- 即将被请求的文件: -->
<section>
  <h1 id="myh1">我是后来被添加的!</h1>
  <h1>我是后来被添加的!</h1>
  <h1>我是后来被添加的!</h1>
  <h1>我是后来被添加的!</h1>
  <h1>我是后来被添加的!</h1>
  <h1>我是后来被添加的!</h1>
  <h1>我是后来被添加的!</h1>
</section>
<script class="myjs">
  const h1EL = document.querySelector("#myh1")
  h1EL.style.color = "blue"
</script>
```

```JavaScript
const headerEl = document.querySelector("header")
// 请求html文件的测试
const testxhr = new XMLHttpRequest()
testxhr.onload = function() {
  const el= testxhr.response
  headerEl.innerHTML = testxhr.response
  const scriptEl = headerEl.querySelector(".myjs")
  eval(scriptEl.textContent)
}
testxhr.open("get", "./test.html")
testxhr.send()
```

