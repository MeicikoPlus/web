# HTML发展

1990 年，由于对 [Web](https://developer.mozilla.org/zh-CN/docs/Glossary/World_Wide_Web) 未来发展的远见，Tim Berners-Lee 提出了 [超文本](https://developer.mozilla.org/zh-CN/docs/Glossary/Hypertext) 的概念，并在第二年在 [SGML (en-US)](https://developer.mozilla.org/en-US/docs/Glossary/SGML) 的基础上将其正式定义为一个标记语言。

[IETF (en-US)](https://developer.mozilla.org/en-US/docs/Glossary/IETF) 于 1993 年正式开始制定 HTML 规范（不存在的**HTML1.0**），并在经历了几个版本的草案后于 1995 年11月发布了 **HTML 2.0** 版本。

1994年，Berners-Lee 为了 Web 发展而成立了 [W3C (en-US)](https://developer.mozilla.org/en-US/docs/Glossary/W3C)，1996 年W3C 接管了 HTML 的标准化工作。并在1年后发布了 **HTML 3.2** 推荐标准。（W3C推荐标准）

1997年12月 **HTML 4.0** 发布，并在 2000 年成为 [ISO (en-US)](https://developer.mozilla.org/en-US/docs/Glossary/ISO) 标准。（W3C推荐标准）

1999年12月 **HTML 4.01**（微小改进）发布。（W3C推荐标准）

自那以后，W3C 几乎放弃了 HTML 而转向 [XHTML](https://developer.mozilla.org/zh-CN/docs/Glossary/XHTML)，并于 2004 年成立了另一个独立的小组 [WHATWG](https://developer.mozilla.org/zh-CN/docs/Glossary/WHATWG)。幸运的是，WHATWG 后来又转回来参与了 [HTML5](https://developer.mozilla.org/zh-CN/docs/Glossary/HTML5) 标准的制定，两个组织（译注：即 W3C 和 WHATWG）在 2008 年发布了第一份草案，并在 2014 年发布了 **HTML5.0** 标准的最终版。（W3C推荐标准）

备注：WHATWG（Web Hypertext Application Technology Working GroupWeb 超文本应用程序技术工作组）是一个负责[维护与开发 Web 标准](https://spec.whatwg.org/)的社区，他们的工作成果包括 [DOM](https://developer.mozilla.org/zh-CN/docs/Glossary/DOM)、Fetch API，和 [HTML](https://developer.mozilla.org/zh-CN/docs/Glossary/HTML)。一些来自 Apple、Mozilla，和 Opera 的员工在 2004 年建立了 WHATWG。

# HTML5

## 狭义HTML5

HTML5是HTML最新的修订版本（第5版），2014年10月由万维网联盟（W3C）完成标准制定，是下一代的HTML的总称。HTML5的设计目的是为了在**移动设备**上支持多媒体。HTML5 是下一代 HTML 标准，增加了很多新特性。HTML5 的新增特性主要是针对于以前的不足，增加了一些新的标签、新的表单、表单属性、音视频的支持等。

## 广义HMTL5

广义上讲HTML5实际指的是包括HTML、CSS和JavaScript在内的一套技术组合，伴随HTML5一同成为标准的Web前端开发技术的统称。

# HTML5 浏览器支持

HTML5的新特性都有兼容性问题，基本是 IE9+ 以上版本的浏览器才支持，如果不考虑兼容性问题，可以大量使用这些新特性。最新版本的 Safari、Chrome、Firefox 以及 Opera 支持某些 HTML5 特性。Internet Explorer 9 将支持某些 HTML5 特性。

![](https://gitee.com/Jinxizhen/pic_resource/raw/master/images/browsers_say.png)

所有现代浏览器都支持 HTML5。此外，所有浏览器，不论新旧，都会自动把未识别元素当做行内元素来处理。

正因如此，您可以把 HTML5 元素定义为块级元素帮助老式浏览器处理”未知的“ HTML 元素。

HTML5 定义了八个新的**语义** HTML 元素。所有都是**块级**元素。您可以把 CSS **display** 属性设置为 **block**，以确保老式浏览器中正确的行为：

IE9 以下版本浏览器兼容HTML5的方法，使用html5shiv包：

```html
<!--[if lt IE 9]>    
<script src="http://cdn.static.runoob.com/libs/html5shiv/3.7/html5shiv.min.js"></script> 
<![endif]-->
```

载入后，初始化新标签的CSS：

```css
header, section, footer, aside, nav, main, article, figure {
    display: block; 
}
```

# HTML5的新特性

HTML5添加了很多新元素及功能，比如: 图形的绘制，多媒体内容，更好的页面结构，更好的形式处理，和几个api拖放元素，定位，包括网页应用程序缓存，存储，网络工作者等。

## 语义标签

HTML5语义标签，可以使开发者更方便清晰构建页面的布局，这种语义化标准主要是针对搜索引擎的，对移动端也更加友好。

更多新标签：https://www.w3school.com.cn/html/html5_new_elements.asp

| 标签        | 描述                                     |
| :---------- | :--------------------------------------- |
| `<article>` | 定义文档内的文章。                       |
| `<aside>`   | 定义页面内容之外的内容。页面的侧边栏内容 |
| `<footer>`  | 定义文档或节的页脚。                     |
| `<header>`  | 定义文档或节的页眉。                     |
| `<main>`    | 定义文档的主内容。                       |
| `<nav>`     | 定义文档内的导航链接。                   |
| `<section>` | 定义文档中的节。                         |
| `<time>`    | 定义日期/时间。                          |

![img](https://gitee.com/Jinxizhen/pic_resource/raw/master/images/1239961-20190527090526985-576135815.png)

## 增强型表单

### 新增input类型

HTML5 拥有多个新的表单输入类型（type属性值）。这些新特性提供了更好的输入控制和验证。

详细内容：https://www.runoob.com/html/html5-form-input-types.html

| 值                 | 描述                                                         |
| :----------------- | :----------------------------------------------------------- |
| ==color==          | 定义拾色器。                                                 |
| ==date==           | 定义 date 控件（包括年、月、日，不包括时间）。               |
| ==datetime==       | 定义 date 和 time 控件（包括年、月、日、时、分、秒、几分之一秒，基于 UTC 时区）。 |
| ==datetime-local== | 定义 date 和 time 控件（包括年、月、日、时、分、秒、几分之一秒，不带时区）。 |
| ==email==          | 定义用于 e-mail 地址的字段。                                 |
| ==month==          | 定义 month 和 year 控件（不带时区）。                        |
| **==number==**     | 定义用于输入数字的字段。                                     |
| ==range==          | 定义用于精确值不重要的输入数字的控件（比如 slider 控件）。   |
| ==search==         | 定义用于输入搜索字符串的文本字段。                           |
| ==tel==            | 定义用于输入电话号码的字段。                                 |
| ==time==           | 定义用于输入时间的控件（不带时区）。                         |
| ==url==            | 定义用于输入 URL 的字段。                                    |
| ==week==           | 定义 week 和 year 控件（不带时区）。                         |

### 新的表单属性

详细内容：https://www.runoob.com/html/html5-form-attributes.html

| 属性                | 值             | 描述                                                         |
| :------------------ | :------------- | :----------------------------------------------------------- |
| **==placeholder==** | text           | 规定帮助用户填写输入字段的提示。                             |
| ==autocomplete==    | on/off         | 规定是否使用输入字段的自动完成功能。                         |
| ==autofocus==       | autofocus      | 规定输入字段在页面加载时是否获得焦点。（不适用于 type="hidden"） |
| ==required==        | required       | 指示输入字段的值是必需的。                                   |
| **==multiple==**    | multiple       | 如果使用该属性，则允许一个以上的值。                         |
| ==width==           | pixels/%       | 定义 input 字段的宽度。（适用于 type="image"）               |
| ==height==          | pixels/%       | 定义 input 字段的高度。（适用于 type="image"）               |
| ==pattern==         | regexp_pattern | 规定输入字段的值的模式或格式。例如 pattern="[0-9]" 表示输入值必须是 0 与 9 之间的数字。 |
| ==max==             | number/date    | 规定输入字段的最大值。请与 "min" 属性配合使用，来创建合法值的范围。 |
| ==min==             | number/date    | 规定输入字段的最小值。请与 "max" 属性配合使用，来创建合法值的范围。 |
| ==step==            | number         | 规定输入字的的合法数字间隔。                                 |

## 音频和视频

音频标签`<audio>` 和 视频标签 `<video>` 使用它们可以很方便的在页面中嵌入音频和视频，而不再去使用 flash 和其他浏览器插件。

### 音频

`<audio>` 标签定义声音，比如音乐或其他音频流。

目前，`<audio>` 元素支持的3种文件格式：MP3、Wav、Ogg。

| 浏览器            | MP3  | Wav  | Ogg  |
| :---------------- | :--- | :--- | :--- |
| Internet Explorer | YES  | NO   | NO   |
| Chrome            | YES  | YES  | YES  |
| Firefox           | YES  | YES  | YES  |
| Safari            | YES  | YES  | NO   |
| Opera             | YES  | YES  | YES  |

#### 用法

```html
<audio 
 src="http://jinxizhen.gitee.io/your_music/src/qiaobianguniang.mp3"
 controls
 loop
></audio>
```

说明：音频不可以自动播放，如果想要实现自动播放可以通过JavaScript解决。 

低版本浏览器兼容性写法，如果不兼容显示“您的浏览器不支持 audio 元素。”

```html
<audio controls>
  <source src="horse.ogg" type="audio/ogg">
  <source src="horse.mp3" type="audio/mpeg">
  您的浏览器不支持 audio 元素。
</audio>
```

#### 属性

| 属性                                                         | 值                           | 描述                                                         |
| :----------------------------------------------------------- | :--------------------------- | :----------------------------------------------------------- |
| [autoplay](https://www.runoob.com/tags/att-audio-autoplay.html) | autoplay                     | 如果出现该属性，则音频在就绪后马上播放。                     |
| [controls](https://www.runoob.com/tags/att-audio-controls.html) | controls                     | 如果出现该属性，则向用户显示音频控件（比如播放/暂停按钮）。  |
| [loop](https://www.runoob.com/tags/att-audio-loop.html)      | loop                         | 如果出现该属性，则每当音频结束时重新开始播放。               |
| [muted](https://www.runoob.com/tags/att-audio-muted.html)    | muted                        | 如果出现该属性，则音频输出为静音。                           |
| [preload](https://www.runoob.com/tags/att-audio-preload.html) | auto<br />metadata<br />none | 规定当网页加载时，音频是否默认被加载以及如何被加载。<br />auto - 当页面加载后载入整个音频<br />metadata - 当页面加载后只载入元数据<br />none - 当页面加载后不载入音频 |
| [src](https://www.runoob.com/tags/att-audio-src.html)        | URL                        | 规定音频文件的 URL。                                         |

### 视频

`<video>` 标签定义视频，比如电影片段或其他视频流。

目前，`<video> `元素支持三种视频格式：MP4、WebM、Ogg。

| 浏览器            | MP4  | WebM | Ogg  |
| :---------------- | :--- | :--- | :--- |
| Internet Explorer | YES  | NO   | NO   |
| Chrome            | YES  | YES  | YES  |
| Firefox           | YES  | YES  | YES  |
| Safari            | YES  | NO   | NO   |
| Opera             | YES  | YES  | YES  |

#### 用法

```html
<video 
  src="https://videos.nfsq.com.cn/asset/336b633adf441b9467ddcc24831ca4ff/c04edd6066afab2b068627ef6fda4a63.mp4"
  controls
  loop
  autoplay
  muted
></video>
```

说明：给视频标签添加 muted 属性来静音自动播放视频

低版本浏览器兼容性写法，如果不兼容显示“您的浏览器不支持 video 标签。

```html
<video width="320" height="240" controls>
    <source src="movie.mp4" type="video/mp4">
    <source src="movie.ogg" type="video/ogg">
    您的浏览器不支持 video 标签。
</video>
```

#### 属性

| 属性                                                         | 值                 | 描述                                                         |
| :----------------------------------------------------------- | :----------------- | :----------------------------------------------------------- |
| [autoplay](https://www.runoob.com/tags/att-video-autoplay.html) | autoplay           | 如果出现该属性，则视频在就绪后马上播放。                     |
| [controls](https://www.runoob.com/tags/att-video-controls.html) | controls           | 如果出现该属性，则向用户显示控件，比如播放按钮。             |
| [height](https://www.runoob.com/tags/att-video-height.html) | pixels           | 设置视频播放器的高度。                                       |
| [loop](https://www.runoob.com/tags/att-video-loop.html) | loop               | 如果出现该属性，则当媒介文件完成播放后再次开始播放。         |
| [muted](https://www.runoob.com/tags/att-video-muted.html) | muted              | 如果出现该属性，视频的音频输出为静音。                       |
| [poster](https://www.runoob.com/tags/att-video-poster.html) | URL              | 规定视频正在下载时显示的图像，直到用户点击播放按钮。         |
| [preload](https://www.runoob.com/tags/att-video-preload.html) | auto metadata none | 如果出现该属性，则视频在页面加载时进行加载，并预备播放。如果使用 "autoplay"，则忽略该属性。 |
| [src](https://www.runoob.com/tags/att-video-src.html) | URL              | 要播放的视频的 URL。                                         |
| [width](https://www.runoob.com/tags/att-video-width.html) | pixels           | 设置视频播放器的宽度。                                       |

## Canvas

------

`<canvas>` 标签定义图形，比如图表和其他图像，您必须使用脚本来绘制图形。

HTML5 `<canvas>` 元素用于图形的绘制，通过脚本 (通常是JavaScript)来完成。`<canvas>` 标签只是图形容器，您必须使用脚本来绘制图形。你可以通过多种方法使用 canvas 绘制路径,盒、圆、字符以及添加图像。

```html
<canvas id="myCanvas" width="200" height="100" style="border:1px solid #000000;">
</canvas>
```

使用 JavaScript 来绘制图像，canvas 元素本身是没有绘图能力的。所有的绘制工作必须在 JavaScript 内部完成：

```js
var c=document.getElementById("myCanvas");
var ctx=c.getContext("2d");
ctx.fillStyle="#FF0000";
ctx.fillRect(0,0,150,75);
```

详细用法：https://www.runoob.com/w3cnote/html5-canvas-intro.html

canvas手册：https://www.runoob.com/tags/ref-canvas.html

## SVG绘图

### 什么是SVG？

- SVG 指可伸缩矢量图形 (Scalable Vector Graphics)
- SVG 用于定义用于网络的基于矢量的图形
- SVG 使用 XML 格式定义图形
- SVG 图像在放大或改变尺寸的情况下其图形质量不会有损失
- SVG 是万维网联盟的标准

### SVG优势

与其他图像格式相比（比如 JPEG 和 GIF），使用 SVG 的优势在于：

- SVG 图像可通过文本编辑器来创建和修改
- SVG 图像可被搜索、索引、脚本化或压缩
- SVG 是可伸缩的
- SVG 图像可在任何的分辨率下被高质量地打印
- SVG 可在图像质量不下降的情况下被放大

### SVG 与 Canvas两者间的区别

SVG 是一种使用 XML 描述 2D 图形的语言。

Canvas 通过 JavaScript 来绘制 2D 图形。

SVG 基于 XML，这意味着 SVG DOM 中的每个元素都是可用的。您可以为某个元素附加 JavaScript 事件处理器。

在 SVG 中，每个被绘制的图形均被视为对象。如果 SVG 对象的属性发生变化，那么浏览器能够自动重现图形。

Canvas 是逐像素进行渲染的。在 canvas 中，一旦图形被绘制完成，它就不会继续得到浏览器的关注。如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象。

### Canvas 与 SVG 的比较

下表列出了 canvas 与 SVG 之间的一些不同之处。

| Canvas                                             | SVG                                                     |
| :------------------------------------------------- | :------------------------------------------------------ |
| 依赖分辨率                                         | 不依赖分辨率                                            |
| 不支持事件处理器                                   | 支持事件处理器                                          |
| 弱的文本渲染能力                                   | 最适合带有大型渲染区域的应用程序（比如谷歌地图）        |
| 能够以 .png 或 .jpg 格式保存结果图像               | 复杂度高会减慢渲染速度（任何过度使用 DOM 的应用都不快） |
| 最适合图像密集型的游戏，其中的许多对象会被频繁重绘 | 不适合游戏应用                                          |

SVG教程：https://www.runoob.com/svg/svg-tutorial.html

## 地理定位

HTML5 Geolocation（地理定位）用于定位用户的位置。鉴于该特性可能侵犯用户的隐私，除非用户同意，否则用户位置信息是不可用的。

详细解析：https://www.runoob.com/html/html5-geolocation.html

```JS
var x=document.getElementById("demo");
function getLocation()
{
  // 检测是否支持地理定位
  if (navigator.geolocation){
    // 如果支持，则运行 getCurrentPosition() 方法。
    // 如果获取位置成功，则向参数showPosition中规定的函数返回一个 coordinates 对象
    // 第二个参数用于处理错误。它规定当获取用户位置失败时运行的函数
    navigator.geolocation.getCurrentPosition(showPosition, showError);
  } else {
    // 如果不支持，则向用户显示一段消息。
    x.innerHTML="该浏览器不支持获取地理位置。";
  }
}
// showPosition() 函数获得并显示经度和纬度
function showPosition(position)
{
  x.innerHTML="纬度: " + position.coords.latitude + "<br>经度: " + position.coords.longitude;    
}

// 错误处理函数
function showError(error) {
  switch (error.code) {
    case error.PERMISSION_DENIED:
      x.innerHTML = "用户拒绝对获取地理位置的请求。"
      break;
    case error.POSITION_UNAVAILABLE:
      x.innerHTML = "位置信息是不可用的。"
      break;
    case error.TIMEOUT:
      x.innerHTML = "请求用户地理位置超时。"
      break;
    case error.UNKNOWN_ERROR:
      x.innerHTML = "未知错误。"
      break;
  }
}
```

## 拖放API

拖放是一种常见的特性，即抓取对象以后拖到另一个位置。在 HTML5 中，拖放是标准的一部分，任何元素都能够拖放。

### 用法

当元素拖动时，我们可以检查其拖动的数据。

详细解析：https://www.runoob.com/html/html5-draganddrop.html

```html
<div draggable="true" ondragstart="drag(event)"></div>
<script>
function drap(ev){
    console.log(ev);
}
</script>
```

### 拖动事件

| 拖动生命周期 | 属性名      | 描述                                           |
| ------------ | ----------- | ---------------------------------------------- |
| 拖动开始     | ondragstart | 在拖动操作开始时执行脚本                       |
| 拖动过程中   | ondrag      | 只要脚本在被拖动就运行脚本                     |
| 拖动过程中   | ondragenter | 当元素被拖动到一个合法的防止目标时，执行脚本   |
| 拖动过程中   | ondragover  | 只要元素正在合法的防止目标上拖动时，就执行脚本 |
| 拖动过程中   | ondragleave | 当元素离开合法的防止目标时                     |
| 拖动结束     | ondrop      | 将被拖动元素放在目标元素内时运行脚本           |
| 拖动结束     | ondragend   | 在拖动操作结束时运行脚本                       |

## WebWorker

### 什么是 Web Worker？

当在 HTML 页面中执行脚本时，页面的状态是不可响应的，直到脚本已完成。web worker 是运行在后台的 JavaScript，独立于其他脚本，不会影响页面的性能。您可以继续做任何愿意做的事情：点击、选取内容等等，而此时 web worker 在后台运行。

Web Worker可以通过加载一个脚本文件，进而创建一个独立工作的线程，在主线程之外运行。

### 基本使用

Web Worker的基本原理就是在当前javascript的主线程中，使用Worker类加载一个javascript文件来开辟一个新的线程，起到互不阻塞执行的效果，并且提供主线程和新县城之间数据交换的接口：postMessage、onmessage。

详细解析：https://www.runoob.com/html/html5-webworkers.html

```js
//worker.js
onmessage = function (evt) {
  //通过evt.data获得发送来的数据
  var d = evt.data;
  //将获取到的数据发送会主线程
  postMessage(d);
}
```

```html
<p></p>
<script type="text/javascript">
  //WEB页主线程
  //创建一个Worker对象并向它传递将在新线程中执行的脚本的URL
  var worker = new Worker("worker.js");
  //向worker发送数据
  worker.postMessage("hello world");     
  //接收worker传过来的数据函数
  worker.onmessage = function (evt) {    
    //输出worker发送来的数据
    console.log(evt.data);   
    document.querySelector('p').innerHTML = evt.data;   
  }
</script>
```

## Web Storage 存储

HTML5 web 存储,一个比cookie更好的本地存储方式。

### 什么是 HTML5 Web 存储

使用HTML5可以在本地存储用户的浏览数据。早些时候，本地存储使用的是 cookie。但是Web 存储需要更加的安全与快速。这些数据不会被保存在服务器上，但是这些数据只用于用户请求网站数据上。它也可以存储大量的数据，而不影响网站的性能。数据以 键/值 对存在, web网页的数据只允许该网页访问使用。

WebStorage是HTML新增的本地存储解决方案之一，但并不是取代cookie而指定的标准，cookie作为HTTP协议的一部分用来处理客户端和服务器的通信是不可或缺的，session正式依赖与实现的客户端状态保持。WebSorage的意图在于解决本来不应该cookie做，却不得不用cookie的本地存储。websorage拥有5M的存储容量，而cookie却只有4K，这是完全不能比的。

### 用法

客户端存储数据有两个对象，其用法基本是一致。

localStorage：持久化存储，用于长久保存整个网站的数据，保存的数据没有过期时间，直到手动去除。

sessionStorage：会话存储，用于临时保存同一窗口(或标签页)的数据，在关闭窗口或标签页之后将会删除这些数据。

详细解析：https://www.runoob.com/html/html5-webstorage.html

```js
localStorage.setItem(key,value);//保存数据

let value = localStorage.getItem(key);//读取数据

localStorage.removeItem(key);//删除单个数据

localStorage.clear();//删除所有数据

let key = localStorage.key(index);//得到某个索引的值
```

## WebSocket

  WebSocket协议为web应用程序客户端和服务端之间提供了一种全双工通信机制。

特点：

 （1）握手阶段采用HTTP协议，默认端口是80和443

 （2）建立在TCP协议基础之上，和http协议同属于应用层

 （3）可以发送文本，也可以发送二进制数据。

 （4）没有同源限制，客户端可以与任意服务器通信。

 （5）协议标识符是ws（如果加密，为wss），如ws://localhost:8023

详细解析：https://www.runoob.com/html/html5-websocket.html
