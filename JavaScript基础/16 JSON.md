# **JSON**

## JSON介绍

在目前的开发中, JSON(JavaScript Object Notataion)是一种非常重要的数据格式, 它**不是编程语言**, 而**是一种可以在服务器和客户端之间传输最常用的数据格式**.

虽然JSON被提出来的时候主要用于JavaScript, 但目前已经独立于编程语言, 很多编程语言都实现了将JSON转换成对应模型的方式.

### 其它的传输格式

1. XML: 在早期的网络传输中主要是使用XML来进行数据交换的, 但是这种格式在解析, 传输等各种方面都弱于JSON, 所以目前已经很少在使用了.

2. Protobuf: 另外一个在网络传输中目前已经越来越多使用的传输格式, 但支持JavaScript的时间较晚, 所以使用较少.

   

## JSON的使用场景

1. 网络数据的传输JSON数据.

2. 项目的某些配置文件(比如一些小程序的app.json).

3. 非关系型数据库(NoSQL)将json作为存储格式.

   

## JSON的基本语法

JSON支持三种类型的值, 且有严格的语法:

1. 简单值:数字, 字符串, 布尔, null... ...

   ```json
   123
   ```

   

2. 对象值.

   ```json
   {
   	"name": "meiciko",
     "age": 18,
     "firend": {
       "name": "teriri"
     }
   }
   ```

   JSON数据的key必须加上双引号.

   不可以写undifined.

3. 数组值.

   ```json
   [
     "123",
     123
   ]
   ```

   

JSON不支持注释.



## JSON序列化

某些情况我们希望将JavaScript中的复杂类型转化成JSON格式的字符串. 这样方便我们对其进行处理.

stringify: 将JavaScript类型转换成对应的JSON字符串.

```JavaScript
let obj = {
  name: "meiciko",
  age: 18,
  friend: {
    name: "teriri"
  }
}

// 将这个对象存储在localStorage中
localStorage.setItem("info", obj)
console.log(localStorage.getItem("info")) // [object Object]
// 所以当想要存一个对象到localStorage的时候, 需要进行一次序列化
// 先将obj对象序列化(对应json的字符串形式)
let objJSONString = JSON.stringify(obj)
console.log(objJSONString)

// 将对象存储到localStarage
localStorage.setItem("info", objJSONString)

// 查看存储的对象
let item = localStorage.getItem("info")
console.log(item, typeof item) 
// {"name":"meiciko","age":18,"friend":{"name":"teriri"}} string
```

## JSON反序列化

parse方法: 解析JSON字符串, 转回对应的JavaScript类型.

```JavaScript
// 将字符串转问到对象(反序列化) (接上文)
let newObj = JSON.parse(item)
console.log(newObj)
// 以下是输出结果(展开后)
{name: 'meiciko', age: 18, friend: {…}}
age: 18
friend: {name: 'teriri'}
name: "meiciko"
[[Prototype]]: Object
```

## Stringify的参数replacer

JSON.stringfy()方法将一个JavaScript对象或值转换为JSON字符串.

如果指定了一个指定的replacer数组, 则可以**选择性的仅包含数组指定的属性**.

```JavaScript
let obj = {
  name: "meiciko",
  age: 18,
  friend: {
    name: "teriri"
  }
}
const obj2 = JSON.stringify(obj, ['name', 'age'])
// {"name": "meiciko", "age": "18"}
```

如果指定了replacer是一个函数, 则可以**选择性地替换值**.

```
// 承接上文
const obj3 = JSON.stringify(obj, (key, value) => {
	console.log(key, value)
	if (key === "name"){
		return "coder"
	}
	return value
})
console.log(obj3)

// 以下为输出结果:
{name: 'meiciko', age: 18, friend: {…}}
test.html:20 name meiciko
test.html:20 age 18
test.html:20 friend {name: 'teriri'}name: "teriri"[[Prototype]]: Object
test.html:20 name teriri
test.html:26 {"name":"coder","age":18,"friend":{"name":"coder"}}
```

## parse的reviver函数

JSON.parse()方法用来解析JSON字符串, 构造由字符串描述的JavaScript值或者对象, 但是提供可选的reviver函数用以在返回之前对所得到的对象执行变换操作.

```javascript
const obj = JSON.parse(JSON字符串, (key, value) => {
	if (key === "time") {
		return value + 2
	}
	return value
})
```

