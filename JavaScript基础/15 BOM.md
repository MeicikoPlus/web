# **认识 JavaScript BOM**

**BOM是浏览器对象模型**(Browser Object Model), 简称为BOM, 是**由浏览器提供的用于处理文档(document)之外的所有内容的其它对象**(比如navigator, location, history等等).

JavaScript有一个非常重要的运行环境就是浏览器, 而浏览器本身又作为一共应用程序需要对其本身进行操作, 所以通常浏览器会有对应的对象类型(BOM), 我们可以将BOM堪称**是连接JavaScript脚本与浏览器窗口的桥梁**.

**BOM主要包含以下的对象模型**:

1. window: 包括全局属性, 方法, 控制浏览器窗口的相关的属性, 方法...
2. location: 浏览器连接到的对象的位置(URL).
3. history: 操作浏览器的历史.
4. navigator: 用户代理(浏览器)的状态和标识(很少用到).
5. screen: 屏幕的窗口信息(很少用到).



------

# **window对象**

window对象在浏览器中可以从两个视角来看待:

1. 全局对象: ECMAScript其实是有一个全局对象的, 这个对象在Node中是global, 在浏览器中是window对象(现在统一叫globalthis).
2. 浏览器窗口对象: 作为浏览器窗口时, 提供了对浏览器操作相关的API.

这两个视角存在大量重叠的部分,不需要刻意去区分.

**放在window对象上的所有属性都可以被访问**.

使用var定义的变量会被添加到window对象中.

window默认给我们提供了全局的函数和类: setTimeout, Date等等.

## 常见的window属性和方法

### 常见对象

window对象和方法有很多, 这里只介绍常用的几种, 更多可以查看文档.

**outerHeight, innerHeight, scrollX, scrollY**, 这四个已经提到过, 在[13. JavaScript事件监听,md]中已经记录.

**screenX, screenY**指的是浏览器的左上角距离屏幕的左上角的X, Y距离(不是很常用).

```JavaScript
console.log(window.screenX)
console.log(window.screenY)
```



### 常见方法

1. window.open("网络地址"): 打开一共新的地址, 一般结合着按钮点击去使用...

   ```javascript
   const btn = document.querySelector(".btn")
   btn.onclick = function() {
   	window.open("地址", "_self")
     // open() 函数的第二个参数为target, 用法同<a>
   }
   ```

2. window.close(): 这个方法没有参数也没有返回值, 直接调用即可, 效果是关闭窗口(很少用, 一般用于关闭第二个参数是_blank的页面).

3. window.onload(): 整个页面已经加载完成所有的资源.

   ```JavaScript
   window.onload - function() {
   	// 代码块
   }
   ```

4. window.onfocus(): 使窗口获取焦点.

   ```JavaScript
   window.onfocus = function() {
   	// 代码块
   }
   ```

5. window.onblur(): 窗口失去焦点

6. window.hashchange(): hash值(网页连接#后的内容)发生改变.

   如果想要获取hash值可以使用location.hash, 当然也可以使用location.hash去改变hash值.

更多的方法可以查询文档.



------



# **Location**

## location对象常见的属性和方法

location(常用于表示URL信息)更多的是去操作URL.

### 常见属性

1. href: 当前window对应的超链接URL, 可以理解为整个URL;
2. protocol: 当前的协议(http等等);
3. host: 主机地址;
4. hostname: 主机地址(不带上端口);
5. port: 端口;
6. pathname: 路径;
7. search: 查询字符串(?...&...);
8. hash: 哈希值(也可以叫片段);
9. username: URL中的username(很多浏览器已经禁用);
10. password: URL中的password(很多浏览器已经禁用);

```JavaScript
// 获取完整的URL
console.log(location.href) // 一般来说会对url中的中文进行编码
// 获取url的信息
console.log(location.hostname) // 获取主机地址
console.log(location.host) // 包含端口号
... ...
```

可以得出, location其实是url的一共抽象实现.

### 常见方法

1. assign: 赋值一个新的URL, 并且跳转到该URL中.
2. replace: 打开一个URL, 并且跳转到该URL中(不同的是不会在浏览记录中留下新的记录).
3. reload: 重新加载页面, 可以传入一共Boolean类型(刷新).

```javascript
location.assign("地址")
location.replace("地址") // 不同于assign的是用此方法打开新的网页是无法返回的(⬅).
location.reload()
```



------



# **URLSearchParams**

URLSearchParams定义了一些实用的方法去去处理URL的查询字符串.

也可以将一个字符串转换成URLSearchParams的类型.

也可以将一共URLSearchParams类型转换成字符串.

```JavaScript
let str = "?name=why&age=18&height=1.88" // 模拟一个查询字符串
// 这个时候如果想要处理这个字符串可以使用split("&"), 但是很麻烦, 也处理不掉?等符号.
let searchParams = new URLSearchParams(str) // 创建一个对象
console.log(searchParams.get("name")) // why
console.log(searchParams.get("age")) // 18
console.log(searchParams.get("height")) // 1.88
// 使用起来非常方便.
```

## URLSearchParams常用的方法

1. get(): 获取搜索参数的值.
2. set(): 设置一共搜索参数和值.
3. append(): 追加一共搜索参数和值.
4. has(): 判断是否由某个搜索参数.
5. toString(): 转换为字符串形式.

```JavaScript
// 接着上述代码
searchParams.append("address", "广州")
searchParams.get("address") // 广州
console.log(typeof searchParams.get("name").toString()) // string
searchParams.set("name", "meiciko")
console.log(searchParams.get("name")) // meiciko
```



------

# **history**

history对象**允许我们访问浏览器曾经的会话历史记录**.

**现在的前端页面路由, 是通过监听hash内容的变化, 来决定对页面的渲染, 这样可以实现对服务器的请求很少(不需要整个页面的去请求)(url改变后, 整个页面并没有刷新)**.

而**修改url但是不让页面刷新**, 有两种办法, 一种是修改hash, 一种是修改history. 

```JavaScript
// 假设有一个btn的元素
const btn = document.querySelector(".btn")
btn.onclick = function() {
	history.pushState({name: "why", age: 18}, "", "/why") 
  // 执行这段代码后, 页面是绝对没有刷新的!(中间的参数可以忽略)
}
```



## history属性和方法

### 属性

1. length: 会话中的记录条数.
2. state: 当前保留的状态值.



### 方法

1. back(): 返回上一页, 等价于history.go(-1);
2. forward(): 前进下一页, 等价于history.go(1);
3. go(): 加载历史中的某一页;
4. pushState(): 打开一个指定的网址;
5. replaceState(): 打开一个新的地址, 并且使用replace(location中的, 且不能返回);