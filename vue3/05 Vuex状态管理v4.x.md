# Vuex简介

## 介绍

Vuex 是一个专为 Vue.js 应用程序开发的**状态（数据）管理模式**。**在Vue中实现集中式状态管理的一个Vue插件**，对vue应用中多个组件的共享状态进行集中式的管理（读/写），并以相应的规则保证状态以一种可预测的方式发生变化。也是一种组件间通信的方式，且适用于任意组件间通信。

在vue开发中，每个组件都有自己的独立的数据，整个项目中的所有组件可以通过bus总线进行传值，但是如果出现组件之间需要共用同一组数据时，数据管理就会非常麻烦。vuex是vue的状态(数据)管理工具，它采取了一种集中管理数据的思想，将整个项目中所有的公共数据放在一个统一的仓库中，然后任何组件都可以从这个仓库中读取数据，也可以通过仓库提供的方法修改数据。

## 什么是“状态管理模式”？

让我们从一个简单的 Vue 计数应用开始：

```js
const Counter = {
  // 状态
  data () {
    return {
      count: 0
    }
  },
  // 视图
  template: `
    <div>{{ count }}</div>
  `,
  // 操作
  methods: {
    increment () {
      this.count++
    }
  }
}

createApp(Counter).mount('#app')
```

这个状态自管理应用包含以下几个部分：

- **state**，驱动应用的数据源；
- **view**，以声明方式将 **state** 映射到视图；
- **actions**，响应在 **view** 上的用户输入导致的状态变化。

以下是一个表示“单向数据流”理念的简单示意：

<img src="https://vuex.vuejs.org/flow.png" style="zoom: 33%;" />

但是，当我们的应用遇到**多个组件共享状态**时，单向数据流的简洁性很容易被破坏：

- 多个视图依赖于同一状态。
- 来自不同视图的行为需要变更同一状态。

对于问题一，传参的方法对于多层嵌套的组件将会非常繁琐，并且对于兄弟组件间的状态传递无能为力。

对于问题二，我们经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝。

以上的这些模式非常脆弱，通常会导致无法维护的代码。

因此，我们为什么不把组件的共享状态抽取出来，以一个全局单例模式管理呢？在这种模式下，我们的组件树构成了一个巨大的“视图”，不管在树的哪个位置，任何组件都能获取状态或者触发行为！

通过定义和隔离状态管理中的各种概念并通过强制规则维持视图和状态间的独立性，我们的代码将会变得更结构化且易维护。

这就是 Vuex 背后的基本思想，Vuex 是专门为 Vue.js 设计的状态管理库，以利用 Vue.js 的细粒度数据响应机制来进行高效的状态更新。

![](https://vuex.vuejs.org/vuex.png)

##  什么情况下我应该使用 Vuex？

Vuex 可以帮助我们管理共享状态，并附带了更多的概念和框架。这需要对短期和长期效益进行权衡。

如果您不打算开发大型单页应用，使用 Vuex 可能是繁琐冗余的。确实是如此——如果您的应用够简单，您最好不要使用 Vuex。一个简单的 [store 模式](https://cn.vuejs.org/v2/guide/state-management.html#简单状态管理起步使用)就足够您所需了。但是，如果您需要构建一个中大型单页应用，您很可能会考虑如何更好地在组件外部管理状态，Vuex 将会成为自然而然的选择。

使用场景总结为：

- 多个组件依赖于同—状态
- 来自不同组件的行为需要变更同—状态

## 官网

Vue3.x对应Vuex4.x版本

v4版本的官网：https://vuex.vuejs.org/zh/

## 安装

以Vue2中的方式使用为例：

方式一：在浏览器中直接使用，需要使用script标签导入vue-router.js

```html
<script src="https://unpkg.com/vuex@4"></script>
```

方式二：在脚手架中使用

```bash
npm install vuex@next --save
```

# Vuex的使用

## 安装vuex

```bash
npm install vuex@next --save
```

## 创建Store实例

在src目录中添加store文件夹，在store文件夹中创建`index.js`，在`index.js`中创建store实例

```js
// store/index.js

import { createStore } from 'vuex';

// store/index.js

export default createStore({
  // 在严格模式下，无论何时发生了状态变更且不是由 mutation 函数引起的，将会抛出错误。这能保证所有的状态变更都能被调试工具跟踪到
  strict: process.env.NODE_ENV !== 'production',
  
  // 设置仓库的初始状态：初始数据
  state:{
    count: 1,
		base: 10,
		message: 'Hello Vuex!',
  },
  
  // getter 就像计算属性一样，只有当它依赖的值发生了改变才会被重新计算。
  // 1、一般是做数据处理：排序、保留小数点等
  // 2、不需要直接改变getters中的属性，getters中所依赖的数据发生变化时，getters中的属性会自动计算
  getters:{
    bigCount: state => state.count * 2,
		baseCount: state => state.count * state.base,
  },
  
  // mutation: 定义一些函数，作用：改变state中数据
	// 1、改变state中数据唯一的方法就是提交mutation
	// 2、在mutation中的所有操作都必须是同步的
  mutations:{
		INCREASE_COUNT(state, value) {
			state.count += value;
		},
		DECREASE_COUNT(state, value) {
			state.count -= value;
		},
		SET_BASE(state, value) {
			state.base = value;
		},
  },
  
  // action: 类似mutation，也是定义一些函数，和mutation有些不一样
	// 1、action中不能直接改变state，需要在action中提交mutation
	// 2、action中可以包含异步操作：ajax请求等
  actions:{
		setCount(context, data) {
			if (data.type == 0) {
				context.commit('INCREASE_COUNT', data.value);
			} else {
				context.commit('DECREASE_COUNT', data.value);
			}
		},

		// 不停的点击按钮时候，让count的值每隔一秒种增加或者减少1
		setCountWaitOneSecond({ dispatch }, data) {
			setTimeout(() => {
				dispatch('setCount', data); //在一个action中分发另外一个action
			}, 1000);
		},
		setCountWidthBase({ dispatch, state }, data) {
      dispatch('setCount', { type: data.type, value: state.base });
		},
  }
});
```

在仓库store实例中添加数据，vuex的状态，就是数据

- state       仓库中数据存储位置， 
- getters     仓库数据的处理，类似于组件的computed
- mutations   同步方式改变仓库的数据，通过commit提交mutation来改变state中的数据
- actions     异步方式修改仓库的数据，通过dispatch触发action的异步操作来修改state，在action中依旧需要提交mutation才能修改state

## 状态的访问与修改

store状态的访问与修改有两种方式：

- 在JS模块中
- 在vue组件中

### 在JS模块中使用store

在JS模块中访问状态比较简单，只需要导入store实例，就可以直接访问和修改状态：

```js
// 在任意js文件中导入store实例
import store from './store'

// 访问仓库的状态state
store.state.count

// 访问仓库的getters
store.getters.bigCount

// 提交mutation改变状态
store.commit('INCREASE_COUNT', 1); 

// 分发action
store.dispatch('setCount', {type:0, value: 1});
```

### 在vue组件中使用store

Vue的组件有很多，并且所有的组件都是根组件的后代组件，如果按照在JS模块的方式导入store实例，显然很麻烦，不过vue提供了一种很方便的方式在每一个组件中访问store实例。

为了在 Vue 组件中访问 `this.$store` ，你需要为 Vue 实例提供创建好的 store。Vuex 提供了一个从根组件向所有子组件，以 `store` 选项的方式“注入”该 store 的机制：

```js
import { createApp } from 'vue';
import App from './App.vue';
import store from './store';

const app = createApp(App);

// 在根组件挂载store实例之后，在任何一个子组件中都能通过 this.$store 访问 store 实例
app.use(store);

app.mount('#app');
```

这样，就可以开心的在任意一个组件中通过 `this.$store` 来访问和修改状态

- 在组件内部

```js
this.$store //获取store实例

this.$store.state.count //访问state的数据

this.$store.getters.bigCount // 访问getters的数据

this.$store.commit('INCREASE_COUNT', 1); //提交mutation

this.$store.dispatch('setCount', {type:0, value: 1}); // 分发action
```

- 在组件的模板中

```html
<!-- 访问state的数据 -->
<p>count = {{$store.state.count}}</p>

<!-- 访问getters的数据 -->
<p>bigCount = {{$store.getters.bigCount}}</p>

<!-- 提交mutation -->
<button @click="$store.commit('INCREASE_COUNT', 1);">提交mutation</button>

<!-- 分发action -->
<button @click="$store.dispatch('setCount', {type:0, value: 1});">提交mutation</button>
```

# 组合式API

## 访问store

可以通过调用 `useStore` 函数，来在 `setup` 钩子函数中访问 store。这与在组件中使用选项式 API 访问 `this.$store` 是等效的。

```vue
<script setup>
  import { useStore } from 'vuex'

  // 获取store代替this.$store
  const store = useStore()
</script>
```

请注意，在模板中我们仍然可以访问 `$store`，所以不需要在 `setup` 中返回 `store` 。

## 访问 State 和 Getter

为了访问 state 和 getter，需要创建 `computed` 引用以保留响应性，这与在选项式 API 中创建计算属性等效。

```vue
<script setup>
  import { computed } from 'vue'
  import { useStore } from 'vuex'

  const store = useStore()

  // 在 computed 函数中访问 state
  const count = computed(() => store.state.count);

  // 在 computed 函数中访问 getter
  const double = computed(() => store.getters.double);
</script>
```

## 访问 Mutation 和 Action

要使用 mutation 和 action 时，只需要在 `setup` 钩子函数中调用 `commit` 和 `dispatch` 函数。

```vue
<script setup>
  import { computed } from 'vue'
  import { useStore } from 'vuex'

  const store = useStore()

  // 使用 mutation
  const increase = value => store.commit('INCREASE_COUNT', value);

  // 使用 action
  const setCount = data => store.dispatch('setCount', data);
  
   // 直接触发mutation的方法
  store.commit('INCREASE_COUNT')

  // 直接触发actions的方法
  store.dispatch('setCount')
</script>
```

# 从 3.x 迁移到 4.0

几乎所有的 Vuex 4 API 都与 Vuex 3 保持不变。但是，仍有一些非兼容性变更需要注意。

- 非兼容性变更
  - [安装过程](https://vuex.vuejs.org/zh/guide/migrating-to-4-0-from-3-x.html#安装过程)
  - [TypeScript 支持](https://vuex.vuejs.org/zh/guide/migrating-to-4-0-from-3-x.html#TypeScript-支持)
  - [打包产物已经与 Vue 3 配套](https://vuex.vuejs.org/zh/guide/migrating-to-4-0-from-3-x.html#打包产物已经与-Vue-3-配套)
  - [“createLogger”函数从核心模块导出](https://vuex.vuejs.org/zh/guide/migrating-to-4-0-from-3-x.html#“createLogger”函数从核心模块导出)
- 新特性
  - [全新的“useStore”组合式函数](https://vuex.vuejs.org/zh/guide/migrating-to-4-0-from-3-x.html#全新的“usestore”组合式函数)

## 非兼容性变更

### 安装过程

为了与 Vue 3 初始化过程保持一致，Vuex 的安装方式已经改变了。用户现在应该使用新引入的 `createStore` 方法来创建 store 实例。

```js
import { createStore } from 'vuex'

export const store = createStore({
  state () {
    return {
      count: 1
    }
  }
})
```

要将 Vuex 安装到 Vue 实例中，需要用 `store` 替代之前的 Vuex 传递给 `use` 方法。

```js
import { createApp } from 'vue'
import { store } from './store'
import App from './App.vue'

const app = createApp(App)

app.use(store)

app.mount('#app')
```

提示

从技术上讲这并不是一个非兼容性变更，仍然可以使用 `new Store(...)` 语法，但是建议使用上述方式以保持与 Vue 3 和 Vue Router Next 的一致。

### TypeScript 支持

为了修复 [issue #994](https://github.com/vuejs/vuex/issues/994)，Vuex 4 删除了 `this.$store` 在 Vue 组件中的全局类型声明。当使用 TypeScript 时，必须声明自己的[模块补充(module augmentation)](https://www.typescriptlang.org/docs/handbook/declaration-merging.html#module-augmentation)。

将下面的代码放到项目中，以允许 `this.$store` 能被正确的类型化：

```tsx
// vuex-shim.d.ts

import { ComponentCustomProperties } from 'vue'
import { Store } from 'vuex'

declare module '@vue/runtime-core' {
  // 声明自己的 store state
  interface State {
    count: number
  }

  interface ComponentCustomProperties {
    $store: Store<State>
  }
}
```

在 [TypeScript 支持](https://vuex.vuejs.org/zh/guide/typescript-support.html)章节可以了解到更多。

### 打包产物已经与 Vue 3 配套

下面的打包产物分别与 Vue 3 的打包产物配套：

- ```
  vuex.global(.prod).js
  ```

  - 通过`<script src="...">` 标签直接用在浏览器中，将 Vuex 暴露为全局变量。
  - 全局构建为 IIFE ， 而不是 UMD ，并且只能与 `<script src="...">` 一起使用。
  - 包含硬编码的 prod/dev 分支，并且生产环境版本已经压缩过。生产环境请使用 `.prod.js` 文件。

- ```
  vuex.esm-browser(.prod).js
  ```

  - 用于通过原生 ES 模块导入使用(在浏览器中通过 `<script type="module">` 标签使用)。

- ```
  vuex.esm-bundler.js
  ```

  - 用于与 `webpack`， `rollup`， `parcel` 等构建工具一起使用。
  - 通过 `process.env.NODE_ENV` 环境变量决定应该运行在生产环境还是开发环境（必须由构建工具替换）。
  - 不提供压缩后的构建版本(与打包后的其他代码一起压缩)

- ```
  vuex.cjs.js
  ```

  - 通过 `require` 在 Node.js 服务端渲染使用。

### “createLogger”函数从核心模块导出

在 Vuex 3 中，`createLogger` 方法从 `vuex/dist/logger` 文件中导出，但是现在该方法已经包含在核心包中了，应该直接从 `vuex` 包中引入。

```
import { createLogger } from 'vuex'
```

## 新特性

### 全新的“useStore”组合式函数

Vuex 4 引入了一个新的 API 用于在组合式 API 中与 store 进行交互。可以在组件的 `setup` 钩子函数中使用 `useStore` 组合式函数来检索 store。

```js
import { useStore } from 'vuex'

export default {
  setup () {
    const store = useStore()
  }
}
```







