





# ESLint介绍

ESLint 可组装的JavaScript和JSX检查工具

ESLint最初是由[Nicholas C. Zakas](http://nczonline.net/) 于2013年6月创建的开源项目。它的目标是提供一个插件化的javascript代码检测工具。

## 为什么要使用ESLint

ESLint 是一个开源的 JavaScript 代码检查工具,。代码检查是一种静态的分析，常用于寻找有问题的模式或者代码，并且不依赖于具体的编码风格。对大多数编程语言来说都会有代码检查，一般来说编译程序会内置检查工具。

但是由于`javascript`的动态弱类型语言特性，导致在开发中如果不加以约束会容易出错，也正是因为这种特性导致当程序出现错误的时候，我们需要花费更多的时间在执行的过程中不断去调试，`Eslint`的出现就是为了让开发人员可以在开发的过程中就发现错误而非在执行过程中。

ESLint是javaScript语法和风格的检查工具，把潜在的不良代码全部找出来，但检查不出逻辑问题。校验我们写的代码，给代码定义一个规范，项目里的代码必须按照这个规范写。帮助我们避免一些非常低级的错误，一些格式上的问题导致我们在运行生产环境的时候出现一些不明所以的报错。在跟团队协作的时候，每个人都保持同一个风格进行代码书写，这样团队内部相互去看别人的代码的时候，就可以更容易的看懂。

`Eslint`保持其插件的特性，让开发人员自定义定制属于自己的规则，也可以去遵循一些大的社区或者团队的规范直接继承下来用于使用，其所有规则都是可插入的，同时为了方便使用，也对其内置了一些规则。

## 文档

中文文档：https://eslint.nodejs.cn/

英文文档：https://eslint.org/

# ESLint配置文件

## 配置文件类型

使用eslint我们需要配置一份eslint的配置文件放置在项目的根目录上，配置文件由很多种写法：

- `.eslintrc.*`：新建文件，位于项目根目录：
  - `.eslintrc.js`：使用commonJS规范配置， 然后输出一个配置对象，推荐使用
  - `.eslintrc.yaml` ：使用YAML格式配置
  - `.eslintrc.yml`：使用YAML格式配置
  - `.eslintrc.json`：使用JSON格式的配置，ESLint 的 JSON 文件允许 JavaScript 风格的注释
  - `.eslintrc`：可以使 JSON 也可以是 YAML**(弃用)** 
- `package.json` 中添加 `eslintConfig`属性：不需要创建文件，在原有文件基础上写

ESLint 会查找和自动读取它们，所以以上配置文件只需要存在一个即可

以 `.eslintrc.js` 配置文件为例：

```javascript
module.exports = {
  // 解析器
  parser: "esprima",
  // parser解析代码时的参数
  parserOptions: {},
  // 代码运行的环境
  env: {},
  // 指定环境为我们提供了预置的全局变量
  globals: {},
  // 插件
  plugins: [],
  // 继承其他规则
  extends: [],
  // 共享设置
  setting: {},
  // 检测配置文件步骤
  root: true,
  // 具体规则配置
  rules: {},
  // ...
};
```

详细配置规则见：https://eslint.bootcss.com/docs/user-guide/configuring

## parser

ESLint默认使用Espree作为其解析器，你可以在配置文件中指定一个不同的解析器，同时babel-eslint也是用得最多的解析器。

```json
{
  "parser": "espree",
}
```

以下解析器与 ESLint 兼容：

- [Esprima](https://www.npmjs.com/package/esprima)
- [Babel-ESLint](https://www.npmjs.com/package/babel-eslint) - 一个对[Babel](https://babeljs.io/)解析器的包装，使其能够与 ESLint 兼容。
- [@typescript-eslint/parser](https://www.npmjs.com/package/@typescript-eslint/parser) - 将 TypeScript 转换成与 estree 兼容的形式，以便在ESLint中使用。

注意，在使用自定义解析器时，为了让 ESLint 在处理非 ECMAScript 5 特性时正常工作，配置属性 `parserOptions` 仍然是必须的。解析器会被传入 `parserOptions`，但是不一定会使用它们来决定功能特性的开关。

## parserOption

parserOption是parser解析代码时的参数。

ESLint 允许你指定你想要支持的 JavaScript 语言选项。默认情况下，ESLint 支持 ECMAScript 5 语法。你可以覆盖该设置，以启用对 ECMAScript 其它版本和 JSX 的支持。

解析器选项可以在 `.eslintrc.*`  文件使用 `parserOptions` 属性设置。可用的选项有：

```json
{
  "parserOption": {
    //指定要使用的ECMAScript版本，默认值5，你可以使用 6、7、8、9 或 10 来指定你想要使用的 ECMAScript 版本。你也可以用使用年份命名的版本号指定为 2015（同 6），2016（同 7），或 2017（同 8）或 2018（同 9）或 2019 (same as 10)
    "ecmaVersion": 5,
    //设置为script(默认)或module（如果你的代码是ECMAScript模块)
    "sourceType": "script",
    //这是个对象，表示你想使用的额外的语言特性,所有选项默认都是false
    "ecmafeatures": {
      //允许在全局作用域下使用return语句
      "globalReturn": false,
      //启用全局strict模式（严格模式）
      "impliedStrict": false,
      //启用JSX
      "jsx": false,
      //启用对实验性的objectRest/spreadProperties的支持
      "experimentalObjectRestSpread": false
    }
  },
}
```

## env

指定环境，每个环境都有自己预定义的全局变量，可以同时指定多个环境，不矛盾。

```json
"env": {
  //效果同配置项ecmaVersion一样
  "es6": true,
  "browser": true,
  "node": true,
  "commonjs": false,
  "mocha": true,
  "jquery": true,
  //如果你想使用来自某个插件的环境时，确保在plugins数组里指定插件名
  //格式为：插件名/环境名称（插件名不带前缀）
  "react/node": true
},
```

更多环境配置：https://eslint.bootcss.com/docs/user-guide/configuring#specifying-environments

## globals

当访问当前源文件内未定义的变量时，[no-undef](https://eslint.bootcss.com/docs/rules/no-undef) 规则将发出警告。如果你想在一个源文件里使用全局变量，推荐你在 ESLint 中定义这些全局变量，这样 ESLint 就不会发出警告了。

要在配置文件中配置全局变量，请将 `globals` 配置属性设置为一个对象，该对象包含以你希望使用的每个全局变量。对于每个全局变量键，将对应的值设置为 `"writable"` 以允许重写变量，或 `"readonly"` 不允许重写变量。例如：

```json
{
  "globals": {
    "var1": "writable",
    "var2": "readonly"
  },
}
```

由于历史原因，布尔值 `false` 和字符串值 `"readable"` 等价于 `"readonly"`。类似地，布尔值 `true` 和字符串值 `"writeable"` 等价于 `"writable"`。但是，不建议使用旧值。

## extends

### 规则继承

一个配置文件可以被基础配置中的已启用的规则继承。

`extends` 属性值可以是：

- 指定配置的字符串(配置文件的路径、可共享配置的名称、`eslint:recommended` 或 `eslint:all`)
- 字符串数组：每个配置继承它前面的配置

可选的配置项如下：

- 字符串eslint：recommended，该配置项启用一系列核心规则，这些规则报告一些常见问题，即在(规则页面)中打勾的规则
- 一个可以输出配置对象的可共享配置包，如eslint-config-standard
  - 可共享配置包是一个导出配置对象的简单的npm包，包名称以eslint-config-开头，使用前要安装
  - extends属性值可以省略包名的前缀eslint-config-
- 一个输出配置规则的插件包，如eslint-plugin-react
  - 一些插件也可以输出一个或多个命名的配置
  - extends属性值为，plugin：包名/配置名称
- 一个指向配置文件的相对路径或绝对路径
- 字符串eslint：all，启用当前安装的ESLint中所有的核心规则
  - 该配置不推荐在产品中使用，因为它随着ESLint版本进行更改。使用的话，请自己承担风险

```json
{
  "extends": [
    "eslint:recommended",
    "standard",
    "plugin:react/recommended",
    "./node_modules/coding-standard/.eslintrc-es6",
    "eslint:all"
  ]
}
```

### 推荐团队规范开源配置

上述是一些基础的配置，具体的使用呢可以自己去配置，一般这类规则的配置可以参照市面上比较流行的规范，使用别人的规范加自己的配置进行结合。市面上目前最为流行的开源配置使用最多的是`airbnb团队`的配置。下面这些配置值得推荐：

- [eslint:recommended](https://eslint.bootcss.com/docs/rules/) ESLint内置的推荐规则在么有讲到 所有打钩的就是内置规则
- [eslint:all](https://eslint.bootcss.com/docs/rules/)：ESLint 内置的所有规则；
- [eslint-config-standard](https://github.com/standard/eslint-config-standard)：standard 的 JS 规范；
- [eslint-config-prettier](https://github.com/prettier/eslint-config-prettier)：关闭和 ESLint 中以及其他扩展中有冲突的规则；
- [eslint-config-airbnb-base](https://github.com/airbnb/javascript/tree/master/packages/eslint-config-airbnb-base)：airbab 的 JS 规范；
- [eslint-config-alloy](https://github.com/AlloyTeam/eslint-config-alloy)：腾讯 AlloyTeam 前端团队出品，可以很好的针对你项目的技术栈进行配置选择，比如可以选 React、Vue（现已支持 Vue 3.0）、TypeScript 等；

### 如何使用

想要使用别人的配置通常只需要下载对应的依赖并且加入到`extends`继承下来即可，可以配置为字符串或者数组均可。

```json
{
  extends: [
    'eslint:recommended', // 是 ESLint 官方的扩展,内置推荐规则
    'plugin:vue/essential', // 是插件类型扩展
    'eslint-config-standard', // eslint-config 开头的都可以省略掉前面 直接使用standard即可
    '@vue/prettier', // @开头扩展和 eslint-config 一样，只是在 npm 包上面加了一层作用域 scope；
    './node_modules/coding-standard/.eslintrc-es6' // 一个执行配置文件的路径
  ]
}

```

## plugins

ESLint 支持使用第三方插件。在使用插件之前，你必须使用 npm 安装它。

### ESlint插件的作用

首先我们需要知道的`Eslint`本质只是一个代码检测工具，默认情况下也只能检测`js`文件，如果我们需要在工程化中加入去兼容其他语法例如[.vue]、[.jsx]等其他格式的文件时就没有办法实现，所以我们需要加入一些插件来实现对非`js格式`的文件进行检测。

### 如何配置plugin

ESlint相关的插件有两种命令名形式，带命名空间的和带的，比如

- eslint-plugin-xxx：以eslint-plugin-开头的都可以直接省略掉，类似上面的extends继承规则只需要 xxx就行
- @开头表示带命名空间的就正常引入即可

在配置文件里配置插件时，可以使用 `plugins` 关键字来存放插件名字的列表。

```json
{
  "plugins": [
    "eslint-plugin-vue",
    // 插件名称可以省略eslint-plugin-前缀
    "react", ',// 是指 eslint-plugin-react        
    '@jquery/jquery',  // 是指 @jquery/eslint-plugin-jquery        
    '@foobar', // 是指 @foobar/eslint-plugin    ] 
  ],
}
```

当需要基于插件进行 extends 和 rules 的配置的时候，需要加上插件的引用，比如：

```json
{
  plugins: [
    'jquery',   // eslint-plugin-jquery
    '@foo/foo', // @foo/eslint-plugin-foo
    '@bar,      // @bar/eslint-plugin
  ],
  extends: [
    'plugin:jquery/recommended',
    'plugin:@foo/foo/recommended',
    'plugin:@bar/recommended'
  ],
  rules: {
    'jquery/a-rule': 'error',
    '@foo/foo/some-rule': 'error',
    '@bar/another-rule': 'error'
  },
}

```

### plugin与extend的区别

- extend提供的是eslint现有规则的一系列预设
- 而plugin则提供了除预设之外的自定义规则，当你在eslint的规则里找不到合适的的时候就可以借用插件来实现了

## root

如果同一个目录下有多个配置文件，ESLint 只会使用一个。优先级顺序如下：

1. `.eslintrc.js`
2. `.eslintrc.yaml`
3. `.eslintrc.yml`
4. `.eslintrc.json`
5. `.eslintrc`
6. `package.json`

Eslint检测配置文件步骤：

1. 在要检测的文件同一目录里寻找.eslintrc.*和package.json
2. 紧接着在父级目录里寻找，一直到文件系统的根目录
3. 如果在前两步发现有root：true的配置，停止在父级目录中寻找.eslintrc
4. 如果以上步骤都没有找到，则回退到用户主目录~/.eslintrc中自定义的默认配置

```json
{
  "root": true,
}
```

## settings  

ESLint 支持在配置文件添加共享设置。你可以添加 `settings` 对象到配置文件，它将提供给每一个将被执行的规则。如果你想添加的自定义规则而且使它们可以访问到相同的信息，这将会很有用，并且很容易配置。

```json
{
  "settings": {
    "sharedData": "Hello"
  },
}
```

## rules

### 基础配置规则

- `"off"` 或 `0` - 关闭规则
- `"warn"` 或 `1` - 开启规则，使用警告级别的错误：`warn` (不会导致程序退出)
- `"error"` 或 `2` - 开启规则，使用错误级别的错误：`error` (当被触发的时候，程序会退出)

正常的配置通常是键值对的形式，那么这一类的配置是没有属性的只需要开启关闭即可类似于

```json
{
	"rules":{
    "semi": "error", // 禁止使用分号
		"eqeqeq": "error", // 要求使用 === 和 !==
	}
}
```

当然部分属性有自己的属性，在控制开关的同时还需要配置属性，例如：

```json
{
  rules: {
    'quotes': ['error', 'single'],  // 如果不是单引号，则报错
    'one-var': ['error', {
      'var': 'always',  // 每个函数作用域中，只允许 1 个 var 声明
      'let': 'never',   // 每个块作用域中，允许多个 let 声明
      'const': 'never', // 每个块作用域中，允许多个 const 声明
    }]
  }
}

```

### 常用配置规则列表

| 配置属性                    | 配置说明                                                     | 推荐配置                                                     |
| --------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| comma-dangle                | 是否允许对象中出现结尾逗号                                   | ["error", "never"]                                           |
| no-cond-assign              | 条件语句的条件中不允许出现赋值运算符                         | 2                                                            |
| no-console                  | 不允许出现console语句                                        | 2                                                            |
| no-constant-condition       | 条件语句的条件中不允许出现恒定不变的量                       | 2                                                            |
| no-control-regex            | 正则表达式中不允许出现控制字符                               | 2                                                            |
| no-debugger                 | 不允许出现debugger语句                                       | 2                                                            |
| no-dupe-args                | 函数定义的时候不允许出现重复的参数                           | 2                                                            |
| no-dupe-keys                | 对象中不允许出现重复的键                                     | 2                                                            |
| no-duplicate-case           | switch语句中不允许出现重复的case标签                         | 2                                                            |
| no-empty                    | 不允许出现空的代码块                                         | 2                                                            |
| no-empty-character-class    | 正则表达式中不允许出现空的字符组                             | 2                                                            |
| no-ex-assign                | 在try catch语句中不允许重新分配异常变量                      | 2                                                            |
| no-extra-boolean-cast       | 不允许出现不必要的布尔值转换                                 | 2                                                            |
| no-extra-parens             | 不允许出现不必要的圆括号                                     | 0                                                            |
| no-extra-semi               | 不允许出现不必要的分号                                       | 2                                                            |
| no-func-assign              | 不允许重新分配函数声明                                       | 2                                                            |
| no-inner-declarations       | 不允许在嵌套代码块里声明函数                                 | ["error", "functions"]                                       |
| no-invalid-regexp           | 不允许在RegExp构造函数里出现无效的正则表达式                 | 2                                                            |
| no-irregular-whitespace     | 不允许出现不规则的空格                                       | 2                                                            |
| no-negated-in-lhs           | 不允许在in表达式语句中对最左边的运算数使用取反操作           | 2                                                            |
| no-obj-calls                | 不允许把全局对象属性当做函数来调用                           | 2                                                            |
| no-regex-spaces             | 正则表达式中不允许出现多个连续空格                           | 2                                                            |
| quote-props                 | 对象中的属性名是否需要用引号引起来                           | 2                                                            |
| no-sparse-arrays            | 数组中不允许出现空位置                                       | 2                                                            |
| no-unreachable              | 在return，throw，continue，break语句后不允许出现不可能到达的语句 | 2                                                            |
| use-isnan                   | 要求检查NaN的时候使用isNaN()                                 | 2                                                            |
| valid-jsdoc                 | 强制JSDoc注释                                                | ["error", {"requireReturn": false, "requireParamDescription": false,     "requireReturnDescription": true   }] |
| valid-typeof                | 在使用typeof表达式比较的时候强制使用有效的字符串             | ["error", { "requireStringLiterals": true   }]               |
| block-scoped-var            | 将变量声明放在合适的代码块里                                 | 2                                                            |
| complexity                  | 限制条件语句的复杂度                                         | 0                                                            |
| consistent-return           | 无论有没有返回值都强制要求return语句返回一个值               | 2                                                            |
| curly                       | 强制使用花括号的风格                                         | ["error", "all"]                                             |
| default-case                | 在switch语句中需要有default语句                              | 0                                                            |
| dot-notation                | 获取对象属性的时候使用点号                                   | ["error", {"allowKeywords": false, "allowPattern": ""}]      |
| eqeqeq                      | 比较的时候使用严格等于                                       | ["error", "smart"]                                           |
| no-alert                    | 不允许使用alert，confirm，prompt语句                         | 1                                                            |
| no-caller                   | 不允许使用arguments.callee和arguments.caller属性             | 2                                                            |
| guard-for-in                | 监视for in循环，防止出现不可预料的情况                       | 0                                                            |
| no-div-regex                | 不能使用看起来像除法的正则表达式                             | 2                                                            |
| no-else-return              | 如果if语句有return，else里的return不用放在else里             | 0                                                            |
| no-labels                   | 不允许标签语句                                               | ["error", {     "allowLoop": false     "allowSwitch": false   }] |
| no-eq-null                  | 不允许对null用==或者!=                                       | 2                                                            |
| no-eval                     | 不允许使用eval()                                             | 2                                                            |
| no-extend-native            | 不允许扩展原生对象                                           | 2                                                            |
| no-extra-bind               | 不允许不必要的函数绑定                                       | 2                                                            |
| no-fallthrough              | 不允许switch按顺序全部执行所有case                           | 2                                                            |
| no-floating-decimal         | 不允许浮点数缺失数字                                         | 2                                                            |
| no-implied-eval             | 不允许使用隐式eval()                                         | 2                                                            |
| no-iterator                 | 不允许使用__iterator__属性                                   | 2                                                            |
| no-lone-blocks              | 不允许不必要的嵌套代码块                                     | 2                                                            |
| no-loop-func                | 不允许在循环语句中进行函数声明                               | 2                                                            |
| no-multi-spaces             | 不允许出现多余的空格                                         | 2                                                            |
| no-multi-str                | 不允许用\来让字符串换行                                      | 2                                                            |
| no-global-assign            | 不允许重新分配原生对象                                       | 2                                                            |
| no-new                      | 不允许new一个实例后不赋值或者不比较                          | 2                                                            |
| no-new-func                 | 不允许使用new Function                                       | 2                                                            |
| no-new-wrappers             | 不允许使用new String，Number和Boolean对象                    | 2                                                            |
| no-octal                    | 不允许使用八进制字面值                                       | 2                                                            |
| no-octal-escape             | 不允许使用八进制转义序列                                     | 2                                                            |
| no-param-reassign           | 不允许重新分配函数参数                                       | 0                                                            |
| no-redeclare                | 不允许变量重复声明                                           | 2                                                            |
| no-return-assign            | 不允许在return语句中使用分配语句                             | 2                                                            |
| no-script-url               | 不允许使用javascript:void(0)                                 | 2                                                            |
| no-self-compare             | 不允许自己和自己比较                                         | 2                                                            |
| no-sequences                | 不允许使用逗号表达式                                         | 2                                                            |
| no-throw-literal            | 不允许抛出字面量错误 throw "error"                           | 2                                                            |
| no-unused-expressions       | 不允许无用的表达式                                           | 2                                                            |
| no-void                     | 不允许void操作符                                             | 2                                                            |
| no-warning-comments         | 不允许警告备注                                               | [1, {"terms": ["todo", "fixme", "any other term"]}]          |
| no-with                     | 不允许使用with语句                                           | 2                                                            |
| radix                       | 使用parseInt时强制使用基数来指定是十进制还是其他进制         | 1                                                            |
| vars-on-top                 | var必须放在作用域顶部                                        | 0                                                            |
| wrap-iife                   | 立即执行表达式的括号风格                                     | [2, "any"]                                                   |
| yoda                        | 不允许在if条件中使用yoda条件                                 | [2, "never", {"exceptRange": true}]                          |
| strict                      | 使用严格模式                                                 | [2, "function"]                                              |
| no-catch-shadow             | 不允许try catch语句接受的err变量与外部变量重名               | 2                                                            |
| no-label-var                | 不允许标签和变量同名                                         | 2                                                            |
| no-shadow                   | 外部作用域中的变量不能与它所包含的作用域中的变量或参数同名   | 2                                                            |
| no-shadow-restricted-names  | js关键字和保留字不能作为函数名或者变量名                     | 2                                                            |
| no-undef                    | 不允许未声明的变量                                           | 2                                                            |
| no-undef-init               | 不允许初始化变量时给变量赋值undefined                        | 2                                                            |
| no-undefined                | 不允许把undefined当做标识符使用                              | 2                                                            |
| no-unused-vars              | 不允许有声明后未使用的变量或者参数                           | [2, {"vars": "all", "args": "after-used"}]                   |
| no-use-before-define        | 不允许在未定义之前就使用变量                                 | [2, "nofunc"]                                                |
| brace-style                 | 大括号风格                                                   | [2, "1tbs", { "allowSingleLine": false}]                     |
| camelcase                   | 强制驼峰命名规则                                             | [2, {"properties": "never"}]                                 |
| comma-style                 | 逗号风格                                                     | [2, "last"]                                                  |
| consistent-this             | 当获取当前环境的this是用一样的风格                           | [0, "self"]                                                  |
| eol-last                    | 文件以换行符结束                                             | 2                                                            |
| func-names                  | 函数表达式必须有名字                                         | 0                                                            |
| func-style                  | 函数风格，规定只能使用函数声明或者函数表达式                 | 0                                                            |
| key-spacing                 | 对象字面量中冒号的前后空格                                   | [2, {"beforeColon": false, "afterColon": true}]              |
| max-nested-callbacks        | 回调嵌套深度                                                 | 0                                                            |
| new-cap                     | 构造函数名字首字母要大写                                     | [2, {"newIsCap": true, "capIsNew": false}]                   |
| new-parens                  | new时构造函数必须有小括号                                    | 2                                                            |
| newline-after-var           | 变量声明后必须空一行                                         | 0                                                            |
| no-array-constructor        | 不允许使用数组构造器                                         | 2                                                            |
| no-inline-comments          | 不允许行内注释                                               | 0                                                            |
| no-lonely-if                | 不允许else语句内只有if语句                                   | 0                                                            |
| no-mixed-spaces-and-tabs    | 不允许混用tab和空格                                          | [2, "smart-tabs"]                                            |
| no-multiple-empty-lines     | 空行最多不能超过两行                                         | [2, {"max": 2}]                                              |
| no-nested-ternary           | 不允许使用嵌套的三目运算符                                   | 2                                                            |
| no-new-object               | 禁止使用new Object()                                         | 2                                                            |
| fun-call-spacing            | 函数调用时，函数名与()之间不能有空格                         | 2                                                            |
| no-ternary                  | 不允许使用三目运算符                                         | 0                                                            |
| no-trailing-spaces          | 一行最后不允许有空格                                         | 2                                                            |
| no-underscore-dangle        | 不允许标识符以下划线开头                                     | 2                                                            |
| no-extra-parens             | 不允许出现多余的括号                                         | 0                                                            |
| one-var                     | 强制变量声明放在一起                                         | 0                                                            |
| operator-assignment         | 赋值运算符的风格                                             | 0                                                            |
| padded-blocks               | 块内行首行尾是否空行                                         | [2, "never"]                                                 |
| quote-props                 | 对象字面量中属性名加引号                                     | 0                                                            |
| quotes                      | 引号风格                                                     | [1, "single", "avoid-escape"]                                |
| semi                        | 强制语句分号结尾                                             | [2, "always"]                                                |
| semi-spacing                | 分后前后空格                                                 | [2, {"before": false, "after": true}]                        |
| sort-vars                   | 变量声明时排序                                               | 0                                                            |
| space-before-blocks         | 块前的空格                                                   | [2, "always"]                                                |
| space-before-function-paren | 函数定义时括号前的空格                                       | [2, {"anonymous": "always", "named": "never"}]               |
| space-infix-ops             | 操作符周围的空格                                             | [2, {"int32Hint": true}]                                     |
| keyword-spacing             | 关键字前后的空格                                             | 2                                                            |
| space-unary-ops             | 一元运算符前后不要加空格                                     | [2, { "words": true, "nonwords": false}]                     |
| wrap-regex                  | 正则表达式字面量用括号括起来                                 | 2                                                            |
| no-var                      | 使用let和const代替var                                        | 0                                                            |
| generator-star-spacing      | 生成器函数前后空格                                           | [2, "both"]                                                  |
| max-depth                   | 嵌套块深度                                                   | 0                                                            |
| max-len                     | 一行最大长度，单位为字符                                     | 0                                                            |
| max-params                  | 函数最多能有多少个参数                                       | 0                                                            |
| max-statements              | 函数内最多有几个声明                                         | 0                                                            |
| no-bitwise                  | 不允许使用位运算符                                           | 0                                                            |
| no-plusplus                 | 不允许使用++ --运算符                                        | 0                                                            |
| indent                      | 强制一致的缩进风格                                           | 2                                                            |
| no-delete-var               | 不允许使用delete操作符                                       | 2                                                            |
| no-proto                    | 不允许使用__proto__属性                                      | 2                                                            |

### 规则的优先级

- 如果 extends 配置的是一个数组，那么最终会将所有规则项进行合并，出现冲突的时候，后面的会覆盖前面的；
- 通过 rules 单独配置的规则优先级比 extends 高；

# 使用步骤

## 安装

- 局部安装：`npm install eslint --save-dev`
- 版本要求：nodejs >= 6.14、 npm 3+

## 创建配置文件

用cli命令行工具直接生成配置文件.eslintrc.js：

```shell
npx eslint --init
```

执行 `npx eslint --init` 会生成一份基础的ESlint配置文件，你可以选择`JS`、`JSON`、`YAML`等格式、同时在其中可以选择一些你需要的规则，如果你是初次使用，那么建议你全部回车，在选择框架的地方建议选择none，暂时我们先不去考虑如何集成框架，接下来就可以生成.eslintrc.js文件，配置如下：

```js
module.exports = {
	// Environments，指定代码的运行环境。不同的运行环境，全局变量不一样，指明运行环境这样ESLint就能识别特定的全局变量。同时也会开启对应环境的语法支持，例如：es6。
	env: {
		browser: true, 
		es2021: true,
	},
	// 可共享配置的名称、eslint:recommended 或 eslint:all,表示默认开启一些内置的规则，包含的，在 https://eslint.bootcss.com/docs/rules/ 中可以查看内置规则
  extends: 'eslint:recommended',
  // 解析选项
	parserOptions: {
		ecmaVersion: 'latest', // 指定你想要使用的 ECMAScript 版本
		sourceType: 'module', //"script" (默认) 或 "module"（如果你的代码是 ECMAScript 模块)
	},
	// 配置规则的地方
	rules: {
	},
};
```

## 配置参考

```json
module.exports = {
  parserOptions: {
    ecmaVersion: "latest", // 指定你想要使用的 ECMAScript 版本
    sourceType: "module",  //"script" (默认) 或 "module"（如果你的代码是 ECMAScript 模块)
  },
  env: {
    node: true, // 启用node中全局变量
    browser: true, // 启用浏览器中全局变量
  },
  extends: ["eslint:recommended"],// 继承 Eslint 推荐规则
  rules: {
    "no-var": "error", // 不能使用 var 定义变量
    "semi": "error", // 禁止使用分号
		"eqeqeq": "error", // 要求使用 === 和 !==
  },
};
```

## 执行校验

通过上面的介绍可以自己为自己配置一系列规则，那么我们如何触发呢？

校验单个文件

```
npx eslint a.js b.js
```

校验一个目录

```
npx eslint src
```

校验非`js`格式的文件，通常`Eslint`只能校验js文件，如果我们要校验`.vue`、`.jsx`文件该怎么做呢，只配置`vue-eslint-parser`的解析器是不够的，还需要让Eslint在查找文件的时候找得到`.vue`文件。可以通过`--ext`指定需要校验的文件格式

```
npx eslint --ext .js,.jsx,.vue src
```

在上面的规则当中进入官方文档的配置规则，在全部规则的列表里面带有标志的规则表示可以被Eslint自动修复，那么我们如何自动修复呢？只需要通过`--fix`的命名即可，和检验文件的命令相似，只需要再加上修复命令即可

```
npx eslint --fix src
```

当然在项目中每次自己输入太过于繁琐也不好记，我们可以在`package.json`中配置检测和修复命令：

```json
{
  "scripts": {
    "lint": "npx eslint --ext .js,.jsx,.vue src",
    "lint:fix": "npx eslint --fix --ext .js,.jsx,.vue src",
  }
}
```

## .eslintignore

过滤自己不需要检测的文件，在某些情况下我们不需要检测某些文件，或者由于某些原因在当前场景下不想去检测某个文件，我们只需要在项目根目录去创建`.eslintignore`文件告诉 ESLint 去忽略特定的文件和目录，在其中指定目录或者文件即可，路径是以根目录为相对地址的路径。

`.eslintignore` 文件是一个纯文本文件，其中的每一行都是一个 glob 模式表明哪些路径应该忽略检测。

除了 `.eslintignore` 文件中的模式，ESLint默认忽略 `/node_modules/*` 和 `/bower_components/*` 中的文件。

例如：把下面 `.eslintignore` 文件放到当前工作目录里，将忽略项目根目录下的 `node_modules`，`bower_components` 以及 `build/` 目录下除了 `build/index.js` 的所有文件。

```
# /node_modules/* and /bower_components/* 默认是忽略的

# 忽略 built 目录下的文件，除了 build/index.js
build/*
!build/index.js

# 忽略dist目录下所有文件
dist
```

# 参考

- [ESLint官网](https://eslint.bootcss.com/docs/user-guide/configuring)
- [Eslint使用入门指南](https://juejin.cn/post/7067072359995457567)
- [Eslint进阶使用指南](https://juejin.cn/post/7067722555783643144)
