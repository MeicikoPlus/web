# **JavaScript逻辑运算符**

JavaScript常用的逻辑运算符主要有三个: **或**(||), **与**(&&), **非**(!).

三个逻辑运算符的含义如下:

|      |                              |
| ---- | ---------------------------- |
| \|\| | 只需要一个为真就可以返回true |
| &&   | 需要同时为真才能返回true     |
| !    | 取反                         |

当出现以下的表达式的时候:

```javascript
let rs = a || b
// 当出现上述的表达式的时候, 表达式通常会从左到右进行计算.
// 表达式将会把数据全部转换成布尔值.
// 表达式会在计算到true的时候停止.
// 如果都不为true, 则返回后一个值的初始值.
```

```javascript
let rs = 0 || 1
console.log(rs) // 1
let rs2 = 0 || null
console.log(rs2) // null
```

当把 || 换为 && , 表达式将变为找到false的时候返回, 如果都是true, 则会返回最后一个.

```javascript
let rs = 1 && 0
console.log(rs) // 0
let rs2 = 1 && [1, 2, 3]
console.log(rs2) // (3) [1, 2, 3]
```

------



## 三元运算符

三元运算符的作用: 当我们**需要根据一个条件去赋值**的时候, 比如我们想要比较数字的大小来获取某个较大或较小的数字,  这个时候可以使用三元运算符, 也可以叫**条件运算符**.

三元运算符避免了使用ifelse导致代码臃肿的问题, 也是JavaScript唯一一个有多个操作的运算符.

三元运算符示例:

```javascript
let result = condition ? value1 : value2
// condition: 判断语句
// value1: 如果条件成立, 拿走分号左边的值
// value2: 如果条件不成立, 拿走分号右边的值.

// 举例:
let num1 = 1
let num2 = 3
let num3 = num1 > num2 ? num1 : num2
console.log(num3) // 3
```



