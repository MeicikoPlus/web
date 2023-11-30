# 前言

全面拥抱 `Pinia` 吧！  

Pinia.js 是新一代的状态管理器，由 Vue.js团队中成员所开发的，因此也被认为是下一代的 Vuex，即 Vuex5.x，在 Vue3.0 的项目中使用也是备受推崇。

2021年11月24日，尤大在 Twitter 上宣布：`Pinia` 正式成为 Vue 官方的状态库，意味着 `Pinia` 就是 `Vuex5` ，`Pinia` 的优点： 

- 同时支持 Composition Api 和 Options api 的语法
- 去掉 mutations ，只有 state 、getters 和 actions （actions 支持同步和异步）
- 更完善的 Typescript 支持
- 足够轻量，压缩后的体积只有1.6kb;
- 清晰、显式的代码拆分
- 没有模块嵌套，只有 store 的概念，store 之间可以自由使用，更好的代码分割；
- 无需手动添加 store，store 一旦创建便会自动添加；

# Pinia是什么

## 简介

Pinia [起始](https://github.com/vuejs/pinia/commit/06aeef54e2cad66696063c62829dac74e15fd19e)于 2019 年 11 月左右的一次实验，其目的是设计一个拥有[组合式 API](https://github.com/vuejs/composition-api) 的 Vue 状态管理库。从那时起，我们就倾向于同时支持 Vue 2 和 Vue 3，并且不强制要求开发者使用组合式 API，我们的初心至今没有改变。除了**安装**和 **SSR** 两章之外，其余章节中提到的 API 均支持 Vue 2 和 Vue 3。虽然本文档主要是面向 Vue 3 的用户，但在必要时会标注出 Vue 2 的内容，因此 Vue 2 和 Vue 3 的用户都可以阅读本文档。

## 为什么你应该使用 Pinia？

Pinia 是 Vue 的专属状态管理库，它允许你跨组件或页面共享状态。如果你熟悉组合式 API 的话，你可能会认为可以通过一行简单的 `export const state = reactive({})` 来共享一个全局状态。对于单页应用来说确实可以，但如果应用在服务器端渲染，这可能会使你的应用暴露出一些安全漏洞。 而如果使用 Pinia，即使在小型单页应用中，你也可以获得如下功能：

- Devtools 支持
  - 追踪 actions、mutations 的时间线
  - 在组件中展示它们所用到的 Store
  - 让调试更容易的 Time travel
- 热更新
  - 不必重载页面即可修改 Store
  - 开发时可保持当前的 State
- 插件：可通过插件扩展 Pinia 功能
- 为 JS 开发者提供适当的 TypeScript 支持以及**自动补全**功能。
- 支持服务端渲染

## 为什么取名 Pinia？

Pinia (发音为 `/piːnjʌ/`，类似英文中的 “peenya”) 是最接近有效包名 piña (西班牙语中的 *pineapple*，即“菠萝”) 的词。 菠萝花实际上是一组各自独立的花朵，它们结合在一起，由此形成一个多重的水果。 与 Store 类似，每一个都是独立诞生的，但最终它们都是相互联系的。 它(菠萝)也是一种原产于南美洲的美味热带水果。

## 对比 Vuex

Pinia 起源于一次探索 Vuex 下一个迭代的实验，因此结合了 Vuex 5 核心团队讨论中的许多想法。最后，我们意识到 Pinia 已经实现了我们在 Vuex 5 中想要的大部分功能，所以决定将其作为新的推荐方案来代替 Vuex。

与 Vuex 相比，Pinia 不仅提供了一个更简单的 API，也提供了符合组合式 API 风格的 API，最重要的是，搭配 TypeScript 一起使用时有非常可靠的类型推断支持。

### RFC

最初，Pinia 没有经过任何 RFC 的流程。我基于自己开发应用的经验，同时通过阅读其他人的代码，为使用 Pinia 的用户工作，以及在 Discord 上回答问题等方式验证了一些想法。 这些经历使我产出了这样一个可用的解决方案，并适应了各种场景和应用规模。我会一直在保持其核心 API 不变的情况下发布新版本，同时不断优化本库。

现在 Pinia 已经成为推荐的状态管理解决方案，它和 Vue 生态系统中的其他核心库一样，都要经过 RFC 流程，它的 API 也已经进入稳定状态。

### 对比 Vuex 3.x/4.x

> Vuex 3.x 只适配 Vue 2，而 Vuex 4.x 是适配 Vue 3 的。

Pinia API 与 Vuex(<=4) 也有很多不同，即：

- *mutation* 已被弃用。它们经常被认为是**极其冗余的**。它们初衷是带来 devtools 的集成方案，但这已不再是一个问题了。
- 无需要创建自定义的复杂包装器来支持 TypeScript，一切都可标注类型，API 的设计方式是尽可能地利用 TS 类型推理。
- 无过多的魔法字符串注入，只需要导入函数并调用它们，然后享受自动补全的乐趣就好。
- 无需要动态添加 Store，它们默认都是动态的，甚至你可能都不会注意到这点。注意，你仍然可以在任何时候手动使用一个 Store 来注册它，但因为它是自动的，所以你不需要担心它。
- 不再有嵌套结构的**模块**。你仍然可以通过导入和使用另一个 Store 来隐含地嵌套 stores 空间。虽然 Pinia 从设计上提供的是一个扁平的结构，但仍然能够在 Store 之间进行交叉组合。**你甚至可以让 Stores 有循环依赖关系**。
- 不再有**可命名的模块**。考虑到 Store 的扁平架构，Store 的命名取决于它们的定义方式，你甚至可以说所有 Store 都应该命名。

## 官网

- https://pinia.vuejs.org/zh/

## 安装

```bash
npm install pinia
```

> TIP
>
> 如果你的应用使用的是 Vue 2，你还需要安装组合式 API 包：`@vue/composition-api`。

如果你正在使用 Vue CLI，你可以试试这个[**非官方插件**](https://github.com/wobsoriano/vue-cli-plugin-pinia)。

# Pinia的使用

## 安装pinia

```bash
npm install pinia
```

## 创建一个 pinia 实例(根 store)

在src目录中添加stores文件夹，在stores文件夹中创建`index.js`，在`index.js`中创建store实例

```js
import { createPinia } from 'pinia';

const pinia = createPinia();

export default pinia;
```

## 将pinia传递给Vue应用

```js
import { createApp } from 'vue';
import App from './App.vue';
import pinia from './stores';

const app = createApp(App);

app.use(pinia);

app.mount('#app');
```

如果你使用的是 Vue 2，你还需要安装一个插件，并在应用的根部注入创建的 `pinia`：

```js
// src/stores/index.js
import Vue from 'vue';
import { createPinia, PiniaVuePlugin } from 'pinia';

Vue.use(PiniaVuePlugin);

const pinia = createPinia();

export default pinia;
```

```js
import Vue from 'vue';
import App from './App.vue';
import pinia from './stores';

new Vue({
  render: h => h(App),
  // 请注意，同一个`pinia'实例
  // 可以在同一个页面的多个 Vue 应用中使用。 
  pinia, 
}).$mount('#app'); 
```

这样才能提供 devtools 的支持。在 Vue 3 中，一些功能仍然不被支持，如 time traveling 和编辑，这是因为 vue-devtools 还没有相关的 API，但 devtools 也有很多针对 Vue 3 的专属功能，而且就开发者的体验来说，Vue 3 整体上要好得多。在 Vue 2 中，Pinia 使用的是 Vuex 的现有接口(因此不能与 Vuex 一起使用)。

## 使用 Store

虽然我们前面定义了一个 store，但在 `setup()` 调用 `useStore()` 之前，store 实例是不会被创建的：

```vue
<script setup>
import { useCounterStore } from '@/stores/counter';
import { ref, computed } from 'vue';
import { storeToRefs } from 'pinia';

const counterStore = useCounterStore();

// 一、获取state
// 1、直接访问
// console.log(counterStore.count);
// 2、作为计算属性
// const count = computed(() => counterStore.count);
// 3、state 也可以使用解构，但使用解构会使其失去响应式，这时候可以用 pinia 的 storeToRefs。
const { count } = storeToRefs(counterStore);

// 二、获取getters
// 1、直接获取
// console.log(counterStore.bigCount);
// 2、作为计算属性
// const bigCount = computed(() => counterStore.bigCount);
// 3、使用storeToRefs解构
const { bigCount } = storeToRefs(counterStore);

// 三、修改state
// 1、直接修改state，但是不推荐使用
// counterStore.count = 12;
// 2、推荐通过action修改state

// 四、调用action
// counterStore.setCount({ type: 0, value: 1 });
// counterStore.setCountWaitOneSecond({ type: 0, value: 1 });
const { setCount, setCountWaitOneSecond } = counterStore;
</script>

<template>
	<div>
		<p>count = {{ count }}</p>
		<p>counterStore.count = {{ counterStore.count }}</p>
		<p>bigCount = {{ bigCount }}</p>
		<p>counterStore.bigCount = {{ counterStore.bigCount }}</p>
		<button @click="counterStore.setCount({ type: 0, value: 1 })">setCount</button>
		<button @click="setCount({ type: 0, value: 1 })">setCount</button>
		<button @click="setCountWaitOneSecond({ type: 0, value: 1 })">setCountWaitOneSecond</button>	</div>
  </div>
</template>
```

你可以定义任意多的 store，但为了让使用 pinia 的益处最大化(比如允许构建工具自动进行代码分割以及 TypeScript 推断)，**你应该在不同的文件中去定义 store**。

如果你还不会使用 `setup` 组件，[你也可以通过**映射辅助函数**来使用 Pinia](https://pinia.vuejs.org/zh/cookbook/options-api.html)。

一旦 store 被实例化，你可以直接访问在 store 的 `state`、`getters` 和 `actions` 中定义的任何属性。我们将在后续章节继续了解这些细节，目前自动补全将帮助你使用相关属性。

请注意，`store` 是一个用 `reactive` 包装的对象，这意味着不需要在 getters 后面写 `.value`，就像 `setup` 中的 `props` 一样，**如果你写了，我们也不能解构它**：

```js
export default defineComponent({
  setup() {
    const store = useCounterStore()
    // ❌ 这将无法生效，因为它破坏了响应性
    // 这与从 `props` 中解构是一样的。
    const { name, doubleCount } = store

    name // "eduardo"
    doubleCount // 2

    return {
      // 始终是 "eduardo"
      name,
      // 始终是 2
      doubleCount,
      // 这个将是响应式的
      doubleValue: computed(() => store.doubleCount),
      }
  },
})
```

为了从 store 中提取属性时保持其响应性，你需要使用 `storeToRefs()`。它将为每一个响应式属性创建引用。当你只使用 store 的状态而不调用任何 action 时，它会非常有用。请注意，你可以直接从 store 中解构 action，因为它们也被绑定到 store 上：

```js
import { storeToRefs } from 'pinia'

export default defineComponent({
  setup() {
    const store = useCounterStore()
    // `name` and `doubleCount` 都是响应式 refs
    // 这也将为由插件添加的属性创建 refs
    // 同时会跳过任何 action 或非响应式(非 ref/响应式)属性
    const { name, doubleCount } = storeToRefs(store)
    // 名为 increment 的 action 可以直接提取
    const { setCount } = store

    return {
      name,
      doubleCount,
      setCount,
    }
  },
})
```

## store的理解

### Store 是什么？

Store (如 Pinia) 是一个保存状态和业务逻辑的实体，它并不与你的组件树绑定。换句话说，**它承载着全局状态**。它有点像一个永远存在的组件，每个组件都可以读取和写入它。它有**三个概念**，[state](https://pinia.vuejs.org/zh/core-concepts/state.html)、[getter](https://pinia.vuejs.org/zh/core-concepts/getters.html) 和 [action](https://pinia.vuejs.org/zh/core-concepts/actions.html)，我们可以假设这些概念相当于组件中的 `data`、 `computed` 和 `methods`。

### 应该在什么时候使用 Store?

一个 Store 应该包含可以在整个应用中访问的数据。这包括在许多地方使用的数据，例如显示在导航栏中的用户信息，以及需要通过页面保存的数据，例如一个非常复杂的多步骤表单。

另一方面，你应该避免在 Store 中引入那些原本可以在组件中保存的本地数据，例如，一个元素在页面中的可见性。

并非所有的应用都需要访问全局状态，但如果你的应用确实需要一个全局状态，那 Pinia 将使你的开发过程更轻松。

# 核心概念

## 定义 Store

在深入研究核心概念之前，我们得知道 Store 是用 `defineStore()` 定义的，它的第一个参数要求是一个**独一无二的**名字：

```js
import { defineStore } from 'pinia'

// 你可以对 defineStore() 的返回值进行任意命名，
// 但最好使用 store 的名字，同时以 use 开头且以 Store 结尾。(比如 `useUserStore`，`useCartStore`，`useProductStore`)
// 第一个参数是你的应用中 Store 的唯一 ID。
export const useCounterStore = defineStore('counter', {
  // 其他配置...
})
```

这个**名字** ，也被用作 *id* ，是必须传入的， Pinia 将用它来连接 store 和 devtools。为了养成习惯性的用法，将返回的函数命名为 *use...* 是一个符合组合式函数风格的约定。

`defineStore()` 的第二个参数可接受两类值：Setup 函数或 Option 对象。

### Option Store

与 Vue 的选项式 API 类似，我们也可以传入一个带有 `state`、`actions` 与 `getters` 属性的 Option 对象

```js
import { defineStore } from 'pinia';

export const useCounterStore = defineStore('counter', {
  // 推荐使用 完整类型推断的箭头函数
  state: () => {
    return {
      count: 10,
    };
  },
  getters: {
    bigCount: state => state.count * 2,
  },
  // action可以包含同步操作，也可以包含异步操作
  actions: {
    setCount({ type, value }) {
      if (type == 0) {
        this.count += value;
      } else {
        this.count -= value;
      }
    },
    // 不停的点击按钮时候，让count的值每隔一秒种增加或者减少1
    setCountWaitOneSecond({ type, value }) {
      this.setCount({ type, value }); // action 间的相互调用，直接用 this 访问即可。
    },
  },
})
```

你可以认为 `state` 是 store 的数据 (`data`)，`getters` 是 store 的计算属性 (`computed`)，而 `actions` 则是方法 (`methods`)。

为方便上手使用，Option Store 应尽可能直观简单。

### Setup Store

也存在另一种定义 store 的可用语法。与 Vue 组合式 API 的 [setup 函数](https://cn.vuejs.org/api/composition-api-setup.html) 相似，我们可以传入一个函数，该函数定义了一些响应式属性和方法，并且返回一个带有我们想暴露出去的属性和方法的对象。

```js
import { defineStore } from 'pinia';
import { ref, computed } from 'vue';


export const useCounterStore = defineStore('counter', () => {
	// 1、ref() 就是 state 属性
	const count = ref(10);

	// 2、computed() 就是 getters
	const bigCount = computed(() => count.value * 2);

	// 3、function() 就是 actions
	const setCount = ({ type, value }) => {
		if (type == 0) {
			count.value += value;
		} else {
			count.value -= value;
		}
	};

	const setCountWaitOneSecond = ({ type, value }) => {
		setTimeout(() => {
			setCount({ type, value }); // action 间的相互调用，直接调用函数即可。
		}, 1000);
	};

	return {
		count,
		bigCount,
		setCount,
		setCountWaitOneSecond,
	};
});

```

在 *Setup Store* 中：

- `ref()` 就是 `state` 属性
- `computed()` 就是 `getters`
- `function()` 就是 `actions`

Setup store 比 Option Store 带来了更多的灵活性，因为你可以在一个 store 内创建侦听器，并自由地使用任何组合式函数。

## State

### 定义state

在大多数情况下，state 都是你的 store 的核心。人们通常会先定义能代表他们 APP 的 state。在 Pinia 中，state 被定义为一个返回初始状态的函数。这使得 Pinia 可以同时支持服务端和客户端。

```js
import { defineStore } from 'pinia'

const useCounterStore = defineStore('counter', {
  // 为了完整类型推理，推荐使用箭头函数
  state: () => {
    return {
      // 所有这些属性都将自动推断出它们的类型
      count: 10,
      name: 'Eduardo',
      isAdmin: true,
      items: [],
      hasChanged: true,
    }
  },
})
```

### 访问 state

默认情况下，你可以通过 `store` 实例访问 state，直接对其进行读写。

```js
import { useCounterStore } from '@/stores/counter';

const counterStore = useCounterStore();

counterStore.count++
```

### 重置 state

你可以通过调用 store 的 `$reset()` 方法将 state 重置为初始值。

```js
import { useCounterStore } from '@/stores/counter';

const counterStore = useCounterStore();

counterStore.$reset();
```

### 变更 state

除了用 `counterStore.count++` 直接改变 store，你还可以调用 `$patch` 方法。它允许你用一个 `state` 的补丁对象在同一时间更改多个属性：

```js
counterStore.$patch({
  count: store.count + 1,
  age: 120,
  name: 'DIO',
})
```

不过，用这种语法的话，有些变更真的很难实现或者很耗时：任何集合的修改（例如，向数组中添加、移除一个元素或是做 `splice` 操作）都需要你创建一个新的集合。因此，`$patch` 方法也接受一个函数来组合这种难以用补丁对象实现的变更。

```js
counterStore.$patch((state) => {
  state.items.push({ name: 'shoes', quantity: 1 })
  state.hasChanged = true
})
```

两种变更 store 方法的主要区别是，`$patch()` 允许你将多个变更归入 devtools 的同一个条目中。同时请注意，**直接修改 `state`，`$patch()` 也会出现在 devtools 中**，而且可以进行 time travel(在 Vue 3 中还没有)。

### 替换 state

你**不能完全替换掉** store 的 state，因为那样会破坏其响应性。但是，你可以 *patch* 它。

```js
// 这实际上并没有替换`$state`
counterStore.$state = { count: 24 }
// 在它内部调用 `$patch()`：
counterStore.$patch({ count: 24 })
```

你也可以通过变更 `pinia` 实例的 `state` 来设置整个应用的初始 state。

```js
pinia.state.value = {}
```

## Getter

### 定义getter

Getter 完全等同于计算属性。可以通过 `defineStore()` 中的 `getters` 属性来定义它们。**推荐**使用箭头函数，并且它将接收 `state` 作为第一个参数：

```js
export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 10,
  }),
  getters: {
		bigCount: state => state.count * 2,
  },
})
```

大多数时候，getter 仅依赖 state，不过，有时它们也可能会使用其他 getter。因此，即使在使用常规函数定义 getter 时，我们也可以通过 `this` 访问到**整个 store 实例**，**但(在 TypeScript 中)必须定义返回类型**。这是为了避免 TypeScript 的已知缺陷，**不过这不影响用箭头函数定义的 getter，也不会影响不使用 `this` 的 getter**。

```js
export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 10,
  }),
  getters: {
    // 自动推断出返回类型是一个 number
    bigCount(state) {
      return state.count * 2
    },
    // 返回类型**必须**明确设置
    bigPlusOne(): number {
      // 整个 store 的 自动补全和类型标注 ✨
      return this.bigCount + 1
    },
  },
})
```

然后你可以直接访问 store 实例上的 getter 了：

```vue
<script setup>
  import { useCounterStore } from '@/stores/counter';

  const counterStore = useCounterStore();
</script>

<template>
  <p>Big count is {{ counterStore.bigCount }}</p>
</template>
```

### 访问其他 getter

与计算属性一样，你也可以组合多个 getter。通过 `this`，你可以访问到其他任何 getter。即使你没有使用 TypeScript，你也可以用 [JSDoc](https://jsdoc.app/tags-returns.html) 来让你的 IDE 提示类型。

```js
export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 10,
  }),
  getters: {
    // 类型是自动推断出来的，因为我们没有使用 `this`
    bigCount: (state) => state.count * 2,
    // 这里我们需要自己添加类型(在 JS 中使用 JSDoc)
    // 可以用 this 来引用 getter
    /**
     * 返回 count 的值乘以 2 加 1
     *
     * @returns {number}
     */
    bigCountPlusOne() {
      // 自动补全 ✨
      return this.bigCount + 1
    },
  },
})
```

### 向 getter 传递参数

*Getter* 只是幕后的**计算**属性，所以不可以向它们传递任何参数。不过，你可以从 *getter* 返回一个函数，该函数可以接受任意参数：

```js
export const useStore = defineStore('main', {
  getters: {
    getUserById: (state) => {
      return (userId) => state.users.find((user) => user.id === userId)
    },
  },
})
```

并在组件中使用：

```vue
<script setup>
  import { useMainStore } from '@/stores/main';

  const mainStore = useStore();
  const getUserById = mainStore.getUserById;
</script>

<template>
  <p>User 2: {{ getUserById(2) }}</p>
</template>
```

请注意，当你这样做时，**getter 将不再被缓存**，它们只是一个被你调用的函数。不过，你可以在 getter 本身中缓存一些结果，虽然这种做法并不常见，但有证明表明它的性能会更好：

```js
export const useStore = defineStore('main', {
  getters: {
    getActiveUserById(state) {
      const activeUsers = state.users.filter((user) => user.active)
      return (userId) => activeUsers.find((user) => user.id === userId)
    },
  },
})
```

### 访问其他 store 的 getter

想要使用另一个 store 的 getter 的话，那就直接在 *getter* 内使用就好：

```js
import { useOtherStore } from './other-store'

export const useStore = defineStore('main', {
  state: () => ({
    // ...
  }),
  getters: {
    otherGetter(state) {
      const otherStore = useOtherStore()
      return state.localData + otherStore.data
    },
  },
})
```

## Action

### 定义action

Action 相当于组件中的 method。它们可以通过 `defineStore()` 中的 `actions` 属性来定义，**并且它们也是定义业务逻辑的完美选择。**

```js
export const useCounterStore = defineStore('counter', {
	state: () => {
		return {
			count: 10,
		};
	},
	// action可以包含同步操作，也可以包含异步操作
	actions: {
		setCount({ type, value }) {
			if (type == 0) {
				this.count += value;
			} else {
				this.count -= value;
			}
		},
		// 不停的点击按钮时候，让count的值每隔一秒种增加或者减少1
		setCountWaitOneSecond({ type, value }) {
			setTimeout(() => {
				this.setCount({ type, value }); // action 间的相互调用，直接用 this 访问即可。
			}, 1000);
		},
	},
});
```

类似 getter，action 也可通过 `this` 访问**整个 store 实例**，并支持**完整的类型标注(以及自动补全✨)**。**不同的是，`action` 可以是异步的**，你可以在它们里面 `await` 调用任何 API，以及其他 action！下面是一个使用 [Mande](https://github.com/posva/mande) 的例子。请注意，你使用什么库并不重要，只要你得到的是一个`Promise`，你甚至可以(在浏览器中)使用原生 `fetch` 函数：

```js
import { mande } from 'mande'

const api = mande('/api/users')

export const useUsers = defineStore('users', {
  state: () => ({
    userData: null,
    // ...
  }),

  actions: {
    async registerUser(login, password) {
      try {
        this.userData = await api.post({ login, password })
        showTooltip(`Welcome back ${this.userData.name}!`)
      } catch (error) {
        showTooltip(error)
        // 让表单组件显示错误
        return error
      }
    },
  },
})
```

你也完全可以自由地设置任何你想要的参数以及返回任何结果。当调用 action 时，一切类型也都是可以被自动推断出来的。

Action 可以像方法一样被调用：

```html
<script setup>
  const counterStore = useCounterStore();
  
  // 调用action
  // 作为 store 的一个方法调用该 action
  counterStore.setCount({ type: 0, value: 1 });
  counterStore.setCountWaitOneSecond({ type: 0, value: 1 });
  
</script>
```

### 访问其他 store 的 action

想要使用另一个 store 的话，那你直接在 *action* 中调用就好了：

```js
import { useAuthStore } from './auth-store'

export const useSettingsStore = defineStore('settings', {
  state: () => ({
    preferences: null,
    // ...
  }),
  actions: {
    async fetchUserPreferences() {
      const auth = useAuthStore()
      if (auth.isAuthenticated) {
        this.preferences = await fetchPreferences()
      } else {
        throw new Error('User must be authenticated')
      }
    },
  },
})
```

## 插件

由于有了底层 API 的支持，Pinia store 现在完全支持扩展。以下是你可以扩展的内容：

- 为 store 添加新的属性
- 定义 store 时增加新的选项
- 为 store 增加新的方法
- 包装现有的方法
- 改变甚至取消 action
- 实现副作用，如[本地存储](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
- **仅**应用插件于特定 store

插件是通过 `pinia.use()` 添加到 pinia 实例的。最简单的例子是通过返回一个对象将一个静态属性添加到所有 store。

```js
import { createPinia } from 'pinia'

// 在安装此插件后创建的每个 store 中都会添加一个名为 `secret` 的属性。
// 插件可以保存在不同的文件中
function SecretPiniaPlugin() {
  return { secret: 'the cake is a lie' }
}

const pinia = createPinia()
// 将该插件交给 Pinia
pinia.use(SecretPiniaPlugin)

// 在另一个文件中
const store = useStore()
store.secret // 'the cake is a lie'
```

这对添加全局对象很有用，如路由器、modal 或 toast 管理器。

Pinia 插件是一个函数，可以选择性地返回要添加到 store 的属性。它接收一个可选参数，即 *context*。

```js
export function myPiniaPlugin(context) {
  context.pinia // 用 `createPinia()` 创建的 pinia。 
  context.app // 用 `createApp()` 创建的当前应用(仅 Vue 3)。
  context.store // 该插件想扩展的 store
  context.options // 定义传给 `defineStore()` 的 store 的可选对象。
  // ...
}
```

然后用 `pinia.use()` 将这个函数传给 `pinia`：

```js
pinia.use(myPiniaPlugin)
```

插件只会应用于**在 `pinia` 传递给应用后**创建的 store，否则它们不会生效。

## 在组件外使用 store

Pinia store 依靠 `pinia` 实例在所有调用中共享同一个 store 实例。大多数时候，只需调用你定义的 `useStore()` 函数，完全开箱即用。例如，在 `setup()` 中，你不需要再做任何事情。但在组件之外，情况就有点不同了。 实际上，`useStore()` 给你的 `app` 自动注入了 `pinia` 实例。这意味着，如果 `pinia` 实例不能自动注入，你必须手动提供给 `useStore()` 函数。 你可以根据不同的应用，以不同的方式解决这个问题。

### 单页面应用

如果你不做任何 SSR(服务器端渲染)，在用 `app.use(pinia)` 安装 pinia 插件后，对 `useStore()` 的任何调用都会正常执行：

```js
import { useUserStore } from '@/stores/user'
import { createApp } from 'vue'
import App from './App.vue'

// ❌  失败，因为它是在创建 pinia 之前被调用的
const userStore = useUserStore()

const pinia = createPinia()
const app = createApp(App)
app.use(pinia)

// ✅ 成功，因为 pinia 实例现在激活了
const userStore = useUserStore()
```

为确保 pinia 实例被激活，最简单的方法就是将 `useStore()` 的调用放在 pinia 安装后才会执行的函数中。

让我们来看看这个在 Vue Router 的导航守卫中使用 store 的例子。

```js
import { createRouter } from 'vue-router'
const router = createRouter({
  // ...
})

// ❌ 由于引入顺序的问题，这将失败
const userStore = useUserStore()

router.beforeEach((to, from, next) => {
  // 我们想用这里的 store
  if (store.isLoggedIn) next()
  else next('/login')
})

router.beforeEach((to) => {
  // ✅ 这样做是可行的，因为路由器在安装完之后就会开始导航。
  // Pinia 也将被安装。
  const userStore = useUserStore()

  if (to.meta.requiresAuth && !store.isLoggedIn) return '/login'
})
```

# 组合式 Store

组合式 store 是可以相互使用，Pinia 当然也支持它。但有一个规则需要遵循：

如果**两个或更多的 store 相互使用**，它们不可以通过 *getters* 或 *actions* 创建一个无限循环。它们也不可以**同时**在它们的 setup 函数中直接互相读取对方的 state：

```js
const useX = defineStore('x', () => {
  const y = useY()

  // ❌ 这是不可以的，因为 y 也试图读取 x.name
  y.name

  function doSomething() {
    // ✅ 读取 computed 或 action 中的 y 属性
    const yName = y.name
    // ...
  }

  return {
    name: ref('I am X'),
  }
})

```

```js
const useY = defineStore('y', () => {
  const x = useX()

  // ❌ 这是不可以的，因为 x 也试图读取 y.name
  x.name

  function doSomething() {
    // ✅ 读取 computed 或 action 中的 x 属性
    const xName = x.name
    // ...
  }

  return {
    name: ref('I am Y'),
  }
})
```

## 嵌套 store

注意，如果一个 store 使用另一个 store，你可以直接导入并在 *actions* 和 *getters* 中调用 `useStore()` 函数。然后你就可以像在 Vue 组件中那样使用 store。

对于 *setup store* ，你直接使用 store 函数**顶部**的一个 store：

```js
import { useUserStore } from './user'

export const useCartStore = defineStore('cart', () => {
  const user = useUserStore()

  const summary = computed(() => {
    return `Hi ${user.name}, you have ${state.list.length} items in your cart. It costs ${state.price}.`
  })

  function purchase() {
    return apiPurchase(user.id, this.list)
  }

  return { summary, purchase }
})
```

## 共享 Getter

你可以直接在一个 *getter* 中调用 `useOtherStore()`：

```js
import { defineStore } from 'pinia'
import { useUserStore } from './user'

export const useCartStore = defineStore('cart', {
  getters: {
    summary(state) {
      const user = useUserStore()

      return `Hi ${user.name}, you have ${state.list.length} items in your cart. It costs ${state.price}.`
    },
  },
})
```

## 共享 Actions

*actions* 也一样：

```js
import { defineStore } from 'pinia'
import { useUserStore } from './user'

export const useCartStore = defineStore('cart', {
  actions: {
    async orderCart() {
      const user = useUserStore()

      try {
        await apiOrderCart(user.token, this.items)
        // 其他 action
        this.emptyCart()
      } catch (err) {
        displayError(err)
      }
    },
  },
})
```

# 处理组合式函数

组合式函数是利用 Vue 组合式 API 来封装和复用有状态逻辑的函数。无论你是自己写，还是使用外部库，或者两者都有，你都可以在 pinia store 中充分发挥组合式函数的力量。

## Option Stores

当定义一个 option store 时，你可以在 `state` 属性中调用组合式函数：

```js
export const useAuthStore = defineStore('auth', {
  state: () => ({
    user: useLocalStorage('pinia/auth/login', 'bob'),
  }),
})
```

请记住，**你只能返回可写的状态**(例如，一个 `ref()`)。下面是一些可用的组合式函数的示例：

- [useLocalStorage](https://vueuse.org/core/useLocalStorage/)
- [useAsyncState](https://vueuse.org/core/useAsyncState/)

下面是一些不可在 option store 中使用的组合式函数(但可在 setup store 中使用)：

- [useMediaControls](https://vueuse.org/core/useMediaControls/): exposes functions
- [useMemoryInfo](https://vueuse.org/core/useMemory/): exposes readonly data
- [useEyeDropper](https://vueuse.org/core/useEyeDropper/): exposes readonly data and functions

## Setup Stores

另外，当定义一个 setup store 时，你几乎可以使用任何组合式函数，因为每一个属性都会被辨别为 state 、action 或者 getter：

```js
import { defineStore, skipHydrate } from 'pinia'
import { useMediaControls } from '@vueuse/core'

export const useVideoPlayer = defineStore('video', () => {
  // 我们不会直接暴露这个元素
  const videoElement = ref<HTMLVideoElement>()
  const src = ref('/data/video.mp4')
  const { playing, volume, currentTime, togglePictureInPicture } =
    useMediaControls(video, { src })

  function loadVideo(element: HTMLVideoElement, src: string) {
    videoElement.value = element
    src.value = src
  }

  return {
    src,
    playing,
    volume,
    currentTime,

    loadVideo,
    togglePictureInPicture,
  }
})
```

# 数据持久化

插件 pinia-plugin-persist 可以辅助实现数据持久化功能。

## 安装

```bash
npm i pinia-plugin-persist --save
```

## 使用

```javascript
// src/stores/index.js

import { createPinia } from 'pinia'
import piniaPluginPersist from 'pinia-plugin-persist'

const pinia = createPinia();
pinia.use(piniaPluginPersist)

export default pinia;
```

接着在对应的 store 里开启 persist 即可。

```javascript
export const useUserStore = defineStore('user', {
	state: () => {
		return {
			name: '未知',
      age: 0,
      sex:'未知'
		};
	},
	getters: {
		userinfo: state => `这是${state.name}，今年${state.age}岁`,
	},
	actions: {
		setUser(name, age) {
			this.name = name;
			this.age = age;
		},
	},
	// 开启数据缓存
	persist: {
    enabled: true,
	},
});

```

数据默认存在 sessionStorage 里，并且会以 store 的 id 作为 key。

## 自定义 key

你也可以在 strategies 里自定义 key 值，并将存放位置由 sessionStorage 改为 localStorage。

```javascript
persist: {
  enabled: true,
  strategies: [
    {
      key: 'my_user',
      storage: localStorage,
    }
  ]
}
```

## 持久化部分 state

默认所有 state 都会进行缓存，你可以通过 paths 指定要持久化的字段，其他的则不会进行持久化。

```javascript
state: () => {
  return {
    name: '张三',
    age: 18,
    gender: '男'
  }  
},

persist: {
  enabled: true,
  strategies: [
    {
      storage: localStorage,
      paths: ['name', 'age']
    }
  ]
}
```

上面我们只持久化 name 和 age，并将其改为localStorage, 而 gender 不会被持久化，如果其状态发生更改，页面刷新时将会丢失，重新回到初始状态，而 name 和 age 则不会。
