# Babel 是什么？

Babel 是一个 JavaScript 编译器

中文文档：https://www.babeljs.cn/

Babel 是一个工具链，主要用于将采用 ECMAScript 2015+ 语法编写的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。这意味着，你可以用ES6+的方式编写程序，又不用担心现有环境是否支持。下面列出的是 Babel 能为你做的事情：

- 语法转换
- 通过 Polyfill 方式在目标环境中添加缺失的特性 （通过引入第三方 polyfill 模块，例如 [core-js](https://github.com/zloirock/core-js)）
- 源码转换（codemods）

> 注：polyfill(polyfiller)，指的是一个代码块（通常是 Web 上的 JavaScript）。这个代码块向开发者提供了一种技术， 这种技术可以让浏览器提供原生支持，抹平不同浏览器对API兼容性的差异。
>
> 比如数组中`Array.prototype.includes`, 不同浏览器，不同版本，有些支持，有些不支持。`Polyfill`帮你把这些差异化抹平，不支持的变得支持了。

# 使用指南

本指南将想你展示如何将 ES2015+ 语法的 JavaScript 代码编译为能在当前浏览器上工作的代码。这将涉及到新语法的转换和缺失特性的修补。

## 第一步：初始化项目

使用npm init 初始化项目。

```shell
npm init
```

## 第二步：安装模块包

运行以下命令安装所需的包

```bash
npm install --save-dev @babel/core @babel/cli @babel/preset-env
```

- `@babel/core`：Babel 的核心功能包含在 `@babel/core` 模块中。
- `@babel/cli`：Babel 自带的 CLI 命令行工具，可通过命令行编译文件。
- `@babel/preset-env`：智能预设，用于编译 ES2015+ 语法。

> --save简写为-S；--save-dev简写为-D
>
> --save和 --save-dev 的区别
>
> --save 的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。
>
> --save-dev 的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。
>
> dependencies 节点下的模块是项目运行必备的，比如： bootstrap、vue、express，所以应该使用 --save 的形式安装；
> devDependencies 节点下的模块是在开发时需要用的，比如项目中使用构建工具webkpack、 gulp，用来辅助压缩js、css、html等。这些模块在我们的项目部署后是不需要的，所以我们可以使用 --save-dev 的形式安装；

## 第三步：babel配置文件

### 配置文件

配置文件由很多种写法：

- `babel.config.*`：新建文件，位于项目根目录
  - `babel.config.js`
  - `babel.config.json`
- `.babelrc.*`：新建文件，位于项目根目录
  - `.babelrc`
  - `.babelrc.js`
  - `.babelrc.json`
- `package.json` 中添加 babel选项：不需要创建单独的文件，在package.json 文件中添加babel配置

不管是那种配置文件，Babel 会查找和自动读取它们，所以以上配置文件只需要存在一个即可。

### 具体配置

在项目的根目录下创建一个命名为 `babel.config.json` 的配置文件（需要 `v7.8.0` 或更高版本），并将以下内容复制到此文件中：

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1",
          "ie": '11',
        },
        "useBuiltIns": "usage",
        "corejs": "3.6.5"
      }
    ]
  ]
}
```

如果你使用的是 Babel 的旧版本，则文件名为 `babel.config.js`。推荐使用js文件，添加注释比较方便，也符合平时写代码的习惯。

```js
module.exports = {  
  presets: [
    [
      "@babel/preset-env",
      {
        targets: {
          edge: "17",
          firefox: "60",
          chrome: "67",
          safari: "11.1",
          ie: '11',
        },
        useBuiltIns: "usage",
        corejs: "3.6.4",
      },
    ],
  ] 
};
```

## 第四步：编译ES6+代码

### 编译文件

如果你希望转换结果写入一个文件，使用 `--out-file` 或 `-o` 参数指定输出文件

在根目录创建main.js并编写ES6+代码，运行下面的命令把main.js中的代码编译到index.js中

```shell
./node_modules/.bin/babel main.js -o index.js
```

你可以利用 npm@5.2.0 所自带的 npm 包运行器将 `./node_modules/.bin/babel` 命令缩短为 npx babel

```shell
npx babel main.js -o index.js
```

要在 **每次文件修改后** 编译该文件，请使用 `--watch` 或 `-w` 参数：

```
npx babel main.js -o index.js -w
```

### 编译整个目录

编译整个 `src` 目录下的文件并输出到 `lib` 目录，输出目录可以通过 `--out-dir` 或 `-d` 指定。

在根目录创建src目录，把main.js放在src目录中，运行下面的命令将 `src` 目录下的所有代码编译到 `lib` 目录：

```bash
npx babel src -d lib
```

编译整个 `src` 目录下的文件并将输出合并为一个文件。

```
npx babel src -d index.js
```

### 配置编译命令

将命令配置在 `package.json` 文件的 `scripts` 字段中:

```json
"scripts": {
  "build": "npx babel src -d lib"
}
```

执行 `npm run build` 编译。

# babel详解

## 核心模块介绍

-  `@babel/core`：Babel 的核心功能包含在 `@babel/core` 模块中。没有它，在 `babel` 的世界里注定寸步难行。不安装 `@babel/core`，无法使用 `babel` 进行编译。

-  `@babel/cli`：`babel` 提供的命令行工具，主要是提供 `babel` 这个命令，可通过命令行编译文件。

新建项目名为babel-demo，并使用`npm init -y` 进行初始化，创建 `src/main.js`，文件内容如下

```js
const arr = [1, 2, 3];
arr.forEach(item => {
	console.log('item: ' + item);
});
```

安装模块

```shell
npm install --save-dev @babel/core @babel/cli
```

将命令配置在 `package.json` 文件的 `scripts` 字段中:

```json
"scripts": {
  "build": "npx babel src -d lib"
}
```

执行 `npm run build` 编译，编译结果为

```js
const arr = [1, 2, 3];
arr.forEach(item => {
  console.log('item: ' + item);
});
```

现在我们没有配置任何插件，编译前后的代码是完全一样的。因为 `Babel` 虽然开箱即用，但是什么也不做，如果想要 `Babel` 做一些实际的工作，就需要为其添加插件(`plugin`)。

## 插件

`Babel` 构建在插件之上，使用现有的或者自己编写的插件可以组成一个转换通道，`Babel` 的插件分为两种: 语法插件和转换插件。

### 语法插件

这些插件只允许 `Babel` **解析（parse）** 特定类型的语法（不是转换），可以在 `AST` 转换时使用，以支持解析新语法，例如：

```js
import * as babel from "@babel/core";
const code = babel.transformFromAstSync(ast, {
  //支持可选链
  plugins: ["@babel/plugin-proposal-optional-chaining"],
  babelrc: false
}).code;
```

### 转换插件

转换插件会启用相应的语法插件(因此不需要同时指定这两种插件)，这点很容易理解，如果不启用相应的语法插件，意味着无法解析，连解析都不能解析，又何谈转换呢？

### 常见插件

`Babel`官网列举出了一份非常详尽的[Plugin List](https://www.babeljs.cn/docs/plugins-list)。

关于常见的`Plugin`其实大多数都集成在了`babel-preset-env`中，当你发现你的项目中并不能支持最新的`js`语法时，此时我们可以查阅对应的`Babel Plugin List`找到对应的语法插件添加进入`babel`配置。

### 插件的使用

如果插件发布在 `npm` 上，可以直接填写插件的名称， `Babel` 会自动检查它是否已经被安装在 `node_modules` 目录下，在项目目录下新建 `babel.config.js` 文件，配置如下：

```json
module.exports = {
  plugins: ["@babel/plugin-transform-arrow-functions"]
}
```

执行 `npm run build`进行编译，编译结果为：

```js
const arr = [1, 2, 3];
arr.forEach(function (item) {
  console.log('item: ' + item);
});
```

现在，我们仅支持转换箭头函数，如果想将其它的新的JS特性转换成低版本，需要使用其它对应的 `plugin` 。如果我们一个个配置的话，会非常繁琐，因为你可能需要配置几十个插件，这显然非常不便，那么有没有什么办法可以简化这个配置呢？

使用babel的预设（preset）。

## 预设（preset）

**所谓`Preset`就是一些`Plugin`组成的合集**，你可以将`Preset`理解称为就是一些的`Plugin`整合称为的一个包。

### 官方 Preset

- @babel/preset-env
- @babel/preset-flow
- @babel/preset-react
- @babel/preset-typescript

### @babel/preset-env

`@babel/preset-env` 主要作用是对我们所使用的并且目标浏览器中缺失的功能进行代码转换和加载 `polyfill`，在不进行任何配置的情况下，`@babel/preset-env` 所包含的插件将支持所有最新的JS特性(ES2015,ES2016等，不包含 stage 阶段)，将其转换成ES5代码。

`@babel/preset-env`内部集成了绝大多数`plugin`（`State > 3`）的转译插件，它会根据对应的参数进行代码转译。

在 `babel.config.js` 的配置文件文件中添加如下内容：

```json
module.exports = {
  presets: ["@babel/preset-env"]
}
```

执行 `npm run build`进行编译，编译结果为：

```js
const arr = [1, 2, 3];
arr.forEach(function (item) {
  console.log('item: ' + item);
});
```

> `@babel/preset-env`不会包含任何低于 Stage 3 的 JavaScript 语法提案。如果需要兼容低于`Stage 3`阶段的语法则需要额外引入对应的`Plugin`进行兼容。

> 需要额外注意的是`babel-preset-env`仅仅针对语法阶段的转译，比如转译箭头函数，`const/let`语法。针对一些`Api`或者`Es 6`内置模块的`polyfill`，`preset-env`是无法进行转译的。

### @babel/preset-flow

如果您使用了 Flow，则建议您使用此预设（preset）。Flow 是一个针对 JavaScript 代码的静态类型检查器。此预设（preset）包含以下插件：

- @babel/plugin-transform-flow-strip-types

### @babel/preset-react

通常我们在使用`React`中的`jsx`时，实质上`jsx`最终会被编译称为`React.createElement()`方法。

`@babel/preset-react`这个预设起到的就是将`jsx`进行转译的作用。

### @babel/preset-typescript

对于`TypeScript`代码，我们有两种方式去编译`TypeScript`代码成为`JavaScript`代码。

1. 使用`tsc`命令，结合`cli`命令行参数方式或者`tsconfig`配置文件进行编译`ts`代码。
2. 使用`babel`，通过`babel-preset-typescript`代码进行编译`ts`代码。

# @babel/preset-env

`@babel/preset-env`是一个智能的`babel`预设，让你能使用最新的`JavaScript`语法，它会帮你转换成代码的目标运行环境支持的语法，提升你的开发效率并让打包后的代码体积更小。

## 浏览器列表集合browserslist

对基于浏览器，我们推荐使用一个`.browserslistrc`文件指定编译目标。如果没有配置`targets`或者`ignoreBrowserslistConfig`, `@babel/preset-env`将使用默认的`Browserslist`配置。

Browserslist 将使用来自以下来源之一的浏览器和 Node.js 版本查询，详见[browserslist文档](https://github.com/browserslist/browserslist)：

1. `browserslist`键入`package.json`当前或父目录中的文件。 **我们推荐这种方式。**
2. `.browserslistrc`当前或父目录中的配置文件。
3. `browserslist`当前或父目录中的配置文件。
4. `BROWSERSLIST`环境变量。
5. 如果上述方法没有产生有效的结果，Browserslist 将使用默认值： `> 0.5%, last 2 versions, Firefox ESR, not dead`.

如果你想支持市场份额大于0.25%而且忽略安全更新的浏览器如`IE 10`和`BlackBerry`的语法转换和语法实现, 你可以采用如下的配置：

.browserlistrc

```
> 0.25%
not dead
```

或者你可以在`package.json`文件中添加browserslist选项配置支持的浏览器：

```
"browserslist": "> 0.25%, not dead"
```

推荐配置：

```json
{
  "browserslist": "last 2 versions, > 1%, not dead",
}
```

或者：

```json
{
  "browserslist": [
    "last 2 versions", 
    "> 1%", 
    "not dead"
  ]
}
```

## 配置项

`babel-preset-env` 的主要参数选项有：

- targets
- targets.node：如果你的编译针对当前的`node`版本, 你可以指定`node`配置项, 如果你指定为`curren`, 将等同于 `"node": process.versions.node`
- targets.browsers：一个利用`browserlist`的查询选项, 如`last 2 versions, > 5%, safari tp`
- spec : 启用更符合规范的转换，但速度会更慢，默认为 `false`
- loose：是否使用 `loose mode`，默认为 `false`
- modules：将 ES6 module 转换为其他模块规范，可选 `"adm" | "umd" | "systemjs" | "commonjs" | "cjs" | false`，默认为 `false`
- debug：启用debug，默认 `false`
- include：一个包含使用的 `plugins` 的数组
- exclude：一个包含不使用的 `plugins` 的数组
- useBuiltIns：为 `polyfills` 应用 `@babel/preset-env` ，可选 `"usage" | "entry" | false`，默认为 `false`

## targets

`targets`数据类型: `string | Array<string> | { [string]: string }`，如果没有 browserslist ，默认最外层的 targets 选项，否则默认 `{}`

- targets描述你的项目支持的目标环境

它可以是一个浏览器的用户范围：

```js
{
  targets: "> 0.25%, not dead"
}
```

或具体某个产品的版本：

```js
{
  targets: {
    chrome: "58",
    ie: "11"
  }
}
```

环境如: `chrome`, `opera`, `edge`, `firefox`, `safari`, `ie`, `ios`, `android`, `node`, `electron`

由于 preset-env 的最初设计目标是帮助用户使用 preset-latest 轻松的过渡，因此在为指定 targets 时，preset-env 会将所有 ES2015-ES2020 的代码转化为 ES5 。

我们不建议以这种方式使用预设环境，因为它没有利用其针对特定目标环境版本的能力。

因此， preset-env 的行为与 browserslists 不同：当 Babel 没有 targets 配置或者没有 browserslists 配置，它不会默认查询，如果你想使用默认查询，你就需要显示的配置（browserslists的默认是 > 0.5%, last 2 versions, Firefox ESR, not dead ）：

```json
{
  "presets": [["@babel/preset-env", { "targets": "defaults" }]]
}
```

如果你也不想用 > 0.5%, last 2 versions, Firefox ESR, not dead ，就得显式配置。

# Polyfill

## Polyfill 介绍

什么是 polyfill ，为什么需要 polyfill：

首先我们来理清楚这三个概念:

- 最新`ES`语法，比如：箭头函数，`let/const`。
- 最新`ES Api`，比如`Promise`
- 最新`ES`实例/静态方法，比如`String.prototype.includes`

`@babel/preset-env`仅仅只会转化最新的`es`语法，并不会转化对应的`Api`和实例方法，比如说`ES6`中的`Array.from`静态方法。`babel`是不会转译这个方法的，如果想在低版本浏览器中识别并且运行`Array.from`方法达到我们的预期就需要额外引入`polyfill`进行在`Array`上添加实现这个方法。

语法层面的转化`@babel/preset-env`完全可以胜任。但是一些内置方法模块，仅仅通过`@babel/preset-env`的语法转化是无法进行识别转化的，所以就需要一系列类似”垫片“的工具进行补充实现这部分内容的低版本代码实现。这就是所谓的`polyfill`的作用。

比如我在代码里使用了 Array.prototype.includes 方法:

```js
// 源代码
const arr = [1, 2, 3];
arr.forEach(item => {
	console.log('item: ' + item);
});

const flag = arr.includes(2);
console.log('flag=', flag);

// 编译之后的代码

"use strict";

var arr = [1, 2, 3];
arr.forEach(function (item) {
  console.log('item: ' + item);
});
var flag = arr.includes(2);
console.log('flag=', flag);
```

可以清楚的看到，所谓 ES 语法比如 const、lest、箭头函数都已经被成功转译，但是对于 Array.prototype.includes 可以看到并没有被实现。

如果要实现低版本浏览器下这些新的 ES APi 或者静态/实例方法的话，就需要使用 polyfill 来处理了。

针对于`polyfill`方法的内容，babel实现 polyfill 有两种方式：

- `@babel/polyfill`
- `@babel/runtime`
- `@babel/plugin-transform-runtime`

## @babel/polyfill

首先我们来看看第一种实现`polyfill`的方式：@babel/polyfill

### @babel/polyfill是什么

通过[@babel/polyfill](https://www.babeljs.cn/docs/babel-polyfill)通过往全局对象上添加属性以及直接修改内置对象的`Prototype`上添加方法实现`polyfill`。

比如说我们需要支持`String.prototype.include`，在引入`@babel/polyfill`这个包之后，它会在全局`String`的原型对象上添加`include`方法从而支持我们的`Js Api`。

我们说到这种方式本质上是往全局对象/内置对象上挂载属性，所以这种方式难免会造成全局污染。

### 如何使用@babel/polyfill

在`@babel/preset-env`中存在一个`useBuiltIns`参数，它存在三种配置，这个参数决定了如何在`preset-env`中使用`@babel/polyfill`。

- `useBuiltIns`--`"usage"`| `"entry"`| `false`

```json
{
  "presets": [
    [
      "@babel/preset-env", 
      {
        "useBuiltIns": false
      }
    ]
  ]
}
```

#### false

useBuiltIns 默认值为 false ，也就是说不进行任何 polyfill 的转译。

当我们使用`preset-env`传入`useBuiltIns`参数时候，默认为`false`。它表示仅仅会转化最新的`ES`语法，并不会转化任何`Api`和方法。就和我们在刚才 Demo 中演示的那样，仅仅转译 JavasScript 语法而不处理对应的 API。

通常，如果我们不需要 `@babel/preset-env`为我们的代码增加 polyfill 的时候，可以配置为 false 关闭 `@babel/preset-env` 的 polyfil 。

#### entry

useBuiltIns 第二个值为 entry ，它表示在入口处引入 polyfill 进行处理。

需要我们在项目入口文件中手动引入一次`core-js`，它会根据我们配置的浏览器兼容性列表(`browserList`)然后**全量**引入不兼容的`polyfill`。

当使用 `useBuiltIns: 'entry'` 时需要额外在项目的入口文件引入这两个包。

- `import 'core-js/stable';`
- `import 'regenerator-runtime/runtime';`

> 在 `babel@7.4` 版本之前使用 entry 配置时需要在入口引入`@babel/polyfill`，在7.4 版本之后`@babel/polyfill`被废弃了，它变成另外两个包的集成。`"core-js/stable"; "regenerator-runtime/runtime";`，更换为这两个包。他们的使用方式是一致的，只是在入口文件中引入的包不同了。

```js
// 项目入口文件main.js
import 'core-js/stable';
import 'regenerator-runtime/runtime';

const arr = [1, 2, 3];
arr.forEach(item => {
	console.log('item: ' + item);
});

const flag = arr.includes(2);
console.log('flag=', flag);
```

配置文件babel.config.js修改为：

```json
module.exports = {
	presets: [
		[
			'@babel/preset-env',
			{
				useBuiltIns: 'entry',
				corejs: '3.6.5',
				targets: {
					browsers: '> 0.5%, ie >= 11',
				},
			},
		],
	],
}
```

关于 Babel 配置，这里主要有两点和大家强调一下：

- 首先，我们使用 `useBuiltIns: 'entry'` 配置。它表示我们告诉 Babel 我们需要使用 polyfill ，使用polyfill 的方式是在入口文件中引入 polyfill 。

- 其次关于 corejs: 3，我们使用了最新的 3 版本。corejs 版本2目前已经不被维护了，关于 corejs 的作用你可以将它理解称为上边我们讲到的 polyfill 中关于 ES API 以及内置模块的核心实现内容。换句话说，corejs 内部帮我们实现了一系列低版本浏览器不支持的 API 内置模块等方法。

> 需要注意的是，在我们使用`useBuiltIns:entry/usage`时，需要额外指定`core-js`这个参数。默认为使用`core-js 2.0`，所谓的`core-js`就是我们上文讲到的“垫片”的实现。它会实现一系列内置方法或者`Promise`等`Api`。

> `core-js 2.0`版本是跟随`preset-env`一起安装的，不需要单独安装哦～

执行编译命令npm run build编译文件

```js
"use strict";

...

require("core-js/modules/es.array.includes.js");

require("core-js/modules/es.array.iterator.js");

require("core-js/modules/es.array.join.js");

require("core-js/modules/es.array.map.js");
...

var arr = [1, 2, 3];
arr.forEach(function (item) {
  console.log('item: ' + item);
});
var flag = arr.includes(2);
console.log('flag=', flag); 
```

可以看到输出的打包结果其实有几百行代码。这是因为当我们使用 useage:entry 配置时，babel 会根据配置的 targets 浏览器兼容性列表来决定。从而将目标浏览器下不支持的内容在项目入口处进行全量引入，分别挂载在对应全局对象上从而达到 polyfill 的作用。

entry用法总结：

这种方式需要两个步骤的配置：

- 首先在 preset-env 中配置 useBuiltIns: 'entry' 。
- 其次需要在项目的入口文件中引入相应的包。

如此之后，Babel 就会根据我们配置需要支持的浏览器列表，将目标浏览器中不支持的 polyfill 进行全量引入并且实现。

#### usage

在明白了 entry 的含义之后，我们来看看 useBuiltIns 的另一种方法：usage。

针对于 `useBuiltIns:entry` 配置的方式，存在一个比较致命的问题：

上边我们说到配置为`entry`时，`perset-env`会基于我们的浏览器兼容列表进行全量引入`polyfill`。所谓的全量引入比如说我们代码中仅仅使用了`Array.from`这个方法。但是`polyfill`并不仅仅会引入`Array.from`，同时也会引入`Promise`、`Array.prototype.include`等其他并未使用到的方法。这就会造成包中引入的体积太大了。

针对这种情况，useBuiltIns 提供了另一种配置方式:usage。

当我们配置`useBuintIns:usage`时，会根据配置的浏览器兼容，以及代码中 **使用到的`Api` 进行引入`polyfill`按需添加。**

修改main.js文件如下：

```js
// 不需要在入口文件引入这两个库了
// import 'core-js/stable';
// import 'regenerator-runtime/runtime';

const arr = [1, 2, 3];
arr.forEach(item => {
	console.log('item: ' + item);
});

const flag = arr.includes(2);
console.log('flag=', flag);

```

修改配置文件babel.config.js为：

```json
module.exports = {
	presets: [
		[
			'@babel/preset-env',
			{
				useBuiltIns: 'usage',
				corejs: '3.6.5',
				targets: {
					browsers: '> 0.5%, ie >= 11',
				},
			},
		],
	],
}
```

执行编译命令npm run build编译文件

```js
"use strict";

require("core-js/modules/es.object.to-string.js");

require("core-js/modules/es.array.includes.js");

var arr = [1, 2, 3];
arr.forEach(function (item) {
  console.log('item: ' + item);
});
var flag = arr.includes(2);
console.log('flag=', flag);
```

此时可以看到，打包出来的代码仅仅剩下十几行行。这即是因为我们仅仅使用了 Array.prototype.includes 。

使用 usage 时，代码中仅仅会引入使用到的 Array.prototype.includes 对应的 polyfill 内容。

usage 用法总结：

我们可以看到配合 preset-env 的 usage 参数，Babel 会智能的分析源代码中使用到的内容。

**它仅仅会为我们引入目标浏览器中不支持并且我们在代码中使用到的内容，会剔除没有使用到的 polyfill 内容。**

针对于一些虽然目标浏览器不支持的内容，比如 Promise 但是这里我们代码中并没有使用，它即不会将相应的 polyfill 内容打包到最终结果中。相对于 entry 选项，usage 看起来更加智能化也更加的轻量。

#### usage和entry的区别。

我们以项目中引入`Promise`为例。

当我们配置`useBuintInts:entry`时，仅仅会在入口文件全量引入一次`polyfill`。你可以这样理解:

```js
// 当使用entry配置时
...
// 一系列实现polyfill的方法
global.Promise = promise

// 其他文件使用时
const a = new Promise()
```

而当我们使用`useBuintIns:usage`时，`preset-env`只能基于各个模块去分析它们使用到的`polyfill`从而进入引入。

`preset-env`会帮助我们智能化的在需要的地方引入，比如:

```js
// a. js 中
import "core-js/modules/es.promise";
...
```
```js
// b.js中

import "core-js/modules/es.promise";
...
```

- 在`usage`情况下，如果我们存在很多个模块，那么无疑会多出很多冗余代码(`import`语法)。
- 同样在使用`usage`时因为是模块内部局部引入`polyfill`所以按需在模块内进行引入，而`entry`则会在代码入口中一次性引入。

`usageBuintIns`不同参数分别有不同场景的适应度，具体参数使用场景还需要大家结合自己的项目实际情况找到最佳方式。

#### usage和entry的最佳实践

大多数文章都会告诉你 entry 这种方式一无是处，直接使用 usage 就好了。但实际并不是这样的。

首先 usage 的确更加轻量和智能化，但是假如这样的业务场景下：

通常，我们在使用 Babel 时会将 Babel 编译排除 node_modules 目录（第三方模块）。此时如果使用 usage 参数，如果我们依赖的一些第三方包中使用到了一些比较新的 ES 内置模块，比如它使用了Promise。但此时我们的代码中并没有使用 Promise ，如果使用 usage 参数那么问题就来了。

**代码中没有使用 Promise 自然 Promise 的 polyfill 并不会编译到最终的输出目录中，而第三种模块依赖了 Promise 但此时没有 Polyfill 浏览器并不认识这个东西。**

也许你会强调，那么我使用 babel 编译我的第三方模块呢，又或者我在入口处额外单独引入 Promise 的 polyfill 总可以吧。

首先，在入口文件中单独引入 Promise 是假设在已知前提下既是说我了解第三方库代码中使用 Promise 而我的代码中没有 Promise 我需要 polyfil。

这样的情况在多人合作的大型项目下只能说一种理想情况。

其次使用 Babel 编译第三方模块我个人是强烈不推荐的，抛开编译慢而且可能会造成重复编译造成体积过大的问题。

**这种情况下，使用 entry 配置它不香吗？**

**usage 相比 entry 固然更加轻量和智能，但是针对于业务场景下如果对于包体积没有强烈的要求下，我更加推荐你使用 entry 配合在入口处引入项目中所需 polyfill 因为这会避免很多第三方模块没有对应 polyfill 的造成的奇怪问题。**

### usage和entry存在的缺点

同时，上文我们讲到的所有都是针对于日常业务场景下的实践。

首先，在使用 useBuiltIns 配置开启 polyfil 之后的编译代码中，存在这样一段代码：

```js
$({
  target: 'Array ',
  proto: true
}, {
  includes: function includes (el
    /* , fromIndex =0*/
  ) {
    return $includes(this, el, arguments.length > 1 ? arguments[1] : undefined);
  }
});
```

这个函数本质上做的内容你可以将它简单理解成为：

```js
Object.defineProperty(Array.prototype,'includes', {
    value: // ...polyfill 实现的函数
})
```

**无论是 entry 还是 usage 本质上都是通过注入浏览器不支持的 polyfill 在对应的全局对象上增加功能实现，这样无疑是会污染全局环境。**

假如此时你在开发一款类库，使用 useBuiltIns 在库的编译上实现 polyfill ，当你开发完毕提交 Github 看似大功告成时。

此时一位印度小哥使用了你的库，但小哥在自己的代码中重新定义了 Array.prototype.includes 方法的实现，额外填充了一些逻辑。

OK，小哥安装你开发的库。运行代码，不出意外代码报错了。因为你库中的 Array.prototype.includes polyfill 实现污染了全局，影响了印度小哥自己定义的代码。

此时，印度小哥熟练的打开 Github 在你库 issue 上用它蹩脚的英语慰问你全家....

此时，在开发类库时我们迫切的需要一种并不会污染全局的 polyfill ，@babel/runtime 的出现拯救了我于水深火热之中。

## @babel/runtime

`@babel/polyfill`是存在污染全局变量的副作用，在实现`polyfill`时`Babel`还提供了另外一种方式去让我们实现这功能，那就是`@babel/runtime`。

简单来说 @babel/runtime 提供了一种不污染全局作用域的 polyfill 的方式，但是不不够智能需要我们自己在代码中手动引入相关 polyfill 对应的包。，`@babel/runtime`更像是一种**按需加载的解决方案**，比如哪里需要使用到`Promise`，`@babel/runtime`就会在他的文件顶部添加`import promise from 'babel-runtime/core-js/promise'`。

同时上边我们讲到对于`preset-env`的`useBuintIns`配置项，我们的`polyfill`是`preset-env`帮我们智能引入。而`babel-runtime`则会将引入方式由智能完全交由我们自己，我们需要什么自己引入什么。

它的用法很简单，只要我们去安装`npm install --save @babel/runtime`后，在需要使用对应的`polyfill`的地方去单独引入就可以了。比如：

```js
// a.js 中需要使用Promise 我们需要手动引入对应的运行时polyfill
import Promise from 'babel-runtime/core-js/promise'

const promsies = new Promise()
```

总而言之，`babel/runtime`你可以理解称为就是一个运行时“哪里需要引哪里”的工具库。

> 针对`babel/runtime`绝大多数情况下我们都会配合`@babel/plugin-transfrom-runtime`进行使用达到智能化`runtime`的`polyfill`引入。

## @babel/plugin-transform-runtime

### `babel-runtime`存在的问题

`babel-runtime`在我们手动引入一些`polyfill`的时候，它会给我们的代码中注入一些类似`_extend()， classCallCheck()`之类的工具函数，这些工具函数的代码会包含在编译后的每个文件中，比如：

```js
class Circle {}
// babel-runtime 编译Class需要借助_classCallCheck这个工具函数
function _classCallCheck(instance, Constructor) { //... } 
var Circle = function Circle() { _classCallCheck(this, Circle); };
```

如果我们项目中存在多个文件使用了`class`，那么无疑在每个文件中注入这样一段冗余重复的工具函数将是一种灾难。

所以针对上述提到的两个问题:

- `babel-runtime`无法做到智能化分析，需要我们手动引入。
- `babel-runtime`编译过程中会重复生成冗余代码。

我们就要引入我们的主角`@babel/plugin-transform-runtime`。

### 什么是`@babel/plugin-transform-runtime`

@babel/plugin-transform-runtime 这个插件正式基于 @babel/runtime 可以更加智能化的分析我们的代码，同时 @babel/plugin-transform-runtime 支持一个 helper 参数默认为 true 它会提取 @babel/runtime 编译过程中一些重复的工具函数变成外部模块引入的方式。

### `@babel/plugin-transform-runtime`的作用

`@babel/plugin-transform-runtime`插件的作用恰恰就是为了解决上述我们提到的`run-time`存在的问题而提出的插件。

- `babel-runtime`无法做到智能化分析，需要我们手动引入。

`@babel/plugin-transform-runtime`插件会智能化的分析我们的项目中所使用到需要转译的`js`代码，从而实现模块化从`babel-runtime`中引入所需的`polyfill`实现。

- `babel-runtime`编译过程中会重复生成冗余代码。

`@babel/plugin-transform-runtime`插件提供了一个`helpers`参数，详见[helpers文档](https://www.babeljs.cn/docs/babel-plugin-transform-runtime#helpers)。

> helper：比如 Babel 使用 _extend 函数把一个对象的属性复制到另一个对象上，这里的 _extend 就是 helper

这个`helpers`参数开启后可以将上边提到编译阶段重复的工具函数，比如`classCallCheck, extends`等代码转化称为`require`语句。此时，这些工具函数就不会重复的出现在使用中的模块中了。比如这样：

```js
// @babel/plugin-transform-runtime会将工具函数转化为require语句进行引入
// 而非runtime那样直接将工具模块代码注入到模块中
var _classCallCheck = require("@babel/runtime/helpers/classCallCheck"); 
var Circle = function Circle() { _classCallCheck(this, Circle); };
```

### 如何使用`@babel/plugin-transform-runtime`

安装下载模块

```shell
npm install --save-dev @babel/plugin-transform-runtime
```

并[`@babel/runtime`](https://www.babeljs.cn/docs/babel-runtime)作为生产依赖项（因为它用于“运行时”）。

```sh
npm install --save @babel/runtime
```

| `corejs`选项 | 安装命令                                    |
| ------------ | ------------------------------------------- |
| `false`      | `npm install --save @babel/runtime`         |
| `2`          | `npm install --save @babel/runtime-corejs2` |
| `3`          | `npm install --save @babel/runtime-corejs3` |

修改配置文件babel.config.js为：

```json
module.exports = {
	presets: [
		[
			'@babel/preset-env',
      {
        // 其实默认就是false，这里我为了和大家刻意强调不要混在一起使用
        useBuiltIns: false,
      },
		],
	],
    "plugins": [
    [
      "@babel/plugin-transform-runtime",
      {
        absoluteRuntime: false,
        // polyfill使用的corejs版本
        // 需要注意这里是@babel/runtime-corejs3 和 preset-env 中是不同的 npm 包
        corejs: 3,
        // 切换对于 @babel/runtime 造成重复的 _extend() 之类的工具函数提取
        // 默认为true 表示将这些工具函数抽离成为工具包引入而不必在每个模块中单独定义
        helpers: true,
        // 切换生成器函数是否污染全局 
        // 为true时打包体积会稍微有些大 但生成器函数并不会污染全局作用域
        regenerator: true,
        version: '7.0.0',
      }
    ]
  ]
}
```

执行编译命令npm run build编译文件

```js
var _interopRequireDefault = require("@babel/runtime-corejs3/helpers/interopRequireDefault")["default"];

var _forEach = _interopRequireDefault(require("@babel/runtime-corejs3/core-js-stable/instance/for-each"));

var _includes = _interopRequireDefault(require("@babel/runtime-corejs3/core-js-stable/instance/includes"));

// import 'core-js/stable';
// import 'regenerator-runtime/runtime';
var arr = [1, 2, 3];
(0, _forEach["default"])(arr).call(arr, function (item) {
  console.log('item: ' + item);
});
var flag = (0, _includes["default"])(arr).call(arr, 2);
console.log('flag=', flag);
var str = 'hello world!';
console.log((0, _includes["default"])(str).call(str, 'l'));
```

同样相对于 @babel/plugin-transform-runtime 配合 @babel/runtime 使用的话，他们会智能的分析我们代码。

仅仅抽离保留我们代码中使用到的新语法提供 polyfill 实现，但是 @babel/runtime 相对于 usage 而言还存在一个更加值得注意的点**它并不会污染全局作用域**。

可以看到最终打包后的代码，之前 usage 是使用 `$({target:'Array',proto:true}) ...` 直接在 Array.prototype 上定义对应的 polyfill 实现。

而 @babel/runtime 打包后的结果可以明显的看到我们是借助引入的 `_includes` 方来调用的。

## 总结polyfill

在`babel`中实现`polyfill`主要有两种方式：

- 一种是通过`@babel/polyfill`配合`preset-env`去使用，这种方式可能会存在污染全局作用域。
- 一种是通过`@babel/runtime`配合`@babel/plugin-transform-runtime`去使用，这种方式并不会污染作用域。

全局引入会污染全局作用域，但是相对于局部引入来说。它会增加很多额外的引入语句，增加包体积。

在`useBuintIns:usage`情况下其实和`@babel/plugin-transform-runtime`情况下是类似的作用，

通常我个人选择是会在开发类库时遵守不污染全局为首先使用`@babel/plugin-transform-runtime`，而在业务开发中使用`@babel/polyfill`。

- babel-runtime 是为了减少重复代码而生的。 babel生成的代码，可能会用到一些_extend()， classCallCheck() 之类的工具函数，默认情况下，这些工具函数的代码会包含在编译后的文件中。如果存在多个文件，那每个文件都有可能含有一份重复的代码。

- babel-runtime插件能够将这些工具函数的代码转换成require语句，指向为对babel-runtime的引用，如 require('babel-runtime/helpers/classCallCheck'). 这样， classCallCheck的代码就不需要在每个文件中都存在了。

## @babel/runtime 为什么不适合业务项目

说了这么多，那么@babel/plugin-transform-runtime真的如我们想象中的那么完美无瑕嘛？

@babel/runtime 配合 @babel/plugin-transform-runtime 的确可以解决 usage 污染全局作用域的问题，使用它来开发类库看起来非常完美。

有些小伙伴可能就会想到，既然它提供和 usage 一样的智能化按需引入同时还不会污染全局作用域。那么，为什么我不能直接在业务项目中直接使用 @babel/runtime ，这样岂不是更好吗？

答案肯定是否定的，任何事情都存在它的两面性。

**需要额外注意的是 transform runtime 与环境无关，它并不会因为我们的页面的目标浏览器动态调整 polyfill 的内容，而 useBuiltIns 则会根据配置的目标浏览器而决定是否需要引入相应的 polyfill。**

# 探索最佳实践方案

首先，任何配置项目都有它自己存在的意义。笔者个人认为所谓最佳配置实践方案并不是指某一种固定配置，而是说**结合不同的业务场景下寻找最适合的配置落地方案。**

@babel/runtime && @babel/preset-env 这两种其实完全是为不同场景下设计的 polyfill 解决方案，

## 业务

在日常业务开发中，对于全局环境污染的问题往往并不是那么重要。而业务代码最终的承载大部分是浏览器端，所以如果针对不同的目标浏览器支持度从而引入相应的 polyfill 无疑会对我们的代码体积产生非常大的影响，此时选择 preset-env 开启 useBuiltIns 的方式会更好一些。

**所以简单来讲，我个人强烈推荐在日常业务中尽可能使用 @babel/preset-env 的 useBuiltIns 配置配合需要支持的浏览器兼容性来提供 polyfill 。**

同时关于业务项目中究竟应该选择 useBuiltIns 中的 entry 还是 usage ，我在上边已经和大家详细对比过这两种方式。究竟应该如何选择这两种配置方案，在不同的业务场景下希望大家可以根据场景来选择最佳方案。而不是一概的认为 entry 无用无脑使用 usage 。

## 类库

在我们开发类库时往往和浏览器环境无关所以在进行 polyfill 时最主要考虑的应该是不污染全局环境，此时选择 @babel/runtime 无疑更加合适。

**在类库开发中仅仅开启 @babel/preset-env 的语法转化功能配合 @babel/runtime 提供一种不污染全局环境的 polyfill 可能会更加适合你的项目场景。**

## Tips

关于提供 polyfill 的方法，`useBuiltIns`并且`@babel/plugin-transform-runtime`是互斥的。两者都用于添加 polyfill：第一个全局添加它们，第二个添加它们而不将它们附加到全局范围。

**我强烈建议大家不要同时开启两种polyfill，这两个东西完全是 Babel 提供给不同场景下的不同解决方案**。

它不仅会造成重复打包的问题还有可能在某些环境下直接造成异常，具体你可以参考这个 [Issue](https://github.com/babel/babel/issues/10271)。

**当然，你同样可以在业务项目中配合 @babel/preset-env 的 polyfill 同时使用 @babel/plugin-transform-runtime 的 helper 参数来解决多个模块内重复定义工具函数造成冗余的问题。**

但是切记设置 runtime 的 `corejs:false` 选项，关闭 runtime 提供的 polyfill 的功能，仅保留一种 polyfill 提供方案。

最后，无论是哪一种 polyfill 的方式，我强烈推荐你使用 corejs@3 版本来提供 polyfill。

配置一： `@babel/preset-env` + `@babel/runtime` + `core-js@3`

安装脚本：

```shell
npm install --save-dev @babel/cli @babel/core @babel/preset-env @babel/plugin-transform-runtime
npm install --save @babel/runtime core-js@3
```

babel.config.json 配置文件

```js
module.exports = {  
  "presets": [
    [
      "@babel/preset-env", {
        "useBuiltIns": "usage",
        "corejs": 3
      }
    ]
  ],
  "plugins": [
    ["@babel/plugin-transform-runtime"]
  ]
};
```

配置二： `@babel/preset-env` + `@babel/runtime-corejs3`

安装脚本：

```
npm install --save-dev @babel/cli @babel/core @babel/preset-env @babel/plugin-transform-runtime
npm install --save @babel/runtime-corejs3
```

babel.config.json 配置文件

```js
module.exports = {  
  "presets": [
    [
      "@babel/preset-env"
    ]
  ],
  "plugins": [
    [
      "@babel/plugin-transform-runtime", {
        "corejs": 3
      }
    ]
  ]
};
```

# 参考

- [babel使用指南](https://www.babeljs.cn/docs/)
- [「前端基建」探索不同项目场景下Babel最佳实践方案](https://juejin.cn/post/7051355444341637128)
- [「前端基建」带你在Babel的世界中畅游](https://juejin.cn/post/7025237833543581732)
- [不容错过的 Babel7 知识](https://juejin.cn/post/6844904008679686152)
