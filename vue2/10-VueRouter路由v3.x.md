# 路由的理解

## 介绍

路由(英文：route)就是对应关系， 路由分为**前端路由**和**后端路由**，比如前端路由就是 Hash 地址与组件之间的对应关系。

具体来说：一个路由就是一组key-value的对应关系

- key 为 url 路径
- value 可能是函数function 或 组件component

多个路由，需要经过路由器(英文：router)的管理。

![5nFCPLdBhoiKVts.png](https://s2.loli.net/2022/03/13/J5KHgqlikma9ZpA.png)

## 后端路由

### 后端路由概念

理解：value 是 函数function, 用于处理客户端提交的请求。 

工作过程：服务器接收到一个请求时，根据用户**请求路径URL**找到对应的**处理函数** 来处理请求，返回响应数据。

路由这个概念最先是后端出现的。在以前用模板引擎开发页面时，经常会看到这样

```text
http://hometown.xxx.edu.cn/bbs/forum.html
```

有时还会有带`.asp`或`.php`的路径，这就是所谓的SSR(Server Side Render) 服务端渲染，通过服务端渲染，直接返回页面。

其响应过程是这样的

- 浏览器发出请求

- 服务器监听到80端口（或443）有请求过来，并解析url路径

- 根据服务器的路由配置，返回相应信息（可以是 html 字串，也可以是 json 数据，图片等）

- 浏览器根据数据包的`Content-Type`来决定如何解析数据

简单来说路由就是用来跟后端服务器进行交互的一种方式，通过不同的路径，来请求不同的资源，请求不同的页面是路由的其中一种功能。

### Node.js 中的路由

在nodejs中路由是指应用程序如何响应客户端对特定端点的请求，该端点是 URL（或路径）和特定的 HTTP 请求方法（GET、POST 等）。

每个路由可以有一个或多个处理函数，当路由匹配时执行。

路由定义采用以下结构：`app.method(path, handler)`

- `app`：express的实例
- `method`：http请求方法的小写，比如：`app.get()`、`app.post()`
- `path`：请求的url地址
- `handler`：当路由被匹配的时候执行的函数，该函数有两个参数request和response，分别表示请求对象和响应对象

```js
app.get('/index', function (req, res) {
	res.render('index.html', {});
});
```

## 前端路由

### 前端路由概念

理解：value 是 组件component，用于展示页面内容。 

工作过程：当浏览器的路径改变时，对应的组件就会显示。

前端路由：指的是url与组件的映射关系，据不同的 url 地址展示不同的内容或页面，通过url的变化实现页面的局部变化，而不需要刷新页面；

前端路由就是把不同路由对应不同的内容或页面的任务交给前端来做，之前是通过服务端根据 url 的不同返回不同的页面实现的。

路由与普通的js显隐实现页面切换的区别：路由有url支持分享、链接、收藏；

### 单页SPA

HTML5之后，流行单页网站(single page web application: SPA 单页应用)，单页应用指的是一个web应用只有一个HTML页面，一个页面有不同的部分，点击页面导航部分不会刷新页面，也不会跳转页面，而是根据url地址的不同，显示不同的部分。

单页应用中不同部分的展示，需要通过组件的展示与切换来实现，所有组件的展示与切换都在这唯一的一个页面内完成。此时，不同组件之间的切换需要通过前端路由来实现。

单页应用不仅仅是在页面交互是无刷新的，连页面跳转都是无刷新的，为了实现单页应用，所以就有了前端路由。

### 什么时候使用前端路由

在单页面应用，大部分页面结构不变，只改变部分内容的使用

### 前端路由有什么优点和缺点

- 优点

​      用户体验好，不需要每次都从服务器全部获取，快速展现给用户

- 缺点

​     使用浏览器的前进，后退键的时候会重新发送请求，没有合理地利用缓存

​     单页面无法记住之前滚动的位置，无法在前进，后退的时候记住滚动的位置

### 前端路由实现原理

前端路由的工作方式：

- 用户点击了页面上的路由链接

- 导致了 URL 地址栏中的【 Hash 值或者URL路径】发生了变化

- 前端路由监听了到【 Hash 地址或者URL路径】的变化

- 前端路由把当前【 Hash 地址或者URL路径】对应的组件渲染都浏览器中

#### hash 模式 

前端路由的实现其实很简单。本质上就是检测 url 的变化，截获 url 地址，然后解析来匹配路由规则。

在 2014 年之前，大家是通过 hash 来实现路由，url hash 就是类似于

```http
https://segmentfault.com/a/1190000011956628#articleHeader2
```

这种 `#`。后面 hash 值的变化，并不会导致浏览器向服务器发出请求，浏览器不发出请求，也就不会刷新页面。另外每次 hash 值的变化，还会触发 `hashchange` 这个事件，通过这个事件我们就可以知道 hash 值发生了哪些变化。

hash 的实现相对来说要简单方便些，而且不用服务器来支持。

> url地址分为6部分：协议、IP(域名)、端口号、路径、参数、hash哈希值。
>
> http://www.example.com/login?username=tom#print
>
> 哈希值：url中第一个#后边的内容都是这个url的哈希值
>
> url中的哈希值是给前端浏览器使用的，最初，浏览器能够使用hash实现页面锚点。
>
> hash仅作为前端浏览器的参考，当浏览器根据一个url向服务器发送请求时，哈希值是不会发送的。
>
> hashchange 使用来监听页面hash值的变化的事件：window.addEventListener("hashchange", fun);

假如我们要用 hash 的模式实现一个路由，那么流程应该是这样的。

![image-20220313231311007](https://s2.loli.net/2022/03/13/CJLzRQrshlX72xy.png)

#### hash路由的实现

```html
<!-- 导航 -->
<nav>
  <div class="w">
    <a href="#home">首页</a>
    <a href="#friend">好友</a>
    <a href="#setting">设置</a>
  </div>
</nav>
<!-- 展示页面 -->
<div class="w">
  <div id="view"></div>
</div>

<script>
  const page = {
    home: `<h1>这是首页</h1>`,
    friend: `<h1>好友页面</h1>`,
    setting: `<h1>关于页面</h1>`,
  }

  const view = document.getElementById("view");
  // 设置首页
  view.innerHTML = page.home;

  // 当页面url中的哈希值发生变化时触发
  window.onhashchange = function (ev) {
    const index = ev.newURL.indexOf("#");
    const hash = ev.newURL.substring(index + 1);
    view.innerHTML = page[hash];
  }
</script>
```

#### HTML5 History 模式

14年后，因为HTML5标准发布。多了两个 API，`pushState` 和 `replaceState`，通过这两个 API 可以改变 url 地址且不会发送请求。同时还有 `onpopstate` 事件。通过这些就能用另一种方式来实现前端路由了，但原理都是跟 hash 实现相同的。用了 HTML5 的实现，单页路由的 url 就不会多出一个`#`，变得更加美观。但因为没有 `#` 号，所以当用户刷新页面之类的操作时，浏览器还是会给服务器发送请求。为了避免出现这种情况，所以这个实现需要服务器的支持，需要把所有路由都重定向到根页面。

假如我们要用 History 的模式实现一个路由，那么流程应该是这样的。

![preview](https://s2.loli.net/2022/03/13/yijCx2vLI3hUKda.jpg)

# Vue Router的介绍

## 简介

前端路由是现代SPA应用必备的功能，每个现代前端框架都有对应的实现，例如vue-router、react-router。

Vue Router是Vue.js 官方的**路由管理器**。它和 Vue.js 的核心深度集成，使用 Vue.js + Vue Router 让构建单页面应用变得易如反掌。包含的功能有：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于 Vue.js 过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的 CSS class 的链接
- HTML5 历史模式或 hash 模式，在 IE9 中自动降级
- 自定义的滚动条行为

## 官网

vue-router目前主要有两个版本v3.x和v.4x。

- Vue2.x对应vue-router的v3.x版本

- Vue3.x对应vue-router的v4.x版本

v3版本的官网：https://v3.router.vuejs.org/zh/

v4版本的官网：https://router.vuejs.org/zh/

## 安装

以Vue2中的方式使用为例：

方式一：在浏览器中直接使用，需要使用script标签导入vue-router.js

```html
<script src="https://unpkg.com/vue-router@3/dist/vue-router.js"></script>
```

方式二：在脚手架中使用

安装vue-router的时候需要指定版本

- vue2.x对应vue-router3.x：`npm install vue-router@3`
- vue3.x对应vue-router4.x：`npm install vue-router@4`

# Vue Router的使用步骤

## 在浏览器中的使用

### 第一步：安装vue-router

```html
<script src="https://unpkg.com/vue-router@3/dist/vue-router.js"></script>
```
### 第二步：添加路由导航链接及路由出口

在Vue根组件的模板中添加添加路由导航链接及路由出口

- 路由导航链接：点击不同的导航链接，导航到匹配的组件
- 路由出口：导航链接匹配的组件渲染的位置

```html
<div id="app">
  <nav id="nav-list">
    <div class="w">
      <!-- 路由导航链接 -->
      <!-- 通过传入 `to` 属性指定链接. -->
      <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
      <router-link to="/">首页</router-link>
      <router-link to="/friend">好友</router-link>
      <router-link to="/setting">设置</router-link>
    </div>
  </nav>

  <div class="w">
    <!-- 路由出口 -->
  	<!-- 路由匹配到的组件将渲染在这里 -->
    <router-view id="view"></router-view>
  </div>
</div>
```
### 第三步：创建路由组件

这一步就是创建vue组件，等配置好路由之后，随后将组件映射到路由。

- 路由组件不需要使用`Vue.component()` 或者 `components` 注册，直接使用组件配置对象创建。
- 当路由路径匹配的时候，会在路由出口自动渲染路由组件
- 路由组件默认没有名字，可以通过name选项给组件起名字，方便调试

```js
const home = {
  name: 'Home',
  template: '<h1>首页</h1>',
}
const friend = {
  name: 'Friend',
  template: '<h1>好友</h1>',
}
const setting = {
  name: 'Setting',
  template: '<h1>设置</h1>',
}
```

### 第四步：配置路由规则

这一步主要是创建路由管理器对象 `router`，并配置路由列表（路由表）`routes`。

先创建路由对象route，把每一个路由对象存入数组 routes 中，在路由对象中配置路由规则：

- path：定义路由url地址，一个url路径对应一个组件
- component：定义路由url地址映射的Vue组件

```js
const routes = [
  { path: '/', component: home },
  { path: '/friend', component: friend },
  { path: '/setting', component: setting },
];
```

再创建路由管理器对象router，创建路由管理器对象的时候，需要传入上面定义的路由规则对象routes：

```js
const router = new VueRouter({
  routes, // (缩写) 相当于 routes: routes
});
```

也可以把上面两步合并在一起：

```js
const router = new VueRouter({
  routes: [
    { path: '/', component: home },
    { path: '/friend', component: friend },
    { path: '/setting', component: setting },
  ]
});
```

### 第五步：注入路由

在根组件实例中通过 router 配置参数注入路由，从而让整个应用都有路由功能：

```js
new Vue({
  el: '#app',
  router, // (缩写) 相当于 router: router,
});
```

## 在脚手架中的使用

vue脚手架项目目录分析：

- public是静态资源文件夹。

- src是项目源代码文件夹，整个项目的所有代码都再src中。在src文件夹中，main.js是整个项目的入口文件。当运行项目时，vue-cli会使用webpack对main.js进行打包。

  - main.js 入口文件

  - App.vue   根组件下的app组件，后面所有的组件是添加在app组件内

  - components 目录 ==存放普通组件==

  - views/pages 目录 ==存放路由组件==

  - router目录 存放路由的配置文件index.js

  - store目录 存放vuex的配置文件

  - assets目录 存放静态资源

普通组件和路由组件都是vue的单文件组件，创建方式是一样的。

- 普通组件：可以导入到任意组件中使用，导入之后一定要在当前组件内的components属性中注册组件
- 路由组件：路由组件用在路由配置文件文件中，不需要注册，通过路由匹配，直接渲染在路由出口的组件

### 第一步：安装vue-router

```bash
npm install vue-router@3
```

### 第二步：添加路由导航链接及路由出口

在`App.vue`组件中添加路由导航链接及路由出口

```vue
// app.vue

<template>
	<div id="app" class="app">
 
		<nav>
			<div class="w">
				<router-link to="/">首页</router-link>
				<router-link to="/friend">好友</router-link>
				<router-link to="/setting">设置</router-link>
			</div>
		</nav>

    <div class="w">
      <router-view></router-view>
    </div>
    
	</div>
</template>
```

### 第三步：创建路由组件

路由组件都放在src目录下的views或者pages文件中

### 第四步：配置路由规则

在项目src目录中创建router文件夹，在文件中创建index.js文件，在index.js文件中配置路由

- 在一个模块化的打包系统中，您必须显式地通过 `Vue.use()` 来安装 vue-router：

```js
// router/index.js

import Vue from 'vue';
import VueRouter from 'vue-router';

Vue.use(VueRouter);

// 导入路由组件
import home from '../views/home.vue';
import friend from '../views/friend.vue';
import setting from '../views/setting.vue';

const router = new VueRouter({
  routes: [
    { path: '/', component: home},
    { path: '/friend', component: friend},
    { path: '/setting', component: setting},
  ],
});

// 导出路由
export default router;
```

### 第五步：注入路由

在main.js导入路由配置文件 index.js，并在根组件中注入路由

```js
// main.js

import Vue from 'vue';
import App from './App.vue';

// 导入路由配置文件
import router from './router';


Vue.config.productionTip = false;
new Vue({
  render: h => h(App),
  router,// 注入路由
}).$mount('#app');
```

# router-link 组件

## 作用

`<router-link>` 组件用于设置导航链接。 通过 `to` 属性指定目标地址，默认渲染成带有正确链接的 `<a>` 标签，可以通过配置 `tag` 属性生成别的标签.。另外，当目标路由成功激活时，链接元素自动设置一个表示激活的 CSS 类名。

`<router-link>` 比起写死的 `<a href="...">` 会好一些，理由如下：

- 无论是 HTML5 history 模式还是 hash 模式，它的表现行为一致，所以，当你要切换路由模式，或者在 IE9 降级使用 hash 模式，无须作任何变动。
- 在 HTML5 history 模式下，`router-link` 会守卫点击事件，让浏览器不再重新加载页面。
- 当你在 HTML5 history 模式下使用 `base` 选项之后，所有的 `to` 属性都不需要写 (基路径) 了。

请注意，我们没有使用常规的 `a` 标签，而是使用一个自定义组件 `router-link` 来创建链接。这使得 Vue Router 可以在不重新加载页面的情况下更改 URL，处理 URL 的生成以及编码。我们将在后面看到如何从这些功能中获益。

## `router-link` 的Props：

### to

- to是必须设置的属性。
- 表示目标路由的链接。当被点击后，内部会立刻把 `to` 的值传到 `router.push()`，所以这个值可以是一个**字符串**或者是**描述目标位置的对象**。

```html
<!-- 字符串 -->
<router-link to="/home">Home</router-link>
<!-- 渲染结果 -->
<a href="/home">Home</a>

<!-- 使用 v-bind 的 JS 表达式 -->
<router-link :to="'/home'">Home</router-link>

<!-- 同上 -->
<router-link :to="{ path: '/home' }">Home</router-link>

<!-- 命名的路由 -->
<router-link :to="{ name: 'user', params: { userId: '123' }}">User</router-link>

<!-- 带查询参数，下面的结果为 `/register?plan=private` -->
<router-link :to="{ path: '/register', query: { plan: 'private' }}">
  Register
</router-link>
```

### replace

- 默认值为 false。
- 设置 `replace` 属性的话，当点击时，会调用 `router.replace()` 而不是 `router.push()`，于是导航后不会留下 history 记录。

```html
<router-link to="/abc" replace></router-link>
```

### tag

- 默认值: `"a"`。
-  `tag` 指定渲染为何种标签，默认为a标签。设置为其他标签它还是会监听点击，触发导航。

```html
<router-link to="/foo">foo</router-link>
<!-- 渲染结果 -->
<a to="/foo">foo<a>

<router-link to="/foo" tag="li">foo</router-link>
<!-- 渲染结果 -->
<li>foo</li>
```

### active-class

- 默认值: `"router-link-active"`。

- 设置链接激活时使用的 CSS 类名。默认值可以通过路由的构造选项 `linkActiveClass` 来全局配置。

### exact

- 默认值: `false`。

- “是否激活”默认类名的依据是**包含匹配**。 举个例子，如果当前的路径是 `/a` 开头的，那么 `<router-link to="/a">` 也会被设置 CSS 类名。

按照这个规则，每个路由都会激活 `<router-link to="/">`！想要链接使用“精确匹配模式”，则使用 `exact` 属性：

```html
<!-- 这个链接只会在地址为 / 的时候被激活 -->
<router-link to="/" exact></router-link>
```

### exact-active-class

- 默认值: `"router-link-exact-active"`。

- 配置当链接被精确匹配的时候应该激活的 class。注意默认值也是可以通过路由构造函数选项 `linkExactActiveClass` 进行全局配置的。

### event

- 默认值: `'click'`。

- 声明可以用来触发导航的事件。可以是一个字符串或是一个包含字符串的数组。
- 给 `<router-link>`绑定原生DOM事件依然需要添加 `.native` 修饰符

# router-view 组件

## 作用

`router-view` 将显示与 url 对应的组件。你可以把它放在任何地方，以适应你的布局。

`<router-view>` 组件是一个 functional 组件，渲染路径匹配到的视图组件。`<router-view>` 渲染的组件还可以内嵌自己的 `<router-view>`，根据嵌套路径，渲染嵌套组件。

- 其他属性 (非 router-view 使用的属性) 都直接传给渲染的组件，比如 calss属性， 很多时候，每个路由的数据都是包含在路由参数中。

- 因为它也是个组件，所以可以配合 `<transition>` 和 `<keep-alive>` 使用。如果两个结合一起用，要确保在内层使用 `<keep-alive>`：

```html
<transition>
  <keep-alive>
    <router-view></router-view>
  </keep-alive>
</transition>
```

## **`router-view` 的Props**：

`name`：

- 默认值: `"default"`。
- 如果 `<router-view>`设置了名称，则会渲染对应的路由配置中 `components` 下的相应组件，常用于命名组件。

## 路由组件缓存

当路由匹配到新的组件之后，原来的组件被销毁，新的组件被创建。

路由缓存：使用keep-alive包裹router-view把切换出去的组件缓存起来。

# new VueRouter()

`new VueRouter()` 用于创建一个可以被 Vue 应用程序使用的路由管理器实例，参数是一个配置对象，有以下可以传递的属性列表：

```js
new VueRouter({
  mode:'',
  base:'',
  linkActiveClass:'',
  linkExactActiveClass:'',
  routes:[],
  scrollBehavior(){},
  parseQuery / stringifyQuery: Function,
  fallback: true,
});
```

## mode

- 默认值: `"hash" (浏览器环境) | "abstract" (Node.js 环境)`
- 可选值: `"hash" | "history" | "abstract"`
- 配置路由模式:
  - `hash`: 使用 URL hash 值来作路由。支持所有浏览器，包括不支持 HTML5 History Api 的浏览器。
  - `history`: 依赖 HTML5 History API 和服务器配置。
  - `abstract`: 支持所有 JavaScript 运行环境，如 Node.js 服务器端。**如果发现没有浏览器的 API，路由会自动强制进入这个模式。**

## base

- 默认值: `"/"`
- 应用的基路径。例如，如果整个单页应用服务在 `/app/` 下，然后 `base` 就应该设为 `"/app/"`。

## linkActiveClass

- 默认值: `"router-link-active"`

- 全局配置 `<router-link>` 默认的激活的 class。如果什么都没提供，则会使用 router-link-active。

## linkExactActiveClass

- 默认值: `"router-link-exact-active"`
- 全局配置 `<router-link>` 默认的精确激活的 class。如果什么都没提供，则会使用 `router-link-exact-active`。

## routes

当用户通过 `routes`  选项或者 [`router.addRoute()`](https://router.vuejs.org/zh/api/#addroute) 来添加路由时，可以得到路由记录。 有三种不同的路由记录:

- 单一视图记录：有一个 `component` 配置
- 多视图记录：有一个 `components` 配置
- 重定向记录：没有 `component` 或 `components` 配置，因为重定向记录永远不会到达。

`routes` 选项用来配置应该添加到路由的初始路由列表，路由表数组中存放的是一个一个的路由对象。

我们称呼 `routes` 配置中的每个路由对象为 **路由记录**。路由记录可以是嵌套的，在 `children` 数组存放的路由对象也是路由记录。

```js
const router = new VueRouter({
  routes: [
    // 下面的对象就是路由记录
    {
      path: '/foo',
      component: Foo,
      children: [
        // 这也是个路由记录
        { path: 'bar', component: Bar }
      ]
    }
  ]
})
```

路由记录的值是一个对象类型 `RouteConfig` ，路由记录有以下可以传递的属性列表：

```js
interface RouteConfig = {
  path: string,// 导航路径
  component?: Component,// 路由导航链接匹配的组件
  name?: string, // 命名路由
  components?: { [name: string]: Component }, // 命名视图组件
  redirect?: string | Location | Function, // 重定向
  props?: boolean | Object | Function, // props参数
  alias?: string | Array<string>, // 别名
  children?: Array<RouteConfig>, // 嵌套路由
  beforeEnter?: (to: Route, from: Route, next: Function) => void,
  meta?: any, // 元信息
  caseSensitive?: boolean, // 匹配规则是否大小写敏感？(默认值：false)
  pathToRegexpOptions?: Object // 编译正则的选项
}
```

- `path`：路由导航链接的路径，和router-link组件的to属性保持一致。应该以 `/` 开头，除非该记录是另一条记录的子记录。可以定义参数：`/users/:id` 匹配 `/users/1` 以及 `/users/posva`。

- `component`：路由导航链接匹配的组件。

- `name`：路由记录独一无二的名称。

- `components`：命名视图的字典，如果没有，包含一个键为 `default` 的对象。

- `redirect`：如果路由是直接匹配的，那么重定向到哪里呢。重定向发生在所有导航守卫之前，并以新的目标位置触发一个新的导航。也可以是一个接收目标路由地址并返回我们应该重定向到的位置的函数。

- `props`：允许将参数作为 props 传递给由 `router-view` 渲染的组件。当传递给一个**多视图记录**时，它应该是一个与`组件`具有相同键的对象，或者是一个应用于每个组件的`布尔值`。

- `alias`：路由的别名。允许定义类似记录副本的额外路由。这使得路由可以简写为像这种 `/users/:id` 和 `/u/:id`。 **所有的 `alias` 和 `path` 值必须共享相同的参数**。

- `children`：当前记录的嵌套路由。

- `beforeEnter`：允许将参数作为 props 传递给由 `router-view` 渲染的组件。当传递给一个**多视图记录**时，它应该是一个与`组件`具有相同键的对象，或者是一个应用于每个组件的`布尔值`。

- `meta`：路由元信息，在记录上附加自定义数据。

- `caseSensitive`：使路由匹配区分大小写，默认为`false`。注意这也可以在路由级别上设置。

- `pathToRegexpOptions`：编译正则的选项

## scrollBehavior

- `scrollBehavior` 在页面之间导航时控制滚动的函数。

## parseQuery / stringifyQuery

- 提供自定义查询字符串的解析/反解析函数。覆盖默认行为。

## fallback

- 默认值: `true`
- 当浏览器不支持 `history.pushState` 控制路由是否应该回退到 `hash` 模式。默认值为 `true`。
- 在 IE9 中，设置为 `false` 会使得每个 `router-link` 导航都触发整页刷新。它可用于工作在 IE9 下的服务端渲染应用，因为一个 hash 模式的 URL 并不支持服务端渲染。

# 根组件注入路由后的信息

## 注入的信息

通过在 Vue 根实例的 `router` 配置传入 router 实例，会给Vue实例的根组件及其所有的子级组件注入以下信息：

- 注入的属性，下面这些属性成员会被注入到每个子组件：

  - `this.$router`：router 实例。和 new VueRouter() 创建的路由管理器对象功能一致。

  - `this.$route`：当前激活的路由对象（路由信息对象）。这个属性是只读的，里面的属性是 immutable (不可变) 的，不过你可以 watch (监测变化) 它。

- 增加的组件配置选项，这些都是组件内的守卫：

  - **beforeRouteEnter**

  - **beforeRouteUpdate**

  - **beforeRouteLeave**

## `$router` 与 `$route`的理解

### `$router` 与 `$route`的关系：

一个Vue应用只有一个路由管理器对象router，当匹配一条路由记录就会生成一个新的路由对象route，多个路由对象route是通过路由管理器对象router来管理的。

- `$router` 是路由管理器对象，在组件内使用 `$router` 获取，可以获取当前路由对象，也可以调用一些函数控制路由前进、后退、绑定路由导航守卫等。
- `$route` 是路由对象，在组件内使用 `$route` 获取，表示当前激活的路由的状态信息，存储了当前路由的路径信息、路径参数等内容。

### `$router` 与 `$route`的访问：

在 Vue 根实例的 `router` 配置项传入 router 实例后：

- 在任意组件实例中可以通过 `this.$router`来访问路由管理对象 `router`，或者在组件的模板中使用 `$router` 
- 在任意组件实例中可以通过 `this.$route`来访问路由对象 ，或者在组件的模板中使用 `$route`

### `$router`和 `new VueRouter()`的关系：

一个Vue应用只有一个路由管理器对象router，在整个文档中，我们会经常使用 `router` 实例，请记住：

- 在组件中获取的`this.$router` 与直接使用通过 `new VueRouter()`创建的 `router` 实例完全相同。
- 我们使用 `this.$router` 的原因是，我们不想在每个需要操作路由的组件中都导入路由。

### 每个组件内的 `$route` 是不一样的

每一个路由组件都有自己的路由对象，使用 `$route` 获取：在组件内通过 `this.$route` 使用；在组件的模板中可以直接使用 `$route`。

`$route`：路由对象是不可变 (immutable) 的，每次成功的导航后都会产生一个新的路由对象。所以在每一个匹配成功的路由组件中都有一个`$route`。

# 路由管理器实例 router

## router 实例属性

- `router.app`：配置了 `router` 的 Vue 根实例。

- `router.mode`：路由使用的模式。

- `router.currentRoute`：当前**路由管理器**对应的**路由对象**。

- `router.START_LOCATION`：以路由对象的格式展示初始路由地址，即路由开始的地方。可用在导航守卫中以区分初始导航。

```js
import VueRouter from 'vue-router'

const router = new VueRouter({
  // ...
})

router.beforeEach((to, from) => {
  if (from === VueRouter.START_LOCATION) {
    // 初始导航
  }
})
```

## router 实例方法

### 全局的导航守卫

- router.beforeEach

- router.beforeResolve

- router.afterEach

```js
router.beforeEach((to, from, next) => {
  /* 必须调用 `next` */
})

router.beforeResolve((to, from, next) => {
  /* 必须调用 `next` */
})

router.afterEach((to, from) => {})
```

### 编程式导航

- router.push

- router.replace

- router.go

- router.back

- router.forward

```js
router.push(location, onComplete?, onAbort?)
router.push(location).then(onComplete).catch(onAbort)
router.replace(location, onComplete?, onAbort?)
router.replace(location).then(onComplete).catch(onAbort)
router.go(n)
router.back()
router.forward()
```

### 添加获取路由

- `router.resolve`：解析目标位置 (格式和 `<router-link>` 的 `to` prop 一样)。

  - `current` 是当前默认的路由 (通常你不需要改变它)

  - `append` 允许你在 `current` 路由上附加路径 (如同 [`router-link`](https://v3.router.vuejs.org/zh/api/#router-link.md-props))

```JS
const resolved: {
  location: Location;
  route: Route;
  href: string;
} = router.resolve(location, current?, append?)
```

- `router.addRoutes`：动态添加更多的路由规则。参数必须是一个符合 `routes` 选项要求的数组。

> *已废弃*：使用 `router.addRoute()`代替。

```js
router.addRoutes(routes: Array<RouteConfig>)
```

- `router.addRoute`：添加一条新路由规则。如果该路由规则有 `name`，并且已经存在一个与之相同的名字，则会覆盖它。

```ts
addRoute(route: RouteConfig): () => void
```

- `router.addRoute`：添加一条新的路由规则记录作为现有路由的子路由。如果该路由规则有 `name`，并且已经存在一个与之相同的名字，则会覆盖它。

```ts
addRoute(parentName: string, route: RouteConfig): () => void
```

- `router.getRoutes`：获取所有活跃的路由记录列表。**注意只有文档中记录下来的 property 才被视为公共 API**，避免使用任何其它 property，例如 `regex`，因为它在 Vue Router 4 中不存在。

### 路由事件

- `router.onReady`

```js
router.onReady(callback, [errorCallback])
```

该方法把一个回调排队，在路由完成初始导航时调用，这意味着它可以解析所有的异步进入钩子和路由初始化相关联的异步组件。

这可以有效确保服务端渲染时服务端和客户端输出的一致。

第二个参数 `errorCallback` 只在 2.4+ 支持。它会在初始化路由解析运行出错 (比如解析一个异步组件失败) 时被调用。

- `router.onError`

```js
router.onError(callback)
```

注册一个回调，该回调会在路由导航过程中出错时被调用。注意被调用的错误必须是下列情形中的一种：

- 错误在一个路由守卫函数中被同步抛出；
- 错误在一个路由守卫函数中通过调用 `next(err)` 的方式异步捕获并处理；
- 渲染一个路由的过程中，需要尝试解析一个异步组件时发生错误。

# 路由对象实例 $route

## 路由对象

我们称呼 `routes` 配置中的每个路由对象为 **路由记录**。一个路由匹配到的所有路由记录会暴露为 `$route` 对象，称作 **路由对象**。

- 一个**路由对象 (route object)** 表示当前激活的路由的状态信息，包含了当前 URL 解析得到的信息，还有 URL 匹配到的**路由记录 (route records)**。

- **路由对象**是不可变 (immutable) 的，每次成功的导航后都会产生一个新的对象。

## $route出现的位置

路由对象出现在多个地方:

- 在组件内，即 `this.$route`

- 在 `$route` 观察者回调内

- `router.match(location)` 的返回值

- 导航守卫的参数：

  ```js
  router.beforeEach((to, from, next) => {
    // `to` 和 `from` 都是路由对象
  })
  ```

- `scrollBehavior` 方法的参数:

  ```js
  const router = new VueRouter({
    scrollBehavior(to, from, savedPosition) {
      // `to` 和 `from` 都是路由对象
    }
  })
  ```

## $route 属性

- `$route.path`：当前路由的路径，总是解析为绝对路径，如 `"/foo/bar"`。

- `$route.params`：一个 key/value 对象，包含了动态片段和全匹配片段，如果没有路由参数，就是一个空对象。

- `$route.query`：一个 key/value 对象，表示 URL 查询参数。例如，对于路径 `/foo?user=1`，则有 `$route.query.user == 1`，如果没有查询参数，则是个空对象。

- `$route.hash`：当前路由的 hash 值 (带 `#`) ，如果没有 hash 值，则为空字符串。

- `$route.fullPath`：完成解析后的 URL，包含查询参数和 hash 的完整路径。

- `$route.meta`：附加到从父级到子级合并（非递归）的所有匹配记录的任意数据。

- `$route.matched`：一个数组，包含当前路由的所有嵌套路径片段的**路由记录** 。路由记录就是 `routes` 配置数组中的对象副本 (还有在 `children` 数组)。

  ```js
  const router = new VueRouter({
    routes: [
      // 下面的对象就是路由记录
      {
        path: '/foo',
        component: Foo,
        children: [
          // 这也是个路由记录
          { path: 'bar', component: Bar }
        ]
      }
    ]
  })
  ```

  当 URL 为 `/foo/bar`，`$route.matched` 将会是一个包含从上到下的所有对象 (副本)。

- `$route.name`：当前路由的名称，如果有的话。

- `$route.redirectedFrom`：如果存在重定向，即为重定向来源的路由的名字。

# 命名路由

## 使用name命名路由

除了 `path` 之外，你还可以为任何路由提供 `name`。这有以下优点：

- 没有硬编码的 URL
- `params` 的自动编码/解码。
- 防止你在 url 中出现打字错误。
- 绕过路径排序（如显示一个）

```js
const routes = [
  {
    path: '/user/:username',
    name: 'user',
    component: User,
  },
]
```

## 使用name导航路由

要链接到一个命名的路由，可以向 `router-link` 组件的 `to` 属性传递一个对象：

```html
<router-link :to="{ name: 'user', params: { username: 'erina' }}">
  User
</router-link>
```

这跟代码调用 `router.push()` 是一回事：

```js
router.push({ name: 'user', params: { username: 'erina' } })
```

在这两种情况下，路由将导航到路径 `/user/erina`。

## 命名路由案例

```html
<div id="app">
  <nav id="nav-list">
    <div class="w">
      <router-link :to="{name:'home'}">首页</router-link>
      <router-link :to="{name:'friend'}">好友</router-link>
      <router-link :to="{name:'setting'}">设置</router-link>
    </div>
  </nav>
  <div class="w">
    <router-view id="view"></router-view>
  </div>
</div>

<script>
  const home = { template: '<header>首页</header>'}
  const friend = { template: '<header>好友</header>'}
  const setting = {template: '<header>设置</header>',}

  const router = new VueRouter({ 
    routes: [
      { path: '/', component: home, name: 'home' },
      { path: '/friend', component: friend, name: 'friend' },
      { path: '/setting', component: setting, name: 'setting' },
    ]
  });

  new Vue({
    el: '#app',
    router,
  });
</script>
```

# 嵌套路由

## 概念

一些应用程序的 UI 由多层嵌套的组件组成。在这种情况下，URL 的片段通常对应于特定的嵌套组件结构，例如：

```
/setting/login                     /user/regist
+------------------+                  +-----------------+
| setting          |                  | Setting         |
| +--------------+ |                  | +-------------+ |
| | login        | |  +------------>  | | regist      | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

通过 Vue Router，你可以使用嵌套路由配置来表达这种关系。

## 顶层路由出口

顶层的路由出口（主路由出口）：

- 在根组件模板中添加 `<router-view></router-view>` 是主路由出口
- 主路由出口只有一个
- 主路由出口渲染顶层路由匹配的组件

```html
<div id="app">
  <nav id="nav-list">
    <div class="w">
      <router-link to="/">首页</router-link>
      <router-link to="/friend">好友</router-link>
      <router-link to="/setting">设置</router-link>
      <router-link to="/setting/login">登录</router-link>
      <router-link to="/setting/regist">注册</router-link>
    </div>
  </nav>

  <!-- 顶层路由出口 -->
  <div class="w">
    <router-view id="view"></router-view>
  </div>
</div>
```

## 嵌套的路由出口

同样地，一个被渲染的路由组件也可以包含自己嵌套的 `<router-view>`。例如，如果我们在 `Setting` 组件的模板内添加一个 `<router-view>`：

- 在setting组件中的`<router-view></router-view>`是嵌套的路由出口（子路由的出口）
- 子路由出口可能有很多个
- 子路由出口渲染的是嵌套路由匹配的组件

```js
const Home = { template: '<h1>首页</h1>' };
const Friend = { template: '<h1>好友</h1>' };
const Setting = { template: '<div><h1>设置</h1><router-view></router-view></div>' };
const Login = {template: '<div>登录</div>'};
const Regist = {template: '<div>注册</div>'};
```

## 嵌套的路由

要将组件渲染到这个嵌套的 `router-view` 中，我们需要在路由中配置 `children`：

- children 用来添加子路由，每一个路由都可以添加子路由，一个子路由对应一个子路由的出口
- 如你所见，`children` 配置只是另一个路由数组，就像 `routes` 本身一样。因此，你可以根据自己的需要，不断地嵌套视图。
- 要注意，以 / 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。

```js
const router = new VueRouter({
  routes: [
    // 
    { path: '/', component: Home },
    { path: '/friend', component: Friend },
    {
      path: '/setting',
      component: Setting,
      // children 用来添加子路由，每一个路由都可以添加子路由，一个子路由对应一个子路由的出口
      children: [
        // 路径不是以/开头，会把上级路径/setting作为嵌套的路径
        // 当 /setting/login 匹配成功的时候，Login 将被渲染到 Setting 的 <router-view> 内部
        // router-link组件to属性的值为 /setting/login
        { path: 'login', component: Login },
        // 路径是以/开头：要注意，以 / 开头的嵌套路径会被当作根路径。 这让你充分的使用嵌套组件而无须设置嵌套的路径。
        // 当 /login 匹配成功的时候，Login 将被渲染到 Setting 的 <router-view> 内部
        // router-link组件to属性的值为 /login
        // { path: '/login', component: login },
        { path: 'regist', component: Regist },
      ],
    },
  ],
});
```

## 带路径参数的嵌套

路由导航链接改为：

```html
<div id="app">
  <nav id="nav-list">
    <div class="w">
      <router-link to="/">首页</router-link>
      <router-link to="/friend">好友</router-link>
      <router-link to="/setting/tom">设置</router-link>
      <router-link to="/setting/tom/login">登录</router-link>
      <router-link to="/setting/tom/regist">注册</router-link>
    </div>
  </nav>

  <!-- 顶层路由出口 -->
  <div class="w">
    <router-view id="view"></router-view>
  </div>
</div>
```

增加`SettingHome`路由组件：

```js
const Home = { template: '<h1>首页</h1>' };
const Friend = { template: '<h1>好友</h1>' };
const Setting = { template: '<div><h1>好友</h1><router-view></router-view></div>' };
const SettingHome = { template: '<div>SettingHome</div>' };
const Login = { template: '<div>登录</div>' };
const Regist = { template: '<div>注册</div>' };
```

此时，按照上面的配置，当你访问 `/setting/tom` 时，在 `Setting` 的 `router-view` 里面什么都不会呈现，因为没有匹配到嵌套路由。也许你确实想在那里渲染一些东西。在这种情况下，你可以提供一个空的嵌套路径：

```js
const routes = [
  {
    path: '/setting/:name',
    component: Setting,
    children: [
      // 当/setting/:name 匹配成功
      // SettingHome 将被渲染到 Setting 的 <router-view> 内部
      { path: '', component: SettingHome },

      // ...其他子路由
    ],
  },
]
```

## 嵌套的命名路由

在处理命名路由时，**你通常会给子路由命名**：

```js
const routes = [
  {
    path: '/setting/:name',
    component: Setting,
    // 请注意，只有子路由具有名称
    children: [{ path: '', name: 'setting', component: SettingHome }],
  },
]
```

这将确保导航到 `/setting/:name` 时始终显示嵌套路由。

在一些场景中，你可能希望导航到命名路由而不导航到嵌套路由。例如，你想导航 `/setting/:name` 而不显示嵌套路由。那样的话，你还可以**命名父路由**，但请注意**重新加载页面将始终显示嵌套的子路由**，因为它被视为指向路径`/setting/:name` 的导航，而不是命名路由：

```js
const routes = [
  {
    path: '/setting/:name',
    name: 'setting-parent'
    component: User,
    children: [{ path: '', name: 'setting', component: SettingHome }],
  },
]
```

# 命名视图

## 非嵌套的命名视图

有时候想同时 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 `sidebar` (侧导航) 和 `main` (主内容) 两个视图，这个时候命名视图就派上用场了。

你可以在界面中拥有多个单独命名的视图，而不是只有一个单独的出口。

- 如果 `router-view` 没有设置名字，那么默认为 `default`
- 如果 `router-view` 有名字，则使用name属性设置视图的名字

```html

<div id="app">
  <nav id="nav-list">
    <div class="w">
      <router-link to="/">首页</router-link>
      <router-link to="/friend">好友</router-link>
      <router-link to="/setting">设置</router-link>
    </div>
  </nav>

  <div class="w">
     <!-- 如果 router-view 有名字，则使用name属性设置视图的名字 -->
    <router-view class="view left-siderbar" name="LeftSidebar"></router-view>
     <!-- 如果 router-view 没有设置名字，那么默认为 default -->
    <router-view class="view main-content"></router-view>
    <router-view class="view right-sidebar" name="RightSidebar"></router-view>
  </div>
</div>
```

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 `components` 配置 (带上 s)：

```js
const home = { template: '<header>首页</header>' };
const friend = { template: '<header>好友</header>' };
const MainContent = { template: '<header>主要内容</header>' };
const LeftSidebar = { template: '<div>左侧边导航</div>' };
const RightSidebar = { template: '<div>右侧边导航</div>' };

const router = new VueRouter({
  routes: [
    { path: '/', component: home },
    { path: '/friend', component: friend },
    {
      path: '/setting',
      // 匹配命名视图的组件
      components: {
        default: MainContent,
        LeftSidebar,
        RightSidebar,
      },
    },
  ],
});
```

## 嵌套命名视图

我们也有可能使用命名视图创建嵌套视图的复杂布局。这时你也需要命名用到的嵌套 `router-view` 组件。我们以一个设置面板为例：

```text
/settings/emails                                       /settings/profile
+-----------------------------------+                  +------------------------------+
| UserSettings                      |                  | UserSettings                 |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
| | Nav | UserEmailsSubscriptions | |  +------------>  | | Nav | UserProfile        | |
| |     +-------------------------+ |                  | |     +--------------------+ |
| |     |                         | |                  | |     | UserProfilePreview | |
| +-----+-------------------------+ |                  | +-----+--------------------+ |
+-----------------------------------+                  +------------------------------+
```

- `Nav` 只是一个常规组件。
- `UserSettings` 是一个视图组件。
- `UserEmailsSubscriptions`、`UserProfile`、`UserProfilePreview` 是嵌套的视图组件。

**注意**：我们先忘记 HTML/CSS 具体的布局的样子，只专注在用到的组件上。

`UserSettings` 组件的 `<template>` 部分应该是类似下面的这段代码：

```html
<!-- UserSettings.vue -->
<div>
  <h1>User Settings</h1>
  <NavBar/>
  <router-view/>
  <router-view name="helper"/>
</div>
```

嵌套的视图组件在此已经被忽略了，但是你可以在[这里 ](https://jsfiddle.net/posva/22wgksa3/)找到完整的源代码。

然后你可以用这个路由配置完成该布局：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/settings',
      // 你也可以在顶级路由就配置命名视图
      component: UserSettings,
      children: [
        {
          path: 'emails',
          component: UserEmailsSubscriptions,
        }, 
        {
          path: 'profile',
          components: {
            default: UserProfile,
            helper: UserProfilePreview
          },
        },
      ]
    },
  ]
})
```

# 重定向和别名

## 重定向

重定向也是通过 `routes` 配置来完成，下面例子是从 `/home` 重定向到 `/`：

```js
const routes = [{ path: '/home', redirect: '/' }]
```

重定向的目标也可以是一个命名的路由：

```js
const routes = [{ path: '/home', redirect: { name: 'homepage' } }]
```

甚至是一个方法，动态返回重定向目标：

```js
const routes = [
  {
    // /search/screens -> /search?q=screens
    path: '/search/:searchText',
    redirect: to => {
      // 方法接收目标路由作为参数
      // return 重定向的字符串路径/路径对象
      return { path: '/search', query: { q: to.params.searchText } }
    },
  },
  {
    path: '/search',
    // ...
  },
]
```

请注意，**导航守卫并没有应用在跳转路由上，而仅仅应用在其目标上**。在上面的例子中，在 `/home` 路由中添加 `beforeEnter` 守卫不会有任何效果。

在写 `redirect` 的时候，可以省略 `component` 配置，因为它从来没有被直接访问过，所以没有组件要渲染。唯一的例外是嵌套路由：如果一个路由记录有 `children` 和 `redirect` 属性，不能省略 `component` 属性。

```html
<div id="app">
  <nav id="nav-list">
    <div class="w">
      <router-link to="/home">首页</router-link>
      <router-link to="/friend">好友</router-link>
      <router-link to="/setting">设置</router-link>
      <router-link to="/setting/login">登录2</router-link>
      <router-link to="/setting/regist">注册</router-link>
      <hr />
    </div>
  </nav>
  <div class="w">
    <router-view id="view"></router-view>
  </div>
</div>

<script>
  const Home = { template: '<h1>首页</h1>' };
  const Friend = { template: '<h1>好友</h1>' };
  const Setting = { template: '<div><h1>设置</h1><router-view></router-view></div>' };
  const SettingHome = { template: '<div>SettingHome</div>' };
  const Login = { template: '<div>登录</div>' };
  const Regist = { template: '<div>注册</div>' };
  const Other = { template: '<div>其他</div>' };

  const router = new VueRouter({
    routes: [
      { path: '/home', redirect: '/' },
      { path: '/', component: Home },
      { path: '/friend', component: Friend },
      { path: '/other', component: Other },
      {
        path: '/setting',
        redirect: '/Other' ,
        component: Setting,// component不能省略，因为还要匹配显示嵌套的路由
        children: [
          { path: 'login', component: Login },
          { path: 'regist', component: Regist },
        ],
      },
    ],
  });

  new Vue({
    el: '#app',
    router,
  });
</script>
```

## 相对重定向

也可以重定向到相对位置：

```js
const routes = [
  {
    // 将总是把/users/123/posts重定向到/users/123/profile。
    path: '/users/:id/posts',
    redirect: to => {
      // 该函数接收目标路由作为参数
      // 相对位置不以`/`开头
      // 或 { path: 'profile'}
      return 'profile'
    },
  },
]
```

## 别名

重定向是指当用户访问 `/home` 时，URL 会被 `/` 替换，然后匹配成 `/`。那么什么是别名呢？

**将 `/` 别名为 `/home`，意味着当用户访问 `/home` 时，URL 仍然是 `/home`，但会被匹配为用户正在访问 `/`。**

上面对应的路由配置为：

```js
const routes = [{ path: '/', component: Homepage, alias: '/home' }]
```

通过别名，你可以自由地将 UI 结构映射到一个任意的 URL，而不受配置的嵌套结构的限制。使别名以 `/` 开头，以使嵌套路径中的路径成为绝对路径。你甚至可以将两者结合起来，用一个数组提供多个别名：

```js
const routes = [
  {
    path: '/users',
    component: UsersLayout,
    children: [
      // 为这 3 个 URL 呈现 UserList
      // - /users
      // - /users/list
      // - /people
      { path: '', component: UserList, alias: ['/people', 'list'] },
    ],
  },
]
```

如果你的路由有参数，请确保在任何绝对别名中包含它们：

```js
const routes = [
  {
    path: '/users/:id',
    component: UsersByIdLayout,
    children: [
      // 为这 3 个 URL 呈现 UserDetails
      // - /users/24
      // - /users/24/profile
      // - /24
      { path: 'profile', component: UserDetails, alias: ['/:id', ''] },
    ],
  },
]
```

# 编程式导航和声明式导航

**路由导航：“导航”表示路由正在发生改变。**

路由导航有两种形式：分为声明式和编程式：

| 声明式                    | 编程式             |
| ------------------------- | ------------------ |
| `<router-link :to="...">` | `router.push(...)` |

## 声明式导航

声明式：`<router-link :to="...">` 。

当你点击 `<router-link>` 时，内部会调用 `router.push(...)`，所以点击 `<router-link :to="...">` 相当于调用 `router.push(...)` ：

## 编程式导航

编程式导航：`router.push(...)`。 不借助 `<router-link>`  实现路由跳转，让路由跳转更加灵活 

该方法的参数可以是一个字符串路径，或者一个描述地址的对象。例如：

```js
// 字符串路径
router.push('/users/eduardo')

// 带有路径的对象
router.push({ path: '/users/eduardo' })

// 命名的路由，并加上参数，让路由建立 url
router.push({ name: 'user', params: { username: 'eduardo' } })

// 带查询参数，结果是 /register?plan=private
router.push({ path: '/register', query: { plan: 'private' } })

// 带 hash，结果是 /setting#team
router.push({ path: '/setting', hash: '#team' })
```

**注意**：如果提供了 `path`，`params` 会被忽略，上述例子中的 `query` 并不属于这种情况。取而代之的是下面例子的做法，你需要提供路由的 `name` 或手写完整的带有参数的 `path` ：

```js
const username = 'eduardo'
// 我们可以手动建立 url，但我们必须自己处理编码
router.push(`/user/${username}`) // -> /user/eduardo
// 同样
router.push({ path: `/user/${username}` }) // -> /user/eduardo

// 如果可能的话，使用 `name` 和 `params` 从自动 URL 编码中获益
router.push({ name: 'user', params: { username } }) // -> /user/eduardo

// `params` 不能与 `path` 一起使用
router.push({ path: '/user', params: { username } }) // -> /user
```

当指定 `params` 时，可提供 `string` 或 `number` 参数（或者对于可**重复的参数**可提供一个数组）。**任何其他类型（如 `undefined`、`false` 等）都将被自动字符串化**。对于**可选参数**，你可以提供一个空字符串（`""`）来跳过它。

由于属性 `to` 与 `router.push` 接受的对象种类相同，所以两者的规则完全相同：

```html
<router-link to="/users/eduardo"></router-link>

<router-link :to="{ path: '/users/eduardo' }"></router-link>

<router-link :to="{ name: 'user', params: { username: 'eduardo' } }"></router-link>

<router-link :to="{ path: '/register', query: { plan: 'private' } }"></router-link>
```

使用 `<router-link :to="...">` 等价于 router.push(...) 会增加一条历史记录。

不管是声明式的导航，还是编程式的导航，想要导航到不同的 URL，都会调用 `router.push` 方法。这个方法会向 history 栈添加一个新的记录，所以，当用户点击浏览器后退按钮时，可以回到之前的 URL。

`router.push` 和所有其他导航方法都会返回一个 Promise 对象，让我们可以等到导航完成后才知道是成功还是失败。

```js
router.push('/users/eduardo').then(()=>{});
```

## 替换当前位置

它的作用类似于 `router.push`，唯一不同的是，它在导航时不会向 history 添加新记录，正如它的名字所暗示的那样——它取代了当前的条目。

| 声明式                            | 编程式                |
| --------------------------------- | --------------------- |
| `<router-link :to="..." replace>` | `router.replace(...)` |

也可以直接在传递给 `router.push` 的 `routeLocation` 中增加一个属性 `replace: true` ：

```js
router.push({ path: '/home', replace: true })
// 相当于
router.replace({ path: '/home' })
```

## 横跨历史

该方法采用一个整数作为参数，表示在历史堆栈中前进或后退多少步，类似于 `window.history.go(n)`。

```js
// 前进：向前移动一条记录，与 router.forward() 相同
router.go(1);
router.forward();

// 后退：返回一条记录，与 router.back() 相同
router.go(-1);
router.back();

// 前进 3 条记录
router.go(3);

// 如果没有那么多记录，静默失败
router.go(-100);
router.go(100;
```

## 篡改历史

你可能已经注意到，`router.push`、`router.replace` 和 `router.go` 是 [`window.history.pushState`、`window.history.replaceState` 和 `window.history.go`](https://developer.mozilla.org/en-US/docs/Web/API/History) 的翻版，它们确实模仿了 `window.history` 的 API。

因此，如果你已经熟悉 [Browser History APIs](https://developer.mozilla.org/en-US/docs/Web/API/History_API)，在使用 Vue Router 时，操作历史记录就会觉得很熟悉。

还有值得提及的，Vue Router 的导航方法 (`push`、 `replace`、 `go`) 在各类路由模式 (`history`、 `hash` 和 `abstract`) 下表现一致,，都能始终正常工作。

# 动态路由匹配

## 带参数的动态路由匹配

很多时候，我们需要将给定匹配模式的路由映射到同一个组件。例如，我们可能有一个 `User` 组件，它应该对所有用户进行渲染，但用户 ID 不同。在 Vue Router 中，我们可以在路径中使用一个动态字段来实现，我们称之为 *路径参数* ：

```JS
const User = {
  template: '<div>User</div>',
}

// 这些都会传递给 `createRouter`
const routes = [
  // 动态字段以冒号开始
  { path: '/users/:id', component: User },
]
```

现在像 `/users/johnny` 和 `/users/jolyne` 这样的 URL 都会映射到同一个路由。

**路径参数** 用冒号 `:` 表示。当一个路由被匹配时，它的 `params` 的值将在每个组件中以 `this.$route.params` 的形式暴露出来。因此，我们可以通过更新 `User` 的模板来呈现当前的用户 ID：

```JS
const User = {
  template: '<div>User {{ $route.params.id }}</div>',
}
```

你可以在同一个路由中设置有多个 ***路径参数***，它们会映射到 `$route.params` 上的相应字段。例如：

| 匹配模式                       | 匹配路径                 | $route.params                            |
| ------------------------------ | ------------------------ | ---------------------------------------- |
| /users/:username               | /users/eduardo           | `{ username: 'eduardo' }`                |
| /users/:username/posts/:postId | /users/eduardo/posts/123 | `{ username: 'eduardo', postId: '123' }` |

例子：

```html
<div id="app">
  <nav>
    <div class="w">
      <router-link to="/">首页</router-link>
      <router-link to="/user/张三/1">张三</router-link>
      <router-link to="/user/李四/2">李四</router-link>
    </div>
  </nav>

  <div class="w">
    <router-view></router-view>
  </div>
</div>
<script>
  const home = { template: '<h1>首页</h1>' };
  const user = {
    template: `
          <div>
            <h1>好友信息</h1>
            <p>姓名：{{$route.params.username}}</p>
            <p>id：{{$route.params.id}}</p>
  				</div>`,
  };

  const router = new VueRouter({
    routes: [
      { path: '/', component: home },
      { path: '/user/:username/:id?', component: user },
    ],
  });

  new Vue({
    el: '#app',
    router,
  });
</script>
```

## 响应路由参数的变化

使用带有参数的路由时需要注意的是，当用户从 `/users/johnny` 导航到 `/users/jolyne` 时，**相同的组件实例将被重复使用**。因为两个路由都渲染同个组件，比起销毁再创建，复用则显得更加高效。**不过，这也意味着组件的生命周期钩子不会被调用**。

要对同一个组件中参数的变化做出响应的话，你可以简单地 watch `$route` 对象上的任意属性，在这个场景中，就是 `$route.params` ：

```js
const User = {
  template: '...',
  watch: {
    $route(to, from) {
      // 对路由变化作出响应...
      console.log('to, from: ', to, from);
    },
  },
}
```

或者，使用 `beforeRouteUpdate` 导航守卫，它也可以取消导航：

```js
const User = {
  template: '...',
  beforeRouteUpdate(to, from, next) {
    // 对路由变化做出响应...
    console.log('to, from: ', to, from);
    next();
  },
}
```

## 捕获所有路由或 404 Not found 路由

常规参数只匹配 url 片段之间的字符，用 `/` 分隔。如果我们想匹配**任意路径**，我们可以使用自定义的 ***路径参数*** 正则表达式，在 ***路径参数*** 后面的括号中加入 正则表达式 :

```js
const routes = [
  // 将匹配所有内容并将其放在 `$route.params.pathMatch` 下
  { path: '/:pathMatch(.*)*', name: 'NotFound', component: NotFound },
  // 将匹配以 `/user-` 开头的所有内容，并将其放在 `$route.params.afterUser` 下
  { path: '/user-:afterUser(.*)', component: UserGeneric },
]
```

简单写法：匹配**任意路径**，使用通配符 (`*`)：

```js
const routes = [
  // 将匹配所有内容并将其放在 `$route.params.pathMatch` 下
  { path: '*', name: 'NotFound', component: NotFound },
  // 将匹配以 `/user-` 开头的所有内容，并将其放在 `$route.params.pathMatch` 下
  { path: '/user-*', component: UserGeneric },
]
```

当使用***通配符***路由时，请确保路由的顺序是正确的，也就是说含有***通配符***的路由应该放在最后。路由 `{ path: '*' }` 通常用于客户端 404 错误。如果你使用了*History 模式*，请确保正确配置你的服务器。

在这个特定的场景中，我们在括号之间使用了**自定义正则表达式**，并将`pathMatch` 参数标记为可选可重复。这样做是为了让我们在需要的时候，可以通过将 `path` 拆分成一个数组，直接导航到路由：

```js
this.$router.push({
  name: 'NotFound',
  // 保留当前路径并删除第一个字符，以避免目标 URL 以 `//` 开头。
  params: { pathMatch: this.$route.path.substring(1).split('/') },
  // 保留现有的查询和 hash 值，如果有的话
  query: this.$route.query,
  hash: this.$route.hash,
})
```

## 高级匹配模式

Vue Router 使用自己的路径匹配语法，其灵感来自于 `express`，因此它支持许多高级匹配模式，如可选的参数，零或多个 / 一个或多个，甚至自定义的正则匹配规则。请查看**高级匹配**文档来探索它们。

## 匹配优先级

有时候，同一个路径可以匹配多个路由，此时，匹配的优先级就按照路由的定义顺序：路由定义得越早，优先级就越高。

# 路由的高级匹配

大多数应用都会使用 `/setting` 这样的静态路由和 `/users/:userId` 这样的动态路由，就像我们刚才在动态路由匹配中看到的那样，但是 Vue Router 可以提供更多的方式！

## 在参数中自定义正则

当定义像 `:userId` 这样的参数时，我们内部使用以下的正则 `([^/]+)` (至少有一个字符不是斜杠 `/` )来从 URL 中提取参数。这很好用，除非你需要根据参数的内容来区分两个路由。想象一下，两个路由 `/:orderId` 和 `/:productName`，两者会匹配完全相同的 URL，所以我们需要一种方法来区分它们。最简单的方法就是在路径中添加一个静态部分来区分它们：

```js
const routes = [
  // 匹配 /o/3549
  { path: '/o/:orderId' },
  // 匹配 /p/books
  { path: '/p/:productName' },
]
```

但在某些情况下，我们并不想添加静态的 `/o` `/p` 部分。由于，`orderId` 总是一个数字，而 `productName` 可以是任何东西，所以我们可以在括号中为参数指定一个自定义的正则：

```js
const routes = [
  // /:orderId -> 仅匹配数字
  { path: '/:orderId(\\d+)' },
  // /:productName -> 匹配其他任何内容
  { path: '/:productName' },
]
```

现在，转到 `/25` 将匹配 `/:orderId`，其他情况将会匹配 `/:productName`。`routes` 数组的顺序并不重要!

TIP

确保**转义反斜杠( `\` )**，就像我们对 `\d` (变成`\\d`)所做的那样，在 JavaScript 中实际传递字符串中的反斜杠字符。

## 可重复的参数

如果你需要匹配具有多个部分的路由，如 `/first/second/third`，你应该用 `*`（0 个或多个）和 `+`（1 个或多个）将参数标记为可重复：

```js
const routes = [
  // /:chapters ->  匹配 /one, /one/two, /one/two/three, 等
  { path: '/:chapters+' },
  // /:chapters -> 匹配 /, /one, /one/two, /one/two/three, 等
  { path: '/:chapters*' },
]
```

这将为你提供一个参数数组，而不是一个字符串，并且在使用命名路由时也需要你传递一个数组：

```js
// 给定 { path: '/:chapters*', name: 'chapters' },
router.resolve({ name: 'chapters', params: { chapters: [] } }).href
// 产生 /
router.resolve({ name: 'chapters', params: { chapters: ['a', 'b'] } }).href
// 产生 /a/b

// 给定 { path: '/:chapters+', name: 'chapters' },
router.resolve({ name: 'chapters', params: { chapters: [] } }).href
// 抛出错误，因为 `chapters` 为空 
```

这些也可以通过在**右括号后**添加它们与自定义正则结合使用：

```js
const routes = [
  // 仅匹配数字
  // 匹配 /1, /1/2, 等
  { path: '/:chapters(\\d+)+' },
  // 匹配 /, /1, /1/2, 等
  { path: '/:chapters(\\d+)*' },
]
```

## Sensitive 与 strict 路由配置

<div style="color:#f00;">v4.x版本的特性。</div>

默认情况下，所有路由是不区分大小写的，并且能匹配带有或不带有尾部斜线的路由。例如，路由 `/users` 将匹配 `/users`、`/users/`、甚至 `/Users/`。这种行为可以通过 `strict` 和 `sensitive` 选项来修改，它们可以既可以应用在整个全局路由上，又可以应用于当前路由上：

```js
const router = new VueRouter({
  routes: [
    // 将匹配 /users/posva 而非：
    // - /users/posva/ 当 strict: true
    // - /Users/posva 当 sensitive: true
    { path: '/users/:id', sensitive: true },
    // 将匹配 /users, /Users, 以及 /users/42 而非 /users/ 或 /users/42/
    { path: '/users/:id?' },
  ]
  strict: true, // applies to all routes
})
```

## 可选参数

你也可以通过使用 `?` 修饰符(0 个或 1 个)将一个参数标记为可选：

```js
const routes = [
  // 匹配 /users 和 /users/posva
  { path: '/users/:userId?' },
  // 匹配 /users 和 /users/42
  { path: '/users/:userId(\\d+)?' },
]
```

请注意，`*` 在技术上也标志着一个参数是可选的，但 `?` 参数不能重复。

## 调试

如果你需要探究你的路由是如何转化为正则的，以了解为什么一个路由没有被匹配，或者，报告一个 bug，你可以使用[路径排名工具](https://paths.esm.dev/?p=AAMeJSyAwR4UbFDAFxAcAGAIJXMAAA..#)。它支持通过 URL 分享你的路由。

# 路由传参总结

## 第一种：动态路径传参

- 在路由记录中配置动态路径 `{ path: '/user/:username/:id?', component: user }`，
- 在组件中使用 `$route.params.username` 和  `$route.params.id`获取参数

```html
<div id="app">
  <nav>
    <div class="w">
      <router-link to="/">首页</router-link>
      <router-link to="/user/张三/1">张三</router-link>
      <router-link to="/user/李四/2">李四</router-link>
    </div>
  </nav>

  <div class="w">
    <router-view></router-view>
  </div>
</div>
<script>
  const home = { template: '<h1>首页</h1>' };
  const user = {
    template: `
          <div>
            <h1>好友信息</h1>
            <p>姓名：{{$route.params.username}}</p>
            <p>id：{{$route.params.id}}</p>
  				</div>`,
  };

  const router = new VueRouter({
    routes: [
      { path: '/', component: home },
      { path: '/user/:username/:id?', component: user },
    ],
  });

  new Vue({
    el: '#app',
    router,
  });
</script>
```

## 第二种：params传参

在声明式或者编程式导航中只能使用`name`和`params`组合传参：

```html
<router-link :to="{ name: 'user', params:{username:'张三', id: 1 }}"> 声明式 </router-link>

<button @click="$router.push({name:'user', params:{username: '李四', id: 2 }})">编程式</button>
```

在组件中使用 params 获取参数：

-  获取username：`$route.params.username` 
- 获取id：  `$route.params.id`

注意：如果在导航中如果提供了 `path`和`params`的组合，`params` 会被忽略，导致传参失败，下面的写法无法得到参数，不要使用：

```html
<router-link :to="{ path:'/user', params:{username: '王五', id: 3 }}">不起作用的传参方式</router-link>
```

使用params传参存在的问题：

1. 刷新页面参数会丢失
2. 使用`params`只能配合`name`使用，不能配合`path`使用

解决刷新页面，参数丢失的问题：

- 动态路径传参
- 动态路径传参也可以配合`name`和`params`的组合使用
- 使用query传参
- 使用params传参之后在created中使用本地保存把参数保存起来

```html
<div id="app">
  <nav>
    <div class="w">
      <router-link to="/">首页</router-link>
      <router-link :to="{ name:'user', params:{username: user.name, id: user.id }}"> 声明式 </router-link>
      <button @click="$router.push({name:'user', params:{username: '李四', id: 2 }})">编程式</button>
      <router-link :to="{ path:'/user', params:{username: '王五', id: 3 }}">不起作用的传参方式</router-link>
    </div>
  </nav>

  <div class="w">
    <router-view></router-view>
  </div>
</div>
<script>
  Vue.config.productionTip = false;

  const home = {
    name: 'home',
    template: '<h1>首页</h1>',
  };

  const user = {
    name: 'user',
    template: `
        <div>
          <h1>好友信息</h1>
          <p>姓名：{{$route.params.username}}</p>
          <p>id：{{$route.params.id}}</p>
  			</div>`,
  };

  const router = new VueRouter({
    routes: [
      { path: '/', name: 'home', component: home },
      // { path: '/user', name: 'user', component: user },
      // params传参可以配合路径参数使用
      { path: '/user/:username/:id', name: 'user', component: user },
    ],
  });

  new Vue({
    el: '#app',
    data: {
      user: { id: 1, name: '张三', sex: '男', age: 18 },
    },
    router,
  });
</script>
```

## 第三种：query传参

既可以在声明式或者编程式导航中使用`name`和`query`的组合传参，也可以使用`path`和`query`的组合传参：

- 使用`name`和`query`的组合传参：

  ```html
  <router-link :to="{name:'user', query:{username: user.name, id: user.id}}">声明式1</router-link>
  <button @click="$router.push({name:'user', query:{username: '王五', id: 3}})">编程式1</button>
  ```

- 使用`path`和`query`的组合传参：

  ```html
  <router-link :to="{path:'/user', query:{username: '李四', id: 2}}">声明式2</router-link>
  <button @click="$router.push({path:'/user', query:{username: '钱六', id: 4}})">编程式2</button>
  ```

`query` 传参类似get请求的传参，传参之后，浏览器地址栏会展示传递的参数：`#/user?username=张三&id=1`

不管是使用使用`name`和`query`的组合，还是使用`path`和`query`的组合，在组件中都是使用 query 获取参数：

-  获取username：`$route.query.username` 
-  获取id：  `$route.query.id`获取参数。

```html
<div id="app">
  <nav>
    <div class="w">
      <router-link to="/">首页</router-link>
      <router-link :to="{name:'user', query:{username: user.name, id: user.id}}">声明式1</router-link>
      <router-link :to="{path:'/user', query:{username: '李四', id: 2}}">声明式2</router-link>
      <button @click="$router.push({name:'user', query:{username: '王五', id: 3}})">编程式1</button>
      <button @click="$router.push({path:'/user', query:{username: '钱六', id: 4}})">编程式2</button>
    </div>
  </nav>

  <div class="w">
    <router-view></router-view>
  </div>
</div>
<script>
  const home = {
    name: 'home',
    template: '<h1>首页</h1>',
  };
  const user = {
    name: 'user',
    template: `
    	<div>
        <h1>好友信息</h1>
        <p>姓名：{{$route.query.username}}</p>
        <p>id：{{$route.query.id}}</p>
  		</div>`,
  };

  const router = new VueRouter({
    routes: [
      { path: '/', name: 'home', component: home },
      { path: '/user', name: 'user', component: user },
    ],
  });

  const vm = new Vue({
    el: '#app',
    router,
    data: {
      user: { id: 1, name: '张三', sex: '男', age: 18 },
    },
  });
</script>
```

## `$route.params`和`$route.query`的区别

`params` 参数属于路径的一部分，匹配的时候路由的路径需要使用路径参数匹配

`query` 参数不属于路径的一部分，匹配的时候路由的路径不需要添加路径参数来匹配

无论是`params`还是`query`参数，最终匹配完成都会解析到当前这个路由对象 `$route` 的`params`和`query`属性中

显示路由组件的时候，会把当这个路由对象，传递给组件当中的`this.$route`，所以`this.$route`就可以获取到之前传递的参数

- params参数保存在`this.$route.params.xxx`   
- query参数保存在`this.$route.query.yyy`

共同点

- 都是为了在界面跳转的时候进行传参

不同点：

- 接收参数：query参数使用 `this.$route.query`获取, param参数使用`this.$route.params`获取。 **注意是`$route` 而不是 `$router` 哦** 
- 浏览器地址栏中的显示：使用 query 参数在浏览器地址栏中显示，而使用 params的时候参数不会在地址栏中显示。可以这样理解，query更像是ajax中的get方法，而params则更加类似于post方法
- 刷新浏览器的效果
  - 命名路由搭配params，刷新页面参数会丢失
  - 查询参数搭配query，刷新页面数据不会丢失
- query可以和path或者name搭配使用，params只能和name搭配使用

## 常见问题

### 问题一

描述：

- 编程式路由跳转到当前路由(参数不变)，多次执行会抛出`NavigationDuplicated`的警告错误         
- 声明式路由跳转内部已经处理

原因： 

- `vue-router3.1.0`之后，引入了promise的语法，如果没有通过参数指定成功或者失败回调函数就返回一个promise且内部会判断如果要跳转的路径和参数都没有会抛出一个失败的promise

报错：

- `Navigation cancelled from "/" to "/recommended" with a new navigation.`

解决：

- 在跳转时指定成功或失败的回调函数，或者catch处理错误         
- 修改Vue原型上的push和replace方法(推荐使用)

```js
//将原有的push方法地址，保存起来，后期还能拿到原来的
const originalPush = VueRouter.prototype.push;
//可以大胆的去修改原型的push，让原型的push指向另外一个函数
VueRouter.prototype.push = function (location, onResolve, onReject) {
    // location就是我们调用this.$router.push，传递过来的对象 { name:'regist', query:{username:this.zhangsan.name, age: this.zhangsan.age}
    if (onResolve==undefined && onReject==undefined) {
        //证明调用的时候只传递了个匹配路由对象，没有传递成功或者失败的回调
        return originalPush.call(this, location).catch(err => err);
    } else {
        //证明调用的时候传递了成功或者失败的回调,或者都有
        return originalPush.call(this, location, onResolve, onReject);
    }
}
```

```js
const originalPush = VueRouter.prototype.push;
VueRouter.prototype.push = function push(location) {
	return originalPush.call(this, location).catch(err => err);
};
const originalReplace = VueRouter.prototype.replace;
VueRouter.prototype.replace = function push(location) {
	return originalReplace.call(this, location).catch(err => err);
};
```

### 问题二

描述：

- 如果指定name和params组合，但params中数据是一个""，无法跳转，路径会出问题
- 前提是路由params参数要可传可不传

解决：

- ①不指定params
- ②指定params参数值为undefined

# 将 props 传递给路由组件

在组件中使用 `$route` 会使之与其对应路由形成高度耦合，这限制了组件的灵活性，因为它只能用于特定的 URL。虽然这不一定是件坏事，但我们可以通过 `props` 配置来解除这种行为：

> 解除耦合：通过拆分一个复杂的页面，在模块中定义各个功能，解除各部分之间的耦合，当某一个功能需要改变时，模块可以帮助我们快速的解决问题，模块暴露的接口不变时，模块内部的变化对于其他模块没有影响，通过模块解除耦合，提升系统的可维护性。
>
>
> 耦合性是编程中的一个判断代码模块构成质量的属性，不影响已有功能，但影响未来拓展，与之对应的是内聚性。
>
> 耦合性：也称块间联系。指软件系统结构中各模块间相互联系紧密程度的一种度量。模块之间联系越紧密，其耦合性就越强，模块的独立性则越差。模块间耦合高低取决于模块间接口的复杂性、调用的方式及传递的信息。
>
> 内聚性：又称块内联系。指模块的功能强度的度量，即一个模块内部各个元素彼此结合的紧密程度的度量。若一个模块内各元素（语名之间、程序段之间）联系的越紧密，则它的内聚性就越高。
>
> 因此，现代程序讲究高内聚低耦合，即将功能内聚在同一模块，模块与模块间尽可能独立，互相依赖低。没有绝对没有耦合的模块组，只有尽量降低互相之间的影响。使模块越独立越好。
>

使用 `props` 将组件和路由解耦。

**取代与 `$route` 的耦合**：

```js
const User = {
  template: '<div>User {{ $route.params.id }}</div>'
}
const routes = [{ path: '/user/:id', component: User }]
```

**通过 `props` 解耦**

```js
const User = {
  // 请确保添加一个与路由参数完全相同的 prop 名
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const routes = [  path: '/user/:id', component: User, props: true } 
```

这允许你在任何地方使用该组件，使得该组件更容易重用和测试。

## 布尔模式

布尔模式解耦：

- 在路由记录中把 `props` 设置为 `true` ，`$route.params` 将被设置为组件的 props。
- 需要在 `user` 组件中使用 `props` 定义`username` 和 `id` 属性来接收 `$route.params` 中的 `username` 和`id` 属性的值
- 布尔模式只能解耦  `$route.params` 的数据，不能解耦 `$route.query` 的数据

```html
<div id="app">
  <nav>
    <div class="w">
      <router-link to="/">首页</router-link>
      <router-link :to=" '/user/'+user.name + '/'+ user.id ">路径传参 </router-link>
      <router-link :to="{name:'user', params:{username: user.name, id: user.id}}">params传参</router-link>
      <router-link :to="{name:'user', query:{username: user.name, id: user.id}}">query传参</router-link>
    </div>
  </nav>

  <div class="w">
    <router-view></router-view>
  </div>
</div>
<script>
  const home = {
    name: 'home',
    template: '<h1>首页</h1>',
  }
  const user = {
    name: 'user',
    props: ['id', 'username'],
    template: `
      <div>
        <h1>好友信息</h1>
        <p>解耦的 姓名：{{username}}</p>
        <p>解耦的 id：{{id}}</p>
  		</div>`,
  }

  const router = new VueRouter({
    routes: [
      { path: '/', name: 'home', component: home },
      { path: '/user/:username?/:id?', name: 'user', component: user, props: true },
    ]
  });

  new Vue({
    el: '#app',
    router,
    data:{
      user: { id: 1, name: '张三', sex: '男', age: 18 }
    }
  });
</script>
```

命名视图的布尔模式：

- 对于有命名视图的路由，你必须为每个命名视图定义 `props` 配置：

```js
const routes = [
  {
    path: '/user/:id',
    components: { default: User, sidebar: Sidebar },
    props: { default: true, sidebar: false }
  }
]
```

## 对象模式

- 当 `props` 是一个对象时，它将原样设置为组件 props。当 props 是静态的时候很有用。
- 缺点：无法获取动态的 `params` 或者 `query` 参数。

```js
const router = new VueRouter({
  routes: [
    { path: '/', name: 'home', component: home },
    {
      path: '/user/:username?/:id?',
      name: 'user',
      component: user,
      props: {
        username: '李四',
        id: 2,
      },
    },
  ],
})
```

## 函数模式

你可以创建一个返回 props 的函数。这允许你将参数转换为其他类型，将静态值与基于路由的值相结合等等。

- 把props定义为函数，参数是路由对象，不管是 `params` 参数，还是 `query` 参数，都可以获取到，还可以添加静态参数。

```js
const router = new VueRouter({
  routes: [
    { path: '/', name: 'home', component: home },
    {
      path: '/user/:username?/:id?',
      name: 'user',
      component: user,
      props(route) {
        return {
          // 可以使用 params或者 query 参数
          username: route.params.username || route.query.username || '李四',
          id: route.params.id || route.query.id || '2' ,
          v1: 10,
          v2: 20,
        };
      },
    },
  ],
});
```

请尽可能保持 `props` 函数为无状态的，因为它只会在路由发生变化时起作用。如果你需要状态来定义 props，请使用包装组件，这样 vue 才可以对状态变化做出反应。

# 导航守卫

vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。主要有三种导航守卫：

- **全局**的导航守卫
- **单个路由独享**的导航守卫
- **组件内**的导航守卫

## 全局导航守卫

全局导航守卫有三个：

- 全局前置守卫：`router.beforeEach`
- 全局解析守卫：`router.beforeResolve`
- 全局后置钩子：`router.afterEach`

### 全局前置守卫

你可以使用 `router.beforeEach` 注册一个全局前置守卫：

```js
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
  
  next();// 必须执行next函数
})
```

当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 **等待中**。

每个守卫方法接收三个参数：

- **`to: Route`**: 即将要进入的目标 路由对象 `route`
- **`from: Route`**: 当前导航正要离开的路由
- **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。
  - **`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。
  - **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
  - **`next('/')` 或者 `next({ path: '/' })` 或者 `next({ name: 'xxx' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。
    - 你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项
    - 任何用在 `router-link` 的 `to` prop或 [`router.push`](https://v3.router.vuejs.org/zh/api/#router-push) 中的选项都可以在 `next` 函数中使用
  - **`next(error)`**: 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://v3.router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

**确保 `next` 函数在任何给定的导航守卫中都被严格调用一次。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错**。这里有一个在用户未能验证身份时重定向到 `/login` 的示例：

```js
// 错误用例
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' });
  // 如果用户未能验证身份，则 `next` 会被调用两次
  next();
})
```

```js
// 下面是正确的版本
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' });
  else next();
})
```

### 全局解析守卫

你可以用 `router.beforeResolve` 注册一个全局守卫。这和 `router.beforeEach` 类似，因为它在 **每次导航**时都会触发，但是确保在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被正确调用**。

```js
router.beforeResolve((to, from, next) => {
  // ...
  
  next();// 必须执行next函数
})
```

`router.beforeResolve` 是获取数据或执行任何其他操作（如果用户无法进入页面时你希望避免执行的操作）的理想位置。

### 全局后置钩子

你也可以注册全局后置钩子，然而和守卫不同的是，这些钩子不会接受 `next` 函数也不会改变导航本身：

```js
router.afterEach((to, from) => {
  // ...
  
  // 没有next函数
})
```

`router.afterEach` 它们对于分析、更改页面标题、声明页面等辅助功能以及许多其他事情都很有用。

## 路由独享的导航守卫

你可以在路由配置上直接定义 `beforeEnter` 守卫：

```js
const router = new VueRouter({
  routes: [
    {
      path: '/users/:id',
      component: UserDetails,
      // beforeEnter守卫与全局前置守卫的方法参数是一样的。
      beforeEnter: (to, from, next) => {
        // ...
        next();// 必须执行next函数
      }
    }
  ]
})
```

`beforeEnter` 守卫 **只在进入路由时触发**，不会在 `params`、`query` 或 `hash` 改变时触发。例如，从 `/users/2` 进入到 `/users/3` 或者从 `/users/2#info` 进入到 `/users/2#projects`。它们只有在 **从一个不同的** 路由导航时，才会被触发。

## 组件内的导航守卫

在路由组件内直接定义组件内的导航守卫，组件内的守卫也有三个：

- 进入：`beforeRouteEnter`
- 更新：`beforeRouteUpdate` 
- 离开：`beforeRouteLeave`

```js
const Foo = {
  template: `...`,
  beforeRouteEnter(to, from, next) {
    // 在渲染该组件的对应路由被 confirm 前调用
    // 不！能！获取组件实例 `this`
    // 因为当守卫执行前，组件实例还没被创建
    
    next();// 必须执行next函数
  },
  beforeRouteUpdate(to, from, next) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
    // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 可以访问组件实例 `this`
    
    next();// 必须执行next函数
  },
  beforeRouteLeave(to, from, next) {
    // 导航离开该组件的对应路由时调用
    // 可以访问组件实例 `this`
    
    next();// 必须执行next函数
  }
}
```

`beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 `next`来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数。

```js
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以**不支持**传递回调，因为没有必要了。

```js
beforeRouteUpdate (to, from, next) {
  // 可以直接使用 this
  this.name = to.params.name
  next()
}
```

这个离开守卫通常用来禁止用户在还未保存修改前突然离开。该导航可以通过 `next(false)` 来取消。

```js
beforeRouteLeave (to, from, next) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (answer) {
    next()
  } else {
    next(false)
  }
}
```

## 完整的导航解析流程

1. 导航被触发。
2. 在失活的组件里调用 `beforeRouteLeave` 守卫。
3. 调用全局的 `beforeEach` 守卫。
4. 在重用的组件里调用 `beforeRouteUpdate` 守卫(2.2+)。
5. 在路由配置里调用 `beforeEnter`。
6. 解析异步路由组件。
7. 在被激活的组件里调用 `beforeRouteEnter`。
8. 调用全局的 `beforeResolve` 守卫(2.5+)。
9. 导航被确认。
10. 调用全局的 `afterEach` 钩子。
11. 触发 DOM 更新。
12. 调用 `beforeRouteEnter` 守卫中传给 `next` 的回调函数，创建好的组件实例会作为回调函数的参数传入。

## 导航守卫的例子

### beforeEach的使用

登录验证：

vue-router的登陆权限判断主要是在全局钩子函数中进行的，我们在router.js文件中的定义路由里，将需要登陆权限的页面加上meta属性，值是对象的形式，然后在该对象中自定义一个属性，属性值就是一个Boolean值，这时候在`router/index.js`文件的全局前置守卫中进行判断，如果需要跳转的页面的自定义属性值为true，那么将进行判断其是否登录，如果没有登录，则告诉用户登录，如果有登录，那么进行页面跳转。

在 `router/index.js`页面：

```js
const router = new VueRouter({
  mode: 'history',
  routes: [
    { path: '/', name: 'home',	component: () => import('@/views/home') },
    { path: '/news', name: 'news', component: () => import('@/views/news.vue') ,
    { path: '/login', name: 'login', component: () => import('@/views/login.vue') },
    { path: '/regist', name: 'regist', component: () => import('@/views/regist.vue') },
    // 添加meta属性，定义requiresAuth字段为true，具有requiresAuth为true的页面需要登录才能访问
    { path: '/write', name: 'write', component: () => import('@/views/write.vue'), meta: { requiresAuth: true } },
    { path: '/create', name: 'create', component: () => import('@/views/create.vue'), meta: { requiresAuth: true } },
  ],
});

router.beforeEach((to, from, next) => {
  // 需要登录之后才能跳转的页面
  if (to.meta.requiresAuth) {
    // 判断token是否存在，如果token存在，直接跳转，如果token不存在再跳转到登录页面
    if (store.state.token) {
      // token存在，正常跳转 
      next();
    } else {
      // token不存在，跳转到登录页面：登录页面的时候，把当前要去的页面地址带上，登录成功之后，再回跳过来
      router.push({ name: 'login', query: { redirect: to.path } });
    }
  } else {
    // 不需要登录，正常跳转
    next();
  }
});
```

登录 `login.vue`：

```js
import { loginApi } from '@/api/user';

async login(){
  const res = await loginApi(this.formData);
  if (res.err_no == 0) {
    // 登录成功,保存token
    this.$store.commit('SET_TOKEN', res.token);
    // 获取用户信息
    this.$store.dispatch('getUser');
    if (this.$route.query.redirect) {
      // 是通过token过期或者不存的时候来到的登录，要回跳到之前的页面
      this.$router.replace({ path: this.$route.query.redirect });
    } else {
      // 直接点击登录回到首页
      this.$router.replace({ path: '/' });
    }
  } else {
    // 登录失败
    this.$message.error(res.data);
  }
}
```

状态管理 `store/index.js`：

```js
import Vue from 'vue';
import Vuex from 'vuex';
Vue.use(Vuex);

import { userApi } from '@/api/user';
import router from '@/router';

export default new Vuex.Store({
  state: {
    loading: true,
    token: localStorage.getItem('token') ? localStorage.getItem('token') : '',
    user: {},
  },
  mutations: {
    SET_LOADING (state, val) {
      state.loading = val;
    },
    SET_TOKEN (state, val) {
      // 设置token的同时，把token保存在本地
      state.token = val;
      state.token ? localStorage.setItem('token', state.token) : localStorage.removeItem('token');
    },
    SET_USER (state, val) {
      state.user = val;
    }
  },
  actions: {
    async getUser ({commit}) {
      let res = await userApi();
      if (res.err_no == 0) {
        commit('SET_USER', res.data);
      }
    },
    logout ({commit}) {
      commit('SET_TOKEN', '');
      commit('SET_USER', {});
      // 退出登录之后，回到首页
      router.replace({ path: '/' });
    }
  }
});
```

### beforeRouteEnter的使用

```html
<div id="app">
  <nav class="nav">
    <div class="w">
      <router-link :to="{name:'home'}">首页</router-link>
      <router-link :to="{name:'pins'}">沸点</router-link>
      <router-link :to="{name:'course'}">课程</router-link>
    </div>
  </nav>

  <div class="w">
    <router-view id="view"></router-view>
  </div>
</div>

<script>
  const home = {
    name: 'home',
    template: `
      <div class="home">
        <nav class="nav-item">
          <router-link v-for="item in items" :key="item.cate_id" :to="{name:item.name, params:{id:item.cate_id,title:item.title}}">{{item.title}}</router-link>  
  </nav>
        <router-view></router-view>
  	</div>`,
    data () {
      return {
        items: [
          { title: '全部', name: 'recommended', cate_id: '' },
          { title: '后端', name: 'backend', cate_id: '6809637769959178254' },
          { title: '前端', name: 'frontend', cate_id: '6809637767543259144' },
          { title: 'Android', name: 'android', cate_id: '6809635626879549454' },
        ]
      }
    },
  }
  const pins = { name: 'pins', template: `<div class="pins">沸点</div>` }
  const course = { name: 'course', template: `<div class="course">课程</div>` }

  const detail = {
    name: 'detail',
    props: ['title', 'id'],
    template: `<div class="detail">详情-{{title}}-{{id}} </div>`,
    // 渲染到同一个组件，created函数只会触发一次
    created () {
      console.log('detail created this.$route.params：', this.$route.params);
      this.getList(this.$route);
    },
    // 通过路由导航到组件时触发一次 beforeRouteEnter 守卫
    beforeRouteEnter (to, from, next) {
      next(vm => {
        // 在 beforeRouteEnter 中获取 this.$route 和 to 都是最新的路由对象
        console.log('detail beforeRouteEnter $route.params=', vm.$route.params);
        console.log('detail beforeRouteEnter to.params=', to.params);
        vm.getList(to);
      });
    },
    methods: {
      getList (route) {
        console.log('获取数据: ', route.params);
      }
    },
  }

  const router = new VueRouter({
    routes: [
      {
        path: '/',
        name: 'home',
        component: home,
        children: [
          { path: 'recommended', name: 'recommended', component: detail, props: true },
          { path: '/backend', name: 'backend', component: detail, props: true },
          { path: '/frontend', name: 'frontend', component: detail, props: true },
          { path: '/android', name: 'android', component: detail, props: true },
        ]
      },
      { path: '/pins', name: 'pins', component: pins },
      { path: '/course', name: 'course', component: course },
    ]
  });


  new Vue({
    el: '#app',
    router,
  });
</script>
```

### beforeRouteUpdate的使用

```html
<div id="app">
  <nav class="nav">
    <div class="w">
      <router-link :to="{name:'home'}">首页</router-link>
      <router-link :to="{name:'pins'}">沸点</router-link>
      <router-link :to="{name:'course'}">课程</router-link>
    </div>
  </nav>

  <div class="w">
    <router-view id="view"></router-view>
  </div>
</div>

<script>
  // 点击不同的选项，都是渲染到detail组件，在detail组件中created只会执行一次
  // 点击不用的选项，想要重新发请求，重新执行getList
  // 当路径变化的时候，渲染到同一个组件，组件内的声明周期函数只会在组件第一次渲染的时候执行，想要methods中的getList函数重复执行，就需要用到组件内的导航守卫
  const home = {
    name: 'home',
    template: `
      <div class="home">
        <nav class="nav-item">
          <router-link v-for="item in items" :key="item.cate_id" :to="{name:'detail', params:{id:item.cate_id, title:item.title}}">{{item.title}}</router-link>  
  </nav>
        <router-view></router-view>
  </div>`,
    data() {
      return {
        items: [
          { title: '全部', cate_id: '' },
          { title: '后端', cate_id: '6809637769959178254' },
          { title: '前端', cate_id: '6809637767543259144' },
          { title: 'Android', cate_id: '6809635626879549454' },
        ],
      };
    },
  };
  const pins = { name: 'pins', template: `<div class="pins">沸点</div>` };
  const course = { name: 'course', template: `<div class="course">课程</div>` };

  const detail = {
    name: 'detail',
    props: ['title', 'id'],
    template: `<div class="detail"> <hr>详情-{{title}}-{{id}} </div>`,
    created() {
      console.log('detail created', this.$route.params);
      this.getList(this.$route);
    },
    // 当导航的路由路径发生变化，导航到同一个组件时触发 beforeRouteUpdate 守卫，该守卫会触发多次
    beforeRouteUpdate(to, from, next) {
      // 在beforeRouteUpdate里面使用$route获取的路由对象是旧路由对象，新的路由对象使用to获取
      console.log('路由传值：$route title=', this.$route.params.title, this.$route.params.id);
      console.log('路由传值： to    title=', to.params.title, to.params.id);
      this.getList(to);
      next();
    },

    methods: {
      getList(route) {
        console.log('获取数据: ', route.params);
      },
    },
  };

  const router = new VueRouter({
    routes: [
      {
        path: '/',
        name: 'home',
        component: home,
        children: [{ path: '/detail/:id', name: 'detail', component: detail, props: true }],
      },
      { path: '/pins', name: 'pins', component: pins },
      { path: '/course', name: 'course', component: course },
    ],
  });

  new Vue({
    el: '#app',
    router,
  });
</script>
```

# 历史模式

## history 模式

`vue-router` 默认 hash 模式 —— 使用 URL 的 hash 来模拟一个完整的 URL，于是当 URL 改变时，页面不会重新加载。

如果不想要很丑的 hash，我们可以用路由的 **history 模式**，这种模式充分利用 `history.pushState` API 来完成 URL 跳转而无须重新加载页面。

```js
const router = new VueRouter({
  mode: 'history',
  routes: [...]
})
```

当你使用 history 模式时，URL 就像正常的 url，例如 `http://yoursite.com/user/id`，也好看！

不过这种模式要玩好，还需要后台配置支持。因为我们的应用是个单页客户端应用，如果后台没有正确的配置，当用户在浏览器直接访问 `http://oursite.com/user/id` 就会返回 404，这就不好看了。

所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 `index.html` 页面，这个页面就是你 app 依赖的页面。

## 后端配置例子

**注意**：下列示例假设你在根目录服务这个应用。如果想部署到一个子目录，你需要使用 Vue CLI 的 `publicPath` 选项 和相关的 router `base`  选项。你还需要把下列示例中的根目录调整成为子目录 (例如用 `RewriteBase /name-of-your-subfolder/` 替换掉 `RewriteBase /`)。

### 原生 Node.js

```js
const http = require('http')
const fs = require('fs')
const httpPort = 80

http.createServer((req, res) => {
  fs.readFile('index.html', 'utf-8', (err, content) => {
    if (err) {
      console.log('We cannot open "index.html" file.')
    }

    res.writeHead(200, {
      'Content-Type': 'text/html; charset=utf-8'
    })

    res.end(content)
  })
}).listen(httpPort, () => {
  console.log('Server listening on: http://localhost:%s', httpPort)
})
```

### 基于 Node.js 的 Express

对于 Node.js/Express，请考虑使用 [connect-history-api-fallback 中间件](https://github.com/bripkens/connect-history-api-fallback)。

```js
const express = require('express');
const history = require('connect-history-api-fallback');

const app = express();
app.use(history());


app.listen(3000);
```

# 路由元信息

有时，你可能希望将任意信息附加到路由上，如过渡名称、谁可以访问路由等。这些事情可以通过接收属性对象的`meta`属性来实现，并且它可以在路由地址和导航守卫上都被访问到。定义路由的时候你可以这样配置 `meta` 字段：

```js
const routes = [
  {
    path: '/posts',
    component: PostsLayout,
    children: [
      {
        path: 'new',
        component: PostsNew,
        // 只有经过身份验证的用户才能创建帖子
        meta: { requiresAuth: true }
      },
      {
        path: ':id',
        component: PostsDetail
        // 任何人都可以阅读文章
        meta: { requiresAuth: false }
      }
    ]
  }
]
```

那么如何访问这个 `meta` 字段呢？

首先，我们称呼 `routes` 配置中的每个路由对象为 **路由记录**。路由记录可以是嵌套的，因此，当一个路由匹配成功后，它可能匹配多个路由记录。

例如，根据上面的路由配置，`/posts/new` 这个 URL 将会匹配父路由记录 (`path: '/posts'`) 以及子路由记录 (`path: 'new'`)。

一个路由匹配到的所有路由记录会暴露为 `$route` 对象(还有在导航守卫中的路由对象)的`$route.matched` 数组。我们需要遍历这个数组来检查路由记录中的 `meta` 字段，但是 Vue Router 还为你提供了一个 `$route.meta` 方法，它是一个非递归合并**所有 `meta`** 字段的（从父字段到子字段）的方法。这意味着你可以简单地写

```js
router.beforeEach((to, from, next) => {
  // 而不是去检查每条路由记录
  // to.matched.some(record => record.meta.requiresAuth)
  if (to.meta.requiresAuth && !auth.isLoggedIn()) {
    // 此路由需要授权，请检查是否已登录
    // 如果没有，则重定向到登录页面
    next({
      path: '/login',
      // 保存我们所在的位置，以便以后再来
      query: { redirect: to.fullPath }
    });
  }else{
    // 此路由不需要授权，直接跳转页面
    next();
  }
})
```

不要忘记可以在路由元信息中添加任意数据，比如页面的标题和是否显示导航栏：

```js
let router = new VueRouter({
  routes: [
    { 
      path: "/", 
      component: home,
    	meta:{
        title:'首页',
        isNav: true,
      }
    },
    { 
      path: "/friend", 
      component: friend,
    	meta:{
        title:'好友',
        isNav: true,
      }
    },
    { 
      path: '/friend/:name', 
      component: chat,
    	meta:{
        title:'好友列表',
        isNav: false,
      }
    },
  ],
}
```

# 过渡动效

`<router-view>` 是基本的动态组件，所以我们可以用 `<transition>` 组件给它添加一些过渡效果：

```html
<transition>
  <router-view></router-view>
</transition>
```

Transition 的所有功能在这里同样适用。

## 单个路由的过渡

上面的用法会给所有路由设置一样的过渡效果，如果你想让每个路由组件有各自的过渡效果，可以在各路由组件内使用 `<transition>` 并设置不同的 name。

```js
const Foo = {
  template: `
    <transition name="slide">
      <div class="foo">...</div>
    </transition>
  `,
}

const Bar = {
  template: `
    <transition name="fade">
      <div class="bar">...</div>
    </transition>
  `,
}
```

## 基于路由的动态过渡

还可以基于当前路由与目标路由的变化关系，动态设置过渡效果：

```html
<!-- 使用动态的 transition name -->
<transition :name="transitionName">					
  <router-view></router-view>
</transition>
```

```js
// 接着在父组件内
// watch $route 决定使用哪种过渡
watch: {
  $route' (to, from) {
    const toDepth = to.path.split('/').length
    const fromDepth = from.path.split('/').length
    this.transitionName = toDepth < fromDepth ? 'slide-right' : 'slide-left'
  }
}
```

# 滚动行为

## scrollBehavior

使用前端路由，当切换到新路由时，想要页面滚到顶部，或者是保持原先的滚动位置，就像重新加载页面那样。 `vue-router` 能做到，而且更好，它让你可以自定义路由切换时页面如何滚动。

**注意: 这个功能只在支持 `history.pushState` 的浏览器中可用。**

当创建一个 Router 实例，你可以提供一个 `scrollBehavior` 方法：

```js
const router = new VueRouter({
  routes: [...],
  scrollBehavior (to, from, savedPosition) {
    // return 期望滚动到哪个的位置
  }
})
```

`scrollBehavior` 方法接收 `to` 和 `from` 路由对象。第三个参数 `savedPosition` 当且仅当 `popstate` 导航 (通过浏览器的 前进/后退 按钮触发) 时才可用。

这个方法返回滚动位置的对象信息，长这样：

- `{ x: number, y: number }`
- `{ selector: string, offset? : { x: number, y: number }}` (offset 只在 2.6.0+ 支持)

如果返回一个 falsy (译者注：falsy 不是 `false`，[参考这里](https://developer.mozilla.org/zh-CN/docs/Glossary/Falsy))的值，或者是一个空对象，那么不会发生滚动。

举例：

```js
scrollBehavior (to, from, savedPosition) {
  return { x: 0, y: 0 }
}
```

对于所有路由导航，简单地让页面滚动到顶部。

返回 `savedPosition`，在按下 后退/前进 按钮时，就会像浏览器的原生表现那样：

```js
scrollBehavior (to, from, savedPosition) {
  if (savedPosition) {
    return savedPosition
  } else {
    return { x: 0, y: 0 }
  }
}
```

如果你要模拟“滚动到锚点”的行为：

```js
scrollBehavior (to, from, savedPosition) {
  if (to.hash) {
    return {
      selector: to.hash
    }
  }
}
```

我们还可以利用路由元信息更细颗粒度地控制滚动。

## 异步滚动

你也可以返回一个 Promise 来得出预期的位置描述：

```js
scrollBehavior (to, from, savedPosition) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ x: 0, y: 0 })
    }, 500)
  })
}
```

将其挂载到从页面级别的过渡组件的事件上，令其滚动行为和页面过渡一起良好运行是可能的。但是考虑到用例的多样性和复杂性，我们仅提供这个原始的接口，以支持不同用户场景的具体实现。

## 平滑滚动

只需将 `behavior` 选项添加到 `scrollBehavior` 内部返回的对象中，就可以为[支持它的浏览器 ](https://developer.mozilla.org/en-US/docs/Web/API/ScrollToOptions/behavior)启用原生平滑滚动：

```js
scrollBehavior (to, from, savedPosition) {
  if (to.hash) {
    return {
      selector: to.hash,
      behavior: 'smooth',
    }
  }
}
```

# 路由组件懒加载

## 为什么需要懒加载？

Vue是单页面应用，如果没有应用懒加载，运用webpack打包构建应用后，JavaScript 文件变得非常大，造成进入首页时，需要加载的内容过多，时间过长，会出现长时间的白屏，即使做了loading也是不利于用户体验。而运用懒加载则可以把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了，可以有效的分担首页所承担的加载压力，减少首页加载用时。

什么是懒加载？

- 懒加载简单来说就是延迟加载或按需加载，即在需要的时候的时候进行加载。

如何实现？

- 常用的懒加载方式有两种：使用**vue异步组件** 和 **ES6中的import**

## vue异步组件

方法如下：`component：resolve=>(require(['需要加载的路由的地址'])，resolve)`

```js
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'Home',
      component: resolve => require(["@/views/home"], resolve),
    }
  ]
})
```

## ES6中的import

结合 Vue 的异步组件和 Webpack 的代码分割功能，轻松实现路由组件的懒加载。

首先，可以将异步组件定义为返回一个 Promise 的工厂函数 (该函数返回的 Promise 应该 resolve 组件本身)：

```js
const Foo = () => Promise.resolve({
  /* 组件定义对象 */
})
```

第二，在 Webpack 2 中，我们可以使用动态 import语法来定义代码分块点 (split point)：

```js
import('./Foo.vue') // 返回 Promise
```

结合这两者，这就是如何定义一个能够被 Webpack 自动代码分割的异步组件。

```js
// import Foo from '@/views/Foo.vue'
// 替换成
const Foo = () => import('@/views/Foo.vue')
```

在路由配置中什么都不需要改变，只需要像往常一样使用 `Foo`：

```js
const router = new VueRouter({
  routes: [
    { path: '/foo', component: Foo },
  ]
})
```

总结如下：

- `const Home = () =>import('需要加载的组件地址');`

- `component` (和 `components`) 配置接收一个返回 Promise 组件的函数，Vue Router **只会在第一次进入页面时才会获取这个函数**，然后使用缓存数据。

```js
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router);

// 先导入组件，再使用
const Home = () =>import("@/views/home");

export default new Router({
  routes: [
    {
      path: '/',
      name: 'Home',
      component: Home,
    },
    {
      path: '/user',
      name: 'User'
      // 或者直接写在路由记录中导入并使用
     	component: () =>import("@/views/user"),
    }
  ]
})
```

## 一般组件懒加载

导入一般组件也可以使用：`() =>import('需要加载的组件地址');`

```vue
<template>
  <div class="home">
  	<One-com></One-com>
  </div>
</template>

<script>
export default {
  components:{
    "One-com":() => import('./One-com')
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>
```

## 路由懒加载的优化

https://panjiachen.gitee.io/vue-element-admin-site/zh/guide/advanced/lazy-loading.html

# 动态路由

是在某些情况下，你可能想在应用程序已经运行的时候添加或删除路由。具有可扩展接口(如 后台管理系统)这样的应用程序可以使用它来扩展应用程序。

## 添加路由

动态路由主要通过两个函数实现。`router.addRoute()` 和 `router.removeRoute()`。它们**只**注册一个新的路由，也就是说，如果新增加的路由与当前位置相匹配，就需要你用 `router.push()` 或 `router.replace()` 来**手动导航**，才能显示该新路由。我们来看一个例子：

想象一下，只有一个路由的以下路由：

```js
const router = createRouter({
  history: createWebHistory(),
  routes: [{ path: '/:articleName', component: Article }],
})
```

进入任何页面，`/setting`，`/store`，或者 `/3-tricks-to-improve-your-routing-code` 最终都会呈现 `Article` 组件。如果我们在 `/setting` 上添加一个新的路由：

```js
router.addRoute({ path: '/setting', component: Setting })
```

页面仍然会显示 `Article` 组件，我们需要手动调用 `router.replace()` 来改变当前的位置，并覆盖我们原来的位置（而不是添加一个新的路由，最后在我们的历史中两次出现在同一个位置）：

```js
router.addRoute({ path: '/setting', component: Setting })
// 我们也可以使用 this.$route 或 route = useRoute() （在 setup 中）
router.replace(router.currentRoute.value.fullPath)
```

记住，如果你需要等待新的路由显示，可以使用 `await router.replace()`。

v3版本中的 `router.addRoutes`：

```js
router.addRoute([
  { path: '/friend', component: Friend },
  { path: '/setting', component: Setting }
])
```

## 在导航守卫中添加路由

如果你决定在导航守卫内部添加或删除路由，你不应该调用 `router.replace()`，而是通过返回新的位置来触发重定向：

```js
router.beforeEach((to, from, next) => {
  if (!hasNecessaryRoute(to)) {
    router.addRoute(generateRoute(to))
    // 触发重定向
    next(to.fullPath);
  }else{
    next();
  }
})
```

上面的例子有两个假设：第一，新添加的路由记录将与 `to` 位置相匹配，实际上导致与我们试图访问的位置不同。第二，`hasNecessaryRoute()` 在添加新的路由后返回 `false`，以避免无限重定向。

因为是在重定向中，所以我们是在替换将要跳转的导航，实际上行为就像之前的例子一样。而在实际场景中，添加路由的行为更有可能发生在导航守卫之外，例如，当一个视图组件挂载时，它会注册新的路由。

## 删除路由

有一下几个不同的方法来删除现有的路由：

方法一：通过添加一个名称冲突的路由。如果添加与现有途径名称相同的途径，会先删除路由，再添加路由：

```js
router.addRoute({ path: '/setting', name: 'setting', component: Setting })
// 这将会删除之前已经添加的路由，因为他们具有相同的名字且名字必须是唯一的
router.addRoute({ path: '/other', name: 'setting', component: Other })
```

方法二：通过调用 `router.addRoute()` 返回的回调：

- 当路由没有名称时，这很有用。

```js
const removeRoute = router.addRoute(routeRecord)
removeRoute() // 删除路由如果存在的话
```

方法三：通过使用 `router.removeRoute()`按名称删除路由：

- 需要注意的是，如果你想使用这个功能，但又想避免名字的冲突，可以在路由中使用 `Symbol` 作为名字。

```js
router.addRoute({ path: '/setting', name: 'setting', component: Setting })
// 删除路由
router.removeRoute('setting')
```

当路由被删除时，**所有的别名和子路由也会被同时删除**

## 添加嵌套路由

要将嵌套路由添加到现有的路由中，可以将路由的 *name* 作为第一个参数传递给 `router.addRoute()`，这将有效地添加路由，就像通过 `children` 添加的一样：

```js
router.addRoute({ name: 'admin', path: '/admin', component: Admin })
router.addRoute('admin', { path: 'settings', component: AdminSettings })
```

这等效于：

```js
router.addRoute({
  name: 'admin',
  path: '/admin',
  component: Admin,
  children: [{ path: 'settings', component: AdminSettings }],
})
```

## 查看现有路由

Vue Router 提供了两个功能来查看现有的路由：

- `router.hasRoute()`：检查路由是否存在。
- `router.getRoutes()`：获取一个包含所有路由记录的数组。

```js
router.hasRoute('home');


const routes = router.getRoutes();
```