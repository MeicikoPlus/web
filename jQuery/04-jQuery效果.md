# jquery 效果

## 隐藏和显示

`jQuery` 实现显示和隐藏使用的是 `hide()` 和 `show()` 这两个方法：

```js
语法如下：
$(selector).hide(speed,callback)
$(selector).show(speed,callback)
```

参数一： `speed` 可以固定隐藏、显示的速度，可以取一下值：slow，fast或者毫秒。

参数二：显示或者隐藏效果==结束后执行的回调函数==。

使用示例：

```html
<div class="box"></div>
<button id="hide">隐藏</button>
<button id="show">显示</button>
<script src="./jquery-3.7.1.js"></script>
<script>
  $("#hide").click(function() {
    $(".box").hide(1000) // 1000毫秒的过渡效果
  })
  $("#show").click(function() {
    $(".box").show("fast") // 速度较快的过渡效果
  })
</script>
```

### jQuery toggle()

通过 `jQuery`，可以使用 `toggle()`方法来切换 `hide()` 和 `show()` 方法：

```js
语法：$(selector).toggle(speed,callback)
```

```js
$("#hide").click(function() {
  $(".box").toggle(1000, "swing", function() {
    console.log("动画结束")
  })
})
```



## 淡入淡出

淡入淡出的效果使用的下面四种 `fade` 方法：

- fadeIn()
- fadeOut()
- fadeToggle()
- fadeTo()

### fadeln

这个方法用于淡入已隐藏的元素：

```js
$(selector).fadeIn(speed,callback)
```

参数一：规定过渡动画效果的时长，可以取slow、fast或者毫秒。

参数二：规定过渡动画结束后的回调函数。

### fadeOut

这个方法用于淡出可见元素：

```js
$(selector).fadeOut(speed,callback);
```

### fadeToggle

jQuery fadeToggle() 方法可以在 fadeIn() 与 fadeOut() 方法之间进行切换。

如果元素已淡出，则 fadeToggle() 会向元素添加淡入效果。

如果元素已淡入，则 fadeToggle() 会向元素添加淡出效果。

```js
$(selector).fadeToggle(speed,callback);
```



## 滑动效果

### slideDown

这个方法用于向下滑动元素：

```js
$(selector).slideDown(speed,callback);
```

### slideUp

这个方法用于向上滑入元素：

```js
$(selector).slideUp(speed,callback);
```

### slideToggle

Query slideToggle() 方法可以在 slideDown() 与 slideUp() 方法之间进行切换。

如果元素向下滑动，则 slideToggle() 可向上滑动它们。

如果元素向上滑动，则 slideToggle() 可向下滑动它们。

```js
$(selector).slideToggle(speed,callback);
```





