# 什么是 axios

Axios 是一个基于 promise 的 HTTP 库，可以用在**浏览器**和 **node.js 中**。

Axios 是专注于网络数据请求的库。

- 相比于原生的 XMLHttpRequest 对象，axios 简单易用。
- 相比于 jQuery，axios 更加轻量化，只专注于网络数据请求。

中文文档：http://www.axios-js.com/

## 特性

- 从浏览器中创建 XMLHttpRequests
- 从 node.js 创建 http 请求
- 支持 Promise API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 XSRF

## 在浏览器使用axios

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

// 或者 

<script src="https://cdn.bootcdn.net/ajax/libs/axios/0.26.0/axios.min.js"></script>
```

然后在JS中使用直接 axios

```js
axios.get()
axios.post()
axios({})
```

## 在nodejs中使用axios

```js
npm i axios
```

然后导入axios使用

```js
const axios = require('axios');

axios.get()
axios.post()
axios({})
```

#  axios发起GET请求

axios发起GET请求请求格式：

```js
axios.get('url', { params: { /*参数*/ } }).then(callback).catch(callback)

// 或者

axios.get('url?key=value&key=value').then(callback).catch(callback)
```

举例如下：

```js
axios.get('http://127.0.0.1:3000/api/get',{
  params: { username: '张三', age: 18 }
}).then(res=>{
  console.log(res.data);
});
```

#  axios发起POST请求

axios发起POST请求格式：

```js
axios.post('url', { /*参数*/ }).then(callback).catch(callback)
```

举例如下：

```js
axios.post('http://127.0.0.1:3000/api/post',{username: '张三', age: 18}).then(res=>{
  console.log(res.data);
});
```

# axios() 发起请求

axios 也提供了类似于 jQuery 中 $.ajax() 的函数，语法如下：

```js
axios({
  method: '请求方法',
  url: '请求的URL地址',
  data: { /* POST数据 */ },
  params: { /* GET参数 */ }
}).then(callback)
```

## 使用axios()发起GET请求

GET 参数要通过 params 属性提供

```JS
axios({
  method: 'get',
  url: '/api/get',
  params: {username: '张三', age: 18}
}).then(res=>{
  console.log(res.data);
});
```

## 使用axios()发起POST请求

POST 数据要通过 data 属性提供

```JS
axios({
  method: 'post',
  url: baseUrl+'/api/post',
  data: {username: '张三', age: 18}
}).then(res=>{
  console.log(res.data);
});
```

# axios传参总结

## axios.get()请求传参

**1、拼接在url后面 **

```js
axios.get('http://127.0.0.1:3000/login?username=张三&age=18').then(data=>{});
```

 **2、写在第二个参数 **

```js
$.get('http://127.0.0.1:3000/login',{params:{username:'张三', age: 18}}).then(data=>{});
```

## axios.post()请求传参

### 参数格式

默认情况下，axios将post请求的JavaScript对象序列化为JSON。 

- 发送application/json格式的数据，在nodejs需要使用 `app.use(express.json());`把参数解析到req.body 中。

- 发送application/x-www-form-urlencoded格式的数据，在nodejs需要使用 `app.use(express.urlencoded({ extended: false }));`把参数解析到req.body 中

```js
// app.js

// 解析 application/x-www-form-urlencoded 格式的数据
app.use(express.urlencoded({ extended: false }));

// 解析 application/json 格式的数据
app.use(express.json());
```

在nodejs中设置以上两种解析方式，既可以解析 application/json 格式的数据，也能解析application/x-www-form-urlencoded 格式的数据，这样在客户端传递参数的时候就没有限制了，两种类型的参数服务端都可以解析。

### urlencoded类型的数据

**客户端**：在请求体中使用urlencoded格式的数据

**服务端**：对应服务端设置：`app.use(express.urlencoded({ extended: false }));`用来解析urlencoded格式的数据

```js
axios.post('http://127.0.0.1:3000/regist','username=张三&age=18').then(data=>{});
```

### json类型的数据

**客户端**：在请求体中使用json格式的数据

**服务端**：对应服务端设置：`app.use(express.json());`用来解析json格式的数据

```js
axios.post('http://127.0.0.1:3000/regist',{username:'张三', age: 18}).then(data=>{});
```

## axios()请求传参

get请求传参

```js
axios({
  method: 'get',
  url: '/api/get',
  // params: {username: '张三', age: 18}
  // 或者
  params: 'username=张三&age18',
}).then(res=>{
  console.log(res.data);
});
```

post请求传参

```js
axios({
  method: 'post',
  url: baseUrl+'/api/post',
  //data: {username: '张三', age: 18}, //对应服务端设置：`app.use(express.json());`
  // 或者
  data: 'username=张三&age18', // 对应服务端设置：`app.use(express.urlencoded({ extended: false }));`
}).then(res=>{
  console.log(res.data);
});
```

## 使用formdata类型的数据

在请求体使用formdata格式的数据，必须使用POST请求，此时在服务端的POST请求需要使用multer模块添加form.none() 进行处理。

**方式一**：使用FormData的实例 append 方法添加参数

```js
let data = new FormData();
data.append('username', '张三');
data.append('age', '12');
```

**方式二**：使用form表单的数据

创建formdata对象的时候传入原生form标签对象，会自动把form标签中带有name属性的表单元素的value值提取出来，相当于执行了 data.append('username','张三')。

```js
let form = document.forms[0];
let data = new FormData(form);
```

**注意**：使用方式二，页面上需要添加form标签，并且input[type=text]、input[type=password]、input[type=radio]、input[type=checkbox]、select等表单元素必须添加name属性，否则会忽略该表单元素的值

发请求的时候，可以使用axios.post或者axios()

```js
axios.post('/api/post', data).then(res => {
  console.log(res.data);
});

// 或者

axios({
  method: 'post',
  url: '/api/post',
  data,
}).then(res => {
  console.log(res.data);
});
```

