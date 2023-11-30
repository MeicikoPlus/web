# default

前面所有的导出功能都是有名字的导出（`named exports`）：

- 在导出 `export` 时指定的名字；
- 在导入 `import` 时需要知道具体的名字；

但是其实还有一种导出叫做 ==默认导出==（`default export`）：

- 默认导出 `export` 时可以不指定名字；
- 在导入时不需要使用 `{}`，并且可以自己来指定名字；
- 它也在方便我们和现有的 `CommonJS` 等规范相互操作；



## 具体使用

导入操作：（原本导入）

```js
import { add } form "./add.js"
```

在上述操作中，==如果文件名称已经足以代表这个文件会拿出的内容的时候，并且导出的只有一个东西的时候==，在导出的时候可以这样写：

```js
// add.js
function add(x, y) {
  return x + y
}

export default add
```

这是==第一种写法==，这样来做的时候，这个文件默认导出的就是这个函数，这样，在导入的文件中可以这样写：

```js
import "随便写个名字，因为是默认到处所以没有名字" form "./add.js"
// 这个随便起的名字就会指向导出的数据
```



==第二种写法==：在导出的文件：

```js
export default function(x, y) {
  return x + y
}
```

在导入的文件中：

```js
import fn form "./add.js"
```

注意：==一个文件只能有一个默认导出==；

