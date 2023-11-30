



# **条件判断语句**

## 什么是代码块:

很多时候, 单行代码难以完成一个特定的功能, 可以将很多代码放在一个块中, 这个**用来完成某个特定功能的块内的全部代码的集成**就是一个代码块.

## 单分支结构

单分支语句: if (条件表达式)

```JavaScript
if (条件判断) {
	// 代码块
}
```

## 多分支结构

多分支语句: if ... else ...

```JavaScript
if (条件判断) {
	// 代码块
} else { // 上述条件的否定情况下执行
  // 代码块
}
```

多分支结构: if ... else if ... else ....

```javascript
if (条件判断) {
	// 代码块
} else if (条件判断) {
  // 代码块
} else {
	// 代码块
}
```

## Switch语句

switch是分支结构的一种语句, 它通过**判断变量或者表达式的结果是否等于case语句的常量, 来执行想要的分支语句**.

与if不同的是, **switch的等值判断是全等判断**( === )

**switch至少要有一个case代码块和一个可选的default选项**:

```JavaScript
switch(变量或者表达式的结果){
    case 常量1:
        //语句1
        break
    case 常量2:
        //语句2
        break
    default:
        //语句3
}
```

接下来是一个简单的示例:

```javascript
// 从弹窗获取输入的数字并转换成number类型.
var num = Number(prompt("请输入(1?2?3):"))
switch(num){
	case 1 :
		console.log('播放上一首')
		break
	case 2 :
		console.log('开始播放')
		break
	case 3 :
		console.log('播放下一首')
		break
	default :
		console.log('出错了')
}
```

## 循环语句

### while循环

```javascript
while (循环判断语句) {
	代码块
}

// 一个简单的案例:
let num = 1
while (num < 5) {
  num++
	console.log(num) // out: 2 3 4 5
}
```

接下来是一个利用while()的猜数字游戏框架:

```javascript
// 判断玩家是否愿意重新进行游戏
let ctn = true
while (ctn) {
  // 生成一个(1~10)随机数
  let rnum = Math.floor(Math.random() * 10)
  console.log("生成的随机数为:", rnum)
  let num = Number(prompt("请猜一个(0~10)的数字:"))
  for (let i = 1; i <= 5; i++) {
    if (num == rnum) {
      let str = prompt('恭喜你, 猜对了! 还想继续玩吗? (y/n)')
      if (str == "n") {
        ctn = false
      }
      break
    } else if (num != rnum && i != 5) {
      num = Number(prompt(`猜错了, 您还有${5 - i}次机会:`))
    } else {
      let str = prompt('已经没有机会了, 还想继续玩吗? (y/n)')
      if (str == "n") {
        ctn = false
      }
      break
    }
  }
}
```

上述代码中使用了"ctn"作为变量来主导游戏是否重开不重开, 利用while来控制整体逻辑.

## do while语句

do while循环与while循环很相似, 但是不同但是do while不管条件成立不成立, 都会执行一次 !

```javascript
do{
    //代码块
}while(循环条件)
```

## for 循环

```JavaScript
// for循环用法如下
for(begin; condition; step){
    //循环代码块
}
//例如我们打印1~100的数字
for(var i = 1; i <= 100 ; i++){
    console.log(i)
}
```

begin: 进入循环时执行一次.

condition: 每次循环时进行检查.

step: 每次循环迭代后才会执行.

for循环**经常用于嵌套**来解决问题:

```JavaScript
//如果我想打印出一个三角形的乘法表:
document.write('<table>')
for(var i = 1; i <= 9; i++){
    document.write("<tr>")
    for(var j = 1; j <= i; j++){
        document.write('<td>'+j+'×'+i+'='+i*j+'</td>') 
    }
    document.write("</tr>")
}
document.write('</table>')
```



```	javascript
for (var x = -6; x <= 6; x++) {
  for (var y = 0; y < (13 - Math.abs(x)*2 - 1)/2; y++) {
    document.write("&nbsp;")
  }
  for (var n = 0; n < Math.abs(x) * 2 + 1; n++) {
    document.write("*")
  }
  document.write("<br>")
}
    
```



## 循环控制

**break**: 直接跳出循环, 循环结束 ( 上述猜数字游戏中有使用到 ).

**continue**: 跳过本次循环 , 去执行下一次循环.



### for循环案例练习

题目为：

​		![](..\images\for循环练习\for循环练习.jpg)

```JavaScript
for (var x = -6; x <= 6; x++) {
  for (var y = 0; y < (13 - Math.abs(x)*2 - 1)/2; y++) {
    document.write("&nbsp;")
  }
  for (var n = 0; n < Math.abs(x) * 2 + 1; n++) {
    document.write("*")
  }
  document.write("<br>")
}

document.write("<br>")
document.write("<br>")
document.write("<br>")

for (var x = -3; x <= 3; x++) {
  for (var y = 0; y < Math.abs(x) + 1; y++) {
    document.write("*")
  }
  document.write("<br>")
}

document.write("<br>")
document.write("<br>")
document.write("<br>")

for (var x = -3; x <= 3; x++) {
  for (var y = 0; y < Math.abs(x) + 1; y++) {
    document.write("&nbsp;&nbsp;")
  }
  for (var n =0; n < 4 - Math.abs(x); n++){
    document.write("*")
  }
  document.write("<br>")
}

document.write("<br>")
document.write("<br>")
document.write("<br>")

for (var x = 0; x < 5; x++) {
  for (var y = 0; y < x; y++) {
    document.write("&nbsp;&nbsp;")
  }
  document.write("*")
  document.write("<br>")
}

document.write("<br>")
document.write("<br>")
document.write("<br>")

for (var x = 5; x > 0; x--) {
  for (var y = 0; y < x; y++) {
    document.write("&nbsp;&nbsp;")
  }
  document.write("*")
  document.write("<br>")
}

document.write("<br>")
document.write("<br>")
document.write("<br>")

for (var x = -3; x <=3; x++) {
  if (x == 0) continue 
  for (var y = 0; y < 3 - Math.abs(x); y++) {
    document.write("&nbsp;")
  }
  document.write("*")
  for (var n = 0; n < 2 * (Math.abs(x) - 1); n++) {
    document.write("&nbsp;")
  }
  document.write("*")
  document.write("<br>")
}

document.write("<br>")
document.write("<br>")
document.write("<br>")

for (var x = 0; x < 5; x++) {
  document.write("*")
  if (x == 0 || x == 4) {
    for (var y = 0; y < 3; y++) {
      document.write("*")
    }
  } else {
    for (var y = 0; y < 3; y++) {
      document.write("&nbsp;")
    }
  }
  document.write("*")
  document.write("<br>")
}

document.write("<br>")
document.write("<br>")
document.write("<br>")

for (var x = 0; x < 5; x++) {
  document.write("*")
  if (x == 0 || x == 4) {
    for (var y = 0; y < x - 1; y++) {
      document.write("*")
    }
  } else {
    for (var n = 0; n < x; n++) {
      document.write("&nbsp;")
    }
    document.write("*")
  }

  document.write("<br>")
}

document.write("<br>")
document.write("<br>")
document.write("<br>")

for (var x = -3; x <= 3; x++) {
  for (var y = 0; y < 7 - 2*Math.abs(x); y++) {
    document.write("○")
  }
  document.write("<br>")
}

document.write("<br>")
document.write("<br>")
document.write("<br>")

for (var x = -3; x <= 3; x++) {
  for (var y = 0; y < 2*Math.abs(x); y++) {
    document.write("&nbsp;&nbsp;")
  }
  for (var z = 0; z < 7 - 2*Math.abs(x); z++) {
    document.write("○")
  }
  document.write("<br>")
}
```

### for循环中的一写对称的思路

==当遇到类似沙漏这着对称的情况下，可以考虑使用负数来做，这样使用绝对值的时候，可以实现对称的效果。==
