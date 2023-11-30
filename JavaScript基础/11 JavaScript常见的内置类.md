# **JavaScript常见内置类**

## 原始包装类型

原始包装类型并非对象类型, 所以从理论上来说, 它们是没有办法去获取属性和调用方法的, 但是我们却经常可以看到我们将一个自负床进行了一次.length的操作, 那么为什么可以调用属性(或者方法)了呢? 

这是因为**JavaScript为了使它获得属性和调用方法, 对他进行了一次封装**, 也就是, 当一个string对象使用了length对象是,. 会线创建一个初始值相同的string对象, 然后调用length, 最好再把封装的方法清理.

具体操作:

1. 根据原始值, 创建一个原始类型对应的包装类型对象.
2. 调用对应的属性或者方法, 返回一个新的值.
3. 创建的包装类型会被销毁.
4. 通常JavaScript引擎会进行很多的优化, 它可以通过跳过创建包装被的过程在内部直接完成属性的获取或者方法的调用.

## Number数字类型

当我们定义了一个num = 123, 类似这种的变量, 其实就相当于new 了一个number()的构造函数.

它有一些自己的方法和属性:

1. toString(2): 转换成2进制(字符串类型), 2可以换成其他的数字.

2. toFixed(digits): 格式化一个数字, 保留digits位的小数.

   ```JavaScript
   let num = 3.1415926
   num = num.toFixed(2)
   console.log(num) // 3.14
   ```

3. Number.parseInt(string[, radix), 将字符串解析成整数, 也有对应的全局方法parseInt;

4. Number.parseFloat(string), 将字符串解析成浮点数, 也有对应的全局方法prseFloat;

   ```JavaScript
   let num = "123.321"
   num = Number(num) // 将字符串类型转换成了浮点数(小数)(自动转换它的类型)
   // 123.321
   num = Number.parseInt(num) // 将字符转换成了整数类型
   // 123
   num = Number.parseFloat(num) // 将字符串转换成了浮点数类型
   // 123,321
   ```

   

## Math数学对象

math是一个内置对象(不是一个构造函数), 它**拥有一些数学属性和数学函数方法**.

- Math.PI: 这是Math对象的一个属性, 意思是圆周率.
- Math.floor(num): 向下取整, 相当于舍弃掉小数点后面的位数, 可以理解为返回小于或者定于num的最大整数.
- Math.abs(num): 返回num的绝对值.
- Math.ceil(num): 返回大于或等于num的最小整数, 向上取整.
- Math.round(num): 返回num四舍五入的整数.
- Math.max(num1, num2, ... ...): 返回这组数据中的最大值.
- Math.minin(num1, num2, ... ...): 返回这组数据中的最小值.
- Math.random(num): 返回一个0到1之间的随机数.
- Math.sqrt(num): 返回num的平方根.	
- Math.pow(x, y): 返回x的y次方.

值得注意的是, math对象种的所有办法都是静态办法, 没有办法通过创建一个math对象的实例来调用.

## String字符串类型

- .length: 获取字符串的长度.
- .charAt(): 用来返回指定位置的字符.
- .concat(): 用于两个或者多个字符串的拼接.
- .indexof(): 返回某个字符或者字符串在字符串中首次出现的位置.
- .lastIndexOf(): 返回某个字符在字符串中最后一次出现时的位置.
- replace(): 在字符串中的一些字符替换为另一些字符或者替换一个正则表达式.
- slice(): 提取字符串中的一部分并返回一个新的字符串.
- split(): 用指定的分隔符将一个字符串分割为多个子字符串, 并把结果放在一个数组中.
- toLowerCase() : 字符串转换成小写字母.
- toUpperCase(): 将字符串转换为大写字母.
- startsWith(): 判断字符串是否以某个字符串开头.
- endsWith(): 判断字符串是否以某个字符串结尾.

## Array数组类型

- push() :在数组的末尾添加一个或者多个元素.
- pop() : 删除数组末尾的一个元素, 并且返回删除的元素.
- shift() : 删除数组的第一个元素, 并且返回删除的元素.
- unshift() : 在数组开头添加一个或者多个元素.
- splice(): 在数组中添加或者删除元素, 可以在指定位置添加元素, 也可以删除指定位置的元素.
- slice(): 返回数组的一部分, 不会修改数组, 而是返回一个新数组.
- concat() : 将两个或者多个数组组合成为一个新的数组.
- reverse() : 反转数组的元素顺序.
- sort() : 按照指定规则对数组进行排序.
- indexof() : 查找树组是否包含指定元素, 并且返回他的位置.
- lastIndexOf() : 查找数组中最后一次出现指定元素的位置.
- join() : 将数组中所有元素转换成一个字符串, 并且返回该字符串.
- toString() : 将数组转换成一个字符串, 并且返回字符串.
- forEach() : 对数组中每个元素执行指定的操作.
- map() : 将数组中每个元素都执行指定操作, 并且返回一个新的数组.
- filter() : 根据指定条件筛选出符合条件的数组元素, 并且返回一个新的数组.
- reduce() : 对数组中的元素一次执行指定的操作, 并且返回器最终的结果.
- 操作符...: ...[1, 2, 3]的效果是吧列表转换为参数列表即1, 2, 3，也可以这样做：
  ```js
  var arr = [1, 2, 3]
  var arr2 = [4, 5, 6]
  var arr3 = [...arr, ...arr2]
  
  // 或者
  var arr4 = arr.push(...arr2)
  ```

  

## Date对象

- new Date(): 创建一个'Data'的对象, 返回当前的时间.
- Date.now(): 获取当前时间的时间戳.
- Date,parse(str): 可以获取一个字符串的日期, 并且输出对应的时间戳.
- getTime(): 获取当前时间的时间戳
- getFullYear(): 获取年份, 返回四位数字.
- getMonth(): 获取月份, 返回0 - 11的数字, 需要加上1才是实际的月份.
- getData(): 获取一个月的第几天, 返回1-31之间的数字.
- getHours(): 获取小时, 返回0-23之间的数字
- getMinutes(): 获取分钟,返回0-59之间的数字.
- getSconds(): 获取秒数, 返回0-59之间的数字.
- getMilliseconds(): 获取毫秒数, 返回0-999之间的数字.
- toString(): 将当前日期转化成为字符串的形式.
- toLocaleDataString(): 将当前的日期部分转换成本地字符串的形式.

## 时间变化练习

```JavaScript
// 选择类名为 "date" 的第一个元素，并存储到变量 time_elements 中网页中的元素
const time_elements = document.querySelector('.date');

// 定义 updateClock 函数，用于更新时钟显示
function updateClock() {
  const nowtime = getCurrentTime();
  time_elements.innerHTML = nowtime;
}

// 定义 getCurrentTime 函数，用于获取当前时间
function getCurrentTime() {
  const nowDate = new Date();
  const year = nowDate.getFullYear();
  const month = formattingTime(nowDate.getMonth() + 1);
  const monthDate = formattingTime(nowDate.getDate());
  const hour = formattingTime(nowDate.getHours());
  const minute = formattingTime(nowDate.getMinutes());
  const second = formattingTime(nowDate.getSeconds());
  const nowtime = `${year}年${month}月${monthDate}日${hour}:${minute}:${second}`;
  return nowtime;
}

// 定义 formattingTime 函数，用于将数字转换为两位数的字符串
function formattingTime(str){
  let strtime = String(str);
  return strtime.padStart(2, '0');
}

// 每秒钟调用一次 updateClock 函数，实时更新时间显示
setInterval(updateClock, 1000);
```

上述代码也可以用一个对象去实现, 这样还可以单独调用时间中的一些单独的比如分钟等等...

```JavaScript
const time_elements = document.querySelector('.date');
const timeObj = {
  year: 0,
  month: 0,
  monthDate: 0,
  hour: 0,
  minute: 0,
  second: 0,
  updateTime: function() {
    const nowDate = new Date();
    this.year = nowDate.getFullYear();
    this.month = this.formattingTime(nowDate.getMonth() + 1);
    this.monthDate = this.formattingTime(nowDate.getDate());
    this.hour = this.formattingTime(nowDate.getHours());
    this.minute = this.formattingTime(nowDate.getMinutes());
    this.second = this.formattingTime(nowDate.getSeconds());
  },
  formattingTime: function(str) {
    let strtime = String(str);
    return strtime.padStart(2, '0');
  },
  getNowTime: function() {
    this.updateTime();
    const nowtime = `${this.year}年${this.month}月${this.monthDate}日${this.hour}:${this.minute}:${this.second}`;
    return nowtime;
  }
};

setInterval(function() {
  const nowtime = timeObj.getNowTime();
  time_elements.innerHTML = nowtime;
}, 1000);
```

