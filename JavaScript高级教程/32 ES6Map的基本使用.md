# Map 

在ES6中 `Map` 代表的是映射，用于存储映射关系（键值对）；

那么，作为同样存储字符串的 `Object` 对象，它们有什么区别呢？

- 对象的存储映射关系==只能用字符串作为属性名称==；
- 某些情况下我们希望==通过其他类型==作为 `key` ，比如对象，但 `Object` 这个时候会自动将对象转换为字符串作为键；

来看下面的案例：

```js
const info = {name: "meiciko"}
const obj = {
  [info]: "hellow world",
  address: "CN"
}
console.log(obj)
```

```js
{[object Object]: 'hellow world', address: 'CN'}
```

可以看到，对象类型作为键值，会转化为 `[object Object]` 字符串，难以使用。



## 创建MAP

创建 `Map` 的方法类似于 `set`

```js
const info = {name: "meiciko"}
const map = new Map()
map.set(info, "hellow") // 使用对象来设置key
```





## 常见属性和方法

### size属性

`size` 属性用来返回 `map` 元素的个数

```js
const info = {name: "meiciko"}
const map = new Map()
map.set(info, "hellow")
console.log(map.size) // 1
```



### 常见方法

` map` 的常用方法与 `set` 的类似。

| 方法              | 效果                                        |
| ----------------- | ------------------------------------------- |
| set(key, value)   | 在map中添加key，value，并返回整个map对象    |
| get(key)          | 根据key值，来获取map中的value               |
| has(key)          | 判断是否包含一个key，返回布尔类型           |
| delete(key)       | 根据key值来清空一个键值对，并且返回布尔类型 |
| clear()           | 清空所有的元素                              |
| forEach(callback) | 通过forEach来遍历Map                        |







------



# WeakMap

和  `map` 类型的另外一个数据结构称之为 `WeakMap` 也是键值对的形式存在的；

他和 `map` 的区别在于：

1. WeakMap 的 key 只能使用对象，不接受其他类型作为 key；
2. WeakMap 的 key 对对象的引用是弱引用，如果灭有其他引用引用这个对象，GC可以回收该对象；



## 注意事项

WeakMap不能够遍历，它既没有forEach方法，也不支持使用for of 方式进行遍历；
