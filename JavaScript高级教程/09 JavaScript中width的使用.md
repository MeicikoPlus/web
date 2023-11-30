# With语句的使用

**with语句**  扩展了一个语句的作用域链。

使用语句：

```JavaScript
var obj = {
  message "hellow world"
}

width (obj) {
  console.log(message)
}
```

可以在查找message的时候去obj中查找，如果obj中没有，再去全局作用域中查找。

目前不推荐使用with！

因为可能存在一些混淆错误和兼容性的问题。

