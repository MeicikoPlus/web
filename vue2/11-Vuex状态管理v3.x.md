# Vuex简介

## 介绍

Vuex 是一个专为 Vue.js 应用程序开发的**状态（数据）管理模式**。**在Vue中实现集中式状态管理的一个Vue插件**，对vue应用中多个组件的共享状态进行集中式的管理（读/写），并以相应的规则保证状态以一种可预测的方式发生变化。也是一种组件间通信的方式，且适用于任意组件间通信。

在vue开发中，每个组件都有自己的独立的数据，整个项目中的所有组件可以通过bus总线进行传值，但是如果出现组件之间需要共用同一组数据时，数据管理就会非常麻烦。vuex是vue的状态(数据)管理工具，它采取了一种集中管理数据的思想，将整个项目中所有的公共数据放在一个统一的仓库中，然后任何组件都可以从这个仓库中读取数据，也可以通过仓库提供的方法修改数据。

## 什么是“状态管理模式”？

让我们从一个简单的 Vue 计数应用开始：

```js
new Vue({
  // state
  data () {
    return {
      count: 0
    }
  },
  // view
  template: `
    <div>{{ count }}</div>
  `,
  // actions
  methods: {
    increment () {
      this.count++
    }
  }
})
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

Vuex目前主要有两个版本v3.x和v.4x。

- Vue2.x对应Vuex的v3.x版本

- Vue3.x对应Vuex的v4.x版本

v3版本的官网：https://v3.vuex.vuejs.org/zh/

v4版本的官网：https://vuex.vuejs.org/zh/

## 安装

以Vue2中的方式使用为例：

方式一：在浏览器中直接使用，需要使用script标签导入vux.js

```html
<script src="https://unpkg.com/vuex@3"></script>
```

方式二：在脚手架中使用

安装vue-router的时候需要指定版本

- vue2.x对应vuex3.x：`npm install vuex@3`
- vue3.x对应vuex4.x：`npm install vuex@4`

# Vuex的使用

## 安装vuex

```bash
npm install vuex@3
```

## 导入vuex并使用

在src目录中添加store文件夹，在store文件夹中创建`index.js`，在`index.js`中安装Vuex，创建store实例

- 在一个模块化的打包系统中，您必须显式地通过 `Vue.use()` 来安装 Vuex：

```js
// store/index.js

import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
```

## 新建Store仓库对象

在store文件夹中的`index.js`中创建仓库实例store并导出：

```js
// store/index.js

const store = new Vuex.Store({
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
  }
  
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
  }
  
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

export default sotre;
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

Vue的组件有很多，并且所有的组件都是根组件 `new Vue`的后代组件，如果按照在JS模块的方式导入store实例，显然很麻烦，不过vue提供了一种很方便的方式在每一个组件中访问store实例。

为了在 Vue 组件中访问 `this.$store` ，你需要为 Vue 实例提供创建好的 store。Vuex 提供了一个从根组件向所有子组件，以 `store` 选项的方式“注入”该 store 的机制：

```js
new Vue({
    // 在根组件挂载store实例之后，在任何一个子组件中都能通过 this.$store 访问 store 实例
    store,//挂载store
    render: h => h(App),
}).$mount('#app')
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

# 核心概念

## store

每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。Vuex 和单纯的全局对象有以下两点不同：

Vuex 和单纯的全局对象有以下两点不同：

1、**Vuex 的状态存储是响应式的**。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。

2、**你不能直接改变 store 中的状态**。改变 store 中的状态的唯一途径就是显式地提交 (commit) mutation。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。

```js
new Vuex.Store({
 
});
```

## State

### 单一状态树

Vuex 使用**单一状态树**——是的，用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源”而存在。这也意味着，每个应用将仅仅包含一个 store 实例。单一状态树让我们能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照。

存储在 Vuex 中的数据和 Vue 实例中的 `data` 遵循相同的规则。

```js
new Vuex.Store({
  state: {
		count: 1,
		base: 10,
		message: 'Hello Vuex!',
	},
});
```

### 访问State

State 会暴露为 `store.state` 对象，你可以以属性的形式访问这些值：

```js
store.state.count
```

### 函数形式的State

在模块化的vuex中，将 store 分割成**模块（module）**。每个模块拥有自己的 state，此时state应该是一个函数，与组件实例的data是一个函数保持一致：

```js
new Vuex.Store({
  state:() => ({
		count: 1,
		base: 10,
		message: 'Hello Vuex!',
	}),
});
```

### 组件仍然保有局部状态

使用 Vuex 并不意味着你需要将**所有的**状态放入 Vuex。虽然将所有的状态放到 Vuex 会使状态变化更显式和易调试，但也会使代码变得冗长和不直观。如果有些状态严格属于单个组件，最好还是作为组件的局部状态。你应该根据你的应用开发需要进行权衡和确定。

## Getters 

### vue的计算属性

有时候我们需要从 store 中的 state 中派生出一些状态，例如对count进行翻倍处理：

```js
computed: {
  bigCount () {
    return this.$store.state.count * 2;
  }
}
```

如果有多个组件需要用到此属性，我们要么复制这个函数，或者抽取到一个共享函数然后在多处导入它——无论哪种方式都不是很理想。

### store的计算属性

Vuex 允许我们在 store 中定义“getter”（可以认为是 store 的计算属性）。

就像组件的计算属性一样，getter 的返回值会根据它的依赖被缓存起来，且只有当它的依赖值发生了改变才会被重新计算。

Getter 接受 state 作为其第一个参数：

```js
new Vuex.Store({
	getters: {
		bigCount: state => state.count * 2,
		baseCount: state => state.count * state.base,
	},
});
```

### 访问Getter

Getter 会暴露为 `store.getters` 对象，你可以以属性的形式访问这些值：

```js
store.getters.bigCount
```

### Getter的其他参数

Getter 也可以接受其他 getter 作为第二个参数：

```js
getters: {
  // ...
  doubleBaseCount: (state, getters) => {
    return getters.baseCount * 2
  }
}
```

你也可以通过让 getter 返回一个函数，来实现给 getter 传参。在你对 store 里的数组进行查询时非常有用。

```js
getters: {
  // ...
  getTodoById: (state) => (id) => {
    return state.todos.find(todo => todo.id === id)
  }
}
```

```js
store.getters.getTodoById(2) // -> { id: 2, text: '...', done: false }
```

注意，getter 在通过方法访问时，每次都会去进行调用，而不会缓存结果。

## Mutations

要记住：**更改 Vuex 的 store 中的状态的唯一方法是提交 mutation**。

### 注册mutation

Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    INCREASE_COUNT (state) {
      // 变更状态
      state.count++
    }
  }
})
```

### 提交mutation

你不能直接调用一个 mutation handler。这个选项更像是事件注册：“当触发一个类型为 `INCREASE_COUNT` 的 mutation 时，调用此函数。”要唤醒一个 mutation handler，你需要以相应的 type 调用 **store.commit** 方法：

```js
store.commit('INCREASE_COUNT')
```

### 提交载荷（Payload）

你可以向 `store.commit` 传入额外的参数，即 mutation 的 **载荷（payload）**：

```js
// ...
mutations: {
  INCREASE_COUNT (state, n) {
    state.count += n
  }
}
store.commit('INCREASE_COUNT', 10)
```

在大多数情况下，载荷应该是一个对象，这样可以包含多个字段并且记录的 mutation 会更易读：

```js
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
store.commit('INCREASE_COUNT', {
  amount: 10
})
```

### 对象风格的提交方式

提交 mutation 的另一种方式是直接使用包含 `type` 属性的对象：

```js
store.commit({
  type: 'INCREASE_COUNT',
  amount: 10
})
```

当使用对象风格的提交方式，整个对象都作为载荷传给 mutation 函数，因此 handler 保持不变：

```js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

### Mutation 需遵守 Vue 的响应规则

既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。这也意味着 Vuex 中的 mutation 也需要与使用 Vue 一样遵守一些注意事项：

1. 最好提前在你的 store 中初始化好所有所需属性。
2. 当需要在对象上添加新属性时，你应该

- 使用 `Vue.set(obj, 'newProp', 123)`, 或者

- 以新对象替换老对象。例如，利用对象展开运算符 我们可以这样写：

  ```js
  state.obj = { ...state.obj, newProp: 123 }
  ```

### 使用常量替代 Mutation 事件类型

使用常量替代 mutation 事件类型在各种 Flux 实现中是很常见的模式。这样可以使 linter 之类的工具发挥作用，同时把这些常量放在单独的文件中可以让你的代码合作者对整个 app 包含的 mutation 一目了然：

```js
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```

用不用常量取决于你——在需要多人协作的大型项目中，这会很有帮助。但如果你不喜欢，你完全可以不这样做。

### Mutation 必须是同步函数

一条重要的原则就是要记住 **mutation 必须是同步函数**。为什么？请参考下面的例子：

```js
mutations: {
  someMutation (state) {
    api.callAsyncMethod(() => {
      state.count++
    })
  }
}
```

现在想象，我们正在 debug 一个 app 并且观察 devtool 中的 mutation 日志。每一条 mutation 被记录，devtools 都需要捕捉到前一状态和后一状态的快照。然而，在上面的例子中 mutation 中的异步函数中的回调让这不可能完成：因为当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用——实质上任何在回调函数中进行的状态的改变都是不可追踪的。

## Actions

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

### 注册action

让我们来注册一个简单的 action：

```js
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    INCREASE_COUNT (state) {
      state.count++
    }
  },
  actions: {
    setCount (context) {
      context.commit('INCREASE_COUNT')
    }
  }
})
```

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个 mutation，或者通过 `context.state` 和 `context.getters` 来获取 state 和 getters。

实践中，我们会经常用到 ES2015 的 参数解构 来简化代码（特别是我们需要调用 `commit` 很多次的时候）：

```js
actions: {
  setCount ({ commit }) {
    commit('INCREASE_COUNT')
  }
}
```

### 分发 Action

Action 通过 `store.dispatch` 方法触发：

```js
store.dispatch('INCREASE_COUNT')
```

乍一眼看上去感觉多此一举，我们直接分发 mutation 岂不更方便？实际上并非如此，还记得 **mutation 必须同步执行**这个限制么？Action 就不受约束！我们可以在 action 内部执行**异步**操作：

```js
actions: {
  setCountWaitOneSecond ({ commit }) {
    setTimeout(() => {
      commit('INCREASE_COUNT')
    }, 1000)
  }
}
```

Actions 支持同样的载荷方式和对象方式进行分发：

```js
// 以载荷形式分发
store.dispatch('setCount', {
  amount: 10
})

// 以对象形式分发
store.dispatch({
  type: 'setCount',
  amount: 10
})
```

来看一个更加实际的购物车示例，涉及到**调用异步 API** 和**分发多重 mutation**：

```js
actions: {
  checkout ({ commit, state }, products) {
    // 把当前购物车的物品备份起来
    const savedCartItems = [...state.cart.added]
    // 发出结账请求，然后乐观地清空购物车
    commit(types.CHECKOUT_REQUEST)
    // 购物 API 接受一个成功回调和一个失败回调
    shop.buyProducts(
      products,
      // 成功操作
      () => commit(types.CHECKOUT_SUCCESS),
      // 失败操作
      () => commit(types.CHECKOUT_FAILURE, savedCartItems)
    )
  }
}
```

注意我们正在进行一系列的异步操作，并且通过提交 mutation 来记录 action 产生的副作用（即状态变更）。

### 组合 Action

Action 通常是异步的，那么如何知道 action 什么时候结束呢？更重要的是，我们如何才能组合多个 action，以处理更加复杂的异步流程？

首先，你需要明白 `store.dispatch` 可以处理被触发的 action 的处理函数返回的 Promise，并且 `store.dispatch` 仍旧返回 Promise：

```js
actions: {
  actionA ({ commit }) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit('someMutation')
        resolve()
      }, 1000)
    })
  }
}
```

现在你可以：

```js
store.dispatch('actionA').then(() => {
  // ...
})
```

在另外一个 action 中也可以：

```js
actions: {
  // ...
  actionB ({ dispatch, commit }) {
    return dispatch('actionA').then(() => {
      commit('someOtherMutation')
    })
  }
}
```

最后，如果我们利用 `async / await`，我们可以如下组合 action：

```js
// 假设 getData() 和 getOtherData() 返回的是 Promise

actions: {
  async actionA ({ commit }) {
    commit('gotData', await getData())
  },
  async actionB ({ dispatch, commit }) {
    await dispatch('actionA') // 等待 actionA 完成
    commit('gotOtherData', await getOtherData())
  }
}
```

> 一个 `store.dispatch` 在不同模块中可以触发多个 action 函数。在这种情况下，只有当所有触发函数完成后，返回的 Promise 才会执行。

# 把store的数据变为组件的数据 

## 访问state

访问state，一般我们把state中的状态作为组件的计算属性来属性来使用。

**方式一**：使用 `this.$store.state` 获取状态 ：

由于 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在计算属性中返回某个状态，每当 `store.state.count` 变化的时候，都会重新求取计算属性，并且触发更新相关联的 DOM。

```js
computed:{
  count(){
    return this.$store.state.count;
  }
}
```

**方式二**：使用 mapState 辅助函数：

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性，让你少按几次键。

`mapState` 函数返回的是一个对象。可以直接使用对象展开运算符，将`mapState` 函数返回的对象与 `computed` 合并：

```js
import {mapState} from 'vuex

computed: {
  // 使用对象展开运算符将此对象混入到外部对象中
  // 映射 this.count 为 store.state.count
  
  // 借助mapState生成计算属性：count、message（对象写法）--- 对象写法可以对属性进行重命名
  ...mapState({count:'count', message:'message'}),
    
  // 借助mapState生成计算属性：count、message（数组写法）--- 需要和state中的属性名保持一致
  ...mapState(['count', 'message']),
}
```

## 访问getters

访问getters，一般我们把getters中的数据作为组件的计算属性来属性

**方式一**：使用 `this.$store.getters` 获取状态 

```js
computed:{
  count(){
    return this.$store.getters.bigCount;
  }
}
```

**方式二**：使用 mapGetters 辅助函数

```js
import {mapGetters} from 'vuex

computed: {
    // 借助mapGetters生成计算属性：bigCount（对象写法）
  ...mapGetters({bigCount:'bigCount'}]
                 
  // 借助mapGetters生成计算属性：bigCount（数组写法）
  ...mapGetters(['bigCount']),
}
```

## 提交mutation

**方式一**：直接使用 `this.$store.commit()` 提交

```js
this.$store.commit('INCREASE_COUNT', 1);
```

**方式二**：使用 `mapMutations` 辅助函数将组件中的 methods 映射为 `store.commit` 调用：

```js
import { mapMutations } from 'vuex';

methods:{
  increase1() {
      // this.$store.commit('INCREASE_COUNT', 1);
      this.INCREASE_COUNT(1);
		},
  
  // 借助mapMutation生成函数：setCount（对象写法）
  ...mapMutations({INCREASE_COUNT:'INCREASE_COUNT'}),
  
  // 借助mapMutation生成函数：SET_COUNT（数组写法）
  ...mapMutations(['INCREASE_COUNT']),//把SET_COUNT映射为组件自己的函数
}
```

## 分发action

**方式一**：直接使用 `this.$store.dispatch()` 分发：

```js
this.$store.dispatch('setCount', { type: 0, value: 2 });
```

**方式二**：使用 `mapActions` 辅助函数将组件的 methods 映射为 `store.dispatch` 调用：

```js
import { mapActions } from 'vuex';

methods:{
  increase() {
    // this.$store.dispatch('setCount', { type: 0, value: 2 });
    this.setCount({ type: 0, value: 2 });
  },
      
  // 借助mapActions生成函数：setCount（对象写法）
  ...mapActions({setCount:'setCount'}),
  
  // 借助mapActions生成函数：setCount（数组写法）
  ...mapActions(['setCount']),
}
```

# Vuex模块化

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割。

```js
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

## 块的局部状态

- 对于模块内部的 mutation 和 getter，接收的第一个参数是**模块的局部状态对象**。

- 对于模块内部的 action，局部状态通过 `context.state` 暴露出来，根节点状态则为 `context.rootState`

- 对于模块内部的 getter，根节点状态会作为第三个参数暴露出来： `rootState`, `rootGetters`

默认情况下，模块内部的 action、mutation 和 getter 是注册在全局命名空间的——这样使得多个模块能够对同一 mutation 或 action 作出响应。

```js
const moduleA = {
  // state 是模块内部私有的
	state() {
		return {
			count: 1,
		};
	},
  // getter 是注册在全局的
	getters: {
    // state 模块局部的状态对象
    // rootState 根节点的状态对象
		bigCount(state, getters, rootState) {
		},
	},
  // mutation 是注册在全局的
	mutations: {
    // state 模块局部的状态对象
		INCREASE_COUNT(state, value) {
			state.count += value;
		},
	},
  // action 是注册在全局的
	actions: {
    // state 模块局部的状态对象
    // rootState 根节点的状态对象
		setCount({ commit, dispatch, state, getters, rootState }, data) {
		},
	},
};
```

## 模块动态注册

在 store 创建**之后**，你可以使用 `store.registerModule` 方法注册模块：

```
import { createStore } from 'vuex'

const store = createStore({ /* 选项 */ })

// 注册模块 `myModule`
store.registerModule('myModule', {
  // ...
})

// 注册嵌套模块 `nested/myModule`
store.registerModule(['nested', 'myModule'], {
  // ...
})
```

之后就可以通过 `store.state.myModule` 和 `store.state.nested.myModule` 访问模块的状态。

模块动态注册功能使得其他 Vue 插件可以通过在 store 中附加新模块的方式来使用 Vuex 管理状态。例如，[`vuex-router-sync`](https://github.com/vuejs/vuex-router-sync) 插件就是通过动态注册模块将 Vue Router 和 Vuex 结合在一起，实现应用的路由状态管理。

你也可以使用 `store.unregisterModule(moduleName)` 来动态卸载模块。注意，你不能使用此方法卸载静态模块（即创建 store 时声明的模块）。

注意，你可以通过 `store.hasModule(moduleName)` 方法检查该模块是否已经被注册到 store。需要记住的是，嵌套模块应该以数组形式传递给 `registerModule` 和 `hasModule`，而不是以路径字符串的形式传递给 module。

## 保留 state

在注册一个新 module 时，你很有可能想保留过去的 state，例如从一个服务端渲染的应用保留 state。你可以通过 `preserveState` 选项将其归档：`store.registerModule('a', module, { preserveState: true })`。

当你设置 `preserveState: true` 时，该模块会被注册，action、mutation 和 getter 会被添加到 store 中，但是 state 不会。这里假设 store 的 state 已经包含了这个 module 的 state 并且你不希望将其覆写。

## 模块数据的访问和修改

在`src/store`目录中创建 `modules` 目录，在 `modules` 目录中创建 `counter.js` 模块和 `user.js` 模块：

```js
// src/store/modules/counter.js

// 1、state: 是局部状态，获取局部状态的属性，使用 store.state.counter.count
// 2、getters/mutations/actions 也是注册在全局的，可以直接访问
const counter = {
  state: () => ({ 
  	count: 1,
  }),
 	getters: {
		bigCount(state, getters, rootState) {
			return state.count * 2;
		},
		baseCount(state, getters, rootState) {
			return state.count * rootState.base;
		},
	},
	mutations: {
		INCREASE_COUNT(state, value) {
			state.count += value;
		},
		DECREASE_COUNT(state, value) {
			state.count -= value;
		},
	},
	actions: {
		setCount({ commit, dispatch, state, getters, rootState }, data) {
			if (data.type == 0) {
				commit('INCREASE_COUNT', data.value);
			} else {
				commit('DECREASE_COUNT', data.value);
			}
		},
		setCountWaitOneSecond({ dispatch }, data) {
			if (timer) return;
			timer = setTimeout(() => {
				dispatch('setCount', data);
				timer = null;
			}, 1000);
		},
		setCountWidthBase({ dispatch, state, rootState }, data) {
			dispatch('setCount', { type: data.type, value: rootState.base });
		},
	},
}
```

```js
// src/store/modules/user.js

const user = {
  state: () => ({
  	token: '',
  }),
	getters: {
		token: state => state.token,
	},
	mutations: {
		SET_TOKEN(state, value) {
			state.token = value;
		},
	},
	actions: {
		login({ commit }) {
			commit('SET_TOKEN', 'abc');
		},
		logout({ commit }) {
			commit('SET_TOKEN', '');
		},
	},
}
```

```js
// src/store/index.js

import counter from '@/store/modules/counter';
import user from '@/store/modules/user';

// 在初始仓库中
// 1、state: 是全局状态，获取全局状态的属性使用 store.state.base
// 2、getters/mutations/actions 是注册在全局的，可以直接访问 
const store = new Vuex.Store({
  strict: process.env.NODE_ENV !== 'production',
  state: {
    base: 10,
	},
	getters: {},
	mutations: {
		SET_BASE(state, value) {
			state.base = value;
		},
	},
	actions: {},
  modules: {
    counter,
    user,
  }
})

store.counter.count // -> counter 的状态
store.user.token // -> user 的状态
```

### 访问state

**模块中的state是注册在局部（模块内）的，不能直接访问。**

**方式一**：使用 `this.$store.state` 获取 store 实例的状态 

```js
$store.state.base // 访问根节点的状态

// 访问模块的状态不能直接获取，需要加模块名读取
$store.state.counter.count // 访问counter模块的状态
$store.state.user.token // 访问user模块的状态
```

**方式二**：使用 `mapState` 辅助函数，获取 store 实例的状态

如果使用了模块化，不能使用 mapState 直接映射counter和user模块中的state，因为模块中的state是局部的。使用mapState 映射模块名，通过模块名获取state中的属性。

```js
// <p>message = {{ message }}</p>
// <p>base = {{ base }}</p>
// <p>counter.count = {{ counter.count }}</p>
// <p>user.token = {{ user.token }}</p>

import {mapState} from 'vuex

computed: {
    ...mapState(['message', 'base', 'counter', 'user']),
}
```

### 访问getters

**模块中的getters是注册在全局的，可以直接访问。**

**方式一**：使用 `this.$store.getters` 获取 store 实例的 getters

```js
this.$store.getters.base

this.$store.getters.bigCount

this.$store.getters.token
```

**方式二**：使用 `mapGetters` 辅助函数可以获取 store 实例的 getters

```js
import {mapGetters} from 'vuex

computed: {
  ...mapGetters(['bigCount', 'baseCount', 'token']),
}
```

### 提交mutation

**模块中的mutation是注册在全局的，可以直接访问。**

**方式一**：直接使用 `this.$store.commit()` 提交

```js
this.$store.commit('INCREASE_COUNT', 1);
```

**方式二**：使用 `mapMutations` 辅助函数 

```js
import { mapMutations } from 'vuex';

methods:{
  increase() {
    this.INCREASE_COUNT(1);
  },
  
  ...mapMutations(['INCREASE_COUNT']),
}
```

### 分发action

**模块中的action是注册在全局的，可以直接访问。**

**方式一**：直接使用 `this.$store.dispatch()`分发

```js
this.$store.dispatch('setCount', { type: 0, value: 2 });
```

**方式二**：使用 `mapActions` 辅助函数 

```js
import { mapActions } from 'vuex';

methods:{
  increase() {
    this.setCount({ type: 0, value: 2 });
  },
      
  ...mapActions(['setCount']),
}
```

# 带命名空间的模块化

默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的——这样使得多个模块能够对同一 mutation 或 action 作出响应。

如果希望你的模块具有更高的封装度和复用性，你可以通过添加 `namespaced: true` 的方式使其成为带命名空间的模块。当模块被注册后，它的所有 getter、action 及 mutation 都会自动根据模块注册的路径调整命名。

```js
const store = new Vuex.Store({
  modules: {
    account: {
      namespaced: true,

      // 模块内容（module assets）
      state: () => ({ ... }), // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
      getters: {
        isAdmin () { ... } // -> getters['account/isAdmin']
      },
      actions: {
        login () { ... } // -> dispatch('account/login')
      },
      mutations: {
        login () { ... } // -> commit('account/login')
      },

      // 嵌套模块
      modules: {
        // 继承父模块的命名空间
        myPage: {
          state: () => ({ ... }),
          getters: {
            profile () { ... } // -> getters['account/profile']
          }
        },

        // 进一步嵌套命名空间
        posts: {
          namespaced: true,

          state: () => ({ ... }),
          getters: {
            popular () { ... } // -> getters['account/posts/popular']
          }
        }
      }
    }
  }
})
```

启用了命名空间的 getter 和 action 会收到局部化的 `getter`，`dispatch` 和 `commit`。换言之，你在使用模块内容时不需要在同一模块内额外添加空间名前缀。更改 `namespaced` 属性后不需要修改模块内的代码。

## 在带命名空间的模块内访问全局内容（Global Assets）

如果你希望使用全局 state 和 getter，`rootState` 和 `rootGetters` 会作为第三和第四参数传入 getter，也会通过 `context` 对象的属性传入 action。

若需要在全局命名空间内分发 action 或提交 mutation，将 `{ root: true }` 作为第三参数传给 `dispatch` 或 `commit` 即可。

```js
modules: {
  foo: {
    namespaced: true,

    getters: {
      // 在这个模块的 getter 中，`getters` 被局部化了
      // 你可以使用 getter 的第四个参数来调用 `rootGetters`
      someGetter (state, getters, rootState, rootGetters) {
        getters.someOtherGetter // -> 'foo/someOtherGetter'
        rootGetters.someOtherGetter // -> 'someOtherGetter'
        rootGetters['bar/someOtherGetter'] // -> 'bar/someOtherGetter'
      },
      someOtherGetter: state => { ... }
    },

    actions: {
      // 在这个模块中， dispatch 和 commit 也被局部化了
      // 他们可以接受 `root` 属性以访问根 dispatch 或 commit
      someAction ({ dispatch, commit, getters, rootGetters }) {
        getters.someGetter // -> 'foo/someGetter'
        rootGetters.someGetter // -> 'someGetter'
        rootGetters['bar/someGetter'] // -> 'bar/someGetter'

        dispatch('someOtherAction') // -> 'foo/someOtherAction'
        dispatch('someOtherAction', null, { root: true }) // -> 'someOtherAction'

        commit('someMutation') // -> 'foo/someMutation'
        commit('someMutation', null, { root: true }) // -> 'someMutation'
      },
      someOtherAction (ctx, payload) { ... }
    }
  }
}
```

## 在带命名空间的模块注册全局 action

若需要在带命名空间的模块注册全局 action，你可添加 `root: true`，并将这个 action 的定义放在函数 `handler` 中。例如：

```js
{
  actions: {
    someOtherAction ({dispatch}) {
      dispatch('someAction')
    }
  },
  modules: {
    foo: {
      namespaced: true,

      actions: {
        someAction: {
          root: true,
          handler (namespacedContext, payload) { ... } // -> 'someAction'
        }
      }
    }
  }
}
```

## 带命名空间的绑定函数

当使用 `mapState`, `mapGetters`, `mapActions` 和 `mapMutations` 这些函数来绑定带命名空间的模块时，写起来可能比较繁琐：

```js
computed: {
  ...mapState({
    a: state => state.some.nested.module.a,
    b: state => state.some.nested.module.b
  }),
  ...mapGetters([
    'some/nested/module/someGetter', // -> this['some/nested/module/someGetter']
    'some/nested/module/someOtherGetter', // -> this['some/nested/module/someOtherGetter']
  ])
},
methods: {
  ...mapActions([
    'some/nested/module/foo', // -> this['some/nested/module/foo']()
    'some/nested/module/bar' // -> this['some/nested/module/bar']()
  ])
}
```

对于这种情况，你可以将模块的空间名称字符串作为第一个参数传递给上述函数，这样所有绑定都会自动将该模块作为上下文。于是上面的例子可以简化为：

```js
computed: {
  ...mapState('some/nested/module', {
    a: state => state.a,
    b: state => state.b
  }),
  ...mapGetters('some/nested/module', [
    'someGetter', // -> this.someGetter
    'someOtherGetter', // -> this.someOtherGetter
  ])
},
methods: {
  ...mapActions('some/nested/module', [
    'foo', // -> this.foo()
    'bar' // -> this.bar()
  ])
}
```

而且，你可以通过使用 `createNamespacedHelpers` 创建基于某个命名空间辅助函数。它返回一个对象，对象里有新的绑定在给定命名空间值上的组件绑定辅助函数：

```js
import { createNamespacedHelpers } from 'vuex'

const { mapState, mapActions } = createNamespacedHelpers('some/nested/module')

export default {
  computed: {
    // 在 `some/nested/module` 中查找
    ...mapState({
      a: state => state.a,
      b: state => state.b
    })
  },
  methods: {
    // 在 `some/nested/module` 中查找
    ...mapActions([
      'foo',
      'bar'
    ])
  }
}
```

## 给插件开发者的注意事项

如果你开发的[插件（Plugin）](https://vuex.vuejs.org/zh/guide/plugins.html)提供了模块并允许用户将其添加到 Vuex store，可能需要考虑模块的空间名称问题。对于这种情况，你可以通过插件的参数对象来允许用户指定空间名称：

```js
// 通过插件的参数对象得到空间名称
// 然后返回 Vuex 插件函数
export function createPlugin (options = {}) {
  return function (store) {
    // 把空间名字添加到插件模块的类型（type）中去
    const namespace = options.namespace || ''
    store.dispatch(namespace + 'pluginAction')
  }
}
```

## 命名空间模块数据的访问和修改

```js
// 在counter模块中：
const counter = {
  namespaced: true,
  
  // 模块内的状态已经是嵌套的了，使用 `namespaced` 属性不会对其产生影响
  // state 是模块内部私有的
  state: () => ({  
  	count: 1,
  }),
  
 	getters: {
    // state、getters是counter模块的局部state和getters
    // rootState、rootGetters是根节点的state和getters
    
    // 访问getter：getters['counter/bigCount']
		bigCount(state, getters, rootState, rootGetters) { 
			return state.count * 2; 
		},
		baseCount(state, getters, rootState, rootGetters) {
			return state.count * rootState.base;
		},
	},
	mutations: {
    // 提交mutation：commit('counter/INCREASE_COUNT')
		INCREASE_COUNT(state, value) {
			state.count += value;  
		},
		DECREASE_COUNT(state, value) {
			state.count -= value;
		},
	},
	actions: {
    // 分发action：dispatch('counter/setCount', {type:0, data:1})
		setCount({ commit, dispatch, state, getters, rootState, rootGetters }, data) {
			if (data.type == 0) {    
				commit('INCREASE_COUNT', data.value);
			} else {
				commit('DECREASE_COUNT', data.value);
			}
		},
		setCountWaitOneSecond({ dispatch }, data) {
			if (timer) return;
			timer = setTimeout(() => {
				dispatch('setCount', data);
				timer = null;
			}, 1000);
		},
		setCountWidthBase({ dispatch, state, rootState }, data) {
			dispatch('setCount', { type: data.type, value: rootState.base });
		},
	},
}
```

```js
const user = {
  namespaced: true,
  state: () => ({
  	token: localStorage.getItem('token') ? localStorage.getItem('token') : '',
  }),
	getters: {
		token: state => state.token,
	},
	mutations: {
		SET_TOKEN(state, value) {
			state.token = value;
			state.token ? localStorage.setItem('token', state.token) : localStorage.removeItem('token');
		},
	},
	actions: {
		login({ commit }) {
			commit('SET_TOKEN', 'abc');
		},
		logout({ commit }) {
			commit('SET_TOKEN', '');
		},
	},
}


```

```js
import counter from '@/store/modules/counter';
import user from '@/store/modules/user';

const store = new Vuex.Store({
  strict: process.env.NODE_ENV !== 'production',
  state: {
    base: 10,
	},
	getters: {},
	mutations: {
		SET_BASE(state, value) {
			state.base = value;
		},
	},
	actions: {},
  modules: {
    counter,
    user,
  }
})

store.counter.count // -> counter 的状态
store.user.token // -> user 的状态
```

### 访问state

**模块中的state已经是注册在局部（模块内）的，使用命名空间对state没有产生影响。**

**方式一**：使用 `this.$store.state` 获取 store 实例的状态 

```js
$store.state.base // 访问根节点的状态

// 访问模块的状态不能直接获取，需要加模块名读取
$store.state.counter.count // 访问counter模块的状态
$store.state.user.token // 访问user模块的状态
```

**方式二**：使用 `mapState` 辅助函数，获取 store 实例的状态

```js
// <p>message = {{ message }}</p>
// <p>base = {{ base }}</p>
// <p>count = {{ count }}</p>
// <p>token = {{ token }}</p>

computed: {
  ...mapState(['message', 'base']),
  ...mapState('counter',['count']),
  ...mapState('user',['token']),
}
```

### 访问getters

**命名空间模块中的getters是注册在局部的，getters会根据模块注册的路径调整。**

**方式一**：使用 `this.$store.getters` 获取 store 实例的 getters

```js
this.$store.getters.bigBase

this.$store.getters['counter/bigCount']

this.$store.getters['user/token']
```

**方式二**：使用 `mapGetters` 辅助函数可以获取 store 实例的 getters

```js
computed: {
  ...mapGetters(['bigBase']),
  ...mapGetters('counter',['bigCount', 'baseCount']),
}
```

### 提交mutation

**命名空间模块中的mutation是注册在局部的，mutation会根据模块注册的路径调整。**

**方式一**：直接使用 `this.$store.commit()` 提交

```js
this.$store.commit('counter/INCREASE_COUNT', 1);
```

**方式二**：使用 `mapMutations` 辅助函数 

```js
methods:{
  increase() {
    this.INCREASE_COUNT(1);
  },
  
  ...mapMutations('counter',['INCREASE_COUNT']),
}
```

### 分发action

**命名空间模块中的action是注册在局部的，action会根据模块注册的路径调整。**

**方式一**：直接使用 `this.$store.dispatch()`

```js
this.$store.dispatch('counter/setCount', { type: 0, value: 2 });
```

**方式二**：使用 `mapActions` 辅助函数 

```js
methods:{
  increase() {
    this.setCount({ type: 0, value: 2 });
  },
      
  ...mapActions('counter', ['setCount']),
}
```

# Vuex进阶

##  项目结构

Vuex 并不限制你的代码结构。但是，它规定了一些需要遵守的规则：

1. 应用层级的状态应该集中到单个 store 对象中。
2. 提交 **mutation** 是更改状态的唯一方法，并且这个过程是同步的。
3. 异步逻辑都应该封装到 **action** 里面。

只要你遵守以上规则，如何组织代码随你便。如果你的 store 文件太大，只需将 action、mutation 和 getter 分割到单独的文件。

对于大型应用，我们会希望把 Vuex 相关代码分割到模块中。下面是项目结构示例：

```bash
├── index.html
├── main.js
├── api
│   └── ... # 抽取出API请求
├── components
│   ├── App.vue
│   └── ...
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    ├── actions.js        # 根级别的 action
    ├── mutations.js      # 根级别的 mutation
    └── modules
        ├── cart.js       # 购物车模块
        └── products.js   # 产品模块
```

请参考[购物车示例 ](https://github.com/vuejs/vuex/tree/dev/examples/shopping-cart)。

## 插件

Vuex 的 store 接受 `plugins` 选项，这个选项暴露出每次 mutation 的钩子。Vuex 插件就是一个函数，它接收 store 作为唯一参数：

```js
const myPlugin = store => {
  // 当 store 初始化后调用
  store.subscribe((mutation, state) => {
    // 每次 mutation 之后调用
    // mutation 的格式为 { type, payload }
  })
}
```

然后像这样使用：

```js
const store = new Vuex.Store({
  // ...
  plugins: [myPlugin]
})
```

### 内置 Logger 插件

> 如果正在使用 [vue-devtools](https://github.com/vuejs/vue-devtools)，你可能不需要此插件。

Vuex 自带一个日志插件用于一般的调试:

```js
import createLogger from 'vuex/dist/logger'

const store = new Vuex.Store({
  plugins: [createLogger()]
})
```

## 严格模式

开启严格模式，仅需在创建 store 的时候传入 `strict: true`：

```js
const store = new Vuex.Store({
  // ...
  strict: true
})
```

在严格模式下，无论何时发生了状态变更且不是由 mutation 函数引起的，将会抛出错误。这能保证所有的状态变更都能被调试工具跟踪到。

### 开发环境与发布环境

**不要在发布环境下启用严格模式**！严格模式会深度监测状态树来检测不合规的状态变更——请确保在发布环境下关闭严格模式，以避免性能损失。

类似于插件，我们可以让构建工具来处理这种情况：

```js
const store = new Vuex.Store({
  // ...
  strict: process.env.NODE_ENV !== 'production'
})
```

## 表单处理

当在严格模式中使用 Vuex 时，在属于 Vuex 的 state 上使用 `v-model` 会比较棘手：

```html
<input v-model="obj.message">
```

假设这里的 `obj` 是在计算属性中返回的一个属于 Vuex store 的对象，在用户输入时，`v-model` 会试图直接修改 `obj.message`。在严格模式中，由于这个修改不是在 mutation 函数中执行的, 这里会抛出一个错误。

用“Vuex 的思维”去解决这个问题的方法是：给 `<input>` 中绑定 value，然后侦听 `input` 或者 `change` 事件，在事件回调中调用一个方法:

```html
<input :value="message" @input="updateMessage">
```

```js
// ...
computed: {
  ...mapState({
    message: state => state.obj.message
  })
},
methods: {
  updateMessage (e) {
    this.$store.commit('updateMessage', e.target.value)
  }
}
```

下面是 mutation 函数：

```js
// ...
mutations: {
  updateMessage (state, message) {
    state.obj.message = message
  }
}
```

### 双向绑定的计算属性

必须承认，这样做比简单地使用“`v-model` + 局部状态”要啰嗦得多，并且也损失了一些 `v-model` 中很有用的特性。另一个方法是使用带有 setter 的双向绑定计算属性：

```html
<input v-model="message">
```

```js
// ...
computed: {
  message: {
    get () {
      return this.$store.state.obj.message
    },
    set (value) {
      this.$store.commit('updateMessage', value)
    } 
  }
}
```

# Vuex数据持久化存储

Vuex**本质**：一个保存在**内存中的对象**，可以理解成一个**全局变量**，全局变量也可能产生**内存泄漏**。

存在的**问题**：当页面刷新后该对象就会被重新初始化，之前存的数据就拿不到了，于是在使用这些数据的地方就可能发生报错;

**解决方法**：把state中的数据做一个持久化存储或者说备份，一般就存在localStorage、sessionStorage或者cookies中。

以localStorage为例，存储在localStorage，数据不会因为页面或浏览器的关闭而丢失，只有手动清除；

- 在Vuex初始化的时候就尝试去localStorage里面读取之前的数据，再存回state中；
- 这样当页面刷新或关闭后再打开时，state中还是有之前的数据;

存sessionStorage的话，数据会在页面关闭后被清除。这些方式都可以自己手动实现，但使用一些第三方插件实现和管理起来会更方便，如 vuex-persistedstate、 vuex-persist。

**注意**：

不管是localStorage还是sessionStorage相对Vuex来说安全性都要差一些，因为前两者都是可以直接在浏览器控制台进行查看，修改和删除的，也可以借助一个第三方加密插件来优化一下；由于localStorage是不会过期的，我们也可以给他设置一个过期时间

## 方案一：手动实现持久化

使用sessionStorage或者localStorage或者cookies存储。

以localStorage为例封装存储函数，也可以直接使用localStorage，在src目录下创建 `utils/storage.js` ：

```js
/**
 * 存储localStorage
 */
export const setStore = (name, content) => {
  if (!name) return
  if (typeof content !== 'string') {
    content = JSON.stringify(content)
  }
  window.localStorage.setItem(name, content)
}

/**
 * 获取localStorage
 */
export const getStore = name => {
  if (!name) return
  return window.localStorage.getItem(name)
}

/**
 * 获取localStorage JSON对象
 */
export const getStoreJSON = name => {
  if (!name) return
  const v = window.localStorage.getItem(name)
  if (!v) return
  return JSON.parse(v)
}

/**
 * 删除localStorage
 */
export const removeStore = name => {
  if (!name) return
  window.localStorage.removeItem(name)
}
```

然后在vuex中的使用： 

```js
import { setStore, getStore } from '@/utils/storage.js'

new Vuex.Store({
  state: {
    count: getStore('count') || 0
  },
  mutations: {
    UPDATE_COUNT(state, count) {
      state.count++
      setStore('count', state.count)
    }
  }
});
```

每当我们去修改状态的时候，都会往 storage中存储一下，浏览器刷新的时候，就会先从 storage中取，达到持久化存储的效果。

## 方案二：使用第三方插件 

### [vuex-persistedstate](https://github.com/robinvdvleuten/vuex-persistedstate)

```js
import Vue from "vue";
import Vuex from "vuex";
import user from './modules/user'

// 安装插件
// npm install vuex-persistedstate
// 引入插件
import createPersistedState from "vuex-persistedstate";

Vue.use(Vuex);

export default new Vuex.Store({
  state:{},
  gettters:{},
  mutations:{},
  actions:{},
  modules:{
    user
  }
  /* vuex数据持久化配置 */
  plugins: [
    createPersistedState({
      // storage:存储位置，localStorage或sessionStorage或cookie
      // cookie 存储方式有区别，下面单独讲
      // 默认存储在localStorage中
      storage: window.sessionStorage,
      // 存储的 key 值，默认是vuex
      key: "store",
      // 要存储的数据，render函数的参数是state对象
      reducer(state) { 
        // 要存储的数据：本项目采用es6扩展运算符的方式存储了state中所有的数据
        return { ...state };
      }
    })
  ]
})
```

上面是将所有的store中的state状态都持久化存储了，如果是想只持久化某一个模块的数据，则将上面plugins修改为下面写法 

```js
/* vuex数据持久化配置 */
plugins: [
  createPersistedState({
    storage: window.sessionStorage,
    key: "store",
    render(state) {
      // 也可以只存需要的数据，以key:value的形式
      return { userName: state.userName }
    },
    // 如果是模块化的vuex，只持久化存储user模块的状态
    paths: ['user']
  })
]
```

使用cookie存储：

```js
// 存cookie的话可以再引入两个cookie插件，方便对cookie进行操作 
// npm install --save cookie js-cookie 

import * as Cookies from 'js-cookie'; 
import cookie from 'cookie'; 

export default new Vuex.Store({
  plugins: [    
    persistedState({      
      storage: {        
        getItem: key => Cookies.get(key),        
        setItem: (key, value) => Cookies.set(key, value, { expires: 7}),        
        removeItem: key => Cookies.remove(key)     
      }   
    }) 
  ]
})
```

### [vuex-persist](https://github.com/championswimmer/vuex-persist)

vuex-persist不需要手动存取 storage，而是直接将状态保存至 localStorage 或者cookie中。

```js
// 安装 
// npm install --save vuex-persist 

// 使用
import VuexPersistence from 'vuex-persist'

// 实例化插件，配置和第一个插件差不多
const vuexLocal = new VuexPersistence({
	storage: window.localStorage,
  render(state) {
    return { ...state }
    // 我这里直接把state中的全部数据解构存进去，
    // 也可以只存需要的数据，以key:value的形式
    // return {userName:state.userName}
  }
})

const store = new Vuex.Store({
 state:{},
  gettters:{},
  mutations:{},
  actions:{},
   // 传入配置后的插件实例
  plugins:[vuexLocal.plugin]
})
```

