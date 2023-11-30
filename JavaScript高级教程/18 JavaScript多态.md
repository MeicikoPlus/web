# 多态

前面提到过，面向对象主要有三大特性：封装，继承，多态。

注：==继承是多态的前提==！



## JavaScript有多态吗？

维基百科对多态的定义：多态（polymorphism）是指不同数据类型的实体提供统一接口，或使用一个单一的符号来表示多个不同的类型。

即：==不同的数据类型进行同一个操作，表现出不同的行为，就是多态的体现==。

比如不同的动物，都会发出叫声，但是狗是汪汪汪，猫是喵喵喵等等。

```JavaScript
class Animal {
  sound() {
    console.log("开叫！")
  }
}

class Dog extends Animal {
  sound() {
    super()
    console.log("汪汪汪~")
  }
}

class cat extends Animal {
  sound() {
    super()
    console.log("喵喵喵~")
  }
}

class pig extends Animal {
  sound() {
    super()
    console.log("哼哼哼~")
  }
}
```

严格意义下的面向对象语言中，多态存在如下条件：

- 必须有继承（实现类接口）；
- 必须有父类的引用指向子类元素；



其实还有另一种理解，既然多态是对同一种操作的不同行为：

```JavaScript
function add(a, b) {
  return a + b
}
add(1, 2) // 3
add("1", "2") // "12"
```

上面就是多态，所以也可以说，JavaScript到处都是多态。