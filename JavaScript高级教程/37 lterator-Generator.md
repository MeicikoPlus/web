# **迭代器以及可迭代对象**

## 什么是迭代器

==迭代器==：==使用户在容器对象（container，例如链表或者数组）上遍访的**对象**， 使用该接口无需关心对象内部实现细节==。

- 迭代器的行为==像数据库中的光标==，迭代器最早出现在1974年设计的CLU编程语言中；
- 在各种编程语言的实现中，迭代器的实现方式各不相同，但是基本都有迭代器，比如java，Python。

也就是说，==我们通过一个对象来对一个容器进行遍历，我们就称这个对象为迭代器==。

<img src="https://s2.loli.net/2023/08/21/zVkNZpHd9gYyKIf.png" alt="image.png" style="zoom: 67%;" />

从上述迭代器的定义我们可以看出来，==迭代器是帮助我们对某个数据结构进行遍历的对象==。

在JavaScript中，迭代器也是一个具体的对象，这个对象也需要符合迭代器协议！

- 迭代器协议定义了==产生一些列的值（无论是有限个还是无限个）的标准方式==；
- ==在JavaScript中这个标准就是一个**特定的next方法**==。

 

next 方法有如下的要求：

- 一个无参数或者一个参数的函数，返回一个应当拥有一下两个属性的对象：
- done：
  - 如果迭代器可以产生序列中的下一个值，则为false（等价于没有指定done这个属性）；
  - 如果迭代器应将序列迭代完毕，啧则true，这种情况下，value是可选的，如果它依旧存在，即为迭代结束之后的默认返回值；

- value：
  - 迭代器返回的任何JavaScript值，done为true时可以省略；



## 根据要求来描述一个迭代器

```js
// 迭代器要求，必须实现next方法
const names = ["meiciko", "kobe", "jams"]

let index = 0
// 给上述的数组创建一个迭代器
const nameIterator = {
  next: function(){
    // done: Boolean
    // value: 具体值/undefined
    if (index < names.length) {
      return {done: false, value: names[index++]}
    } else {
      return {done: true}
    }
  }
}

const res1 = nameIterator.next()
const res2 = nameIterator.next()
const res3 = nameIterator.next()
const res4 = nameIterator.next() // done: true, value: undefined
console.log(res1, res2, res3, res4)
```

上述其实就可以认为是一个迭代器，但是这个迭代器只是names这个数组的迭代器，不能迭代其他的容器。



## 二次优化

相比于上述代码，这次添加的是创建一函数用来生成一个容器对应的迭代器：

```js
const names = ["meiciko", "jams", "kobe"]
const nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

// 封装一个函数，返回一个数组的迭代器
function createArrayIterator(arr) {
  let index = 0
  return {
    next: function() {
      if (index < arr.length) {
        return {done: false, value: arr[index++]}
      } else {
        return {done: true}
      }
    }
  }
}

const namesIterator = createArrayIterator(names)
const name1 = namesIterator.next()
const name2 = namesIterator.next()
const name3 = namesIterator.next()
console.log(name1, name2, name3)

const numsIterator = createArrayIterator(nums)
const num1 = numsIterator.next()
const num2 = numsIterator.next()
const num3 = numsIterator.next()
const num4 = numsIterator.next()
const num5 = numsIterator.next()
console.log(num1, num2, num3, num4, num5)
```



