# **JavaScript类与对象**

## 面向对象中创建一系列的对象

第一种: 手动创建(存在大量重复代码).

```JavaScript
var stu_1 = {
    name: "stu1",
    age: 18
}
var stu_2 = {
    name: "stu2",
    age: 18
}
var ... ...
// 我们可以这样创建一个一个的对象, 我想创建更多的学生对象, 会很麻烦, 而且有很多相似的地方, 我们需要找一个方式来减少我们写的重复代码.
```

第二种方式: 工厂函数.

```javascript
funtion foo(n, a) {
    let stu = {}
    stu.name = n
    stu.age =a
    return stu
}
let stu1 = foo("stu1", 18)
let stu1 = foo("stu2", 18)
```

第三种方式: 构造函数.

```JavaScript
function create_stu(name, age, height) {
    this.name = name
    this.age = age
    this.height = height
    this.message = function() {
        console.log(`名字是:${this.name}, 年龄是:${this.age}, 身高是:${this.height}`)    
    }
}
let stu1 = new create_stu("ckw", 18, 1.80)
```

## 构造函数

构造函数是一个普通的函数, 从表达形式来说, 和千千万万的普通函数没有任何的区别, 但是**如果一个普通的函数被new操作符调用了以后, 这个函数就会被称之为构造函数.**

如果一个函数被**new**操作符调用了, 那么它会执行下面的操作:

1. 在内存种创建一个新的对象, 这个对象是一个空对象.
2. 这个对象内部的属性会被赋值为该构造函数的属性.
3. 构造凹函数的内部的this会指向创建出来的新的对象.
4. 执行构造函数内部的代码.
5. 如果构造函数没有返回非空对象, 那么会返回创建出来的新对象.

```JavaScript
function create_stu(name, age, height) {
    this.name = name
    this.age = age
    this.height = height
    this.message = function() {
        console.log(`名字是:${this.name}, 年龄是:${this.age}, 身高是:${this.height}`)    
    }
}
let stu1 = new create_stu("meiciko", 18, 1.80)
```

注: 在ES5之前, 都是用function来声明一个构造函数(类), 然后通过new关键字对其进行调用.

在ES6后. JavaScript可以像其它语言一样, 使用class来声明一个类.
