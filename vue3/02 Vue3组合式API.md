# 组合式API介绍

## 什么是组合式 API？

组合式 API (Composition API) 是一系列 API 的集合，使我们可以使用函数而不是声明选项的方式书写 Vue 组件。它是一个概括性的术语，涵盖了以下方面的 API：

- [响应式 API](https://cn.vuejs.org/api/reactivity-core.html)：例如 `ref()` 和 `reactive()`，使我们可以直接创建响应式状态、计算属性和侦听器。
  - [生命周期钩子](https://cn.vuejs.org/api/composition-api-lifecycle.html)：例如 `onMounted()` 和 `onUnmounted()`，使我们可以在组件各个生命周期阶段添加逻辑。

- [依赖注入](https://cn.vuejs.org/api/composition-api-dependency-injection.html)：例如 `provide()` 和 `inject()`，使我们可以在使用响应式 API 时，利用 Vue 的依赖注入系统。

组合式 API 是 Vue 3 及 [Vue 2.7](https://blog.vuejs.org/posts/vue-2-7-naruto.html) 的内置功能。对于更老的 Vue 2 版本，可以使用官方维护的插件 [`@vue/composition-api`](https://github.com/vuejs/composition-api)。在 Vue 3 中，组合式 API 基本上都会配合 [`<script setup>`](https://cn.vuejs.org/api/sfc-script-setup.html) 语法在单文件组件中使用。下面是一个使用组合式 API 的组件示例：

```vue
<script setup>
import { ref, onMounted } from 'vue'

// 响应式状态
const count = ref(0)

// 更改状态、触发更新的函数
function increment() {
  count.value++
}

// 生命周期钩子
onMounted(() => {
  console.log(`计数器初始值为 ${count.value}。`)
})
</script>

<template>
  <button @click="increment">点击了：{{ count }} 次</button>
</template>
```

虽然这套 API 的风格是基于函数的组合，但**组合式 API 并不是函数式编程**。组合式 API 是以 Vue 中数据可变的、细粒度的响应性系统为基础的，而函数式编程通常强调数据不可变。

## 为什么要有组合式 API？

### 更好的逻辑复用

组合式 API 最基本的优势是它使我们能够通过[组合函数](https://cn.vuejs.org/guide/reusability/composables.html)来实现更加简洁高效的逻辑复用。在选项式 API 中我们主要的逻辑复用机制是 mixins，而组合式 API 解决了 [mixins 的所有缺陷](https://cn.vuejs.org/guide/reusability/composables.html#vs-mixins)。

组合式 API 提供的逻辑复用能力孵化了一些非常棒的社区项目，比如 [VueUse](https://vueuse.org/)，一个不断成长的工具型组合式函数集合。组合式 API 还为其他第三方状态管理库与 Vue 的响应式系统之间的集成提供了一套简洁清晰的机制，例如 [RxJS](https://vueuse.org/rxjs/readme.html#vueuse-rxjs)。

### 更灵活的代码组织

许多用户喜欢选项式 API 的原因是它在默认情况下就能够让人写出有组织的代码：大部分代码都自然地被放进了对应的选项里。然而，选项式 API 在单个组件的逻辑复杂到一定程度时，会面临一些无法忽视的限制。这些限制主要体现在需要处理多个**逻辑关注点**的组件中，这是我们在许多 Vue 2 的实际案例中所观察到的。

我们以 Vue CLI GUI 中的文件浏览器组件为例：这个组件承担了以下几个逻辑关注点：

- 追踪当前文件夹的状态，展示其内容
- 处理文件夹的相关操作 (打开、关闭和刷新)
- 支持创建新文件夹
- 可以切换到只展示收藏的文件夹
- 可以开启对隐藏文件夹的展示
- 处理当前工作目录中的变更

#### Option Api

选项式API（Option Api）需要在特定的区域（data，methods，watch，computed...）编写负责相同功能的代码。如果我们为相同的逻辑关注点标上一种颜色，那将会是这样：

![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1bd101840df446c78d52e9c14711aae7~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

#### Option Api的缺陷

你可以看到，处理相同逻辑关注点的代码被强制拆分在了不同的选项中，位于文件的不同部分。在一个几百行的大组件中，要读懂代码中的一个逻辑关注点，需要在文件中反复上下滚动，这并不理想。

另外，如果我们想要将一个逻辑关注点抽取重构到一个可复用的工具函数中，需要从文件的多个不同部分找到所需的正确片段。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/568b0ced69f241d282cf2c512e4e5f33~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

#### Composition Api

使用传统的option选项写组件的时候问题，随着业务复杂度越来越高，代码量会不断的加大；由于相关业务的代码需要遵循option的配置写到特定的区域，导致后续维护非常的复杂，同时代码可复用性不高。

这种碎片化使得理解和维护复杂组件变得困难。选项的分离掩盖了潜在的逻辑问题。此外，在处理单个业务逻辑时，我们必须不断地“跳转”相关代码的选项块。

如果能够将同一个业务逻辑相关代码收集在一起会更好。而这正是组合式 API 使我们能够做到的。而组合式API就是为了解决这个问题而生的。

Composition API字面意思是组合式API，它是为了实现基于函数的逻辑复用机制而产生的。主要思想是，我们将它们定义为从新的setup函数返回的JavaScript变量，而不是将组件的功能(例如method、computed等)定义为对象属性。

使用组合式API，我们可以更加优雅的组织我们的代码，函数，让相关功能的代码更加有序的组织在一起。如果用组合式 API（Composition Api） 重构这个组件，将会变成下面右边这样：

![img](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d05799744a6341fd908ec03e5916d7b6~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

![img](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4146605abc9c4b638863e9a3f2f1b001~tplv-k3u1fbpfcp-zoom-in-crop-mark:4536:0:0:0.awebp)

现在与同一个逻辑关注点相关的代码被归为了一组：我们无需再为了一个逻辑关注点在不同的选项块间来回滚动切换。此外，我们现在可以很轻松地将这一组代码移动到一个外部文件中，不再需要为了抽象而重新组织代码，大大降低了重构成本，这在长期维护的大型项目中非常关键。

### 更好的类型推导

近几年来，越来越多的开发者开始使用 [TypeScript](https://www.typescriptlang.org/) 书写更健壮可靠的代码，TypeScript 还提供了非常好的 IDE 开发支持。然而选项式 API 是在 2013 年被设计出来的，那时并没有把类型推导考虑进去，因此我们不得不做了一些[复杂到夸张的类型体操](https://github.com/vuejs/core/blob/44b95276f5c086e1d88fa3c686a5f39eb5bb7821/packages/runtime-core/src/componentPublicInstance.ts#L132-L165)才实现了对选项式 API 的类型推导。但尽管做了这么多的努力，选项式 API 的类型推导在处理 mixins 和依赖注入类型时依然不甚理想。

因此，很多想要搭配 TS 使用 Vue 的开发者采用了由 `vue-class-component` 提供的 Class API。然而，基于 Class 的 API 非常依赖 ES 装饰器，在 2019 年我们开始开发 Vue 3 时，它仍是一个仅处于 stage 2 的语言功能。我们认为基于一个不稳定的语言提案去设计框架的核心 API 风险实在太大了，因此没有继续向 Class API 的方向发展。在那之后装饰器提案果然又发生了很大的变动，在 2022 年才终于到达 stage 3。另一个问题是，基于 Class 的 API 和选项式 API 在逻辑复用和代码组织方面存在相同的限制。

相比之下，组合式 API 主要利用基本的变量和函数，它们本身就是类型友好的。用组合式 API 重写的代码可以享受到完整的类型推导，不需要书写太多类型标注。大多数时候，用 TypeScript 书写的组合式 API 代码和用 JavaScript 写都差不太多！这也让许多纯 JavaScript 用户也能从 IDE 中享受到部分类型推导功能。

### 更小的生产包体积

搭配 `<script setup>` 使用组合式 API 比等价情况下的选项式 API 更高效，对代码压缩也更友好。这是由于 `<script setup>` 形式书写的组件模板被编译为了一个内联函数，和 `<script setup>` 中的代码位于同一作用域。不像选项式 API 需要依赖 `this` 上下文对象访问属性，被编译的模板可以直接访问 `<script setup>` 中定义的变量，无需一个代码实例从中代理。这对代码压缩更友好，因为本地变量的名字可以被压缩，但对象的属性名则不能。

## 与选项式 API 的关系

### 取舍

一些从选项式 API 迁移来的用户发现，他们的组合式 API 代码缺乏组织性，并得出了组合式 API 在代码组织方面“更糟糕”的结论。我们建议持有这类观点的用户换个角度思考这个问题。

组合式 API 不像选项式 API 那样会手把手教你该把代码放在哪里。但反过来，它却让你可以像编写普通的 JavaScript 那样来编写组件代码。这意味着**你能够，并且应该在写组合式 API 的代码时也运用上所有普通 JavaScript 代码组织的最佳实践**。如果你可以编写组织良好的 JavaScript，你也应该有能力编写组织良好的组合式 API 代码。

选项式 API 确实允许你在编写组件代码时“少思考”，这是许多用户喜欢它的原因。然而，在减少费神思考的同时，它也将你锁定在规定的代码组织模式中，没有摆脱的余地，这会导致在更大规模的项目中难以进行重构或提高代码质量。在这方面，组合式 API 提供了更好的长期可维护性。

### 组合式 API 是否覆盖了所有场景？

组合式 API 能够覆盖所有状态逻辑方面的需求。除此之外，只需要用到一小部分选项：`props`，`emits`，`name` 和 `inheritAttrs`。如果使用 `<script setup>`，那么 `inheritAttrs` 应该是唯一一个需要用额外的 `<script>` 块书写的选项了。

如果你在代码中**只**使用了组合式 API (以及上述必需的选项)，那么你可以通过配置[编译时标记](https://github.com/vuejs/core/tree/main/packages/vue#bundler-build-feature-flags)来去掉 Vue 运行时中针对选项式 API 支持的代码，从而减小生产包大概几 kb 左右的体积。注意这个配置也会影响你依赖中的 Vue 组件。

### 可以同时使用两种 API 吗？

可以。你可以在一个选项式 API 的组件中通过 [`setup()`](https://cn.vuejs.org/api/composition-api-setup.html) 选项来使用组合式 API。

然而，我们只推荐你在一个已经基于选项式 API 开发了很久、但又需要和基于组合式 API 的新代码或是第三方库整合的项目中这样做。

### 选项式 API 会被废弃吗？

不会，我们没有任何计划这样做。选项式 API 也是 Vue 不可分割的一部分，也有很多开发者喜欢它。我们也意识到组合式 API 更适用于大型的项目，而对于中小型项目来说选项式 API 仍然是一个不错的选择。

## 与 Class API 的关系

我们不再推荐在 Vue 3 中使用 Class API，因为组合式 API 提供了很好的 TypeScript 集成，并具有额外的逻辑重用和代码组织优势。

## 和 React Hooks 的对比

组合式 API 提供了和 React Hooks 相同级别的逻辑组织能力，但它们之间有着一些重要的区别。

React Hooks 在组件每次更新时都会重新调用。这就产生了一些即使是经验丰富的 React 开发者也会感到困惑的问题。这也带来了一些性能问题，并且相当影响开发体验。例如：

- Hooks 有严格的调用顺序，并不可以写在条件分支中。
- React 组件中定义的变量会被一个钩子函数闭包捕获，若开发者传递了错误的依赖数组，它会变得“过期”。这导致了 React 开发者非常依赖 ESLint 规则以确保传递了正确的依赖，然而，这些规则往往不够智能，保持正确的代价过高，在一些边缘情况时会遇到令人头疼的、不必要的报错信息。
- 昂贵的计算需要使用 `useMemo`，这也需要传入正确的依赖数组。
- 在默认情况下，传递给子组件的事件处理函数会导致子组件进行不必要的更新。子组件默认更新，并需要显式的调用 `useCallback` 作优化。这个优化同样需要正确的依赖数组，并且几乎在任何时候都需要。忽视这一点会导致默认情况下对应用进行过度渲染，并可能在不知不觉中导致性能问题。
- 要解决变量闭包导致的问题，再结合并发功能，使得很难推理出一段钩子代码是什么时候运行的，并且很不好处理需要在多次渲染间保持引用 (通过 `useRef`) 的可变状态。

相比起来，Vue 的组合式 API：

- 仅调用 `setup()` 或 `<script setup>` 的代码一次。这使得代码更符合日常 JavaScript 的直觉，不需要担心闭包变量的问题。组合式 API 也并不限制调用顺序，还可以有条件地进行调用。
- Vue 的响应性系统运行时会自动收集计算属性和侦听器的依赖，因此无需手动声明依赖。
- 无需手动缓存回调函数来避免不必要的组件更新。Vue 细粒度的响应性系统能够确保在绝大部分情况下组件仅执行必要的更新。对 Vue 开发者来说几乎不怎么需要对子组件更新进行手动优化。

我们承认 React Hooks 的创造性，它是组合式 API 的一个主要灵感来源。然而，它的设计也确实存在上面提到的问题，而 Vue 的响应性模型恰好提供了一种解决这些问题的方法。

# setup函数

`setup()` 钩子是在组件中使用组合式 API 的入口，通常只在以下情况下使用：

1. 需要在非单文件组件中使用组合式 API 时。
2. 需要在基于选项式 API 的组件中集成基于组合式 API 的代码时。

> 注意
>
> 对于结合单文件组件使用的组合式 API，推荐通过 [`<script setup> `](https://cn.vuejs.org/api/sfc-script-setup.html) 以获得更加简洁及符合人体工程学的语法。

## 基本使用

组合式API是从 `setup()` 函数开始的，在组件创建之前会自动执行 `setup()` 函数，是组合式API的入口。

- 直接在  `setup()` 中声明组件的状态、函数、计算属性等，之前写在选项式API中的data、methos、computed等等，现在全部写在   `setup()` 中

- 需要在在 `setup()` 的最后位置中返回一个对象，该对象应该包含在  `setup()` 中声明组件的状态、函数、计算属性等，该对象会暴露给组件模板和组件实例，在组件模板和组件实例上，可以直接访问该对象的属性
- 其它的选项也可以通过组件实例来获取 `setup()` 暴露的属性

```html
<div id="app">
  <p>num = {{num}}</p>
  <button @click="fn">绑定fn函数</button>
</div>

<script>
  let { createApp, onMounted } = Vue;
  const app = createApp({
    // 在选项式API中通过this是可以访问setup函数return出来的属性
    data() {
      return {
        num: 1,
      };
    },
    created() {
      console.log('created num=', this.num);
      this.fn();
    },
    methods: {
      fn() {
        console.log('选项式中的 fn函数');
      },
    },
    setup (props) {
      // 声明组件的状态（非响应式）
      let num = 2;
      
      // 声明组件的函数
      let fn = () => {
        num++;
        console.log('组合式中的 fn函数 num', num);
      }
      
      // 生命周期函数
      onMounted(()=>{
        console.log('组合式中的 onMounted ', num);
      });

      // setup函数返回一个对象，该对象的属性可以直接在模板中使用 {{num}}
      return {
        num, // 暴露属性
        fn,  // 暴露函数
      }
    }
  });
  app.mount('#app');
</script>
```

注意：使用了组合式API，就不要再使用选项式API

- 在选项式API配置选项（data、methos、computed...）中通过this可以访问组合式API  `setup()` 函数中的属性和方法，但是不推荐这样做
- 在组合式API  `setup()` 函数中不能使用this访问选项式API配置选项（data、methos、computed...）中的数据
- 如果在选项式API中添加了属性和方法，在组合式API中也添加了同名属性和方法，组合式中的数据覆盖了选项式的数据
- setup() 应该同步地返回一个对象。唯一可以使用 async setup() 的情况是，该组件是 Suspense 组件的后裔。

在 `setup()` 函数中并不含对组件实例的访问权，即在 setup() 中访问 this 会是 undefined。如果想要访问组件实例可以使用 `getCurrentInstance`，但是按照组合式API设计的思想，我们是不需要访问组件实例的：

```js
let { createApp, getCurrentInstance } = Vue;
const app = createApp({
  setup (props) {
    let num = 2;
    // 在setup函数或者组合式声明周期函数中使用 getCurrentInstance 获取组件实例，然后通过 ctx 属性获取当前上下文
    // 但是不要为了使用this而是专门使用 getCurrentInstance，因为在组合式API中不需要使用this了
    const instance = getCurrentInstance();
    console.log(instance);
    console.log(instance.ctx.num);
  }
});
```

## 访问 Props

`setup` 函数的第一个参数是组件的 `props`。和标准的组件一致，一个 `setup` 函数的 `props` 是响应式的，并且会在传入新的 props 时同步更新。

```js
export default {
  props: {
    title: String
  },
  setup(props) {
    console.log(props.title)
  }
}
```

请注意如果你解构了 `props` 对象，解构出的变量将会丢失响应性。因此我们推荐通过 `props.xxx` 的形式来使用其中的 props。

如果你确实需要解构 `props` 对象，或者需要将某个 prop 传到一个外部函数中并保持响应性，那么你可以使用 [toRefs()](https://cn.vuejs.org/api/reactivity-utilities.html#torefs) 和 [toRef()](https://cn.vuejs.org/api/reactivity-utilities.html#toref) 这两个工具函数：

```js
import { toRefs, toRef } from 'vue'

export default {
  setup(props) {
    // 将 `props` 转为一个其中全是 ref 的对象，然后解构
    const { title } = toRefs(props)
    // `title` 是一个追踪着 `props.title` 的 ref
    console.log(title.value)

    // 或者，将 `props` 的单个属性转为一个 ref
    const title = toRef(props, 'title')
  }
}
```

## Setup 上下文

传入 `setup` 函数的第二个参数是一个 **Setup 上下文**对象。上下文对象暴露了其他一些在 `setup` 中可能会用到的值：

```js
export default {
  setup(props, context) {
    // 透传 Attributes（非响应式的对象，等价于 $attrs）
    console.log(context.attrs)

    // 插槽（非响应式的对象，等价于 $slots）
    console.log(context.slots)

    // 触发事件（函数，等价于 $emit）
    console.log(context.emit)

    // 暴露公共属性（函数）
    console.log(context.expose)
  }
}
```

该上下文对象是非响应式的，可以安全地解构：

```js
export default {
  setup(props, { attrs, slots, emit, expose }) {
    ...
  }
}
```

`attrs` 和 `slots` 都是有状态的对象，它们总是会随着组件自身的更新而更新。这意味着你应当避免解构它们，并始终通过 `attrs.x` 或 `slots.x` 的形式使用其中的属性。此外还需注意，和 `props` 不同，`attrs` 和 `slots` 的属性都**不是**响应式的。如果你想要基于 `attrs` 或 `slots` 的改变来执行副作用，那么你应该在 `onBeforeUpdate` 生命周期钩子中编写相关逻辑。

## 暴露公共属性

`expose` 函数用于显式地限制该组件暴露出的属性，当父组件通过[模板引用](https://cn.vuejs.org/guide/essentials/template-refs.html#ref-on-component)访问该组件的实例时，将仅能访问 `expose` 函数暴露出的内容：

```js
export default {
  setup(props, { expose }) {
    // 让组件实例处于 “关闭状态”
    // 即不向父组件暴露任何东西
    expose()

    const publicCount = ref(0)
    const privateCount = ref(0)
    // 有选择地暴露局部状态
    expose({ count: publicCount })
  }
}
```

## 与渲染函数一起使用

`setup` 也可以返回一个[渲染函数](https://cn.vuejs.org/guide/extras/render-function.html)，此时在渲染函数中可以直接使用在同一作用域下声明的响应式状态：

```js
import { h, ref } from 'vue'

export default {
  setup() {
    const count = ref(0)
    return () => h('div', count.value)
  }
}
```

返回一个渲染函数将会阻止我们返回其他东西。对于组件内部来说，这样没有问题，但如果我们想通过模板引用将这个组件的方法暴露给父组件，那就有问题了。

我们可以通过调用 [`expose()`](https://cn.vuejs.org/api/composition-api-setup.html#exposing-public-properties) 解决这个问题：

```js
import { h, ref } from 'vue'

export default {
  setup(props, { expose }) {
    const count = ref(0)
    const increment = () => ++count.value

    expose({
      increment
    })

    return () => h('div', count.value)
  }
}
```

此时父组件可以通过模板引用来访问这个 `increment` 方法。

# 响应式核心reactive和ref

## 用reactive声明响应式状态

### reactive概念

reactive返回一个对象的响应式代理。

- 响应式转换是“深层”的：它会影响到所有嵌套的属性。一个响应式对象也将深层地解包任何 [ref](https://cn.vuejs.org/api/reactivity-core.html#ref) 属性，同时保持响应性。

- 值得注意的是，当访问到某个响应式数组或 `Map` 这样的原生集合类型中的 ref 元素时，不会执行 ref 的解包。

- 若要避免深层响应式转换，只想保留对这个对象顶层次访问的响应性，请使用 [shallowReactive()](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive) 作替代。

- 返回的对象以及其中嵌套的对象都会通过 [ES Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) 包裹，因此**不等于**源对象，建议只使用响应式代理，避免使用原始对象。

### reactive的使用

我们可以使用 [`reactive()`](https://cn.vuejs.org/api/reactivity-core.html#reactive) 函数创建一个响应式**对象**或**数组**：

```html
<script>
import { reactive } from 'vue';

export default {
	setup() {
		// 组件状态
		const zhangsan = reactive({ name: '张三', age: 12 });
		const nums = reactive([60, 70, 80]);

		// 组件函数
		function increment() {
			zhangsan.age++;
			nums.push(nums[nums.length - 1] + 10);
		}

		return {
			zhangsan,
			nums,
			increment,
		};
	},
};
</script>

<template>
	<button @click="increment">按钮</button>
  <p> zhangsan.age：{{ zhangsan.age }}</p>
  <p>nums：{{ nums }}</p>
</template>
```

>  `setup()` 函数的简化用法，详细内容见后面setup语法糖：
>
> 在 `setup()` 函数中手动暴露大量的状态和方法非常繁琐。幸运的是，我们可以通过使用构建工具来简化该操作。当使用单文件组件（SFC）时，我们可以使用 `<script setup>` 来大幅度地简化代码。
>
> ```vue
> <script setup>
>   import { reactive } from 'vue'
> 
>   const zhangsan = reactive({  name: '张三', age: 12 });
> 
>   function increment() {
>     zhangsan.age++
>   }
> </script>
> 
> <template>
>   <button @click="increment">{{ zhangsan.age++ }}</button>
> </template>
> ```
>
> `<script setup>` 中的顶层的导入和变量声明可在同一组件的模板中直接使用。你可以理解为模板中的表达式和 `<script setup>` 中的代码处在同一个作用域中。

### reactive的深层响应性

在 Vue 中，状态都是默认深层响应式的。这意味着即使在更改深层次的对象或数组，你的改动也能被检测到。

```vue
<script setup>
  import { reactive } from 'vue'

  const obj = reactive({
    nested: { count: 0 },
    arr: ['foo', 'bar']
  })

  function mutateDeeply() {
    // 以下更改都会响应式的更新模板内容
    obj.nested.count++
    obj.arr.push('baz')
  }
</script>

<template>
  <button @click="mutateDeeply">{{ obj }}</button>
</template>
```

### reactive 的局限性

`reactive()` API 有两条限制：

1. 仅对对象类型有效（对象、数组和 `Map`、`Set` 这样的[集合类型](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects#使用键的集合对象)），而对 `string`、`number` 和 `boolean` 这样的 [原始类型](https://developer.mozilla.org/zh-CN/docs/Glossary/Primitive) 无效。

2. 因为 Vue 的响应式系统是通过属性访问进行追踪的，因此我们必须始终保持对该响应式对象的相同引用。这意味着我们不可以随意地“替换”一个响应式对象，因为这将导致对初始引用的响应性连接丢失：

   ```js
   let state = reactive({ count: 0 })
   
   // 给state重新赋值值一个响应式对象，上面的引用 ({ count: 0 }) 将不再被追踪（响应性连接已丢失！）
   state = reactive({ count: 1 })
   ```

### reactive 响应式丢失的情况

当我们将响应式对象的属性赋值或解构至本地变量时，或是将该属性传入一个函数时，我们会失去响应性：

- 将响应式对象的属性赋值至本地变量失去响应性

  ```js
  const state = reactive({ count: 0 })
  
  // n 是一个局部变量，n 和 state.count，失去响应性连接
  let n = state.count
  // 不影响原始的 state，改变的仅仅是局部变量n的值，将无法跟踪 state.count 的变化
  n++;
  console.log('n: ', n); // 1
  console.log('state.count: ', state.count); // 0
  ```

- 将响应式对象的属性解构至本地变量失去响应性

  ```js
  const state = reactive({ count: 0 })
  // count 是一个局部变量，count 也和 state.count 失去了响应性连接
  let { count } = state
  // 不会影响原始的 state，改变的仅仅是局部变量count的值，将无法跟踪 state.count 的变化
  count++
  console.log('count: ', count); // 1
  console.log('state.count: ', state.count); // 0
  ```

- 将响应式对象的属性传入一个函数失去响应式

  ```js
  const state = reactive({ count: 6 });
  
  function doubleTen(data) {
    data *= 10;
    return data * 2;
  }
  
  // doubleTen 函数接收一个普通数字，并且将无法跟踪 state.count 的变化
  console.log(doubleTen(state.count)); // 120
  console.log(state.count); // 6
  ```

### 响应式代理 vs. 原始对象

响应式对象其实是 [JavaScript Proxy](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Proxy)，其行为表现与一般对象相似。不同之处在于 Vue 能够跟踪对响应式对象属性的访问与更改操作。所以`reactive()` 返回的是一个原始对象的 [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy)，它和原始对象是不相等的：

```js
const obj = {}
const proxyObj = reactive(obj)

// 代理对象和原始对象不是全等的
console.log(proxyObj === obj) // false
```

只有代理对象是响应式的，更改原始对象不会触发更新。因此，使用 Vue 的响应式系统的最佳实践是 **仅使用你声明对象的代理版本**。

为保证访问代理的一致性，对同一个原始对象调用 `reactive()` 会总是返回同样的代理对象，而对一个已存在的代理对象调用 `reactive()` 会返回其本身：

```js
// 在同一个对象上调用 reactive() 会返回相同的代理
console.log(reactive(obj) === proxyObj) // true

// 在一个代理上调用 reactive() 会返回它自己
console.log(reactive(proxyObj) === proxyObj) // true
```

这个规则对嵌套对象也适用。依靠深层响应性，响应式对象内的嵌套对象依然是代理：

```js
const proxyObj = reactive({})

const obj = {}
proxyObj.nested = obj

console.log(proxyObj.nested === obj) // false
```

## 用ref定义响应式变量

### ref的概念

`reactive()` 的种种限制归根结底是因为 JavaScript 没有可以作用于所有值类型的 “引用” 机制。为此，Vue 提供了一个 [`ref()`](https://cn.vuejs.org/api/reactivity-core.html#ref) 方法来允许我们创建可以使用**任何值类型**的响应式 **ref**。

ref 接受一个任意类型的内部值，返回一个响应式的、可更改的 ref 对象，此对象只有一个指向其内部值的属性 `.value`。

- ref 对象是可更改的，可以为 `.value` 赋予新的值。ref 的 `.value` 属性也是响应式的，即所有对 `.value` 的操作都将被追踪，并且写操作会触发与之相关的副作用。

- 如果将一个对象赋值给 ref，那么这个对象将通过 [reactive()](https://cn.vuejs.org/api/reactivity-core.html#reactive) 转为具有深层次响应式的对象。这也意味着如果对象中包含了嵌套的 ref，它们将被深层地解包。

- 若要避免这种深层次的转换，请使用 [`shallowRef()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowref) 来替代。

### 值类型的ref

当值为值类型时，会用`Object.defineProperty() `添加 `get()` 和 `set()` 实现响应式：

```vue
<script setup>
  import { ref } from 'vue'

  const count = ref(0)
  console.log(count.value) // 0

  function increment() {
    count.value++
    console.log(count.value) // 1 2 3 ...
  }
</script>

<template>
  <button @click="increment">{{ count }}</button>
</template>
```

### 对象类型的ref

当值为对象类型时，会用 `reactive()` 自动转换它的 `.value`，实现响应式：

```vue
<script setup>
import { ref } from 'vue';

// 数组
const nums = ref([1, 2, 3]);
console.log(nums.value); // Proxy {0: 1, 1: 2, 2: 3}

// 对象
const objRef1 = ref({ count: 1 });
const objRef2 = ref({ count: 2 });
console.log(objRef1.value.count); //  1
console.log(objRef2.value.count); //  2
  
function increment() {
	nums.value.push(nums.value[nums.value.length - 1] + 1);
	objRef1.value.count++;
  // 一个包含对象类型值的 ref 可以响应式地替换整个对象
	objRef2.value = { count: nums.value.length };
}
</script>

<template>
	<button @click="increment">按钮</button>
	<p>nums: {{ nums }}</p>
	<p>objRef1.count: {{ objRef1.count }}</p>
	<p>objRef2.count: {{ objRef2.count }}</p>
</template>
```

- ref 被传递给函数，不会丢失响应性：

  ```js
  const count = ref(1);
  
  // 该函数接收一个 ref，需要通过 .value 取值，但它会保持响应性
  function doubleTen(data) {
  	data.value *= 10;
  	return data.value * 2;
  }
  
  console.log(doubleTen(count)); //  20
  console.log(count.value); //  10
  ```

- ref 从一般对象上被解构时，不会丢失响应性：

  ```js
  const obj = {
    foo: ref(1),
    bar: ref(2),
  };
  
  // 使用解构仍然是响应式的
  const { foo, bar } = obj
  
  // 该函数接收一个 ref，需要通过 .value 取值，但它会保持响应性
  function doubleTen(data) {
  	data.value *= 10;
  	return data.value * 2;
  }
  console.log(doubleTen(foo)); //  20
  console.log(foo.value); //  10
  ```

简言之，`ref()` 让我们能创造一种对任意值的 “引用”，并能够在不丢失响应性的前提下传递这些引用。这个功能很重要，因为它经常用于将逻辑提取到 [组合函数](https://cn.vuejs.org/guide/reusability/composables.html) 中。

### ref的解包

#### ref 在模板中的解包

当 ref 在**模板**中作为顶层属性被访问时，它们会被自动“解包”，所以不需要使用 `.value`：

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}
</script>

<template>
  <button @click="increment">
    {{ count }} <!-- 无需 .value -->
  </button>
</template>
```

**请注意，仅当 ref 是模板渲染上下文的顶层属性时才适用自动“解包”。** 

上面的 count 是顶层属性，但下面例子的 obj.foo 不是顶层属性：

```js
const obj = { foo: ref(1) }

console.log(obj.foo); // RefImpl {__v_isShallow: false, dep: undefined, __v_isRef: true, _rawValue: 1, _value: 1}

// 渲染的结果会是一个 [object Object]1，因为 obj.foo 是一个 ref 对象
console.log(obj.foo + 1); // [object Object]1

// 正确用法应该使用 obj.foo.value
console.log(obj.foo.value + 1); // 2


// 或者我们可以通过将 foo 改成顶层属性来解决这个问题：
const { foo } = obj;
// 现在渲染结果将是 2
console.log(foo + 1); // 2
```

需要注意的是，如果一个 ref 是文本插值（即一个 `{{ }}` 符号）计算的最终值，它也将被解包。因此在模板中可以单独使用 `obj.foo` 下面的渲染结果将为 `1`：

```template
{{ obj.foo }}
```

这只是文本插值的一个方便功能，相当于 `{{ obj.foo.value }}`。

#### ref 在响应式对象中的解包

当一个 `ref` 被嵌套在一个响应式对象中，作为属性被访问或更改时，它会自动解包，因此会表现得和一般的属性一样：

```js
const count = ref(0)
const state = reactive({
  foo: count,
})

console.log(state.foo) // 0

state.foo = 1
console.log(count.value) // 1
```

如果将一个新的 ref 赋值给一个关联了已有 ref 的属性，那么它会替换掉旧的 ref：

```js
const count = ref(0)
const state = reactive({
  foo: count,
})

console.log(state.foo) // 0

const otherCount = ref(2)
state.foo = otherCount
console.log(state.foo) // 2

// 原始 count 现在已经和 state.foo 失去联系
console.log(count.value) // 1
```

只有当嵌套在一个深层响应式对象内时，才会发生 ref 解包。当其作为[浅层响应式对象](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive)的属性被访问时不会解包。

#### 数组和集合类型的 ref 解包

跟响应式对象不同，当 ref 作为响应式数组或像 `Map` 这种原生集合类型的元素被访问时，不会进行解包。

```js
const books = reactive([ref('Vue 3 Guide')])
// 这里需要 .value
console.log(books[0].value)

const map = reactive(new Map([['count', ref(0)]]))
// 这里需要 .value
console.log(map.get('count').value)
```

### 数组声明与赋值的技巧

数组赋值存在的问题：使用reactive声明数组，给空数组赋值不能直接赋值，因为`arr = [1, 2, 3];` 让arr失去了响应式。

```js
let arr = reactive([]);
arr = [1, 2, 3]; // arr会失去响应式
```

解决方法如下：

方法一：使用ref声明数组，在组件模板中直接使用arr，在setup中需要使用arr.value

```js
const arr = ref([]);
arr.value = [1, 2, 3];
```

方法二：使用reactive声明数组，使用的push方法添加新的元素

```js
const arr = reactive([]);
arr.push(...[1, 2, 3]);
```

方法三：创建一个响应式对象，对象的属性是数组，给该属性直接赋值

```js
const obj = reactive({
  arr: [],
})
obj.arr = [1, 2, 3];
```

## reactive和ref的对比 

### ref的研究

1. ref创建一个响应式数据，一般来说用于创建简单类型的响应式对象，比如String、Number、boolean类型；
2. 当我们给ref传递一个值之后，如果使用的是基本类型响应式依赖 `Object.defineProperty() `的 `get()` 和 `set()` ，如果ref使用的是引用类型，ref函数底层会自动将ref转换成reactive; `ref(18) `等价于`reactive({value:18})；`

3. 需要注意的是ref定义的值在组件模板中使用直接使用所定义的字段，但是在setup函数中获取或者修改值需要通过value，当然还有一些自动解包的场景
4. ref也可以创建引用类型，对于复杂的对象，值是一个被proxy拦截处理过的对象，但是里面的属性不是RefImpl类型的对象，proxy代理的对象同样被挂载到value上，所以可以通过obj.value.key来读取属性，这些属性同样也是响应式的，更改时可以触发视图的更新

### reactive研究

1. reactive里面参数定义必须是对象或者数组(json/arr)，本质将传入的数据包装成proxy对象；

2. 基于Es6的Proxy实现，通过Reflect反射代理操作源对象，相比于reactive定义的浅层次响应式数据对象，reactive定义的是更深层次的响应式数据对象；

### 总结

1. 一般来说，ref被用来定义简单的字符串或者数值，而reactive被用来定义对象数组等

2. 实际上都能用，而且ref也可以去定义简单的对象和数组，也是具有响应式的，不过官方文档中有提到如果将对象分配为ref值，则可以通过reactive方法使该对象具有高度的响应式。

## DOM 更新时机

当你更改响应式状态后，DOM 会自动更新。然而，你得注意 DOM 的更新并不是同步的。相反，Vue 将缓冲它们直到更新周期的 “下个时机” 以确保无论你进行了多少次状态更改，每个组件都只更新一次。

若要等待一个状态改变后的 DOM 更新完成，你可以使用 [nextTick()](https://cn.vuejs.org/api/general.html#nexttick) 这个全局 API：

```vue
<script setup>
  import { ref, nextTick } from 'vue'

  const count = ref(0)

  function increment() {
    count.value++
    nextTick(() => {
      // 访问更新后的 DOM
    })
  }
</script>

<template>
  <button @click="increment">{{ count }}</button>
</template>
```

# 响应式工具函数

## isRef()

检查某个值是否为 ref。

```js
const count = ref(10);			
const result = isRef(count);
console.log(result); // true
```

## unref()

如果参数是 ref，则返回内部值，否则返回参数本身。这是 `val = isRef(val) ? val.value : val` 计算的一个语法糖

```js
const count = ref(10);			
const result = unref(count);
console.log(result); // 10
```

## toRef()和toRefs()

### toRef()

基于响应式对象上的一个属性，创建一个对应的 ref。这样创建的 ref 与其源属性保持同步：改变源属性的值将更新 ref 的值，反之亦然。

```js
const state = reactive({
  foo: 1,
  bar: 2
})

const fooRef = toRef(state, 'foo')

// 更改该 ref 会更新源属性
fooRef.value++
console.log(state.foo) // 2

// 更改源属性也会更新该 ref
state.foo++
console.log(fooRef.value) // 3
```

请注意，这不同于：

```js
const fooRef = ref(state.foo)
```

上面这个 ref **不会**和 `state.foo` 保持同步，因为这个 `ref()` 接收到的是一个纯数值。

`toRef()` 这个函数在你想把一个 prop 的 ref 传递给一个组合式函数时会很有用：

```vue
<script setup>
import { toRef } from 'vue'

const props = defineProps(['foo'])

// 将 `props.foo` 转换为 ref，然后传入一个组合式函数
useSomeFeature(toRef(props, 'foo'))
</script>
```

当 `toRef` 与组件 props 结合使用时，关于禁止对 props 做出更改的限制依然有效。尝试将新的值传递给 ref 等效于尝试直接更改 props，这是不允许的。在这种场景下，你可能可以考虑使用带有 `get` 和 `set` 的 [`computed`](https://cn.vuejs.org/api/reactivity-core.html#computed) 替代。详情请见[在组件上使用 `v-model`](https://cn.vuejs.org/guide/components/events.html#usage-with-v-model) 指南。

即使源属性当前不存在，`toRef()` 也会返回一个可用的 ref。这让它在处理可选 props 的时候格外实用，相比之下 [`toRefs`](https://cn.vuejs.org/api/reactivity-utilities.html#torefs) 就不会为可选 props 创建对应的 refs。

### toRefs()

将一个响应式对象转换为一个普通对象，这个普通对象的每个属性都是指向源对象相应属性的 ref。每个单独的 ref 都是使用 [`toRef()`](https://cn.vuejs.org/api/reactivity-utilities.html#toref) 创建的。

```js
const state = reactive({
  foo: 1,
  bar: 2
})

const stateAsRefs = toRefs(state)

// 这个 ref 和源属性已经“链接上了”
state.foo++
console.log(stateAsRefs.foo.value) // 2

stateAsRefs.foo.value++
console.log(state.foo) // 3
```

当从组合式函数中返回响应式对象时，`toRefs` 相当有用。使用它，消费者组件可以解构/展开返回的对象而不会失去响应性：

```js
function useFeatureX() {
  const state = reactive({
    foo: 1,
    bar: 2
  })

  // ...基于状态的操作逻辑

  // 在返回时都转为 ref
  return toRefs(state)
  
  // 等价于下面的写法
  return {
    ...toRefs(state)
  }
}

// 可以解构而不会失去响应性
const { foo, bar } = useFeatureX()
```

`toRefs` 在调用时只会为源对象上可以枚举的属性创建 ref。如果要为可能还不存在的属性创建 ref，请改用 [`toRef`](https://cn.vuejs.org/api/reactivity-utilities.html#toref)。

### toRef()和toRefs()的对比

- toRef得到的结果是reactive对象中的某个属性转换为ref变量，并与原属性保持同步。如果该属性在原对象上不存在，会创建出一个新的ref变量
- toRefs得到的结果是reactive对象中的所有属性转换为ref变量，并与原属性保持同步。只会创建出原对象上存在的属性对应的ref变量，不会创建新的ref变量

有以下数据：

```js
const zhangsan = reactive({
  name: '张三',
  age: 18,
});
```

#### 使用toRef()的结果

- 使用toRef()，根据zhagnsan对象的属性创建ref变量，和原属性保持同步

```js
// 语义上等价于 const ageRef = ref(zhangsan.age); 把 zhangsan.age 转换为一个ref变量
// 但是，直接使用 ref 创建 ageRef，ageRef 与 zhangsan.age 的之间的关联将会丢失
const ageRef = toRef(zhangsan, 'age');
console.log('ageRef.value: ', ageRef.value);// ageRef.value:  18

zhangsan.age++;
console.log('zhangsan.age: ', zhangsan.age); // zhangsan.age:  19
console.log('ageRef.value: ', ageRef.value); // ageRef.value:  19

ageRef.value++;
console.log('zhangsan.age: ', zhangsan.age); // zhangsan.age:  20
console.log('ageRef.value: ', ageRef.value); // ageRef.value:  20
```

- 使用ref，根据zhagnsan对象的属性创建ref变量，不会和原属性保持同步，相当于创建了一个新的属性

```js
const ageRef = ref(zhangsan.age);
console.log('ageRef.value: ', ageRef.value); // ageRef.value:  18

zhangsan.age++;
console.log('zhangsan.age: ', zhangsan.age); // zhangsan.age:  19
console.log('ageRef.value: ', ageRef.value); // ageRef.value:  18

ageRef.value++;
console.log('zhangsan.age: ', zhangsan.age); // zhangsan.age:  19
console.log('ageRef.value: ', ageRef.value); // ageRef.value:  19
```

- 不存在的属性，得到新的ref变量

```js
const weightRef = toRef(zhangsan, 'weight');
console.log('weightRef.value: ', weightRef.value);// weightRef.value:  undefined
```

#### 使用toRefs()的结果

- 使用toRefs，根据zhagnsan对象的属性age和name，得到ref属性：zhangsanRef.name 和 zhangsanRef.age

```js
const zhangsanRef = toRefs(zhangsan);
console.log('zhangsanRef: ', zhangsanRef); // {name: ObjectRefImpl, age: ObjectRefImpl}
console.log('zhangsanRef.name.value: ', zhangsanRef.name.value); // zhangsanRef.name.value:  张三
console.log('zhangsanRef.age.value: ', zhangsanRef.age.value); // zhangsanRef.name.value:  18

zhangsan.age++;
console.log('zhangsan.age: ', zhangsan.age); // zhangsan.age:  19
console.log('zhangsanRef.age.value: ', zhangsanRef.age.value); // zhangsanRef.name.value:  19

zhangsanRef.age.value++;
console.log('zhangsan.age: ', zhangsan.age); // zhangsan.age:  20
console.log('zhangsanRef.age.value: ', zhangsanRef.age.value); // zhangsanRef.name.value:  20
```

### torefs的妙用

#### 在setup函数中的使用

```vue
<template>
  <p>zhangsan.age: {{ zhangsan.age }}</p>
  <p>age: {{ age }}</p>
</template>

<script>
import { reactive, toRefs } from 'vue'

export default {
  setup() {
    
    const zhangsan = reactive({  name: '张三', age: 12 });
    
    return {
      // 1、直接放回 zhangsan 在模板中可以使用 {{ zhangsan.age }}
      zhangsan,    
      
      // 2、通过扩展运算符配合toRefs展开zhangsan对象
      // 可以在组件的模板中直接使用zhangsan对象的name和age属性 {{ age }}，并保证了name和age的响应式
      ...toRefs(zhangsan),
      
      // 注意不能直接使用扩展运算符展开zhangsan，虽然这样也能在模板中使用name和age属性，但是name和age不是响应式的
      ...zhangsan,
    }
  }
}
</script>
```

#### 在setup语法糖中的使用

```vue
<template>
  <p>name: {{ name }}</p>
  <p>age: {{ age }}</p>
</template>

<script setup>
import { reactive } from 'vue'

const zhangsan = reactive({  name: '张三', age: 12 });

// 可以在组件的模板中直接使用zhangsan对象的name和age属性 {{ age }}，并保证了name和age的响应式
const { name, age} = ...toRefs(zhangsan);

</script>
```

## isProxy()

检查一个对象是否是由 [`reactive()`](https://cn.vuejs.org/api/reactivity-core.html#reactive)、[`readonly()`](https://cn.vuejs.org/api/reactivity-core.html#readonly)、[`shallowReactive()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive) 或 [`shallowReadonly()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreadonly) 创建的代理。

```js
const zhangsan = reactive({name: '张三' });
const result = isProxy(zhangsan);
console.log(result); // true
```

## isReactive()

检查一个对象是否是由 [`reactive()`](https://cn.vuejs.org/api/reactivity-core.html#reactive) 或 [`shallowReactive()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreactive) 创建的代理。

```js
const zhangsan = reactive({name: '张三' });
const result = isReactive(zhangsan);
console.log(result); // true
```

## isReadonly()

- `isReadonly()` 检查传入的值是否为只读对象。只读对象的属性可以更改，但他们不能通过传入的对象直接赋值。

- `readonly()` 接受一个对象 (不论是响应式还是普通的) 或是一个 `ref`，返回一个原值的只读代理。
  - 只读代理是深层的：对任何嵌套属性的访问都将是只读的。它的 ref 解包行为与 `reactive()` 相同，但解包得到的值是只读的。
  - 要避免深层级的转换行为，请使用 `shallowReadonly()` 作替代。

通过 [`readonly()`](https://cn.vuejs.org/api/reactivity-core.html#readonly) 和 [`shallowReadonly()`](https://cn.vuejs.org/api/reactivity-advanced.html#shallowreadonly) 创建的代理都是只读的，因为他们是没有 `set` 函数的 [`computed()`](https://cn.vuejs.org/api/reactivity-core.html#computed) ref。

```js
const result1 = isReadonly(zhangsan);
console.log(result1); // false

const zhangsanReadonly = readonly(zhangsan);
const result2 = isReadonly(zhangsanReadonly);
console.log(result2); // true
```

# 计算属性

## 只读计算属性

计算属性使用 `computed()` 实现，我们在这里定义了一个计算属性 `total` 和 `result`：

```vue
<script setup>
  import { ref, computed } from 'vue'

  const chinese = ref(0);
  const math = ref(0);
  const english = ref(0);

  // 只读的计算属性
  const total = computed(() => chinese.value + math.value + english.value);
  const result = computed(() => {
    if (total.value > 240) {
      return '优秀';
    } else if (total.value > 180) {
      return '良好';
    } else {
      return '不及格';
    }
  });
</script>

<template>
  语文: <input type="number" v-model.number="chinese"><br>
  数学: <input type="number" v-model.number="math"><br>
  英语: <input type="number" v-model.number="english"><br>
  <p>总分 = {{chinese + math + english}}</p>
  <p>total = {{total}}</p>
  <p>result = {{result}}</p>
</template>
```

`computed()` 方法期望接收一个 getter 函数，返回值为一个**计算属性 ref**。和其他一般的 ref 类似，你可以通过 `total.value` 访问计算结果。计算属性 ref 也会在模板中自动解包，因此在模板表达式中引用时无需添加 `.value`。

Vue 的计算属性会自动追踪响应式依赖。它会检测到 `total` 依赖于 `chinese、math、english`三个属性，所以当 `chinese、math、english` 任何一个属性改变时，任何依赖于 `total` 的绑定都会同时更新。

**计算属性值会基于其响应式依赖被缓存**。一个计算属性仅会在其响应式依赖更新时才重新计算。这意味着只要  `chinese、math、english`三个属性不改变，无论多少次访问 `total` 都会立即返回先前的计算结果，而不用重复执行 getter 函数。

## 可写计算属性

计算属性默认是只读的。当你尝试修改一个计算属性时，你会收到一个运行时警告。只在某些特殊场景中你可能才需要用到“可写”的属性，你可以通过同时提供 getter 和 setter 来创建：

```vue
<script setup>
  import { ref, computed } from 'vue'

  const count = ref(1);
  const bigCount = computed({
    // 获取计算属性的值，比如 console.log(bigCount.value) 会执行get函数
    get () {
      return count.value * 2;
    },
    // 设置计算属性的值，比如 bigCount.value = 10; 会执行set函数
    set (value) {
      count.value = value / 2;
    }
  });
</script>

<template>
  <input type="number" v-model.number="bigCount"> bigCount = {{bigCount}} <br>
</template>
```

# 侦听器

计算属性允许我们声明性地计算衍生值。然而在有些情况下，我们需要在状态变化时执行一些“副作用”：例如更改 DOM，或是根据异步操作的结果去修改另一处的状态。

在组合式 API 中，我们可以使用 `watch` 函数在每次响应式状态发生变化时触发回调函数。

## watch

### 概念

`watch()`侦听一个或多个响应式数据源，并在数据源变化时调用所给的回调函数。

语法：`watch(source, callback, options?)`

- 第一个参数是侦听器的**源**。这个来源可以是以下几种：

  - 一个函数，返回一个值

  - 一个 ref

  - 一个响应式对象 reactive

  - ...或是由以上类型的值组成的数组


- 第二个参数是在发生变化时要调用的回调函数。这个回调函数接受三个参数：

  - 新值

  - 旧值

  - 以及一个用于注册副作用清理的回调函数。
    - 该回调函数会在副作用下一次重新执行前调用，可以用来清除无效的副作用，例如等待中的异步请求。
    - 当侦听多个来源时，回调函数接受两个数组，分别对应来源数组中的新值和旧值。


- 第三个可选的参数是一个对象，支持以下这些选项：

  - **`immediate`**：在侦听器创建时立即触发回调。第一次调用时旧值是 `undefined`。

  - **`deep`**：如果源是对象，强制深度遍历，以便在深层级变更时触发回调。

  - **`flush`**：调整回调函数的刷新时机。

  - **`onTrack / onTrigger`**：调试侦听器的依赖。


### 基本使用

```vue
<script setup>
  import { ref, watch } from 'vue'

  const count = ref(1);
  const zhangsan = reactive({
    name: '张三',
    age: 18,
  }); 

  watch(count, (newValue, oldValue) => {
    console.log('count: ', newValue, oldValue);
  });
 
</script>

<template>
	<button @click="count++">count = {{count}}</button>
	<button @click="zhangsan.age++">zhangsan.age = {{zhangsan.age}}</button>
</template>
```

- 监听一个 ref

```js
// 当count.value的值发生变化时，触发回调函数
watch(count, (newValue, oldValue) => {
	console.log('count: ', newValue, oldValue);
});

count.value++;
```

- 监听一个响应式对象 reactive：当直接侦听一个响应式对象时，侦听器会自动启用深层模式

```js
// zhangsan对象的任何一个属性发生变化时，都会触发回调函数
watch(zhangsan, (newValue, oldValue) => {
  console.log('zhangsan: ', newValue, oldValue);
});

zhangsan.age++;
```

- 监听一个 getter 函数：当使用 getter 函数作为源时，回调只在此函数的返回值变化时才会触发。

```js
// 当count.value的值发生变化时，触发回调函数
watch(() => count.value, (newValue, oldValue) => {
	console.log('count.value: ', newValue, oldValue);
});
count.value++;


// 当zhangsan.age的值发生变化时，触发回调函数
watch(() => zhangsan.age, (newValue, oldValue) => {
  console.log('zhangsan.age: ', newValue, oldValue);
});
zhangsan.age++;
```

- 或是由以上类型的值组成的数组

```js
// 注意多个同步更改只会触发一次侦听器。
watch([count, () => zhangsan.age], ([newCount, newAge], [oldCount, oldAge]) => {
  console.log('newCount =',newCount,'newZhangsanAge =', newAge, 'oldCount', oldCount, 'oldZhangsanAge =', oldAge);
});
```

### 立即监听

`watch()` 默认是懒侦听的，即仅在侦听源发生变化时才执行回调函数。最初绑定的时候是不会执行的，要等到所监听的值发生改变时才执行监听计算。

添加 `{ immediate: true }`  属性实现立即监听。

```js
watch(
  () => count.value,
  (newVal, oldVal) => {
    // 第一次调用时旧值是 undefined
   	console.log('count.value: ', newValue, oldValue);
  },
  { immediate: true }// 立即执行
)
```

### 深层侦听器

#### 监听响应式对象

直接给 `watch()` 传入一个响应式对象，会隐式地创建一个深层侦听器——该回调函数在所有嵌套的变更时都会被触发：

- 监听reactive对象，对象中任何一个值发生变化都会触发监听函数
- 注意：此处无法正确的获取oldValue，因为newValue和oldValue是同一个对象
- 注意：强制开启了深度监听（deep: false配置无效）

```js
const zhangsan = reactive({
  name: '张三',
  age: 18,
}); 


watch(
  zhangsan, 
  (newValue, oldValue) => {
    console.log(newValue === oldValue); // true
  },
  { deep: false }, // 设置deep无效
);

zhangsan.age++;
```

#### 监听getter函数

当使用 getter 函数作为源时，回调只在此函数的返回值变化时才会触发，一个返回响应式对象的 getter 函数：

- 只有在返回不同的对象时，才会触发回调
- 如果只是改变响应式对象中的某个属性的值，不会触发回调

```js
const zhangsan = reactive({
  name: '张三',
  age: 18,
  friend: {
    age: 20,
  }
}); 

watch(
  () => zhangsan.friend,
  () => {
    // 仅当 zhangsan.friend 被替换时触发
     console.log('zhangsan.friend: ', newValue, oldValue);
  }
);

zhangsan.friend = { age: 21 }; // 会触发watch监听，newValue 此处和 oldValue 是不相等的
zhangsan.friend.age++; // 不会触发watch监听
```

如果你想让回调在深层级变更时也能触发，你需要使用 `{ deep: true }` 强制侦听器进入深层级模式。

- 监听深度嵌套对象或数组中的属性变化时，需要 deep 选项设置为 true，强制转成深层侦听器
- 在深层级模式时，如果回调函数由于深层级的变更而被触发，那么新值和旧值将是同一个对象

给上面这个例子显式地加上 `deep` 选项：

```js
watch(
  () => zhangsan.friend,
  (newValue, oldValue) => {
    // 注意：newValue 此处和 oldValue 是相等的，除非 zhangsan.friend 被整个替换了
    console.log('zhangsan.friend: ', newValue, oldValue);
  }, 
  { deep: true }, // 此时设置deep有效
);

// 会触发watch监听，此时 newValue 此处和 oldValue 是相等的
zhangsan.friend.age++; 

// 会触发watch监听，此时 newValue 此处和 oldValue 是不相等的
zhangsan.friend = { age: 21 }; 
```

> 谨慎使用
>
> 深度侦听需要遍历被侦听对象中的所有嵌套的属性，当用于大型数据结构时，开销很大。因此请只在必要时才使用它，并且要留意性能。

## watchEffect

### 概念

watchEffect立即运行一个函数，同时响应式地追踪其依赖，并在依赖更改时重新执行。

- 第一个参数就是要运行的副作用函数。这个副作用函数的参数也是一个函数，用来注册清理回调。清理回调会在该副作用下一次执行前被调用，可以用来清理无效的副作用，例如等待中的异步请求 。

- 第二个参数是一个可选的选项，可以用来调整副作用的刷新时机或调试副作用的依赖。

默认情况下，侦听器将在组件渲染之前执行。设置 `flush: 'post'` 将会使侦听器延迟到组件渲染之后再执行。在某些特殊情况下 (例如要使缓存失效)，可能有必要在响应式依赖发生改变时立即触发侦听器。这可以通过设置 `flush: 'sync'` 来实现。然而，该设置应谨慎使用，因为如果有多个属性同时更新，这将导致一些性能和数据一致性的问题。

返回值是一个用来停止该副作用的函数。

### 基本使用

`watch()` 是懒执行的：仅当数据源变化时，才会执行回调。但在某些场景中，我们希望在创建侦听器时，立即执行一遍回调。

举例来说，我们想请求一些初始数据，然后在相关状态更改时重新请求数据。我们可以这样写：

```js
const url = ref('https://...');
const data = ref(null);

async function getData() {
  const response = await axios(url.value);
  data.value = await response.data;
}

// 立即获取
getData()
// ...再侦听 url 变化
watch(url, getData)
```

我们可以用 [`watchEffect` 函数](https://cn.vuejs.org/api/reactivity-core.html#watcheffect) 来简化上面的代码。`watchEffect()` 会立即执行一遍回调函数，如果这时函数产生了副作用，Vue 会自动追踪副作用的依赖关系，自动分析出响应源。上面的例子可以重写为：

```js
const url = ref('https://...');
const data = ref(null);

watchEffect(async () => {
   const response = await axios(url.value);
  data.value = await response.datal;
})
```

这个例子中，回调会立即执行。在执行期间，它会自动追踪 `url.value` 作为依赖（和计算属性的行为类似）。每当 `url.value` 变化时，回调会再次执行。

> TIP
>
> `watchEffect` 仅会在其**同步**执行期间，才追踪依赖。在使用异步回调时，只有在第一个 `await` 正常工作前访问到的属性才会被追踪。

### watch与watchEffect对比

`watch` 和 `watchEffect` 都能响应式地执行有副作用的回调。它们之间的主要区别是追踪响应式依赖的方式：

- `watch` 只追踪明确侦听的数据源。它不会追踪任何在回调中访问到的东西。另外，仅在数据源确实改变时才会触发回调。`watch` 会避免在发生副作用时追踪依赖，因此，我们能更加精确地控制回调函数的触发时机。
- `watchEffect`，则会在副作用发生期间追踪依赖。它会在同步执行过程中，自动追踪所有能访问到的响应式属性。这更方便，而且代码往往更简洁，但有时其响应性依赖关系会不那么明确。

与 `watchEffect()` 相比，`watch()` 使我们可以：

- 懒执行副作用；
- 更加明确是应该由哪个状态触发侦听器重新执行；
- 可以访问所侦听状态的前一个值和当前值。

```js
const count = ref(1);
const zhangsan = reactive({
  name: '张三',
  age: 18,
}); 

// 懒执行副作用，初始运行不会执行，当count.value的值发生变到时候才会执行
// 响应性依赖关系十分明确：更加明确是应该由状态count触发侦听器重新执行
// 可以访问所侦听状态的前一个值和当前值 newValue 和 oldValue
watch(count, (newValue, oldValue) => {
  console.log('count: ', newValue, oldValue);
  
  // zhangsan.age 的值发生变化不会执行执行副作用
  let age = zhangsan.age;
  console.log('watch', age);
});

// 立即执行一次副作用，初始运行执行一次
// 响应性依赖关系不那么明确：每当count.value 或者 zhangsan.age 的值发生变化时，执行一次
watchEffect(() => {
  let val = count.value;
  let age = zhangsan.age;
  console.log('watchEffect', val, age);
});
```

## 回调的触发时机

当你更改了响应式状态，它可能会同时触发 Vue 组件更新和侦听器回调。

默认情况下，用户创建的侦听器回调，都会在 Vue 组件更新**之前**被调用。这意味着你在侦听器回调中访问的 DOM 将是被 Vue 更新之前的状态。

如果想在侦听器回调中能访问被 Vue 更新**之后**的 DOM，你需要指明 `flush: 'post'` 选项：

```js
watch(
  count, 
  (newValue, oldValue) => {
    // 可以访问更新之后的DOM
    console.log('count: ', newValue, oldValue);
  }, 
 	{flush: 'post'}
);


watchEffect(
  () => {
    // 可以访问更新之后的DOM
		console.log('watchEffect', count.value);
	},
  { flush: 'post' }
);
```

后置刷新的 `watchEffect()` 有个更方便的别名 `watchPostEffect()`：

```js
import { watchPostEffect } from 'vue'

// watchPostEffect是 watchEffect 带有 flush: 'post' 选项的别名。
watchPostEffect(() => {
  /* 在 Vue 更新后执行 */
})
```

## 停止侦听器

在 `setup()` 或 `<script setup>` 中用同步语句创建的侦听器，会自动绑定到宿主组件实例上，并且会在宿主组件卸载时自动停止。因此，在大多数情况下，你无需关心怎么停止一个侦听器。

一个关键点是，侦听器必须用**同步**语句创建：如果用异步回调创建一个侦听器，那么它不会绑定到当前组件上，你必须手动停止它，以防内存泄漏。如下方这个例子：

```vue
<script setup>
import { watchEffect } from 'vue'

// 它会自动停止
watchEffect(() => {})

// ...这个则不会！
setTimeout(() => {
  watchEffect(() => {})
}, 100)
</script>
```

要手动停止一个侦听器，请调用 `watch` 或 `watchEffect` 返回的函数：

```js
const unwatch = watch(() => {})

// ...当该侦听器不再需要时
unwatch()
```

```js
const unwatch = watchEffect(() => {})

// ...当该侦听器不再需要时
unwatch()
```

注意，需要异步创建侦听器的情况很少，请尽可能选择同步创建。如果需要等待一些异步数据，你可以使用条件式的侦听逻辑：

```js
// 需要异步请求得到的数据
const data = ref(null)

watchEffect(() => {
  if (data.value) {
    // 数据加载后执行某些操作...
  }
})
```

## 监听数组的研究

方法一：使用ref声明数组，在组件模板中直接使用arr，在setup中需要使用arr.value

- 使用push等改变数组的元素 `arr.value.push(6);`

  - 监听 ref 数组`watch(arr, callback)`，当使用 push、splice等方法改变数组元素内容，需要加上 `{ deep: true }` 选项，才能触发watch，但此时nowValue和oldVlue输出的值是**一样的**
  - 使用getter函数监听数组 `watch(() => arr.value, callback)`，需要加上 `{ deep: true }` 选项，才能触发watch，此时nowValue和oldVlue输出的值是**一样的**

  - 使用getter函数，配合扩展运算符，监听一个新的数组 `watch(() => [...arr.value], callback)`，不需要加上 `{ deep: true }` 选项，也能触发watch，此时nowValue和oldVlue输出的值是**不一样的** **（推荐）**


- 给数组赋值新的对象 `arr.value = [11, 22, 33];`
  - 监听 ref 数组，不需要加上 `{ deep: true }` 选项，也能触发watch，此时nowValue和oldVlue输出的值是**不一样的**
  - 使用getter函数监听数组 `watch(() => arr.value, callback)`，不需要加上 `{ deep: true }` 选项，也能触发watch，此时nowValue和oldVlue输出的值是**不一样的**
  - 使用getter函数，配合扩展运算符，监听一个新的数组 `watch(() => [...arr.value], callback)`，不需要加上 `{ deep: true }` 选项，也能触发watch，此时nowValue和oldVlue输出的值是**不一样的**

```js
const arr = ref([1, 2, 3, 4, 5]);
nextTick(() => {
	arr.value.push(6);
	arr.value = [11, 22, 33];
});
```

```js
watch(
	arr,
	(n, o) => {
		console.log('arr1: ', n === o);
	},
	{ deep: true }
);

watch(
	() => arr.value,
	(n, o) => {
		console.log('arr2: ', n === o); 
	},
	{ deep: true }
);

watch(
	() => [...arr.value],
	(n, o) => {
		console.log('arr3: ', n === o); 
	}
);
```

方法二：使用reactive声明数组，使用的push方法添加新的元素

- 使用push等改变数组的元素 `arr.push(6);`

  - 监听 reactive 数组 `watch(arr, callback)`，当使用 push、splice等方法改变数组元素内容，不需要加上 `{ deep: true }` 选项，也能触发watch，但此时nowValue和oldVlue输出的值是**一样的**

  - 使用getter函数，配合扩展运算符，监听一个新的数组 `watch(() => [...arr], callback)`，不需要加上 `{ deep: true }` 选项，也能触发watch，此时nowValue和oldVlue输出的值是**不一样的** **（推荐）**


```js
const arr = reactive([]);
nextTick(() => {
	arr.push(6);
});
```

```js
watch(
	arr,
	(n, o) => {
		console.log('arr1: ', n, o, n === o); // true
	}
);

watch(
	() => [...arr],
	(n, o) => {
		console.log('arr3: ', n, o, n === o); // false
	}
);
```

方法三：创建一个响应式对象，对象的属性是数组，给该属性直接赋值

- 使用push等改变数组的元素 `obj.arr.push(6);`

  - 直接监听 reactive对象 `watch(obj, callback)`，当使用 push、splice等方法改变数组元素内容，不需要加上 `{ deep: true }` 选项，也能触发watch，但此时nowValue和oldVlue输出的值**是一样的**

  - 推荐使用扩展运算符 `[...obj.arr]`，使用getter函数监听一个新的数组 `watch(() => [...obj.arr], callback)`，不需要加上 `{ deep: true }` 选项，也能触发watch，此时nowValue和oldVlue输出的值是**不一样的** **（推荐）**

- 给数组赋值新的对象  `obj.arr = [11, 22, 33];`
  - 直接监听 reactive对象 `watch(obj, callback)`，当使用 push、splice等方法改变数组元素内容，不需要加上 `{ deep: true }` 选项，也能触发watch，但此时nowValue和oldVlue输出的值是**不一样的**
  - 直接使用getter函数监听数组 `watch(() => obj.arr, callback)`，不需要加上 `{ deep: true }` 选项，也能触发watch，此时nowValue和oldVlue输出的值是**不一样的**
  - 使用getter函数，配合扩展运算符，监听一个新的数组 `watch(() => [...obj.arr], callback)`，不需要加上 `{ deep: true }` 选项，也能触发watch，此时nowValue和oldVlue输出的值是**不一样的**

```js
const obj = reactive({
  arr: [1, 2, 3],
});


nextTick(() => {
	// obj.arr.push(6);
	obj.arr = [11, 22, 33];
});
```

```vue
<script setup>
import { reactive, ref, watch,nextTick } from 'vue';
// 引入子组件

const obj = reactive({
  arr: [1, 2, 3],
});

watch(
	obj,
	(n, o) => {
		console.log('arr1: ', n, o, n === o); // true
	},
);

watch(
	() => obj.arr,
	(n, o) => {
		console.log('arr2: ', n, o, n === o); // true
	},
);

watch(
	() => [...obj.arr],
	(n, o) => {
		console.log('arr3: ', n, o, n === o); // false
	},
);

nextTick(() => {
	// obj.arr.push(6);
	obj.arr = [11, 22, 33];
});

</script>
```

# 生命周期钩子

每个 Vue 组件实例在创建时都需要经历一系列的初始化步骤，比如设置好数据侦听，编译模板，挂载实例到 DOM，以及在数据改变时更新 DOM。在此过程中，它也会运行被称为生命周期钩子的函数，让开发者有机会在特定阶段运行自己的代码。

## 注册周期钩子

组合式API通过在生命周期钩子前面加上 “on” 来访问组件的生命周期钩子。

举例来说，`onMounted` 钩子可以用来在组件完成初始渲染并创建 DOM 节点后运行代码：

```vue
<script setup>
import { onMounted } from 'vue'

onMounted(() => {
  console.log(`the component is now mounted.`)
})
</script>
```

还有其他一些钩子，会在实例生命周期的不同阶段被调用，最常用的是 [`onMounted`](https://cn.vuejs.org/api/composition-api-lifecycle.html#onmounted)、[`onUpdated`](https://cn.vuejs.org/api/composition-api-lifecycle.html#onupdated) 和 [`onUnmounted`](https://cn.vuejs.org/api/composition-api-lifecycle.html#onunmounted)。

当调用 `onMounted` 时，Vue 会自动将回调函数注册到当前正被初始化的组件实例上。这意味着这些钩子应当在组件初始化时被**同步**注册。例如，请不要这样做：

```js
setTimeout(() => {
  onMounted(() => {
    // 异步注册时当前组件实例已丢失
    // 这将不会正常工作
  })
}, 100)
```

注意这并不意味着对 `onMounted` 的调用必须放在 `setup()` 或 `<script setup>` 内的词法上下文中。`onMounted()` 也可以在一个外部函数中调用，只要调用栈是同步的，且最终起源自 `setup()` 就可以。

## 生命周期图示

下面是实例生命周期的图表。你现在并不需要完全理解图中的所有内容，但以后它将是一个有用的参考。

![组件生命周期图示](https://cn.vuejs.org/assets/lifecycle.16e4c08e.png)

## 选项式和组合式对比

在组合式API中，因为 `setup` 是围绕 `beforeCreate` 和 `created` 生命周期钩子运行的，所以不需要显式地定义它们。换句话说，在`beforeCreate` 和 `created`钩子中编写的任何代码都应该直接在 setup 函数中编写。

| **Option API**  | Composition API （setup中） |
| --------------- | --------------------------- |
| beforeCreate    | 不需要                      |
| created         | 不需要                      |
| beforeMount     | onBeforeMount               |
| mounted         | onMounted                   |
| beforeUpdate    | onBeforeUpdate              |
| updated         | onUpdated                   |
| beforeUnmount   | onBeforeUnmount             |
| unmounted       | onUnmounted                 |
| errorCaptured   | onErrorCaptured             |
| renderTracked   | onRenderTracked             |
| renderTriggered | onRenderTriggered           |
| activated       | onActivated                 |
| deactivated     | onDeactivated               |

## 钩子函数详细信息

### onBeforeMount

 注册一个钩子，在组件被挂载之前被调用。

- 当这个钩子被调用时，组件已经完成了其响应式状态的设置，但还没有创建 DOM 节点。它即将首次执行 DOM 渲染过程。

### onMounted

注册一个回调函数，在组件挂载完成后执行。

组件在以下情况下被视为已挂载：

- 其所有同步子组件都已经被挂载 (不包含异步组件或 `<Suspense>` 树内的组件)。
- 其自身的 DOM 树已经创建完成并插入了父容器中。注意仅当根容器在文档中时，才可以保证组件 DOM 树也在文档中。

这个钩子通常用于执行需要访问组件所渲染的 DOM 树相关的副作用。

通过模板引用访问一个元素：

```vue
<script setup>
import { ref, onMounted } from 'vue'

const el = ref()

onMounted(() => {
  el.value // <div>
})
</script>

<template>
  <div ref="el"></div>
</template>
```

### onBeforeUpdate

注册一个钩子，在组件即将因为响应式状态变更而更新其 DOM 树之前调用。

- 这个钩子可以用来在 Vue 更新 DOM 之前访问 DOM 状态。在这个钩子中更改状态也是安全的。

### onUpdated

注册一个回调函数，在组件因为响应式状态变更而更新其 DOM 树之后调用。

- 父组件的更新钩子将在其子组件的更新钩子之后调用。

- 这个钩子会在组件的任意 DOM 更新后被调用，这些更新可能是由不同的状态变更导致的。如果你需要在某个特定的状态更改后访问更新后的 DOM，请使用 [nextTick()](https://cn.vuejs.org/api/general.html#nexttick) 作为替代。

> WARNING
>
> 不要在 updated 钩子中更改组件的状态，这可能会导致无限的更新循环！	

### onBeforeUnmount

注册一个钩子，在组件实例被卸载之前调用。

- 当这个钩子被调用时，组件实例依然还保有全部的功能。

### onUnmounted	

注册一个回调函数，在组件实例被卸载之后调用。

一个组件在以下情况下被视为已卸载：

- 其所有子组件都已经被卸载。
- 所有相关的响应式作用 (渲染作用以及 `setup()` 时创建的计算属性和侦听器) 都已经停止。

可以在这个钩子中手动清理一些副作用，例如计时器、DOM 事件监听器或者与服务器的连接。	

```vue
<script setup>
import { onMounted, onUnmounted } from 'vue'

let intervalId
onMounted(() => {
  intervalId = setInterval(() => {
    // ...
  })
})

onUnmounted(() => clearInterval(intervalId))
</script>
```

### onErrorCaptured

注册一个钩子，在捕获了后代组件传递的错误时调用。

错误可以从以下几个来源中捕获：

- 组件渲染
- 事件处理器
- 生命周期钩子
- `setup()` 函数
- 侦听器
- 自定义指令钩子
- 过渡钩子

这个钩子带有三个实参：错误对象、触发该错误的组件实例，以及一个说明错误来源类型的信息字符串。

你可以在 `errorCaptured()` 中更改组件状态来为用户显示一个错误状态。注意不要让错误状态再次渲染导致本次错误的内容，否则组件会陷入无限循环。

这个钩子可以通过返回 `false` 来阻止错误继续向上传递。请看下方的传递细节介绍。

**错误传递规则**

- 默认情况下，所有的错误都会被发送到应用级的 [`app.config.errorHandler`](https://cn.vuejs.org/api/application.html#app-config-errorhandler) (前提是这个函数已经定义)，这样这些错误都能在一个统一的地方报告给分析服务。
- 如果组件的继承链或组件链上存在多个 `errorCaptured` 钩子，对于同一个错误，这些钩子会被按从底至上的顺序一一调用。这个过程被称为“向上传递”，类似于原生 DOM 事件的冒泡机制。
- 如果 `errorCaptured` 钩子本身抛出了一个错误，那么这个错误和原来捕获到的错误都将被发送到 `app.config.errorHandler`。
- `errorCaptured` 钩子可以通过返回 `false` 来阻止错误继续向上传递。即表示“这个错误已经被处理了，应当被忽略”，它将阻止其他的 `errorCaptured` 钩子或 `app.config.errorHandler` 因这个错误而被调用。

### onRenderTracked

注册一个调试钩子，当组件渲染过程中追踪到响应式依赖时调用。

**这个钩子仅在开发模式下可用，且在服务器端渲染期间不会被调用。**

### onRenderTriggered

注册一个调试钩子，当响应式依赖的变更触发了组件渲染时调用。

**这个钩子仅在开发模式下可用，且在服务器端渲染期间不会被调用。**

### onActivated

注册一个回调函数，若组件实例是 [`<KeepAlive>`](https://cn.vuejs.org/api/built-in-components.html#keepalive) 缓存树的一部分，当组件被插入到 DOM 中时调用。

### onDeactivated

注册一个回调函数，若组件实例是 [`<KeepAlive>`](https://cn.vuejs.org/api/built-in-components.html#keepalive) 缓存树的一部分，当组件从 DOM 中被移除时调用。

# 模板引用

虽然 Vue 的声明性渲染模型为你抽象了大部分对 DOM 的直接操作，但在某些情况下，我们仍然需要直接访问底层 DOM 元素。要实现这一点，我们可以使用特殊的 `ref` attribute：

```html
<input ref="inputRef">
```

`ref` 是一个特殊的 attribute，和 `v-for` 中提到的 `key` 类似。它允许我们在一个特定的 DOM 元素或子组件实例被挂载后，获得对它的直接引用。这可能很有用，比如说在组件挂载时将焦点设置到一个 input 元素上，或在一个元素上初始化一个第三方库。

`ref` 用于注册元素或子组件的引用。

- 使用选项式 API，引用将被注册在组件的 `this.$refs` 对象里
- 使用组合式 API，引用将存储在与名字匹配的 `ref` 里
- 如果用于普通 DOM 元素，引用将是元素本身；如果用于子组件，引用将是子组件的实例。
-  `ref` 可以接收一个函数值，用于对存储引用位置的完全控制：

关于 ref 注册时机的重要说明：因为 ref 本身是作为渲染函数的结果来创建的，必须等待组件挂载后才能对它进行访问。

`this.$refs` 也是非响应式的，因此你不应该尝试在模板中使用它来进行数据绑定。

## 访问一个组件或者元素

- 为了通过组合式 API 获得该模板引用，我们需要声明一个同名的 ref
- 你只可以**在组件挂载后**才能访问模板引用，在 `onMounted` 或者 `nextTick` 中都可以
- 如果你想在模板中的表达式上访问 `inputRef`，在初次渲染时会是 `null`。这是因为在初次渲染前这个元素还不存在呢！

```vue
<script setup>
  import { ref, onMounted } from 'vue'

  // 声明一个 ref 来存放该元素的引用
  // 必须和模板里的 ref 同名
  const inputRef = ref(null);
  const childRef = ref(null);

  // 组件渲染后才能访问 inputRef.value，否则 inputRef.value 还没有被赋值
  onMounted(() => {
    inputRef.value.focus()
  });

  // 或者在 nextTick 中访问
  nextTick(() => {
    console.log(inputRef.value);
    console.log(childRef.value);
  });  
</script>

<template>
  <input ref="inputRef" />
  <child ref='childRef'/>
</template>
```

如果不使用 `<script setup>`，需确保从 `setup()` 返回 ref：

```js
export default {
  setup() {
    const inputRef = ref(null)
    // ...
    return {
      inputRef
    }
  }
}
```

## 访问多个组件或者元素

> 需要 v3.2.25 及以上版本

当在 `v-for` 中使用模板引用时，对应的 ref 中包含的值是一个数组，它将在元素被挂载后包含对应整个列表的所有元素：

- 这种情况仅适用于 v-for `循环数是固定的情况` 
- 因为如果 v-for `循环数` 在初始化之后发生改变，那么就会导致 childRefs 再一次重复添加，childRefs 中会出现重复的子组件实例

```vue
<script setup>
import { ref, onMounted } from 'vue'

const list = ref([1, 2, 3, 4, 5]);

// 子组件或者元素实例数组
const itemRefs = ref([]);

onMounted(() => console.log(itemRefs.value));
</script>

<template>
  <ul>
    <li v-for="item in list" ref="itemRefs">
      {{ item }}
    </li>
  </ul>
</template>
```

应该注意的是，ref 数组**并不**保证与源数组相同的顺序。

## 函数模板引用

除了使用字符串值作名字，`ref` attribute 还可以绑定为一个函数，会在每次组件更新时都被调用。该函数会收到元素引用作为其第一个参数：

```html
<input :ref="(el) => { /* 将 el 赋值给一个数据属性或 ref 变量 */ }">
```

注意我们这里需要使用动态的 `:ref` 绑定才能够传入一个函数。当绑定的元素被卸载时，函数也会被调用一次，此时的 `el` 参数会是 `null`。你当然也可以绑定一个组件方法而不是内联函数。

```vue
<script setup>
import { ref, onMounted } from 'vue';

// 声明一个 ref 来存放该元素的引用
const input1 = ref(null);
const input2 = ref(null);
    
function setInput(el){
  input2.value = el;
}

onMounted(() => {
	input1.value.focus();
});
</script>

<template>
	<!-- 1、使用内联函数 -->
	<input :ref="el => (input1 = el)" />
	<!-- 2、使用组件函数 -->
	<input :ref="setInput" />
</template>
```

## 组件上的 ref

模板引用也可以被用在一个子组件上。这种情况下引用中获得的值是组件实例：

```vue
<script setup>
import { ref, onMounted } from 'vue'
import Child from './Child.vue'

const child = ref(null)

onMounted(() => {
  // child.value 是 <Child /> 组件的实例
})
</script>

<template>
  <Child ref="child" />
</template>
```

如果一个子组件使用的是选项式 API 或没有使用 `<script setup>`，被引用的组件实例和该子组件的 `this` 完全一致，这意味着父组件对子组件的每一个属性和方法都有完全的访问权。这使得在父组件和子组件之间创建紧密耦合的实现细节变得很容易，当然也因此，应该只在绝对需要时才使用组件引用。大多数情况下，你应该首先使用标准的 props 和 emit 接口来实现父子组件交互。

有一个例外的情况，使用了 `<script setup>` 的组件是**默认私有**的：一个父组件无法访问到一个使用了 `<script setup>` 的子组件中的任何东西，除非子组件在其中通过 `defineExpose` 宏显式暴露：

```vue
<script setup>
import { ref } from 'vue'

const a = 1
const b = ref(2)

defineExpose({
  a,
  b
})
</script>
```

当父组件通过模板引用获取到了该组件的实例时，得到的实例类型为 `{ a: number, b: number }` (ref 都会自动解包，和一般的实例一样)。

## 侦听模板引用

在声明周期钩子 onMounted 中可以访问模板引用，如果需要侦听一个模板引用 ref 的变化，确保考虑到其值为 `null` 的情况：

```js
watchEffect(() => {
  if (inputRef.value) {
    inputRef.value.focus()
  } else {
    // 此时还未挂载，或此元素已经被卸载（例如通过 v-if 控制）
  }
})
```

`watchEffect()` 在 DOM 挂载或更新之前运行副作用，所以当侦听器运行时，模板引用还未被更新。因此，使用模板引用的侦听器应该用 `{ flush: 'post' }` 选项来定义，这将在 DOM 更新后运行副作用，确保模板引用与 DOM 保持同步，并引用正确的元素。

```js
watchEffect(
	() => {
    inputRef.value.focus();
		console.log('inputRef.value: ', inputRef.value);
	},
	{ flush: 'post' }
);
```

或者直接使用`watchEffect(()=>{ }, { flush: 'post' })` 的别名函数 `watchPostEffect()`:

```js
watchPostEffect(() => {
  inputRef.value.focus();
  console.log('inputRef.value: ', inputRef.value);
});
```

# 依赖注入

## Prop 逐级透传问题

通常情况下，当我们需要从父组件向子组件传递数据时，会使用 [props](https://cn.vuejs.org/guide/components/props.html)。想象一下这样的结构：有一些多层级嵌套的组件，形成了一颗巨大的组件树，而某个深层的子组件需要一个较远的祖先组件中的部分数据。在这种情况下，如果仅使用 props 则必须将其沿着组件链逐级传递下去，这会非常麻烦：

![Prop 逐级透传的过程图示](https://cn.vuejs.org/assets/prop-drilling.11201220.png)

注意，虽然这里的 `<Footer>` 组件可能根本不关心这些 props，但为了使 `<DeepChild>` 能访问到它们，仍然需要定义并向下传递。如果组件链路非常长，可能会影响到更多这条路上的组件。这一问题被称为“prop 逐级透传”，显然是我们希望尽量避免的情况。

`provide` 和 `inject` 可以帮助我们解决这一问题。一个父组件相对于其所有的后代组件，会作为**依赖提供者**。任何后代的组件树，无论层级有多深，都可以**注入**由父组件提供给整条链路的依赖。

![Provide/inject 模式](https://cn.vuejs.org/assets/provide-inject.3e0505e4.png)

## Provide (提供)

### 概念

`provide()` 函数提供一个值，可以被后代组件注入。

`provide(注入名, 值)`接收两个参数：

- 第一个参数是要注入的 key，被称为**注入名**
  - 注入名可以是一个字符串或是一个 `Symbol`

  - 后代组件会用注入名来查找期望注入的值：`inject(注入名)`

  - 一个组件可以多次调用 `provide()`，使用不同的注入名，注入不同的依赖值。

- 第二个参数是提供的值
  - 提供的值可以是任意类型，包括普通变量，响应式的状态ref、reactive，函数等
  - 提供的响应式状态使后代组件可以由此和提供者建立响应式的联系。


与注册生命周期钩子的 API 类似，`provide()` 必须在组件的 `setup()` 阶段同步调用。

### 在组件中使用 Provide

在`<script setup>`中使用，给子孙组件注入数据：

```vue 
<script setup>
import { provide } from 'vue'

// 1、非响应式的 provide
let num = 6;
provide('num', num);
provide('message', 'hello!');

// 2、响应式的provide：
// 为了增加 provide 值和 inject 值之间的响应性，我们可以在 provide 值时使用 ref 或 reactive。
const count = ref(10);
const zhangsan = reactive({ name: '张三', age: 18 });
provide('count', count);
provide('zhangsan', zhangsan);
  
</script>
```

如果不使用 `<script setup>`，请确保 `provide()` 是在 `setup()` 同步调用的：

```js
import { provide } from 'vue'

export default {
  setup() {
    provide('message', 'hello!')
  }
}
```

### 应用层 Provide

除了在一个组件中提供依赖，我们还可以在整个应用层面提供依赖：

```js
import { createApp } from 'vue'

const app = createApp({})

app.provide('message', 'hello!')
```

在应用级别提供的数据在该应用内的所有组件中都可以注入。这在你编写[插件](https://cn.vuejs.org/guide/reusability/plugins.html)时会特别有用，因为插件一般都不会使用组件形式来提供值。

## Inject (注入)

### 概念

`inject()` 注入一个由祖先组件或整个应用 (通过 `app.provide()`) 提供的值。

- 第一个参数是注入的 key。
  - Vue 会遍历父组件链，通过匹配 key 来确定所提供的值。
  - 如果父组件链上多个组件对同一个 key 提供了值，那么离得更近的组件将会“覆盖”链上更远的组件所提供的值。
  - 如果没有能通过 key 匹配到值，`inject()` 将返回 `undefined`，除非提供了一个默认值。

- 第二个参数是可选的，即在没有匹配到 key 时使用的默认值。
  - 它也可以是一个工厂函数，用来返回某些创建起来比较复杂的值。
- 第三个参数是可选的，如果默认值本身就是一个函数，那么你必须将 `false` 作为第三个参数传入，表明这个函数就是默认值，而不是一个工厂函数。

与注册生命周期钩子的 API 类似，`inject()` 必须在组件的 `setup()` 阶段同步调用。

### 在组件中使用 Inject 

在`<script setup>`中使用，注入祖先组件提供的数据：

```vue
<script setup>
import { inject, toRefs} from 'vue'
  
// 注入非响应式数据
const num = inject('num');
const message = inject('message');

// 注入ref
const count = inject('count');
  
// 注入reactive
const { age } = toRefs(inject('zhangsan'));
</script>
```

注入数据的注意事项：

- 如果提供的值是非响应式数据，可以直接使用

- 如果提供的值是一个 ref，注入进来的会是该 ref 对象，而**不会**自动解包为其内部的值。这使得注入方组件能够通过 ref 对象保持了和供给方的响应性链接。
- 如果提供的值是一个reactive，注入进来的也是该reactive对象，注入方组件能够通过 reactive 对象保持了和供给方的响应性链接。如果想解构该对象，需要使用 toRefs

同样的，如果没有使用 `<script setup>`，`inject()` 需要在 `setup()` 内同步调用：

```js
import { inject } from 'vue'

export default {
  setup() {
    const message = inject('message')
    return { message }
  }
}
```

### 注入默认值

默认情况下，`inject` 假设传入的注入名会被某个祖先链上的组件提供。如果该注入名的确没有任何组件提供，则会抛出一个运行时警告。

如果在注入一个值时不要求必须有提供者，那么我们应该声明一个默认值，和 props 类似：

```js
// 如果没有祖先组件提供 "message"
// value 会是 "这是默认值"
const value = inject('message', '这是默认值')
```

在一些场景中，默认值可能需要通过调用一个函数或初始化一个类来取得。为了避免在用不到默认值的情况下进行不必要的计算或产生副作用，我们可以使用工厂函数来创建默认值：

```js
const value = inject('key', () => new ExpensiveClass())
```

## 和响应式数据配合使用

当提供 / 注入响应式的数据时，**建议尽可能将任何对响应式状态的变更都保持在供给方组件中**。这样可以确保所提供状态的声明和变更操作都内聚在同一个组件内，使其更容易维护。

有的时候，我们可能需要在注入方组件中更改数据。在这种情况下，我们推荐在供给方组件内声明并提供一个更改数据的方法函数：

```vue
<!-- 在供给方组件内 -->
<script setup>
import { provide, ref } from 'vue'

const location = ref('North Pole')

function updateLocation() {
  location.value = 'South Pole'
}

provide('location', {
  location,
  updateLocation
})
</script>
```

```vue
<!-- 在注入方组件 -->
<script setup>
import { inject } from 'vue'

const { location, updateLocation } = inject('location')
</script>

<template>
  <button @click="updateLocation">{{ location }}</button>
</template>
```

最后，如果你想确保提供的数据不能被注入方的组件更改，你可以使用 `readonly()`来包装提供的值。

```vue
<script setup>
import { ref, provide, readonly } from 'vue'

const count = ref(0)
provide('read-only-count', readonly(count))
</script>
```

## 使用 Symbol 作注入名

至此，我们已经了解了如何使用字符串作为注入名。但如果你正在构建大型的应用，包含非常多的依赖提供，或者你正在编写提供给其他开发者使用的组件库，建议最好使用 Symbol 来作为注入名以避免潜在的冲突。

我们通常推荐在一个单独的文件中导出这些注入名 Symbol：

```js
// keys.js
export const myInjectionKey = Symbol()
```

```js
// 在供给方组件中
import { provide } from 'vue'
import { myInjectionKey } from './keys.js'

provide(myInjectionKey, { /*
  要提供的数据
*/ });
```

```js
// 注入方组件
import { inject } from 'vue'
import { myInjectionKey } from './keys.js'

const injected = inject(myInjectionKey)
```

# 插槽

## 子组件

```vue
<script setup>
import { useSlots, reactive } from 'vue';
const zhangsan = reactive({
	name: '张三',
	age: '18',
});

const slots = useSlots();
// 匿名插槽使用情况
console.log('slots.default: ', slots.default());
// 具名插槽使用情况
console.log('slots.title: ', slots.title());
</script>

<template>
	<!-- 匿名插槽 -->
	<slot />
	<!-- 具名插槽 -->
	<slot name="title" />
	<!-- 作用域插槽 -->
	<slot name="footer" :scope="zhangsan" />
</template>
```

## 父组件

```vue 
<script setup>
// 引入子组件
import Child from './Child.vue';
</script>

<template>
	<Child>
		<!-- 匿名插槽 -->
		<span>我是默认插槽</span>
		<!-- 具名插槽 -->
		<template #title>
			<h1>我是具名插槽</h1>
			<h1>我是具名插槽</h1>
			<h1>我是具名插槽</h1>
		</template>
		<!-- 作用域插槽 -->
		<template #footer="{ scope }">
			<footer>作用域插槽——姓名：{{ scope.name }}，年龄{{ scope.age }}</footer>
		</template>
	</Child>
</template>
```

# setup语法糖

`<script setup>` 是在单文件组件 (SFC) 中使用组合式 API 的编译时语法糖。当同时使用 SFC 与组合式 API 时该语法是默认推荐。相比于普通的 `<script>` 语法，它具有更多优势：

- 更少的样板内容，更简洁的代码。
- 能够使用纯 TypeScript 声明 props 和自定义事件。
- 更好的运行时性能 (其模板会被编译成同一作用域内的渲染函数，避免了渲染上下文代理对象)。
- 更好的 IDE 类型推导性能 (减少了语言服务器从代码中抽取类型的工作)。

## 基本语法

要启用该语法，需要在 `<script>` 代码块上添加 `setup` attribute：

```vue
<script setup>
console.log('hello script setup')
</script>
```

里面的代码会被编译成组件 `setup()` 函数的内容。这意味着与普通的 `<script>` 只在组件被首次引入的时候执行一次不同，`<script setup>` 中的代码会在**每次组件实例被创建的时候执行**。

当使用 `<script setup>` 的时候，任何在 `<script setup>` 声明的**顶层的绑定** (包括变量，函数声明，以及 import 导入的内容) 都能在模板中直接使用：

```vue
<script setup>

// 变量
const msg = 'Hello!'
  
// 函数
function log() {
  console.log(msg)
}
</script>

<template>
  <button @click="log">{{ msg }}</button>
</template>
```

import 导入的内容也会以同样的方式暴露。这意味着我们可以在模板表达式中直接使用导入的 helper 函数，而不需要通过 `methods` 选项来暴露它：

```vue
<script setup>
import { capitalize } from './helpers'
</script>

<template>
  <div>{{ capitalize('hello') }}</div>
</template>
```

## 响应式

响应式状态需要明确使用[响应式 API](https://cn.vuejs.org/api/reactivity-core.html) 来创建。和 `setup()` 函数的返回值一样，ref 在模板中使用的时候会自动解包：

```vue
<script setup>
import { ref, reactive } from 'vue';

// 变量
const msg = 'Hello!'
const count = ref(10);
const zhangsan = reactive({ name: '张三', age: 18 });
  
</script>

<template>
  <button @click="count++">{{ count }}</button>
	{{zhangsan.age}}
</template>
```

## 使用组件

`<script setup>` 范围里的值也能被直接作为自定义组件的标签名使用：

```vue
<script setup>
import MyComponent from './MyComponent.vue'
</script>

<template>
  <MyComponent />
</template>
```

这里 `MyComponent` 应当被理解为像是在引用一个变量。如果你使用过 JSX，此处的心智模型是类似的。其 kebab-case 格式的 `<my-component>` 同样能在模板中使用——不过，我们强烈建议使用 PascalCase 格式以保持一致性。同时这也有助于区分原生的自定义元素。

### 动态组件

由于组件是通过变量引用而不是基于字符串组件名注册的，在 `<script setup>` 中要使用动态组件的时候，应该使用动态的 `:is` 来绑定：

```vue
<script setup>
import Foo from './Foo.vue'
import Bar from './Bar.vue'
</script>

<template>
  <component :is="Foo" />
  <component :is="someCondition ? Foo : Bar" />
</template>
```

请注意组件是如何在三元表达式中被当做变量使用的。

### 递归组件

一个单文件组件可以通过它的文件名被其自己所引用。例如：名为 `FooBar.vue` 的组件可以在其模板中用 `<FooBar/>` 引用它自己。

请注意这种方式相比于导入的组件优先级更低。如果有具名的导入和组件自身推导的名字冲突了，可以为导入的组件添加别名：

```js
import { FooBar as FooBarChild } from './components'
```

### 命名空间组件

可以使用带 `.` 的组件标签，例如 `<Foo.Bar>` 来引用嵌套在对象属性中的组件。这在需要从单个文件中导入多个组件的时候非常有用：

```vue
<script setup>
import * as Form from './form-components'
</script>

<template>
  <Form.Input>
    <Form.Label>label</Form.Label>
  </Form.Input>
</template>
```

## 使用自定义指令

全局注册的自定义指令将正常工作。本地的自定义指令在 `<script setup>` 中不需要显式注册，但他们必须遵循 `vNameOfDirective` 这样的命名规范：

```vue
<script setup>
const vMyDirective = {
  beforeMount: (el) => {
    // 在元素上做些操作
  }
}
</script>
<template>
  <h1 v-my-directive>This is a Heading</h1>
</template>
```

如果指令是从别处导入的，可以通过重命名来使其符合命名规范：

```vue
<script setup>
import { myDirective as vMyDirective } from './MyDirective.js'
</script>
```

## defineProps()

在 `<script setup>` 中，使用 `defineProps` 来代替 `props` 选项，实现父组件向子组件传值：

- `defineProps` 不需要导入，可以直接在 `<script setup>` 中使用

- `defineProps` 接收与 `props` 选项相同的值
- `defineProps` 在选项传入后，会提供恰当的类型推导。
- 传入到 `defineProps` 的选项会从 setup 中提升到模块的作用域。因此，传入的选项不能引用在 setup 作用域中声明的局部变量。这样做会引起编译错误。但是，它*可以*引用导入的绑定，因为它们也在模块作用域内。

父组件：

```vue
<script setup>
// 引入子组件
import child from './child.vue'
import { ref } from 'vue';

const count = ref(10);
</script>

<template>
  <child :count='count'/>  
</template>
```

子组件：

```vue
<script setup>
import { toRefs } from 'vue';  
  
// 声明props
const props = defineProps([ 'count' ]);
// 或者
const props = defineProps({
  count: Number,
});

// 1、在setup内解构 props 需要使用toRefs，或者使用props.count
// 2、在组件模板中可以使用props.count，也可以直接使用count
//const { count } = toRefs(props);
//console.log('count: ', count);

</script>

<template>
  <p>props.count:{{ props.count }}</p>
  <p>count:{{ count }}</p>
</template>
```

## defineEmits()

在 `<script setup>` 中，使用 `defineEmits` 来代替 `emits` 选项，实现子组件发射自定义事件：

- `defineEmits` 不需要导入，可以直接在 `<script setup>` 中使用

- `defineEmits` 接收与 `emits` 选项相同的值。

   `defineEmits` 在选项传入后，会提供恰当的类型推导。

- 传入到 `defineEmits` 的选项会从 setup 中提升到模块的作用域。因此，传入的选项不能引用在 setup 作用域中声明的局部变量。这样做会引起编译错误。但是，它*可以*引用导入的绑定，因为它们也在模块作用域内。

父组件

```vue
<script setup>
// 引入子组件
import child from './child.vue'
import { ref } from 'vue';

const count = ref(0);
</script>

<template>
	count: {{count}}
  <child @customClick="count = $event"/>  
</template>
```

子组件

```vue
<script setup>
import { ref } from 'vue';

const num = ref(6);
  
  
// 发射事件
const emit = defineEmits(['customClick']);
const btnClick = () => {
	emit('customClick', num.value);
};

</script>

<template>
	<button @click="btnClick">发射事件</button>
  <button @click="$emit('customClick', num)">发射事件</button>
</template>
```

## defineExpose()

在标准组件写法里，子组件的数据都是默认隐式暴露给父组件的，但使用 `<script setup>` 的组件，所有数据只是默认 `return` 给组件的模板使用，不会暴露到组件外，所以父组件是无法直接通过挂载 `ref` 变量获取子组件的数据 或者 `$parent` 链获取到父组件的数据。

如果要调用组件的数据，需要先在组件显示的暴露出来，才能够正确的拿到，这个操作，就是由 `defineExpose` 来完成。

- `defineEmits` 不需要导入，可以直接在 `<script setup>` 中使用
- 通过 `defineExpose` 来显式指定在 `<script setup>` 组件中要暴露出去的属性

子组件

```vue
<script setup>
import { reactive, ref, toRefs } from 'vue';

const num = 1;
const count = ref(2);
const zhangsan = reactive({ name: '张三' });

// 将方法、变量暴露给父组件使用，父组件才可通过ref API拿到子组件暴露的数据
defineExpose({
	num,
	count,
	// 解构 zhangsan
	...toRefs(zhangsan),
	// 声明方法
	changeName() {
		zhangsan.name = '小三';
	},
});
</script>

<template>
	<span>{{ zhangsan.name }}</span>
</template>
```

父组件

```vue
<script setup>
import { nextTick, ref } from 'vue';
// 引入子组件
import child from './child.vue';

// 子组件ref
const childRef = ref();

// 当父组件通过模板引用的方式获取到当前组件的实例，获取到的实例的ref 会和在普通实例中一样被自动解包
nextTick(() => {
	// 获取子组件属性
	console.log(childRef.value.num); // 1
	console.log(childRef.value.count); // 2   count自动解包
	console.log(childRef.value.name); // 张三  name自动解包
	// 执行子组件方法
	childRef.value.changeName();
	console.log(childRef.value.name); // 小三
});
</script>

<template>
	<child ref="childRef" />
</template>
```

## useSlots() 和 useAttrs()

在 `<script setup>` 使用 `slots` 和 `attrs` 的情况应该是相对来说较为罕见的，因为可以在模板中直接通过 `$slots` 和 `$attrs` 来访问它们。在你的确需要使用它们的罕见场景中，可以分别用 `useSlots` 和 `useAttrs` 两个辅助函数：

```vue
<script setup>
import { useSlots, useAttrs } from 'vue'

const slots = useSlots()
const attrs = useAttrs()
</script>
```

`useSlots` 和 `useAttrs` 是真实的运行时函数，它的返回与 `setupContext.slots` 和 `setupContext.attrs` 等价。它们同样也能在普通的组合式 API 中使用。

## 与普通的 `<script>` 一起使用

`<script setup>` 可以和普通的 `<script>` 一起使用。普通的 `<script>` 在有这些需要的情况下或许会被使用到：

- 声明无法在 `<script setup>` 中声明的选项，例如 `inheritAttrs` 或插件的自定义选项。
- 声明模块的具名导出 (named exports)。
- 运行只需要在模块作用域执行一次的副作用，或是创建单例对象。

```vue
<script>
// 普通 <script>, 在模块作用域下执行 (仅一次)
runSideEffectOnce()

// 声明额外的选项
export default {
  inheritAttrs: false,
  customOptions: {}
}
</script>

<script setup>
// 在 setup() 作用域中执行 (对每个实例皆如此)
</script>
```

## 顶层 `await`

`<script setup>` 中可以使用顶层 `await`。结果代码会被编译成 `async setup()`：

```vue
<script setup>
const post = await fetch(`/api/post/1`).then((r) => r.json())
</script>
```

另外，await 的表达式会自动编译成在 `await` 之后保留当前组件实例上下文的格式。

> 注意
>
> `async setup()` 必须与 [`Suspense` 内置组件](https://cn.vuejs.org/guide/built-ins/suspense.html)组合使用，`Suspense` 目前还是处于实验阶段的特性，会在将来的版本中稳定。

## 限制

由于模块执行语义的差异，`<script setup>` 中的代码依赖单文件组件的上下文。当将其移动到外部的 `.js` 或者 `.ts` 文件中的时候，对于开发者和工具来说都会感到混乱。因此，**`<script setup>`** 不能和 `src` attribute 一起使用。

# 组合式函数 hook

## 什么是“组合式函数”？

在 Vue 应用的概念中，“组合式函数”(Composables) 是一个利用 Vue 的组合式 API 来封装和复用**有状态逻辑**的函数。

当构建前端应用时，我们常常需要复用公共任务的逻辑。例如为了在不同地方格式化时间，我们可能会抽取一个可复用的日期格式化函数。这个函数封装了**无状态的逻辑**：它在接收一些输入后立刻返回所期望的输出。复用无状态逻辑的库有很多，比如你可能已经用过的 [lodash](https://lodash.com/) 或是 [date-fns](https://date-fns.org/)。

相比之下，有状态逻辑负责管理会随时间而变化的状态。一个简单的例子是跟踪当前鼠标在页面中的位置。在实际应用中，也可能是像触摸手势或与数据库的连接状态这样的更复杂的逻辑。

## 鼠标跟踪器示例

如果我们要直接在组件中使用组合式 API 实现鼠标跟踪功能，它会是这样的：

```vue
<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const x = ref(0)
const y = ref(0)

function update(event) {
  x.value = event.pageX
  y.value = event.pageY
}

onMounted(() => window.addEventListener('mousemove', update))
onUnmounted(() => window.removeEventListener('mousemove', update))
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```

但是，如果我们想在多个组件中复用这个相同的逻辑呢？我们可以把这个逻辑以一个组合式函数的形式提取到外部文件中：

```js
// mouse.js
import { ref, onMounted, onUnmounted } from 'vue'

// 按照惯例，组合式函数名以“use”开头
export function useMouse() {
  // 被组合式函数封装和管理的状态
  const x = ref(0)
  const y = ref(0)

  // 组合式函数可以随时更改其状态。
  function update(event) {
    x.value = event.pageX
    y.value = event.pageY
  }

  // 一个组合式函数也可以挂靠在所属组件的生命周期上
  // 来启动和卸载副作用
  onMounted(() => window.addEventListener('mousemove', update))
  onUnmounted(() => window.removeEventListener('mousemove', update))

  // 通过返回值暴露所管理的状态
  return { x, y }
}
```

下面是它在组件中使用的方式：

```vue
<script setup>
import { useMouse } from './mouse.js'

const { x, y } = useMouse()
</script>

<template>Mouse position is at: {{ x }}, {{ y }}</template>
```

如你所见，核心逻辑完全一致，我们做的只是把它移到一个外部函数中去，并返回需要暴露的状态。和在组件中一样，你也可以在组合式函数中使用所有的[组合式 API](https://cn.vuejs.org/api/#composition-api)。现在，`useMouse()` 的功能可以在任何组件中轻易复用了。

更酷的是，你还可以嵌套多个组合式函数：一个组合式函数可以调用一个或多个其他的组合式函数。这使得我们可以像使用多个组件组合成整个应用一样，用多个较小且逻辑独立的单元来组合形成复杂的逻辑。实际上，这正是为什么我们决定将实现了这一设计模式的 API 集合命名为组合式 API。

举例来说，我们可以将添加和清除 DOM 事件监听器的逻辑也封装进一个组合式函数中：

```js
// event.js
import { onMounted, onUnmounted } from 'vue'

export function useEventListener(target, event, callback) {
  // 如果你想的话，
  // 也可以用字符串形式的 CSS 选择器来寻找目标 DOM 元素
  onMounted(() => target.addEventListener(event, callback))
  onUnmounted(() => target.removeEventListener(event, callback))
}
```

有了它，之前的 `useMouse()` 组合式函数可以被简化为：

```js
// mouse.js
import { ref } from 'vue'
import { useEventListener } from './event'

export function useMouse() {
  const x = ref(0)
  const y = ref(0)

  useEventListener(window, 'mousemove', (event) => {
    x.value = event.pageX
    y.value = event.pageY
  })

  return { x, y }
}
```

## 异步状态示例

`useMouse()` 组合式函数没有接收任何参数，因此让我们再来看一个需要接收一个参数的组合式函数示例。在做异步数据请求时，我们常常需要处理不同的状态：加载中、加载成功和加载失败。

```vue
<script setup>
import { ref } from 'vue'

const data = ref(null)
const error = ref(null)

fetch('...')
  .then((res) => res.json())
  .then((json) => (data.value = json))
  .catch((err) => (error.value = err))
</script>

<template>
  <div v-if="error">Oops! Error encountered: {{ error.message }}</div>
  <div v-else-if="data">
    Data loaded:
    <pre>{{ data }}</pre>
  </div>
  <div v-else>Loading...</div>
</template>
```

如果在每个需要获取数据的组件中都要重复这种模式，那就太繁琐了。让我们把它抽取成一个组合式函数：

```js
// fetch.js
import { ref } from 'vue'

export function useFetch(url) {
  const data = ref(null)
  const error = ref(null)

  fetch(url)
    .then((res) => res.json())
    .then((json) => (data.value = json))
    .catch((err) => (error.value = err))

  return { data, error }
}
```

现在我们在组件里只需要：

```vue
<script setup>
import { useFetch } from './fetch.js'

const { data, error } = useFetch('...')
</script>
```

`useFetch()` 接收一个静态的 URL 字符串作为输入，所以它只执行一次请求，然后就完成了。但如果我们想让它在每次 URL 变化时都重新请求呢？那我们可以让它同时允许接收 ref 作为参数：

```js
// fetch.js
import { ref, isRef, unref, watchEffect } from 'vue'

export function useFetch(url) {
  const data = ref(null)
  const error = ref(null)

  function doFetch() {
    // 在请求之前重设状态...
    data.value = null
    error.value = null
    // unref() 解包可能为 ref 的值
    fetch(unref(url))
      .then((res) => res.json())
      .then((json) => (data.value = json))
      .catch((err) => (error.value = err))
  }

  if (isRef(url)) {
    // 若输入的 URL 是一个 ref，那么启动一个响应式的请求
    watchEffect(doFetch)
  } else {
    // 否则只请求一次
    // 避免监听器的额外开销
    doFetch()
  }

  return { data, error }
}
```

这个版本的 `useFetch()` 现在同时可以接收静态的 URL 字符串和 URL 字符串的 ref。当通过 [`isRef()`](https://cn.vuejs.org/api/reactivity-utilities.html#isref) 检测到 URL 是一个动态 ref 时，它会使用 [`watchEffect()`](https://cn.vuejs.org/api/reactivity-core.html#watcheffect) 启动一个响应式的 effect。该 effect 会立刻执行一次，并在此过程中将 URL 的 ref 作为依赖进行跟踪。当 URL 的 ref 发生改变时，数据就会被重置，并重新请求。

这里是一个[升级版的 `useFetch()`](https://sfc.vuejs.org/#eyJBcHAudnVlIjoiPHNjcmlwdCBzZXR1cD5cbmltcG9ydCB7IHJlZiwgY29tcHV0ZWQgfSBmcm9tICd2dWUnXG5pbXBvcnQgeyB1c2VGZXRjaCB9IGZyb20gJy4vdXNlRmV0Y2guanMnXG5cbmNvbnN0IGJhc2VVcmwgPSAnaHR0cHM6Ly9qc29ucGxhY2Vob2xkZXIudHlwaWNvZGUuY29tL3RvZG9zLydcbmNvbnN0IGlkID0gcmVmKCcxJylcbmNvbnN0IHVybCA9IGNvbXB1dGVkKCgpID0+IGJhc2VVcmwgKyBpZC52YWx1ZSlcblxuY29uc3QgeyBkYXRhLCBlcnJvciwgcmV0cnkgfSA9IHVzZUZldGNoKHVybClcbjwvc2NyaXB0PlxuXG48dGVtcGxhdGU+XG4gIExvYWQgcG9zdCBpZDpcbiAgPGJ1dHRvbiB2LWZvcj1cImkgaW4gNVwiIEBjbGljaz1cImlkID0gaVwiPnt7IGkgfX08L2J1dHRvbj5cblxuXHQ8ZGl2IHYtaWY9XCJlcnJvclwiPlxuICAgIDxwPk9vcHMhIEVycm9yIGVuY291bnRlcmVkOiB7eyBlcnJvci5tZXNzYWdlIH19PC9wPlxuICAgIDxidXR0b24gQGNsaWNrPVwicmV0cnlcIj5SZXRyeTwvYnV0dG9uPlxuICA8L2Rpdj5cbiAgPGRpdiB2LWVsc2UtaWY9XCJkYXRhXCI+RGF0YSBsb2FkZWQ6IDxwcmU+e3sgZGF0YSB9fTwvcHJlPjwvZGl2PlxuICA8ZGl2IHYtZWxzZT5Mb2FkaW5nLi4uPC9kaXY+XG48L3RlbXBsYXRlPiIsImltcG9ydC1tYXAuanNvbiI6IntcbiAgXCJpbXBvcnRzXCI6IHtcbiAgICBcInZ1ZVwiOiBcImh0dHBzOi8vc2ZjLnZ1ZWpzLm9yZy92dWUucnVudGltZS5lc20tYnJvd3Nlci5qc1wiXG4gIH1cbn0iLCJ1c2VGZXRjaC5qcyI6ImltcG9ydCB7IHJlZiwgaXNSZWYsIHVucmVmLCB3YXRjaEVmZmVjdCB9IGZyb20gJ3Z1ZSdcblxuZXhwb3J0IGZ1bmN0aW9uIHVzZUZldGNoKHVybCkge1xuICBjb25zdCBkYXRhID0gcmVmKG51bGwpXG4gIGNvbnN0IGVycm9yID0gcmVmKG51bGwpXG5cbiAgYXN5bmMgZnVuY3Rpb24gZG9GZXRjaCgpIHtcbiAgICAvLyByZXNldCBzdGF0ZSBiZWZvcmUgZmV0Y2hpbmcuLlxuICAgIGRhdGEudmFsdWUgPSBudWxsXG4gICAgZXJyb3IudmFsdWUgPSBudWxsXG4gICAgXG4gICAgLy8gcmVzb2x2ZSB0aGUgdXJsIHZhbHVlIHN5bmNocm9ub3VzbHkgc28gaXQncyB0cmFja2VkIGFzIGFcbiAgICAvLyBkZXBlbmRlbmN5IGJ5IHdhdGNoRWZmZWN0KClcbiAgICBjb25zdCB1cmxWYWx1ZSA9IHVucmVmKHVybClcbiAgICBcbiAgICB0cnkge1xuICAgICAgLy8gYXJ0aWZpY2lhbCBkZWxheSAvIHJhbmRvbSBlcnJvclxuICBcdCAgYXdhaXQgdGltZW91dCgpXG4gIFx0ICAvLyB1bnJlZigpIHdpbGwgcmV0dXJuIHRoZSByZWYgdmFsdWUgaWYgaXQncyBhIHJlZlxuXHQgICAgLy8gb3RoZXJ3aXNlIHRoZSB2YWx1ZSB3aWxsIGJlIHJldHVybmVkIGFzLWlzXG4gICAgXHRjb25zdCByZXMgPSBhd2FpdCBmZXRjaCh1cmxWYWx1ZSlcblx0ICAgIGRhdGEudmFsdWUgPSBhd2FpdCByZXMuanNvbigpXG4gICAgfSBjYXRjaCAoZSkge1xuICAgICAgZXJyb3IudmFsdWUgPSBlXG4gICAgfVxuICB9XG5cbiAgaWYgKGlzUmVmKHVybCkpIHtcbiAgICAvLyBzZXR1cCByZWFjdGl2ZSByZS1mZXRjaCBpZiBpbnB1dCBVUkwgaXMgYSByZWZcbiAgICB3YXRjaEVmZmVjdChkb0ZldGNoKVxuICB9IGVsc2Uge1xuICAgIC8vIG90aGVyd2lzZSwganVzdCBmZXRjaCBvbmNlXG4gICAgZG9GZXRjaCgpXG4gIH1cblxuICByZXR1cm4geyBkYXRhLCBlcnJvciwgcmV0cnk6IGRvRmV0Y2ggfVxufVxuXG4vLyBhcnRpZmljaWFsIGRlbGF5XG5mdW5jdGlvbiB0aW1lb3V0KCkge1xuICByZXR1cm4gbmV3IFByb21pc2UoKHJlc29sdmUsIHJlamVjdCkgPT4ge1xuICAgIHNldFRpbWVvdXQoKCkgPT4ge1xuICAgICAgaWYgKE1hdGgucmFuZG9tKCkgPiAwLjMpIHtcbiAgICAgICAgcmVzb2x2ZSgpXG4gICAgICB9IGVsc2Uge1xuICAgICAgICByZWplY3QobmV3IEVycm9yKCdSYW5kb20gRXJyb3InKSlcbiAgICAgIH1cbiAgICB9LCAzMDApXG4gIH0pXG59In0=)，出于演示目的，我们人为地设置了延迟和随机报错。

## 约定和最佳实践

### 命名

组合式函数约定用驼峰命名法命名，并以“use”作为开头。

### 输入参数

尽管其响应性不依赖 ref，组合式函数仍可接收 ref 参数。如果编写的组合式函数会被其他开发者使用，你最好在处理输入参数时兼容 ref 而不只是原始的值。[`unref()`](https://cn.vuejs.org/api/reactivity-utilities.html#unref) 工具函数会对此非常有帮助：

```js
import { unref } from 'vue'

function useFeature(maybeRef) {
  // 若 maybeRef 确实是一个 ref，它的 .value 会被返回
  // 否则，maybeRef 会被原样返回
  const value = unref(maybeRef)
}
```

如果你的组合式函数在接收 ref 为参数时会产生响应式 effect，请确保使用 `watch()` 显式地监听此 ref，或者在 `watchEffect()` 中调用 `unref()` 来进行正确的追踪。

### 返回值

你可能已经注意到了，我们一直在组合式函数中使用 `ref()` 而不是 `reactive()`。我们推荐的约定是组合式函数始终返回一个包含多个 ref 的普通的非响应式对象，这样该对象在组件中被解构为 ref 之后仍可以保持响应性：

```js
// x 和 y 是两个 ref
const { x, y } = useMouse()
```

从组合式函数返回一个响应式对象会导致在对象解构过程中丢失与组合式函数内状态的响应性连接。与之相反，ref 则可以维持这一响应性连接。

如果你更希望以对象属性的形式来使用组合式函数中返回的状态，你可以将返回的对象用 `reactive()` 包装一次，这样其中的 ref 会被自动解包，例如：

```js
const mouse = reactive(useMouse())
// mouse.x 链接到了原来的 x ref
console.log(mouse.x)
```

```html
Mouse position is at: {{ mouse.x }}, {{ mouse.y }}
```

### 副作用

在组合式函数中的确可以执行副作用 (例如：添加 DOM 事件监听器或者请求数据)，但请注意以下规则：

- 如果你的应用用到了[服务端渲染](https://cn.vuejs.org/guide/scaling-up/ssr.html) (SSR)，请确保在组件挂载后才调用的生命周期钩子中执行 DOM 相关的副作用，例如：`onMounted()`。这些钩子仅会在浏览器中被调用，因此可以确保能访问到 DOM。
- 确保在 `onUnmounted()` 时清理副作用。举例来说，如果一个组合式函数设置了一个事件监听器，它就应该在 `onUnmounted()` 中被移除 (就像我们在 `useMouse()` 示例中看到的一样)。当然也可以像之前的 `useEventListener()` 示例那样，使用一个组合式函数来自动帮你做这些事。

### 使用限制

组合式函数在 `<script setup>` 或 `setup()` 钩子中，应始终被**同步地**调用。在某些场景下，你也可以在像 `onMounted()` 这样的生命周期钩子中使用他们。

这个限制是为了让 Vue 能够确定当前正在被执行的到底是哪个组件实例，只有能确认当前组件实例，才能够：

1. 将生命周期钩子注册到该组件实例上
2. 将计算属性和监听器注册到该组件实例上，以便在该组件被卸载时停止监听，避免内存泄漏。

>  TIP
>
> `<script setup>` 是唯一在调用 await 之后仍可调用组合式函数的地方。编译器会在异步操作之后自动为你恢复当前的组件实例。

## 通过抽取组合式函数改善代码结构

抽取组合式函数不仅是为了复用，也是为了代码组织。随着组件复杂度的增高，你可能会最终发现组件多得难以查询和理解。组合式 API 会给予你足够的灵活性，让你可以基于逻辑问题将组件代码拆分成更小的函数：

vue

```
<script setup>
import { useFeatureA } from './featureA.js'
import { useFeatureB } from './featureB.js'
import { useFeatureC } from './featureC.js'

const { foo, bar } = useFeatureA()
const { baz } = useFeatureB(foo)
const { qux } = useFeatureC(baz)
</script>
```

在某种程度上，你可以将这些提取出的组合式函数看作是可以相互通信的组件范围内的服务。

## 在选项式 API 中使用组合式函数

如果你正在使用选项式 API，组合式函数必须在 `setup()` 中调用。且其返回的绑定必须在 `setup()` 中返回，以便暴露给 `this` 及其模板：

js

```
import { useMouse } from './mouse.js'
import { useFetch } from './fetch.js'

export default {
  setup() {
    const { x, y } = useMouse()
    const { data, error } = useFetch('...')
    return { x, y, data, error }
  },
  mounted() {
    // setup() 暴露的属性可以在通过 `this` 访问到
    console.log(this.x)
  }
  // ...其他选项
}
```

## 与其他模式的比较

### 和 Mixin 的对比

Vue 2 的用户可能会对 [mixins](https://cn.vuejs.org/api/options-composition.html#mixins) 选项比较熟悉。它也让我们能够把组件逻辑提取到可复用的单元里。然而 mixins 有三个主要的短板：

1. **不清晰的数据来源**：当使用了多个 mixin 时，实例上的数据属性来自哪个 mixin 变得不清晰，这使追溯实现和理解组件行为变得困难。这也是我们推荐在组合式函数中使用 ref + 解构模式的理由：让属性的来源在消费组件时一目了然。
2. **命名空间冲突**：多个来自不同作者的 mixin 可能会注册相同的属性名，造成命名冲突。若使用组合式函数，你可以通过在解构变量时对变量进行重命名来避免相同的键名。
3. **隐式的跨 mixin 交流**：多个 mixin 需要依赖共享的属性名来进行相互作用，这使得它们隐性地耦合在一起。而一个组合式函数的返回值可以作为另一个组合式函数的参数被传入，像普通函数那样。

基于上述理由，我们不再推荐在 Vue 3 中继续使用 mixin。保留该功能只是为了项目迁移的需求和照顾熟悉它的用户。

### 和无渲染组件的对比

在组件插槽一章中，我们讨论过了基于作用域插槽的[无渲染组件](https://cn.vuejs.org/guide/components/slots.html#renderless-components)。我们甚至用它实现了一样的鼠标追踪器示例。

组合式函数相对于无渲染组件的主要优势是：组合式函数不会产生额外的组件实例开销。当在整个应用中使用时，由无渲染组件产生的额外组件实例会带来无法忽视的性能开销。

我们推荐在纯逻辑复用时使用组合式函数，在需要同时复用逻辑和视图布局时使用无渲染组件。

### 和 React Hooks 的对比

如果你有 React 的开发经验，你可能注意到组合式函数和自定义 React hooks 非常相似。组合式 API 的一部分灵感正来自于 React hooks，Vue 的组合式函数也的确在逻辑组合能力上与 React hooks 相近。然而，Vue 的组合式函数是基于 Vue 细粒度的响应性系统，这和 React hooks 的执行模型有本质上的不同。

## 更多hook的理解

- [Vue3中的Hook函数（对比mixin）](https://juejin.cn/post/7035109549258326046)
- [Vue3必学技巧-自定义Hooks-让写Vue3更畅快](https://juejin.cn/post/7083401842733875208)
- [浅谈：为啥vue和react都选择了Hooks🏂？](https://juejin.cn/post/7066951709678895141)
- [vueuse:我不许身为vuer的前端,你的工具集只有lodash!](https://bbs.huaweicloud.com/blogs/324277?utm_source=zhihu&utm_medium=bbs-ex&utm_campaign=other&utm_content=content)

