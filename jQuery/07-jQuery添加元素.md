# 添加元素

添加元素有四个常用的方法：

- append() - 在被选元素的结尾插入内容
- prepend() - 在被选元素的开头插入内容
- after() - 在被选元素之后插入内容
- before() - 在被选元素之前插入内容

```html
<p><b>这是一个段落</b></p>
<script src="./jquery-3.7.1.js"></script>
<script>
  $("p").append("在结尾插入")
  $("p").prepend("在开头插入")
  $("p").after("在所选元素之后插入内容")
  $("p").before("在所选元素之前插入内容")
</script>
```

