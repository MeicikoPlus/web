# CSS 三大特性

CSS的三个特性是指：层叠性、继承性、优先级。

# 层叠性

`CSS(Cascading Style Sheets)`又称为层叠样式表，所以这个第一个特性就是层叠性。要说层叠性就要先明确一个定义：**样式冲突** 。因为层叠性就是解决样式冲突的问题的。

**样式冲突**：是指一个标签指定了相同样式同值的情况。一般情况，如果出现样式冲突，会按照**书写顺序最后的为准** 。样式冲突与浏览器的渲染原理有关：一般打开网页，会先下载文档内容，加载头部的样式资源，然后按照从上而下，自外而内的顺序渲染`DOM`内容。

**所以在运行的过程中，上面的样式先执行，下面的样式元素会将上面的层叠掉** 。

层叠性是指当一个标签被设置了多个重复的样式的时候，一个属性会覆盖另外一个属性。

层叠性主要遵循的原则是就近原则，在不考虑优先级的情况下，在多个样式中最终生效的样式是离标签最近的样式。

层叠性原则： 

- 样式冲突，遵循的原则是就近原则，哪个样式离结构近，就执行哪个样式
- 样式不冲突，不会层叠

![image-20211029165017185](https://gitee.com/Jinxizhen/pic_resource/raw/master/images/image-20211029165017185.png)



**结论**：无论在同一个`div`中还是在不同的`div`中，后面的样式将前面的层叠掉了，所以这就是`css`的层叠性。

# 继承性

**继承性** 是指书写`css`样式表时，子标签会继承父标签的某些样式，有一些样式是具有继承性的，想要设置一个可继承的属性，只需将它应用于父元素即可。

## 优缺点

| 优点                                      | 缺点                                                         |
| ----------------------------------------- | ------------------------------------------------------------ |
| 继承可以简化代码，降低`css`样式的复杂性。 | 如果在网页中所有的元素都大量使用继承样式，那么判断样式的来源就会很困难。 |

## 可以继承的样式

并不是所有的`css`属性都可以继承，对于 **字体、文本属性等网页中通用的样式** 可以使用继承。例如：字体、字号、颜色、行距等可以在`body`元素中统一设置，然后通过继承影响文档中所有文本。而有一些属性就不具有继承性：边框、外边距、内边距、背景、定位、元素高属性。

| 可继承的（字体、文本属性等）                         | 不可继承的                                   |
| ---------------------------------------------------- | -------------------------------------------- |
| 颜色、font-开头、text-开头、line-开头的、white-space | 边框、外边距、内边距、背景、定位、高度、浮动 |

## 行高的继承性

```html
<style>
  body {
    font: 12px/1.5 "Microsoft YaHei";
  }
  div{
    /* div标签的行高继承body，文字大小继承body，div的行高为 18 = 12*1.5 */
  }
  p{
    /* p标签行高继承body行高1.5，p标签行高为 24 = 16*1.5 */
    font-size: 16px;
  }
</style>
</head>
<body>
  <div>好好学习，天天向上！</div>
  <p>生命在于运动</p>
</body>
```

- 如果子元素没有设置行高，则会继承父元素的行高为 1.5
- 此时子元素的行高是：当前子元素的文字大小 * 1.5 
- body 行高 1.5 这样写法最大的优势就是里面子元素可以根据自己文字大小自动调整行高

# 优先级

定义`css`样式时，经常出现两个或更多选择器应用在同一元素上，这时就会出现优先级的问题。

## css特殊性（权重Specificity）

关于`css`权重，我们需要一套计算公式去计算，这就是`css`特性。

| 元素                                                         | 贡献值                    |
| ------------------------------------------------------------ | ------------------------- |
| 继承、通用选择器（*）、子选择器（>）、相邻选择器（+）、同胞选择器（~）（+） | 0，0，0，0                |
| 标签选择器、伪元素选择器                                     | 0，0，0，1                |
| 类选择器、伪类选择器、属性选择器                             | 0，0，1，0                |
| ID选择器                                                     | 0，1，0，0                |
| 行内样式 style=“”                                            | 1，0，0，0                |
| !important                                                   | 无穷大                    |
| max-height、max-width覆盖width、height                       | 大于无穷大                |
| min-height、min-width                                        | 大于max-height、max-width |

遇到有贡献值的就进行**累加**，例如：

- `div ul li ---> 0,0,0,3`
- `.nav ul li ---> 0,0,1,2`
- `a:hover ---> 0,0,1,1`
- `.nav a ---> 0,0,1,1`
- `#nav p ---> 0,1,0,1`

**数位的进位是256，不满足就不会有进位：** `0,0,0,5 + 0,0,0,5 = 0,0,0,10` 而不是`0,0,1,0`，所以**不会存在`10`个`div`能赶上一个类选择器的情况** ，但是`256`个`div`的话，就不一定了。

- **权重不会继承**，所以父元素的权重再高也和子元素没有关系
- 继承的权重是0， 如果该元素没有直接选中，不管父元素权重多高，子元素得到的权重都是 0

- 如果有`!important`那么相加的那些无论多高都不管用

- 如果有`max-height/max-width`那么`!important`不管用，如果同时有`min-height/min-width`和 `max-height/max-width`，那么`max-height/max-width`的不管用

## 权重简单记忆法

- 通配符和继承权重为	            0
-  标签选择器为                            1
- 类、伪类、属性选择器为         10
- id选择器                                    100
- 行内样式表为 style=””             1000
-  !important 无穷大

![image-20211106172504012](https://gitee.com/Jinxizhen/pic_resource/raw/master/images/image-20211106172504012.png)

例如：

- `div ul li ---> 1+1+1 = 3`
- `.nav ul li ---> 10+1+1 = 12`
- `a:hover ---> 10+1 = 11`
- `.nav a ---> 10+1 = 11`
- `#nav p ---> 100+1 = 101`

## 总结

权重由高到低排序 `min-height/min-width` > `max-height/max-width` > `!important` > 行内样式 > ID选择器 > 类选择器、属性选择器和伪类选择器 > 元素选择器、伪元素选择器 > 通用选择器 、 继承样式

- 使用了`!important`声明的规则；

- 内嵌在`HTML`元素的`style`属性里面的声明；

- 使用了`ID`选择器的规则

- 使用了类选择器、属性选择器、伪元素和伪类选择器的规则

- 使用过了元素选择器的规则

- 只包含一个通用选择器的规则

- 继承自父元素的样式优先级是最低的

# 其他注意事项

## 特殊标签的样式需要通过样式层叠覆盖

给`body`指定样式，`a`标签和`h`标签都不会改变

因为`a`标签和`h`标签都是特殊的标签，都有自己默认的样式，要想改变，就应该在其元素中定义将元素的样式层叠掉。

## 继承的权重为0

```html
<div id="father" class="c1">
  <p id="son" class="c2">
    试问这行文字是什么颜色？
  </p>
</div>

<style type="text/css">
  #father #son{  /*0,2,0,0*/
    color:blue;
  }
  
  #father p.c2{  /*0,1,1,1*/
    color:black;
  }
  
  div.c1 p.c2{  /*0,0,2,2*/
    color:red;
  } 
  
  #father{  /*0,0,0,0*/
    color:green!important;
  }
</style>

<!--字体颜色是蓝色-->
```

## 权重一样，层叠性起作用

```html
<div id="box1" class="c1">
  <div id="box2" class="c2">
    <div id="box3" class="c3">
      文字
    </div>
  </div>
</div>

<style type="text/css">
  .c1 .c2 div{  /*0,0,2,1*/
    color:blue;
  }
  
  div #box3{  /*0,1,0,1*/
    color:green;
  }
  
  #box1 div{  /*0,1,0,1*/
    color:yellow;
  }
</style>

<!--字体颜色是黄色-->
```

## 超越!important

超越!important：max-width与width冲突的时候，max-width会覆盖width，width被忽略，即使width使用了!important

```html
<style>
  div {
    width: 480px !important;
    height: 100px;
    background-color: blueviolet;
    max-width: 200px;
  }
</style>
</head>

<body>
  <div>好好学习，天天向上！</div>
</body>
```

![image-20211029180702367](https://gitee.com/Jinxizhen/pic_resource/raw/master/images/image-20211029180702367.png)

##  超越最大

超越最大：当min-width与max-width冲突的时候，max-width被忽略，比如设置的min-width比max-width大时，min-width权重大

```html
<style>
  div {
    max-width: 200px!important;
    height: 300px;
    background-color: blueviolet;
    min-width: 300px;
  }
</style>
</head>

<body>
  <div>好好学习，天天向上！</div>
</body>
```

![image-20211029181201091](https://gitee.com/Jinxizhen/pic_resource/raw/master/images/image-20211029181201091.png)

# CSS初始化

CSS reset：CSS初始化是指重设浏览器的样式。因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。

最简单的初始化方法就是： `* { padding: 0; margin: 0;}` 

## 京东的样式初始化

```css
* {
	margin: 0;
	padding: 0;
  /* 添加 盒模型 border-box */
  box-sizing: border-box;
}
em,
i {
	font-style: normal;
}
li {
	list-style: none;
}
img {
	border: 0;
	vertical-align: middle;
}
button {
	cursor: pointer;
}
a {
	font-size: inherit;
	color: inherit;
  text-decoration: none;
}
a:hover {
	/* color: #c81623; */
}
button,
input {
	font-family: Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB,
		'\5B8B\4F53', sans-serif;
}
/* 定义font-family时,最好在最后加一个sans-serif,这样如果所列出的字体都不能用，则默认的sans-serif字体能保证调用; */
body {
	-webkit-font-smoothing: antialiased;
	background-color: #fff;
	font: 12px/1.5 Microsoft YaHei, Heiti SC, tahoma, arial, Hiragino Sans GB,
		'\5B8B\4F53', sans-serif;
	color: #666;
}
/*公共样式类*/
.hide,
.none {
	display: none;
}
.clearfix::after {
	visibility: hidden;
	clear: both;
	display: block;
	content: '';
	height: 0;
}
.clearfix {
	*zoom: 1;
}
.w {
	margin: auto;
	width: 1190px;
}
/* 省略号 */
.ellipsis {
	white-space: nowrap;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ellipsis2 {
	display: -webkit-box;
	overflow: hidden;
	text-overflow: ellipsis;
	-webkit-box-orient: vertical;
	-webkit-line-clamp: 2;
}
```

## 淘宝的样式初始化

```css
blockquote,
body,
button,
dd,
dl,
dt,
fieldset,
form,
h1,
h2,
h3,
h4,
h5,
h6,
hr,
input,
legend,
li,
ol,
p,
pre,
td,
textarea,
th,
ul {
	margin: 0;
	padding: 0;
}
body,
button,
input,
select,
textarea {
	font: 12px/1.5 tahoma, arial, 'Hiragino Sans GB', '\5b8b\4f53', sans-serif;
}
h1,
h2,
h3,
h4,
h5,
h6 {
	font-size: 100%;
}
address,
cite,
dfn,
em,
var {
	font-style: normal;
}
code,
kbd,
pre,
samp {
	font-family: courier new, courier, monospace;
}
small {
	font-size: 12px;
}
ol,
ul {
	list-style: none;
}
a {
	text-decoration: none;
}
a:hover {
	text-decoration: underline;
}
sup {
	vertical-align: text-top;
}
sub {
	vertical-align: text-bottom;
}
legend {
	color: #000;
}
fieldset,
img {
	border: 0;
}
button,
input,
select,
textarea {
	font-size: 100%;
}
button {
	border-radius: 0;
}
table {
	border-collapse: collapse;
	border-spacing: 0;
}

```

