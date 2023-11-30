# 多倍图

一张100px * 100px（css像素）的图片，如果在手机上打开，该图片会按照相应的物理像素比放大倍数，进而会使图片失真。

比如：`100px*100px`在dpr为1的设备上显示就是`100px*100px`，但是在dpr为2的设备上显示就是`200px*200px`，图片被放大了，这样图片就会模糊。为了解决这个图片失真的问题，通常我们会使用倍图来提高图片的质量，通常使用的多倍图是二倍图。

二倍图：设计的图片尺寸比实际的图片尺寸大2倍，这种方式就是2倍图，当然还存在3倍图、4倍图等

对于二倍图的使用方法：设计一张放大二倍后的图片：图片尺寸为 200px *200px，然后再用css样式进行缩小为原来的一半大小：图片css样式尺寸设置为：100px * 100px (css像素)，这时再用该图片，就不会出现图片失真的情况，适用于img插入图片，也适用于背景图片。

# 对图片的处理

加载网页时，平均`60%`以上的流量来自 **加载图片**。指定图像宽度时使用**相对单位**，**防止意外溢出视口**，如 `width: 50%`，将图片宽度设置为包含元素宽度的 `50%`。因为 **css** 允许内容溢出容器， 需要使用**`max-width: 100%` 来保证图像及其他内容不会溢出**。使用 img 元素的 `alt` 属性提供描述，描述有助于提高网站的可访问性，能提供语境给屏幕阅读器及其他辅助性技术。**维护自适应页面中图片宽高比固定**比较常用的方法是使用**`padding`**设置。对于不同`dpr`以及不同分辨率/尺寸的屏幕，为了避免资源浪费和等待时间延长，需要针对不同的屏幕使用合适的图片，加载的图片分为通过标签引入的图片和背景图片。

# srcset 和 sizes

**对于`<img>`引入的图片，如果想要图片适应不同像素密度的屏幕，并且屏幕上显示图片的实际尺寸相同，使用**`srcset`**属性用来指定多张图像。**它的值是一个逗号分隔的字符串，每个部分都是一张图像的 URL，后面接一个空格，后接是像素密度描述符。浏览器根据当前设备的像素密度，选择需要加载的图像。如果`srcset`属性都不满足条件，那么就加载`src`属性指定的默认图像。

- `srcset` 定义了允许浏览器选择的图像集，以及每个图像的大小（使用w单位），浏览器根据宽、高和像素密度来加载相应的图片资源。
  - 属性格式：图片地址 宽度描述w 像素密x，多个资源之间用逗号分隔。
- `sizes`定义了一组媒体条件（例如屏幕宽度），指明当某些媒体条件为真时，什么样的图片尺寸是最佳选择，给浏览器提供一个预估的图片显示宽度。
  - 属性格式：媒体查询 宽度描述（支持px），多条规则用逗号分隔


```html
 <!--srcset属性给出了三个图像URL，适应三种不同的像素密度。
	在iPhone6上面加载foo@2x.jpg;
	在iPhone6Plus上面加载foo@3x.jpg;
-->
<img srcset="foo@1x.jpg 1, foo@2x.jpg 2x, foo@3x.jpg 3x" src="foo@2x.jpg">
```

如果想要针对不同屏幕，使用不同分辨率版本和尺寸的图片，使用属性`srcse` 和 `sizes` 。

```html
<!-- 视口<=320px 时图片宽度为 280px； 
	视口<=480px时，图片宽度为 440px; 
	其他情况为 800px -->
<img srcset = "foo-320w.jpg 320w, foo-480w.jpg 480w, foo-800w.jpg 800w"
     sizes = "(max-width: 320px) 280px, (max-width: 480px) 440px, 800px"
     src = "foo-800w.jpg">
```

> 浏览器的查询过程：

- 查看设备宽度；
- 检查`sizes`列表中哪个媒体条件是第一个为真；
- 查看给予该媒体查询的槽大小；
- 加载`srcset`列表中引用的最接近所选的槽大小的图像

# 异步加载

**< img> 引入的图片，使用js自带的异步加载图片。**根据不同的`dpr`，加载不同分辨率的图片。

```html
<img id="img" data-src1x="xxx@1x.jpg" data-src2x="xxx@2x.jpg" data-src3x="xxx@3x.jpg"/>
```

```javascript
var dpr = window.devicePixelRatio;
if(dpr > 3){
    dpr = 3;
};

var imgSrc = $('#img').data('src'+dpr+'x');
var img = new Image();
img.src = imgSrc;
img.onload = function(imgObj){
    $('#img').remove().prepend(imgObj);//替换img对象
};
```

# picture

**为不同的视口提供不同的图片，使用`<picture>`标签。**`<picture>`是`html5`中定义的一个容器标签，内部使用`<source>`和`<image>`，**浏览器会匹配`<source>`的`type`,`media`,`srcset`等属性，找到最适合当前布局 / 视口宽度 / 设备像素密度的图像进行加载。**这里的`<img>`标签是浏览器不支持`picture`元素，或者支持`picture`但没有合适的媒体定义时的后备，不能省略。

```html
<picture>
  <!-- 
	屏幕尺寸<=320使用cat-vertical.jpg；
	屏幕尺寸<=600px 使用cat-horizontal.jpg;
	屏幕尺寸>600px 使用cat-.jpg;
	-->
  <source media="(max-width: 320px)" srcset="cat-vertical.jpg">
  <source media="(max-width: 600px)" srcset="cat-horizontal.jpg">
  <img src="cat.jpg" alt="cat">
</picture>
```

# Image-set

**对于背景图片，使用`image-set`根据用户设备的分辨率匹配合适的图像**， 同时要考虑兼容性问题。

```html
<style>
.css {
    background-image: url(1x.png); /*不支持image-set的情况下显示*/
    background: -image-set(
            url(1x.png) 1x,/* 支持image-set的浏览器的[普通屏幕]下 */
            url(2x.png) 2x,/* 支持image-set的浏览器的[2倍Retina屏幕] */
            url(3x.png) 3x/* 支持image-set的浏览器的[3倍Retina屏幕] */
    );
}
</style>
```

# media query

> **对于背景图片，使用媒体查询自动切换不同分辨率的版本**

```html
<style>
/* 普通显示屏(设备像素比例小于等于1)使用1倍的图 */
.css{
    background-image: url(img_1x.png);
}

/* 高清显示屏(设备像素比例大于等于2)使用2倍图  */
@media only screen and (-webkit-min-device-pixel-ratio:2){
    .css{
        background-image: url(img_2x.png);
    }
}

/* 高清显示屏(设备像素比例大于等于3)使用3倍图  */
@media only screen and (-webkit-min-device-pixel-ratio:3){
    .css{
        background-image: url(img_3x.png);
    }
}
</style>
```