# Number对象

Number对象是JS三大包装对象之一，是数字对应的包装对象，可以作为构造函数使用，也可以作为工具函数使用。

## 作为构造函数

作为构造函数时，它用于生成值为数值的对象。

Number 是对应数字的引用类型。要创建一个 Number 对象，就使用 Number 构造函数并传入一个数值，如下例所示：

```JS
let numObj = new Number(10);
```

Number 类型重写了 `valueOf()`、`toLocaleString()` 和 `toString()`方法。`valueOf()`方法返回 Number 对象表示的原始数值，另外两个方法返回数值字符串。

```js
console.log(numObj.valueOf()); // 10
console.log(numObj.toString());// '10'
```

## 作为工具函数

作为工具函数时，它可以将任何类型的值转为数字。

```js
Number(true) // 1
Number('12') // 12
```

注意：不要将基本类型中的数字 与 数字对象 弄混了。

基本类型中的数字（原始值） ：

```JS
var num = 10;
var num = Number(10);
console.log(typeof num); // number 
console.log(num instanceof Number); // false 
```

引用类型中的数字对象（引用值）：

```js
let numObj = new Number(10);
console.log(typeof numObj); // object 
console.log(numObj instanceof Number); // true 
```

通常我们使用数字，直接使用Number的原始值，而不是使用Number对象。

# Number的实例方法

**num.toFixed(digits)** 

- 功能：返回包含指定小数点位数的数字字符串
- 参数：可选，小数点后数字的个数；介于 0 到 20 （包括）之间。如果忽略该参数，则默认为 0。
- 返回值：返回一个数字字符串

通常使用 toFixed 方法给数字保留几位小数，取的是四舍五入的值。

**num.toExponential(digits)**

- 功能：返回以科学记数法（也称为指数记数法）表示的数值字符串
- 参数：可选。一个整数，用来指定小数点后有几位数字；默认情况下用尽可能多的位数来显示数字。
- 返回值：返回一个数字字符串

# Number的静态属性

**最大数字**：

- `Number.MAX_VALUE` 属性表示在 JavaScript 里所能表示的最大数字。

- `MAX_VALUE` 属性值接近于 `1.79E+308`。大于 `MAX_VALUE` 的值代表 "`Infinity`"。

**最小正值数字**：

- `Number.MIN_VALUE` 属性表示在 JavaScript 中所能表示的最小的正值数字。

- `MIN_VALUE` 属性是 JavaScript 里最接近 0 的正值，而不是最小的负值。
- `MIN_VALUE` 的值约为 5e-324。小于 `MIN_VALUE` ("underflow values") 的值将会转换为 0。

**最小数字**：

- 最小数字用 `-Number.MAX_VALUE` 表示

**非数字** NaN：

`Number.NaN` 表示“非数字”（Not-A-Number）。和 `NaN` 相同。

判断一个值是否是`NaN`

- `NaN`如果通过 `==` 、 `!=` 、 `===` 、以及 `!==`与其他任何值比较都将不相等 -- 包括与其他 NAN值进行比较。必须使用 `Number.isNaN()` 或 `isNaN()` 函数。
- 在执行自比较之中：也只有NaN，比较之中不等于它自己。

# Number的静态方法

ES6 将全局方法`parseInt()`和`parseFloat()`，移植到`Number`对象上面，行为完全保持不变。

**Number.parseInt(string, radix )**

- 功能：解析一个字符串并返回指定基数的十进制整数。
- 参数1：必须，要被解析的值。如果参数不是一个字符串，则将其转换为字符串(使用  `ToString `抽象操作)。字符串开头的空白符将会被忽略。
- 参数2：可选，从 `2` 到 `36`，表示字符串的基数。例如指定 16 表示被解析值是十六进制数。请注意，10不是默认值！
- 返回值：从给定的字符串中解析出的一个整数。如果给定值不能被转换成数值，则会返回 NaN。

> 字符串最前面的空格会被忽略，从第一个非空格字符开始转换。
>
> 如果第一个字符不是数字字符、加号或减号，parseInt()立即返回 NaN。这意味着空字符串也会返回 NaN。
>
> 如果第一个字符是数字字符、加号或减号，则继续依次检测每个字符，直到字符串末尾，或碰到非数字字符。
>
> 该方法和全局的 `parseInt()` 函数具有一样的函数功能。
>
> ECMAScript 2015添加了这部分 (其目的是对全局变量进行模块化)。

**Number.parseFloat(string)** 

- 功能：把一个字符串解析成浮点数。
- 参数：必须，要解析的字符串。
- 返回值：给定值被解析成浮点数。如果给定值不能被转换成数值，则会返回 NaN。

> parseFloat()函数的工作方式跟 parseInt()函数类似，都是从位置 0 开始检测每个字符。
>
> parseFloat()也是解析到字符串末尾或者解析到一个无效的浮点数字字符为止。这意味着第一次出现的小数点是有效的，但第二次出现的小数点就无效了，此时字符串的剩余字符都会被忽略。因此，"22.34.5"将转换成 22.34。
>
> 该方法和全局的 `parseFloat()` 函数具有一样的函数功能。
>
> ECMAScript 2015添加了这部分 (其目的是对全局变量进行模块化)。

**Number.isNaN(value)**

- 功能：方法确定传递的值是否为 NaN，并且检查其类型是否为 Number。它是原来的全局 isNaN() 的更稳妥的版本。
- 参数：必须，要检测是否为 NaN 的值。
- 返回值：一个布尔值，表示给定的值是否是 NaN。

> 和全局函数 `isNaN()` 相比，`Number.isNaN()` 不会自行将参数转换成数字，只有在参数是值为 `NaN` 的数字时，才会返回 `true`。


isNaN会尝试将参数转化为数字类型：

- 能够转为数字的参数返回false
- 任何不能转换为数字的参数都会返回true

```JS
console.log(isNaN(NaN)); //true
console.log(Number.isNaN(0/0)); //true
console.log(isNaN('你好')); //true
console.log(isNaN(undefined)); //true

console.log(isNaN(1)); //false
console.log(isNaN(0)); //false
console.log(isNaN(true)); //false
console.log(isNaN(false)); //false
console.log(isNaN('1')); //false
console.log(isNaN('')); //false
console.log(isNaN(null)); //false
```

Number.isNaN() 该方法不会将参数转换成数字类型：

- 只有在参数是真正的数字类型，且值为 NaN 的时候才会返回 true

```JS
console.log(Number.isNaN(NaN)); //true
console.log(Number.isNaN(0/0)); //true

console.log(Number.isNaN('你好')); //false
console.log(Number.isNaN(undefined)); //false
console.log(Number.isNaN(1)); //false
console.log(Number.isNaN(0)); //false
console.log(Number.isNaN(true)); //false
console.log(Number.isNaN(false)); //false
console.log(Number.isNaN('1')); //false
console.log(Number.isNaN('')); //false
console.log(Number.isNaN(null)); //false
```

**Number.isInteger(value)**

- 功能：方法确定传递的值是否为整数。
- 参数：必须，要检测是否为 整数 的值。
- 返回值：一个布尔值，表示给定的值是否是整数。

> 如果被检测的值是整数，则返回 `true`，否则返回 `false`。注意 `NaN` 和正负 `Infinity`不是整数。

```javascript
Number.isInteger(25) // true
Number.isInteger(25.1) // false
```

JavaScript 内部，整数和浮点数采用的是同样的储存方法，所以 25 和 25.0 被视为同一个值。

```javascript
Number.isInteger(25) // true
Number.isInteger(25.0) // true
```

如果参数不是数值，`Number.isInteger`返回`false`。

```javascript
Number.isInteger() // false
Number.isInteger(null) // false
Number.isInteger('15') // false
Number.isInteger(true) // false
```
