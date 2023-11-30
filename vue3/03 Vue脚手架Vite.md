# Vite

## 简介

下一代的前端工具链，为开发提供极速响应。

官网：https://cn.vitejs.dev/

Vite（法语意为 "快速的"，发音 `/vit/`，发音同 "veet"）是一种新型前端构建工具，能够显著提升前端开发体验。它主要由两部分组成：

- 一个开发服务器，它基于 [原生 ES 模块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) 提供了 [丰富的内建功能](https://cn.vitejs.dev/guide/features.html)，如速度快到惊人的 [模块热更新（HMR）](https://cn.vitejs.dev/guide/features.html#hot-module-replacement)。
- 一套构建指令，它使用 [Rollup](https://rollupjs.org/) 打包你的代码，并且它是预配置的，可输出用于生产环境的高度优化过的静态资源。

Vite 意在提供开箱即用的配置，同时它的 [插件 API](https://cn.vitejs.dev/guide/api-plugin.html) 和 [JavaScript API](https://cn.vitejs.dev/guide/api-javascript.html) 带来了高度的可扩展性，并有完整的类型支持。

vite特点：

- 极速的服务启动：使用原生 ESM 文件，无需打包!
- 轻量快速的热重载：无论应用程序大小如何，都始终极快的模块热替换（HMR）

- 丰富的功能：对 TypeScript、JSX、CSS 等支持开箱即用。

- 优化的构建：可选 “多页应用” 或 “库” 模式的预配置 Rollup 构建

- 通用的插件：在开发和构建之间共享 Rollup-superset 插件接口。

- 完全类型化的API：灵活的 API 和完整的 TypeScript 类型。


## 创建项目

```bash
npm create vite@latest
```

> 兼容性注意
>
> Vite 需要 Node.js 版本 14.18+，16+。然而，有些模板需要依赖更高的 Node 版本才能正常运行，当你的包管理器发出警告时，请注意升级你的 Node 版本。

## 启动项目

把项目运行到开发环境，在开发项目的过程中使用：

 ```bash
cd  项目目录 #进入项目目录

npm i 		  #安装模块包

npm run dev #启动项目      
 ```

在浏览器的地址栏访问：http://127.0.0.1:5173打开项目。

## 打包项目

打包的文件会输出到dist文件中，一般是项目开发完毕 或者 阶段性功能完毕打包输出文件，发布上线：

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
│   └── vite.scg: 页签图标
├── src
│   ├── assets: 存放静态资源
│   │   └── logo.png
│   │── components: 存放组件
│   │   └── HelloWorld.vue
│   │── App.vue: 汇总所有组件
│   │── main.js: 入口文件
├── .gitignore: git版本管制忽略的配置
├── index.html: 主页面
├── package.json: 应用包配置文件 
├── README.md: 应用描述文件
├── package-lock.json：包版本控制文件
├── vite.config.js：项目配置文件
```

## `index.html` 与项目根目录

你可能已经注意到，在一个 Vite 项目中，`index.html` 在项目最外层而不是在 `public` 文件夹内。这是有意而为之的：在开发期间 Vite 是一个服务器，而 `index.html` 是该 Vite 项目的入口文件。

Vite 将 `index.html` 视为源码和模块图的一部分。Vite 解析 `<script type="module" src="...">` ，这个标签指向你的 JavaScript 源码。甚至内联引入 JavaScript 的 `<script type="module">` 和引用 CSS 的 `<link href>` 也能利用 Vite 特有的功能被解析。另外，`index.html` 中的 URL 将被自动转换，因此不再需要 `%PUBLIC_URL%` 占位符了。

与静态 HTTP 服务器类似，Vite 也有 “根目录” 的概念，即服务文件的位置，在接下来的文档中你将看到它会以 `<root>` 代称。源码中的绝对 URL 路径将以项目的 “根” 作为基础来解析，因此你可以像在普通的静态文件服务器上一样编写代码（并且功能更强大！）。Vite 还能够处理依赖关系，解析处于根目录外的文件位置，这使得它即使在基于 monorepo 的方案中也十分有用。

## vue-cli和Vite区别

[Vue CLI](https://cli.vuejs.org/zh/) 是官方提供的基于 Webpack 的 Vue 工具链，在vue2及之前的版本都是用vue-cli脚手架来创建vue的项目。

[Vite](https://cn.vitejs.dev/) 是一个轻量级的、速度极快的构建工具，对 Vue SFC 提供第一优先级支持。作者是尤雨溪，同时也是 Vue 的作者！

随着Vue3的发布，尤雨溪发布了vite工具，也可以用来创建vue的项目，是针对于vue3项目。

- vue-cli可以创建vue2 项目，也可以创建vue3的项目
- vite是用来创建vue3的项目

## Vite的优势

- vite创建项目速度极快
- vite启动项目速度极快
- vite不仅能够创建vue的项目，还能创建react项目等

为什么vite启动项目比较快：是应为在vite项目直接本地加载的ESM（ES的模块）模块，不需要打包，直接加载js模块运行，就像我们自己在HTML文件中导入js文件运行项目一样。

为什么vue-cli启动项目很慢：每次运行项目，需要把项目使用webpack进行编译和打包，使用npm run serve是把项目打包在内存中，打包项目需要把浏览器不能识别的代码都编译成浏览器识别的代码，这些过程比较耗费时间，所以比较慢。

# 使用CSS

## 引用静态CSS资源

导入 `.css` 文件将会把内容插入到 `<style>` 标签中，同时也带有 HMR 支持。也能够以字符串的形式检索处理后的、作为其模块默认导出的 CSS。

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
npm install -D sass
```

2. 在style标签中添加 lang="scss"，注意是 'scss' 不是 'sass'

```vue
<style lang="scss">

</style>
```

### 使用less

1. 安装模块

```bash
npm install -D less
```

2. 在style标签上添加 lang="less"

```vue
<style lang="less">

</style>
```

# 单文件组件 CSS 功能

## 组件作用域 CSS

当 `<style>` 标签带有 `scoped` attribute 的时候，它的 CSS 只会影响当前组件的元素，和 Shadow DOM 中的样式封装类似。使用时有一些注意事项，不过好处是不需要任何的 polyfill。它的实现方式是通过 PostCSS 将以下内容：

```vue
<style scoped>
.example {
  color: red;
}
</style>

<template>
  <div class="example">hi</div>
</template>
```

转换为：

```vue
<style>
.example[data-v-f3f3eg9] {
  color: red;
}
</style>

<template>
  <div class="example" data-v-f3f3eg9>hi</div>
</template>
```

### 子组件的根元素

使用 `scoped` 后，父组件的样式将不会渗透到子组件中。不过，子组件的根节点会同时被父组件的作用域样式和子组件的作用域样式影响。这样设计是为了让父组件可以从布局的角度出发，调整其子组件根元素的样式。

### 深度选择器

处于 `scoped` 样式中的选择器如果想要做更“深度”的选择，也即：影响到子组件，可以使用 `:deep()` 这个伪类：

```vue
<style scoped>
.a :deep(.b) {
  /* ... */
}
</style>
```

上面的代码会被编译成：

```css
.a[data-v-f3f3eg9] .b {
  /* ... */
}
```

>  TIP
>
> 通过 `v-html` 创建的 DOM 内容不会被作用域样式影响，但你仍然可以使用深度选择器来设置其样式。

### 插槽选择器

默认情况下，作用域样式不会影响到 `<slot/>` 渲染出来的内容，因为它们被认为是父组件所持有并传递进来的。使用 `:slotted` 伪类以明确地将插槽内容作为选择器的目标：

```vue
<style scoped>
:slotted(div) {
  color: red;
}
</style>
```

### 全局选择器

如果想让其中一个样式规则应用到全局，比起另外创建一个 `<style>`，可以使用 `:global` 伪类来实现 (看下面的代码)：

```vue
<style scoped>
:global(.red) {
  color: red;
}
</style>
```

### 混合使用局部与全局样式

你也可以在同一个组件中同时包含作用域样式和非作用域样式：

```vue
<style>
/* 全局样式 */
</style>

<style scoped>
/* 局部样式 */
</style>
```

### 作用域样式须知

- **作用域样式并没有消除对 class 的需求**。由于浏览器渲染各种各样 CSS 选择器的方式，`p { color: red }` 结合作用域样式使用时 (即当与 attribute 选择器组合的时候) 会慢很多倍。如果你使用 class 或者 id 来替代，例如 `.example { color: red }`，那你几乎就可以避免性能的损失。
- **小心递归组件中的后代选择器**！对于一个使用了 `.a .b` 选择器的样式规则来说，如果匹配到 `.a` 的元素包含了一个递归的子组件，那么所有的在那个子组件中的 `.b` 都会匹配到这条样式规则。

## CSS Modules

一个 `<style module>` 标签会被编译为 [CSS Modules](https://github.com/css-modules/css-modules) 并且将生成的 CSS class 作为 `$style` 对象暴露给组件：

```vue
<template>
  <p :class="$style.red">This should be red</p>
</template>

<style module>
.red {
  color: red;
}
</style>
```

得出的 class 将被哈希化以避免冲突，实现了同样的将 CSS 仅作用于当前组件的效果。

参考 [CSS Modules spec](https://github.com/css-modules/css-modules) 以查看更多详情，例如 [global exceptions](https://github.com/css-modules/css-modules#exceptions) 和 [composition](https://github.com/css-modules/css-modules#composition)。

### 自定义注入名称

你可以通过给 `module` attribute 一个值来自定义注入 class 对象的属性名：

```vue
<template>
  <p :class="classes.red">red</p>
</template>

<style module="classes">
.red {
  color: red;
}
</style>
```

### 与组合式 API 一同使用

可以通过 `useCssModule` API 在 `setup()` 和 `<script setup>` 中访问注入的 class。对于使用了自定义注入名称的 `<style module>` 块，`useCssModule` 接收一个匹配的 `module` attribute 值作为第一个参数：

```js
import { useCssModule } from 'vue'

// 在 setup() 作用域中...
// 默认情况下, 返回 <style module> 的 class
useCssModule()

// 具名情况下, 返回 <style module="classes"> 的 class
useCssModule('classes')
```

## CSS 中的 `v-bind()`

单文件组件的 `<style>` 标签支持使用 `v-bind` CSS 函数将 CSS 的值链接到动态的组件状态：

```vue
<template>
  <div class="text">hello</div>
</template>

<script>
export default {
  data() {
    return {
      color: 'red'
    }
  }
}
</script>

<style>
.text {
  color: v-bind(color);
}
</style>
```

这个语法同样也适用于 [`<script setup>`](https://cn.vuejs.org/api/sfc-script-setup.html)，且支持 JavaScript 表达式 (需要用引号包裹起来)：

```vue
<script setup>
const theme = {
  color: 'red'
}
</script>

<template>
  <p>hello</p>
</template>

<style scoped>
p {
  color: v-bind('theme.color');
}
</style>
```

实际的值会被编译成哈希化的 CSS 自定义属性，因此 CSS 本身仍然是静态的。自定义属性会通过内联样式的方式应用到组件的根元素上，并且在源值变更的时候响应式地更新。

# 静态资源处理

## 将资源引入为 URL

服务时引入一个静态资源会返回解析后的公共路径：

```js
import imgUrl from './img.png'
document.getElementById('hero-img').src = imgUrl
```

例如，`imgUrl` 在开发时会是 `/img.png`，在生产构建后会是 `/assets/img.2d8efhg.png`。

行为类似于 Webpack 的 `file-loader`。区别在于导入既可以使用绝对公共路径（基于开发期间的项目根路径），也可以使用相对路径。

- `url()` 在 CSS 中的引用也以同样的方式处理。
- 如果 Vite 使用了 Vue 插件，Vue SFC 模板中的资源引用都将自动转换为导入。
- 常见的图像、媒体和字体文件类型被自动检测为资源。
- 引用的资源作为构建资源图的一部分包括在内，将生成散列文件名，并可以由插件进行处理以进行优化。
- 较小的资源体积小于 [`assetsInlineLimit` 选项值](https://cn.vitejs.dev/config/build-options.html#build-assetsinlinelimit) 则会被内联为 base64 data URL。

## `public` 目录

如果你有下列这些资源：

- 不会被源码引用（例如 `robots.txt`）
- 必须保持原有文件名（没有经过 hash）
- ...或者你压根不想引入该资源，只是想得到其 URL。

那么你可以将该资源放在指定的 `public` 目录中，它应位于你的项目根目录。该目录中的资源在开发时能直接通过 `/` 根路径访问到，并且打包时会被完整复制到目标目录的根目录下。

目录默认是 `<root>/public`，但可以通过 [`publicDir` 选项](https://cn.vitejs.dev/config/shared-options.html#publicdir) 来配置。

请注意：

- 引入 `public` 中的资源永远应该使用根绝对路径 —— 举个例子，`public/icon.png` 应该在源码中被引用为 `/icon.png`。
- `public` 中的资源不应该被 JavaScript 文件引用。

## new URL(url, import.meta.url)

[import.meta.url](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/import.meta) 是一个 ESM 的原生功能，会暴露当前模块的 URL。将它与原生的 [URL 构造器](https://developer.mozilla.org/zh-CN/docs/Web/API/URL) 组合使用，在一个 JavaScript 模块中，通过相对路径我们就能得到一个被完整解析的静态资源 URL：

```js
const imgUrl = new URL('./img.png', import.meta.url).href

document.getElementById('hero-img').src = imgUrl
```

这在现代浏览器中能够原生使用 - 实际上，Vite 并不需要在开发阶段处理这些代码！

这个模式同样还可以通过字符串模板支持动态 URL：

```js
function getImageUrl(name) {
  return new URL(`./dir/${name}.png`, import.meta.url).href
}
```

在生产构建时，Vite 才会进行必要的转换保证 URL 在打包和资源哈希后仍指向正确的地址。然而，这个 URL 字符串必须是静态的，这样才好分析。否则代码将被原样保留、因而在 `build.target` 不支持 `import.meta.url` 时会导致运行时错误。

```js
// Vite 不会转换这个
const imgUrl = new URL(imagePath, import.meta.url).href
```

# 使用图片资源

## 放在public中

放在pubilc中，打包的时候会直接复制pubilc中的图片，放在pubilc中的图片相当于放在服务器根目录的资源

- 在组件中使用：`<template>`、`<script>`都是使用 /开头，比如 /img/icon-home.png

```vue
<template>
	<div>
    <img src="/img/head.png" alt="" />
    <img :src="imgUrl" alt="" />
    <div class="box" style="background-image:url('/img/head.png');">box</div>
    <div class="box" :style="{background:'url('+ imgUrl +')'}">box</div>
  </div>
</template>

<script setup>
const imgUrl = new URL('/img/head.png', import.meta.url).href;
</script>
```

## 放在assets中

- 在`<template>`中使用，可以使用 ./ 或者 ../ 开头的路径，比如：./img/head.png
- 在`<script>`中，可以使用 import 导入的方式
- 在`<style>`中使用，可以使用 ./ 或者 ../ 开头的路径，比如：./img/head.png

路径别名@：@ 指向 `<projectRoot>/src`的别名

```vue
<script setup>
  // 第一种方式：适用于处理单个链接的资源文件
  import imgUrl from './assets/img/head.png';
  
 	// 第二种方式：适用于处理多个链接的资源文件
	/* 工具文件目录： src/utilS/img-tools.js
    export const useImg = (url) => {
      return new URL(`../assets/img/${url}`, import.meta.url).href
    }
  */
	import { useImg } from '@/utils/img-tools';

</script>


<template>
	<div>
    <!-- 第二种方式：直接使用相对路径 -->
		<img alt="Vue logo" src="@/assets/img/head.png" />
		<img alt="Vue logo" src="./assets/img/head.png" />
		<img :src="imgUrl" alt="" />
    <img :src="useImg('head.png')" alt="" />

		<div class="box1" :style="{ background: 'url(' + imgUrl + ')' }">box1</div>
		<div class="box2">box2</div>
  </div>
</template>



<style>
.box1 {
	width: 100px;
	height: 100px;
}
.box2 {
	width: 100px;
	height: 100px;
  /* 如果是背景图片引入的方式（一定要使用相对路径） */
	background-image: url('./assets/img/head.png');
}
</style>
```

# vite.config.js

当以命令行方式运行 `vite` 时，Vite 会自动解析项目根目录下名为 `vite.config.js` 的文件。

## 常见配置

- 使用 `defineConfig` 工具函数配置智能提示

- Vite 也直接支持 TS 配置文件。你可以在 `vite.config.ts` 中使用 `defineConfig` 工具函数。

Vite 支持在配置文件中使用 ESM 语法。这种情况下，配置文件会在被加载前自动进行预处理。

```js
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
 // 开发或生产环境服务的公共基础路径。默认 /
  base: '/',
  // 作为静态资源服务的文件夹。默认： "public"
  publicDir: 'public',
  // 需要用到的插件数组。
  plugins: [vue()],
  // 路径别名
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    },
    // 导入模块的时候可以省略模块的后缀名
    extensions: ['.mjs', '.js', '.ts', '.jsx', '.tsx', '.json', '.vue']
  }
  server: {
    host: '127.0.0.1',//指定服务器应该监听哪个 IP 地址。
    port: 3000,//指定开发服务器端口。
    // open: true,//在开发服务器启动时自动在浏览器中打开应用程序
  }
})
```

## 设置代理服务器

方式一：设置单个代理

```js
import { defineConfig } from 'vite'
export default defineConfig({
  server:{
    proxy:{
      '/douyu': 'http://open.douyucdn.cn',
    }
  }
}
```

方式二：设置多个代理

```js
import { defineConfig } from 'vite'
export default defineConfig({
  server:{
    proxy:{
      // 代理1
      '/api': {
        target: 'http://127.0.0.1:3000',
        changeOrigin: true, 
        rewrite: (path) => path.replace(/^\/api/, '')      
      },
      // 代理2
      '/douyu':{
        target: 'http://open.douyucdn.cn',
        rewrite: (path) => path.replace(/^\/douyu/, '')      
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

# 环境变量和模式

## 环境变量

Vite 在一个特殊的 **`import.meta.env`** 对象上暴露环境变量。这里有一些在所有情况下都可以使用的内建变量：

- **`import.meta.env.MODE`**: {string} 应用运行的[模式](https://cn.vitejs.dev/guide/env-and-mode.html#modes)。
- **`import.meta.env.BASE_URL`**: {string} 部署应用时的基本 URL。他由[`base` 配置项](https://cn.vitejs.dev/config/shared-options.html#base)决定。
- **`import.meta.env.PROD`**: {boolean} 应用是否运行在生产环境。
- **`import.meta.env.DEV`**: {boolean} 应用是否运行在开发环境 (永远与 `import.meta.env.PROD`相反)。
- **`import.meta.env.SSR`**: {boolean} 应用是否运行在 [server](https://cn.vitejs.dev/guide/ssr.html#conditional-logic) 上。

## 生产环境替换

在生产环境中，这些环境变量会在构建时被**静态替换**，因此，在引用它们时请使用完全静态的字符串。动态的 key 将无法生效。例如，动态 key 取值 `import.meta.env[key]` 是无效的。

它还将替换出现在 JavaScript 和 Vue 模板中的字符串。这本应是非常少见的，但也可能是不小心为之的。在这种情况下你可能会看到类似 `Missing Semicolon` 或 `Unexpected token` 等错误，例如当 `"process.env``.NODE_ENV"` 被替换为 `""development": "`。有一些方法可以避免这个问题：

- 对于 JavaScript 字符串，你可以使用 unicode 零宽度空格来分割这个字符串，例如： `'import.meta\u200b.env.MODE'`。
- 对于 Vue 模板或其他编译到 JavaScript 字符串的 HTML，你可以使用 [`wbr` 标签](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/wbr)，例如：`import.meta.<wbr>env.MODE`。

## .env 文件

Vite 使用 [dotenv](https://github.com/motdotla/dotenv) 从你的 [环境目录](https://cn.vitejs.dev/config/shared-options.html#envdir) 中的下列文件加载额外的环境变量：

```bash
.env                # 所有情况下都会加载
.env.local          # 所有情况下都会加载，但会被 git 忽略
.env.[mode]         # 只在指定模式下加载
.env.[mode].local   # 只在指定模式下加载，但会被 git 忽略
```

环境加载优先级

- 一份用于指定模式的文件（例如 `.env.production`）会比通用形式的优先级更高（例如 `.env`）。

- 另外，Vite 执行时已经存在的环境变量有最高的优先级，不会被 `.env` 类文件覆盖。例如当运行 `VITE_SOME_KEY=123 vite build` 的时候。

- `.env` 类文件会在 Vite 启动一开始时被加载，而改动会在重启服务器后生效。

加载的环境变量也会通过 `import.meta.env` 以字符串形式暴露给客户端源码。

为了防止意外地将一些环境变量泄漏到客户端，只有以 `VITE_` 为前缀的变量才会暴露给经过 vite 处理的代码。例如下面这些环境变量：

```
VITE_SOME_KEY=123
DB_PASSWORD=foobar
```

只有 `VITE_SOME_KEY` 会被暴露为 `import.meta.env.VITE_SOME_KEY` 提供给客户端源码，而 `DB_PASSWORD` 则不会。

```js
console.log(import.meta.env.VITE_SOME_KEY) // 123
console.log(import.meta.env.DB_PASSWORD) // undefined
```

如果你想自定义 env 变量的前缀，请参阅 [envPrefix](https://cn.vitejs.dev/config/shared-options.html#envprefix)。

安全注意事项

- `.env.*.local` 文件应是本地的，可以包含敏感变量。你应该将 `.local` 添加到你的 `.gitignore` 中，以避免它们被 git 检入。
- 由于任何暴露给 Vite 源码的变量最终都将出现在客户端包中，`VITE_*` 变量应该不包含任何敏感信息。

## TypeScript 的智能提示

默认情况下，Vite 在 [`vite/client.d.ts`](https://github.com/vitejs/vite/blob/main/packages/vite/client.d.ts) 中为 `import.meta.env` 提供了类型定义。随着在 `.env[mode]` 文件中自定义了越来越多的环境变量，你可能想要在代码中获取这些以 `VITE_` 为前缀的用户自定义环境变量的 TypeScript 智能提示。

要想做到这一点，你可以在 `src` 目录下创建一个 `env.d.ts` 文件，接着按下面这样增加 `ImportMetaEnv` 的定义：

```
/// <reference types="vite/client" />

interface ImportMetaEnv {
  readonly VITE_APP_TITLE: string
  // 更多环境变量...
}

interface ImportMeta {
  readonly env: ImportMetaEnv
}
```

如果你的代码依赖于浏览器环境的类型，比如 [DOM](https://github.com/microsoft/TypeScript/blob/main/lib/lib.dom.d.ts) 和 [WebWorker](https://github.com/microsoft/TypeScript/blob/main/lib/lib.webworker.d.ts)，你可以在 `tsconfig.json` 中修改 [lib](https://www.typescriptlang.org/tsconfig#lib) 字段来获取类型支持。

```
{
  "lib": ["WebWorker"]
}
```

## 模式

默认情况下，开发服务器 (`dev` 命令) 运行在 `development` (开发) 模式，而 `build` 命令则运行在 `production` (生产) 模式。

这意味着当执行 `vite build` 时，它会自动加载 `.env.production` 中可能存在的环境变量：

```
# .env.production
VITE_APP_TITLE=My App
```

在你的应用中，你可以使用 `import.meta.env.VITE_APP_TITLE` 渲染标题。

然而，重要的是要理解 **模式** 是一个更广泛的概念，而不仅仅是开发和生产。一个典型的例子是，你可能希望有一个 “staging” (预发布|预上线) 模式，它应该具有类似于生产的行为，但环境变量与生产环境略有不同。

你可以通过传递 `--mode` 选项标志来覆盖命令使用的默认模式。例如，如果你想为我们假设的 staging 模式构建应用：

```
vite build --mode staging
```

为了使应用实现预期行为，我们还需要一个 `.env.staging` 文件：

```
# .env.staging
NODE_ENV=production
VITE_APP_TITLE=My App (staging)
```

现在，你的 staging 应用应该具有类似于生产的行为，但显示的标题与生产环境不同。
