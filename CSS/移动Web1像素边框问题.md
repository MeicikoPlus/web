# 1像素问题的产生

由于设备像素比DPR的差异，在css里写的1px的时候，由于它是逻辑像素，根据设备像素比DPR渲染到手机上的1px实际上是 2×2 个像素点，有的屏幕甚至用到了 3×3 个像素点，所以 border: 1px 在移动端会渲染为 2px 的边框。

 由于每个设备的屏幕尺寸不一样，就导致每个物理像素渲染出来的大小也不同，如果在尺寸比较大的设备上，1px渲染出来的样子相当的粗矿，虽然用户在实际使用的时候，很难发现这 1px 的差异，但是设计师往往会在这 1px 上较劲，这就产生了经典的 “一像素问题”。

# 如何解决

核心思路，就是 在web中，浏览器为我们提供了**window.devicePixelRatio**来帮助我们获取dpr。在css中，可以使用媒体查询**min-device-pixel-ratio**，区分dpr：我们根据这个像素比，来算出他对应应该有的大小，但是暴露个非常大的兼容问题。

![image-20211116214004965](https://gitee.com/Jinxizhen/pic_resource/raw/master/images/image-20211116214004965.png)

其中Chrome把0.5px四舍五入变成了1px，而firefox/safari能够画出半个像素的边，并且Chrome会把小于0.5px的当成0，而Firefox会把不小于0.55px当成1px，Safari是把不小于0.75px当成1px，进一步在手机上观察iOS的Chrome会画出0.5px的边，而安卓(5.0)原生浏览器是不行的。所以直接设置0.5px不同浏览器的差异比较大，并且我们看到不同系统的不同浏览器对小数点的px有不同的处理。所以如果我们把单位设置成小数的px包括宽高等，其实不太可靠，因为不同浏览器表现不一样。

> 最简单的解决办法，就是用图片做边框，只是修改颜色不太方便。除此之外，还有两种常用的办法。

# transform:scale

使用伪类 ::after 或者 ::before 创建 1px 的边框，然后通过 @media 适配不同的设备像素比，然后调整缩放比例，从而实现一像素边框。

首先用伪类创建边框

```css
.border-bottom{
    position: relative;
    border-top: none !important;
}
.border-bottom::after {
    content: " ";
    position: absolute;
    left: 0;
    bottom: 0;
    width: 100%;
    height: 1px;
    background-color: #e4e4e4;
    -webkit-transform-origin: left bottom;
    transform-origin: left bottom;
}
```

然后通过媒体查询来适配：

```css
/* 2倍屏 */
@media only screen and (-webkit-min-device-pixel-ratio: 2.0) {
    .border-bottom::after {
        -webkit-transform: scaleY(0.5);
        transform: scaleY(0.5);
    }
}

/* 3倍屏 */
@media only screen and (-webkit-min-device-pixel-ratio: 3.0) {
    .border-bottom::after {
        -webkit-transform: scaleY(0.33);
        transform: scaleY(0.33);
    }
}
```

这种办法的边框并不是真正的 border，而是高度或者宽度为 1px 的块状模型，一般都用来画分割线。

不过，这种方式可以满足各种场景，如果需要满足圆角，只需要给伪类也加上`border-radius`即可。

# viewport

网页的内容都渲染在 viewport 上，所以设备像素比的差异，直接影响的也是 viewport 的大小。

通过 js 获取到设备像素比，然后动态添加 `<meta>` 标签。

```html
<script type="text/javascript">
   (function() {
       var scale = 1.0;
       if (window.devicePixelRatio === 2) {
           scale *= 0.5;
       }
       if (window.devicePixelRatio === 3) {
           scale *= 0.333333;
       }
       var text = '<meta name="viewport" content="initial-scale=' + scale + ', maximum-scale=' + scale +', minimum-scale=' + scale + ', width=device-width, user-scalable=no" />';
       document.write(text);       
    })();
</script>
```

# 其他方案

| 方案/优缺点              | 兼容性                            | 颜色                       | 圆角                                    | 总结                                                         |
| ------------------------ | --------------------------------- | -------------------------- | --------------------------------------- | ------------------------------------------------------------ |
| 0.5px 边框               | 无法兼容安卓设备、 iOS 8 以下设备 | 支持                       | 支持                                    | 简单，不需要过多代码                                         |
| 使用 border-image        | 无                                | 修改颜色麻烦，需要替换图片 | 圆角需要特殊处理，并且边缘会模糊        | 可以设置单条,多条边框，且没有性能瓶颈的问题                  |
| 使用background-image实现 | 无                                | 修改颜色麻烦, 需要替换图片 | 圆角需要特殊处理，并且边缘会模糊        | 可以设置单条,多条边框，没有性能瓶颈的问题                    |
| 多背景渐变实现           | 多背景图片有兼容性问题            | 支持                       | 不支持                                  | 可以实现单条、多条边框，边框的颜色随意设置，但是代码量不少   |
| 使用 box-shadow 模拟边框 | 无                                | 边框有阴影，颜色变浅       | 支持                                    | 代码里少，可以满足所有场景                                   |
| viewport + rem 实现      | 无                                | 支持                       | 支持                                    | 所有场景都能满足，一套代码，可以兼容基本所有布局，但是老项目修改代价过大，只适用于新项目 |
| 伪类 + transform 实现    | 无                                | 支持                       | 支持(伪类和本体类都需要加border-radius) | 所有场景都能满足，对于已经使用伪类的元素(例如clearfix)，可能需要多层嵌套 |