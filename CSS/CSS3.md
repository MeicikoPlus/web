# CSS发展

1994 年，Hkon Wium Lie 最初提出了 CSS 的想法，联合当时正在设计 Argo 的浏览器的Bert Bos，他们决定一起合作设计 CSS，于是创造了 CSS 的最初版本。

紧接着，他们在芝加哥的Mosaic and the Web 大会上第一次正式提出了 CSS 的建议，1995 年他们一起再次展示了这个建议。当时 W3C 刚刚建立，W3C 对 CSS 很感兴趣，为此专门组织了一次讨论会。

1996 年 12 月，W3C 推出了 CSS 规范的第一版本。

1997 年，W3C 颁布 **CSS1.0** 版本 ，CSS1.0 较全面地规定了文档的显示样式，可分为选择器、样式属性、伪类 / 对象几个部分。

这一规范立即引起了各方的关注，随即微软和网景公司的浏览器均能支持 CSS1.0，这为 CSS 的发展奠定了基础。

1998 年，W3C 发布了 CSS 的第二个版本 **CSS2.0**，目前的主流浏览器都采用这标准。

CSS2 的规范是基于 CSS1 设计的，包含了 CSS 所有的功能，并扩充和改进了很多更加强大的属性。包括选择器、位置模型、布局、表格样式、媒体类型、伪类、光标样式。

2005 年 12 月，W3C 开始 **CSS3** 标准的制定，到目前为止该标准还没有最终定稿。

# CSS3

CSS3 被拆分为"模块"。旧规范已拆分成小块，还增加了新的。

W3C 的 CSS3 规范仍在开发。但是，现在新的浏览器已经都支持 CSS3 属性。

# CSS3 浏览器支持

对CSS3是没有一个W3C标准的，但全部的主流浏览器都已经支持许多新的功能。下面是有关所有的新的CSS3属性和他们的浏览器支持的参考：

参考菜鸟教程浏览器支持手册：https://www.runoob.com/cssref/css3-browsersupport.html

# CSS3新特性

一些重要的 CSS3 新特性如下：

- 选择器
- 盒模型
- 背景和边框
- 文字特效
- 渐变
- 2D/3D转换
- 动画
- 弹性盒子(Flex Box)
- 多列布局
- 用户界面
- 多媒体查询

## 选择器

### 属性选择器

属性选择器可以根据元素的属性及属性值来选择元素

| 选择器            | 含义                             |
| ----------------- | -------------------------------- |
| ==E[att^="val"]== | 属性att的值以"val"开头的元素     |
| ==E[att$="val"]== | 属性att的值以"val"结尾的元素     |
| ==E[att*="val"]== | 属性att的值包含"val"字符串的元素 |

### 结构伪类选择器

结构伪类选择器，利用DOM树实现元素过滤，通过文档结构的相互关系来匹配元素，从而减少HTML文档对ID或类的依赖，有助于保持代码干净整洁。

#### 选择子元素 xxx-child

| 选择器                    | 含义                                                         |
| ------------------------- | ------------------------------------------------------------ |
| ==E:last-child==          | 选择父元素的最后一个子元素E，等同于 E:nth-last-child(1)      |
| ==E:only-child==          | 选择父元素下仅有的一个子元素，且该子元素匹配E元素，<br>等同于:first-child:last-child 或 :nth-child(1):nth-last-child(1) |
| **==E F:nth-child(n)==**  | 选择父元素E的第n个子元素F（n值起始值为1，而不是0）。<br />1、其中n可以是整数：1，2，3。<br />2、关键字：even 表示第偶数个子元素；odd表示第奇数个子元素。<br />3、公式：2n+1 表示第偶数个子元素，2n表示第奇数个子元素。 |
| ==E F:nth-last-child(n)== | 选择父元素E的倒数第n个子元素F。<br />此选择器与E F:nth-child(n)选择器计算顺序刚好相反，但使用方法都是一样的<br/>其中 :nth-last-child(1)始终匹配最后一个元素，与 :last-child等同 |

#### 选择同种元素 xxx-of-type

| 选择器                    | 含义                                                         |
| ------------------------- | ------------------------------------------------------------ |
| ==E:first-of-type==       | 选择父元素下使用同种标签的第一个子元素E，等同于:nth-of-type(1) |
| ==E:last-of-type==        | 选择父元素下使用同种标签的最后一个子元素E，等同于:nth-last-of-type(1) |
| ==E:only-of-type==        | 选择父元素下使用同种标签的唯一一个子元素，且该子元素匹配E元素<br>等同于:first-of-type:last-of-type或 :nth-of-type(1):nth-last-of-type(1) |
| ==E:nth-of-type(n)==      | 选择父元素内具有指定类型的第n个E元素，<br />与:nth-child()作用类似，但是仅匹配使用同种标签的元素 |
| ==E:nth-last-of-type(n)== | 选择父元素内具有指定类型的倒数第n个E元素<br />与:nth-last-child() 作用类似，但是仅匹配使用同种标签的元素 |

### 伪元素选择器

伪元素选择器表现得像是往文档中添加新的HTML元素一样，利用CSS创建新标签，而不需要直接在文档中HTML标签，从而简化HTML结构。伪元素开头为双冒号`::`。

| 选择器              | 含义                                                         |
| ------------------- | ------------------------------------------------------------ |
| ==E::first-line==   | 选择E元素的第一行                                            |
| ==E::first-letter== | 选择E元素的第一个字符，能实现类似印刷的首字放大的效果        |
| ==E::before==       | 前缀，在E元素之前插入生成的内容<br />用来和content属性一起使用，并且必须定义content属性 |
| ==E::after==        | 前缀，在E元素之后插入生成的内容<br />用来和content属性一起使用，并且必须定义content属性 |
| ==::placeholder==   | 选取带有占位符文本的表单元素，并让您设置占位符文本的样式<br />占位符文本使用 placeholder 属性设置，这个属性规定描述输入字段期望值的提示文本 |
| ==E::selection==    | 匹配元素中被用户选中或处于高亮状态的部分                     |

## 盒模型

box-sizing 改变盒子大小的计算方式：https://www.runoob.com/css3/css3-box-sizing.html

默认情况下`box-sizing: content-box;`，元素的宽度(width) 计算方式如下：

- 元素实际宽度 = width(宽度) + padding(内边距) + border(边框) 

设置为`box-sizing: border-box;`后，元素的宽度计算如下：

- 元素实际宽度 = width(宽度) 

 `box-sizing: border-box;` 效果更好，也正是很多开发人员需要的效果。很多浏览器已经支持 `box-sizing: border-box;` ，所有元素使用 box-sizing 是比较推荐的：

```css
* {
    box-sizing: border-box;
}
```

## 背景和边框

### 新背景属性

https://www.runoob.com/css3/css3-backgrounds.html

| 属性                                                         | 描述                     |
| :----------------------------------------------------------- | :----------------------- |
| [background-clip](https://www.runoob.com/cssref/css3-pr-background-clip.html) | 规定背景的绘制区域。     |
| [background-origin](https://www.runoob.com/cssref/css3-pr-background-origin.html) | 规定背景图片的定位区域。 |
| [background-size](https://www.runoob.com/cssref/css3-pr-background-size.html) | 规定背景图片的尺寸。     |

### 新边框属性

https://www.runoob.com/css3/css3-borders.html

| 属性                                                         | 说明                                           |
| :----------------------------------------------------------- | :--------------------------------------------- |
| [border-image](https://www.runoob.com/cssref/css3-pr-border-image.html) | 设置所有边框图像的速记属性。                   |
| [border-radius](https://www.runoob.com/cssref/css3-pr-border-radius.html) | 一个用于设置所有四个边框- *-半径属性的速记属性 |
| [box-shadow](https://www.runoob.com/cssref/css3-pr-box-shadow.html) | 附加一个或多个下拉框的阴影                     |

## 文字特效

新文本属性：https://www.runoob.com/css3/css3-text-effects.html

| 属性                                                         | 描述                                                    |
| :----------------------------------------------------------- | :------------------------------------------------------ |
| [hanging-punctuation](https://www.runoob.com/cssref/css3-pr-hanging-punctuation.html) | 规定标点字符是否位于线框之外。                          |
| [punctuation-trim](https://www.runoob.com/cssref/css3-pr-punctuation-trim.html) | 规定是否对标点字符进行修剪。                            |
| [text-align-last](https://www.runoob.com/cssref/css3-pr-text-align-last.html) | 设置如何对齐最后一行或紧挨着强制换行符之前的行。        |
| [text-emphasis](https://www.runoob.com/css3/css3-pr-text-emphasis.html) | 向元素的文本应用重点标记以及重点标记的前景色。          |
| [text-justify](https://www.runoob.com/cssref/css3-pr-text-justify.html) | 规定当 text-align 设置为 "justify" 时所使用的对齐方法。 |
| [text-outline](https://www.runoob.com/cssref/css3-pr-text-outline.html) | 规定文本的轮廓。                                        |
| [text-overflow](https://www.runoob.com/cssref/css3-pr-text-overflow.html) | 规定当文本溢出包含元素时发生的事情。                    |
| [text-shadow](https://www.runoob.com/cssref/css3-pr-text-shadow.html) | 向文本添加阴影。                                        |
| [text-wrap](https://www.runoob.com/cssref/css3-pr-text-wrap.html) | 规定文本的换行规则。                                    |
| [word-break](https://www.runoob.com/cssref/css3-pr-word-break.html) | 规定非中日韩文本的换行规则。                            |
| [word-wrap](https://www.runoob.com/cssref/css3-pr-word-wrap.html) | 允许对长的不可分割的单词进行分割并换行到下一行。        |

## 渐变

https://www.runoob.com/css3/css3-gradients.html

CSS3 渐变（gradients）可以让你在两个或多个指定的颜色之间显示平稳的过渡。

以前，你必须使用图像来实现这些效果。但是，通过使用 CSS3 渐变（gradients），你可以减少下载的时间和宽带的使用。此外，渐变效果的元素在放大时看起来效果更好，因为渐变（gradient）是由浏览器生成的。

CSS3 定义了两种类型的渐变（gradients）：

- **线性渐变（Linear Gradients）- 向下/向上/向左/向右/对角方向**
- **径向渐变（Radial Gradients）- 由它们的中心定义**

### 语法

线性渐变：`background-image: linear-gradient(direction, color-stop1, color-stop2, ...);`

径向渐变：`background-image: radial-gradient(shape size at position, start-color, ..., last-color);`

### 属性值

| 值                                                           | 说明                                      |
| :----------------------------------------------------------- | :---------------------------------------- |
| none                                                         | 无图像背景会显示。这是默认                |
| [linear-gradient()](https://www.runoob.com/cssref/func-linear-gradient.html) | 创建一个线性渐变的 "图像"(从上到下)       |
| [radial-gradient()](https://www.runoob.com/cssref/func-radial-gradient.html) | 用径向渐变创建 "图像"。 (center to edges) |

参考：

https://www.w3cplus.com/css3/new-css3-linear-gradient.html

https://www.w3cplus.com/css3/new-css3-radial-gradient.html

## 2D/3D转换

### 2D转换

新转换属性：https://www.runoob.com/css3/css3-2dtransforms.html

| Property                                                     | 描述                   | CSS  |
| :----------------------------------------------------------- | :--------------------- | :--- |
| [transform](https://www.runoob.com/cssref/css3-pr-transform.html) | 适用于2D或3D转换的元素 | 3    |
| [transform-origin](https://www.runoob.com/cssref/css3-pr-transform-origin.html) | 允许您更改转化元素位置 |      |

2D 转换方法

| 函数                            | 描述                                     |
| :------------------------------ | :--------------------------------------- |
| matrix(*n*,*n*,*n*,*n*,*n*,*n*) | 定义 2D 转换，使用六个值的矩阵。         |
| translate(*x*,*y*)              | 定义 2D 转换，沿着 X 和 Y 轴移动元素。   |
| translateX(*n*)                 | 定义 2D 转换，沿着 X 轴移动元素。        |
| translateY(*n*)                 | 定义 2D 转换，沿着 Y 轴移动元素。        |
| scale(*x*,*y*)                  | 定义 2D 缩放转换，改变元素的宽度和高度。 |
| scaleX(*n*)                     | 定义 2D 缩放转换，改变元素的宽度。       |
| scaleY(*n*)                     | 定义 2D 缩放转换，改变元素的高度。       |
| rotate(*angle*)                 | 定义 2D 旋转，在参数中规定角度。         |
| skew(*x-angle*,*y-angle*)       | 定义 2D 倾斜转换，沿着 X 和 Y 轴。       |
| skewX(*angle*)                  | 定义 2D 倾斜转换，沿着 X 轴。            |
| skewY(*angle*)                  | 定义 2D 倾斜转换，沿着 Y 轴。            |

### 3D转换

转换属性：https://www.runoob.com/css3/css3-3dtransforms.html

| 属性                                                         | 描述                                 | CSS  |
| :----------------------------------------------------------- | :----------------------------------- | :--- |
| [transform](https://www.runoob.com/cssref/css3-pr-transform.html) | 向元素应用 2D 或 3D 转换。           | 3    |
| [transform-origin](https://www.runoob.com/cssref/css3-pr-transform-origin.html) | 允许你改变被转换元素的位置。         | 3    |
| [transform-style](https://www.runoob.com/cssref/css3-pr-transform-style.html) | 规定被嵌套元素如何在 3D 空间中显示。 | 3    |
| [perspective](https://www.runoob.com/cssref/css3-pr-perspective.html) | 规定 3D 元素的透视效果。             | 3    |
| [perspective-origin](https://www.runoob.com/cssref/css3-pr-perspective-origin.html) | 规定 3D 元素的底部位置。             | 3    |
| [backface-visibility](https://www.runoob.com/cssref/css3-pr-backface-visibility.html) | 定义元素在不面对屏幕时是否可见。     | 3    |

3D 转换方法

| 函数                                                         | 描述                                      |
| :----------------------------------------------------------- | :---------------------------------------- |
| matrix3d(*n*,*n*,*n*,*n*,*n*,*n*, *n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*,*n*) | 定义 3D 转换，使用 16 个值的 4x4 矩阵。   |
| translate3d(*x*,*y*,*z*)                                     | 定义 3D 转化。                            |
| translateX(*x*)                                              | 定义 3D 转化，仅使用用于 X 轴的值。       |
| translateY(*y*)                                              | 定义 3D 转化，仅使用用于 Y 轴的值。       |
| translateZ(*z*)                                              | 定义 3D 转化，仅使用用于 Z 轴的值。       |
| scale3d(*x*,*y*,*z*)                                         | 定义 3D 缩放转换。                        |
| scaleX(*x*)                                                  | 定义 3D 缩放转换，通过给定一个 X 轴的值。 |
| scaleY(*y*)                                                  | 定义 3D 缩放转换，通过给定一个 Y 轴的值。 |
| scaleZ(*z*)                                                  | 定义 3D 缩放转换，通过给定一个 Z 轴的值。 |
| rotate3d(*x*,*y*,*z*,*angle*)                                | 定义 3D 旋转。                            |
| rotateX(*angle*)                                             | 定义沿 X 轴的 3D 旋转。                   |
| rotateY(*angle*)                                             | 定义沿 Y 轴的 3D 旋转。                   |
| rotateZ(*angle*)                                             | 定义沿 Z 轴的 3D 旋转。                   |
| perspective(*n*)                                             | 定义 3D 转换元素的透视视图。              |

## 动画

### 过渡动画

下表列出了所有的过渡属性：https://www.runoob.com/css3/css3-transitions.html

| 属性                                                         | 描述                                         |
| :----------------------------------------------------------- | :------------------------------------------- |
| [transition](https://www.runoob.com/cssref/css3-pr-transition.html) | 简写属性，用于在一个属性中设置四个过渡属性。 |
| [transition-property](https://www.runoob.com/cssref/css3-pr-transition-property.html) | 规定应用过渡的 CSS 属性的名称。              |
| [transition-duration](https://www.runoob.com/cssref/css3-pr-transition-duration.html) | 定义过渡效果花费的时间。默认是 0。           |
| [transition-timing-function](https://www.runoob.com/cssref/css3-pr-transition-timing-function.html) | 规定过渡效果的时间曲线。默认是 "ease"。      |
| [transition-delay](https://www.runoob.com/cssref/css3-pr-transition-delay.html) | 规定过渡效果何时开始。默认是 0。             |

### 关键帧动画

下面的表格列出了 @keyframes 规则和所有动画属性：

https://www.runoob.com/css3/css3-animations.html

| 属性                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [@keyframes](https://www.runoob.com/cssref/css3-pr-animation-keyframes.html) | 规定动画。                                                   |
| [animation](https://www.runoob.com/cssref/css3-pr-animation.html) | 所有动画属性的简写属性。                                     |
| [animation-name](https://www.runoob.com/cssref/css3-pr-animation-name.html) | 规定 @keyframes 动画的名称。                                 |
| [animation-duration](https://www.runoob.com/cssref/css3-pr-animation-duration.html) | 规定动画完成一个周期所花费的秒或毫秒。默认是 0。             |
| [animation-timing-function](https://www.runoob.com/cssref/css3-pr-animation-timing-function.html) | 规定动画的速度曲线。默认是 "ease"。                          |
| [animation-fill-mode](https://www.runoob.com/cssref/css3-pr-animation-fill-mode.html) | 规定当动画不播放时（当动画完成时，或当动画有一个延迟未开始播放时），要应用到元素的样式。 |
| [animation-delay](https://www.runoob.com/cssref/css3-pr-animation-delay.html) | 规定动画何时开始。默认是 0。                                 |
| [animation-iteration-count](https://www.runoob.com/cssref/css3-pr-animation-iteration-count.html) | 规定动画被播放的次数。默认是 1。                             |
| [animation-direction](https://www.runoob.com/cssref/css3-pr-animation-direction.html) | 规定动画是否在下一周期逆向地播放。默认是 "normal"。          |
| [animation-play-state](https://www.runoob.com/cssref/css3-pr-animation-play-state.html) | 规定动画是否正在运行或暂停。默认是 "running"。               |

## 弹性盒子

下表列出了在弹性盒子中常用到的属性：https://www.runoob.com/css3/css3-flexbox.html

| 属性                                                         | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| [display](https://www.runoob.com/cssref/pr-class-display.html) | 指定 HTML 元素盒子类型。                                     |
| [flex-direction](https://www.runoob.com/cssref/css3-pr-flex-direction.html) | 指定了弹性容器中子元素的排列方式                             |
| [justify-content](https://www.runoob.com/cssref/css3-pr-justify-content.html) | 设置弹性盒子元素在主轴（横轴）方向上的对齐方式。             |
| [align-items](https://www.runoob.com/cssref/css3-pr-align-items.html) | 设置弹性盒子元素在侧轴（纵轴）方向上的对齐方式。             |
| [flex-wrap](https://www.runoob.com/cssref/css3-pr-flex-wrap.html) | 设置弹性盒子的子元素超出父容器时是否换行。                   |
| [align-content](https://www.runoob.com/cssref/css3-pr-align-content.html) | 修改 flex-wrap 属性的行为，类似 align-items, 但不是设置子元素对齐，而是设置行对齐 |
| [flex-flow](https://www.runoob.com/cssref/css3-pr-flex-flow.html) | flex-direction 和 flex-wrap 的简写                           |
| [order](https://www.runoob.com/cssref/css3-pr-order.html)    | 设置弹性盒子的子元素排列顺序。                               |
| [align-self](https://www.runoob.com/cssref/css3-pr-align-self.html) | 在弹性子元素上使用。覆盖容器的 align-items 属性。            |
| [flex](https://www.runoob.com/cssref/css3-pr-flex.html)      | 设置弹性盒子的子元素如何分配空间。                           |

## 多列布局

下表列出了所有 CSS3 的多列属性：https://www.runoob.com/css3/css3-multiple-columns.html

| 属性                                                         | 描述                                      |
| :----------------------------------------------------------- | :---------------------------------------- |
| [column-count](https://www.runoob.com/cssref/css3-pr-column-count.html) | 指定元素应该被分割的列数。                |
| [column-fill](https://www.runoob.com/cssref/css3-pr-column-fill.html) | 指定如何填充列                            |
| [column-gap](https://www.runoob.com/cssref/css3-pr-column-gap.html) | 指定列与列之间的间隙                      |
| [column-rule](https://www.runoob.com/cssref/css3-pr-column-rule.html) | 所有 column-rule-* 属性的简写             |
| [column-rule-color](https://www.runoob.com/cssref/css3-pr-column-rule-color.html) | 指定两列间边框的颜色                      |
| [column-rule-style](https://www.runoob.com/cssref/css3-pr-column-rule-style.html) | 指定两列间边框的样式                      |
| [column-rule-width](https://www.runoob.com/cssref/css3-pr-column-rule-width.html) | 指定两列间边框的厚度                      |
| [column-span](https://www.runoob.com/cssref/css3-pr-column-span.html) | 指定元素要跨越多少列                      |
| [column-width](https://www.runoob.com/cssref/css3-pr-column-width.html) | 指定列的宽度                              |
| [columns](https://www.runoob.com/cssref/css3-pr-columns.html) | column-width 与 column-count 的简写属性。 |

## 用户界面

新的用户界面特性：https://www.runoob.com/css3/css3-user-interface.html

| 属性                                                         | 说明                                           |
| :----------------------------------------------------------- | :--------------------------------------------- |
| [appearance](https://www.runoob.com/cssref/css3-pr-appearance.html) | 允许您使一个元素的外观像一个标准的用户界面元素 |
| [box-sizing](https://www.runoob.com/cssref/css3-pr-box-sizing.html) | 允许你以适应区域而用某种方式定义某些元素       |
| [icon](https://www.runoob.com/cssref/css3-pr-icon.html)      | 为创作者提供了将元素设置为图标等价物的能力。   |
| [nav-down](https://www.runoob.com/cssref/css3-pr-nav-down.html) | 指定在何处使用箭头向下导航键时进行导航         |
| [nav-index](https://www.runoob.com/cssref/css3-pr-nav-index.html) | 指定一个元素的Tab的顺序                        |
| [nav-left](https://www.runoob.com/cssref/css3-pr-nav-left.html) | 指定在何处使用左侧的箭头导航键进行导航         |
| [nav-right](https://www.runoob.com/cssref/css3-pr-nav-right.html) | 指定在何处使用右侧的箭头导航键进行导航         |
| [nav-up](https://www.runoob.com/cssref/css3-pr-nav-up.html)  | 指定在何处使用箭头向上导航键时进行导航         |
| [outline-offset](https://www.runoob.com/cssref/css3-pr-outline-offset.html) | 外轮廓修饰并绘制超出边框的边缘                 |
| [resize](https://www.runoob.com/cssref/css3-pr-resize.html)  | 指定一个元素是否是由用户调整大小               |

## 多媒体查询

### CSS2 多媒体类型

`@media` 规则在 CSS2 中有介绍，针对不同媒体类型可以定制不同的样式规则。

例如：你可以针对不同的媒体类型(包括显示器、便携设备、电视机，等等)设置不同的样式规则。

但是这些多媒体类型在很多设备上支持还不够友好。

### CSS3 多媒体查询

CSS3 的多媒体查询继承了 CSS2 多媒体类型的所有思想： 取代了查找设备的类型，CSS3 根据设置自适应显示。

媒体查询可用于检测很多事情，例如：

- viewport(视窗) 的宽度与高度
- 设备的宽度与高度
- 朝向 (智能手机横屏，竖屏) 。
- 分辨率

目前很多针对苹果手机，Android 手机，平板等设备都会使用到多媒体查询。

### 用法

```css
@media screen and (max-width: 480px) {
    body {
        background-color: lightgreen;
    }
}
```

在屏幕可视窗口尺寸小于 480 像素的设备上修改背景颜色:

### CSS3 多媒体类型

| 值     | 描述                             |
| :----- | :------------------------------- |
| all    | 用于所有多媒体类型设备           |
| print  | 用于打印机                       |
| screen | 用于电脑屏幕，平板，智能手机等。 |
| speech | 用于屏幕阅读器                   |

更多多媒体查询：https://www.runoob.com/css3/css3-mediaqueries.html