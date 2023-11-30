# JavaScript对象增强

## 对象中的属性控制

对象中的属性是很灵活的, 一般我们在定义对象的时候, 会把属性直接定义在对象内部, 或者直接添加到对象内部, 但是这样一来我们就不能对这个属性进行一些限制, 比如这个属性是否可以通过delete删除, 是否可以被forin遍历... ...

当我们==想要对一个属性进行比较精准的控制操作==的时候, 我们可以使用==属性描述符==.

- 通过属性描述符, 可以精准的添加或者修改对象的属性;
- 属性描述需要使用==Object.defineProperty==来对属性进行添加或者修改;

比如, 我们想: 让某个属性不能被删除? 让某个属性不能被写入, 只能被读取? 让某个属性遍历的时候不出现?

简单来说, 属性描述符就是告诉代码哪些能操作什么, 不能操作什么.



**Object.defineProperty() 方法会直接在一个对象上定义一个新的属性, 或者修改一个对象的现有属性, 并且返回此对象**.

使用方法: ==Object.defineProperty(obj, prop, descriptor)==

这个函数可以接收三个参数:

1. obj表示要定义属性的对象;
2. prop表示要定义或者修改的属性的名称或者Symbol;
3. descriptor要定于或修改的属性描述符;

这个函数有返回值, 也会将修改的对象返回.



## 属性描述符的分类

属性描述符可以分为两类:

- ==数据属性==描述符;
- ==存取属性==描述符;

|                | configurable | enmerable | value  | writable | get    | get    |
| -------------- | ------------ | --------- | ------ | -------- | ------ | ------ |
| **数据描述符** | 可以         | 可以      | 可以   | 可以     | 不可以 | 不可以 |
| **存取描述符** | 可以         | 可以      | 不可以 | 不可以   | 可以   | 可以   |





## 数据属性描述符

数据属性描述符有如下四个特性:

- ==configurable==: **表示属性是否可以通过delete删除属性**, 是否可以修改它的特性, 或者是否可以将它修改为存取属性描述符;

  当我们直接在一个对象上定义某个属性时, 这个属性的configurable为true;

  当我们通过属性描述符定义一个属性时, 这个属性的configurable默认为false;

```javascript
// 现在有一个对象:
var obj = {
  name: "meiciko",
  age: 18
}
```

```JavaScript
Object.defineProperty(obj, "name", {
  configurable: false
})
// 这时候使用delete obj.name 是删不掉的, 严格模式下会报错.
// 需要注意的是, 这个属性也可以控制是否同意再次使用Object.defineProperty来对这个属性进行修改.
// 如果再次使用Object.defineProperty对这个属性进行操作,会报错.
```

```javascript
Object.defineProperty(obj, "adress", {})
// 会给obj对象添加一个address的属性, 但是是undefined. 但是这个address的属性的configurable默认为false.

```

- ==Enumerable==: **表示属性是否可以通过for-in或者Object.keys()返回属性**;

  当我们直接在一个对象上定义某个属性时, 这个属性的Enumerable为true;

  当我们通过属性描述符定义一个属性的时候, 这个属性的Enumerable为false;

```JavaScript
// 像上述代码中的:
Object.defineProperty(obj, "adress", {})
// 这样定义出来的obj对象的address属性是不能被for-in或者keys()方法拿到的, 因为默认是false
// 用法相同于: configurable
```

- ==Writable==: 表示是否可以修改属性的值;

  当我们直接在一个对象上定义某个属性的时候, 这个属性的Writable为true;

  当我们通过属性描述符定义一个属性的时候, 这个属性的Writable为false;

- ==value==: 属性的value值, 读取属性时会返回该值, 修改属性时, 会对其进行修改;

  默认情况下value值是undefined

;

## 存取属性描述符

**注: 在vue2响应式原理时候会用到.**

```JavaScript
// 先定义一个对象
var obj = {
  name: "meiciko"
}
var _name = ""
```

对obj属性中的name添加描述符(存取属性描述符)

```JavaScript
Object.defineProperty(obj, "name", {
  configurable: false,
  enumerable: false,
  set: function(value) {
    conole.log("set方法被调用", value)
    _name = value
  },
  get: function() {
    console.log("get方法被调用")
    return _name
  }
})
// 可以用来监听这些设置的内容.
```

上述这样的写法就是存储属性描述符, 顾名思义, 可以存储存的过程和取的过程.

- ==get==: 获取属性时会执行的函数, 默认为undefined;
- ==set==: 设置属性时会执行的函数, 默认为undefined;



## 多个属性的描述符

```JavaScript
var obj = {
  name: "meiciko",
  age: 18,
  height: 2.00
}
```

当我向对这个对象中的多个属性添加一些描述的时候, 一个一个的去写Object.defineProperty是很麻烦的, 这个时候就有多个属性的描述符出现了:

==Object.defineProgerties()==

用法如下:
```JavaScript
Object.defineProgerties(obj, {
  name: {
    // 一些属性描述符
  },
  age: {
    // 一些属性描述符
  },
  // ... ...
})
```



## 对象方法的补充

**获取对象的属性描述符**: 获取某一个属性的描述符(不常用);

- ==getOwnPropertyDescriptor==

```JavaScript
getOwnPropertyDescriptor(obj, "name")
```

- ==getOwnPropertyDescriptors==

```javascript
getOwnPropertyDescriptors(obj)
```



**禁止对象扩展新属性**

==prevenExtensions==

给一个对象添加新的属性会失败(严格模式下会报错).

```JavaScript
var obj = {
  name: "why",
  age: 18
}

Object.prevenExtensions(obj)
obj.address = "河南" // 无法添加, 严格模式下会报错.
```



**禁止配置和删除属性, 密封对象**

==seal==

实际上是调用prevenExtensions, 并且将现有的属性configurable: false.



**不允许就该现有属性**

==freeze==

实际上是调用seal, 并且将现有属性的writable: false.