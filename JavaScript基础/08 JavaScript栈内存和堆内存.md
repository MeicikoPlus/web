# 栈内存和堆内存

数据从保存方式可以把数据分为两种类型, 一种叫**值类型**, 一种叫**引用类型**.

值类型: 原始类型的数据保存方式是**在内存中保存值的本身**.

引用类型: 对象类型的数据保存是**在栈内存中保存数据的存储地址**, 真正的数据保存在堆内存.

```javascript
// 对于如下变量
let name = "meiciko"
let age = 18
let obj = {
  key1: "value1"
  key2: "value2"
}
```

它的存储方式为:

![栈内存和堆内存](..\images\栈内存与堆内存\栈内存和堆内存(1).png)

再看这个案例:

```javascript
let person = {
  name: "meiciko",
  age: 18,
  friend: {
    name: "teriri",
    age: 18
  }
}
let new_friend = person.friend
```

![栈内存和堆内存](..\images\栈内存与堆内存\栈内存和堆内存(2).png)

在栈内存中, 我们存储了person的地址, 在堆内存中, 我们存储了person的各种属性值, 在在堆内存中我们的friend在内部储存了friend的地址, 当我们用一个新的变量new_friend来进行上述的赋值方式时, 会直接吧friend的地址赋值给new_friend.

## 值的传递

```JavaScript
// 先看以下案例
function bar(n){
	n = 100
}
let num = 10
bar(num)
console.log(num) // 10
/*
在上述过程中, 我们首先在栈内存中创建了一个值: num, 
并给他赋值为10, 当我们把这个值传入函数的时候, 
栈内存中会多出来一个值:n 并且n的值也会被赋值为10. 
后来我们在函数内又把n改成了200, 但是函数执行完后会被弹出栈, 
所以n会被销毁, 如果我们这时候打印num的话, 他还是会是10, 
因为从始至终这个num就没有被改变过, 所以我们需要给函数设置返回值.
*/
```

上述代码中, 函数中的值调用的时候是按照值传递的, 但是引用值却是按照引用传递的, 这个例子中的num的值被复制到了n在, 函数内部对n的修改不会影响num.

## 全等比较

```JavaScript
var num1 = 1
var num2 = 1
console.log(num1 === num2)
// true
var obj1 = {}
var obj2 = {}
console.log(obj1 === obj2)
// false
```

第一个输出true是因为它们存储在栈内存中且数值也同, 第二个是因为它们比较的时候, 存储的地址不同, 所以会返回不相同.