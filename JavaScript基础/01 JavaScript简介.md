## JavaScript简介

JavaScript是一门基于**原型**, **头等函数**的语言, 是一门多范式语音, 它支持**面向对象设计**, **指令式编程**以及**函数式编程**.

------



## JavaScript语言的语法和基本对象

1. JavaScript是ECMAScript的语言层面的实现 (**定义语言规范**) .

2. 除了语言规范以外, JavaScript还需要对页面和浏览器进行各种操作 (**用于操作文档的API**) .

3. 除了基本实现外, 还包括DOM操作和BOM操作 (**用于操作浏览器的API**) .

------



## 谁去运行JavaScript

和HTML和CSS一样, JavaScript也是由浏览器 (准确来说是浏览器内部的**js引擎**) 帮我们运行的, 也可以让**node** (node也包含js引擎) 帮忙运行, 但是最后都要留给CPU进行运行, 但是CPU有自己的指令集, 所以需要js引擎帮助我们将js代码翻译成CPU指令来执行.

### 常见的JavaScript引擎

1. SpiderMonkey 
2. Chakra
3. JavaScriptCore
4. V8

### 	浏览器内核和js引擎的关系

以Webkit为例, 它由两部分组成, 一部分是**WebCore**, 另一部分也是**JavaScriptCore**,  前者用来负责HTML的解析, 布局和渲染等等工作, 后者用来解析和执行js代码.

------



## JavaScript的应用场景

 1. Web开发.

 2. 移动端的开发.

 3. 小程序端的开发.

 4. 桌面应用的开发.

 5. 后端开发.

    ------
    
    

## JavaScript的基本规范

	### js代码写在哪?

 1. 写在HTML代码行内部(不推荐)

 2. 写在<script>标签元素内部.

    ```html
    <script>
    	// JavaScript代码块... ...
    </script>
    ```

 3. 写在独立的js文件并且引入.

    ```html
    <script src="JavaScript文件的路径"></script>
    ```

### noscript元素

主要用于针对早期浏览器不支持js的问题. 需要一个**页面优雅降级**的处理方案, noscript元素主要用于给不支持js的浏览器提供代替内容.

	### 注意事项
	
	1.  script不能写成单标签. 包括外联引用的时候也不可以.
	2.  省略type属性 (以前的js中的script标签会使用type属性, 现在已经不需要了) .
	3.  网页的加载顺序遵顼HTML文档的加载顺序, 即自上而下的加载顺序, **推荐将js代码和编写位置放在body子元素的最后一行**.

