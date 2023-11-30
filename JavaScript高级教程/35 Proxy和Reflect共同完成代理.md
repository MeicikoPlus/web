# Proxy 和 Reflect 共同完成代理

```js
const obj = {
  name: "meiciko",
  age: 18
}

const objProxy = new Proxy(obj, {
  set: function(target, key, newValue, receiver) {
    target[key] = newValue
  },
  get: function(target, key, recevier) {
  	...
	}
})
```

上述是 Proxy 单独完成的代理，从我们对代理对象的目的来看（我们不再直接操作原对象，通过代理对象去操作），但是上述代码中的第8行来看，虽然没有问题，但是会有违背之前所说的不再直接操作原对象这句话。

```js
// 我们可以去间接操作（借用Reflect）
const obj = {
  name: "meiciko",
  age: 18
}

const objProxy = new Proxy(obj, {
  set: function(target, key, newValue, receiver) {
    const isSuccess = Reflect.set(target, key, newValue) // 使用Reflect
    if (isSuccess) {
      throw new Error("无法修改这个属性！")
    }
  },
  get: function(target, key, recevier) {
  	...
	}
})
```



当然，除了不再直接操作原对象这一点外，还有第二个好处：

假设我们使用了属性描述符让这个对象对应的属性无法被删除，如果使用正常的第一种直接才做对象的写法的话，如果没有打开严格模式会出现一个静默错误，开启严格模式后会直接报错，但是采取第二种情况后，即使使用了对象的属性描述符，使得这个属性无法被修改，也会返回一个false，而不是出现错误的代码。



