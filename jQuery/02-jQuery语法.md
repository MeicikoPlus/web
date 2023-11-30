# 学习语法

通过 `jQuery` ，可以选取，查询 `HTML` 元素，并对它们执行操作；



## 基础语法

`jQuery` 是通过选取 `HTML` 元素，并对选取的元素执行某些操作：

```js
基础语法：$(selector).action()
```

- 美元符号定义 `jQuery`
- 选择符（`selector`）"查询"和"查找" HTML 元素
- `jQuery` 的 `action()` 执行对元素的操作

使用实例：

```html
<div class="box">
	<p>我是一个段落</p>
</div>
<script src="./jquery-3.7.1.js"></script>
<script>
  $(".box").hide() // 隐藏box元素
  $(".box p").hide() // 隐藏box元素中的p元素
</script>
```



## 文档就绪事件

==为了防止文档在完全加载（就绪）之前运行 `jQuery` 代码，即在DOM加载完成后再对DOM进行操作==，防止文档没有加载完全之前就运行函数导致的操作失败。

第一种写法：

```js
$(document).ready(function(){
	// jQuery代码
});
```

第二种写法（效果同上）：

```js
$(function(){
  // 开始写 jQuery 代码...
})
```



## jQuery 选择器

- `jQuery` 选择器允许您对 HTML 元素组或单个元素进行操作。

- `jQuery` 选择器基于元素的 id、类、类型、属性、属性值等"查找"（或选择）HTML 元素。 它==基于已经存在的 CSS 选择器==，除此之外，它还有一些自定义的选择器。

- `jQuery` 中==所有选择器都以美元符号开头==：$()。

### 元素选择器

```js
获取页面中所有的p元素：$("p")
```

### id选择器

```js
通过id获取某个元素：${"#btn"}
```

### class选择器

```js
通过class来查找所有拥有此类名的元素：$(".test")}
```

```html
<div class="box">1</div>
<div class="box">2</div>
<div class="box">3</div>
<div class="box">4</div>
<script src="./jquery-3.7.1.js"></script>
<script>
  $(".box").click(function() {
    console.log(this.textContent)
  })
</script>
```

### 其他选择器

```js
获取所有元素：$("*")
```

```js
  获取ul中的第一个li元素：$("ul li:first-child")
```

```js
选取所有 target 属性值等于 "_blank" 的 <a> 元素：$("a[target='_blank']")
```

```js
选取所有 target 属性值不等于 "_blank" 的 <a> 元素：$("a[target!='_blank']")
```

```js
选取所有 type="button" 的 <input> 元素 和 <button> 元素: $(":button")
```

```js
选取偶数位置的 <tr> 元素：$("tr:even")
```

```js
选取奇数位置的 <tr> 元素：$("tr:odd")
```

