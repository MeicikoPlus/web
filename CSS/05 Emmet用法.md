Emmet语法的前身是Zen coding，神一样的编码，它可以极大地提高敲代码的速度，被称为前端神器，它使用缩写，来提高html/css的编写速度

目前很多代码编辑器都已支持 Emmet：比如：VS Code内部已经集成该语法

Emmet语法列表 http://docs.emmet.io/cheat-sheet/

# 快速生成HTML结构语法

输入 “!” 或者 "html:5" 按tab键，1秒完成html基本结构的书写

## 类 .

p.name  `<p class="name"></p>`
p.name1.name1.name3 	`<p class="name1 name1 name3"></p>`

## id ##

p#name `<p id="name"></p>`
p.name#name `<p class="name" id="name"></p>`

## 文本 {}

p{你好} `<p>你好</p>`

## 属性 []

p[title=你好] `<p title="你好"></p>`
img[src=1.jpg] `<img src="1.jpg" alt="">`

## 父子关系  >

p>span  `<p><span></span></p> `

## 同级关系 + 

p+span 

```html
<p></p>
<span></span>
```

## 上级 ^

p>span^div

```html
<p><span></span></p>
<div></div>
```



p>span+a^div

```html
<p><span></span><a href=""></a></p>
<div></div>
```



p>span>a^^div

```html
<p><span><a href=""></a></span></p>
<div></div>
```

## 乘法 *

ul>li*5

```html
<ul>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
    <li></li>
</ul>
```

## 分组 ()

`ul>(li>a)*3`

```html
<ul>
    <li><a href=""></a></li>
    <li><a href=""></a></li>
    <li><a href=""></a></li>
</ul>
```



`table>(tr>td*3)*3`

```html
<table>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>
</table>
```

## 自增 $

p.name$*5

```html
<p class="name1"></p>
<p class="name2"></p>
<p class="name3"></p>
<p class="name4"></p>
<p class="name5"></p>
```



`h$.name${h$}*6`

```html
<h1 class="name1">h1</h1>
<h2 class="name2">h2</h2>
<h3 class="name3">h3</h3>
<h4 class="name4">h4</h4>
<h5 class="name5">h5</h5>
<h6 class="name6">h6</h6>
```



p.name$$*5

```html
<p class="name01"></p>
<p class="name02"></p>
<p class="name03"></p>
<p class="name04"></p>
<p class="name05"></p>
```



p.name$$$*5

```html
<p class="name001"></p>
<p class="name002"></p>
<p class="name003"></p>
<p class="name004"></p>
<p class="name005"></p>
```



p.name$@0*5

```html
<p class="name0"></p>
<p class="name1"></p>
<p class="name2"></p>
<p class="name3"></p>
<p class="name4"></p>
```



p.name$@-*5

```html
<p class="name5"></p>
<p class="name4"></p>
<p class="name3"></p>
<p class="name2"></p>
<p class="name1"></p>
```



p.name$@-3*5

```html
<p class="name7"></p>
<p class="name6"></p>
<p class="name5"></p>
<p class="name4"></p>
<p class="name3"></p>
```



p.name$@3*5

```html
<p class="name3"></p>
<p class="name4"></p>
<p class="name5"></p>
<p class="name6"></p>
<p class="name7"></p>
```



`p.name$@$*5`

```html
<p class="name11"></p>
<p class="name22"></p>
<p class="name33"></p>
<p class="name44"></p>
<p class="name55"></p>
```



综合练习：使用快捷方式输出一下结构

```html
<span>前面</span>
<table>
    <tr>
        <td><img src="img/01.jpg" alt=""></td>
        <td><img src="img/02.jpg" alt=""></td>
        <td><img src="img/03.jpg" alt=""></td>
    </tr>
    <tr>
        <td><img src="img/01.jpg" alt=""></td>
        <td><img src="img/02.jpg" alt=""></td>
        <td><img src="img/03.jpg" alt=""></td>
    </tr>
    <tr>
        <td><img src="img/01.jpg" alt=""></td>
        <td><img src="img/02.jpg" alt=""></td>
        <td><img src="img/03.jpg" alt=""></td>
    </tr>
</table>
<span>后面</span>
```



答案：`span{前面}+table>(tr>(td>img[img/$$.jpg])*3)*3^span{后面}`



# 快速生成CSS样式语法

## 在Emmet中每个单位都有一个缩写

- 不写单位默认为px  
- p对应%
- e对应em
- r对应rem

## CSS属性快速生成

fz20		`font-size: 20px;`
w10p	`width: 10%;`
h10e	`height: 10em;`
m10r	`margin: 10rem;`
p10	`padding: 10px;`
m10-20	`margin: 10px 20px;`
p10-20	`padding: 10px 20px;`

pos:a	`position: absolute;`

t10	`top: 10px;`
m30	`margin:  3px;`





# 格式化代码

## VS Code 快速格式化代码

快捷键：shift+alt+f

## VS Code 自动格式化代码

在settings.json下的【工作区设置】中添加以下语句： 

"editor.formatOnType": true, 

"editor.formatOnSave": true

此时保存的时候会自动格式化代码

## 格式化插件

**Prettier - Code formatter：**只关注格式化，只关心格式化文件(最大长度、混合标签和空格、引用样式等)，包括JavaScript · Flow · TypeScript · CSS · SCSS · Less · JSX · Vue · GraphQL · JSON · Markdown

