# 组合函数

## 组合函数的理解

**组合函数**是在JavaScript开发过程中一种对函数的使用技巧、模式。

- 比如我们现在需要对某一个数据进行函数的调用，执行两个函数fn1和fn2，这两个函数是依次执行的。
- 那么**如果我们每次都需要进行两个函数的调用，操作上就会显得重复**。
- 那么**可以将两个函数组合起来，自动依次调用**。
- 这种对函数的组合，我们称之为组合函数（Compose Function）。



## 组合函数的简单案例

假设现在有一堆数字（比如100），现在需要对它们进行两步操作：

1. 现将这个数字×2。
2. 再将这个数字**2。

此时有两个方法：

```javascript
var num = 100

function doblue(num) {
	return num * 2
}

function pow(num) {
	return num ** 2
}
```

```javascript
console.log(pow(doble(num)))
```

总之，想要表达的意思是，对于某些操作一个函数是满足不了需求的，需要接着做另外一种操作。

此时可以将上述两个函数组合在一起生成一个新的函数。

```JavaScript
function composeFn(num) {
  return pow(double(num))
}
```

==上述这种函数就叫做组合函数==。

但是上述这个函数灵活性很低，已经被固定死了，所以最好可以==封装一个通用性函数==。



## 封装一个通用性高的组合函数

```JavaScript

// 更加通用的组合函数（传入多个函数，并自动组合在一起挨个调用）
function composeFn(...fns) {

  // 1. 先做一个判断，判断接收的参数是否是一个函数
  const length = fns.length
  if (length <= 0) return
  for (let i = 0; i < length; i++) {
    const fn = fns[i]
    if (typeof fn !== "function") {
      throw new Error(`index_position = ${i} must be function`)
    }
  }


  // 2. 返回一个新的函数
  return function(...args) {
    // 先取出第一函数，为什么只取出第一个函数？
    // 因为第一个函数可能会接收很多个参数
    // 但是函数只能返回一个值
    // 所以后面的函数肯定只需要接收一个值（返回值）比较稳定。

    let result = fns[0].apply(this, args) // 拿到第一个函数的执行结果
    for (let i = 1; i < length; i++) {
      const fn = fns[i]
      result = fn.apply(this, [result])
    }

    return result
  }
}
```

如果只是想打印的话，可以在函数的参数最后一位传入console.log

```JavaScript
composeFn(函数1, 函数2, ... , console.log)
// 这个函数执行完后就会打印，不是去使用。
```

