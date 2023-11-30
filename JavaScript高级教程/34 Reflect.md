# Reflect

## Reflect 的作用

Reflect 也是 ES6 新增的一个 ==API== ，==它是一个对象（不能使用 new 创建）==，字面的意思是==反射==。

那么这个 Reflect 有什么用呢？

- 它主要==提供了很多操作 JavaScript 对象的方法==，有点像 Object.getPrototypeOf()；
- 比如比如Reflect.getPrototypeOf(target)类似于Object.getPrototypeOf()（获取对象的隐式原型）;
- 比如Reflect.defineProperty(target, propertyKey, attributes)类似于Object.defineProperty() ;



那么，明明 Object 本身就可以做这些操作，那么为什么还需要有 Reflect 这样的新增对象呢？

- 早期的 ECMA 规范中没有考虑到这种对对象本身的操作如何设计会更加规范，所以将这些 API 放在了Objec上；
- 但是 Object作为一个构造函数，这些操作实际上放在它身上并不合适；
- 另外还包含一些类似于 in，delete 操作符，让 js 看起来会有一些奇怪；

- 所以在 ES6 中新增了 Reflect，让我们这些操作都集中到了 Reflect 对象上；
- 另外在使用 Proxy 时，可以做到不操作原对象；



## Reflect 和 Object 的区别

详细可以参考MDN文档。

```js
const obj = {
  name: "why",
  age: 18
}

if (Reflect.deleteProperty(obj, "name")) {
  console.log("删除成功")
} else {
  console.log("没有删除成功")
}
```





## Reflect 的常见方法

Reflect 的常见方法和 Proxy 是一一对应的，也是13个。

1. `Reflect.apply(target, thisArg, argumentsList)`：调用目标函数，并指定this值和参数列表。
2. `Reflect.construct(target, argumentsList[, newTarget])`：使用给定的参数列表创建一个新的实例对象。
3. `Reflect.get(target, propertyKey[, receiver])`：获取目标对象上指定属性的值。
4. `Reflect.set(target, propertyKey, value[, receiver])`：设置目标对象上指定属性的值。
5. `Reflect.has(target, propertyKey)`：检查目标对象是否具有指定属性。
6. `Reflect.deleteProperty(target, propertyKey)`：删除目标对象上指定属性。
7. `Reflect.getOwnPropertyDescriptor(target, propertyKey)`：获取目标对象上指定属性的属性描述符。
8. `Reflect.defineProperty(target, propertyKey, attributes)`：定义或修改目标对象上指定属性的属性描述符。
9. `Reflect.getPrototypeOf(target)`：获取目标对象的原型。
10. `Reflect.setPrototypeOf(target, prototype)`：设置目标对象的原型。
11. `Reflect.isExtensible(target)`：确定目标对象是否可扩展。
12. `Reflect.preventExtensions(target)`：阻止目标对象被扩展。
13. `Reflect.ownKeys(target)`：返回目标对象自身的属性键的数组。



