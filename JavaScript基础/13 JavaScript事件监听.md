# **元素的大小**

元素的大小可以通过下面几个属性来获取:

- clientWidth: contentwidth(内容的宽度) + padding(不包含滚动条).
- clientHeight: contentHeight + padding.
- clientTop: border-top的宽度.
- clientLeft: border-left的宽度.

```JavaScript
const myEl = document.querySelector(".box")
const w = myEl.clientWidth
const h = myEl.clientHeight
console.log(w, h)
```

------



# **JavaScript事件监听**

Web要经常捕捉用户交互的动作, 比如用户点击了某个按钮, 或者鼠标经过某些地方, 或者输入了一段文字, 当某个事件被触发后, 我们可以让JavaScript执行某个函数, 所以我们要针对事件编写事件处理程序.

一般来说, 一个监听动作只能由一个处理函数, 但是如果我们想要让事件监听多个函数(比如我想让一个按钮被点击的时候执行两个监听事件):

```JavaScript
myEl.addEventListener("触发事件(例如click)", "方法体1")
myEl.addEventListener("触发事件(例如click)", "方法体2")
myEl.addEventListener("触发事件(例如click)", "方法体3")
```

一般来说, 也是比较推荐addEventListener()这种方法来写.

------

## 事件流与事件对象

### 事件流

当我们在浏览器上对着一个元素点击时, 你点击的不仅仅是这个元素本身.

实际上, 还有另一种**事件监听流的方式就是从外层到内层(body→span)**, 这种称之为**事件捕获**.

### 事件对象

当一个事件发生的时候, 就有很多相对应的信息, 浏览器会记录到一个对象里面: 

```JavaScript
divEl.onclick = function(event) {
    console.log("div发生了点击", event)
} // event会有对象的信息.
```

event常见的属性和方法:

1. type: 事件的类型.
2. target: 当前事件发生的元素.
3. currentTarget: 当前处理事件的元素.

如下案例:

```HTML
<body>
  <div><button class="btn"></button></div>
  <script>
    const btnEl = document.querySelector(".btn")
    const divEl = document.querySelector("div")
    divEl.onclick = function(event) {
      console.log("target:", event.target)
      console.log("currentTarget:", event.currentTarget)
    } 
  </script>
</body>
```

```JavaScript
// 输出结果如下:
// target: <button class=​"btn">​</button>​
// currentTarget: <div>​…​</div>​
```

### 事件阻断

当我们只希望点击按钮的时候只触发按钮的事件处理, 不希望父元素的事件被冒泡的话, 我们可以使用事件阻断:

```JavaScript
btnEl.addEventListener("click", function(event) {
  event.stopPropagation(); // 阻止事件冒泡
  console.log(this);
});
```

### 事件处理时的this

this在事件处理时是指向方法所绑定的元素的, 所以this和eventTarget都可以拿到所绑定的元素.

```JavaScript
const btnEl = document.querySelector(".btn")
const divEl = document.querySelector("div")
divEl.onclick = function(event) {
  console.log(this)
  console.log(this == divEl)
} 
/*
out: <div><button class="btn"></button></div>
out: true
*/
```

## EventTarget类

我们会发现, 所有的节点, 元素都继承自EventTarget, 事实上, window也继承自EventTarget. 

EventTargrt类是一个DOM接口, 主要用于**添加, 删除, 或者派发Event事件**.

EventTarget常见的方法:

- addEventListener: 注册某个事件类型及其事件处理函数.
- removeEventListener: 移除某个事件类型及其事件处理函数.
- dispatchEvent: 派发某个事件类型到EventTargret上.

```JavaScript
let foo = function() {
    // 方法体
}
divEl.addEventListener("click", foo) // 为对象添加一个点击的事件处理函数
divEl.removeEventListener("click", foo) // 为事件移除一个点击的事件处理函数
```

注: 当使用移除函数的时候, 必须要把移除函数表示清除!

接下来是一个小案例: 创建一个ul, 并把点击的li变为红色:

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .myul{
      padding: 0;
      display: flex;
    }
    .myul li{
      display: inline-block;
      flex: 1;
      height: 60px;
      border: 1px solid black;
      cursor: pointer;
    }
    .active{
      background-color: red;
    }
  </style>
</head>
<body>
  <ul class="myul">
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
  </ul>
  <script>
    const ulEl = document.querySelector(".myul")
    ulEl.addEventListener("click", function(event) {
      const liEl = event.target
      liEl.classList.add("active")
    })
  </script>
</body>
</html>
```



------



## window窗口的大小和滚动

### 大小监听

window的width和height:

- innerWidth, innerHeight: 获取window窗口的宽度和高度(**包含滚动条**)(也就是内容区域, **不包含F12调试工具, 只获取当前视口的宽度和高度**).
- outerWidth, outerHeight: 获取window窗口的整个宽度和高度(**包括调试工具, 工具栏**)(可以理解为整个浏览器的高度和宽度, 包含所有的东西).
- document Element.clientHeight, documentElement.clientWidth: 获取html的宽度和高度(不包含滚动条)(也就是HTML的高度和宽度).

例如:

```JavaScript
const h = window.innerWidth
const w = window.innerHeight
```

### 获取window的滚动区域

获取滚动区域使用的是scrollX和scrollY, 这两个属性分别会获取纵向和横向的位置.

```JavaScript
console.log(scrollX)
console.log(scrollY)
// 当滚动条在初始最顶部没有滚动的时候, 且下部滚动条也在初始位置的时候, 输出: 0 0.
// 当右侧滚动条向下拖一段距离后, 输出: 0 [拖动的距离].
// 以此类推.
```

这两个属性经常会用到, 比如当监听到滚动条向下滚动某段高度的时候, 侧边的悬浮按钮就会出现, 当滚动条到顶部的某个位置的时候, 悬浮按钮不可见等等... ...

### 操作滚动的方法:

操作页面滚动可以使用如下两种方式:

- 方法scrollBy(x, y): 将页面滚动至**相对于**当前位置的(x, y)的位置(在原来滚动的基础上再加上一部分位置).
- scrollTo(PageX, PageY): 将页面滚动至**绝对**坐标.

------

## 常见的鼠标事件

### 移入移出 (mouseover和mouseeenter的区别)

> 首先, mouseenter和mouseleave是一组的.

这两个**不支持冒泡**, 进入子元素依然属于在该元素内, 没有任何的反应 . **该事件只在鼠标指针进入元素时触发一次，并且不会在鼠标指针进入或离开元素的子元素时触发**。它只关注鼠标指针与元素之间的直接交互。

> 其次, mouseover和mouseout是一组的.

这两个**支持冒泡**, 进入元素的子元素的时候, 先调用父元素的mouseout, 再调用子元素的mouseover, 因此支持冒泡, 所以会将mouseover传递到父元素中 。**它会在鼠标指针离开一个元素并进入其子元素时再次触发**。简而言之，它会在鼠标指针进入或离开元素时触发，无论是元素本身还是其子元素。

### 常见事件

| 属性        | 概述                          |
| ----------- | ----------------------------- |
| click       | 点击某个对象触发.             |
| contextmenu | 鼠标右键打开上下文菜单出发.   |
| dblclick    | 双击某个对象时调用的事件语句. |
| mousedown   | 鼠标按钮被按下.               |
| mouseup     | 鼠标按键被松开.               |
| mouseover   | 鼠标移动到某个元素之上.       |
| mouseout    | 鼠标从某元素上移开.           |
| mouseenter  | 鼠标指针移动到元素上触发.     |
| mouseleave  | 鼠标指针移出元素时候触发.     |
| mousemove   | 鼠标被移动.                   |

------



## 常见的表单事件

| 属性     | 描述                              |
| -------- | --------------------------------- |
| onchange | 该事件在表单元素的内容改变时触发. |
| oninput  | 元素获取用户输入时触发.           |
| onfocus  | 元素获取焦点时触发.               |
| onblur   | 元素失去焦点时触发.               |
| onreset  | 表单重置时触发.                   |
| onsubmit | 表单提交时触发.                   |

```html
<form action="">
  <label for="name">姓名:&nbsp;</label>
  <input type="text" id="name"><br>
</form>
<script>
  const nameEl = document.querySelector("#name")
  nameEl.onfocus = function() {
    console.log("type=text的input元素获取了焦点")
  }
  // 失去焦点的方法与这个的用法相同
</script>
```

### onchange和oninput的区别

onchange只有在用户输入完毕并且失去焦点后打印, 但是oninput会在用户输入一个字符就会打印一次.

```html
<form action="">
  <label for="name">姓名:&nbsp;</label>
  <input type="text" id="name"><br>
</form>
<script>
  const nameEl = document.querySelector("#name")
  nameEl.oninput = function() {
    console.log("用户正在输入" + nameEl.value)
  }
  // 使用这个方法后. 用户每输入一个字符都会打印一次
</script>
```

```html
<form action="">
  <label for="name">姓名:&nbsp;</label>
  <input type="text" id="name"><br>
</form>
<script>
  const nameEl = document.querySelector("#name")
  nameEl.onchange = function() {
    console.log("用户输入" + nameEl.value)
  }
  // 这个是在用户输入完毕然后input失去焦点的时候才会打印
</script>
```

### 阻止默认的提交和重置事件

可以通过在表单的提交事件和重置事件上使用`event.preventDefault()`方法来实现阻止提交和重置事件的发生.

阻止提交事件的发生:

```HTML
<form action="">
  <label for="name">姓名:&nbsp;</label>
  <input type="text" id="name"><br>
  <input type="reset" value="重置"><br>
  <input type="submit" value="提交">
</form>
<script>
  const formEl = document.querySelector("form")
  formEl.onsubmit = function(event) {
    console.log("发生了提交事件")
    event.preventDefault() // 阻止默认的事件
  }
</script>
```

阻止重置事件的发生:

```JavaScript
// 获取表单元素
const form = document.getElementById('myForm');
// 绑定重置事件的处理函数
form.addEventListener('reset', function(event) {
  // 阻止表单的默认重置行为
  event.preventDefault();
});
```

------



## 文档加载事件

先看案例:

```HTML
<body>
  <script>
      const divEl = document.querySelector("div")
      divEl.style.backgroundColor = "red"
  </script>
  <div style="width: 100px; height: 100px;"></div>
</body>/
```

在上述HTML代码种, JavaScript会出现错误, 因为JavaScript在div元素还没有加载的时候就已经运行.

但是当使用onload可以避免这个情况发生, 因为onload是将文档中的所有内容, 包括文本, 样式, 图片, 视频... 加载完毕后, 才开始运行:

```HTML
<body>
  <script>
    window.onload = function() {
      const divEl = document.querySelector("div")
      divEl.style.backgroundColor = "red"
    }
  </script>
  <div style="width: 100px; height: 100px;"></div>
</body>
```

### DOMContentLoaded

浏览器已经加载了html, 并且加载了DOM树, 但是外部的图片等等一些外部资源还没有加载完成.

使用方法:

```JavaScript
window.addEventListener("DOMContentLoaded", function() {
  // ...
})
```



------

## 常见的键盘事件

常见的键盘事件有:

- onkeydown: 某个按键被按下, 并且是在按下的瞬间触发.
- onkeypress: 某个按键被按下并且是持续按下, 但是在按钮按下并会持续按住的时候会重复触发.
- onkeyup: 某个按键被松开.

`onkeypress`事件在一些特定情况下可能存在一些不一致性和兼容性问题，而`onkeydown`和`onkeyup`事件更加可靠和一致。因此，建议在实际开发中**优先使用**`onkeydown`和`onkeyup`事件来处理键盘按键事件。

### key和code

我们可以通过key和code来区分按下的键:

```JavaScript
inputEl.onkeydown = function(event) {
    console.log(event.key, event.code)
}
```

code: "按键代码"("keyA", "ArrowLeft"等), 特定于键盘上按键的物理位置.

key: 字符("A", "a"等), 对于非字符的按键, 通常具有于code相同的值.

比如下面的案例: 为用户添加按下Enter时候触发搜索按键:

```JavaScript
const inputEl = document.querySelector("input")
const btnEl = document.querySelector("button")
// 搜索功能
btnEl.onclick = function() {
  console.log("按钮点击搜索:", inputEl.value)
} 
// 用户按键抬起后进行搜索
inputEl.onkeyup = function(event) {
  if (event.code === "Enter"){
    console.log("Enter抬起搜索:", inputEl.value)
  }
}
```

我们可以用event的key或者code去确定激活的是那些按键.

### 按下s, 网页自动获取搜索框焦点

```JavaScript
document.onkeyup = function(event){
    console.log(event.key, event.code)
    if (event.code === "KeyS") {
        console.log("在网页中点击了一个S")   
        input.focus() // 获取焦点的方法
    }
}
```

