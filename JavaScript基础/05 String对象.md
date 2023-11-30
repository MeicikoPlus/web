#  String对象

String 对象是JS三大包装对象之一，是字符串对应的包装对象，可以作为构造函数使用，也可以作为工具函数使用。

## 作为构造函数

作为构造函数时，它用于生成值为字符串的对象。

String 是对应字符串的引用类型。要创建一个 String 对象，使用 String 构造函数并传入一个数值，如下例所示：

```JS
var strObj = new String("hello world"); 
```

String 对象的方法可以在所有字符串原始值上调用。3个继承的方法 valueOf()、toLocaleString()和 toString()都返回对象的原始字符串值。

```js
console.log(strObj.valueOf()); // 'hello world'
console.log(strObj.toString());// 'hello world'
```

## 作为工具函数

作为工具函数时，它可以将任何类型的值转为字符串。

```js
String(true) // 'true'
String(12) // '12'
```

注意：不要将基本类型中的字符串 与 字符串对象 弄混了。

基本类型中的字符串（原始值） ：

```JS
var str = 'hello world';
var str = String('hello world');
console.log(typeof str); // string 
console.log(str instanceof String); // false 
```

引用类型中的字符串对象（引用值）：

```js
var strObj = new String("hello world"); 
console.log(typeof strObj); // object 
console.log(strObj instanceof String); // true 
```

通常我们使用字符串，直接使用String的原始值，而不是使用String对象。

# 字符串的长度

每个 字符串 都有一个 length 属性，表示字符串中字符的数量（字符串的长度）。

```JS
var length = str.length;// 11

// 空字符串的长度为0
```

# 字符串的索引

字符串中每个字符都有一个位置，以数字表示，称为索引。字符串的索引值从0开始，依次是：0 1 2 …

```JS
// 通过索引值获取字符串中的字符
var s1 = str[1]; // 'e'
var s1 = str[str.length - 1]; // 获取最后一个字符'd'

// 如果超出有效范围的索引值会得到undefined
var s1 = str[20]; // undefined
```

# 字符串的不可变

字符串的不可变指的是字符串的值是不可变的，这意味着一旦字符串被创建就不能被改变。

```JS
var str = 'abc'; // 在内存中开辟一块空间给abc 
str[1] = 'B';
console.log(str); // 'abc'

// 注意，这并不意味着 str 永远不能被改变，只是字符串字面量的各个字符不能被改变。
// 改变 str 中的唯一方法是重新给它赋一个值，就像这样：
str = 'ABC';
```

对str执行再次赋值的操作：`str = 'ABC';` ，则会在内存栈中开辟新的空间，变量str指向新的内存空间（存储 `'ABC'`的位置），原来指向的数据 `'abc'` 还在原来的内存中，不会被修改。所以不宜执行太多次字符串赋值操作，效率较低，内存占用较大，且浏览器可能崩溃（数据量很大时，若是number类型则不会崩溃）。

为字符串重复赋值、字符串的拼接等都会开辟新的地址空间来存放字符串的值，消耗内存的操作。


若想拼接大量字符串使用数组和其join方法可以进行优化。

# 字符串的方法

## JavaScript 字符

**str.charAt(index)** 

- 功能：从一个字符串中返回指定索引位置的字符。
- 参数：必须，为目标字符的下标位置(0~length-1)。如果没有提供索引，charAt() 将使用0。
- 返回值：查找到的字符，一个新的字符串。

> charAt方法不会修改原字符串。
>
> 若参数 index 不在 0 与 string.length-1 之间，该方法将返回一个空字符串。

**str.charCodeAt(index)**

- 功能：返回在指定的位置的字符对应的Unicode十进制编码。
- 参数：必须，为目标字符的下标位置。
- 返回值：一个数字，字符对应的Unicode十进制编码值。

> charCodeAt方法不会修改原字符串。
>
> 若参数 index 不在 0 与 string.length-1 之间，该方法将返回NaN。

**String.fromCharCode(num1, num2, num3, …) **

- 功能：把一个十进制的Unicode编码转化为对应的字符。
- 参数：必须，介于 `0` 到 `65535` 之间的数字，可以传递任意多个参数。
- 返回值：一个字符或多个字符组成的字符串。

> 若参数 index 不在 0 与 65535 之间，该方法将返回一个空字符串。

## 字符串位置方法

有两个方法用于在字符串中定位子字符串：indexOf()和 lastIndexOf()。

**str.indexOf(searchString, index)**

- 功能：检索字符串，返回指定子字符串在字符串中首次出现的位置。
- 参数1：必须，检索目标子字符串。
- 参数2：可选，在字符串中开始检索的位置，从这个参数指定的位置开始向字符串末尾搜索，忽略该位置之前的字符。其合法取值是 0 到 string.length - 1。如省略该参数，则将从字符串的首字符开始检索。
- 返回值：查找的字符串 `searchString` 的第一次出现的索引，如果没有找到，则返回 `-1`。

> indexOf方法不会修改原字符串。
>
> 注意：indexOf() 方法对大小写敏感！
>
> 注意：如果要检索的字符串值没有出现，则该方法返回 -1。

**str.lastIndexOf(searchString, index)**

- 功能：从后向前搜索字符串，返回指定子字符串在字符串中最后一次出现的位置。
- 参数1：必须，检索目标子字符串。
- 参数2：可选，在字符串中开始检索的位置，从这个参数指定的位置开始向字符串开头搜索，忽略该位置之后直到字符串末尾的字符。其合法取值是 0 到 string.length - 1。如省略该参数，则将从字符串的最后一个字符开始检索。
- 返回值：返回指定字符串 `searchString` 最后一次出现的索引(该索引仍是以从左至右0开始记数的)，如果没找到则返回-1。

> lastIndexOf方法不会修改原字符串。
>
> 注意：lastIndexOf() 方法对大小写敏感！
>
> 注意：如果要检索的字符串值没有出现，则该方法返回 -1。

indexOf()和 lastIndexOf() 这两个方法都是从字符串中搜索传入的字符串，并返回位置（如果没找到，则返回-1）。两者的区别在于，indexOf()方法从字符串开头开始查找子字符串，而 lastIndexOf()方法从字符串末尾开始查找子字符串。

使用indexOf()可以从来判断字符串中是否存在某个字符。

## 字符串包含方法

ECMAScript 6 增加了 3 个用于判断字符串中是否包含另一个字符串的方法：startsWith()、endsWith()和includes()。

**str.startsWith(searchString, index)**

- 功能：用来判断当前字符串是否以另外一个给定的子字符串开头，并根据判断结果返回 `true` 或 `false`。
- 参数1：必须，检索目标子字符串。
- 参数2：可选，在字符串中开始检索的位置，从这个参数指定的位置开始向字符串末尾搜索，忽略该位置之前的字符。其合法取值是 0 到 string.length - 1。如省略该参数，则将从字符串的首字符开始检索。
- 返回值：如果在字符串的开头找到了给定的字符则返回**`true`**；否则返回**`false`**。

> startsWith方法不会修改原字符串。
>
> 注意：startsWith() 方法对大小写敏感！

这个方法常用于确定一个字符串是否以另一个字符串开头。

**str.endsWith(searchString, length)**

- 功能：用来判断当前字符串是否是以另外一个给定的子字符串“结尾”的，根据判断结果返回 `true` 或 `false`。
- 参数1：必须，检索目标子字符串。
- 参数2：可选，表示应该当作字符串末尾的位置，如果不提供这个参数，那么默认就是字符串长度。如果提供这个参数，那么就好像字符串只有那么多字符一样。
- 返回值：如果传入的子字符串在搜索字符串的末尾则返回**`true`**；否则将返回 **`false`**。

> endsWith方法不会修改原字符串。
>
> 注意：endsWith() 方法对大小写敏感！

这个方法常用于确定一个字符串是否以另一个字符串结尾。

**str.includes(searchString, index)** 

- 功能：用于判断一个字符串是否包含在另一个字符串中，根据情况返回 true 或 false。
- 参数1：必须，检索目标子字符串。
- 参数2：可选，在字符串中开始检索的位置，从这个参数指定的位置开始向字符串末尾搜索，忽略该位置之前的字符。其合法取值是 0 到 string.length - 1。如省略该参数，则将从字符串的首字符开始检索。
- 返回值：如果当前字符串包含被搜寻的字符串，就返回 **`true`**；否则返回 **`false`**。

> includes方法不会修改原字符串。
>
> 注意：includes() 方法对大小写敏感！

这个方法常用于判断一个字符串是否包含另外一个字符串。

## 字符串截取方法

**str.slice(indexStart,  indexEnd)**

- 功能：截取字符串在开始位置（包含）与结束位置（不包含）之间的字符 。
- 参数1：必须，截取的起始位置，接受负值。
- 参数2：可选，截取的结束位置，接受负值。
- 返回值：截取部分，一个新的字符串。

> slice方法不会修改原字符串。
>
> 如果参数 indexStart与 indexEnd相等，那么该方法返回的一个空串。
>
> 如果省略 indexEnd，截取到字符串末尾。
>
> 如果任一参数大于 str.length，则被当作 str.length
>
> slice方法的两个参数接受负值，若为负数，则该参数规定的是从字符串的尾部开始算起的位置。也就是说，-1 指字符串的最后一个字符，-2 指倒数第二个字符，以此类推。
>

**str.substr(indexStart,  length)**

- 功能：截取一个字符串中从指定位置开始到指定字符数的字符。
- 参数1：indexStart，必须，截取的起始位置，接受负值。
- 参数2：length，可选，截取字符串的长度，若未指定，则默认截取到原字符串的末尾。
- 返回值：截取部分，一个新的字符串。

> substr方法不会修改原字符串。
>
> substr方法的第一个参数若为负数，则该参数规定的是从字符串的尾部开始算起的位置。也就是说，-1 指字符串的最后一个字符，-2 指倒数第二个字符，以此类推。
>
> 注意：ECMAscript 没有对该方法进行标准化，因此不建议使用它，未来将可能会被移除掉。如果可以的话，使用 `substring()` 替代它。
>

**str.substring(indexStart,  indexEnd)**

- 功能：截取字符串在开始位置（包含）与结束位置（不包含）之间的字符 。
- 参数1：必须，截取的起始位置，不接受负数（截取的时候包含开始位置的字符）。
- 参数2：可选，截取的结束位置，不接受负数，若未指定，则默认截取到原字符串的末尾（截取的时候不包含结束位置的字符）。
- 返回值：截取部分，一个新的字符串。

> substring方法不会修改原字符串。
>
> 如果参数 indexStart与 indexEnd相等，那么该方法返回的一个空串。
>
> 如果省略 indexEnd，截取到字符串末尾。
>
> 如果任一参数小于 0 或为 NaN，则被当作 0。
>
> 如果任一参数大于 str.length，则被当作 str.length
>
> 如果 indexStart比 indexEnd大，那么该方法在提取子串之前会先交换这两个参数。
>

与 slice() 方法不同的是，substring() 不接受负的参数。

**两者的相同点**：

- 如果indexStart等于indexEnd，返回空字符串
- 如果indexEnd参数省略，则取到字符串末
- 如果某个参数超过string的长度，这个参数会被替换为string的长度
- 都不会修改原字符串

**substirng()的特点**：

- 如果 indexStart > indexEnd，indexStart和indexEnd将被交换
- 如果参数是负数或者不是数字，将会被0替换

**silce()的特点**：

- 如果indexStart > indexEnd不会交换两者
- 如果indexStart小于0，则截取从字符串末尾往前数的第abs(indexStart)个的字符开始(包括该位置的字符)
- 如果indexEnd小于0，则截取在从字符串末尾往前数的第abs(indexEnd)个字符结束(不包含该位置字符)

## 字符串拼接方法

**str.concat(string1, string2, string3, …)**

- 功能：将一个或多个字符串与原字符串拼接形成一个新的字符串，并返回。
- 参数：需要拼接到 `str` 的字符串，可以传任意多个参数。
- 返回值：拼接后的一个新字符串。

> concat方法不会修改原字符串。
>
> 如果参数不是字符串类型，它们在连接之前将会被转换成字符串。

使用方式：强烈建议使用运算符（`+`, `+=`）代替 `concat` 方法。

虽然 concat()方法可以拼接字符串，但更常用的方式是使用加号操作符（+）。而且多数情况下，对于拼接多个字符串来说，使用加号更方便。

## 字符串模式匹配方法

**str.match(regexp|substr)**

- 功能：返回指定位置的字符。检索返回一个字符串匹配正则表达式的结果
- 参数：必须，规定要检索的字符串值或待匹配的 RegExp 对象。
- 返回值：存放匹配结果的数组。该数组的内容依赖于 regexp 是否具有全局标志 g。

> match方法不会修改原字符串。
>
> 如果 regexp 没有标志 g，那么 match() 方法就只能在 str中执行一次匹配。如果没有找到任何匹配的文本， match() 将返回 null。否则，它将返回一个数组，其中存放了与它找到的匹配文本有关的信息。该数组的第 0 个元素存放的是匹配文本，而其余的元素存放的是与正则表达式的子表达式匹配的文本。除了这些常规的数组元素之外，返回的数组还含有两个对象属性 index 和 input。index 属性声明的是匹配文本的起始字符在 str 中的位置，input 属性声明的是对 str 的引用。
>
> 如果 regexp 具有标志 g，则 match() 方法将执行全局检索，找到 str 中的所有匹配子字符串。若没有找到任何匹配的子串，则返回 null。如果找到了一个或多个匹配子串，则返回一个数组。不过全局匹配返回的数组的内容与前者大不相同，它的数组元素中存放的是 str 中所有的匹配子串，而且也没有 index 属性或 input 属性。
>
> match()方法本质上跟 RegExp 对象的 exec()方法相同。

```js
var s = 'hello21 world21';
console.log(s.match(/\d{2}/)); //[ '21', index: 5, input: 'hello21 world21' ]

var s = 'hello21 world21';
console.log(s.match(/\d{2}/g)); //[ '21', '21' ]
```

**str.search(regexp|substr)**

- 功能：用于检索字符串中指定的子字符串，或检索与正则表达式相匹配的子字符串。
- 参数：必须，规定要检索的字符串值或待匹配的 RegExp 对象。
- 返回值：如果匹配成功，则返回正则表达式在字符串中首次匹配项的索引；否则，返回 `-1`。

示例：

```javascript
var s = 'hello world hello';
console.log(s.search('hello')); //0
console.log(s.search(/hello/g)); //0
console.log(s.search(/hello2/)); //-1
```

> search方法不会修改原字符串。
>
> search()方法不执行全局匹配，它将忽略标志 g。也就是说，它只匹配一次。若没匹配到结果，则返回-1。

**str.replace(regexp|substr, newSubStr)**

- 功能：在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的字符串。
- 参数1：regexp/substr，必须，规定子字符串或要匹配的 RegExp 对象。
- 参数2：newSubStr，必须，用于替换掉第一个参数在原字符串中的匹配部分的字符串。
- 返回值：替换后的一个新字符串。

> replace方法返回一个新字符串，并不会修改原字符串。

示例：

```javascript
var s = 'hello world hello';
console.log(s.replace('hello','hi')); //hi world hello
console.log(s.replace(/hello/,'hi')); //hi world hello
console.log(s.replace(/hello/g,'hi')); //hi world hi
```

**str.replaceAll(regexp|substr, newSubStr)**

- 功能： 在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串，该函数会替换所有匹配到的子字符串。
- 参数1：regexp/substr，必须，规定子字符串或要匹配的 RegExp 对象。
- 参数2：newSubStr，必须，用于替换掉第一个参数在原字符串中的匹配部分的所有字符串。
- 返回值：替换后的一个新字符串。

示例：

```javascript
var s = 'hello world hello';
console.log(s.replace('hello','hi')); //hi world hi
console.log(s.replace(/hello/g,'hi')); //hi world hi
```

> replaceAll方法返回一个新字符串，并不会修改原字符串。
>
> 当使用一个 `regex`时，您必须设置全局（“ g”）标志，否则，它将引发 `TypeError`：“必须使用全局 RegExp 调用 replaceAll”。

## 字符串分隔方法

**str.split(separator,howmany)**

- 功能：用指定的分隔符把一个字符串分割成字符串数组，是 Array.join( ) 的逆操作。
- 参数1：separator，必须，字符串或正则表达式，从该参数指定的地方分割原字符串。
- 参数2：howmany，可选，指定返回数组的最大长度。
- 返回值：一个字符串数组。

> split方法返回一个数组，并不会修改原字符串。

## 字符串重复方法

**str.repeat(count)**

- 功能：将字符串复制多少次，然后返回拼接所有副本后的结果。
- 参数1：必须，介于 0 和 +Infinity 之间的整数。表示在新构造的字符串中重复了多少遍原字符串。
- 返回值：复制后的一个新字符串。

> repeat方法返回一个新字符串，并不会修改原字符串。

## 字符串大小写转换方法

**str.toLowerCase() **

- 功能：把字符串转换为小写。
- 返回值：一个新的字符串。

> toLowerCase方法返回一个新字符串，并不会修改原字符串。

**str.toUpperCase()**

- 功能：把字符串转换为大写。
- 返回值：一个新的字符串。

> toUpperCase方法返回一个新字符串，并不会修改原字符串。

## 字符串去空格方法

**str.trim()**

- 功能：用于删除字符串的头尾空白符，空白符包括：空格、制表符 tab、换行符等其他空白符等。
- 返回值：一个新的字符串。

> trim方法返回一个新字符串，并不会修改原字符串。

**str.trimStart()**

- 功能：从字符串的开头删除空格。trimLeft() 是此方法的别名。。
- 返回值：一个新的字符串。

> trimStart方法返回一个新字符串，并不会修改原字符串。

**str.trimEnd()**

- 功能：从一个字符串的末端移除空白字符。trimRight() 是这个方法的别名。
- 返回值：一个新的字符串。

> trimEnd方法返回一个新字符串，并不会修改原字符串。

# 练习

```JS
var str = "give me some sunshine give me some rain give me another changce i wanna grow up once again";
```

- 查询字符串长度
- 查找 me、some、w三个字符串是否存在
- 把me都换成ME
- 将字符串里的所有‘g’ 替换为 ‘G’。（2种写法replace、replaceall）
- 一个数组中存放着若干人名`['唐僧','王小二','猪八戒','孙悟空','王二狗','王小利']`，找出所有姓王的名字
- var str="JAVASCRIPT";把字符串转为小写，返回新的字符串。
- var str="javascript";把字符串转为大写，返回新的字符串。
- var str="javascript";返回字符串中提取的子字符串"cript"、"vascript"。（每个结果3中写法）
- var str="javascript";提取字符串中介于两个指定下标之间的字符"vasc"、"ip"、"ava"。（每个结果3种写法）
- var mystr1="Hello";var mystr2="world!";将一个或多个字符串拼接起来，返回拼接到的新的字符串，原字符串不变。（2种写法）
- 统计上述字符串str中字母a出现了多少次（多种写法）
- 统计上述字符串str中出现次数最多的字母是什么
- 去除字符串 `'  hh   l   e  '` 中所有的空格，至少两种写法（不使用replaceAll）
- 字符串 `'qwert'` 反转，至少两种写法
- 字符串 `'abbaacca'` 去重
- var str = "my-first-friend-lucky-man"将字符串变成驼峰法（每个单词首字母大写：My-First-Friend-Lucky-Man）。
- 自己使用函数封装所有的字符串方法，实现相应的功能。

