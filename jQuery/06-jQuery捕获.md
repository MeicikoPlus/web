# jquery - 捕获

`jQuery` 拥有可操作 `HTML` 元素和属性的强大方法。

## jquery - DOM

`jQuery` 中非常重要的部分，就是操作 DOM 的能力。

`jQuery` 提供一系列与 DOM 相关的方法，这使访问和操作元素和属性变得很容易。

### 获得内容

三个简单实用的用于 `DOM` 操作的 `jQuery` 方法：

- **text()** - 设置或返回所选元素的文本内容
- **html()** - 设置或返回所选元素的内容（包括 HTML 标签）
- **val()** - 设置或返回表单字段的值

使用示例：

```html
<p>这是一个<b>段落</b></p>
<script src="./jquery-3.7.1.js"></script>
<script>
  $(function() {
    const pEl = $("p")
    console.log(pEl.text()) // 这是一个段落
    console.log(pEl.html()) // 这是一个<b>段落</b>
  })
</script>
```

```html
<label for="text">请输入:</label>
<input type="text" id="text"><br>
<button id="get">获取内容</button>
<script src="./jquery-3.7.1.js"></script>
<script>
  $(function() {
    $("#get").click(function(event) {
      alert("text: " + $("#text").val())
    })
  })
</script>
```



### 获取属性

**prop()函数的结果:**

   1.如果有相应的属性，返回指定属性值。

   2.如果没有相应的属性，返回值是空字符串。

**attr()函数的结果:**

   1.如果有相应的属性，返回指定属性值。

   2.如果没有相应的属性，返回值是 undefined。

- ==对于HTML元素本身就带有的固有属性，在处理时，使用prop方法==。
- ==对于HTML元素我们自己自定义的DOM属性，在处理时，使用 attr 方法==。
- 具有 true 和 false 两个属性的属性，如 checked, selected 或者 disabled 使用prop()

```html
<a href="https://www.baidu.com" index="0">百度一下</a>
```

```js
console.log($("a").attr("href")) // https://www.baidu.com
```

```js
console.log($("a").attr("index")) // 0
```



### 设置内容和属性

可以使用上述中的三个相同的方法来设置内容：

- text() - 设置或返回所选元素的文本内容
- html() - 设置或返回所选元素的内容（包括 HTML 标记）
- val() - 设置或返回表单字段的值

```js
$("#btn1").click(function(){
    $("#test1").text("Hello world!");
});
$("#btn2").click(function(){
    $("#test2").html("<b>Hello world!</b>");
});
$("#btn3").click(function(){
    $("#test3").val("RUNOOB");
});
```

#### 设置属性

`jQuery attr()` 方法也用于设置/改变属性值。

```js
$("button").click(function(){
  $("a").attr("href","http://www.jQuery.com");
});
```

