# **DOM是什么?**

window对象提供了**DOM**和**BOM**.

DOM是**文档对象类型**(DocumentObjetModel) 简称DOM, 用于**将页面所有内容表示为可以修改的对象**.

BOM是 **浏览器对象类型**, 用于**处理文档之外的所有内容的其它对象**, 比如navigator, location, history等对象.

**浏览器将我们编写在HTML种的每一个元素都抽象成了一个对象, 所有这些对象都可以用JavaScript来对其进行访问, 也就是我们可以通过JavaScript来操作页面, 我们将这个抽象的过程称之为文档对象类型**.

## DOM Tree

什么是DOM树? 在一个页面中不是只有HTML和head和body元素, 也包含很多子元素, 它们在HTML种最终会形成一个比较抽象的**树结构**, 抽象成DOM的时候, 它们也会形成一个树结构, 我们称之为DOM树.

## DOM节点和HTML元素的区别

虽然DOM常用的节点: 比如元素节点, 文本节点, 等等等等都和HTML并无差别, 但是其实节点涵盖的黑绒要比HTML元素更加的丰富, **因为在HTML中的注释并不会算作是一个元素**, **但是这个算是一个注释节点**, 诸如此类(比如换行或者空行, 这些也算是文本节点, 但是不能被称之为元素.), 所以节点和元素还是有明显的区别的.

------



# **DOM操作**

## 获取所有节点的导航

节点之间的导航:

- 父节点: parentNode
- 前兄弟节点: previousSibling
- 后兄弟节点: nextSibling
- 子节点: childNodes
- 第一个子节点: firstChild
- 第二个子节点: lastChild

元素之间的导航:

- 父元素: parentElement
- 前兄弟元素: previousElementSibling
- 后兄弟元素: nextElementSibling
- 子元素: children
- 第一个子元素: firstElementChild
- 最后一个子元素: lastElementChild

## 高级元素: table/form的导航

## table导航

```HTML
<table>
  <thead> <!--可选的表头部分-->
    <tr>
      <th>表头单元格1</th>
      <th>表头单元格2</th>
      <th>表头单元格3</th>
    </tr>
  </thead>
  <tbody> <!--表格的主体部分-->
    <tr>
      <td>行1，列1</td>
      <td>行1，列2</td>
      <td>行1，列3</td>
    </tr>
    <tr>
      <td>行2，列1</td>
      <td>行2，列2</td>
      <td>行2，列3</td>
    </tr>
  </tbody>
</table>
table元素本身有一些默认样式，包括设置为
display: table，
width: 100%，和
border-collapse: collapse。
th和td元素是表格的单元格，它们默认有一些内边距和边框，可以通过CSS进行样式调整。
tr元素是表格的行，它默认没有样式，但可以通过CSS进行样式调整，例如设置背景颜色。
thead元素是可选的表头部分，它通常包含th元素，用于标识列的标题。
tbody元素是表格的主体部分，它包含tr元素和td元素，用于展示表格数据。
tfoot元素是可选的表格脚部分，它通常包含th或td元素，用于展示表格的汇总信息。
```

table元素也可以创建一个导航, 首先需要拿到table元素, 然后获取内部后代.

1. table元素支持很多属性:

   - table.rows: 元素的集合

   - table.caption/tHead/tFoot: 引用的元素

   - table.tBodies: 元素的集合

2. thead, tfoot, tbody元素提供了rows属性:
   - tbody.rows等: 表格内部的tr的集合

3. tr元素:

   - tr.cells: 在给定的tr中的td和th的单元格的集合

   - tr.sectionRowindex: 给定的tr的封闭的thead, tbody, tfoot中的位置

   - tr.rowindex: 在整个表格中tr的编号(包括表格的所有行)

```JavaScript
// 假设已经拿到了table元素并且命名为tableEl
console.log(tableEl.tHead, tableEl.tBodies, tableEl.rows, tableEl.tFoot) //一些获取子元素的办法
console.log(tableEl.row[2]) // row也是可以通过索引来获取内容的
```

## form导航

拿到from可以用document.froms, 可以加上索引值(表示拿到的第几个form元素).

获取from中的子元素:fromEl.elements, 这个也可以加上[0]的索引.

我们可以通过子元素的name来获取它们.

------

当元素彼此靠近或者相邻的时候, DOM导航非常有用, 而上述的两种获取元素的方法是为了让我获取想要的任意元素, 我们通常可以将两种方法结合使用, 比如我们可以使用任意获取元素的方法获取到某个元素, 然后用导航的方式来进一步获取它的子元素等等, 或者他的兄弟元素等.

| **方法名**                     | **搜索方式** |
| ------------------------------ | ------------ |
| **querSelector**               | css-selector |
| **querSelectorAll**            | css-selector |
| getElementById                 | id           |
| getElementsByName(用的不多)    | name         |
| getElementsByTagName(用的不多) | tag or "*"   |
| getElementsByClassName         | class        |

一般情况下, 更倾向于使用前两种方法.

```HTML
// 通过选择器查询.
// 类似css中的查找方法: (既:.什么什么或者直接是某种元素的方式)
<div class = "keyword" id = "account1"></div>
<div class = "keyword" id = "account2"></div>
<div class = "keyword" id = "account3"></div>
<div class = "keyword" id = "account4"></div>
<div class = "keyword" id = "account5"></div>
```

```JavaScript
const El = document.querySelector(".keyword") // 获取该元素, 而且指挥匹配到第一个叫keyword的元素后就停止并且获取它, 既只获取首个匹配的元素.
// 如果是由很多class值叫keyword的
const Els = document.querySelectorAll(".keyword") // 拿到所有的keyword元素, 注意的是Els是一个可以迭代的对象.
for (const el of Els){
    console.log(el)
} // 使用for...of去迭代这个Els对象来获取里面的每个class值为keyword的元素.
const El = document.querySelector("#account1") 
```

# **节点的属性**

```HTML
<-- 这是接下来举例种的HTML部分 -->
<body>
  <!-- 我是注释 -->
  我是文本
  <div class="box">我是box
      <p>我是段落</p>
  </div>
</body>
```

```JavaScript
const bodyChildNodes = document.body.childNodes // 获取了所有的子节点(包括注释节点, 文本节点和元素节点).
const commentNode = bodyChildNodes[1] // 获取到第一个节点(注释节点).
// 之所以第一个节点的索引不是0, 是因为<body>后面的空格和换行才是第一个节点.
const textNode = bodyChildNodes[2] // 获取到第二个节点(文本).
const divNode = bodyChildNodes[3] // 获取到第三个节点(元素).
```

1. nodeType: 获取节点类型, 一般来说有下面几种: 元素节点, Element或者实际文字, 一个comment节点(注释), Document节点, 描述文档类型的DocumenyType节点... ...

```JavaScript
console.log(commentNode.nodeType, textNode.nodeType, divNode.nodeType) // 查看是一个什么节点
```

2. nodeName: 可以获得节点的名称, 值得提出的是, 还有一个叫tagName, 这个是可以获得元素的标签名词, tagName于nodeName不同的地点在于: tagName只适用于元素节点, node是为任意的节点定义的.

```JavaScript
console.log(commentNode.nodeName, textNode.nodeName, divNode.nodeName) // 查看节点的名称(out: comment #text DIV).
console.log(divNode.tagName) // 查看元素的标签名词(out: DIV).
```

3. innerHTML, textContent, data属性:

   data是针对**非元素节点获取的数据**, **如果是元素节点, 就需要使用innerHTML**, 注意的是i**nnerHTML会把内部的标签和内容也拿到手**,

   但是 **textConten只会拿到元素节点的文本内容**, 标签不会带上.

   **当我们使用innerHTML或者textContent来设置文本内容的时候, 作用是一样的, 但是, 如果我们设置的内容种带有标签比如, innerHTML会把h2元素解析, 把内部的内容作为h2来显示, 但是textContent会把标签也当成文本的一部分去设置**.

   开发种会经常用到innerHTML, textContent, 注意的是, 当**使用这两个效果为元素内部设置内容的时候, 会把原本的内容给覆盖掉.**

```JavaScript
console.log(commentNode.data, textNode.data, divNode.data) // 拿到节点中的数据, (out:  我是注释  我是文本  undefined)
console.log(divNode.innerHTML) // 拿到元素节点的数据, (我是box  <p>我是段落</p>).
console.log(divNode.textContent) // 只拿到元素节点内的文本内容, (out: 我是box  我是段落)
```

4.  hidden属性: 这个属性用来控制元素隐藏, (需要是元素), 本质是把元素添加上了: dispaly: none; 

```JavaScript
divNode.hidden = true
```

------

# **元素的属性和特性**

## 什么是元素的属性和特性

例如元素内部的class, id, title, href... ...都是属性(attribute), 也可以叫做特性.

attribute的分类: 如果是HTML标准制定的attribute, 称之为**标准attribute**, 而我们**自己定义**的attribute, 称之为**非标准attribute**.

不是HTML元素的标准属性, 虽然在HTML中可以自由定义元素的属性，但它们不会自动映射到对应的DOM对象的属性上。

```html
<div class="box" id="box" age="11" name="meiciko"></div>
<script>
  const myEl = document.querySelector(".box")
  console.log(myEl.age) // 输出undefined, 表示找不到.
  console.log(myEl.id) // 输出box
  /*
  上述代码证明了不是HTML元素的标准属性, 
  虽然在HTML中可以自由定义元素的属性，
  但它们不会自动映射到对应的DOM对象的属性上。
  */
  // 想要找到需要用getAttribute(), 下面会介绍到.
  const age = myEl.getAttribute("age");
  console.log(age); // 输出 "11"
</script>
```



## attribute操作

所有的attribute都支持的操作 (假设我们有元素myel) 

| myel.hasAttribute(name)        | 检查特性是否存在                    |
| ------------------------------ | ----------------------------------- |
| myel.getAttribute(name)        | 获得这个特性值                      |
| myel.setAttribute(name, value) | 设置这个特性值                      |
| myel.removeAttribute(name)     | 移除这个特性                        |
| attributes                     | attr对象的集合, 具有name, value属性 |

## property

一般来讲, 对象里面的属性叫做property, 而HTML元素里的属性叫做attribute, 且一般来说, 对于标准的attribute, 会在DOM上创建与其对应的peoperty属性

## attribute和property的相互影响

大多数情况下, 这两个东西相互影响.

但是input的value只能用attribute去影响.

除非特殊的情况, 大多数情况下, 获取, 设置attribute, 推荐使用property的方式, 因为它默认情况下是有类型的.

------

# **操作class的值**

如果需要改的样式较多, 可以采用动态添加class的方式来做.

具体操作class的方法有: 

1. 直接修改: 

   ``` JavaScript
   box.className = "某某某"
   ```

   这种方法会直接替换掉类的字符串, 一般不采取.

2. 使用classList, elem.classList是一个特殊的对象, 它提供了一些方法.

   | .classList.add(class)      | 添加一个类                     |
   | -------------------------- | ------------------------------ |
   | .classList.remove(class)   | 删除/移除一个类                |
   | .classList.toggle(class)   | 如果类不存在就添加, 存在就移除 |
   | .classList.contains(class) | 检查给定的类, 返回true/false   |

​		(注: classlist是一个可以迭代的对象.)

------

# **创建元素**

创建元素的步骤一般如下所示:

```JavaScript
let divEl = document.querySelector(".box")
let h2El = document.createElement("h2")
h2El.innerHTML = "我是标题"
h2El.classList.add("title")
divEl.append(h2El)
```

插入元素的方式有很多, 一般采用如下几种:

- node.addend(): 在node末尾 插入节点或者字符串.
- node.prepend(): 在node开头, 插入节点或者字符串.
- node.before(): 在node前面, 插入节点或者字符串.
- node.after(): 在node后面, 插入节点或者字符串.
- node.replaceWidth(): 将node替换为指定的节点或者字符串.

------

# **克隆元素**

当想要赋值一个元素的时候, 可以使用cloneNode() 方法, 需要在方法的内传入一个布尔值来决定是否深度克隆.

```JavaScript
let newNode = boxEl.cloneNode()
console.log(newNode)
```

默认情况下只是克隆了元素, 并没有克隆元素的内容, 如果想要克隆内容(深度克隆), 需要在内部传入参数.

```JavaScript
let newNode = boxEl.cloneNode(true)
boxEl.after(newNode)
console.log(newNode)
```

------

# **移除元素**

移除元素使用元素自身的remove()方法:

```JavaScript
myEl.remove()
```



------



# **补充**

```JavaScript
<div class="box" id="abc" age="18"></div>
const box = document.querySelector(".box")
// 一般不推荐内部的自己写的自定义属性这样写, 一般这样:
div class="box" id="abc" data-age="18" data-height="1.88"></div>
// 这样, 自己写的自定义属性会存到一个叫dataset的地方, 当我们想查询它的时候, 我们可以:
console.log(box.dataset.age)
console.log(box.dataset.height)
```

