# **Symbol**

Symbol是什么？==Symbol是ES6中新增的一个基本数据类型，翻译为符号==。

那么，为什么需要Symbol呢？

- 在ES6之前，对象的属性名都是字符串形式，那么很容易造成属性名的冲突；

- 比如原本有一个对象，我们希望在其中添加一个新的属性和值，但是我们不确定它在原来内部的环境有什么内容的前提下，很容易造成命名冲突从而覆盖掉原本的属性；

  

## Symbol基本使用过程

```js
const obj = {
  key: value,
  key: value,
  ...
}
```

在ES6之前，定义的对象中的 `key` 都是一个字符串，在加入一个新的属性的时候，有可能会造成属性名的冲突，但是有可能会修改原来的属性值，即对原来的属性值进行了覆盖，虽然在已经确定这个对象有什么东西的时候是可以避免这种情况的，但是也有不知道对象里面有什么的应用场景。

所以这时候就引入了 `Symbol` ，`Symbol` 值是通过Symbol函数来生成的，生成后可以作为属性名，也就是==在ES6中，对象的属性名可以使用字符串，也可以使用Symbol值==。

==Symbol 可以生成一个独一无二的值==，用来解决上述的问题！具体操作如下所示：

```js
const s1 = Symbol()
const obj = {
  [s1]: value, // 计算属性名的写法，详细可以参考19 ES6对象字面量增强
  key: value
}

const s2 = Symbol()
console.log(s1 === s2) // false
obj[s2] = 's2'
```

==Symbol 即使多次创建，它们也是不同的值==，Symbol函数执行后每次创建出来的值都是独一无二的。



## 在对象中获取Symbol

获取 Symbol 对应的 key 

```js
const s1 = Symbol()
var obj = {
  name: "meiciko",
  age: 18
  [si]: "hellow world"
}
console.log(Object.keys(obj)) // ['name', 'age']
```

通过上述代码，我们可以发现，通过 Object.keys() 是无法获取 Symbol 对应的key的，那么，如果想要获取 Symbol 对应 key ，需要使用下述方法：==object.getOwnPropertySymbols(obj)==

```js
const symbolKeys = object.getOwnPropertySymbols(obj)
for (const key of symbolkeys) {
  console.log(obj[key])
}
```





## Symbol  description

Symbol 函数在创建的会后可以传入一个描述 description 。

```js
const s1 = Symbol("ccc")
console.log(s1.description) // 可以通过这种方法拿到描述：ccc
```

如果是通过 Symbol 函数生成的 Symbol ，那么它都是独一无二的！但是有时候我们想要生成一个相同的 Symbol 可以使用 Symbol.for()

```js
const s1 = Symbol("meiciko")
const s2 = Symbol(s1.description)
console.log(s1 === s2) // false
const s3 = Symbol(s1.description)
console.log(s2 === s3) // true
```

如果相同的 key 可以通过相同的 Symbo.for() 来创建。

