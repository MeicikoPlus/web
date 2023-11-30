# 概述 

HTML 是网页使用的语言，定义了网页的结构和内容。浏览器访问网站，其实就是从服务器下载 HTML 代码，然后渲染出网页。

HTML 的全名是“超文本标记语言”（HyperText Markup Language），上个世纪90年代由欧洲核子研究中心的物理学家蒂姆·伯纳斯-李（Tim Berners-Lee）发明。它的最大特点就是支持超链接，点击链接就可以跳转到其他网页，从而构成了整个互联网。

> 什么是 HTML？HTML 是用来描述网页的一种语言。
>
> - HTML 指的是超文本标记语言 (**H**yper **T**ext **M**arkup **L**anguage)
> - HTML 不是一种编程语言，而是一种**标记语言** (markup language)
> - 标记语言是一套**标记标签** (markup tag)
> - HTML 使用**标记标签**来描述网页

1999年，HTML 4.01 版发布，成为广泛接受的 HTML 标准。2014年，HTML 5 发布，这是目前正在使用的版本。

浏览器的网页开发，涉及三种技术：HTML、CSS 和 JavaScript。HTML 语言定义网页的结构和内容，CSS 样式表定义网页的样式，JavaScript 语言定义网页与用户的互动行为。HTML 语言是网页开发的基础，CSS 和 JavaScript 都是基于 HTML 才能生效，即使没有这两者，HTML 本身也能使用，可以完成基本的内容展示。本教程只介绍 HTML 语言。

下面就是一个简单网页的 HTML 源码。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8">
  <title>网页标题</title>
</head>
<body>
  <p>Hello World</p>
</body>
</html>
```

上面这段代码，可以保存成文件`index.html`。浏览器打开这个本地文件，就能看到文字“Hello World”。

浏览器右键菜单的“查看网页源码”（View page source），可以展示当前网页的 HTML 源码。

# 网页的基本概念

## HTML 文档 = 网页

- HTML 文档**描述网页**
- HTML 文档**包含 HTML 标签**和纯文本
- HTML 文档也被称为**网页**

Web 浏览器的作用是读取 HTML 文档，并以网页的形式显示出它们。浏览器不会显示 HTML 标签，而是使用标签来解释页面的内容

## 标签

HTML 标记标签通常被称为 HTML 标签 (HTML tag)。

- HTML 标签是由**尖括号**包围的关键词，比如 `<html>`
- HTML 标签通常是**成对出现**的，比如 `<b> 和 </b>`
- 标签对中的第一个标签是**开始标签**，第二个标签是**结束标签**
- 开始和结束标签也被称为**开放标签**和**闭合标签**

网页的 HTML 代码由许许多多不同的标签（tag）构成。学习 HTML 语言，就是学习各种标签的用法。

下面就是标签的一个例子。

```html
<title>网页标题</title>
```

上面代码中，`<title>`和`</title>`就是一对标签。

标签用来告诉浏览器，如何处理这段代码。标签的内容就是浏览器所要渲染的、展示在网页上的内容。

标签放在一对尖括号里面（比如`<title>`），大多数标签都是成对出现的，分成开始标签和结束标签，结束标签在标签名之前加斜杠（比如`</title>`）。但是，也有一些标签不是成对使用，而是只有开始标签，没有结束标签，比如上一节示例的`<meta>`标签。

```html
<meta charset="utf-8">
```

上面代码中，`<meta>`标签就没有结束标签`</meta>`。

这种单独使用的标签，通常是因为标签本身就足够完成功能了，不需要标签之间的内容。实际应用中，它们主要用来提示浏览器，做一些特别处理。

标签可以嵌套。

```html
<div><p>hello world</p></div>
```

上面代码中，`<div>`标签内部包含了一个`<p>`标签。

嵌套时，必须保证正确的闭合顺序，不能跨层嵌套，否则会出现意想不到的渲染结果。

```html
<div><p>hello world</div></p>
```

上面代码就是错误的嵌套，闭合顺序不正确。

HTML 标签名是大小写不敏感，比如`<title>`和`<TITLE>`是同一个标签。不过，一般习惯都是使用小写。

另外，HTML 语言忽略缩进和换行。下面几种写法的渲染结果是一样的。

```html
<title>网页标题</title>

<title>
  网页标题
</title>

<title>网页
标题</title>
```

进一步说，整个网页的 HTML 代码完全可以写成一行，浏览器照样解析，结果完全一样。所以，正式发布网页之前，开发者有时会把源码压缩成一行，以减少传输的字节数。

各种网页的样式效果，比如内容的缩进和换行，主要靠 CSS 来实现。

## 元素

浏览器渲染网页时，会把 HTML 源码解析成一个标签树，每个标签都是树的一个节点（node）。这种节点就称为网页元素（element）。所以，“标签”和“元素”基本上是同义词，只是使用的场合不一样：标签是从源码角度来看，元素是从编程角度来看，比如`<p>`标签对应网页的`p`元素。

嵌套的标签就构成了网页元素的层级关系。

```html
<div><p>hello world</p></div>
```

上面代码中，`div`元素内部包含了一个`p`元素。上层元素又称为“父元素”，下层元素又称为“子元素”，即`div`是`p`的父元素，`p`是`div`的子元素。

## 块级元素，行内元素

所有元素可以分成两大类：块级元素（block）和行内元素（inline）。

块级元素默认占据一个独立的区域，在网页上会自动另起一行，占据 100% 的宽度。

```html
<p>hello</p>
<p>world</p>
```

上面代码中，`p`元素是块级元素，因此浏览器会将内容分成两行显示。

行内元素默认与其他元素在同一行，不产生换行。比如，`span`就是行内元素，通常用来为某些文字指定特别的样式。

```html
<span>hello</span>
<span>world</span>
```

上面代码中，`span`元素是行内元素，因此浏览器会将两行内容放在一行显示。

## 属性

属性（attribute）是标签的额外信息，使用空格与标签名和其他属性分隔。

```html
<img src="demo.jpg" width="500">
```

上面代码中，`<img>`标签有两个属性：`src`和`width`。

属性可以用等号指定属性值，比如上例的`demo.jpg`就是`src`的属性值。属性值一般放在双引号里面，这不是必需的，但推荐总是使用双引号。

注意，属性名是大小写不敏感的，`onclick`和`onClick`是同一个属性。

HTML 提供大量属性，用来定制标签的行为，后面会详细介绍。

# 网页的基本标签

符合 HTML 语法标准的网页，应该满足下面的**HTML基本结构**。

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8">
  <title></title>
</head>
<body>
</body>
</html>
```

不管多么复杂的网页，都是从上面这个基本结构衍生出来的。

前面说过，HTML 代码的缩进和换行，对于浏览器不产生作用。所以，上面的代码完全可以写成一行，渲染结果不变。上面这样分行写，只是为了提高可读性。

下面就依次介绍，这个基本结构的主要标签。它们构成了网页的骨架。

## 文档类型 DOCTYPE  

### 什么是DOCTYPE  

网页的第一个标签通常是`<!doctype>`，表示文档类型，告诉浏览器如何解析网页。

Web 世界中存在许多不同的文档。只有了解文档的类型，浏览器才能正确地显示文档。

HTML 也有多个不同的版本，只有完全明白页面中使用的确切 HTML 版本，浏览器才能完全正确地显示出 HTML 页面。这就是 <!DOCTYPE> 的用处。

DOCTYPE是document type的简写，它并不是 HTML 标签，也没有结束标签，它是一种标记语言的文档类型声明，即告诉浏览器当前 HTML 是用什么版本编写的。

一般来说，只要像下面这样，简单声明`doctype`为`html`即可。浏览器就会按照 HTML 5 的规则处理网页。

```html
<!doctype html>
```

有时，该标签采用完全大写的形式，以便区别于正常的 HTML 标签。因为`<!doctype>`本质上不是标签，更像一个处理指令。

```
<!DOCTYPE html>
```

注意: DOCTYPE的声明必须是 HTML 文档的第一行，位于html标签之前。大多数Web文档的顶部都有doctype声明，它是在新建一个文档时，由Web创作软件草率处理的众多细节之一。很少人会去注意 doctype ，但在遵循标准的任何Web文档中，它都是一项必需的元素。doctype会影响代码验证，并决定了浏览器最终如何显示你的 Web文档。

### **DOCTYPE**的作用

声明文档的解析类型(document.compatMode)，避免浏览器的怪异模式。

document.compatMode：

- BackCompat：怪异模式，浏览器使用自己的怪异模式解析渲染页面。

- CSS1Compat：标准模式，浏览器使用W3C的标准解析渲染页面。

这个属性会被浏览器识别并使用，但是如果你的页面没有DOCTYPE的声明，那么compatMode默认就是BackCompat，浏览器按照自己的方式解析渲染页面，那么，在不同的浏览器就会显示不同的样式。

如果你的页面添加了`<!DOCTYPE html>`那么，那么就等同于开启了标准模式，那么**浏览器就得老老实实的按照W3C的标准解析渲染页面**，这样一来，你的页面在所有的浏览器里显示的就都是一个样子了。

### 详细文档类型

`<!DOCTYPE html>`的意思是当前页面采取的是 HTML5 版本来显示网页
`<!DOCTYPE html>`文档类型声明标签，告诉浏览器这个页面采取HTML5版本标准来显示页面

详细文档类型：https://www.runoob.com/tags/tag-doctype.html

## 根标签 `<html></html>`

`<html>`标签是网页的顶层容器，即标签树结构的顶层节点，也称为根元素（root element），其他元素都是它的子元素。一个网页只能有一个`<html>`标签。

- `<html>`是页面的根标签，此元素可告知浏览器其自身是一个 HTML 文档
- `<html> 与 </html>` 之间的文本描述网页
- `<html> 与 </html>` 标签限定了文档的开始点和结束点，在它们之间是文档的头部和主体。文档的头部由 `<head>` 标签定义，而主体由` <body> `标签定义

### lang 属性

该标签的`lang`属性，表示网页内容默认的语言。

```html
<html lang="zh-CN">
```

上面代码表示，网页是中文内容。如果是英文内容，`zh-CN`要改成`en`。

- en定义语言为英语，用于英文网页；
- zh-CN定义语言为中文，用于中文网页

其实对于文档显示来说，定义成en的文档也可以显示中文，定义成zh-CN的文档也可以显示英文。这个属性对浏览器和搜索引擎(百度、谷歌等)有作用，我们写网页都写成`<html lang="zh-cn">`

##  头部 `<head></head>`

`<head>`标签是一个容器标签，用于放置网页的元信息。它的内容不会出现在网页上，而是为网页渲染提供额外信息。

```html
<!doctype html>
<html>
  <head>
    <title>网页标题</title>
  </head>
</html>
```

- `<head>`是`<html>`的第一个子元素。如果网页不包含`<head>`，浏览器会自动创建一个。

- `<head>` 标签用于定义文档的头部，它是所有头部元素的容器。
- `<head>` 中的元素可以引用脚本、指示浏览器在哪里找到样式表、提供元信息等等。

文档的头部描述了文档的各种属性和信息，包括文档的标题、在 Web 中的位置以及和其他文档的关系等。

文档的头部的各种属性和信息都是一些说明性数据，绝大多数文档头部包含的数据都不会真正作为内容显示给读者。

`<head>`的子元素一般有下面七个：

- `<meta>`：设置网页的元数据。
- `<link>`：连接外部样式表。
- `<title>`：设置网页标题。
- `<style>`：放置内嵌的样式表。
- `<script>`：引入脚本。
- `<noscript>`：浏览器不支持脚本时，所要显示的内容。
- `<base>`：设置网页内部相对 URL 的计算基准。

注意：

- 请记住始终为文档规定标题！`<head>`中必须添加`<title>`标签，`<title>`标签的内容是网页的标题，会显示在网页标题栏的位置。
- 应该把 `<head> `标签放在文档的开始处，紧跟在 `<html>` 后面，并处于 `<body>` 标签之前。

## 元数据`<meta>`

`<meta>`标签用于设置或说明网页的元数据，必须放在`<head>`里面。一个`<meta>`标签就是一项元数据，网页可以有多个`<meta>`。`<meta>`标签约定放在`<head>`内容的最前面。

不管什么样的网页，一般都可以放置以下两个`<meta>`标签。

```html
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Page Title</title>
</head>
```

上面例子中，第一个`<meta>`标签表示网页采用 UTF-8 格式编码，第二个`<meta>`标签表示网页在手机端可以自动缩放。

`<meta>`标签有五个属性，下面依次介绍。

### charset 属性

`<meta>`标签的charset属性，用来指定网页的编码方式。该属性非常重要，如果设置得不正确，浏览器可能无法正确解码，就会显示乱码。

```html
<meta charset="utf-8">
```

上面代码声明，网页为 UTF-8 编码。虽然开发者可以使用其他的编码方式，但正确的做法几乎总是应该采用 UTF-8。

注意，这里声明的编码方式，应该与网页实际的编码方式一致，即声明了`utf-8`，网页就应该使用 UTF-8 编码保存。如果这里声明了`utf-8`，实际却是使用另一种编码（比如 GB2312），并不会导致浏览器的自动转码，网页可能会显示为乱码。

总结：

- `<meta charset="UTF-8">`是必须要写的代码，否则可能引起乱码的情况。
- 一般情况下，统一使用“UTF-8”编码，尽量统一写成标准的 "UTF-8"，不要写成 "utf8" 或 "UTF8"。
- 把`<meta charset="UTF-8">`写在`<head>`标签内第一行的位置，保证标题及内容不会乱码 

知识延伸：

字符集（Character set）是多个字符的集合，字符集种类较多，每个字符集包含的字符个数不同，常见字符集名

称：ASCII字符集、GB2312字符集、BIG5字符集、 GB18030字符集、Unicode字符集等。计算机要准确的处理各

种字符集文字，就需要进行字符编码（**字符编码**：是指将计算机的二进制编码与某个抽象字符集合一一对应的规

则），以便计算机能够识别和存储各种文字

在`<head>`标签内通过`<meta> `标签的 charset 属性来规定 HTML 文档应该使用哪种字符编码。

charset 常用的值有：GB2312 、BIG5 、GBK 和 UTF-8

- UTF-8 是Unicode字符集的一种编码，也被称为万国码，基本包含了全世界所有国家需要用到的字符为世界统一编码。
  - 好处：可以兼容全世界的操作系统，不会出现乱码情况。
  - 缺点：体积稍大点。

- GB2312是GB2312是中国国家标准的简体中文字符集。它所收录的汉字已经覆盖99.75%的使用频率，基本满足了汉字的计算机处理需要。在中国大陆和新加坡获广泛使用。

  - 好处：体积稍小点。

  - 缺点：国外的浏览者可能会出现乱码，获提示安装语言包。

一般，网站提供给全球看的一般用UTF-8，仅仅针对国内的用GB2312就可以了

### name、content 属性

`<meta>`标签的name属性表示元数据的名字，content属性表示元数据的值。它们合在一起使用，就可以为网页指定一项元数据。

```html
<head>
  <meta name="description" content="HTML 语言入门">
  <meta name="keywords" content="HTML,教程">
  <meta name="author" content="张三">
</head>
```

上面代码包含了三个元数据：`description`是网页内容的描述，`keywords`是网页内容的关键字，`author`是网页作者。

元数据有很多种，大部分涉及浏览器内部工作机制，或者特定的使用场景，这里就不一一介绍了。下面是一些例子。

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
<meta name="application-name" content="Application Name">
<meta name="generator" content="program">
<meta name="subject" content="your document's subject">
<meta name="referrer" content="no-referrer">
```

### http-equiv 、content

`<meta>`标签的http-equiv属性用来覆盖 HTTP 回应的头信息字段，content属性是对应的字段内容。这两个属性与 HTTP 协议相关，属于高级用法，这里就不详细介绍了。

```html
<meta http-equiv="Content-Security-Policy" content="default-src 'self'">
```

上面代码可以覆盖 HTTP 回应的`Content-Security-Policy`字段。

下面是另一些例子。

```html
<meta http-equiv="Content-Type" content="Type=text/html; charset=utf-8">
<meta http-equiv="refresh" content="30">
<meta http-equiv="refresh" content="30;URL='http://website.com'">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
```

## 标题 `<title></title>`

`<title>`标签用于指定网页的标题，会显示在浏览器窗口的标题栏。

浏览器会以特殊的方式来使用标题，并且通常把它放置在浏览器窗口的标题栏或状态栏上。

同样，当把文档加入用户的链接列表或者收藏夹或书签列表时，标题将成为该文档链接的默认名称。

```html
<head>
  <title>网页标题</title>
</head>
```

搜索引擎根据这个标签，显示每个网页的标题。它对于网页在搜索引擎的排序，有很大的影响，应该精心安排，反映网页的主题。

`<title>`标签的内部，不能再放置其他标签，只能放置无格式的纯文本。

## 主体 `<body></body>`

`<body>`标签是一个容器标签，用于放置网页的主体内容。浏览器显示的页面内容，都放置在它的内部。它是`<html>`的第二个子元素，紧跟在`<head>`后面。

- `<body>` 元素定义文档的主体。
- `<body> `元素包含文档的所有内容（比如文本、超链接、图像、表格和列表等等。）

```html
<html>
  <head>
    <title>网页标题</title>
  </head>
  <body>
    <p>hello world</p>
  </body>
</html>
```

# 空格和换行

HTML 语言有自己的空格处理规则。标签内容的头部和尾部的空格，一律忽略不计。

```html
<p>  hello world   </p>
```

上面代码中，`hello`前面的空格和`world`后面的空格，浏览器一律忽略不计。

标签内容里面的多个连续空格（包含制表符`\t`），会被浏览器合并成一个。

```html
<p>hello      world</p>
```

上面代码中，`hello`与`world`之间有多个连续空格，浏览器会将它们合并成一个。网页渲染的结果是，`hello`与`world`之间只有一个空格。

浏览器还会将文本里面的换行符（`\n`）和回车符（`\r`），替换成空格。

```HTML
<p>hello



world
</p>
```

上面代码中，`hello`与`world`之间有多个换行，浏览器会将它们替换成空格，然后再将多个空格合并成一个。网页渲染的结果是，`hello`与`world`之间有一个空格。

这意味着，HTML 源码里面的换行，不会产生换行效果。

# 注释

HTML 代码可以包含注释，浏览器会自动忽略注释。注释以`<!--`开头，以`-->`结尾，下面就是一个注释的例子。

```html
<!-- 这是一个注释 -->
```

注释可以是多行的，并且内部的 HTML 都不再生效了。

```html
<!--
  <p>hello world</p>
-->
```

上面代码是一个注释的区块，内部的代码都是无效的，浏览器不会解析，更不会渲染它们。

注释有助于理解代码的含义，复杂的代码块前面最好加上注释。

# HTML手册

1. W3SCHOOL HTML手册：https://www.w3school.com.cn/html/index.asp
2. 菜鸟教程HTML手册：https://www.runoob.com/html/html-tutorial.html
3. MDN HTML手册：https://developer.mozilla.org/zh-CN/docs/Learn/HTML

