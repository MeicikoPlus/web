# CSS简介

HTML标签只关注内容的语义（结构标准），不关注页面样式的美化（展示标准），如果仅仅使用标签写页面就会导致页面变的很丑，使用 CSS 可以让我们的网页更加丰富多彩，布局更加灵活。

层叠样式表（**C**ascading **S**tyle **S**heet，简称：CSS）是为网页添加样式的代码，也会称之为 CSS 样式表或级联样式表

​    1、CSS是改变HTML文档样式的标记语言，层叠指的是样式的优先级，当产生冲突时以优先级高的为准

​    2、CSS 主要用于设置 HTML 页面中的文本内容（字体、大小、对齐方式等）、图片的外形（宽高、边框样式、边距等）、页面的布局和外观显示样式等

​    3、CSS 是**网页的美容师**，美化 HTML，让HTML更漂亮，让页面布局更简单

​    4、CSS 可以让 HTML 专注去做结构呈现，样式交给 CSS，即 结构 ( HTML ) 与样式( CSS ) 相分离

# CSS 规则集

使用 HTML 时，需要遵从一定的规范，CSS 也是如此。要想熟练地使用 CSS 对网页进行修饰，首先需要了解CSS 样式规则。

<img src="https://s2.loli.net/2022/06/24/DTuQOsMAnleihSP.png" alt="img" style="zoom:150%;" />

上述整个结构称为 **规则集**（通常简称“规则”），各部分释义如下：

- 选择器（**Selector**）

  HTML 元素的名称位于规则集开始。它选择了一个或多个需要添加样式的元素（在这个例子中就是 `h1` 元素）。要给不同元素添加样式只需要更改选择器就行了。

- 声明（**Declaration**）

  一个单独的规则，如 `color: red;` 用来指定添加样式元素的**属性**。

- 属性（**Properties**）

  改变 HTML 元素样式的途径。（本例中 `color` 就是`h1`元素的属性。）CSS 中，由编写人员决定修改哪个属性以改变规则。

- 属性的值（Property value）

  在属性的右边，冒号后面即**属性的值**，它从指定属性的众多外观中选择一个值（我们除了 `red` 之外还有很多属性值可以用于 `color` ）。

注意其他重要的语法：

- 每个规则集（除了选择器的部分）都应该包含在成对的大括号里（`{}`）。
- 在每个声明里要用冒号（`:`）将属性与属性值分隔开。注意：是英文的分号":"，不是中文的分号"："
- 在每个规则集里要用分号（`;`）将各个声明分隔开。注意：是英文的分号";"，不是中文的分号"；"

**注意：在代码，除了写中文是用中文输入，其余都用英文符号，字母a-zA-Z，标点符号：冒号: 分号; 单引号'' 双引号"" 点.等**

# CSS代码风格

```css
/* 紧凑格式 */
body {color: orange;font-size: 30px;}

/* 展开格式 */
body {
  color: orange;
  font-size: 30px;
}
```

1. **样式格式书写：**使用展开格式，不要使用紧凑格式，展开式更加直观，方便代码调试、查看，代码在打包时会把空格及换行去掉
2. **样式大小写风格：**样式选择器，属性名，属性值关键字**全部使用小写字母**
3. **样式空格风格：**选择器（标签）和大括号中间保留空格；属性值前面，冒号后面，保留一个空格

# CSS书写位置

 按照 CSS 样式书写的位置（或者引入的方式），CSS 样式表可以分为三大类：

1. 内部样式表（嵌入式）
2. 行内样式表（行内式）
3. 外部样式表（链接式）

## 内部样式表（内嵌样式表）

写到html页面内部，是将所有的 CSS 代码抽取出来，单独放到一个 `<style> `标签中 

​    1、`<style>` 标签理论上可以放在 HTML 文档的任何地方，但一般会放在文档的`<head>`标签中 

​    2、通过此种方式，可以方便控制当前整个页面中的元素样式设置

​    3、代码结构清晰，但是并没有实现结构与样式完全分离

​    4、使用内部样式表设定 CSS，通常也被称为嵌入式引入

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="author" content="金西振" />
    <title></title>
    <style>
      body {
        color: orange;
        font-size: 30px;
      }
    </style>
  </head>
  <body>
    <div>好好学习，天天向上。</div>
    <span>生命不止，奋斗不息。</span>
  </body>
</html>
```

## 行内样式表（内联样式表）

在元素标签内部的 style 属性中设定 CSS 样式。适合于修改简单样式

   1、style 其实就是标签的属性，样式写在双引号中间，写法要符合 CSS 规范

   2、可以控制当前的标签设置样式

   3、由于书写繁琐，并且没有体现出结构与样式相分离的思想，所以不推荐大量使用，只有对当前元素添加简

   单样式的时候使用

   4、使用行内样式表设定 CSS，通常也被称为行内式引入（行内样式/内联样式）

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="author" content="金西振" />
    <title></title>
  </head>
  <body>
    <div style="background-color: red; color: blue">好好学习，天天向上。</div>
    <span>生命不止，奋斗不息。</span>
  </body>
</html>
```

## 外部样式表

实际开发用的最多是外部样式表， 适合于样式比较多的情况。把CSS样式单独写到CSS文件中，然后CSS文件引入到 HTML 页面中使用.

​    引入外部样式表分为两步：

​    1、新建一个后缀名为 .css 的样式文件，把所有 CSS 代码都放入此文件中。

​    2、在 HTML 页面中，使用`<link>` 标签引入这个文件 

```HTML
<link rel="stylesheet" href="CSS样式文件路径">
```

- rel属性：规定当前文档与被链接文档之间的关系，在这里指定为stylesheet，说明被链接的文档是一个样式文件
- href属性：规定被链接文档的位置url，通常使用相对路径或者网络url

使用外部样式表设定 CSS，通常也被称为外链式或链接式引入，这种方式是开发中常用的方式

```html
<!DOCTYPE html>
<html lang="zh-CN">
  <head>
    <meta charset="UTF-8" />
    <meta name="author" content="金西振" />
    <title></title>
    <link rel="stylesheet" href="index.css">
  </head>
  <body>
    <div>好好学习，天天向上。</div>
    <span>生命不止，奋斗不息。</span>
  </body>
</html>
```

# CSS手册

1. W3SCHOOL CSS手册：https://www.w3school.com.cn/css/index.asp
2. 菜鸟教程CSS手册：https://www.runoob.com/css/css-tutorial.html
3. MDN CSS手册：https://developer.mozilla.org/zh-CN/docs/Learn/CSS
4. 第三方CSS手册：http://css.doyoe.com/

