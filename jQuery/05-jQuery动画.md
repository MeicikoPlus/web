# 动画效果

## animate

`jQuery` 有自己的 `animate` 方法，可以用来创建自定义的动画。

```js
语法：$(selector).animate({params},speed,callback)
```

- 参数一（必须）：定义形成动画的CSS属性；
- 参数二（可选）：规定效果的时长，可以取以下值：slow，fast，毫秒；
- 参数三（可选）：动画完成后执行的回调函数；

需要注意的是，`jQuery` 动画==无法直接去影响颜色的改变==，如果想要对颜色进行修改，需要需要从 [jquery.com](http://jquery.com/download/) 下载 [颜色动画](http://plugins.jquery.com/color/) 插件。

使用案例：

```html
<div class="box"></div>
<button id="hide">开始动画</button>
<script src="./jquery-3.7.1.js"></script>
<script src="https://code.jquery.com/color/jquery.color-2.2.0.js" integrity="sha256-gvMJWDHjgDrVSiN6eBI9h7dRfQmsTTsGU/eTT8vpzNg=" crossorigin="anonymous"></script>
<script>
  $(document).ready(function() {
    $("#hide").click(function() {
      $(".box").animate({
        width: "200px",
        height: "200px",
        backgroundColor: "rgb(0, 0, 0)"
      }, 1000, function() {
        console.log("动画执行结束")
      })
    })
  })
</script>
```



## animate - 使用相对值

`jQuery` 允许使用相对值，只需要在值的前面加上 `+=` 或者 `-=` :

```js
$(document).ready(function() {
  $("#hide").click(function() {
    $(".box").animate({
      width: "+=100px",
      height: "+=100px",
      backgroundColor: "rgb(0, 0, 0)"
    }, 1000, function() {
      console.log("动画执行结束")
    })
  })
})
```



## animate - 使用队列功能

`jQuery` 提供了针对动画的队列功能。

这意味着如果您在彼此之后编写多个 `animate()` 调用，`jQuery` 会创建包含这些方法调用的"内部"队列。然后==逐一运行==这些 `animate` 调用。

```js
$(document).ready(function() {
  $("#hide").click(function() {
    const divEl = $(".box")
    divEl.animate({height:'300px',opacity:'0.4'},"slow");
    divEl.animate({width:'300px',opacity:'0.8'},"slow");
    divEl.animate({height:'100px',opacity:'0.4'},"slow");
    divEl.animate({width:'100px',opacity:'0.8'},"slow");
  })
})
```





## 停止动画

### stop

`jQuery` 的 `stop` 方法用于停止动画或者效果，stop() 方法适用于所有 jQuery 效果函数，包括滑动、淡入淡出和自定义动画。

注：jQuery的`stop()`函数可以对原生的CSS动画产生一些影响，但它并不能完全控制原生的CSS动画。

```js
$(selector).stop(stopAll,goToEnd)
```

- 可选的 stopAll 参数规定是否应该清除动画队列。默认是 false，即仅停止活动的动画，允许任何排入队列的动画向后执行。
- 可选的 goToEnd 参数规定是否立即完成当前动画。默认是 false。

使用案例：

```js
// 在参数stopAll为默认值的时候，仅仅停止活动的动画
$(document).ready(function() {
  $("#hide").click(function() {
    const divEl = $(".box")
    divEl.animate({height:'300px',opacity:'0.4'}, 4000);
    divEl.animate({width:'300px',opacity:'0.8'}, 4000);
    divEl.animate({height:'100px',opacity:'0.4'}, 4000);
    divEl.animate({width:'100px',opacity:'0.8'}, 4000);
  })
  $("#pause").click(function() {
    $(".box").stop()
  })
})
```

