# 解构Destructuring

ES6中新增了一个从数组或者对象中方便获取数据的方法，称之为解构Destructuring。

`解构赋值是一种特殊语法，它使我们可以将数组或对象“拆包”至一系列变量中`。

解构也是一种特殊的语法！



# 数组的解构

## 基本用法

现在假设有一个数组如下：

```js
var arr = ["abc", "cba", "nba", "mba"]
```

现在有一个需求，将数组的前两个或者前三个拿出来赋值给变量，正常做法可能是`var name = arr[0]；var name2 = arr[1]` ...

**使用解构语法**后可以这样写：

```js
var [name1, name2, name3] = arr
```

这样写以后，ES6会从arr数组中从第一个元素挨个给name1，name2，name3赋值。



## 跳过某个元素

```js
// 比如我想要使用第一个元素和第三个元素，不想要第二个，可以如下
var [name1, , name3] = arr
// 不用的地方需要空出来
```



## 解构出数组

需求：前两个解构为单独变量，后两个解构为数组。

```js
var [name1, name2, ...newNames] = arr
```

写法有点类似于剩余参数。



## 解构的默认值

如果数组没有值，其实可以使用默认值。

```js
// 假设一个数组中有undefined
var arr = ["abc", "cba", undefined, "nba", "mba"]
```

```js
// 解构支持默认值
var [name1, name2, name3] = arr // name3为undefined
var [name1, name2, name3 = default] = arr // name3为default
```







# 对象的解构

## 基本用法

对象的解构是没有顺序的，它是**`根据key来解构`**的。

```js
var obj = {
  // ...
}
```

接下来使用解构语法来给变量赋值（用法类似）：

```js
var {name, age, height} = obj
```

当然也可以只解构出自己想要的东西：

```js
var {name, age} = obj
```



## 重命名问题

因为是根据key来结构的，所以变量的名称需要和key名称相同，但是很多时候我们需要重命名

```js
var {height: WHeight, name: WName, age: WAge} = obj
console.log(WHeight, WName, WAge)
```





## 默认值

```js
var {
  height: WHeight, 
  name: WName, 
  age: WAge,
  address: WAddress = "CN" // 设置默认值
} = obj
```





## 对象的剩余内容

```js
var {
  height: WHeight, 
  name: WName, 
  ...newobj
} = obj
console.log(newobj) // 打印的是一个对象
```





# 解构的应用

```js
function getPosition(position) {
  // 直接将传入的对象进行解构
  var {
    x,
    y
  } = position
  console.log(x)
  console.log(y)
}
getPosition({x: 10, y:10})
```

- 开发中拿到一个变量的时候，自动对其进行解构使用。
- 比如对函数的参数进行解构。