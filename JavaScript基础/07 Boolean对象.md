# Boolean对象

Boolean 对象是JS三大包装对象之一，是布尔对应的包装对象，可以作为构造函数使用，也可以作为工具函数使用。

## 作为构造函数

作为构造函数时，它用于生成值为 true 或 false 的对象。

Boolean 是对应布尔值的引用类型。要创建一个 Boolean 对象，就使用 Boolean 构造函数并传入true 或 false，如下例所示：

```JS
var booleanObj = new Boolean(true); 
```

Boolean 的实例会重写 valueOf()方法，返回一个原始值 true 或 false。toString()方法被调用时也会被覆盖，返回字符串"true"或"false"。

```JS
console.log(booleanObj.valueOf()); // true
console.log(booleanObj.toString());// 'true'
```

不过，Boolean 对象在 ECMAScript 中用得很少。不仅如此，它们还容易引起误会，尤其是在布尔表达式中使用 Boolean 对象时，比如：

```js
let falseObject = new Boolean(false); 
if(falseObject){
  // falseObject 作为判断条件为true
}
// 表达式是对 falseObject 对象而不是对它表示的值（false）求值
let result = falseObject && true; 
console.log(result); // true 

let falseValue = false; 
if(falseValue){
  // falseValue 作为判断条件为false
}
result = falseValue && true; 
console.log(result); // false
```

**所有对象在布尔表达式或者判断条件中都会自动转换为 true**。**所以，在使用条件判断的时候，永远都不要使用布尔对象作判断条件。**

## 作为工具函数

作为工具函数时，它可以将任何类型的值转为布尔值。

```JS
Boolean(0) // false
Boolean('') // false
Boolean(null) // false
Boolean(undefined) // false
Boolean(NaN) // false
Boolean(1) // true
Boolean('false') // true
Boolean([]) // true
Boolean({}) // true
Boolean(function () {}) // true
Boolean(/foo/) // true
```

上面代码中几种得到`true`的情况，都值得认真记住。

**注意**：不要将基本类型中的布尔值 `true` 和 `false` 与值为 `true` 和 `false` 的 `Boolean` 对象弄混了。

基本类型中的布尔值 `true` 和 `false` （原始值）：

```JS
var isOpen = true;
var isOpen = Boolean(true);
console.log(typeof isOpen); // boolean 
```

引用类型中值为 `true` 和 `false` 的 `Boolean` 对象（引用值）：

```js
var isOpenObj = new Boolean(true);
console.log(typeof isOpenObj); // object 
```

通常我们使用布尔值，直接使用Boolean的原始值，而不是使用Boolean对象。

# Boolean原始值和引用值

原始值和引用值（new Boolean() 对象）的几个区别：

- typeof 操作符对原始值返回"boolean"，但对引用值返回"object"。

- Boolean 对象是 Boolean 类型的实例，在使用instaceof 操作符时返回 true，但对原始值则返回 false。

```js
console.log(typeof falseObject); // object 
console.log(typeof falseValue); // boolean 

console.log(falseObject instanceof Boolean); // true 
console.log(falseValue instanceof Boolean); // false 
```

理解原始布尔值和 Boolean 对象之间的区别非常重要，强烈建议永远不要使用后者。

最后，对于一些特殊值，`Boolean`对象前面加不加`new`，会得到完全相反的结果，必须小心。

```js
if (Boolean(false)) {
  console.log('true');
} // 无输出

if (new Boolean(false)) {
  console.log('true');
} // true

if (Boolean(null)) {
  console.log('true');
} // 无输出

if (new Boolean(null)) {
  console.log('true');
} // true
```
