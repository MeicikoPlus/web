# Set  and  Map

在ES6之前，我们存储数据的主要方法是对象或者数组，但是在ES6之后，新增了另外两种数据结构：`Set（集合：内部的元素不会重复）` ，`Map（映射：类似于对象，但是不是对象，映射的key可以更为复杂，比如使用一个对象来作为key）` ，以及它们的另外形式 `WeakSet` ，`WeakMap` 。

# Set



## 创建 Set

`Set` 是一个新增的数据结构，可用来保存数据，类似与数组，但是和数组的区别在==元素不能重复==。

创建 `Set` 我们需要通过 `Set `构造函数 （暂时你没有字面量创建的方式）。

```js
// 通过构造函数来创建set
const arr = new Set() 
console.log(arr) // 输出：Set(0) {size:0}
```



## 添加内容

添加内容可以使用 `set.add()`

```js
arr.add(10)
arr.add(20)
arr.add(30)
arr.add(30) // 尝试添加重复元素
```

```js
console.log(arr) // 输出：Set(3) {10, 20, 30} 可以看到set不能添加重复的元素。
```

`set` 还可以添加对象：

```js
const arr = new Set() 
const obj = {}
const info = {}
const newobj = obj
arr.add(obj)
arr.add(info)
arr.add(newobj)
console.log(arr) // 输出：Set(2) {{}, {}}
```



## 应用场景

### 数组去重

`Set` 可以用于数组的去重：

```js
const arr = [1, 1, 2, 2, 3, 3, 4, 4, 5, 5]
const newArr = new Set(arr)
console.log('newArr: ', newArr); // 输出：Set(5) {1, 2, 3, 4, 5}
```

利用 `Set` 去重后，可以将其转化为数组，所以最终版步骤如下所示：

```js
const arr = [1, 1, 2, 2, 3, 3, 4, 4, 5, 5]
const newArr = Array.from(new Set(arr))
console.log('newArr: ', newArr); // 输出：Set(5) [1, 2, 3, 4, 5]
```



## Set 的其他属性和方法

### size 属性

`size` 属性用来返回 `Set` 中元素的个数（尺寸）。

```js
const arr = [1, 1, 2, 2, 3, 3, 4, 4, 5, 5]
const newArr = new Set(arr)
console.log(newArr.size) // 输出： 5
```



### size常用方法

| 方法名                         | 效果                                                 |
| ------------------------------ | ---------------------------------------------------- |
| add(value)                     | 添加某个元素，返回 set 本身。                        |
| delete(value)                  | 从 set 中删除和这个值相等的元素，返回 boolean 类型。 |
| has(value)                     | 判断 set 中是否存在某个元素，返回 boolean 类型。     |
| clear(value)                   | 清空 set 中所有元素，没有返回值。                    |
| forEach(callback, [, thisArg]) | 通过 ForEach 遍历 set 。                             |



### set 同样支持 for ... of

`set` 是一个可迭代对象，所以也支持迭代器。

```js
const arr = [1, 1, 2, 2, 3, 3, 4, 4, 5, 5]
const newArr = new Set(arr)
for (var item of newArr) {
  console.log(item)
}
```

```js
1
2
3
4
5
```







------



# Weak Reference  and  Storng Reference

在接下来学习 `WeakSet` 前，需要先理解两个词：`Weak Reference`，`Storng Reference` ，即 `强引用` 于 `弱引用` 。

已知，当 GC 开清理内存中的垃圾的时候，会查找内存中对象的引用关系，当发现有一个对象被另一个对象引用，GC 不会去主动清除它，但是，如果一个对象弱引用了另一个对象的话，虽然我们可以通过引用来拿到它，但是这个引用会被当做没有被引用，当 GC 发现的时候，可能会被清理掉。

而其他正常情况的引用，可以叫做强引用也可以叫做引用。

```js
let obj1 = {name: "meiciko"}
let obj2 = {name: "meiciko"}
let obj3 = {name: "meiciko"}

const names = [obj1, obj2, obj3]
obj1 = null
obj2 = null
obj3 = null
```

上述数组存储的都是对象对应的内存地址，当把这些变量赋值为 null ，这三个对象也不会被销毁掉，因为其实这三个对象还被数组引用，所以并没有被销毁。

```js
let obj1 = {name: "meiciko"}
let obj2 = {name: "meiciko"}
let obj3 = {name: "meiciko"}

let names = [obj1, obj2, obj3]
obj1 = null
obj2 = null
obj3 = null

const set = new Set(names)
names = null
```

再看上述操作，虽然 arr 这次被销毁掉了，但是这三个一开始被引用的对象并不会被销毁，因为从内存中来开，这三个对象还在被 arr 去引用，虽然我们将 arr 变成了 null 但是，arr 对应的对象此时已经被 set 所引用，所以这三个对象依然存在。





------



# WeakSet

通过上述对强引用和弱引用关系以及案例的理解，再来理解 `WeakSet` ，如果上述的案例中，使用的是 `WeakSet` 结果会变的不一样，在讨论会变成什么样的结果之前，先来理解 `WeakSet` 和 `Set` 之前的区别：

-  `WeakSet` 中==只能存放对象类型，不能存放基本数据类型==；

-  `WeakSet` 对==元素的引用是弱引用（Weak Reference）==，如果没有其他引用对某个对象进行引用，那么 GC 可以对该对象进行回收；

  

## 使用方法

注意：`WeakSet` 只能存储对象类型

```js
let obj1 = {name: "meiciko"}
let obj2 = {name: "meiciko"}
let obj3 = {name: "meiciko"}

const weakSet = new WeakSet()
weakSet.add(obj1) // √
weakSet.add(123) // 报错，明确要求必须添加的是数组对象
```

注意， `WeakSet` 中的内容是弱引用的，所以不保证有时候一定能从 `WeakSet` 中拿到想要的内容。



## 回到案例

回到 Weak Reference  and  Storng Reference 中的案例来看，下面这段代码：

```js
let obj1 = {name: "meiciko"}
let obj2 = {name: "meiciko"}
let obj3 = {name: "meiciko"}

let names = [obj1, obj2, obj3]
obj1 = null
obj2 = null
obj3 = null

const weakSet = new WeakSet(names)
names = null

console.log(weakSet) 
```

```js
/*
	输出：
  WeakSet {{…}, {…}, {…}}
  [[Entries]]
  [[Prototype]]: WeakSet
*/
```

来看看如果没有 `names = null` 这一步输出的结果：

```js
/*
	输出：
	WeakSet {{…}, {…}, {…}}
  [[Entries]]
  0: Object
  1: Object
  2: Object
  [[Prototype]]: WeakSet
*/
```

是的，因为弱引用的关系，导致了 `WeakSet` 对数组 `names` 的引用被忽略，此时的数组相当于没有了引用，所以被清除掉，导致没有拿到内容。



## WeakSet 有什么用？

这个问题不好回答，（很多人也觉得没什么用）



