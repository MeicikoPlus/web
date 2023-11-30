# PC端的web 

在PC端电脑访问的web(网页网站) 页面一般固定宽度的居中在网页显示（**定宽居中**），由于不同浏览器使用的内核不同，web网页在不同的浏览器上显示存在兼容性。

## 浏览器内核

浏览器的内核引擎，基本上是三分天下

- **Trident**：IE 以Trident 作为内核引擎；
- **Gecko**：Firefox 是基于 Gecko 开发；
- **WebKit**：Safari、Google Chrome、Microsoft Edge、Opera浏览器、360浏览器、QQ浏览器、搜狗浏览器都是基于 Webkit 开发。

之前还有Presto内核，Presto是早期Opera的内核，但由于市场选择问题，主要应用在手机平台--Opera mini，后来，在2013年2月Opera宣布转向WebKit引擎，2013年4月Opera宣布放弃WebKit，跟随Google 的新开发的blink 引擎。

## Webkit介绍

广义上来说，WebKit是一个开源项目，其前身来源于KDE的KHTML和KJS，由Apple发起并主导。该项目专注于网页内容的展示，并发展壮大为一流的**网页渲染引擎**。它不是浏览器，而且也不想成为浏览器。 该项目包含两个部分，第一部分是WebCore，其中包含了对HTML，CSS等很多W3C规范的实现；第二部分就是狭义上的WebKit，它主要是各个平台的的移植并提供相对应的**Web接口**，也就是WebView或者类似WebView的组件，这些接口提供操作和显示网页的能力。

目前使用WebKit的主流的浏览器或者WebView包括Chrome, Safari, QtWebKit, Android Browser, IOS平台的WKWebView以及众多的移动平台的浏览器和WebView。

最早的Chromium 是基于WebKit的。Chrome由Chromium开发而来。

现在，Google早就已经退出WebKit而搞自己的Blink。

## Chromium介绍

Chromium是一个建立在WebKit之上的浏览器开源项目，由Google发起并主导。该项目被创建以来发展迅速，很多先进的技术被采用，如跨进程模型，沙箱模型等等。同时，很多新的规范被支持，例如WebGL，Canvas2D，CSS3以及其他很多的HTML5特性，基本上每天你都可以看到它的变化，它的版本升级很快，现在每6个月升级一个版本。在性能方面，它也备受称赞，包括快速启动，网页加载迅速等。

Chrome是Google公司的浏览器产品，它基于chromium开源项目，一般选择稳定的版本作为它的基础，它和chromium的不同点在于chromium是开源试验场（Chromium 相当于 Chrome 的工程版或称实验版），会尝试很多新的东西，当这些东西稳定之后，chrome才会集成进来，这也就是说chrome的版本会落后于chromium。另外，chrome会加入一些私有的codec，比如私有的音视频编解码器，这些仅在chrome中才会出现，而Chromium里相应的组件往往使用开源项目。再次，chrome还会整合Google的很多服务。 最后chrome还会有自动更新的功能，这也是chromium所没有的。

## Blink介绍

2013年5月，Google将WebKit从WebKit开源项目中克隆出来，称为Blink，然后将与 chromium 无关的 Ports 全部移除掉，将代码结构重新整理，就目前而言，Blink 的渲染和WebKit 是一样，但是，以后两者将各自走不同的路（WebKit当年也是以这样的方式来从KHTML中复制出来的）。这样，Google对Blink的改造将自由得多，不像对WebKit的改动时刻受Apple掣肘。从此，Blink与WebKit并驾齐驱，彼此不相关联。总体上，Blink的功能和WebKit相同，是Chromium的绘制引擎。当然，时至今日，Google对Blink在构架上做了一些改动，以更好的适应于Chromium项目。另外，Blink并不是一个完全独立的项目，而是作为Chromium项目的一部分。

所以，Blink不再是WebKit，Blink内核就是谷歌公司，针对WebKit内核，做的修订和精简。去掉了几十万行的没用的复杂代码，让效率更高。然后针对未来的网页格式，做了进一步优化，和效率提升的处理。所以Blink内核可以看成是WebKit的精简高效强化版。

## 总结

简单来讲，WebKit/Blink是一个渲染引擎（Blink是WebKit的分支），Chromium是一个浏览器。Chrome 是Chromium的稳定版。可以这样理解，Chrome 和360、QQ 和 Edge都是基于Chromium内核的浏览器。

未来的浏览器内核引擎三分天下可能是这样的：

- **Gecko**：Firefox 
- **WebKit**：Safari
- **Blink**：Google Chrome、Microsoft Edge

**Trident** 已经被微软放弃。

## 国内浏览器市场

目前国内浏览器浏览器没有自己研发的内核，一般会采用 Webkit/Blink和 Trident 双内核，如 360、UC、QQ、搜狗浏览器等。所谓国产浏览器无非是国内软件产商将 IE 内核和 Chrome 内核组合而成的混合多核浏览器。从技术上来说，就是希望用一款软件满足兼容老网页和享受新技术两方面的需要。这些浏览器都有两种模式：兼容模式（IE 的 Trident 内核）、极速模式（Chrome 的 Webkit 内核），之所以选择 Webkit 是因为 Webkit 的代码写得好，容易看懂和维护，嘿嘿。

## 常见浏览器的内核信息

| 名称                     | 公司               | 内核                           |
| ------------------------ | ------------------ | ------------------------------ |
| Firefox 火狐浏览器       | Mozilla            | Gecko                          |
| Safari 浏览器            | 苹果公司           | WebKit                         |
| Google Chrome 谷歌浏览器 | 谷歌               | Blink                          |
| Opera 浏览器             | Opera Software ASA | 早期是Presto，目前是Blink      |
| Microsoft Edge           | 微软               | 早期是EdgeHTML，目前是Chromium |
| Internet Explorer        | 微软               | Trident                        |
| 360 安全浏览器           | 奇虎 360           | Chromium + Trident             |
| QQ 浏览器                | 腾讯               | Chromium + Trident             |
| 搜狗 浏览器              | 搜狗               | Chromium + Trident             |

## 浏览器私有前缀

CSS3的浏览器私有属性前缀是一个浏览器生产商经常使用的一种方式。浏览器很多，内核也很多，各个内核也提供了一些前缀用来区别各自的特殊样式，这些前缀最开始的目的是为了试验新特性，它暗示该CSS属性或规则尚未成为W3C标准的一部分，但后来就被用来解决兼容性问题了。

浏览器私有前缀是为了兼容老版本的写法，比较新版本的浏览器无须添加。

### 常用前缀

​	(1)	-moz-：代表FireFox浏览器私有属性

​	(2)	-ms-：代表IE浏览器私有属性

​	(3)	-webkit-：代表safari、chrome浏览器私有属性

​	(4)	-o-：代表opera浏览器私有属性

### CSS3前缀+标准代码的顺序

既然CSS3代码中（暂时）需要写上这么多前缀，那么他们的顺序是如何的呢？

结论：是先写私有的CSS3属性，再写标准的CSS3属性。

```css
-webkit-transform:rotate(-3deg); /*为Chrome/Safari*/
-moz-transform:rotate(-3deg); /*为Firefox*/
-ms-transform:rotate(-3deg); /*为IE*/
-o-transform:rotate(-3deg); /*为Opera*/
transform:rotate(-3deg); /*为nothing*/
```

### 去掉CSS3前缀

什么时候我们可以去掉一个属性的CSS3前缀呢？答案是，当一个属性成为标准，并且被Firefox、Chrome等浏览器的最新版普遍兼容的时候。

以border-radius为例：

```css
-moz-border-radius: 12px; /* FF1-3.6 */
-webkit-border-radius: 12px; /* Saf3-4, iOS 1-3.2, Android <1.6 */
border-radius: 12px; /* Opera 10.5, IE9, Saf5, Chrome, FF4, iOS 4, Android 2.1+ */
```

FF4、Saf5以及Chrome都支持border-radius属性了，我们就没有必要写以上两条了，代码变成：

```css
border-radius: 12px;
```

# 移动端开发分类

- 原生app（Native App）
- 混合app（Hybrid App）
- web应用（Web App）

## 原生 App（Native App）

原生 App 是基于操作系统的开发，是传统App开发模式，该开发针对iOS 、Android等不同的手机操作系统，采用不同的语言和框架进行开发，他们只能在各自的操作系统上运行。该模式通常是由“云服务器数据+APP应用客户端”两部份构成，APP应用所有的UI元素、数据内容、逻辑框架均安装在手机终端上。

优点：

1. 可以访问操作系统，获取更多的资源（gps，摄像头，传感器，麦克风等）
2. 速度快，性能高，用户体验好
3. 可以离线使用

缺点：

1. 开发成本高
2. 需要安装和更新，更新与发布需要审核。

## 混合 App（Hybrid App）

Hybrid App是指介于web-app、native-app这两者之间的App，它虽然看上去是一个Native App，但只有一个UI WebView，里面访问的是一个Web App。Hybird App说白了就是使用了Native app的壳，里面其实还是HTML5页面。

优点：

1. 开发成本和难度更低，兼容多个平台
2. 也可以访问手机的操作系统资源。
3. 更新维护更方便

缺点：

1. 用户体验相比原生app稍差。
2. 性能依赖于网速

## web应用 （Web App）

Web应用使用HTML5+CSS3开发页面，为浏览器设计的基于web的应用，可以在各种智能设备的手机浏览器上运行，不需要安装即可运行。该开发具有跨平台的优势，该模式通常由“HTML5云网站+APP应用客户端”两部份构成，APP应用客户端只需安装应用的框架部份，而应用的数据则是每次打开APP的时候，去云端取数据呈现给手机用户。

优点：

1. 支持设备广泛
2. 开发成本低（使用）
3. 可以随时上线与更新，无需审核

缺点：

1. 用户体验极度依赖网速
2. 要求联网

## 总结

三种开发各有优缺点，具体用什么需要根据实际情况而定，比如预算，app注重功能还是内容等。

# 移动端的web

移动web就是运行在手机浏览器里面的web应用程序(网页)，虽然和网页是一样的，但是现代的web特别是移动web已经不再是简简单单的网页了，而是实现了和APP一样的功能，所以现代的网页已经可以称之为应用程序了。

从2014年HTML5正式发布后，移动web就迎来了飞速的发展，因为使用HTML5技术可以更方便、更快捷的开发现代web应用程序，而移动端的手机浏览器都是比较新的，HTML5在移动端的浏览器支持情况都比较好。

## 移动端的浏览器

移动端的浏览器和PC端有些不一样，移动端的浏览器都是在手机上安装的，现在手机阵营可以简化为两个：苹果和安卓。苹果阵营中官方的浏览器 Safari 自然是主流，而安卓系统也基本是 Webkit 的天下，所以在手机端的浏览器都有一个共同的特点：都是**webkit**内核的浏览器，除了火狐浏览器（Gecko 内核）之外，所以在浏览器的兼容性上相对PC端比较统一，很少考虑浏览器兼容性问题。

移动端的Safari、Chrome，包括国内的UC、QQ、百度、搜狗、猎豹等手机浏览器都是基于Webkit开源内核开发的。安卓系统也都有系统自带的浏览器，它们是安卓系统自带的，也是基于Webkit开源内核开发的，但手机产商通常都会对它们进行定制，增加网址导航等功能。所以兼容移动端主流浏览器，只需要处理Webkit内核浏览器的兼容性即可。在移动端我们可以放心使用 HTML5 标签和 CSS3 样式。同时我们浏览器的私有前缀我们只需要考虑添加 webkit 即可。

虽然移动端兼容性比较小，但是移动端设备屏幕尺寸非常多，碎片化严重。

## 移动端分辨率

尤其是Android，你会听到很多种分辨率：

360x640, 480x800, 480x854, 540x960, 720x1280, 1080x1920, 1080x2340, 1440x2560(2k屏), 1440x3200等

近年来iPhone也不甘示弱，碎片化也加剧了：

640x960, 640x1136, 750x1334, 828x1792, 1080x1920, 1080x2340, 1125x2436, 1170x2532,  1284x2778等

作为移动端开发者需要时刻关心这些分辨率，但是作为web前端开发者无需刻意关注这些分辨率，因为我们常用的尺寸单位是 px ，只需要搞懂web前端开发中的 px 在移动端开发中如何应用即可。

常见移动端屏幕尺寸，以iPhone为例：

| 设备                  | 屏幕尺寸 | 像素尺寸 | 开发尺寸 | 物理像素比 dpr |
| --------------------- | ---------- | ----------- | ----------- | -------------- |
| iPhone3g/3gs          | 3.5英寸      | 320x480     | 320*480     | 1.0            |
| iPhone4/4s            | 3.5英寸      | 640x960     | 320*480     | 2.0            |
| iPhone5/5s/5c         | 4.0英寸      | 640x1136    | 320*568     | 2.0            |
| iPhone6/7/8           | 4.7英寸      | 750x1334    | 375*667     | 2.0            |
| iPhone6/7/8 Plus      | 5.5英寸      | 1080x1920   | 414*736     | 3.0            |
| iPhone12/13 Mini      | 5.4英寸      | 1080x2340   | 360*780     | 3.0            |
| iPhone12/13/12/13 Pro | 6.1英寸      | 1170x2532   | 390*844     | 3.0            |
| iPhone12/13 Pro Max   | 6.7英寸      | 1284x2778  | 428*926     | 3.0            |

# 移动web开发设置

## H5文档类型

```html
<!DOCTYPE html>  
```

## 视口 viewport 

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, user-scalable=no">
```

- viewport：视窗，窗口。viewport的宽度决定了html这个标签的宽度
- width：宽度，可设为px或 device-width。比如：width=device-width，表示我们将viewport的宽度设为跟设备一样宽，以iPhone6为例，iPhone6的横向分辨率为750，转换之后所代表的css宽度为375px，这个时候viewport的宽度就为375px，也即是html的宽度为375px。一般设计稿也是以375或者750来出图，这样就可以很方便的在设备上实现页面了。
- height：高度，可设为px或 device-height，通常不用设置
- initial-scale：初始的缩放比例（默认设置为1.0）
- minimum-scale：允许用户缩放到的最小比例（默认设置为1.0）
- maximum-scale：允许用户缩放到的最大比例（默认设置为1.0）  
- user-scalable：是否允许用户通过双指缩放（默认设置为 no 或者 0，因为我们不希望用户放大缩小页面)

## CSS设置

### CSS初始化 normalize.css  

Normalize.css 是一个可以定制的CSS文件，它让不同的浏览器在渲染网页元素的时候形式更统一。

官网：http://necolas.github.io/normalize.css/

Normalize.css只是一个很小的css文件，但它在磨人的HTML元素样式上提供了跨浏览器的高度一致性。

相比于传统的CSS reset，Normalize.css是一种现代的、为HTML5准备的优质替代方案。总之，Normalize.css是一种CSS reset的替代方案。

移动端 CSS 初始化推荐使用 normalize.css

- 保留有用的默认值，不同于许多 CSS 的重置
- 标准化的样式，适用范围广的元素。
- 纠正错误和常见的浏览器的不一致性。
- 一些细微的改进，提高了易用性。
- 使用详细的注释来解释代码。 

把Normalize.css里面的所有内容放在自己的style.css的最上面，那样如果有冲突的话，写在后面的CSS设置默认是会覆盖掉写在前面的。

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    outline: 0;
}
```

### 字体设置

使用无衬线字体

```css
body {
    font-family: "Helvetica Neue", Helvetica, STHeiTi, sans-serif;
}
```

京东字体设置：

```css
京东
body {
    font-family: -apple-system,Helvetica,sans-serif;
}
```

知乎的字体设置：

```css
body {
    font-family: -apple-system,BlinkMacSystemFont,Helvetica Neue,PingFang SC,Microsoft YaHei,Source Han Sans SC,Noto Sans CJK SC,WenQuanYi Micro Hei,sans-serif;
}
```

-apple-system 是在以 WebKit 为内核的浏览器（如 Safari）中，调用 Apple（苹果公司）系统（iOS, macOS, watchOS, tvOS）中默认字体。

现在一般情况下，英文是 San Francisco，中文是苹方BlinkMacSystemFont 是在 Chrome 中实现调用 Apple 的系统字体。

### 特殊CSS样式

```css
html, body {
    -webkit-user-select: none; /*禁止选中文本*/
    user-select: none;
 		-webkit-font-smoothing: antialiased; /*对字体进行抗锯齿渲染可以使字体看起来会更清晰舒服*/
  	-webkit-text-size-adjust: 100%!important; /*禁止IOS调整字体大小*/
}

a, img {
    -webkit-touch-callout: none; /*禁止长按链接与图片弹出菜单*/
}

button,input,optgroup,select,textarea {
   /*在移动端浏览器默认的外观在iOS上加上这个属性才能给按钮和输入框自定义样式*/
    -webkit-appearance:none; /*去掉webkit默认的表单样式*/
}

a,button,input,optgroup,select,textarea {
  	/* 去掉标签点击时的高亮状态 */
    -webkit-tap-highlight-color:rgba(0,0,0,0); /*去掉a、input和button点击时的蓝色外边框和灰色半透明背景*/
}

input::-webkit-input-placeholder {
    color:#ccc; /*修改webkit中input的planceholder样式*/
}

input:focus::-webkit-input-placeholder {
    color:#f00; /*修改webkit中focus状态下input的planceholder样式*/
}

input::-webkit-input-speech-button {
    display: none; /*隐藏Android的语音输入按钮*/
}
```

## 其他设置

```css
指定 Web App 图标：
<link rel="apple-touch-icon" sizes="57x57" href="icon57.png" />  
<link rel="apple-touch-icon" sizes="72x72" href="icon72.png" />  
<link rel="apple-touch-icon" sizes="114x114" href="icon114.png" />    
<link rel="apple-touch-icon" sizes="144x144" href="icon144.png" /> 
<link rel="apple-touch-icon" sizes="196x196" href="icon196.png" />

指定 Web App 启动图片：
<link href="startup-image-320x460.png" media="(device-width: 320px)" rel="apple-touch-startup-image">
<link href="startup-image-640x960.png" media="(device-width: 320px) and (-webkit-device-pixel-ratio: 2)" rel="apple-touch-startup-image">
```

