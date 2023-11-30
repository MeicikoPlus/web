# **JavaScript this关键字**

## this的指向

函数中还有一个this的变量, this变量绝大多数情况会指向一个对象, 再一次默认调用的时候,  this指向的是一个window对象(window对象是全局提供的.) **如果一个函数是被某一个对象引用并且调用它, 那么this就会指向这个对象**.

```JavaScript
var obj = {
    name: "meiciko",
    age: 18,
    run: function() {
        console.log(this) // obj对象
        console.log(obj) // obj对象
        console.log(this === obj) // true
    }
}
obj.run()
```

所以我们可以这样使用this:

```JavaScript
var obj = {
    name: "meiciko",
    age: 18,
    run: function() {
        console.log(`${this.name}正在进行跑步`)
    }
}
obj.run()
```

## this的用法

1. 作为上下文的表示符.

   this关键字可以用于引用函数的上下文，即当前函数被调用时的对象或者全局对象。在函数内部，**this**关键字可以让我们引用函数的执行环境，以便操作执行环境中的属性和方法。(如上述例子)

2. 明确函数执行的作用域.

   **this**关键字可以让我们在不同的作用域中执行函数，因为它的值在函数被调用时是动态决定的。**如果一个函数被多次调用，并且每次调用时都传递了不同的对象作为参数，那么this关键字就可以让函数在不同的对象上执行**。

   ```JavaScript
   const person1 = { name: 'Alice' };
   const person2 = { name: 'Bob' };
   
   function sayHello() {
     console.log(`Hello, my name is ${this.name}.`);
   }
   
   sayHello.call(person1); // Hello, my name is Alice.
   sayHello.call(person2); // Hello, my name is Bob.
   ```

3. 链式调用.

   使用**this**关键字可以实现链式调用，即在一个对象上调用多个方法，每个方法都返回该对象本身，以便支持多次方法调用。

   ```JavaScript
   const calculator = {
     value: 0,
     add: function(num) {
       this.value += num;
       return this;
     },
     subtract: function(num) {
       this.value -= num;
       return this;
     },
     multiply: function(num) {
       this.value *= num;
       return this;
     },
     getValue: function() {
       return this.value;
     }
   }
   
   const result = calculator.add(1).multiply(2).subtract(1).getValue();
   console.log(result); // 1
   // 在这个示例中，我们定义了一个
   // calculator对象，它包含了多个方法，每个方法都返回该对象本身，以便支持链式调用。
   ```

4. 作为构造函数的标识符.

   当使用**new**关键字调用构造函数时，JavaScript会自动创建一个新的对象，并将**this**关键字指向这个新的对象，使得构造函数可以向新对象添加属性和方法。

   ```JavaScript
   function Person(name, age) {
     this.name = name;
     this.age = age;
   }
   
   const person = new Person('Alice', 30);
   console.log(person.name); // Alice
   console.log(person.age); // 30
   // 在这个示例中，我们定义了一个
   // Person构造函数，当使用
   // new关键字调用构造函数时，JavaScript会自动创建一个新的对象，并将
   // this关键字指向这个新的对象，以便构造函数可以向新对象添加属性和方法
   ```

5. 在闭包中使用.

   在闭包中，由于函数内部的变量和方法可以访问外部的变量和方法，**this**关键字可以帮助我们**在闭包中引用外部的对象或者全局对象**。