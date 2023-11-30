# **基本数据类型**

JavaScript有八种基本的数据类型, 其中包含其中原始类型和一种复杂类型.

这几种数据类型有:

1. Number: 数值.
2. String: 字符串.
3. Boolean: 布尔值.
4. Undefined: 未定义.
5. Null: 空.
6. Object: 对象.
7. Biglin (少见) .
8. Symbol (少见) .

当想要查看变量的类型的时候, 可以使用**typeof操作符**.

```javascript
let num = 666
console.log(typeof num) // 查看num的数据类型
```

------



# 数据类型的转化

不同的数据类型之间可以进行类型转化的操作, 比如把一个string类型的字符串转换成number类型... ...

转换类型有两种, 一种是**显示转换** (手动调用相对应的转换函数) , 另一种是**隐式转换** (JavaScript中的运算符和函数会自动赋值给变量正确的类型) .

```javascript
// 隐式转换
let num = 1 // 声明一个变量, JavaScript自动将其赋值为数值类型.
let Num = ture // 声明一个变量, JavaScript自动赋值为布尔类型.
let sum = num + Num
console.log(sum)
out> 2
// 之所以输出结果是2, 是因为在num和Num相加的时候, Num被隐式转换成为了1 (true是1, false是0).
```

```javascript
// 显式转换
let num = 1 // 声明一个变量, JavaScript自动将其赋值为数值类型.
String(num) // 使用对应的函数将其显式转换成string类型.
```



------



# string类型

string类型用来表示字符串, 最常用于对文本的表示. 

string类型的数据需要用单双引号包括 ("") ('') , 但两者并没有区别, 不过还是更推荐双引号.

```javascript
let name = "Meiciko"
```

string还可以用反引号括起来, 这种字符串叫做**模板字符串**, 模板字符串支持在内部使用${}来表示要添加的变量.

```javascript
let name = "Meiciko"
let message = `我的名字是${name}`
```

### 字符串中的转义字符

在一些特殊的场景, 可能会用到一些转义字符.

1. 一个反斜杠加一个单引号: 单引号.
2. 一个反斜杠加一个双引号: 双引号.
3. 两个反斜杠: 反斜杠.
4. \n : 换行符.
5. \r : 回车符.
6. \t : 制表符.
7. \b : 退格符.

------



# Boolean类型

布尔类型用来表示真和假, 它只有两个值, 分别为true和false.

------



# Object类型

Object类型表示一个对象, 对象有自己对应的一些属性, 这个属性可以是多种数据类型, 也可以是一给方法 (函数) , 当然, 也可以是另外一个对象!

对象类型是一种储藏**键值对**的更复杂的数据类型 (key-value), 其中key必须是字符串, value可以是任意类型, 键值对之间用逗号隔开.

当键名中需要空格等元素去声明的时候 , 应该使用双引号包括起来.

```javascript
let person = {
	name: "Meiciko",
	age: 18,
	height: "1.80m",
	run: function() {
    console.log("我在跑路")
  }
	// 在对象内部的函数我们称之为方法.
}
```

### 对象的声明

对象的声明有三种方式:

```JavaScript
// 最常用的方式
let person = {
	name: "Meiciko"
}
```

```JavaScript
// 构造函数
function Book(name) {
  this.name = name
}
let book1 = new Book("JavaScript")
let book2 = new Book("Java")
... ...
```

```JavaScript
// 类似于构造函数的方式
let obj = new Object()
obj.name = "ckw"
```



### 对象的使用

对象的使用通常有访问对象的属性, 添加对象的属性, 删除对象的属性, 修改对象的属性...

```JavaScript
// 这里有一个person对象
let person = {
    name: 'Meiciko',
    age: 18,
    height: '1.80m',
    run: function() {
        console.log('我在跑路')   
    },
    eat: function() {
        console.log('我在干饭')    
    },
  	friend: "Auto",
    "my friend": {
        name: 'teriri',
        age: 50    
    }
}
```

**访问**对象属性:

```javascript
// 这里是对应的访问对象属性的方法: 使用[对象.key]的方式.
console.log(person.name)
console.log(person.age)
console.log(person.friend)
console.log(person["my friend"].name)
// 上述对象中, 因为key值中间有空格, 所以我们不能使用person.my friend的方式去访问, 这个时候需要用[]的方式去访问.
person.run()
```

**添加**对象的属性以及**修改**对象的属性:

```JavaScript
// 添加对象的属性:
person.socre = 100 //如果原来有的话就是修改, 如果原来没有这个属性就是添加上新的属性
person["my friend"].boy_friend = "Meiciko"
person.studying = function(){
    console.log("我正在学习, 勿扰!")
}
```

**删除**对象的属性:

```JavaScript
delete person.age
delete person.height
delete person["my friend"].age
delete person.eat
```



### 对象的小练习

题目: 定义一个商品对象, 商品为雪碧, 价格为5元, 商品id为114514, 拥有一个购买的方法属性, 该属性可以接收购买数量来计算所花费的价格.

```JavaScript
let Goods = {
  name: "雪碧",
  price: 5,
  id: "114514",
  buy: function(num) {
    return num*this.price
  }
}
```



### 对象的遍历

一个对象中可能会有很多的属性, 我们可以用迭代的方法去遍历对象的属性.

```JavaScript
// 这是一个对象
let person = {
    name: 'Meiciko',
    age: 18,
    height: '1.80m',
    run: function() {
        console.log('我在跑路')   
    },
    eat: function() {
        console.log('我在干饭')    
    },
    friend: {
        name: 'teriri',
        age: 50    
    }
}

/*
Object.keys(对象名称): 这个方法用来获取指定对象的所有key值, 获取的是一个array数组.
正常的for循环:
*/
let person_keys = Object.keys(person) // 获取对象所有的key值
for (let i = 0; i < person_keys.length; i++) {
  var key = person_keys[i]
  var value = person[key]
  console.log(`Person对象的key为${key}, key对应的value为:${value}`)
}

// 使用迭代器:
for (let key in person) {
  console.log(`key:${key}, value:${person[key]}`)
}
```

------



# JavaScript数组

**数组是一种特殊的对象**, 在使用typeof访问数组会返回object, 但是JavaScript数组最好使用数组来描述.

**由于数组是一种特殊的对象的缘故, 所以数组的内部的对象可以保存函数, 也可以保存数组, 也可以保存对象**.



## 数组的创建

1. 使用数组文本创建, 处于简介和可读性考虑, 一般使用文本创建的方法.

```javascript
let students = ["student1", "student2", "student3", ... ...]
```

注: 最好不要在数组的最后一个元素之后写逗号, 可能会出现浏览器兼容性的问题.

2. 使用JavaScript关键词new创建

```JavaScript
var cars = new Array("Saab", "Volvo", "BMW")
```



## 访问数组元素

可以通过索引值来获得数组的某个元素, 数组的索引从0开始.

```javascript
let cars = new Array("Saab", "Volvo", "BMW")
let carName = cars[0] // 获取数组的第一个元素
```



## 改变数组元素

可以通过索引来获取元素的位置再进行修改.

```JavaScript
let cars = new Array("Saab", "Volvo", "BMW")
cars[0] = "Opel"
```



## 数组的属性和方法

### length属性

数组拥有length属性, 这个属性用来返回数组的长度 (元素的数目).

```JavaScript
let cars = new Array("Saab", "Volvo", "BMW")
console.log(cars.length) // 获取cars的长度
```



### 将数组转换成字符串

有两种方法, 一种是固定的逗号分隔的字符串, 一种是可以自定义分隔符的字符串

```JavaScript
// 逗号分隔
let fruits = ["Banana", "Orange", "Apple", "Mango"]
let str = fruits.toString()
console.log(str)
// out> Banana,Orange,Apple,Mango
```



join方法可以把数组中每个元素拿出来,  并使用特定的分隔符将它们拼接起来, 分隔符可以自定义.

```JavaScript
// 自定义分隔符
let fruits = ["Banana", "Orange", "Apple", "Mango"]
let str = fruits.join("*")
console.log(str)
// out> Banana*Orange*Apple*Mango
let str2 = fruits.join("")
console.log(str2)
// out> BananaOrangeAppleMango
```



### 删除元素和添加新元素

**删除数组中最后一个元素**, 可以使用pop(), pop()还会返回被删除的值.

```JavaScript
let fruits = ["Banana", "Orange", "Apple", "Mango"]
let i = fruits.pop()
console.log(fruits)
console.log(i)
// out> (3) ['Banana', 'Orange', 'Apple']
// out> Mango
```

**删除数组的首个元素**, 是shift(), 这给方法也会返回被删除的值, **并且把其它元素位移至更低的索引**.

删除数组元素还可以使用delete去直接删除, 但是会把元素变为undefined, 留下未被定义的空洞.

```javascript
delete fruits[0];
```



### 遍历数组

使用forEach(), 这个方法用于调用数组的每个元素, 并将元素传递给回调函数.

注: forEach()对于空数组不会执行回调函数.

```JavaScript
let arr = ["meiciko1", "meiciko2", "meiciko3"]
arr.forEach(function(i) {
  console.log(i)
})
```

forEach() 是不支持break和continue的, 可以使用return或者break去实现:

```javascript
let arr = [1, 2, 3, 4, 5];
// 实现continue
arr.forEach(function (item) {
  if (item === 3) {
      return;
  }
  console.log(item);
});
// 输出: 1, 2, 4, 5
```

```javascript
let arr = [1, 2, 3, 4, 5];
// 实现break
arr.every(function (item) {
	console.log(item);
	return item !== 3;
});
// 输出: 1, 2, 3, false
```

forEach()的其它参数:

| 参数           | 描述                           |
| :------------- | :----------------------------- |
| *currentValue* | 必需。当前元素                 |
| *index*        | 可选。当前元素的索引值。       |
| *arr*          | 可选。当前元素所属的数组对象。 |

```JavaScript
let arr = ["meiciko1", "meiciko2", "meiciko3"]
arr.forEach(function(item, index, arr) {
  console.log(index, item, arr)
})
/*
0 'meiciko1' >(3) ['meiciko1', 'meiciko2', 'meiciko3']
1 'meiciko2' >(3) ['meiciko1', 'meiciko2', 'meiciko3']
2 'meiciko3' >(3) ['meiciko1', 'meiciko2', 'meiciko3']
*/
```





**在数组的末尾增加一个新的元素**, 使用push()方法. 这个方法还会返回新的数组的长度.

```JavaScript
let fruits = ["Banana", "Orange", "Apple", "Mango"]
let i = fruits.push("kiwi")
console.log(fruits)
console.log(i)
// out> (5) ['Banana', 'Orange', 'Apple', 'Mango', 'kiwi']
// out> 5
```

**在数组的开头添加一个新的元素**, 可以使用unshift()方法, 这个方法可以返回新数组的长度, **并且反向位移旧元素**.



### 拼接数组

拼接数组可以使用**splice()**方法. 

这个方法的参数中, 第一个参数用来定义添加新元素的位置 (拼接) (定义的是新元素所在索引值) .

第二个参数用来定义要删除多少元素 (从第一个参数选中索引处开始 (包括自己) , 向后删除指定数量的原数组中的元素) .

其余参数用来定义要添加的新元素.

```JavaScript
let fruits = ["Banana", "Orange", "Apple", "Mango"]
fruits.splice(2, 0, "Lemon", "Kiwi")
console.log(fruits.toString())
// out> Banana,Orange,Lemon,Kiwi,Apple,Mango
```

也可以通过调整参数去删除元素

```JavaScript
fruits.splice(0, 1); // 删除第一个元素
```



### 合并数组

合并数组使用concat()方法. 

这个方法**不会改变现有数组**, 它只会**返回一个新的数组**. 这个方法内部的参数**可以使用任意数量的数组**.

```JavaScript
let arr1 = ["Emma", "Isabella"];
let arr2 = ["Jacob", "Michael", "Ethan"];
let arr3 = ["Joshua", "Daniel"];

let myChildren = arr1.concat(arr2, arr3); 
```



### 裁剪数组

裁剪数字可以使用slice() 方法, 这个方法可以裁剪数组的某个片段并且**返回一个新的数组**.

它可以接收一个参数, 也可以接收两个参数

第一个参数指的是裁剪地点的索引值(从0开始, 并且包含自己), 第二个参数如果存在的话, 代表的是裁剪结束地址索引值, (不包括本身, 比如第二个参数是3, 但是裁剪到索引值为2的地址)

```JavaScript
let fruits = ["Banana", "Orange", "Apple", "Mango"]
var newArr1 = fruits.slice(2)
// out> (2) ['Apple', 'Mango']
var newArr2 = fruits.slice(1, 3)
// out> (2) ['Orange', 'Apple']
```



### 数组排序



#### 以字母顺序排序

sort()方法可以对数组进行排序, reverse()可以反转数组中的元素, reverse()可以配合sort()对数组进行降序排序

```JavaScript
let fruits = ["Banana", "Orange", "Apple", "Mango"]
fruits.sort()
console.log(fruits)
// out> (4) ['Apple', 'Banana', 'Mango', 'Orange']
```



#### 以数字排序 (顺便找出最大最小值)

当数组中是由很多数字组成的时候, 如果使用sort(), 会出现20>100的情况, 因为首字母中2>1, 所以我们可以使用一个比值函数.

```JavaScript
// 升序:
let points = [40, 100, 1, 5, 25, 10]
points.sort(function(a, b){return a - b})
console.log(points)
// out> (6) [1, 5, 10, 25, 40, 100]

// 降序:
points.sort(function(a, b){return b - a})
console.log(points)
// out> (6) [100, 40, 25, 10, 5, 1]
```

比值函数的目的是定义另一种排序顺序, 当sort比较两个值的时候, 会将值发送到函数, 然后同返回值进行排序, 比如比较上述代码中的40和100的时候, 会调用function(10, 100) 根据函数内部的40 - 100 返回一个负值, 排序函数便会将40排序为比100更低的值.

也可以通过这种方式得到数组中的最大值 ( points[0] / points[points.length-1 ) 或者最小值 ( points[0] / points[points.length-1] ).

### 寻找最大最小值	

可以考虑使用Math对象的max或者min方法, 值得注意的是, 这两个函数不能接收array类型的数据, 需要使用apply()函数将array只转换成数值类型:

```JavaScript
let points = [40, 100, 1, 5, 25, 10]
let max_num = Math.max.apply(null, points)

// 注: 
let arr = [1, 2, 3]
console.log(Math.max(1, 2, 3))  // out: 3
console.log(Math.max(arr))  // 无法得到最大值
console.log(Math.max.apply(null, points)) // out: 3
```

不过, 最常用的还是自己创建方法, 将数组第一个值拟定为最大值, 然后向后对比, 当遇到比它大的值的时候将最大值拟定为该值:

```JavaScript
let arr = [1,2,3,5,4]
function Max(list) {
  let max = list[0] // 将最大值拟定为第数组一个数值
  for (let i = 1; i < list.length; i++) {
    // 使用判断语句不断调整max的值
    if (max < list[i]) {
      max = list[i]
      console.log(`最大值已经转变为${max}`)
    }
  }
  return max
}
let max = Max(arr)
console.log(max)
```

### 操作数组每一个元素并返回新数组

使用的map()方法,`map()`是一个数组方法，用于对数组的每个元素进行操作并返回一个新的数组。

`map()`方法**接受一个回调函数作为参数**，该回调函数会被应用到数组的每个元素上。回调函数可以接收三个参数：当前元素的值、当前元素的索引和被调用的数组。

```JavaScript
// 定义一个原始数组
const numbers = [1, 2, 3, 4, 5];

// 使用map()方法创建一个新数组，将原始数组中的每个元素加倍
const doubledNumbers = numbers.map(num => num * 2);

// 打印新数组
console.log(doubledNumbers);
// out: [2, 4, 6, 8, 10]
```



### 过滤数组

filter(), 这个方法用于对数组的过滤, 它会创建一个新的数组, 新数组的元素是通过检查指定数组中符合条件的所有元素.

注: filter()方法不会对空数组进行检测, 不会改变原始数组

```JavaScript
let nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
 
let res = nums.filter((num) => {
  return num > 5;
});
 
console.log(res);  // [6, 7, 8, 9, 10]
```



### 将数组中每个元素按顺序执行一次操作并将结果汇总为单个返回值

**`reduce()`** 方法对数组中的每个元素按序执行一个提供的 **reducer** 函数，每一次运行 **reducer** 会将先前元素的计算结果作为参数传入，最后将其结果汇总为单个返回值。

语法: reduce(callbackFn, initialValue)

```JavaScript
let nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
let sum = nums.reduce((before, after) => before + after, 0)
```

callbackFn: 为数组中每个元素执行的函数。其返回值将作为下一次调用 `callbackFn` 时的 `accumulator` 参数。对于最后一次调用，返回值将作为 `reduce()` 的返回值

initiaValue(可选): 如果指定了initiaValue, 则callbackFn开始运算的第一个值从+nitiaValue开始运行, 不加的话是从数组前两个值开始运行.

