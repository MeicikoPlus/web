# Vue Router的介绍

## 简介

前端路由是现代SPA应用必备的功能，每个现代前端框架都有对应的实现，例如vue-router、react-router。

Vue Router是Vue.js 官方的**路由管理器**。它和 Vue.js 的核心深度集成，使用 Vue.js + Vue Router 让构建单页面应用变得易如反掌。包含的功能有：

- 嵌套路由映射
- 动态路由选择
- 模块化、基于组件的路由配置
- 路由参数、查询、通配符
- 展示由 Vue.js 的过渡系统提供的过渡效果
- 细致的导航控制
- 自动激活 CSS 类的链接
- HTML5 history 模式或 hash 模式
- 可定制的滚动行为
- URL 的正确编码

## 官网

Vue3.x对应vue-router的v4.x版本

v4版本的官网：https://router.vuejs.org/zh/

## 安装

在浏览器中直接使用，需要使用script标签导入vue-router.js：

```html
<script src="https://unpkg.com/vue-router@4"></script>
```

在脚手架中使用：

```bash
npm install vue-router@4
```

# Vue Router的使用步骤

## 在浏览器中的使用

### 第一步：安装vue-router

```html
<script src="https://unpkg.com/vue-router@4"></script>
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

### 第三步：定义路由组件

这一步就是创建vue组件，等配置好路由之后，随后将组件映射到路由。

- 路由组件不需要使用`app.component()` 或者 `components` 注册，直接使用组件配置对象创建。
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
const { createRouter, createWebHashHistory } = VueRouter;

const router = createRouter({
  history: createWebHashHistory(),
  routes, // (缩写) 相当于 routes: routes
});
```

也可以把上面两步合并在一起：

```js
const router = createRouter({
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
const { createApp } = Vue;

const app = createApp({});

app.use(router); // 注入路由

app.mount('#app');
```

## 在脚手架中的使用

vue脚手架项目目录分析：

- public是静态资源文件夹。

- src是项目源代码文件夹，整个项目的所有代码都再src中。在src文件夹中，main.js是整个项目的入口文件。当运行项目时，vue-cli会使用webpack对main.js进行打包。

  - main.js 入口文件

  - App.vue   根组件下的app组件，后面所有的组件是添加在app组件内

  - components 目录 存放一般组件

  - views/pages 目录 存放路由组件

  - router目录 存放路由的配置文件index.js

  - store目录 存放vuex的配置文件

  - assets目录 存放静态资源

一般组件和路由组件都是vue的单文件组件，创建方式是一样的。

- 一般组件：可以导入到任意组件中使用，导入之后一定要在当前组件内的components属性中注册组件
- 路由组件：路由组件用在路由配置文件文件中

### 第一步：安装vue-router

```bash
npm install vue-router@4
```

### 第二步：配置路由规则

在项目src目录中创建router文件夹，在文件中创建index.js文件，在index.js文件中配置路由

```js
// router/index.js

import { createRouter, createWebHistory } from 'vue-router';

import home from '../views/home.vue';
import friend from '../views/friend.vue';
import setting from '../views/setting.vue';

// 创建路由管理器对象，并配置路由表
const router = createRouter({
	history: createWebHistory(),
  routes: [
    { path: '/', component: home},
    { path: '/friend', component: friend},
    { path: '/setting', component: setting},
  ],
});

// 导出路由
export default router;
```

### 第三步：注入路由

在main.js导入路由配置文件 index.js，并在根组件中注入路由

```js
// main.js

import { createApp } from 'vue';
import App from '../App.vue';
import router from './router';

//const app = createApp(App);

//app.use(router); // 注入路由

//app.mount('#app');

createApp(App).use(router).use(store).mount('#app');
```

### 第四步：添加路由导航及出口

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

### 第五步：创建路由组件

路由组件都放在src目录下的views或者pages文件中

# 导航守卫

正如其名，vue-router 提供的导航守卫主要用来通过跳转或取消的方式守卫导航。这里有很多方式植入路由导航中：全局的，单个路由独享的，或者组件级的。

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
const router = createRouter({ ... })

router.beforeEach((to, from) => {
  // ...
  // 返回 false 以取消导航
  return false
})
```

当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于**等待中**。

#### to和from参数

每个守卫方法接收两个参数：

- **`to`**: 即将要进入的目标
- **`from`**: 当前导航正要离开的路由

可以返回的值如下:

- `false`: 取消当前的导航。如果浏览器的 URL 改变了(可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
- 一个[路由地址](https://router.vuejs.org/zh/api/#routelocationraw): 通过一个路由地址跳转到一个不同的地址，就像你调用 [`router.push()`](https://router.vuejs.org/zh/api/#push) 一样，你可以设置诸如 `replace: true` 或 `name: 'home'` 之类的配置。当前的导航被中断，然后进行一个新的导航，就和 `from` 一样。

```js
 router.beforeEach(async (to, from) => {
   if (
     // 检查用户是否已登录
     !isAuthenticated &&
     // ❗️ 避免无限重定向
     to.name !== 'Login'
   ) {
     // 将用户重定向到登录页面
     return { name: 'Login' }
   }
 })
```

如果遇到了意料之外的情况，可能会抛出一个 `Error`。这会取消导航并且调用 [`router.onError()`](https://router.vuejs.org/zh/api/#onerror) 注册过的回调。

如果什么都没有，`undefined` 或返回 `true`，**则导航是有效的**，并调用下一个导航守卫

以上所有都同 **`async` 函数** 和 Promise 工作方式一样：

```js
router.beforeEach(async (to, from) => {
  // canUserAccess() 返回 `true` 或 `false`
  const canAccess = await canUserAccess(to)
  if (!canAccess) return '/login'
})
```

#### 可选的第三个参数 `next`

在之前的 Vue Router 版本中，也是可以使用 *第三个参数* `next` 的。这是一个常见的错误来源，可以通过 [RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0037-router-return-guards.md#motivation) 来消除错误。然而，它仍然是被支持的，这意味着你可以向任何导航守卫传递第三个参数。在这种情况下，**确保 `next`** 在任何给定的导航守卫中都被**严格调用一次**。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错。这里有一个在用户未能验证身份时重定向到`/login`的**错误用例**：

```js
// BAD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  // 如果用户未能验证身份，则 `next` 会被调用两次
  next()
})
```

下面是正确的版本:

```js
// GOOD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
```

### 全局解析守卫

你可以用 `router.beforeResolve` 注册一个全局守卫。这和 `router.beforeEach` 类似，因为它在 **每次导航**时都会触发，但是确保在导航被确认之前，**同时在所有组件内守卫和异步路由组件被解析之后，解析守卫就被正确调用**。这里有一个例子，确保用户可以访问[自定义 meta](https://router.vuejs.org/zh/guide/advanced/meta.html) 属性 `requiresCamera` 的路由：

```js
router.beforeResolve(async to => {
  if (to.meta.requiresCamera) {
    try {
      await askForCameraPermission()
    } catch (error) {
      if (error instanceof NotAllowedError) {
        // ... 处理错误，然后取消导航
        return false
      } else {
        // 意料之外的错误，取消导航并把错误传给全局处理器
        throw error
      }
    }
  }
})
```

`router.beforeResolve` 是获取数据或执行任何其他操作（如果用户无法进入页面时你希望避免执行的操作）的理想位置。

### 全局后置钩子

你也可以注册全局后置钩子，然而和守卫不同的是，这些钩子不会接受 `next` 函数也不会改变导航本身：

```js
router.afterEach((to, from) => {
  sendToAnalytics(to.fullPath)
})
```

它们对于分析、更改页面标题、声明页面等辅助功能以及许多其他事情都很有用。

它们也反映了 [navigation failures](https://router.vuejs.org/zh/guide/advanced/navigation-failures.html) 作为第三个参数：

```js
router.afterEach((to, from, failure) => {
  if (!failure) sendToAnalytics(to.fullPath)
})
```

了解更多关于 navigation failures 的信息在[它的指南](https://router.vuejs.org/zh/guide/advanced/navigation-failures.html)中。

## 路由独享的守卫

你可以直接在路由配置上定义 `beforeEnter` 守卫：

```js
const routes = [
  {
    path: '/users/:id',
    component: UserDetails,
    beforeEnter: (to, from) => {
      // reject the navigation
      return false
    },
  },
]
```

`beforeEnter` 守卫 **只在进入路由时触发**，不会在 `params`、`query` 或 `hash` 改变时触发。例如，从 `/users/2` 进入到 `/users/3` 或者从 `/users/2#info` 进入到 `/users/2#projects`。它们只有在 **从一个不同的** 路由导航时，才会被触发。

你也可以将一个函数数组传递给 `beforeEnter`，这在为不同的路由重用守卫时很有用：

```js
function removeQueryParams(to) {
  if (Object.keys(to.query).length)
    return { path: to.path, query: {}, hash: to.hash }
}

function removeHash(to) {
  if (to.hash) return { path: to.path, query: to.query, hash: '' }
}

const routes = [
  {
    path: '/users/:id',
    component: UserDetails,
    beforeEnter: [removeQueryParams, removeHash],
  },
  {
    path: '/about',
    component: UserDetails,
    beforeEnter: [removeQueryParams],
  },
]
```

请注意，你也可以通过使用[路径 meta 字段](https://router.vuejs.org/zh/guide/advanced/meta.html)和[全局导航守卫](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#global-before-guards)来实现类似的行为。

## 组件内的守卫

最后，你可以在路由组件内直接定义路由导航守卫(传递给路由配置的)

你可以为路由组件添加以下配置：

- `beforeRouteEnter`
- `beforeRouteUpdate`
- `beforeRouteLeave`

```js
const UserDetails = {
  template: `...`,
  beforeRouteEnter(to, from) {
    // 在渲染该组件的对应路由被验证前调用
    // 不能获取组件实例 `this` ！
    // 因为当守卫执行时，组件实例还没被创建！
  },
  beforeRouteUpdate(to, from) {
    // 在当前路由改变，但是该组件被复用时调用
    // 举例来说，对于一个带有动态参数的路径 `/users/:id`，在 `/users/1` 和 `/users/2` 之间跳转的时候，
    // 由于会渲染同样的 `UserDetails` 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
    // 因为在这种情况发生的时候，组件已经挂载好了，导航守卫可以访问组件实例 `this`
  },
  beforeRouteLeave(to, from) {
    // 在导航离开渲染该组件的对应路由时调用
    // 与 `beforeRouteUpdate` 一样，它可以访问组件实例 `this`
  },
}
```

`beforeRouteEnter` 守卫 **不能** 访问 `this`，因为守卫在导航确认前被调用，因此即将登场的新组件还没被创建。

不过，你可以通过传一个回调给 `next` 来访问组件实例。在导航被确认的时候执行回调，并且把组件实例作为回调方法的参数：

```js
beforeRouteEnter (to, from, next) {
  next(vm => {
    // 通过 `vm` 访问组件实例
  })
}
```

注意 `beforeRouteEnter` 是支持给 `next` 传递回调的唯一守卫。对于 `beforeRouteUpdate` 和 `beforeRouteLeave` 来说，`this` 已经可用了，所以*不支持* 传递回调，因为没有必要了：

```js
beforeRouteUpdate (to, from) {
  // just use `this`
  this.name = to.params.name
}
```

这个 **离开守卫** 通常用来预防用户在还未保存修改前突然离开。该导航可以通过返回 `false` 来取消。

```js
beforeRouteLeave (to, from) {
  const answer = window.confirm('Do you really want to leave? you have unsaved changes!')
  if (!answer) return false
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

# 组合式 API

引入 `setup` 和 Vue 的组合式 API，开辟了新的可能性，但要想充分发挥 Vue Router 的潜力，我们需要使用一些新的函数来代替访问 `this` 和组件内导航守卫。

## 在 `setup` 中访问路由管理器和当前路由

因为我们在 `setup` 里面没有访问 `this`，所以我们不能再直接访问 `this.$router` 或 `this.$route`。作为替代，我们使用 `useRouter` 函数：

```html
<script setup>
  import { useRouter, useRoute } from 'vue-router';

  // 必须先声明调用
  const router = useRouter();
  const route = useRoute();
  
    // 路由信息
  console.log(route.query);

  // 路由跳转
  router.push('/newPage');
</script>
```

`route` 对象是一个响应式对象，所以它的任何属性都可以被监听，但你应该**避免监听整个 `route`** 对象。在大多数情况下，你应该直接监听你期望改变的参数。

```html
export default {
  setup() {

  },
}

<script setup>
  import { useRoute } from 'vue-router';
  import { ref, watch } from 'vue';
  
  const route = useRoute();
  const userData = ref();

  // 当参数更改时获取用户信息
  watch(
    () => route.params.id,
    async newId => {
      userData.value = await fetchUser(newId);
    }
  )
</script>
```

请注意，在模板中我们仍然可以访问 `$router` 和 `$route`，所以不需要在 `setup` 中返回 `router` 或 `route`。

## 导航守卫

虽然你仍然可以通过 `setup` 函数来使用组件内的导航守卫，但 Vue Router 将更新和离开守卫作为 组合式 API 函数公开：

```vue
<script setup>
  import { onBeforeRouteLeave, onBeforeRouteUpdate } from 'vue-router'
  import { ref } from 'vue'

  // 与 beforeRouteLeave 相同，无法访问 `this`
  onBeforeRouteLeave((to, from) => {
    const answer = window.confirm(
      'Do you really want to leave? you have unsaved changes!'
    )
    // 取消导航并停留在同一页面上
    if (!answer) return false
  })

  const userData = ref()

  // 与 beforeRouteUpdate 相同，无法访问 `this`
  onBeforeRouteUpdate(async (to, from) => {
    //仅当 id 更改时才获取用户，例如仅 query 或 hash 值已更改
    if (to.params.id !== from.params.id) {
      userData.value = await fetchUser(to.params.id)
    }
  })

</script>
```

组合式 API 守卫也可以用在任何由 `<router-view>` 渲染的组件中，它们不必像组件内守卫那样直接用在路由组件上。

## `useLink`

Vue Router 将 RouterLink 的内部行为作为一个组合式 API 函数公开。它提供了与 [`v-slot` API](https://router.vuejs.org/zh/api/#router-link-s-v-slot) 相同的访问属性：

```js
import { RouterLink, useLink } from 'vue-router'
import { computed } from 'vue'

export default {
  name: 'AppLink',

  props: {
    // 如果使用 TypeScript，请添加 @ts-ignore
    ...RouterLink.props,
    inactiveClass: String,
  },

  setup(props) {
    const { route, href, isActive, isExactActive, navigate } = useLink(props)

    const isExternalLink = computed(
      () => typeof props.to === 'string' && props.to.startsWith('http')
    )

    return { isExternalLink, href, navigate, isActive }
  },
}
```

# 从 Vue-router3 迁移

在 Vue Router API 从 v3（Vue2）到 v4（Vue3）的重写过程中，大部分的 Vue Router API 都没有变化，但是在迁移你的程序时，你可能会遇到一些破坏性的变化。本指南将帮助你了解为什么会发生这些变化，以及如何调整你的程序，使其与 Vue Router4 兼容。

## 破坏性变化

变化的顺序是按其用途排列的。因此，建议按照这个清单的顺序进行。

### new Router 变成 createRouter

Vue Router 不再是一个类，而是一组函数。现在你不用再写 `new Router()`，而是要调用 `createRouter`:

```js
// 以前是
// import Router from 'vue-router'
import { createRouter } from 'vue-router'

const router = createRouter({
  // ...
})
```

### 新的 `history` 配置取代 `mode`

`mode: 'history'` 配置已经被一个更灵活的 `history` 配置所取代。根据你使用的模式，你必须用适当的函数替换它：

- `"history"`: `createWebHistory()`
- `"hash"`: `createWebHashHistory()`
- `"abstract"`: `createMemoryHistory()`

下面是一个完整的代码段：

```js
import { createRouter, createWebHistory } from 'vue-router'
// 还有 createWebHashHistory 和 createMemoryHistory

createRouter({
  history: createWebHistory(),
  routes: [],
})
```

### 移动了 `base` 配置

现在，`base` 配置被作为 `createWebHistory` (其他 history 也一样)的第一个参数传递：

```js
import { createRouter, createWebHistory } from 'vue-router'
createRouter({
  history: createWebHistory('/base-directory/'),
  routes: [],
})
```

### 删除了 `fallback` 属性

创建路由时不再支持 `fallback` 属性：

```
-new VueRouter({
+createRouter({
-  fallback: false,
// other options...
})
```

**原因**: Vue支持的所有浏览器都支持 [HTML5 History API](https://developer.mozilla.org/zh-CN/docs/Web/API/History_API)，因此我们不再需要使用 `location.hash`，而可以直接使用 `history.pushState()`。

### 删除了 `*`（星标或通配符）路由

现在必须使用自定义的 regex 参数来定义所有路由(`*`、`/*`)：

```js
const routes = [
  // pathMatch 是参数的名称，例如，跳转到 /not/found 会得到
  // { params: { pathMatch: ['not', 'found'] } }
  // 这要归功于最后一个 *，意思是重复的参数，如果你
  // 打算直接使用未匹配的路径名称导航到该路径，这是必要的
  { path: '/:pathMatch(.*)*', name: 'not-found', component: NotFound },
  // 如果你省略了最后的 `*`，在解析或跳转时，参数中的 `/` 字符将被编码
  { path: '/:pathMatch(.*)', name: 'bad-not-found', component: NotFound },
]
// 如果使用命名路由，不好的例子：
router.resolve({
  name: 'bad-not-found',
  params: { pathMatch: 'not/found' },
}).href // '/not%2Ffound'
// 好的例子:
router.resolve({
  name: 'not-found',
  params: { pathMatch: ['not', 'found'] },
}).href // '/not/found'
```

> TIP
>
> 如果你不打算使用其名称直接跳转到未找到的路由，则无需为重复的参数添加 `*`。如果你调用 `router.push('/not/found/url')`，它将提供正确的 `pathMatch` 参数。

**原因**：Vue Router 不再使用 `path-to-regexp`，而是实现了自己的解析系统，允许路由排序并实现动态路由。由于我们通常在每个项目中只添加一个通配符路由，所以支持 `*` 的特殊语法并没有太大的好处。参数的编码是跨路由的，无一例外，让事情更容易预测。

### 将 `onReady` 改为 `isReady`

现有的 `router.onReady()` 函数已被 `router.isReady()` 取代，该函数不接受任何参数并返回一个 Promise：

```js
// 将
router.onReady(onSuccess, onError)
// 替换成
router.isReady().then(onSuccess).catch(onError)
// 或者使用 await:
try {
  await router.isReady()
  // 成功
} catch (err) {
  // 报错
}
```

### `scrollBehavior` 的变化

`scrollBehavior` 中返回的对象与 [`ScrollToOptions`](https://developer.mozilla.org/en-US/docs/Web/API/ScrollToOptions) 类似：`x` 改名为 `left`，`y` 改名为 `top`。详见 [RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0035-router-scroll-position.md)。

**原因**：使该对象类似于 `ScrollToOptions`，以使其感觉更像原生 JS API，并有可能启用将来的新配置。

### `<router-view>`、`<keep-alive>` 和 `<transition>`

`transition` 和 `keep-alive` 现在必须通过 `v-slot` API 在 `RouterView` **内部**使用：

```vue 
<router-view v-slot="{ Component }">
  <transition>
    <keep-alive>
      <component :is="Component" />
    </keep-alive>
  </transition>
</router-view>
```

**原因**: 这是一个必要的变化。详见 [related RFC](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0034-router-view-keep-alive-transitions.md).

### 删除 `<router-link>` 中的 `append` 属性

`<router-link>` 中的 `append` 属性已被删除。你可以手动将值设置到现有的 `path` 中：

```vue 
将
<router-link to="child-route" append>to relative child</router-link>
替换成
<router-link :to="append($route.path, 'child-route')">
  to relative child
</router-link>
```

你必须在 *App* 实例上定义一个全局的 `append` 函数：

```js
app.config.globalProperties.append = (path, pathToAppend) =>
  path + (path.endsWith('/') ? '' : '/') + pathToAppend
```

**原因**：`append` 使用频率不高，用户可以很容易地实现。

### 删除 `<router-link>` 中的 `event` 和 `tag` 属性

`<router-link>` 中的 `event` 和 `tag` 属性都已被删除。你可以使用 [`v-slot` API](https://router.vuejs.org/zh/api/#router-link-s-v-slot) 来完全定制 `<router-link>`：

```vue 
将
<router-link to="/about" tag="span" event="dblclick">About Us</router-link>
替换成
<router-link to="/about" custom v-slot="{ navigate }">
  <span @click="navigate" @keypress.enter="navigate" role="link">About Us</span>
</router-link>
```

**原因**：这些属性经常一起使用，以使用与 `<a>` 标签不同的东西，但这些属性是在 `v-slot` API 之前引入的，并且没有足够的使用，因此没有足够的理由为每个人增加 bundle 包的大小。

### 删除 `<router-link>` 中的 `exact` 属性

`exact` 属性已被删除，因为不再存在要修复的警告，所以你应该能够安全地删除它。但，有两件事你应该注意：

- 路由现在是基于它们所代表的路由记录来激活的，而不是路由地址对象及其 `path`、`query` 和 `hash` 属性来激活的
- 只匹配 `path` 部分，`query` 和 `hash` 不再考虑

如果你想自定义这种行为，例如考虑到 `hash` 部分，你应该使用 [`v-slot` API](https://next.router.vuejs.org/api/#router-link-s-v-slot) 来扩展`<router-link>`。

**原因**: 详见 [RFC about active matching](https://github.com/vuejs/rfcs/blob/master/active-rfcs/0028-router-active-link.md#summary)。

### 忽略 mixins 中的导航守卫

目前不支持 mixins 中的导航守卫，你可以在 [vue-router#454](https://github.com/vuejs/router/issues/454) 追踪它的支持情况。

### 删除 `router.match` 改为 `router.resolve`

`router.match` 和 `router.resolve` 已合并到 `router.resolve` 中，签名略有不同。[详见 API](https://router.vuejs.org/zh/api/#resolve)。

**原因**：将用于同一目的的多种方法统一起来。

### 删除 `router.getMatchedComponents()`

`router.getMatchedComponents` 方法现在被删除，因为匹配的组件可以从 `router.currentRoute.value.matched` 中获取：

```js
router.currentRoute.value.matched.flatMap(record =>
  Object.values(record.components)
)
```

**原因**：这个方法只在 SSR 中使用，并且是用户一行就能完成的操作。

### **所有**的导航现在都是异步的

所有的导航，包括第一个导航，现在都是异步的，这意味着，如果你使用一个 `transition`，你可能需要等待路由 *ready* 好后再挂载程序：

```js
app.use(router)
// 注意：在服务器端，你需要手动跳转到初始地址。
router.isReady().then(() => app.mount('#app'))
```

否则会有一个初始过渡，就像你提供了 `appear` 属性到 `transition` 一样，因为路由会显示它的初始地址（什么都没有），然后显示第一个地址。

请注意，**如果在初始导航时有导航守卫**，你可能不想阻止程序渲染，直到它们被解析，除非你正在进行服务器端渲染。否则，在这种情况下，不等待路由准备好挂载应用会产生与 Vue2 中相同的结果。

### 删除 `router.app`

`router.app` 用于表示注入路由的最后一个根组件（Vue 实例）。Vue Router 现在可以被多个 Vue 程序同时安全使用。你仍然可以在使用路由时添加它：

```js
app.use(router)
router.app = app
```

你也可以扩展 `Router` 接口的 TypeScript 定义来添加 `app` 属性。

**原因**：Vue3 写的程序不能在 Vue2 中使用，现在我们使用同一个 Router 实例来支持多个程序，因此拥有 `app` 属性可能会产生误导，因为它是程序而不是根实例。

### 将内容传递给路由组件的 `<slot>`

之前你可以直接传递一个模板，通过嵌套在 `<router-view>` 组件下，由路由组件的 `<slot>` 来渲染：

```vue
<router-view>
  <p>In Vue Router 3, I render inside the route component</p>
</router-view>
```

由于 `<router-view>` 引入了 `v-slot` API，你必须使用 `v-slot` API 将其传递给 `<component>`：

```vue
<router-view v-slot="{ Component }">
  <component :is="Component">
    <p>In Vue Router 3, I render inside the route component</p>
  </component>
</router-view>
```

### 将 `parent` 从路由地址中删除

`parent` 属性已从标准化路由地址（`this.$route` 和 `router.resolve` 返回的对象）中删除。你仍然可以通过 `matched` 数组访问它：

```
const parent = this.$route.matched[this.$route.matched.length - 2]
```

**原因**：同时存在 `parent` 和 `children` 会造成不必要的循环引用，而属性可以通过 `matched` 来检索。

### 删除 `pathToRegexpOptions`

路由的 `pathToRegexpOptions` 和 `caseSensitive` 属性已被 `createRouter()` 的 `sensitive` 和 `strict` 配置取代。现在，当使用 `createRouter()` 创建路由时，它们也可以直接传递。`path-to-regexp` 的任何其他特定配置已被删除，因为 `path-to-regexp` 已不再用于解析路径。

### 删除未命名的参数

由于取消了 `path-to-regexp`，所以不再支持未命名的参数：

- `/foo(/foo)?/suffix` 变成 `/foo/:_(foo)?/suffix`
- `/foo(foo)?` 变成 `/foo:_(foo)?`
- `/foo/(.*)` 变成 `/foo/:_(.*)`

> TIP
>
> 请注意，你可以使用任何名称代替 `_` 作为参数。重点是要提供一个名字。

### `history.state` 的用法[

Vue Router 将信息保存在 `history.state` 上。如果你有任何手动调用 `history.pushState()` 的代码，你应该避免它，或者用的 `router.push()` 和 `history.replaceState()` 进行重构：

```js
// 将
history.pushState(myState, '', url)
// 替换成
await router.push(url)
history.replaceState({ ...history.state, ...myState }, '')
```

同样，如果你在调用 `history.replaceState()` 时没有保留当前状态，你需要传递当前 `history.state`：

```js
// 将
history.replaceState({}, '', url)
// 替换成
history.replaceState(history.state, '', url)
```

**原因**：我们使用历史状态来保存导航信息，如滚动位置，以前的地址等。

### `options` 中需要配置 `routes`

`options` 中的 `routes` 属性现在是必需的。

```js
createRouter({ routes: [] })
```

**原因**：路由的设计是为了创建路由，尽管你可以在以后添加它们。在大多数情况下，你至少需要一条路由，一般每个应用都会编写一次。

### 不存在的命名路由

跳转或解析不存在的命名路由会产生错误：

```js
// 哎呀，我们的名字打错了
router.push({ name: 'homee' }) // 报错
router.resolve({ name: 'homee' }) // 报错
```

**原因**：以前，路由会导航到 `/`，但不显示任何内容（而不是主页）。抛出一个错误更有意义，因为我们不能生成一个有效的 URL 进行导航

### 命名路由缺少必要的 `params`

在没有传递所需参数的情况下跳转或解析命名路由，会产生错误：

```js
// 给与以下路由:
const routes = [{ path: '/users/:id', name: 'user', component: UserDetails }]

// 缺少 `id` 参数会失败
router.push({ name: 'user' })
router.resolve({ name: 'user' })
```

**原因**：同上。

### 带有空 `path` 的命名子路由不再添加斜线

给予任何空 `path` 的嵌套命名路由：

```js
const routes = [
  {
    path: '/dashboard',
    name: 'dashboard-parent',
    component: DashboardParent,
    children: [
      { path: '', name: 'dashboard', component: DashboardDefault },
      {
        path: 'settings',
        name: 'dashboard-settings',
        component: DashboardSettings,
      },
    ],
  },
]
```

现在，导航或解析到命名的路由 `dashboard` 时，会产生一个**不带斜线的** URL：

```js
router.resolve({ name: 'dashboard' }).href // '/dashboard'
```

这对子级 `redirect` 有重要的副作用，如下所示：

```js
const routes = [
  {
    path: '/parent',
    component: Parent,
    children: [
      // 现在将重定向到 `/home` 而不是 `/parent/home`
      { path: '', redirect: 'home' },
      { path: 'home', component: Home },
    ],
  },
]
```

请注意，如果 `path` 是 `/parent/`，这也可以，因为 `home` 到 `/parent/` 的相对地址确实是 `/parent/home`，但 `home` 到 `/parent` 的相对地址是 `/home`。

**原因**：这是为了使尾部的斜线行为保持一致：默认情况下，所有路由都允许使用尾部的斜线。可以通过使用 `strict` 配置和手动添加(或不添加)斜线来禁用它。

### `$route` 属性编码

无论在哪里启动导航，`params`、`query`和 `hash` 中的解码值现在都是一致的（旧的浏览器仍然会产生未编码的 `path` 和 `fullPath`）。初始导航应产生与应用内部导航相同的结果。

给定任何[规范化的路由地址](https://router.vuejs.org/zh/api/#routelocationnormalized):

- `path`, `fullPath`中的值不再被解码了。例如，直接在地址栏上写 "https://example.com/hello world"，将得到编码后的版本："https://example.com/hello world"，而 "path "和 "fullPath "都是"/hello%20world"。
- `hash` 现在被解码了，这样就可以复制过来。`router.push({ hash: $route.hash })` 可以直接用于 [scrollBehavior](https://router.vuejs.org/zh/api/#scrollbehavior) 的 `el` 配置中。
- 当使用 `push`、`resolve` 和 `replace` 并在对象中提供 `string` 地址或 `path` 属性时，**必须进行编码**(像以前的版本一样)。另一方面，`params`、`query` 和 `hash` 必须以未编码的版本提供。
- 斜线字符(`/`)现在已在 `params` 内正确解码，同时仍在 URL 上产生一个编码版本：`%2F`。

**原因**：这样，在调用 `router.push()` 和 `router.resolve()` 时，可以很容易地复制一个地址的现有属性，并使产生的路由地址在各浏览器之间保持一致。`router.push()` 现在是幂等的，这意味着调用 `router.push(route.fullPath)`、`router.push({ hash: route.hash })`、`router.push({ query: route.query })` 和`router.push({ params: route.params })` 不会产生额外的编码。

### TypeScript 变化

为了使类型更一致，更有表现力，有些类型被重新命名：

| `vue-router@3` | `vue-router@4`          |
| -------------- | ----------------------- |
| RouteConfig    | RouteRecordRaw          |
| Location       | RouteLocation           |
| Route          | RouteLocationNormalized |

## 新功能

Vue Router4 中需要关注的一些新功能包括：

- [动态路由](https://router.vuejs.org/zh/guide/advanced/dynamic-routing.html)
- [组合式 API](https://router.vuejs.org/zh/guide/advanced/composition-api.html)



























