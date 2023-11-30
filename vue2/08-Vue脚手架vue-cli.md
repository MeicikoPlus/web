# Vue CLI

## 简介

Vue脚手架（Vue CLI）是Vue.js 开发的标准工具，是一个基于 Vue.js 进行快速开发的完整系统。

官网：https://cli.vuejs.org/zh/

Vue脚手架是vue官方提供的vue脚手架工具，能够创建、测试、构建、调试vue项目。用来帮助程序员快速创建一个基于vue库的模板项目：

- 通过 `@vue/cli` 实现的交互式的项目脚手架。
- 通过 `@vue/cli` + `@vue/cli-service-global` 实现的零配置原型开发。
- 一个运行时依赖 (`@vue/cli-service`)，该依赖：
  - 可升级；
  - 基于 webpack 构建，并带有合理的默认配置；
  - 可以通过项目内的配置文件进行配置；
  - 可以通过插件进行扩展。
- 一个丰富的官方插件集合，集成了前端生态中最好的工具。
- 一套完全图形化的创建和管理 Vue.js 项目的用户界面。

Vue CLI 致力于将 Vue 生态中的工具基础标准化。它确保了各种构建工具能够基于智能的默认配置即可平稳衔接，这样你可以专注在撰写应用上，而不必花好几天去纠结配置的问题。与此同时，它也为每个工具提供了调整配置的灵活性，无需 eject。

使用脚手架开发的项目的特点: 模块化， 组件化，工程化。

## 安装vue-cli

> Node 版本要求
>
> Vue CLI 4.x 需要 [Node.js](https://nodejs.org/) v8.9 或更高版本 (推荐 v10 以上)。你可以使用 [n](https://github.com/tj/n)，[nvm](https://github.com/creationix/nvm) 或 [nvm-windows](https://github.com/coreybutler/nvm-windows) 在同一台电脑中管理多个 Node 版本。

```bash
npm install -g @vue/cli
```

安装之后，你就可以在命令行中访问 `vue` 命令。你可以通过简单运行 `vue`，看看是否展示出了一份所有可用命令的帮助信息，来验证它是否安装成功。

你还可以用这个命令来检查其版本是否正确：

```bash
vue --version
```

## 创建项目

命令行创建项目：

```bash
vue create 项目名称       
```

也可以使用vue的界面化工具创建项目：

```bash
vue ui
```

## 启动项目

在开发环境启动项目，开发环境也会使用webpack打包、编译项目，打包的输出是在内存，适合开发项目的时候使用：

 ```bash
 npm run serve           
 ```

npm run serve  会把项目打包在内存中，并可以使用指定的端口号，默认是8080，在浏览器的地址栏访问：http://127.0.0.1:8080 打开项目。

## 打包项目

在生产环境下使用webpack编译、打包项目，打包的文件会输出到dist文件中，一般是项目开发完毕 或者 阶段性功能完毕打包输出文件，发布上线

```bash
npm run build 				
```

运行dist 文件夹中的index.html，不能直接双击打开index.html，需要把dist文件夹中所有的文件放到服务器静态目录下，可以使用live server插件、anywhere、nodejs 的public目录。

- 开发过程中使用 npm run serve，开发调试比较方便
- 开发完毕项目打包上线，使用 npm run build ，最终是把dist文件中的内容放到服务器的静态目录

## 项目结构

```
├── node_modules 
├── public
│   ├── favicon.ico: 页签图标
│   └── index.html: 主页面
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── components: 存放组件
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件
│   │── main.js: 入口文件
├── .gitignore: git版本管制忽略的配置
├── babel.config.js: babel的配置文件
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
├── package-lock.json：包版本控制文件
```

# 对Vue不同构建版本的解释

当以无构建步骤方式使用 Vue 时，组件模板要么是写在页面的 HTML 中，或者是内联的 JavaScript 字符串。在这些场景中，为了执行动态模板编译，Vue 需要将模板编译器运行在浏览器中。相对的，如果我们使用了构建步骤，由于提前编译了模板，那么就无须再在浏览器中运行了。为了减小打包出的客户端代码体积，Vue 提供了[多种格式的“构建文件”](https://unpkg.com/browse/vue@2.6.14/dist/)以适配不同场景下的优化需求。

- **只包含运行时版**：前缀为 `vue.runtime.*` 的文件是**只包含运行时的版本**：`vue.esm.js`：只包含**核心代码**，不包含编译器，当使用这个版本时，所有的模板都必须由构建步骤预先编译。
- **完整版**：名称中不包含 `.runtime` 的文件则是**完全版**：`vue.runtime.esm.js`：包含**核心代码**和**编译器**，并支持在浏览器中直接编译模板。当然，体积也会因此增长不少。

默认的工具链中都会使用仅含运行时的版本，因为所有 SFC 中的模板都已经被预编译了。如果因为某些原因，在有构建步骤时，你仍需要浏览器内的模板编译，你可以更改构建工具配置，将 `vue` 改为相应的版本 `vue/dist/vue.esm-bundler.js`。

## 完整的版本信息

在 [NPM 包的 `dist/` 目录](https://unpkg.com/browse/vue@2.6.14/dist/)你将会找到很多不同的 Vue.js 构建版本。这里列出了它们之间的差别：

|                               | UMD                | ES Module (基于构建工具使用) | ES Module (直接用于浏览器) |
| :---------------------------- | :----------------- | :--------------------------- | :------------------------- |
| **完整版**                    | vue.js             | vue.esm.js                   | vue.esm.browser.js         |
| **只包含运行时版**            | vue.runtime.js     | vue.runtime.esm.js           | -                          |
| **完整版 (生产环境)**         | vue.min.js         | -                            | vue.esm.browser.min.js     |
| **只包含运行时版 (生产环境)** | vue.runtime.min.js | -                            | -                          |

## 术语

- **完整版**：同时包含**编译器**和运行时的版本。
- **编译器**：用来将模板字符串编译成为 JavaScript 渲染函数的代码。
- **运行时**：用来创建 Vue 实例、渲染并处理虚拟 DOM 等的核心代码。基本上就是除去编译器的其它一切。
- **UMD**：[UMD](https://github.com/umdjs/umd)（Universal Module Definition通用模块定义） 版本可以通过 `<script>` 标签直接用在浏览器中。jsDelivr CDN 的 https://cdn.jsdelivr.net/npm/vue@2.7.10 默认文件就是运行时 + 编译器的 UMD 版本 (`vue.js`)。
- **ES Module (基于构建工具使用)**：为打包工具提供的 ESM：为webpack提供的现代打包工具。ESM 格式被设计为可以被静态分析，所以打包工具可以利用这一点来进行“tree-shaking”并将用不到的代码排除出最终的包。为这些打包工具提供的默认文件 (`pkg.module`) 是只有运行时的 ES Module 构建 (`vue.runtime.esm.js`)。
- **ES Module (直接用于浏览器)**：为浏览器提供的 ESM (2.6+)：用于在现代浏览器中通过 `<script type="module">` 直接导入（  `vue.esm.browser.js`）。

## 运行时 + 编译器 vs. 只包含运行时

如果你需要在客户端编译模板 (比如传入一个字符串给 `template` 选项，或挂载到一个元素上并以其 DOM 内部的 HTML 作为模板)，就将需要加上编译器，即完整版：

- vue.js是完整版的Vue：包含 核心代码 + 模板编译器，可以使用使用template配置项

- vue.runtime.esm.js是运行版的Vue：只包含 核心功能，没有模板编译器，所以不能使用template配置项，需要使用render函数接收到的h函数创建组件

```js
// 运行时 + 编译器：需要编译器 vue.js
new Vue({
  template: '<div>{{ hi }}</div>'
})

// 只包含运行时：不需要编译器 vue.runtime.esm.js
new Vue({
  render (h) {
    return h('div', this.hi)
  }
})
```

## 开发环境 vs. 生产环境模式

对于 UMD 版本来说，开发环境/生产环境模式是硬编码好的：开发环境下用未压缩的代码，生产环境下使用压缩后的代码。

CommonJS 和 ES Module 版本是用于打包工具的，因此我们不提供压缩后的版本。你需要自行将最终的包进行压缩。

CommonJS 和 ES Module 版本同时保留原始的 `process.env.NODE_ENV` 检测，以决定它们应该运行在什么模式下。你应该使用适当的打包工具配置来替换这些环境变量以便控制 Vue 所运行的模式。把 `process.env.NODE_ENV` 替换为字符串字面量同时可以让 UglifyJS 之类的压缩工具完全丢掉仅供开发环境的代码块，以减少最终的文件尺寸。

在 webpack 4+ 中，你可以使用 `mode` 选项：

```js
module.exports = {
  mode: 'production'
}
```

# 单文件组件

## 为什么需要单文件组件

在很多 Vue 项目中，我们使用 `Vue.component` 来定义全局组件，紧接着用 `new Vue({ el: '#app'})` 在每个页面内指定一个容器元素。

这种方式在很多中小规模的项目中运作的很好，在这些项目里 JavaScript 只被用来加强特定的视图。但当在更复杂的项目中，或者你的前端完全由 JavaScript 驱动的时候，下面这些缺点将变得非常明显：

- **全局定义 (Global definitions)** 强制要求每个 component 中的命名不得重复
- **字符串模板 (String templates)** 缺乏语法高亮，在 HTML 有多行的时候，需要用到丑陋的 `\`
- **不支持 CSS (No CSS support)** 意味着当 HTML 和 JavaScript 组件化时，CSS 明显被遗漏
- **没有构建步骤 (No build step)** 限制只能使用 HTML 和 ES5 JavaScript，而不能使用预处理器，如 Pug (formerly Jade) 和 Babel

## 什么是单文件组件 

一个 Vue 单文件组件 (英文 Single-File Component，简称 **SFC**)，通常使用 `*.vue` 作为文件扩展名，它是一种使用了类似 HTML 语法的自定义文件格式，用于定义 Vue 组件。`.vue`是一种特殊的文件格式，使我们能够将一个 Vue 组件的模板HTML、逻辑JS与样式CSS封装在单个文件中。

每一个 `*.vue` 文件都由三种顶层语言块构成：`<template>`、`<script>` 和 `<style>`，以及一些其他的自定义块。`<template>`、`<script>` 和 `<style>` 三个块在同一个文件中封装、组合了组件的视图、逻辑和样式。

- `<template>`：用来写html代码，是组件的模板
- `<script>` ：写js代码，需要导出本组件的组件配置对象，在配置对象中定义这个组件中所需要的数据
- `<style>`：写css样式，设置本组件模板中元素的样式

下面是一个单文件组件的示例：

```vue
<template>
	<div id="app">
		<Hello></Hello>
	</div>
</template>

<script>
import Hello from './components/Hello.vue';

export default {
	name: 'App',
	components: {
		Hello,
	},
};
</script>

<style lang="scss" scoped>
  #app{
    background-color: '#eee';
  }
</style>
```

script标签中是导出组件的配置对象options，是下面的简写

```vue
<script>
const App = Vue.extend({
	name: 'App',
	components: {},
});
export default App;
</script>
```

### `<template>`

- 每个 `*.vue` 文件最多可以包含一个顶层 `<template>` 块。
- 语块包裹的内容将会被提取、传递给 `@vue/compiler-dom`，预编译为 JavaScript 渲染函数，并附在导出的组件上作为其 `render` 选项。

### `<script>`

- 每个 `*.vue` 文件最多可以包含一个 `<script>` 块。
- 这个脚本代码块将作为 ES 模块执行。
- **默认导出**应该是 Vue 的组件选项对象，可以是一个对象字面量或是 [defineComponent](https://cn.vuejs.org/api/general.html#definecomponent) 函数的返回值。

### `<style>`

- 每个 `*.vue` 文件可以包含多个 `<style>` 标签。
- 一个 `<style>` 标签可以使用 `scoped` 或 `module` attribute来帮助封装当前组件的样式。使用了不同封装模式的多个 `<style>` 标签可以被混合入同一个组件。

## 自动名称推导

SFC 在以下场景中会根据**文件名**自动推导其组件名：

- 开发警告信息中需要格式化组件名时；
- DevTools 中观察组件时；
- 递归组件自引用时。例如一个名为 `FooBar.vue` 的组件可以在模板中通过 `<FooBar/>` 引用自己。(同名情况下) 这比明确注册/导入的组件优先级低。

## 预处理器

代码块可以使用 `lang` 这个 attribute 来声明预处理器语言，最常见的用例就是在 `<script>` 中使用 TypeScript：

```vue
<script lang="ts">
  // use TypeScript
</script>
```

`lang` 在任意块上都能使用，比如我们可以在 `<style>` 标签中使用 [SASS](https://sass-lang.com/) 或是 `<template>` 中使用 [Pug](https://pugjs.org/api/getting-started.html)：

```vue
<template lang="pug">
p {{ msg }}
</template>

<style lang="scss">
  $primary-color: #333;
  body {
    color: $primary-color;
  }
</style>
```

## Src 导入

如果你更喜欢将 `*.vue` 组件分散到多个文件中，可以为一个语块使用 `src` 这个 attribute 来导入一个外部文件：

```vue
<template src="./template.html"></template>
<style src="./style.css"></style>
<script src="./script.js"></script>
```

请注意 `src` 导入和 JS 模块导入遵循相同的路径解析规则，这意味着：

- 相对路径需要以 `./` 开头
- 你也可以从 npm 依赖中导入资源

```vue
<!-- 从所安装的 "todomvc-app-css" npm 包中导入一个文件 -->
<style src="todomvc-app-css/index.css" />
```

## 注释

在每一个语块中你都可以按照相应语言 (HTML、CSS、JavaScript 和 Pug 等等) 的语法书写注释。对于顶层注释，请使用 HTML 的注释语法 `<!-- comment contents here -->`

## 快捷方式

```json
"Vue单文件组件模板": {
  "prefix": "govue",
  "body": [
    "<template>",
    "\t<div class=\"$1\">",
    "\t\t",
    "\t</div>",
    "</template>",
    "",
    "<script>",
    "export default {",
    "\tname:'$1'",
    "}",
    "</script>",
    "",
    "<style scoped lang=\"scss\">",
    "",
    "</style>",
  ],
  "description": "Vue单文件组件模板"
}
```

# 风格指南

本项目的风格指南主要是参照 `vue` 官方的[风格指南](https://cn.vuejs.org/v2/style-guide/index.html)。在真正开始使用该项目之前建议先阅读一遍指南，这能帮助让你写出更规范和统一的代码。当然每个团队都会有所区别。其中大部分规则也都配置在了[eslint-plugin-vue](https://github.com/vuejs/eslint-plugin-vue)之中，当没有遵循规则的时候会报错。

当然也有一些特殊的规范，是不能被 eslint 校验的。需要人为的自己注意，并且来遵循。最主要的就是文件的命名规则。

## Component

所有的`Component`文件都是以大写开头 (PascalCase)，这也是官方所[推荐的](https://cn.vuejs.org/v2/style-guide/index.html#单文件组件文件的大小写-强烈推荐)。

但除了 `index.vue`。

例子：

- `@/components/BackToTop/index.vue`
- `@/components/Charts/Line.vue`
- `@/views/search/components/Item.vue`

## JS 文件

所有的`.js`文件都遵循横线连接 (kebab-case)。

例子：

- `@/utils/open-window.js`
- `@/views/svg-icons/require-icons.js`
- `@/components/MarkdownEditor/default-options.js`

## Views

在`views`文件下，代表路由的`.vue`文件都使用横线连接 (kebab-case)，代表路由的文件夹也是使用同样的规则。

例子：

- `@/views/svg-icons/index.vue`
- `@/views/svg-icons/require-icons.js`

使用横线连接 (kebab-case)来命名`views`主要是出于以下几个考虑。

- 横线连接 (kebab-case) 也是官方推荐的命名规范之一 [文档](https://cn.vuejs.org/v2/style-guide/index.html#单文件组件文件的大小写-强烈推荐)
- `views`下的`.vue`文件代表的是一个路由，所以它需要和`component`进行区分(component 都是大写开头)
- 页面的`url` 也都是横线连接的，比如`https://www.xxx.admin/export-excel`，所以路由对应的`view`应该要保持统一
- 没有大小写敏感问题

# HTML 和静态资源

## HTML

### Index 文件

`public/index.html` 文件是一个会被 [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin) 处理的模板。在构建过程中，资源链接会被自动注入。

### 插值

因为 index 文件被用作模板，所以你可以使用 [lodash template](https://lodash.com/docs/4.17.10#template) 语法插入内容：

- `<%= VALUE %>` 用来做不转义插值；
- `<%- VALUE %>` 用来做 HTML 转义插值；
- `<% expression %>` 用来描述 JavaScript 流程控制。

除了[被 `html-webpack-plugin` 暴露的默认值](https://github.com/jantimon/html-webpack-plugin#writing-your-own-templates)之外，所有[客户端环境变量](https://cli.vuejs.org/zh/guide/mode-and-env.html#using-env-variables-in-client-side-code)也可以直接使用。例如，`BASE_URL` 的用法：

```html
<link rel="icon" href="<%= BASE_URL %>favicon.ico">
```

## 处理静态资源

静态资源可以通过两种方式进行处理：

- 在 JavaScript 被导入或在 template/CSS 中通过相对路径被引用。这类引用会被 webpack 处理。
- 放置在 `public` 目录下或通过绝对路径被引用。这类资源将会直接被拷贝，而不会经过 webpack 的处理。

### 从相对路径导入

当你在 JavaScript、CSS 或 `*.vue` 文件中使用相对路径 (必须以 `.` 开头) 引用一个静态资源时，该资源将会被包含进入 webpack 的依赖图中。在其编译过程中，所有诸如 `<img src="...">`、`background: url(...)` 和 CSS `@import` 的资源 URL **都会被解析为一个模块依赖**。

例如，`url(./image.png)` 会被翻译为 `require('./image.png')`，而：

```html
<img src="./image.png">
```

将会被编译到：

```js
h('img', { attrs: { src: require('./image.png') }})
```

在其内部，我们通过 webpack 的 [Assets Modules](https://webpack.js.org/guides/asset-modules/) 配置，用版本哈希值和正确的公共基础路径来决定最终的文件路径，并将小于 8KiB 的资源内联，以减少 HTTP 请求的数量。

### URL 转换规则

- 如果 URL 是一个绝对路径 (例如 `/images/foo.png`)，会原样保留。

- 如果 URL 以 `.` 开头，将会被看作相对的模块依赖，并按照你的本地文件系统上的目录结构进行解析。

- 如果 URL 以 `~` 开头，其后的部分将会被看作模块依赖。这意味着你可以用该特性来引用一个 Node 依赖中的资源：

  ```html
  <img src="~some-npm-package/foo.png">
  ```

- 如果 URL 以 `@` 开头，也会被看作模块依赖。如果你的 webpack 配置中给 `@` 配置了 alias，这就很有用了。所有 `vue-cli` 创建的项目都默认配置了将 `@` 指向 `<projectRoot>/src`。 **(仅作用于模版中)**

### `public` 文件夹

任何放置在 `public` 文件夹的静态资源都会被简单的复制，而不经过 webpack。你需要通过绝对路径来引用它们。

注意我们推荐将资源作为你的模块依赖图的一部分导入，这样它们会通过 webpack 的处理并获得如下好处：

- 脚本和样式表会被压缩且打包在一起，从而避免额外的网络请求。
- 文件丢失会直接在编译时报错，而不是到了用户端才产生 404 错误。
- 最终生成的文件名包含了内容哈希，因此你不必担心浏览器会缓存它们的老版本。

`public` 目录提供的是一个**应急手段**，当你通过绝对路径引用它时，留意应用将会部署到哪里。如果你的应用没有部署在域名的根部，那么你需要为你的 URL 配置 [publicPath](https://cli.vuejs.org/zh/config/#publicpath) 前缀：

- 在 `public/index.html` 或其它通过 `html-webpack-plugin` 用作模板的 HTML 文件中，你需要通过 `<%= BASE_URL %>` 设置链接前缀：

  ```html
  <link rel="icon" href="<%= BASE_URL %>favicon.ico">
  ```

- 在模板中，你首先需要向你的组件传入基础 URL：

  ```js
  data () {
    return {
      publicPath: process.env.BASE_URL
    }
  }
  ```

  然后：

  ```js
  <img :src="`${publicPath}my-image.png`">
  ```

### 何时使用 `public` 文件夹

- 你需要在构建输出中指定一个文件的名字。
- 你有上千个图片，需要动态引用它们的路径。
- 有些库可能和 webpack 不兼容，这时你除了将其用一个独立的 `<script>` 标签引入没有别的选择。

# 使用图片资源

执行 npm run serve 会把vue项目在内存中打包。

执行 npm run build 会把vue项目打包到dist文件内。

图片资源要放在 assets 或者 pubilc 中。

## 放在public中

放在pubilc中，打包的时候会直接复制pubilc中的图片，放在pubilc中的图片相当于放在服务器根目录的资源

- 在组件中使用：`<template>`、`<script>`都是使用 /开头，比如 /img/icon-home.png

```vue
<template>
	<div>
   	<img src="/img/head.png" alt="" />
    <img :src="imgUrl" alt="" />
    <img :src="`${publicPath}img/head.png`">
    
    <div class="box" style="background-image:url('/img/head.png');">box</div>
    <div class="box" :style="{background:'url('+ imgUrl +')'}">box</div>
  </div>
</template>

<script>
  // 在模板中，你首先需要向你的组件传入基础 URL：
export default {
  data () {
    return {
      imgUrl:'/img/head.png',
      // process.env 属性返回的是一个包含用户环境信息的对象
      // process.env.BASE_URL  获取基础路径
      // process.env.NODE_ENV 获取开发环境
      publicPath: process.env.BASE_URL
    }
  }
}

</script>
```

## 放在assets中

assets中，打包的时候会把图片经过webpack处理，把图片当做一个模块

- 在`<template>`中使用，可以使用 ./ 或者 ../ 开头的路径，比如：./img/home.png
- 在`<script>`中，可以使用 import 导入的方式，导入之后需要在data或者computed中定义变量接收
- 在`<style>`中使用，可以使用 ./ 或者 ../ 开头的路径，比如：./img/home.png

路径别名@：@ 在vue-cli中是指向 `<projectRoot>/src`的别名

```vue
<template>
	<div>
    
    <img alt="Vue logo" src="@/assets/logo.png" />
    <img alt="Vue logo" src="./assets/logo.png" />
    <img :src="imgurl" alt="" />
    <img :src="img" alt="" />
   	
    <div :style="{background:'url('+imgurl+')'}">box2</div>
  </div>
</template>

<script>
import timg from '@/assets/img/timg.jpg'; //导入图片
export default {
	name: 'App',
	data() {
		return {
			imgurl: timg,
      img: require('@/assets/logo.png')
		};
	},
}
</script>


<style>
  div{
    background-image: url('./assets/img/timg.jpg');
  }
</style>
```

# 使用CSS

## 引用静态CSS资源

- 所有编译后的 CSS 都会通过 [css-loader](https://github.com/webpack-contrib/css-loader) 来解析其中的 `url()` 引用，并将这些引用作为模块请求来处理。
- 可以根据本地的文件结构用相对路径来引用静态资源。

```js
// main.js

// 引入本地静态资源
import '@/styles/base.scss';
import '@/styles/base.css';
```

## CSS预处理器

### 使用sass

1. 安装模块

```bash
npm install -D sass-loader sass
```

2. 在style标签中添加 lang="scss"，注意是 'scss' 不是 'sass'

```vue
<style lang="scss">

</style>
```

### 使用less

1. 安装模块

```bash
npm install -D less-loader less
```

2. 在style标签上添加 lang="less"

```vue
<style lang="less">

</style>
```

### 其他

使用less或者sass的时候如果报错：this.getOptions is not a function 原因：

- sass中 sass-loader安装的的版本过高，解决：重新安装较低版本

   ```bash
    npm uninstall sass-loader -D
    npm uninstall node-sass -D
    
    npm install sass-loader@10.1.1 -D
    npm install node-sass@5.00 -D
   ```

- less中 less-loader 版本过高，解决：重新安装较低版本

  ```bash
  npm uninstall less-loader -D
  npm install less-loader@5.0.0 -D
  ```

# 组件的样式 Scoped 

当 style 标签有 scoped 属性时，它的 CSS 只作用于当前组件中的元素。这类似于 Shadow DOM 中的样式封装。它有一些注意事项，但不需要任何 polyfill。它通过使用 PostCSS 来实现以下转换：

```html
<style scoped>
.example {
  color: red;
}
</style>

<template>
  <div class="example">hi</div>
</template>
```

转换结果：

```html
<style>
.example[data-v-f3f3eg9] {
  color: red;
}
</style>

<template>
  <div class="example" data-v-f3f3eg9>hi</div>
</template>
```

## 混用本地和全局样式 

你可以在一个组件中同时使用有 scoped 和非 scoped 样式： 

- 本地样式：使用带有scoped属性的style标签
- 全局样式：
  - 使用不带 scoped 属性的style标签
  - 使用base.css在main.js或者index.html中导入

```vue
<style>
/* 全局样式 */
</style>

<style scoped>
/* 本地样式 */
</style>
```

## 子组件的根元素

子组件使用 scoped 后，父组件的样式将不会渗透到子组件中。不过一个子组件的根节点会同时受其父组件的 scoped CSS 和子组件的 scoped CSS 的影响。这样设计是为了让父组件可以从布局的角度出发，调整其子组件根元素的样式。

## 深度作用选择器

深度作用选择器（也叫样式穿透）是类似于 `>>>` 或 `/deep/` 或`::v-deep`这样的操作符。如果你希望 `scoped` 样式中的一个选择器能够作用得“更深”，例如影响子组件，你可以使用 `>>>` 操作符：

```html
<style scoped>
.a >>> .b { /* ... */ }
</style>
```

上述代码将会编译成：

```css
.a[data-v-f3f3eg9] .b { /* ... */ }
```

有些像 Sass 之类的预处理器无法正确解析 `>>>`。这种情况下你可以使用 `/deep/` 或 `::v-deep` 操作符取而代之——两者都是 `>>>` 的别名，同样可以正常工作。

深度作用选择器的主要作用是为了修改第三方组件的样式。

## 动态生成的内容

通过 `v-html` 创建的 DOM 内容不受 scoped 样式影响，但是你仍然可以通过深度作用选择器来为他们设置样式。

## 还有一些要留意

- **Scoped 样式不能代替 class**。考虑到浏览器渲染各种 CSS 选择器的方式，当 `p { color: red }` 是 scoped 时 (即与特性选择器组合使用时) 会慢很多倍。如果你使用 class 或者 id 取而代之，比如 `.example { color: red }`，性能影响就会消除。
- **在递归组件中小心使用后代选择器!** 对选择器 `.a .b` 中的 CSS 规则来说，如果匹配 `.a` 的元素包含一个递归子组件，则所有的子组件中的 `.b` 都将被这个规则匹配。

## 修改第三方组件中的样式

### 情况一：修改子组件的根标签样式

在父组件即使使用scoped，在父组件中依然可以设置子组件根标签的样式。

目的：这样设计是为了让父组件可以从布局的角度出发，调整其子组件根元素的样式。

### 情况二：修改子组件根标签内部标签样式

- 方式一：使用不带 scoped 属性的style标签，设置为全局样式影响子组件
- 方式二：使用带 scoped 属性的style标签，需要使用样式穿透：`>>>` 或者 `/deep/` 或者 `::v-deep`

#### 设置全局样式

使用sytle标签不带scoped属性设置为全局样式，因为第三方组件最终会被渲染成原生html标签，所以直接修改原生html标签即可，通过标签名或者渲染之后的class值选择器都可以 ：

```vue
<style>
  button:hover, .el-button:focus{
    background-color: #80008010 ;
    color: #800080;
    border: 0.5px solid #80008044;
  }
</style>
```

sytle标签不带scoped属性：如果第三方组件中已经对标签设置了样式，使用第一种方式失效，因为标签名选择器优先级较低，此时，添加!important提高优先级，即可使样式生效：

```vue
<style>
  input{
    border-color: red !important;
  }
</style>
```

#### 设置样式穿透

有些样式，提高优先级也不生效，因为style标签有scoped属性，产生了样式隔离，当前组件设置的样式有时不能在第三方组件生效,  此时可以用  vue 样式穿透。  

- 样式穿透：摆脱样式隔离限制，一般用来修改第三方组件中的样式。

- 基本结构:  当前组件标签选择器 >>> 第三方组件标签选择器。
- CSS预处理器无法正确解析 `>>>`。使用 `/deep/` 或 `::v-deep` 操作符取而代之。

```vue
<style>
  .search-box >>> .el-input {
    width: 400px;
  }
</style>
```

# vue.config.js

## 配置文件

从vue-cli 3.0 ，Vue 脚手架隐藏了所有 webpack 相关的配置，若想查看具体的 webpakc 配置， 请执行：

```bash
vue inspect > output.js
```

修改webpack打包选项，修改服务器配置等，需要手动在项目根目录下添加一个配置文件 vue.config.js，当项目启动时，vue-cli会把这个配置文件合并到原本的配置文件中。

新版本的vue-cli会在项目根目录自动创建 vue.config.js，vue对webpack的配置做了很多的修改，但是vue把webpack的配置文件隐藏了，我们想要修改webpack的配置，都写在 vue.config.js 文件中，当项目启动或者打包的时候会把我们自己在 vue.config.js 添加的配置合并到vue添加配置中，详情见：[vue.config.js配置](https://cli.vuejs.org/zh/config/)。

**注意：如果修改了 vue.config.js 需要重启项目，从新加载配置文件。**

## 常见配置

你使用 `@vue/cli-service` 提供的 `defineConfig` 帮手函数，以获得更好的类型提示：

```js
// vue.config.js
const { defineConfig } = require('@vue/cli-service')

module.exports = defineConfig({
  // 是否使用lint警告
  lintOnSave: process.env.NODE_ENV === 'development', 
  // 项目发布部署的路径，默认为 /
  publicPath：'/', 
  // 指定生成的 index.html 的输出路径 (相对于 outputDir)。也可以是一个绝对路径。
  indexPath: 'index.html',
  pages: {
		index: {
			// 项目的入口文件是src目录下 main.js
			entry: 'src/main.js',
			// 打包项目根据提供的模板生成html文件
			template: 'public/index.html',
			// 打包项目在 dist/index.html 的输出
			filename: 'index.html',
			// 当使用 title 选项时，template 中的 title 标签需要是 <title><%= htmlWebpackPlugin.options.title %></title>
			title: '掘金',
		},
	},
  // 生产环境去除log语句
  chainWebpack: config => {
		config.optimization.minimizer('terser').tap(args => {
			args[0].terserOptions.compress.drop_console = true;
			return args;
		});
	},
})
```

## 设置代理服务器

方式一：设置单个代理

1. 优点：配置简单，请求资源时直接发给前端（8080）即可。
2. 缺点：不能配置多个代理，不能灵活的控制请求是否走代理。
3. 工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器 （优先匹配前端资源）

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  devServer:{
    proxy:'http://127.0.0.1:3000'
  }
}
```

方式二：设置多个代理

1. 优点：可以配置多个代理，且可以灵活的控制请求是否走代理。
2. 缺点：配置略微繁琐，请求资源时必须加前缀。

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  devServer:{
    proxy:{
      // 代理1
      //  当你请求是以/api开头的时候，则我帮你代理访问到http://127.0.0.1:3000
      '/api': {
        target: 'http://127.0.0.1:3000',
        // secure: false,// 如果是https接口，需要配置这个参数
        // ws: true, //是否代理websockets， 默认值为true
        
        /*
           changeOrigin设置为true时，服务器收到的请求头中的host为：192.168:3000
           changeOrigin设置为false时，服务器收到的请求头中的host为：127.0.0.1:8080
           changeOrigin默认值为true
        */

        changeOrigin: true, // 设置请求头中的host地址，默认值为true
        //地址中的 /api 仅仅是一个请求转发标志，真正的接口中没有/api，所以在转发时重写请求路径，把/api删掉。
        pathRewrite: {'^/api' : ''}
      },
      // 代理2
      '/douyu':{
        target: 'http://open.douyucdn.cn',
         pathRewrite: {'^/douyu' : ''}
      }
    }

}
```

## 斗鱼接口文档

基础url地址：

- `http://open.douyucdn.cn`

获取房间列表 ---首页

- 请求方法： GET

- `/api/RoomApi/live`
- 参数1：offset 跳过多少条：0 30 60…
- 参数2：limit 请求条数的限制，默认是30

房间详情页：--- 房间详情页面

- 请求方法： GET

- `/api/RoomApi/room/房间id`

获取所有的游戏分类---全部分类页面

- 请求方法： GET

- `/api/RoomApi/game`

获取某一个分类下的房间列表---某个分类下的房间列表页

- 请求方法： GET

- `/api/RoomApi/live/房间分类的id`

# 模式和环境变量

## 软件开发环境

软件开发环境(Software Development Environment，SDE)是指在基本硬件和宿主软件的基础上，为支持系统软件和应用软件的工程化开发和维护而使用的一组软件，简称SDE。它由软件工具和环境集成机制构成，前者用以支持软件开发的相关过程、活动和任务，后者为工具集成和软件的开发、维护及管理提供统一的支持。

项目部署环境一般可分为三种：生产环境，测试环境，开发环境

- 开发环境：开发环境时程序猿们专门用于开发的服务器，配置可以比较随意，为了开发调试方便，一般打开全部错误报告和测试工具，是最基础的环境。

- 测试环境：一般是克隆一份生产环境的配置，一个程序在测试环境工作不正常，那么肯定不能把它发布到生产服务器上，是开发环境到生产环境的过度环境。测试环境一般是部署到公司私有的服务器或者局域网服务器上，主要用于测试是否存在bug，一般会不让用户和其他人看到，并且测试环境会尽量与生产环境相似。

- 生产环境：生产环境是指正式提供对外服务的，一般会关掉错误报告，打开错误日志，是最重要的环境。

上述环境也可以说是系统开发的三个阶段：开发 -> 测试 -> 上线，其中生产环境也就是通产说的真实的环境，最后交给用户的环境。

## 环境变量

项目开发过程中和项目发布上线后，根据开发环境和生产环境设置不同的请求地址，比如：

- http://127.0.0.1:8080/douyu 是开发过程的开发地址
- http://open.douyucdn.cn 是项目上线之后的正式地址

在发送请求的时候，开发过程中使用开发地址，项目打包之后使用正式地址。

比如在使用axios，开发的时候 baseURL设置为http://127.0.0.1:8080/douyu，打包项目的时候baseURL应该替换成 http://open.douyucdn.cn，处理这个问题可以使用 process.env.NODE_ENV 获取环境信息或者设置环境文件.env

## 模式

**模式**是 Vue CLI 项目中一个重要的概念。

vue-cli3中已经基于webpack把process.env.NODE_ENV配置好了，默认情况下，一个 Vue CLI 项目有三个模式：

- `development` 模式用于 `vue-cli-service serve`
- `test` 模式用于 `vue-cli-service test:unit`
- `production` 模式用于 `vue-cli-service build` 和 `vue-cli-service test:e2e`

可以使用 `process.env.NODE_ENV` 获取模式

执行 `npm run serve` 指令 `process.env.NODE_ENV` 打印的值为 `development` 是开发环境

执行 `npm run build` 指令 `process.env.NODE_ENV` 打印的值为 `production` 是生产环境

## 使用 process.env.NODE_ENV

使用 process.env.NODE_ENV 可以判断是开发环境还是生产环境

- 执行 npm run serve，项目是运行在开发环境，process.env.NODE_ENV的值为 development

- 执行 npm run build，项目是运行在生产环境，process.env.NODE_ENV的值为 production

```js
axios.defaults.baseURL = process.env.NODE_ENV !== 'production' 
   ? 'http://127.0.0.1:8080/douyu' : 'http://open.douyucdn.cn';
```

## 使用 .env 文件

### 文件命名

你可以在你的项目**根目录**中放置下列文件来指定环境变量：

```apl
.env                # 在所有的环境中被载入
.env.local          # 在所有的环境中被载入，但会被 git 忽略
.env.[mode]         # 只在指定的模式中被载入
.env.[mode].local   # 只在指定的模式中被载入，但会被 git 忽略

mode的常用值是 development 或者 production
```

文件名必须以如下方式命名，不要乱起名，也无需专门手动控制加载哪个文件，根据响应的指令，加载对应的文件

```bash
.env 							  全局默认配置文件，不论什么环境都会加载的配置文件

.env.development		执行 npm run serve 开发环境下加载的配置文件

.env.production 		执行 npm run build 生产环境下加载的配置文件
```

![image-20220324232008848](https://s2.loli.net/2022/03/24/EpzB4PNvmFRA1k3.png)

### 文件内容

#### 命名方式

一个环境文件只包含环境变量的“键=值”对：

**注意：**属性名必须以VUE_APP_开头，比如VUE_APP_XXX

```bash
VUE_APP_NAME = 张三
```

#### 环境文件加载优先级

为一个特定模式准备的环境文件 (例如 `.env.production`) 将会比一般的环境文件 (例如 `.env`) 拥有更高的优先级。

此外，Vue CLI 启动时已经存在的环境变量拥有最高优先级，并不会被 `.env` 文件覆写。

`.env` 环境文件是通过运行 `vue-cli-service` 命令载入的，因此环境文件发生变化，你需要重启服务。

#### 在客户端侧代码中使用环境变量

.env：

```bash
VUE_APP_NAME = 张三
```

.env.development：

```bash
VUE_APP_NAME= 李四
VUE_APP_AGE=18
VUE_APP_URL=http://127.0.0.1:3000
```

除了 `VUE_APP_*` 变量之外，在你的应用代码中始终可用的还有两个特殊的变量：

- `NODE_ENV`   会是 `"development"`、`"production"` 或 `"test"` 中的一个。具体的值取决于应用运行的模式。
- `BASE_URL`  会和 `vue.config.js` 中的 `publicPath` 选项相符，即你的应用会部署到的基础路径。

### 文件的加载

根据启动命令vue会自动加载对应的环境，vue是根据文件名进行加载的，所以上面说“不要乱起名，也无需专门控制加载哪个文件”

- 执行 `npm run serve` 命令，会自动加载 `.env.development` 文件
- 执行 `npm run build`命令，会自动加载 `.env.production` 文件

注意：`.env` 文件无论是开发还是生成都会加载的公用文件　

使用 `process.env` 获取属性，这是全局属性，任何地方均可使用。

按照上述 .env 和 .env.development 文件的内容，如过我们运行npm run serve  在就先加载.env文件，之后加载.env.development文件，两个文件有同一个项，则后加载的文件就会覆盖掉第一个文件，也即是 .env.development 文件覆盖掉了.env文件的VUE_APP_NAME选项。将会有以下输出：

```bash
 process.env.NODE_ENV 		 输出：development

 process.env.VUE_APP_NAME 	输出：李四

 process.env.VUE_APP_AGE	  输出：18

 process.env.VUE_APP_URL	  输出：http://127.0.0.1:3000
```

# create-vue

现在官方推荐使用 [create-vue](https://github.com/vuejs/create-vue) 来创建基于 [Vite](https://vitejs.dev/) 的新项目。

## 创建项目

创建Vue3项目：

```bash
npm create vue@3
```

如果您需要支持IE11，您可以创建Vue 2项目:

```bash
npm create vue@2
```

注意，版本号( `@3` 或 `@2` )绝对不能被省略，否则 npm 可能会解析为一个缓存的过时的包版本。

## 与Vue CLI的不同

Vue CLI基于webpack，而create-vue基于Vite。Vite支持Vue CLI项目中大多数已配置约定的开箱即用，并且由于其极快的启动和热模块替换速度，提供了显著更好的开发体验。在[这里](https://cn.vitejs.dev/guide/why.html)了解更多关于我们为什么推荐Vite而不是webpack的信息。

与Vue CLI不同的是，create-vue本身只是一个搭建工具：它根据您选择的功能创建一个预先配置的项目，并将其余的任务委托给Vue。以这种方式构建的项目可以直接利用与rollup兼容的Vite插件生态系统。

