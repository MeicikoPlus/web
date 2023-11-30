# 发送数据的不同方式

## 第一种: get请求传参

第一种发送数据的方式是依靠get请求的目标url传入参数的形式来传递的.

```JavaScript
const xhr = new XMLHttpRequest()
xhr.onload() = function() {
  console.log(xhr.response)
}
xhr.reponseType = "json"
// 将数据以参数的形式用get请求发送
xhr.open("get", "http://... ...?name=meiciko&age=18")
xhr.send()
```

get请求有一个缺陷, 因为传递的参数在url中, 是**可见**的, 在传递password的时候也是可见的, 并**不安全**.

## 第二种: post请求x-www-form-urlencoded格式

相比较于get请求, post请求将请求内容放入请求体中的行为更加安全. 

**需要注意的是, 发送请求需要标注请求的数据的格式, 否则服务器不会进行解析**.

```JavaScript
const xhr = new XMLHttpRequest()
xhr.onload() = function() {
  console.log(xhr.response)
}
xhr.reponseType = "json"
xhr.open("post", "http://... ...")
// 将请求放在请求体中, 同时需要标注格式
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded")
xhr.send("name=meiciko&age=18")
```

## 第三种: post请求FormData格式

```html
<form>
  <input type="text">
  <input type="password">
  <input type="submit">
</form>
```

网页中也有一些form的元素存在, 如上所示, 当需要采集上述的信息发送请求的时候, 使用formData的格式可以省去使用JavaScript获取form中子元素的value, 然后拼装信息发送的步骤.

```javascript
const formEl = document.querySelector("form")
const xhr = new XMLHttpRequest()
xhr.onload() = function() {
  console.log(xhr.response)
}
xhr.reponseType = "json"
xhr.open("post", "http://... ...")
const formData = new FormData(formEl)
xhr.send(formData)
```

当使用formData的时候, 是不需要设置请求头中的数据格式的, 因为会自动使用formData类型.

## 第四种: post请求JSON格式

```JavaScript
const xhr = new XMLHttpRequest()
xhr.onload() = function() {
  console.log(xhr.response)
}
xhr.reponseType = "json"
xhr.open("post", "http://... ...")
xhr.setRequestHeader("Content-Type", "application/json")
xhr.send(JSON.stringfy({name: "meiciko", age: 18, height: 1.88}))
```

用法与前面的类似, 不过多做赘述.



------

# Ajax封装

发送请求是一个很繁琐的步骤, 当有发送很多次请求的需求后, 秉承不书写重复代码的原则, 其实可以将请求进行封装, 封装成一个函数:

```JavaScript
/**
 * @param {string} url 请求地址 必填
 * @param {Object} data 数据 必填
 * @param {string} method 请求方式 选填(默认get)
 * @param {Object} header 请求头 选填(默认{})
 * @param {Number} timeout 超时 选填(默认10s)
 * @param {Function} success 回调函数, 用于拿回返回值 必填
 * @param {Function} failure 回调桉树, 用于处理请求失败的结果 必填
 */
function meiciko_ajax(
  {
    url,
    data = {},
    method = "get",
    success,
    failure
  } = {}
) {
  // 创建XMLHttpRequest对象
  const xhr = new XMLHttpRequest()

  // 监听数据
  xhr.onload = function() {
    if (xhr.status >= 200 && xhr.status <= 300) { //发送请求成功
      // 判断是否有success
      success && success(xhr.response)
    } else { // 请求失败
      failure && failure({
        status: xhr.status, 
        message: xhr.message
      })
    }
  }
  // 设置返回结果类型
  xhr.responseType = "json"

  // 判断要执行哪种请求
  if (method.toUpperCase() == "GET") {
    const queryStr = []
    for (const key in data) {
      queryStr.push(`${key}=${data[key]}`)
    }
    url = url + "?" + queryStr.join("&")
    // 设置请求类型
    xhr.open(method, url)
    xhr.send()
  } else {
      // 设置请求类型
    xhr.open(method, url)
    xhr.setRequestHeader("Content-Type", "application/json")
    xhr.send(JSON.stringify(data))
  }
}

```





# 超时（过期）

用法：

```js
xhr.timeout = 3000
```

效果为，如果这个时间后还没有响应，就会取消这个请求！





# 取消请求

假设此时有一个按钮：btnEl

```js
btnEl.onclick = function() {
  xhr.abort() // abort用于取消这个请求
}
```

