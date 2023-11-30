# eval函数

eval函数是什么？

1. ==eval是一个特殊的函数==，它可以==将传入的字符串当做JavaScript代码来运行==；

2. ==eval会将最后一局执行语句的结果作为返回值==。

   

总结：内建函数 eval 允许执行一个代码字符串。

如下所示：

```JavaScript
var evalstr = `var message = "hellow world“; var name = "meiciko"; `
eval(evalstr)

console.log(message)
```

 （注：如果有多行代码最好用分号隔开，且eval函数中的代码可以访问全局变量，严格模式下不可以。）





## eval开发中不推荐使用的原因

不推荐的原因有三点：

1. 可读性很差；
2. eval是一个字符串，那么可能在执行过程中被可以篡改，肯呢个造成被攻击的风险。
3. eval执行必须经过JavaScript解释器，不能被JavaScript引擎优化。