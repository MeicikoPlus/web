# 创建数组

Array 构造函数还有两个 ES6 新增的用于创建数组的静态方法：from()和 of()。

- from()用于将类数组结构转换为数组实例
- of()用于将一组参数转换为数组实例

**Array.from(arrayLike, mapFn, thisArg)**

- 功能：对一个类似数组或可迭代对象创建一个新的，浅拷贝的数组实例。
- 参数1：必须，想要转换成数组的伪数组对象或可迭代对象。
- 参数2：可选，如果指定了该参数，新数组中的每个元素会执行该回调函数。
- 参数3：可选，可选参数，执行回调函数 `mapFn` 时 `this` 对象的值。
- 返回值：一个新的数组实例。

> `Array.from()` 可以通过以下方式来创建数组对象：
>
> - 伪数组对象（拥有一个 `length` 属性和若干索引属性的任意对象）
> - 可迭代对象（可以获取对象中的元素,如 Map和 Set 等）
>
> `Array.from()` 方法有一个可选参数 `mapFn`，让你可以在最后生成的数组上再执行一次 `map`方法后再返回。也就是说` Array.from(obj, mapFn, thisArg) `就相当于` Array.from(obj).map(mapFn, thisArg),` 除非创建的不是可用的中间数组。

```JS
// 字符串生成数组
Array.from('foo');// [ "f", "o", "o" ]

// Set 生成数组
const set = new Set(['foo', 'bar', 'baz', 'foo']);
Array.from(set);// [ "foo", "bar", "baz" ]

// 从 Map 生成数组
const map = new Map([[1, 2], [2, 4], [4, 8]]);
Array.from(map);// [[1, 2], [2, 4], [4, 8]]

const mapper = new Map([['1', 'a'], ['2', 'b']]);
Array.from(mapper.values());// ['a', 'b'];
Array.from(mapper.keys());// ['1', '2'];

// Array.from()对现有数组执行浅复制
const a1 = [1, 2, 3, 4]; 
const a2 = Array.from(a1);// [1, 2, 3, 4] 
alert(a1 === a2); // false
```

**Array.of(element0, element1, ..., elementN)**

- 功能：用于将一组参数，转换为数组。
- 参数：必须，任意个参数，将按顺序成为返回数组中的元素。
- 返回值：一个新的数组实例。

> 这个方法的主要目的，是弥补数组构造函数`Array()`的不足。因为参数个数的不同，会导致`Array()`的行为有差异。

```js
console.log(Array.of(10));// 创建元素值为 10 的数组
console.log(Array.of('10'));// 创建元素值为 '10' 的数组
console.log(Array.of(10,20,30));// 创建元素值为10 20 30 的数组

console.log(new Array(10));// 创建长度为10 的空数组
console.log(new Array(10,20,30));// 创建元素值为10 20 30 的数组
console.log(new Array('10'));// 创建元素值为 '10' 的数组
```

# 检测数组

判断一个变量是否是数组？

使用typeof显示数组类型为object类型，无法判断一个变量是否是数组，数组中提供了`Array.isArray`方法，用来判断一个变量是否是数组。

**Array.isArray(value)**

- 功能：用于确定传递的值是否是一个 Array。
- 参数：必须，需要检测的值。
- 返回值：如果value是 Array ，则返回true，否则为false。

```JS
var arr = [];

console.log(Array.isArray(arr)); // true
// 使用 instanceOf 也能够判断一个变量是否是数组
console.log(arr instanceOf Array); // true

// typeof {} 和 typeof [] 返回值都是object，其实typeof更常用于基本类型 number string boolean的数据类型判断，而 instanceof 则是用于引用类型 Object Array Function等的数据类型判断
console.log(typeof arr); // object
```

# 数组复制和填充方法

**arr.copyWithin(target, beginIndex, endIndex)**

- 功能：在当前数组内部，将指定位置的元素**浅复制**到其他位置（会覆盖原有元素），然后返回当前数组。会修改当前数组，但不会改变原数组的长度。
- 参数1：必须，从该位置开始替换数据。如果为负值，表示从末尾开始计算。
- 参数2：可选，开始复制元素的开始位置（包含），默认为 0。如果为负值，`beginIndex` 将从末尾开始计算。
- 参数3：可选，开始复制元素的结束位置（不包含），默认为 `arr.length`。如果是负数， `endIndex` 将从末尾开始计算。
- 返回值：改变后的数组。

> ==**copyWithin方法会改变原数组。**==
>
> 参数 target、beginIndex 和 endIndex 必须为整数。
>
> 如果 beginIndex 为负，则其指定的索引位置等同于 length+beginIndex，length 为数组的长度。endIndex 也是如此。
>
> 如果 `target` 大于等于 `arr.length`，将会不发生拷贝。如果 `target` 在 `beginIndex` 之后，复制的序列将被修改以符合 `arr.length`。
>
> 如果 `beginIndex` 被忽略，`copyWithin` 将会从0开始复制。
>
> 如果 `endIndex` 被忽略，`copyWithin` 方法将会一直复制至数组结尾（默认为 `arr.length`）。

```js
// 将3号位复制到0号位置
[1, 2, 3, 4, 5].copyWithin(0, 3, 4) // [4, 2, 3, 4, 5]

// 将 4 5 复制到0 和 1 号位置
[1, 2, 3, 4, 5].copyWithin(0, 3) // [4, 5, 3, 4, 5]

// -2相当于3号位置，-1相当于4号位置
[1, 2, 3, 4, 5].copyWithin(0, -2, -1) // [4, 2, 3, 4, 5]
```

**arr.fill(value, beginIndex, endIndex)**

- 功能：用一个固定值填充一个数组中从开始索引到终止索引内的全部元素。不包括终止索引。
- 参数1：必须，用来填充数组元素的值。
- 参数2：可选，填充的开始位置（包含），默认为 0。如果为负值，`beginIndex` 将从末尾开始计算。
- 参数3：可选，填充的结束位置（不包含），默认为 `arr.length`。如果是负数， `endIndex` 将从末尾开始计算。
- 返回值：修改后的数组。

> ==**fill方法会改变原数组。**==
>
> 如果 `beginIndex` 是个负数, 则开始索引会被自动计算成为 `length+beginIndex`, 其中 `length` 是 `this` 对象的 `length `属性值。如果 `endIndex` 是个负数, 则结束索引会被自动计算成为 `length+endIndex`。

```js
[1, 2, 3].fill(4);               // [4, 4, 4]
[1, 2, 3].fill(4, 1);            // [1, 4, 4]
[1, 2, 3].fill(4, 1, 2);         // [1, 4, 3]
[1, 2, 3].fill(4, 1, 1);         // [1, 2, 3]
[1, 2, 3].fill(4, 3, 3);         // [1, 2, 3]
```

# 数组转换方法

所有对象都有 toString()和 valueOf()方法。其中，valueOf()返回的还是数组本身。而 toString()返回由数组中每个值的等效字符串拼接而成的一个逗号分隔的字符串。也就是说，对数组的每个值都会调用其 toString()方法，以得到最终的字符串。

```js
var arr = [0,1,2,3,4,5,6,7,8,9];
console.log(arr.valueOf());// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
console.log(arr.toString());// 0,1,2,3,4,5,6,7,8,9
```

**arr.join(separator)**

- 功能：所有的数组元素被转换成字符串，再用一个分隔符将这些字符串连接起来。
- 参数：可选，指定一个字符串来分隔数组的每个元素。如果需要，将分隔符转换为字符串。如果缺省该值，数组元素用逗号（`,`）分隔。如果`separator`是空字符串(`""`)，则所有元素之间都没有任何字符。
- 返回值：一个所有数组元素连接的字符串。如果 `arr.length` 为0，则返回空字符串。

> join方法返回一个字符串，不会改变原数组。
>
> 如果数组中某一元素是 null 或 undefined，则在 join()、toLocaleString()、toString()和 valueOf()返回的结果中会被转换为空字符串。

# 数组栈方法

ECMAScript 给数组提供几个方法，让它看起来像是另外一种数据结构。数组对象可以像栈一样，也就是一种限制插入和删除项的数据结构。栈是一种后进先出（LIFO，Last-In-First-Out）的结构，也就是最近添加的项先被删除。数据项的插入（称为推入，push）和删除（称为弹出，pop）只在栈的一个地方发生，即栈顶。ECMAScript 数组提供了 push()和 pop()方法，以实现类似栈的行为。

**arr.push(element1, ..., elementN)**

- 功能：将一个或多个元素添加到数组的末尾，并返回该数组的新长度。
- 参数：被添加到数组末尾的元素或多个元素。
- 返回值：返回数组 length 属性值。

> ==push方法返回数组的长度值，**会改变原数组**。==

**arr.pop()**

- 功能：从数组中删除最后一个元素，并返回该元素的值。此方法更改数组的长度。
- 返回值：从数组中删除的元素(当数组为空时返回undefined)。

> ==pop方法返回数组中被删除的元素，**会改变原数组**。==

# 数组队列方法

就像栈是以 LIFO 形式限制访问的数据结构一样，队列以先进先出（FIFO，First-In-First-Out）形式限制访问。队列在列表末尾添加数据，但从列表开头获取数据。因为有了在数据末尾添加数据的 push()方法，所以要模拟队列就差一个从数组开头取得数据的方法了。这个数组方法叫 shift()，它会删除数组的第一项并返回它，然后数组长度减 1。

ECMAScript 也为数组提供了 unshift()方法。顾名思义，unshift()就是执行跟 shift()相反的操作：在数组开头添加任意多个值，然后返回新的数组长度。通过使用 unshift()和 pop()，可以在相反方向上模拟队列，即在数组开头添加新数据，在数组末尾取得数据。

**arr.shift()**

- 功能：从数组中删除**第一个**元素，并返回该元素的值。此方法更改数组的长度。
- 返回值：从数组中删除的元素; 如果数组为空则返回undefined 。 

> ==shift方法返回数组中被删除的元素，**会改变原数组**。==
>
> shift 方法移除索引为 0 的元素(即第一个元素)，并返回被移除的元素，其他元素的索引值随之减 1。如果 length 属性的值为 0 (长度为 0)，则返回 undefined。

**arr.unshift(element1, ..., elementN)**

- 功能：将一个或多个元素添加到数组的开头，并返回该数组的新长度。
- 参数：要添加到数组开头的元素或多个元素。
- 返回值：返回数组 length 属性值。

> ==unshift方法返回数组的长度值，**会改变原数组**。==

注意： 如果传入多个参数，它们会被以块的形式插入到对象的开始位置，它们的顺序和被作为参数传入时的顺序一致。 于是，传入多个参数调用一次 `unshift` ，和传入一个参数调用多次 `unshift` (例如，循环调用)，它们将得到不同的结果。例如:

```JS
var arr = [4,5,6];
arr.unshift(1,2,3);
console.log(arr); // [1, 2, 3, 4, 5, 6]

var arr = [4,5,6]; 
arr.unshift(1);
arr.unshift(2);
arr.unshift(3);
console.log(arr); // [3, 2, 1, 4, 5, 6]
```

# 数组添加/删除/替换方法

**arr.splice(beginIndex, deleteCount, item1, item2, ...)**

- 功能：通过删除或替换现有元素或者原地添加新的元素来修改数组，并以数组形式返回被修改的内容。	
- 参数1：必须，指定修改的开始位置。
- 参数2：可选，整数，表示要移除的数组元素的个数。
- 参数3：可选，要添加进数组的元素，从`beginIndex` 位置开始。如果不指定，则 `splice()` 将只删除数组元素。
- 返回值：由被删除的元素组成的一个数组。如果只删除了一个元素，则返回只包含一个元素的数组。如果没有删除元素，则返回空数组。

> ==**splice方法会改变原数组。**==如果添加进数组的元素个数不等于被删除的元素个数，数组的长度会发生相应的改变。
>
> 如果beginIndex超出了数组的长度，则从数组末尾开始添加内容；如果是负值，则表示从数组末位开始的第几位（从-1计数，这意味着-n是倒数第n个元素并且等价于`array.length-n`）；如果负数的绝对值大于数组的长度，则表示开始位置为第0位。
>
> deleteCount：
>
> - 如果 `deleteCount` 大于 `beginIndex` 之后的元素的总数，则从 `beginIndex` 后面的元素都将被删除（含第 `beginIndex` 位）。
>
> - 如果 `deleteCount` 被省略了，或者它的值大于等于`array.length - beginIndex`(也就是说，如果它大于或者等于`beginIndex`之后的所有元素的数量)，那么`beginIndex`之后数组的所有元素都会被删除。
>
> - 如果 `deleteCount` 是 0 或者负数，则不移除元素。这种情况下，至少应添加一个新元素。

最强大的数组方法就属 splice()，使用它的方式可以有很多种。splice()的主要目的是在数组中间插入元素，但有 3 种不同的方式使用这个方法。

- 删除。需要给 splice()传 2 个参数：要删除的第一个元素的位置和要删除的元素数量。可以从数组中删除任意多个元素，比如 splice(0, 2)会删除前两个元素。
- 插入。需要给 splice()传 3 个参数：开始位置、0（要删除的元素数量）和要插入的元素，可以在数组中指定的位置插入元素。第三个参数之后还可以传第四个、第五个参数，乃至任意多个要插入的元素。比如，splice(2, 0, "red", "green")会从数组位置 2 开始插入字符串"red"和"green"。
- 替换。splice()在删除元素的同时可以在指定位置插入新元素，同样要传入 3 个参数：开始位置、要删除元素的数量和要插入的任意多个元素。要插入的元素数量不一定跟删除的元素数量一致。比如，splice(2, 1, "red", "green")会在位置 2 删除一个元素，然后从该位置开始向数组中插入"red"和"green"。

# 数组搜索和位置方法

ECMAScript 提供两类搜索数组的方法：按严格相等搜索和按断言函数搜索。

## 严格相等

ECMAScript 提供了 3 个严格相等的搜索方法：indexOf()、lastIndexOf()和 includes()。其中，前两个方法在所有版本中都可用，而第三个方法是 ECMAScript 7 新增的。这些方法都接收两个参数：要查找的元素和一个可选的开始搜索位置。indexOf()和 includes()方法从数组前头（第一项）开始向后搜索，而 lastIndexOf()从数组末尾（最后一项）开始向前搜索。

indexOf()和 lastIndexOf()都返回要查找的元素在数组中的位置，如果没找到则返回-1。includes()返回布尔值，表示是否至少找到一个与指定元素匹配的项。在比较第一个参数跟数组每一项时，会使用全等（===）比较，也就是说两项必须严格相等。

**arr.indexOf(searchElement, startIndex)**

- 功能：返回在数组中可以找到一个给定元素的第一个索引，如果不存在，则返回-1。
- 参数1：必须，要查找的元素。
- 参数2：可选，开始查找的位置。
- 返回值：首个被找到的元素在数组中的索引位置；若没有找到则返回 -1。

**indexOf常用于判断数组中是否存在某个元素或者查找该元素的索引值。**

> indexOf方法不会改变原数组。
>
> indexOf使用严格相等（strict equality，即 ===）比较 searchElement 和数组中的元素。
>
> startIndex：如果该索引值大于或等于数组长度，意味着不会在数组里查找，返回-1。如果参数中提供的索引值是一个负值，则将其作为数组末尾的一个抵消，即-1表示从最后一个元素开始查找，-2表示从倒数第二个元素开始查找 ，以此类推。 
>
> 注意：如果参数中提供的索引值是一个负值，并不改变其查找顺序，查找顺序仍然是从前向后查询数组。如果抵消后的索引值仍小于0，则整个数组都将会被查询。其默认值为0.

**arr.lastIndexOf(searchElement, startIndex)**

- 功能： 返回指定元素在数组中的最后一个的索引，如果不存在则返回 -1。从数组的后面向前查找，从 `fromIndex` 处开始。
- 参数1：必须，要查找的元素。
- 参数2：可选，从此位置开始逆向查找。默认为数组的长度减 1(`arr.length - 1`)，即整个数组都被查找。
- 返回值：数组中该元素最后一次出现的索引，如未找到返回-1。

> lastIndexOf方法不会改变原数组。
>
> lastIndexOf 使用严格相等（strict equality，即 ===）比较 searchElement 和数组中的元素。
>
> startIndex：如果该值大于或等于数组的长度，则整个数组会被查找。如果为负值，将其视为从数组末尾向前的偏移。即使该值为负，数组仍然会被从后向前查找。如果该值为负时，其绝对值大于数组长度，则方法返回 -1，即数组不会被查找。

**arr.includes(searchElement, startIndex)**

- 功能：用来判断一个数组是否包含一个指定的值，根据情况，如果包含则返回 `true`，否则返回 `false`。
- 参数1：必须，要查找的元素。
- 参数2：可选，开始查找的位置。默认为 0。如果为负值，则按升序从 `array.length + startIndex` 的索引开始搜 （即使从末尾开始往前跳 `startIndex` 的绝对值个索引，然后往后搜寻）。
- 返回值：如果在数组中找到了 `searchElement`，则返回 `true`，否则返回 `false`。

**includes常用于判断数组中是否存在某个元素。**

> includes方法不会改变原数组。
>
> 使用 `includes()`比较字符串和字符时是区分大小写的。
>
> 如果 `startIndex` 大于等于数组的长度，则将直接返回 `false`，且不搜索该数组。

##  断言函数

ECMAScript 也允许按照定义的断言函数搜索数组，每个索引都会调用这个函数。断言函数的返回值决定了相应索引的元素是否被认为匹配。

断言函数接收 3 个参数：元素、索引和数组本身。其中元素是数组中当前搜索的元素，索引是当前元素的索引，而数组就是正在搜索的数组。断言函数返回真值，表示是否匹配。

find()和 findIndex()方法使用了断言函数。这两个方法都从数组的最小索引开始。find()返回第一个匹配的元素，findIndex()返回第一个匹配元素的索引。这两个方法也都接收第二个可选的参数，用于指定断言函数内部 this 的值。

**arr.find(callback, thisArg)**

- 功能： 返回数组中满足提供的测试函数的第一个元素的值。否则返回 undefined。
- 参数1：必须，数组中的每个元素，都会执行该回调函数，执行时会自动传入下面三个参数：arr.find(function(element, index, array){});
  - element 必须， 当前遍历到的元素。
  - index 可选，当前遍历到的元素索引。
  - array 可选，调用find的数组。
- 参数2：可选，执行回调函数callback时作为this 对象的值。
- 返回值：数组中第一个满足所提供测试函数的元素的值，否则返回 undefined。

**find方法常用于从数组中查找一个符合条件的元素，如果找不到返回undefined。**

> find方法不会改变原数组。
>
> `find`方法对数组中的每一项元素执行一次 `callback` 函数，直至有一个 callback 返回 `true`。当找到了这样一个元素后，该方法会立即返回这个元素的值，否则返回 `undefined`。注意 `callback `函数会为数组中的每个索引调用即从 `0 `到 `length - 1`，而不仅仅是那些被赋值的索引，这意味着对于稀疏数组来说，该方法的效率要低于那些只遍历有值的索引的方法。
>
> 如果提供了 `thisArg`参数，那么它将作为每次 `callback`函数执行时的`this` ，如果未提供，则使用 `undefined`。
>
> 在第一次调用 `callback`函数时会确定元素的索引范围，因此在 `find`方法开始执行之后添加到数组的新元素将不会被 `callback`函数访问到。如果数组中一个尚未被`callback`函数访问到的元素的值被`callback`函数所改变，那么当`callback`函数访问到它时，它的值是将是根据它在数组中的索引所访问到的当前值。被删除的元素仍旧会被访问到，但是其值已经是undefined了。

**arr.findIndex(callback, thisArg)**

- 功能： 返回数组中满足提供的测试函数的第一个元素的**索引**。若没有找到对应元素则返回-1。
- 参数1：必须，数组中的每个元素，都会执行该回调函数，执行时会自动传入下面三个参数：arr.find(function(element, index, array){});
  - element 必须， 当前遍历到的元素。
  - index 可选，当前遍历到的元素索引。
  - array 可选，调用findIndex的数组。
- 参数2：可选，执行回调函数callback时作为this 对象的值。
- 返回值： 数组中通过提供测试函数的第一个元素的**索引**。否则，返回-1。

**findIndex方法与indexof有点像，不过findIndex方法可以自己定义判断条件，更加灵活。**

> find方法不会改变原数组。
>
> `findIndex`方法对数组中的每个数组索引`0..length-1`（包括）执行一次`callback`函数，直到找到一个`callback`函数返回真实值（强制为`true`）的值。如果找到这样的元素，`findIndex`会立即返回该元素的索引。如果回调从不返回真值，或者数组的`length`为0，则`findIndex`返回-1。 与某些其他数组方法（如arr.some）不同，在稀疏数组中，即使对于数组中不存在的条目的索引也会调用回调函数。
>
> 如果一个 `thisArg` 参数被提供给 `findIndex`, 它将会被当作`this`使用在每次回调函数被调用的时候。如果没有被提供，将会使用`undefined`。
>
> 在第一次调用`callback`函数时会确定元素的索引范围，因此在`findIndex`方法开始执行之后添加到数组的新元素将不会被`callback`函数访问到。如果数组中一个尚未被`callback`函数访问到的元素的值被`callback`函数所改变，那么当`callback`函数访问到它时，它的值是将是根据它在数组中的索引所访问到的当前值。被删除的元素仍然会被访问到。

# 数组截取方法

**arr.slice(beginIndex, endIndex)**

- 功能：截取数组在开始位置（包含）与结束位置（不包含）之间的元素，返回一个新的数组对象，原数组不会被改变。	
- 参数1：可选，截取的开始位置。
- 参数2：可选，截取的结束位置。
- 返回值：截取部分，一个新的数组。

> slice方法不会更改现有数组，而是返回一个新数组。
>
> beginIndex：
>
> - 如果该参数为负数，则表示从原数组中的倒数第几个元素开始提取，`slice(-2)` 表示提取原数组中的倒数第二个元素到最后一个元素（包含最后一个元素）。
>
> - 如果省略 `beginIndex`，则 `slice` 从索引 `0` 开始。
>
> - 如果 `beginIndex` 超出原数组的索引范围，则会返回空数组。
>
> endIndex：
>
> - 如果该参数为负数， 则它表示在原数组中的倒数第几个元素结束抽取。 `slice(-2,-1)` 表示抽取了原数组中的倒数第二个元素到最后一个元素（不包含最后一个元素，也就是只有倒数第二个元素）。
>
> - 如果 `end` 被省略，则 `slice` 会一直提取到原数组末尾。
>
> - 如果 `end` 大于数组的长度，`slice` 也会一直提取到原数组末尾。
>
> `slice` 不会修改原数组，只会返回一个浅复制了原数组中的元素的一个新数组。原数组的元素会按照下述规则拷贝：
>
> - 如果该元素是个对象引用 （不是实际的对象），`slice` 会拷贝这个对象引用到新的数组里。两个对象引用都引用了同一个对象。如果被引用的对象发生改变，则新的和原来的数组中的这个元素也会发生改变。
>
> - 对于字符串、数字及布尔值来说（不是 `String`、`Number`或者 `Boolean`对象），`slice` 会拷贝这些值到新的数组里。在别的数组里修改这些字符串或数字或是布尔值，将不会影响另一个数组。
>
> 如果向两个数组任一中添加了新元素，则另一个不会受到影响。

# 数组合并方法

**arr.concat(value1, value2, ..., valueN)**

- 功能： 用于合并两个或多个数组。
- 参数：可选，数组和/或值，将被合并到一个新的数组中。如果省略了所有 `valueN` 参数，则 `concat` 会返回调用此方法的现存数组的一个浅拷贝。
- 返回值：返回一个数组。

> lastIndexOf方法不会更改现有数组，而是返回一个新数组。ZZ
>
> `concat`方法创建一个新的数组，它由被调用的对象中的元素组成，每个参数的顺序依次是该参数的元素（如果参数是数组）或参数本身（如果参数不是数组）。它不会递归到嵌套数组参数中。
>
> `concat`方法不会改变`this`或任何作为参数提供的数组，而是返回一个浅拷贝，它包含与原始数组相合并的相同元素的副本。 原始数组的元素将复制到新数组中，如下所示：
>
> - 对象引用（而不是实际对象）：`concat`将对象引用复制到新数组中。 原始数组和新数组都引用相同的对象。 也就是说，如果引用的对象被修改，则更改对于新数组和原始数组都是可见的。 这包括也是数组的数组参数的元素。
>
> - 基本数据类型如字符串，数字和布尔（不是`String`，`Number`和 `Boolean`对象）：`concat`将字符串和数字的值复制到新数组中。
>
> **注意：**数组/值在连接时保持不变。此外，对于新数组的任何操作（仅当元素不是对象引用时）都不会对原始数组产生影响，反之亦然。

# 数组排序方法

数组有两个方法可以用来对元素重新排序：reverse()和 sort()。reverse仅仅是颠倒元素的位置，sort可以对元素从小到大排序或者定制排列顺序。

**arr.reverse()**

- 功能： 将数组中元素的位置颠倒，并返回该数组。数组的第一个元素会变成最后一个，数组的最后一个元素变成第一个。
- 返回值：颠倒后的数组。

> ==reverse方法会改变原数组。==

**arr.sort(compareFunction)**

- 功能：对数组的元素进行排序，并返回数组。默认排序顺序是在将元素转换为字符串，然后比较字符串来决定顺序。
- 参数：可选，用来指定按某种顺序进行排列的函数。如果省略，元素按照转换为的字符串的各个字符的Unicode位点进行排序。`arr.sort(function(firstEl, secondEl){});`
  - firstEl 第一个用于比较的元素
  - secondEl 第二个用于比较的元素。
- 返回值：排序后的数组。

> ==sort方法会改变原数组。==
>
> 如果没有指明 `compareFunction` ，那么元素会按照转换为的字符串的诸个字符的Unicode位点进行排序。例如 "Banana" 会被排列到 "cherry" 之前。当数字按由小到大排序时，9 出现在 80 之前，但因为（没有指明 `compareFunction`），比较的数字会先被转换为字符串，所以在Unicode顺序上 "80" 要比 "9" 要靠前。

## sort不带比较函数

sort不带比较函数的时候，是将每个元素转换为字符串，然后比较字符串从小到大来决定顺序。

例如 "Banana" 会被排列到 "cherry" 之前。当数字按由小到大排序时，6 出现在 45 之前，比较的数字会先把数字转换为字符串，在Unicode顺序上 "45" 要比 "6" 要靠前。

```js
arr = [12, 6, 22, 25, 45];
arr.sort();//把每个元素转为字符串，然后按照Unicode编码从小到大进行排序
console.log(arr);//[12, 22, 25, 45, 6]
```

## sort带比较函数

如果要得到自己想要的结果，不管是升序还是降序，就需要提供比较函数了：

- `arr.sort(function (x,y) {  })` x和y表示数组中两个相邻的元素。

该函数比较两个值的大小，然后返回一个用于说明这两个值的相对顺序的数字。比较函数两个参数 x 和 y，其返回值如下：

- 若 x 小于 y，即 x - y 小于零，则返回一个小于零的值，不交换元素的位置。
- 若 x 大于 y，即 x - y 大于零，则返回一个大于零的值，交换元素的位置。
- 若 x 等于 y，即 x - y 等于零，则返回 0，不交换元素的位置。

排序规则：return大于0的值，则交换数组相邻2个元素的位置。

```js
var arr = [2, 7, 5, 4];
// 从小到大排序
arr.sort(function(x, y) {
  if (x > y) {
    return 1; //交换位置
  } else if (x < y) {
    return -1; //不交换
  } else {
    return 0; //不交换
  }
});

// 升序
arr.sort(function(x,y){
  return x - y; //若return返回值大于0(即x＞y),则x,y交换位置
});

// 降序
arr.sort((x, y) => {
  return y - x; //若return返回值大于0(即y＞x),则x,y交换位置
});

// 对象的排序
// 年龄从小到大排序
var items = [
  { name: 'Tom', age: 22 },
  { name: 'Lily', age: 24 },
  { name: 'Lucy', age: 20 },
];
items.sort(function (x, y) {
  return x.age - y.age;
});
```

思考：在线的都在前面，不在线的都在后面。

```js
var friends = [
  { name: 'Tom', age: 22, online: true },
  { name: 'Lily', age: 21, online: false },
  { name: 'Lucy', age: 20, online: true },
  { name: '李明', age: 20, online: false },
  { name: '花花', age: 20, online: true },
];
```

## 冒泡排序

冒泡排序（Bubble Sort）是一种简单的排序算法。它重复地走访过要排序的数列，一次比较两个元素，如果它们的顺序错误就把它们交换过来。走访数列的工作是重复地进行直到没有再需要交换，也就是说该数列已经排序完成。这个算法的名字由来是因为越小的元素会经由交换慢慢“浮”到数列的顶端。 

算法原理：

- （1）比较相邻的元素。如果第一个比第二个大，就交换它们两个；
- （2）对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对，这样在最后的元素应该会是最大的数；
- （3）针对所有的元素重复以上的步骤，除了最后一个；
- （4）重复步骤1~3，直到排序完成。

![img](https://gitee.com/Jinxizhen/pic_resource/raw/master/images/849589-20171015223238449-2146169197.gif)

```js
function bubbleSort(arr) {
  var length = arr.length;
  for (var i = 0; i < length - 1; i++) {
    for (var j = 0; j < length - 1 - i; j++) {
      if (arr[j] > arr[j+1]) {  // 相邻元素两两对比
        var temp = arr[j+1];    // 元素交换
        arr[j+1] = arr[j];
        arr[j] = temp;
      }
    }
  }
  return arr;
}
```

# 数组迭代方法

ECMAScript 为数组定义了 5 个迭代方法。每个方法接收两个参数：参数1是数组每一项都运行的函数，参数2是可选的作为函数运行上下文的作用域对象（影响函数中 this 的值）。传给每个方法的函数接收 3个参数：数组元素、元素索引和数组本身。因具体方法而异，这个函数的执行结果可能会也可能不会影响方法的返回值。数组的 5 个迭代方法如下。

- forEach()：对数组每一项都运行传入的函数，没有返回值。
- every()：对数组每一项都运行传入的函数，如果对每一项函数都返回 true，则这个方法返回 true。 
- some()：对数组每一项都运行传入的函数，如果有一项函数返回 true，则这个方法返回 true。
- filter()：对数组每一项都运行传入的函数，函数返回 true 的项会组成数组之后返回。
- map()：对数组每一项都运行传入的函数，返回由每次函数调用的结果构成的数组。

这些方法都不改变调用它们的数组。

**arr.forEach(callback, thisArg)**

- 功能：对数组的每个元素执行一次给定的函数。
- 参数1：必须，数组中的每个元素都会执行该回调函数，执行时会自动传入下面三个参数：`arr.forEach(function(element, index, array){   });`
  - element 必须， 当前遍历到的元素。
  - index 可选，当前遍历到的元素索引。
  - array 可选，调用forEach的数组。
- 参数2：可选，执行回调函数callback时作为this 对象的值。
- 返回值：数组中第一个满足所提供测试函数的元素的值，否则返回 undefined。

> `forEach()` 方法按升序为数组中含有效值的每一项执行一次 `callback` 函数，那些已删除或者未初始化的项将被跳过（例如在稀疏数组上）。
>
> 如果 `thisArg` 参数有值，则每次 `callback` 函数被调用时，`this` 都会指向 `thisArg` 参数。如果省略了 `thisArg` 参数，或者其值为 `null` 或 `undefined`，`this` 则指向全局对象。
>
> **注意：** 除了抛出异常以外，没有办法中止或跳出 `forEach()` 循环。如果你需要中止或跳出循`forEach()` 方法不是应当使用的工具。若你需要提前终止循环，你可以使用：
>
> - 一个简单的 for 循环
> - for...of / for...in 循环
>
> for和forEach有何区别？
>
> - forEach不能使用break、continue, 能使用return。原因：回调函数中可以return。
>
> - for能使用break、continue，但不能使用return，因为for是语句，不是函数，return使用到函数内。

**arr.every(callback, thisArg)**

- 功能：测试数组内的每一项元素是否都能通过某个指定函数的测试。
- 参数1：必须，用来测试每个元素的函数，数组中的每个元素都会执行该回调函数，执行时会自动传入下面三个参数：`arr.every(function(element, index, array){   });`
  - element 必须， 当前遍历到的元素。
  - index 可选，当前遍历到的元素索引。
  - array 可选，调用every的数组。
- 参数2：可选，执行回调函数callback时作为this 对象的值。
- 返回值：如果回调函数的每一次返回都为 true，最终返回 true ，否则返回 false。

> **注意**：若收到一个空数组，此方法在一切情况下都会返回 `true`。
>
> every()意指“每一项”都符合条件。
>
> `every` 方法为数组中的每个元素执行一次 `callback` 函数，直到它找到一个会使 `callback` 返回 false 的元素。如果发现了一个这样的元素，`every` 方法将会立即返回 `false`。否则，`callback` 为每一个元素返回 `true`，`every` 就会返回 `true`。`callback` 只会为那些已经被赋值的索引调用。不会为那些被删除或从未被赋值的索引调用。

**arr.some(callback, thisArg)**

- 功能：测试数组中是不是至少有1个元素通过了被提供的函数测试。
- 参数1：必须，用来测试每个元素的函数，数组中的每个元素都会执行该回调函数，执行时会自动传入下面三个参数：`arr.some(function(element, index, array){   });`
  - element 必须， 当前遍历到的元素。
  - index 可选，当前遍历到的元素索引。
  - array 可选，调用some的数组。
- 参数2：可选，执行回调函数callback时作为this 对象的值。
- 返回值：数组中有至少一个元素通过回调函数的测试就会返回**`true`**；所有元素都没有通过回调函数的测试返回值才会为false。

> **注意：**如果用一个空数组进行测试，在任何情况下它返回的都是`false`。
>
> some()意指“某些项”是否符合条件。
>
> `some()` 为数组中的每一个元素执行一次 `callback` 函数，直到找到一个使得 callback 返回一个“真值”（即可转换为布尔值 true 的值）。如果找到了这样一个值，`some()` 将会立即返回 `true`。否则，`some()` 返回 `false`。`callback` 只会在那些”有值“的索引上被调用，不会在那些被删除或从来未被赋值的索引上调用。

**arr.filter(callback, thisArg)**

- 功能：创建一个新数组，其包含通过所提供测试函数的所有元素。 
- 参数1：必须，用来测试数组中每个元素的函数。返回 `true` 表示该元素通过测试，保留该元素，`false` 则不保留。数组中的每个元素都会执行该回调函数，执行时会自动传入下面三个参数：`arr.filter(function(element, index, array){   });`
  - element 必须， 当前遍历到的元素。
  - index 可选，当前遍历到的元素索引。
  - array 可选，调用filter的数组。
- 参数2：可选，执行回调函数callback时作为this 对象的值。
- 返回值：一个新的、由通过测试的元素组成的数组，如果没有任何数组元素通过测试，则返回空数组。

> filter为“过滤”、“筛选”之意。数组过滤后，返回过滤后的新数组。
>
> `filter` 为数组中的每个元素调用一次 `callback` 函数，并利用所有使得 `callback` 返回 true 或等价于 true 的值的元素创建一个新数组。`callback` 只会在已经赋值的索引上被调用，对于那些已经被删除或者从未被赋值的索引不会被调用。那些没有通过 `callback` 测试的元素会被跳过，不会被包含在新数组中。

练习：找出所有在线的对象存入新数组

```js
var friends = [
  { name: '张三', age: 18, online: true },
  { name: '李四', age: 16, online: false },
  { name: '王五', age: 20, online: true },
  { name: '李明', age: 20, online: false },
  { name: '花花', age: 16, online: true },
];
```

**arr.map(callback, thisArg)**

- 功能：创建一个新数组，其结果是该数组中的每个元素是调用一次提供的函数后的返回值。
- 参数1：必须，生成新数组元素的函数。数组中的每个元素都会执行该回调函数，执行时会自动传入下面三个参数：`arr.map(function(element, index, array){   });`
  - element 必须， 当前遍历到的元素。
  - index 可选，当前遍历到的元素索引。
  - array 可选，调用filter的数组。
- 参数2：可选，执行回调函数callback时作为this 对象的值。
- 返回值：一个由原数组每个元素执行回调函数的结果组成的新数组。

> map意思是“映射”，原数组被“映射”成对应的新数组。
>
> `map` 方法会给原数组中的每个元素都按顺序调用一次  `callback` 函数。`callback` 每次执行后的返回值（包括 `undefined`）组合起来形成一个新数组。 `callback` 函数只会在有值的索引上被调用；那些从来没被赋过值或者使用 `delete` 删除的索引则不会被调用。
>
> 因为`map`生成一个新数组，当你不打算使用返回的新数组却使用`map`是违背设计初衷的，请用`forEach`或者`for-of`替代。
>
> 你不该使用`map`的情况: (1)你不打算使用返回的新数组，或/且 (2)你没有从回调函数中返回值。

练习：

- 年龄不小于18岁，并且是在线状态，添加一个属性：sign: '赶紧出来浪'

- 年龄不小于18岁，并且是非在线状态，添加一个属性：sign: '赶快来摸鱼'

- 其余，添加一个属性：sign: '我是中国好青年'

```js

var friends = [
  { name: '张三', age: 18, online: true },
  { name: '李四', age: 16, online: false },
  { name: '王五', age: 20, online: true },
  { name: '李明', age: 20, online: false },
  { name: '花花', age: 16, online: true },
];
```

# 数组归并方法

ECMAScript 为数组提供了两个归并方法：reduce()和 reduceRight()。这两个方法都会迭代数组的所有项，并在此基础上构建一个最终返回值。reduce()方法从数组第一项开始遍历到最后一项。而 reduceRight()从最后一项开始遍历至第一项。

究竟是使用 reduce()还是 reduceRight()，只取决于遍历数组元素的方向。除此之外，这两个方法没什么区别。

**arr.reduce(callback, initialValue)**

- 功能：对数组中的每个元素执行一个由您提供的**reducer**函数(升序执行)，将其结果汇总为单个返回值。
- 参数1：必须，数组中的每个元素都会执行该回调函数 (如果没有提供 initialValue 则第一个元素除外)，执行时会自动传入下面四个参数：`arr.reduce(function(accumulator, currentValue, index, array){   });`
  - accumulator 必须，累计器，累计回调的返回值; 它是上一次调用回调时返回的累积值，或`initialValue`
  - currentValue 必须， 当前遍历到的元素。
  - index 可选，当前遍历到的元素索引。 如果提供了`initialValue`，则起始索引号为0，否则从索引1起始。
  - array 可选，调用reduce的数组。
- 参数2：可选，作为第一次调用 `callback`函数时的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。
- 返回值：函数累计处理的结果。

> reduce() 减少的意思，是指一种'累计'运算。
>
> 回调函数第一次执行时，`accumulator` 和`currentValue`的取值有两种情况：
>
> - 如果调用`reduce()`时提供了`initialValue`，`accumulator`取值为`initialValue`，`currentValue`取数组中的第一个值；
> - 如果没有提供 `initialValue`，那么`accumulator`取数组中的第一个值，`currentValue`取数组中的第二个值。
>
> **注意：**如果没有提供`initialValue`，reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引。如果提供`initialValue`，从索引0开始。
>
> 如果数组为空且没有提供`initialValue`，会抛出`TypeError`。如果数组仅有一个元素（无论位置如何）并且没有提供`initialValue`， 或者有提供`initialValue`但是数组为空，那么此唯一值将被返回并且`callback`不会被执行。

```js
var arr = [2, 20, 16, 30, 18];

// 情况1：不存在 initvalue
// 第一次遍历：prev的值是数组中第一元素的值，current的值是数组中第二个元素的值
// 从第二次遍历开始：current是数组中下一个元素的值，prev会存储是回调函数上一次return的值
result = arr.reduce(function (prev, current, index) {
  return prev + current;
});
console.log(result);// 186 数组中所有元素的和

// 情况2：存在initValue
// 第一次遍历：prev的是initValue的值10，current的值是数组中第一个元素的值
// 从第二次遍历开始：current是数组中下一个元素的值，prev会存储是回调函数上一次return的值
result = arr.reduce(function (prev, current, index) {
  return prev + current;
}, 10);
console.log(result);// 196
```

**arr.reduceRight(callback, initialValue)**

- 功能：接受一个函数作为累加器（accumulator）和数组的每个值（从右到左）将其减少为单个值。
- 参数1：必须，数组中的每个元素都会执行该回调函数 (如果没有提供 initialValue 则第一个元素除外)，执行时会自动传入下面四个参数：`arr.reduce(function(accumulator, currentValue, index, array){   });`
  - accumulator 必须，累计器，累计回调的返回值；它是上一次调用回调时返回的累积值，或`initialValue`
  - currentValue 必须， 当前遍历到的元素。
  - index 可选，当前遍历到的元素索引。 如果提供了`initialValue`，则起始索引号为0，否则从索引1起始。
  - array 可选，调用reduce的数组。
- 参数2：可选，作为第一次调用 `callback`函数时的第一个参数accumulator的值。 如果没有提供初始值，则将使用数组中的最后一个元素。 在没有初始值的空数组上调用 reduceRight将报错。
- 返回值：函数累计处理的结果。

> `reduceRight` 为数组中每个元素调用一次 `callback` 回调函数，但是数组中被删除的索引或从未被赋值的索引会跳过。回调函数接受四个参数：初始值（或上次调用回调的返回值）、当前元素值、当前索引，以及调用迭代的当前数组。
>
> 首次调用回调函数时，`accumulator` 和 `currentValue` 的可能取值情况有两种：
>
> - 如果在调用 `reduceRight` 时提供了 `initialValue` 参数，则 `accumulator` 等于 `initialValue`，`currentValue` 等于数组中的最后一个元素。
> - 如果没有提供 `initialValue` 参数，则 `accumulator` 等于数组最后一个元素， `currentValue` 等于数组中倒数第二个元素。
>
> 如果数组为空，但提供了 `initialValue` 参数，或如果数组中只有一个元素，且没有提供 `initialValue` 参数，将会直接返回 `initialValue` 参数或数组中的那一个元素。这两种情况下，都不会调用 `callback` 函数。
>
> 如果数组为空，且没有提供 `initialValue` 参数，则会抛出一个 `TypeError` 错误。

# 数组迭代器方法

ES6 中，Array 的原型上暴露了 3 个用于检索数组内容的方法：keys()、values()和entries()。

- keys()返回数组索引的迭代器
- values()返回数组元素的迭代器
- 而 entries()返回索引/值对的迭代器

**arr.keys()**

- 功能：返回一个包含数组中每个**索引键**的Array Iterator对象。
- 返回值：一个新的 Array 迭代器对象。

**arr.values()**

- 功能：返回一个包含数组中每**索引值**的Array Iterator对象。
- 返回值：一个新的 Array 迭代器对象。

**arr.entries()**

- 功能：返回一个包含数组中每个索引的键/值对的Array Iterator对象。
- 返回值：一个新的 Array 迭代器对象。

```js
var arr = ["foo", "bar", "baz", "qux"];
var aKeys = Array.from(arr.keys());
var aValues = Array.from(arr.values());
var aEntries = Array.from(arr.entries());
console.log(aKeys); // [0, 1, 2, 3] 
console.log(aValues); // ["foo", "bar", "baz", "qux"] 
console.log(aEntries); // [[0, "foo"], [1, "bar"], [2, "baz"], [3, "qux"]]
```

# 数组空位

数组的空位指的是，数组的某一个位置没有任何值，比如`Array()`构造函数返回的数组都是空位。

使用数组字面量初始化数组时，可以使用一串逗号来创建空位（hole）。ECMAScript 会将逗号之间相应索引位置的值当成空位，ES6 规范重新定义了该如何处理这些空位。

可以像下面这样创建一个空位数组：

```js
Array(3) // [, , ,]
const options = [,,,,,]; // 创建包含 5 个元素的数组
console.log(options.length); // 5 
console.log(options); // [,,,,,] 
```

注意，空位不是`undefined`，某一个位置的值等于`undefined`，依然是有值的。空位是没有任何值，`in`运算符可以说明这一点。

```js
0 in [undefined, undefined, undefined] // true
0 in [, , ,] // false
```

ES5 对空位的处理，已经很不一致了，大多数情况下会忽略空位。

- `forEach()`, `filter()`, `reduce()`, `every()` 和`some()`都会跳过空位。
- `map()`会跳过空位，但会保留这个值
- `join()`和`toString()`会将空位视为`undefined`，而`undefined`和`null`会被处理成空字符串。

```js
// forEach方法
[,'a'].forEach((x,i) => console.log(i)); // 1

// filter方法
['a',,'b'].filter(x => true) // ['a','b']

// every方法
[,'a'].every(x => x==='a') // true

// reduce方法
[1,,2].reduce((x,y) => x+y) // 3

// some方法
[,'a'].some(x => x !== 'a') // false

// map方法
[,'a'].map(x => 1) // [,1]

// join方法
[,'a',undefined,null].join('#') // "#a##"

// toString方法
[,'a',undefined,null].toString() // ",a,,"
```

ES6 则是明确将空位转为`undefined`。

`Array.from()`方法会将数组的空位，转为`undefined`，也就是说，这个方法不会忽略空位。

```javascript
Array.from(['a',,'b'])
// [ "a", undefined, "b" ]
```

扩展运算符（`...`）也会将空位转为`undefined`。

```javascript
[...['a',,'b']]
// [ "a", undefined, "b" ]
```

`copyWithin()`会连空位一起拷贝。

```javascript
[,'a','b',,].copyWithin(2,0) // [,"a",,"a"]
```

`fill()`会将空位视为正常的数组位置。

```javascript
new Array(3).fill('a') // ["a","a","a"]
```

`for...of`循环也会遍历空位。

```javascript
let arr = [, ,];
for (let i of arr) {
  console.log(1);
}
// 1
// 1
```

上面代码中，数组`arr`有两个空位，`for...of`并没有忽略它们。如果改成`map()`方法遍历，空位是会跳过的。

`entries()`、`keys()`、`values()`、`find()`和`findIndex()`会将空位处理成`undefined`。

```javascript
// entries()
[...[,'a'].entries()] // [[0,undefined], [1,"a"]]

// keys()
[...[,'a'].keys()] // [0,1]

// values()
[...[,'a'].values()] // [undefined,"a"]

// find()
[,'a'].find(x => true) // undefined

// findIndex()
[,'a'].findIndex(x => true) // 0
```

由于空位的处理规则非常不统一，所以建议避免出现空位。

# 扩展运算符

## 扩展运算符的定义

扩展运算符（spread）是三个点（`...`）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列。

```javascript
console.log(...[1, 2, 3])// 1 2 3

console.log(1, ...[2, 3, 4], 5)// 1 2 3 4 5

[...document.querySelectorAll('div')]// [<div>, <div>, <div>]
```

该运算符主要用于函数调用。

```javascript
function push(array, ...items) {
  array.push(...items);
}

function add(x, y) {
  return x + y;
}

const numbers = [4, 38];
add(...numbers) // 42
```

## 扩展运算符的应用

**（1）复制数组**

数组是复合的数据类型，直接复制的话，只是复制了指向底层数据结构的指针，而不是克隆一个全新的数组。

```javascript
const a1 = [1, 2];
const a2 = a1;

a2[0] = 2;
a1 // [2, 2]
```

上面代码中，`a2`并不是`a1`的克隆，而是指向同一份数据的另一个指针。修改`a2`，会直接导致`a1`的变化。

ES5 只能用变通方法来复制数组。

```javascript
const a1 = [1, 2];
const a2 = a1.concat();

a2[0] = 2;
a1 // [1, 2]
```

上面代码中，`a1`会返回原数组的克隆，再修改`a2`就不会对`a1`产生影响。

扩展运算符提供了复制数组的简便写法。

```javascript
const a1 = [1, 2];
// 写法一
const a2 = [...a1];
// 写法二
const [...a2] = a1;
```

上面的两种写法，`a2`都是`a1`的克隆。

**（2）合并数组**

扩展运算符提供了数组合并的新写法。

```javascript
const arr1 = ['a', 'b'];
const arr2 = ['c'];
const arr3 = ['d', 'e'];

// ES5 的合并数组
arr1.concat(arr2, arr3);// [ 'a', 'b', 'c', 'd', 'e' ]

// ES6 的合并数组
[...arr1, ...arr2, ...arr3]// [ 'a', 'b', 'c', 'd', 'e' ]
```

不过，这两种方法都是浅拷贝，使用的时候需要注意。

```javascript
const a1 = [{ foo: 1 }];
const a2 = [{ bar: 2 }];

const a3 = a1.concat(a2);
const a4 = [...a1, ...a2];

a3[0] === a1[0] // true
a4[0] === a1[0] // true
```

上面代码中，`a3`和`a4`是用两种不同方法合并而成的新数组，但是它们的成员都是对原数组成员的引用，这就是浅拷贝。如果修改了引用指向的值，会同步反映到新数组。

**（3）与解构赋值结合**

扩展运算符可以与解构赋值结合起来，用于生成数组。

```javascript
// ES5
a = list[0], rest = list.slice(1)
// ES6
[a, ...rest] = list
```

下面是另外一些例子。

```javascript
const [first, ...rest] = [1, 2, 3, 4, 5];
first // 1
rest  // [2, 3, 4, 5]

const [first, ...rest] = [];
first // undefined
rest  // []

const [first, ...rest] = ["foo"];
first  // "foo"
rest   // []
```

如果将扩展运算符用于数组赋值，只能放在参数的最后一位，否则会报错。

```javascript
const [...butLast, last] = [1, 2, 3, 4, 5];
// 报错

const [first, ...middle, last] = [1, 2, 3, 4, 5];
// 报错
```

**（4）字符串**

扩展运算符还可以将字符串转为真正的数组。

```javascript
[...'hello']// [ "h", "e", "l", "l", "o" ]
```

# for…of

for...of语句在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句。

**迭代数组时常用 for...of**

```js
var arr = ['red', 'green', 'blue'];

// for...of循环读取键值
for(var value of arr) {
  console.log(value); // red green blue
}

var str = "boo";

for (var value of str) {
  console.log(value);// b o o
}


// for...in循环读取键名
for (var key in arr) {
  console.log(key); // 0 1 2
  console.log(arr[key]); // red green blue
}
```

**迭代对象时常用 for...in**

```js
var zhangsan = {
  name: '张三', 
  age: 18,
  say: function () {
    console.log('我叫'+this.name);
  }
};

// key 对应对象中的每一个属性
for (var key in zhangsan) {
  console.log(key);// 'name' 'age' 'say'
  console.log(zhangsan[key]);// '张三' 18 f(){}
}
```

for...of与for...in的区别：无论是for...in还是for...of语句都是迭代一些东西。它们之间的主要区别在于它们的迭代方式。

- for可以用来循环数组，或者类似于数组的对象(比如：字符串)。但for语句不能[直接]循环对象，因为对象没有迭代器，但可以间接使用Object.keys()返回对象的属性名称所组成的迭代器。

- for...in 语句以任意顺序迭代对象的可枚举属性。专门用于对象的迭代。一般不用来迭代数组，但并不表示不能循环数组。

- for...of 语句遍历可迭代对象定义要迭代的数据。专门用于类似数组结构的数据迭代。也可以用来迭代带有迭代器的对象，不能直接循环对象，循环对象时，要使用Object.keys(obj)返回键的迭代器，再去循环。

# 练习

- `[1,2,6,4,5,3,7,8,9]`删除数组中所有的偶数。
- 封装一个函数返回布尔值，判断一个数组中的所有数字是否都是质数。
- `['a','b','b','a','a','c','c','a']`数组排重。
- 使用数组随机获取四位数字不相同的密码。[0-9]

```js
var news = [
  '假期要问回顾：月饼事件惊动阿里高层',
  '上海中秋假期消费者投诉情况：网络销售和食品',
  '马龙登时尚杂志封面 秀胸肌单眼皮放电撩人',
  '阿里巴巴4名工程师 因“刷月饼”被开除',
  '过节出门没带钱？“手机变钱包”不会太远'
];
```

- 找出含有指定关键词的新闻标题，添加编号，并将关键词标记为黄色。比如关键词为“月”，把内容使用document.write显示到页面上，显示效果如下：

0、假期要问回顾：==月==饼事件惊动阿里高层

1、上海中秋假期消费者投诉情况：网络销售和食品

2、马龙登时尚杂志封面 秀胸肌单眼皮放电撩人

3、阿里巴巴4名工程师 因“刷==月==饼”被开除

4、过节出门没带钱？“手机变钱包”不会太远

