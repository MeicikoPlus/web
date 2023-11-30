# 导读

viewport是移动端跨屏适配的基石，吃透这一概念，任何复杂多变的适配需求，都可以手到擒来。

移动端开发中，有一个躲避不掉的HTML meta 声明 `<meta name="viewport">` 。通常被用来做跨屏适配，常见声明如下：

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no">
```

# 视口 viewport

viewport 中文译作“视口”。

维基百科①的解释为：

在计算机图形学理论中，当将一些对象渲染到图像时，存在两个类似区域的相关概念。(视口和窗口)

视口是一个以特定于渲染设备的坐标表示的区域（通常为矩形）。视口范围内的图像会以剪切的形式，投影到到世界坐标窗口中，完成图像的可视化展示。

在 Web 浏览器中，视口是整个文档的可见部分。如果文档大于视口，则用户可以通过滚动来移动视口。

白话描述一下：

- 计算机把图像渲染到显示器的过程中，会先把图像画在一个逻辑层的画布上，然后从这个画布中框选一部分，将其投影到显示层。
- 这个选框就是视口，显示层就是窗口。
- 在浏览器中，我们可以通过滚动条来移动视口以看到更多网页内容。

更形象的视口解释：

![10f3bc7e84e5aeab73b21dc79ee287d9.png](https://gitee.com/Jinxizhen/pic_resource/raw/master/images/10f3bc7e84e5aeab73b21dc79ee287d9.png)

如这张某宝的商品放大效果图，左半图为计算机看到的逻辑层画布，上方半透明选框为视口(viewport)，右半图为浏览器窗口，即用户看到的部分。

**视口（viewport）**可以理解为浏览器显示页面内容的屏幕区域。 

## viewport 的缩放与平移

浏览器中，对页面进行放大的时候，视口的大小如何变化？**视口会变小**。

因为，浏览器窗口中所浏览图像的放大，是依赖于视口的缩小来实现的。

如果不好理解，可以参照下图动画来感受一下。（上面蓝框表示底层画布、红框表示视口，下面表示用户在浏览器窗口中看到的页面）

![](https://i.loli.net/2021/11/19/gAO8LiW4ut1dScI.gif)


同理，当浏览器窗口比较小，而我们想要看到页面下面的内容时，我们需要向下滚动滚动条，浏览器在实现这个的过程中所依赖的，便是视口的下移。

## viewport的DOM API

关于上面的解释，我们来验证一下。

目前已被标准实现的 API 中，有两个 DOM 属性可以用来获取视口的大小。

以宽度为例：

- document.documentElement.clientWidth（不含滚动条）

- window.innerWidth（含滚动条）

<img src="https://gitee.com/Jinxizhen/pic_resource/raw/master/images/37895ffcf8c79b47f865c090fae06acf.png" alt="37895ffcf8c79b47f865c090fae06acf.png" style="zoom: 67%;" />

如图，PC Chrome 中试验，确实如之前解释，放大到 200%后，视口大小缩小了一倍。（小数点默认四舍五入了）

注意：

- 在移动端的浏览器中，对页面手动捏合做放大时，document.documentElement.clientWidth 不会有任何变化。window.innerWidth在 iOS 中会等倍数缩小，在 Android 的不同浏览器中表现差异较大。

- 如果有需要获取初始视口宽度的需求，建议使用document.documentElement.clientWidth  ②。

# 移动端的 viewport

看起来 viewport 并没有太多复杂之处，但是 2010 年左右，移动端时代来了。

注：移动设备的显著特点是屏幕小，考虑到国际社会通行的水平阅读习惯，下文我们只讨论宽度。

## 窄屏设备的问题

移动互联网的早期，屏幕设备的物理像素点宽度多数在 320、480、640 等。而互联网世界的绝大多数站点又是针对 PC 设计的，其文档宽度普遍在 800px 以上。

如果浏览器和针对 PC 制作的网页都不做任何处理，那么在窄屏设备上加载网页，我们看到的效果便是默认显示网页的左上角部分，然后通过水平和竖直方向的滚动来浏览网页的其他部分。

首先，我们看不到网页全局的样子。其次，我们需要通过不断的滚动来保证阅读内容的连续性。这样的体验，有点过于糟糕了。

## 放大的viewport

为了优化“最初为 PC 设计的网页”在移动设备的浏览体验，移动浏览器厂商们想了一个方案，那就是增大页面载入时初始视口的宽度，比如 Android 和 iOS 都比较常见的 980px。

在手机端，html的大小都是980px，为什么？ 

> 这主要是历史原因导致的，因为在移动设备刚流行的时候，网站大多都是pc端的，pc端的页面宽度一般都比较大， 移动设备的宽度比较小，如果pc端页面直接在移动端显示的话，页面就会错乱。 为了解决这个问题，移动端html的大小直接就定成了980px。
>
> 在pc端，html的大小默认是继承了浏览器的宽度，即浏览器多宽，html的大小就是多宽， 但是在移动端，多出来了一个视口的概念，视口说白了就是介于浏览器与html之间的一个东西， 视口的宽度默认定死了980px，因此html的宽度默认就是980px，视口的特点是能够根据设备的宽度进行缩放。 将视口宽度和手机屏幕一样宽，网页基于视口进行布局，网页的布局宽度就和手机屏幕一致了。
>
> 随着智能手机的出现， 980px 这个布局视口宽度出现了，也就是说在手机上浏览器上，默认的布局视口宽度是 980px，默认开发者在 980px 的页面上使用代码进行页面绘制。但是这就导致了一个情况：980px的宽度大于大部分手机屏幕的宽度，为了将页面显示完全，只能对原来的页面进行缩放，如果不进行缩放，那么就需要左右拖动来浏览。

按照  viewport 的缩放，如此的设计，会把逻辑层画布中 980px 的图像投影显示到 414px 的屏幕上，看到的效果便是一个挤在一起看不清楚细节的缩小版页面。

![image-20211119002135618](https://gitee.com/Jinxizhen/pic_resource/raw/master/images/image-20211119002135618.png)

如上图，页面载入时，我们可以一眼看到整个页面的样子了。

不过，该方案依然会有很多问题：

- 对缩小版页面内细节内容的浏览，依然要依靠放大和滚动，体验不好；

- 如果 为 PC 设计的 网页的 CSS 宽度描述大于 980px，那么在移动端展示时，初始页面依然会有滚动条；

- 限制了依据视口宽度做媒体查询(Media queries)机制的有效性，因为视口宽度初始为 980px，浏览器不会以 640px、480px 或更低分辨率来启动对应的媒体查询。

注：媒体查询请注意区分"@media screen and (xxx){}"中的min-device-width 和min-width。前者依据的是设备逻辑宽度(screen.width)，后者依据的是视口宽度(document.documentElement.clientWidth)。

## 可定制的viewport

浏览器厂商：“既然我说的数，你们都各种意见，那好吧，你们自己定吧。”

为了解决上述固定 viewport 宽度的方案所引发的各种问题，Apple 在 iOS Safari 中首先引入了Viewport Meta Tag ，允许 Web 开发人员定制视口的大小和缩放比例，后续其他的移动浏览器厂商也都支持了此标记（约 2010 年）。

虽然至今 W3C 都未将此标记列入标准，但是，这并不影响我们使用它。

如前面 viewport 概念的解释，css 中同样 px 大小的宽高描述，在不同大小的视口状态下，用户在浏览器窗口中看到的页面大小的效果是不同的。有了定制视口的能力，我们可以轻松的做到以下几点③：

- 针对较宽(比如 2000px) PC 设计的页面，我们可以设置 viewport 宽度为 2000，以使得移动设备中初始渲染的页面效果刚好不出现滚动条；

- 利用了媒体查询做了移动端适配的页面，我们可以设置 viewport 宽度为 device-width，以保证媒体查询技术的有效性，同时还能保证横竖屏切换时，px 单位做大小描述的页面元素的视觉大小一致性；

- 需要充分利用屏幕物理像素点做 1 像素极细边线的页面，我们可以设置 viewport 缩放倍数为 1/dpr，以使得 css 中的 1px 刚好对应设备物理像素中的 1 个点；

- 需要根据屏幕宽度弹性伸缩的页面，我们可以结合各种相对长度单位(%/rem/vw 等)，设置合适的 viewport，以实现布局伸缩和内容大小固定的完美统一。

# Viewport Meta Tag 的使用

我们可以在 Apple 或者 [MDN](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Viewport_concepts) 的开发者文档中查看Viewport Meta Tag 的具体用法说明。

需要注意的一点是，目前只有移动端的浏览器支持这一声明方式，PC上是无效的。

在那些难以界定移动还是 PC 的设备中，这种区分可能会存在一些问题，有一些 Web 组织，如 WICG(Web Platform Incubator Community Group)目前在尝试推动解决这个问题。这里不做更多讨论④。

## viewport 属性表

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no">
```

通过设置`<meta name="viewport">`，提供有关视口初始大小的信息，供**移动设备**使用。属性值为：

| 属性            | 属性值                 | 描述                                                |
| :-------------- | :--------------------- | :-------------------------------------------------- |
| width         | 数值 / device-width  | 视口宽度(px)                                        |
| height        | 数值 / device-height | 视口高度(px)                                        |
| initial-scale | 0.0 ~ 10.0             | 设备宽度device-width与视口宽度之间的初始缩放比例    |
| maximum-scale | 0.0 ~ 10.0             | 缩放最大值，必须大于等于minimum-scale的值             |
| minimum-scale | 0.0 ~ 10.0             | 缩放最小值必须大于等于maximum-scale的值                     |
| user-scalable | yes/no            | 是否允许用户缩放，默认`yes`，为`no`时用户不能缩放网页，也可以用1或者0 |
| viewport-fit | contain/cover | 视口填充屏幕的方式。默认值为contain |

## viewport 属性举例

在本小节，我们以iPhone6s手机+Safari浏览器举例，对上述属性做详细说明。

如未做特殊说明，均只讨论竖屏模式。

设备参数说明：

- 操作系统：iOS 12.3.1

- 屏幕物理分辨率：750*1334

- 屏幕逻辑分辨率：375*667 (screen.width/height)

- 设备像素比(dpr)：2 (window.devicePixelRatio)

- 浏览器默认视口宽度：980 (document.documentElement.clientWidth)

### width

```html
<meta name="viewport" content="width=1000" />
```

- document.documentElement.clientWidth 输出 1000

- div 宽度 1000px 时，横向刚好铺满屏幕，超过出现横向滚动条

```html
<meta name="viewport" content="width=device-width" />
```

- 效果等同于 width=375

- document.documentElement.clientWidth 输出 375

- div 宽度 375px 时，横向刚好铺满屏幕，超过出现横向滚动条

### initial-scale

```html
<meta name="viewport" content="initial-scale=1" />
```

- 效果等同于 width=device-width

```html
<meta name="viewport" content="initial-scale=2" />
```

- document.documentElement.clientWidth 输出 188 (375/2)

- div 宽度 188px 时，横向刚好铺满屏幕，超过出现横向滚动条
- scale 倍数越小，视口越大

此处插入一个问题：

iPhone6S 的 safari 中，不做任何 viewport 设置情况下，默认 initial-scale 的值为多少？

### maximum-scale / minimum-scale

```html
<meta
  name="viewport"
  content="initial-scale=2,minimum-scale=1,maximum-scale=3"
/>
```

预期页面初始 1 倍，最小可以缩小到 0.5 倍，最大放大到 2 倍。但是实际表现并非如此：

- 小米 9 的系统浏览器表现符合预期;

- iOS 中 所有 Web 容器均无法缩放 到 比 initial-scale 更小的倍数，即使 minimum-scale 声明了一个更小且合理的取值；

- iOS 微信(7.0.5)的 webview 中，遵守了最大 3 倍声明，但 Safari 可以放大到比 3 倍更高的倍数。

iOS10 开始，为了提高网页在 Safari 中的可访问性，Safari 限制了最小倍数(minimum-scale)并忽略了 最大倍数(maximum-scale) 的声明。

Android 和 iOS 在不同版本不同厂商的 Web 容器中，此属性的表现可能存在较大程度的不一致，请谨慎使用。

### user-scalable

```html
<meta name="viewport" content="initial-scale=1,user-scalable=no" />
```

- Safari 中依然无法缩小可以放大，微信中无法缩放；

- Android 未做测试。

同 4.2.3，此属性同样存在兼容问题，请谨慎使用。

### viewport-fit

```html
<meta name="viewport" content="initial-scale=1,viewport-fit=cover" />
```

此属性为 2017 年 Apple 为了解决 iPhoneX 手机的刘海屏问题，增加的新属性。

相关技术细节和兼容性本文不做更多讨论，详情可以阅读文末参考链接⑤：

## width和initial-scale的取值冲突

同理，width=device-width 和 initial-scale=1 也是等效的。（device-width 对应数值在竖屏模式下为 375，横屏模式下为 667）

既然，两个属性的作用都是设置初始视口大小，那同时设置且存在冲突的情况下，浏览器会怎么处理呢？优先级规则是按书写顺序还是宽度大小？比如下面这样：

```html
<meta name="viewport" content="width=device-width,initial-scale=2" />
```

Safari 的运行结果是“按宽度大小”取更大的，但是这样的对比研究并没有任何意义。因为并没有相应的规范约束这件事情，浏览器的兼容表现肯定是千差万别。

作为开发者，我们要做的，就是避免冲突。要么只写一个，要么两个都计算正确。从语义表达角度看，建议只设置"width"。从计算方便角度看，可以只设置 initial-scale。

以上参照：https://blog.csdn.net/weixin_32821579/article/details/112434380

# 参考资料

[1].https://en.wikipedia.org/wiki/Viewport

[2].https://developer.mozilla.org/en-US/docs/Web/CSS/Viewport_concepts

[3].https://developer.apple.com/library/archive/documentation/AppleApplications/Reference/SafariWebContent/UsingtheViewport/UsingtheViewport.html

[4].https://github.com/WICG/visual-viewport

[5].https://developer.mozilla.org/en-US/docs/Web/CSS/@viewport/viewport-fit

# 移动端视口分类

移动端涉及**布局视口**（Layout Viewport）、**视觉视口**（Visual ViewPort）和**理想视口**（Ideal ViewPort）。

## 布局视口

布局视口是看不见的，浏览器厂商设置的一个固定值，一般是768px~1024px之间。这里我们可以认为它是一个画板用于展示网页。

布局视口相对于用户来说并没有什么影响，布局视口对于开发者来说实际意义重大，它规定开发者需要以这么一个宽度来进行开发，只有将布局视口按照理想视口的宽度来开发，显示出来的页面兼容性才更好、视觉效果和体验也更好。

一般移动设备的浏览器都默认设置了一个布局视口，用于解决早期的PC端页面在手机上显示的问题。iOS, Android基本都将这个视口分辨率设置为 980px，所以PC上的网页大多都能在手机上呈现，只不过元素看上去很小，一般默认可以通过手动缩放网页。

<img src="https://gitee.com/Jinxizhen/pic_resource/raw/master/images/image-20211116230211429.png" alt="image-20211116230211429" style="zoom:67%;" />

## 视觉视口

视觉视口是浏览器可视区域的大小，即用户看到的网页的区域（理解为网页的可视区域）。在 PC 端，浏览器窗口可以随意改变大小，我们可以直观的看到。而在移动端端，大部分手机、平板的浏览器是不支持改变浏览器宽高的，所以其视觉视口就是其屏幕大小。

<img src="https://gitee.com/Jinxizhen/pic_resource/raw/master/images/image-20211116230316297.png" alt="image-20211116230316297" style="zoom:67%;" />

## 理想视口

它对设备来说是最理想的布局视口，用户不需要对页面进行缩放就能完美的显示整个页面。这个理想的宽度是指以CSS像素单位计算的宽度，即屏幕的逻辑像素宽度，跟设备的物理宽度没有关系。在css中，这个宽度就相当于100%的所代表的那个宽度。

通过设置 `<meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">`实现

于是上述的meta设置，就是我们的理想设置，他规定了我们的视口宽度为屏幕宽度，初始缩放比例为1，就是初始时候我们的视觉视口就是理想视口！

理想视口的出现和乔布斯老爷子有关，他规定：布局视口宽度 = 视觉视口宽度 = 设备宽度（网页内容宽度）

手机浏览器窗口大小无法调整，所以视觉视口宽度就是设备宽度，精简写法：布局视口宽度 = 视觉视口宽度

意义：

- 网页内容宽度 = 布局视口宽度

其实也很好理解，就是规定开发者在开发过程中，内容的宽度不能超出布局视口的宽度。

- 布局视口宽度 = 视觉视口宽度

相对于浏览器窗口可以调整的 PC 端而言，视觉视口宽度就是浏览器窗口的宽度。相对于不能调整大小的移动端而言，视觉视口宽度就是设备屏幕大小宽度。这两个宽度相等的意义在于让设备可以将内容宽度显示完整，也就是不能左右滑动，可以上下滑动。也就是这种布局理念才造就了用户从上往下浏览内容的习惯。

其实，即使你布局视口=视觉视口宽度，在内容宽度≠布局视口宽度的情况下，会出现的情况就是内容过宽，需要进行缩放来显示整个内容，但是这就导致了内容的缩小，看起来费劲。但是如果你不缩小就只能显示一部分宽度，就需要左右滑动。

所以乔布斯提出了这种“理想视口”的概念，从而在开发行为方面进行了规范、在浏览器厂商配置进行了相对统一、在用户习惯上达到了满足。由此，三个方面都达到了一种标准化的统一。

乔布斯提出的理想视口概念其实意义重大，他不仅让开发者、浏览器厂商、用户习惯各自形成了一个标准，同时理想视口也是后续很多布局方案的基础。因为有了屏幕宽度 = 布局宽度这个概念后，布局方案都是以这个标准，以此来设计出了响应式布局和单独开发移动端的百分比布局方案。

viewport的深入理解: [点击打开链接](http://www.cnblogs.com/2050/p/3877280.html)

# 视口单位

移动互联网发展起来之后，CSS3引入了 vw、vh、vmin、vmax 长度单位，以方便移动端页面的开发。

vw（Viewport Width）、vh(Viewport Height)是基于视口的单位。

使用 vw、vh 能很好地解决宽度、高度适配问题，比使用 % 单位更高级！

- vw：相对于视口的宽度，视口被均分为100单位的vw。1vw等于视口宽度的1%
- vh：相对于视口的高度，视口被均分为100单位的vh。1vh等于视口高度的1%
- vmin 相对于视口的宽度或高度中较小的那个。其中最小的那个被均分为100单位的vmin 
- vmax 相对于视口的宽度或高度中较大的那个。其中最大的那个被均分为100单位的vmax 

![image-20211117192148565](https://gitee.com/Jinxizhen/pic_resource/raw/master/images/image-20211117192148565.png)

即可以指定 height: x vh;
也可以指定 height: x vw;
比单纯指定 height: x %; 要有用得多！