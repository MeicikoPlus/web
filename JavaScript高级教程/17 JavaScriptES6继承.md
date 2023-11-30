# ES6实现继承

## 原型继承的关系图

<img src="..\images\原型中图像显现\原型继承关系图.png"  />





## 构造函数的类方法

```javascript
function Person(name, age) {
  this.name = name
  this.age = age
}

Person.prototype.eat = function() {console.log("eating")}
Person.prototype.run = function() {console.log("running")}

var p1 = new Person("meiciko", 18)
var p2 = new Person("Meiciko", 18)
```

像下面这种添加在类本身的方法叫做类方法：

```JavaScript
var names = ['meiciko', ... , "abc"]
Person.randomPerson = function() {
  var randomName = names[Math.floor(Math.random() * names.length)]
  return new Person(randomName,Math.floor(Math.random() * 101))
}
```

添加在原型身上的方法叫做实例方法。





## ES6面向对象用法

### 定义类

认识class定义类：

- 在ES6新的标准中使用了class（ES5的保留字）关键字来直接定义类；
- 但是`类的本质上依然是前面所讲的构造函数`，`原型链的语法糖`而已；

那么，如何通过class来定义一个类呢？

==class 类名 {}==

```javascript
class Person {

}

// 创建实例化对象
var p = new Person()
```

还有另一种写法来创建一个类：

```JavaScript
// 这种写法并不常见，是一种类似于函数表达式的方式，了解即可
// 另一种类的定义方法
var Student = class {

}

var stu = new Student()
```





### 类中的内容

通过new关键词去调用person类的时候，会==默认调用class中的constructor方法==。

正常情况下，constructor函数里面是没用东西的，所以我们回去自己实现一个方法。

==constructor只能有一个，不能有多个！==（写多个不会进行函数的重载）

constructor有自己独特的写法：

```JavaScript
class Person {
  constructor (name, age) {
    this.name = name
    this.age = age
  }
}
var p = new Person()
console.log(Person.prototype === p.__proto__) // true
```



### 类中的实例方法

实例方法定义在类的显示原型中，需要使用实例化对象去调用。

在原本的ES5中，定义一个函数需要写在Person的原型中，但是在新的语法中，可以写在class内部。（内聚性）（高内聚，低偶合）

```JavaScript
class Person {
  constructor (name, age) {
    this.name = name
    this.age = age
  }
	// 这些方法本质上是放在Person的prototype上面的（语法糖）
  running() {
    console.log(this.name + "running")
  }
  
  eating() {
    console.log(this.name + "eating")
  }
}


// 创建实例化对象
var p = new Person("meiciko", 18)
p.running() // meicikorunning
```

注意：多个方法之间不需要加逗号！（新语法）



### 类的访问器方法（Get和Set）

当想要对某个属性进行监听的时候，可以写上这个属性对应的访问器（一般是不需要的）

程序中以下划线开头的属性和方法，最好不去在外界访问。

```JavaScript
class Person {
  constructor (name, age) {
    this._name = name
    this.age = age
  }

  set name(value) {
    console.log("设置name")
    this._name = value
  }

  get name() {
    console.log("获取name")
    return this._name
  }

  running() {
    console.log("running")
  }
}

// 创建实例化对象
var p = new Person("meiciko", 18)
p.name = "Meciko"
console.log(p.name)

// 输出结果如下：
设置name
获取name
Meciko
```

可以监听属性的变更和读取。

举一个使用案例：

```JavaScript
class Rectangle {
  constructor(x, y, width, height) {
    this.x = x
    this.y = y
    this.width = width
    this.height = height
  }
  
  get position() {
    return {x: this.x, y: this.y}
  }
  
  get size() {
    return {width: this.width, height:this.height}
  }
}

var rect = new Rectangle(10, 20, 100, 200)
console.log(rect.position) // {x:10, y:20}
console.log(rect.size) // {width:100, height:200}
```



### 类的静态方法

静态方法通常用于定义直接使用类来执行的方法，不需要有类的实例，使用==static==关键词来定义。

静态方法在ES5中叫做类方法，这两个是同一个东西。

使用方法：在方法名字前面加static（仍然使用Person类来举例）

```JavaScript
class Person {
  constructor (name, age) {
    this._name = name
    this.age = age
  }
	
 	// 类方法（静态方法）
	static randomPerson() {
    return new this("Meiciko", 18)
  }
}

var p = new Person("meiciko", 18)
var p2 = Person.randomPerson()
```

类方法的this指向的是Person（因为是Person调用的）



## ES6继承

继承使用的关键词是==extends==

在继承中，父类也叫超类，子类也叫派生类

```JavaScript
class Person {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  
  eating() {console.log("eating")}
}

class Student extends Person {
  constructor(name, age, sno, score) {
    super(name, age) // 调用父类，必须放在this前面
    this.sno = sno
    this.score = score
  }
  
  studying() {console.log("studying")}
}

var stu = new Student("meiciko", 18, 114514, 114514)
console.log(stu.name) // meiciko
stu.eating() // eating
stu.studying() // studying
```



### super关键字

在ES6中，提供了super关键字：

- 执行==super.method(...)==，来调用一个父类的方法。
- 执行==super(...)==来调用一个父类的constructor。

需要注意的是：**在子类（派生类）的构造函数中使用this或者返回默认对象之前，必须先通过super调用父类的构造函数！**



**super的使用位置有三个：子类的构造方法，实例方法，静态方法。**

子类的构造方法重点使用上面已经提到，先说在实例方法中使用。

**有些时候，子类对于父类提供的实例方法并不完全满意**，这种时候可以使用super关键字对父类的方法进行**重写**。

```JavaScript
class Person {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  
  eating() {
    console.log("eating")
  }
}

class Student extends Person{
  constructor () {
    // ...
  }
  
  @overrite
  eating() {
    console.log("在教室里面")
    super.eating()
  }
}
```



还有在类的静态方法中使用：

```JavaScript
class Animal {
  constructor(name) {
    this.name = name
  }
  
  static eating() {
    console.log("static animal sleep")
  }
}

class Dog extends Animal {
 	eating() {
    console.log("在教室里面")
    super sleep() // 使用父类的类方法
  }
}
var dog = new Dog()
dog.eating() // static animal sleep
```

注意，从原型链来看，子类并不能继承父类的类方法，子类想要使用父类的类方法需要另外写一些东西区实现。但是ES6的语法糖中已经帮我们实现了这一步，所以可以直接从子类中调用父类的类方法。



### 继承内置类(Array, Object...)

之前都是继承Person，Animal等等，但是也可以直接继承内置类。

比如：==var arr = new Array(1, 2, 3, 4, 5)==

这样可以在调用些Array的实例方法的基础上再扩展一些额外的方法，比如获取最后一个函数等等

```JavaScript
class MyArray extends Array {
  get lastItem() {
    return this[this.length - 1]
  }
  get firstItem() {
    return this[0]
  }
}
var arr = new MyArray(10, 20, 30)
console.log(arr.lastItem) // 30
console.log(arr.firstItem) // 10
```

这就是相当于对一个类的扩展，在原本的东西上面加上了自己需要的一些方法！



当然，**也可以直接给Array进行扩展**：将方法直接设置在Array的显示原型对象中。

```JavaScript
Array.prototype.lastItem = function() {
  return this[this.length - 1]
}
var arr = new Array(10, 20, 30)
console.log(arr.lastItem()) // 30
```

当使用第二种做法的时候，相当于之后创建的数组对象都可以使用这个方法。



### JavaScript混入（mixin）的用法

在JavaScript不支持多继承，仅仅支持单继承。

```JavaScript
function mixinAnimal(BaseClass) {
  return class extends BaseClass {
    running() {
      console.log("running")
    }
  }
}

function mixinRunner(BaseClass) {
  return class extends BaseClass {
    flying() {
      console.log("flying")
    }
  }
}

class Bird {
  eating() {
    console.log("eating")
  }
}

var NewBird = mixinRunner(mixinAnimal(Bird))
// 上语句话也可以这样写:
class NewBird extends mixinRunner(mixinAnimal(Bird)) {
  
}
var bird = new NewBird()
bird.flying()
bird.running()
bird.eating()
```

这样可以利用混入的方法来实现多继承！



## babel可以将ES6转为ES5代码

ES6中的类是ES5的构造函数的语法糖，所以其实是可以将ES6中的代码转为ES5的代码的，因为它们在底层是等价的。

## ES6中的class转ES5

可以使用网站（babeljs.io）来转换代码！

```JavaScript
class Person {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  
  eating() {console.log("eating")}
}

class Student extends Person {
  constructor(name, age, sno, score) {
    super(name, age)
    this.sno = sno
    this.score = score
  }
  
  studying() {console.log("studying")}
}

var stu = new Student("meiciko", 18, 114514, 114514)
console.log(stu.name)
stu.eating()
stu.studying()
```

转为ES6：
```JavaScript
function _classCallCheck(instance, Constructor) {
  if (!(instance instanceof Constructor)) {
    throw new TypeError("Cannot call a class as a function");
  }
}

var Person = function Person(name, age) {
  _classCallCheck(this, Person);

  this.name = name;
  this.age = age;
};

Person.prototype.eating = function () {
  console.log("eating");
};

var Student = /*#__PURE__*/ (function (_Person) {
  function Student(name, age, sno, score) {
    _classCallCheck(this, Student);

    _Person.call(this, name, age);

    this.sno = sno;
    this.score = score;
  }

  Student.prototype = Object.create(_Person.prototype);
  Student.prototype.constructor = Student;

  Student.prototype.studying = function () {
    console.log("studying");
  };

  return Student;
})(Person);

var stu = new Student("meiciko", 18, 114514, 114514);
console.log(stu.name);
stu.eating();
stu.studying();
```

