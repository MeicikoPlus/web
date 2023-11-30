# 前言

##  为什么需要打包工具？

开发时，我们会使用框架（React、Vue），ES6 模块化语法，Less/Sass 等 css 预处理器等语法进行开发。这些代码要想在浏览器运行必须编译成浏览器能识别的 JS、Css 等语法，才能运行。所以我们需要打包工具帮我们做完这些事。

除此之外，打包工具还能压缩代码、做兼容性处理、提升代码性能等。

##  有哪些打包工具？

- Grunt
- Gulp
- Parcel
- Webpack
- Rollup
- Vite
- ...

目前市面上最流行的依然是 Webpack。

## 开发环境

- VS Code
- webpack:  V5.72.1 
- webpack-cli:  V4.9.2
- Nodejs: v16.14.0

# Webpack 介绍

## 什么是 Webpack 

本质上，webpack 是一个用于现代 JavaScript 应用程序的 **静态模块打包工具**。

当 webpack 处理应用程序时，它会在内部从一个或多个入口点构建一个 依赖图(dependency graph)，然后将你项目中所需的每一个模块组合成一个或多个 bundles，它们均为静态资源，用于展示你的内容。

编译打包输出的bundles文件就是编译好的文件，就可以在浏览器段运行了。

webpack 是最为强大的前端模块管理和打包工具，能够将琐碎的各种文件资源按照依赖关系打包成一个JS依赖文件，通过webpack进行模块化编程。

webpack 默认只对JS进行处理，其他类型文件需要配置loader或者插件进行处理。

## Webpack 产生的背景

为什么打包？因为：

1. 各个依赖文件的关系难以梳理，耦合程度较高，代码难以维护。
2. 把所有依赖包都打包成为一个js文件（bundle.js）文件，会有效降低文件请求次数，一定程度提升性能。
3. 逻辑多、文件多，项目复杂度提高

为什么要用webpack？因为：

1. webpack 还具有“翻译官”的功能，把浏览器不能直接加载的文件通过webpack编译打包后使用，例如将ES6翻译为低版本的语法，将less、sass翻译为css等功能。
2. webpack 不仅强大，而且灵活，默认只能打包 JavaScript，但是webpack有很多功能强大的loader，一切皆可打包，功能丰富的插件，plugin可插拔。

## webpack中文网站

https://webpack.docschina.org/

# Webpack 基本使用

**第一步**：创建项目目录webpack_app，初始化项目 `npm init`，安装webpack和webpack-cli

```bash
npm install webpack webpack-cli -D
```

**第二步**：在项目根目录下创建src文件夹，并在src文件夹下创建index.js作为项目的入口文件，在index.js中导入其他模块编写相关功能。

**第三步**：在使用cmd切到项目目录下执行 `npx webpack` 进行打包

打包后会在项目根目录下创建 dist文件夹，并在dist文件夹下创建一个打包好的 main.js 文件，使用时只需要在html文件中导入main.js即可。

**第四步**：为了方便打包，可以在配置package.json运行指令，生成package.json文件，在 package.json 中 scripts 中加入以下代码，执行 `npm run dev`可以直接编译JS：

```json
"scripts": {
  "dev": "npx webpack"
}
```

webpack默认使用的打包入口文件是 src/index.js，打包后的输出文件是 dist/main.js，如果想要改变入口文件和输出文件的路径则需要在项目根目录下添加 webpack.config.js 来配置webpack，在webpack.config.js配置loader及插件也可以打包除JS外的其它资源。

# Webpack 配置文件

webpack配置的5大核心概念：

- entry（入口）：指示 Webpack 从哪个文件开始打包

- output（输出）：指示 Webpack 打包完的文件输出到哪里去，如何命名等

- loader（加载器）：webpack本身只能处理 js、json等资源，其他资源需要借助 loader，Webpack 才能解析

- plugins（插件）：扩展 Webpack 的功能

- mode（模式）：主要由两种模式：

  - 开发模式：development

  - 生产模式：production

在项目根目录下新建文件：`webpack.config.js`添加如下内容：

```js
const path = require('path');

module.exports = {
	// 1、开发环境development，生产环境 production 的配置，mode 的默认值设置为 production。如果在指令中使用了mode，会覆盖这里mode的值
	mode: 'development',

	// 2、设置入口文件的路径，路径可以是相对路径，也可以是绝对路径，项目打包时运行的入口文件，默认值是 ./src/index.js
	entry: path.resolve(__dirname, 'src/main.js'),

	// 3、指定出口文件，项目打包成功后导出的文件
	output: {
    filename: 'bundle.js', // 设置打包后的文件名
		path: path.resolve(__dirname, './dist'), // 设置出口文件的路径，路径必须是一个绝对路径，path是所有文件输出路径
    clean: true, // 每次打包前清空dist目录，再进行打包
		// 告诉 webpack 在生成的运行时代码中可以使用哪个版本的 ES 特性。
		environment: {
			arrowFunction: false, // 是否支持箭头函数
		},
	},
  
	// 4、配置loader
	module: {
		rules: [],
	},
  
	// 5、插件的配置
	plugins: [],
};
```

运行 `npm run dev`进行打包，这样就可以根据自己的需求修改入口文件及出口文件，后面我们将在webpack.config.js 添加loader及插件来编译其除JS之外的资源。

# Webpack的两种开发模式

Webpack的两种开发模式

- mode=development：开发模式用于开发环境
- mode=production：生产模式生产环境 

开发模式顾名思义就是我们开发代码时使用的模式。这个模式下我们主要做两件事：

- 编译代码，使浏览器能识别运行：开发时我们有样式资源、字体图标、图片资源、html 资源等，webpack 默认都不能处理这些资源，所以我们要加载配置来编译这些资源。

- 代码质量检查，树立代码规范
  - 提前检查代码的一些隐患，让代码运行时能更加健壮。
  - 提前检查代码规范和格式，统一团队编码风格，让代码更优雅美观。

生产模式是开发完成代码后，我们需要得到代码将来部署上线。生产模式主要对代码进行优化，让其运行性能更好。优化主要从两个角度出发:

- 优化代码运行性能

- 优化代码打包速度

下面以开发模式为例，处理各种资源。

```js
const path = require('path');

module.exports = {
	// 开发环境development，生产环境 production 的配置，mode 的默认值设置为 production。如果在指令中使用了mode，会覆盖这里mode的值
	mode: 'development',
};
```

# loader配置

Loader是模块和资源的转换器，借助loader可以加载任何类型的模块或文件，loader 一般以 xxx-loader 的方式命名，xxx 代表了这个 loader 要做的转换功能，比如 css-loader。想要打包其他类型的文件也需要在 webpack.config.js 添加模块打包的配置信息。

## 处理样式资源

Webpack 本身是不能识别除JS之外的资源，所以我们需要借助 Loader 来帮助 Webpack 解析样式资源。我们找 Loader 都应该去[官方文档中找到对应的 Loader](https://webpack.docschina.org/loaders/)，然后使用官方文档找不到的话，可以从 Github 中搜索查询。

### 处理 CSS 资源

处理 CSS 资源需要用到 `css-loader` 和 `style-loader` 两个loader：

- `css-loader`：负责将 CSS 文件编译成 Webpack 能识别的模块
- `style-loader`：会动态创建一个 Style 标签，里面放置 Webpack 中 CSS 模块内容，此时样式就会以 Style 标签的形式在页面上生效。

安装这两个模块包：

```shell
npm i css-loader style-loader -D
```

修改配置文件如下，然后在入口文件导入编写好的css资源 `import './css/index.css';`，运行 `npm run dev` 进行编译：

```js
module.exports = {
  module: {
    rules: [
      // 处理css
      {
        // 用来匹配 .css 结尾的文件
        test: /\.css$/i,
        // use 数组里面 loader 执行顺序是从右到左，从下到上，编译css，这两个loader顺序不能错
				use: [
					'style-loader', // 将js模块中的css通过创建style标签添加到html文件中
					'css-loader', // 将css编译为commonjs模块到js模块中
				],
      },
    ],
  },
};
```

### 处理 Less 资源

处理 Less  资源需要用到 `css-loader` 、 `style-loader` 、`less-loader`：

- `less-loader`：负责将 Less 文件编译成 Css 文件

安装`less-loader`：

```shell
npm i less-loader  -D
```

修改配置文件如下，然后在入口文件导入编写好的less资源 `import './less/index.less';`，运行 `npm run dev` 进行编译：

```js
module.exports = {
  module: {
    rules: [
      // 处理less
      {
        test: /\.less$/,
        use: ["style-loader", "css-loader", "less-loader"],
      },
    ],
  },
};
```

### 处理 Sass和Scss 资源

处理 Less  资源需要用到 `css-loader` 、 `style-loader` 、`sass-loader`、 `sass`：

- `sass-loader`：负责将 sass文件或者scss 编译成 css 文件
- `sass`：`sass-loader` 依赖 `sass` 进行编译

安装`sass-loader`和`sass`：

```shell
npm i sass-loader sass -D
```

修改配置文件如下，然后在入口文件导入编写好的scss资源 `import './scss/index.scss';`，运行 `npm run dev` 进行编译：

```js
module.exports = {
  module: {
    rules: [
      // 处理sass
      {
        test: /\.s[ac]ss$/,
        use: ["style-loader", "css-loader", "sass-loader"],
      },
    ],
  },
};
```

## 资源模块

资源模块(asset module)是一种模块类型，它允许使用资源文件（字体，图标、文本、图片、音视频等）而无需配置额外 loader。

在 webpack 5 之前，通常使用：

- [`raw-loader`](https://v4.webpack.js.org/loaders/raw-loader/) 将文件导入为字符串，用于处理文本文件等资源
- [`url-loader`](https://v4.webpack.js.org/loaders/url-loader/) 将文件作为 data URI 内联到 bundle 中，用于处理图片等资源
- [`file-loader`](https://v4.webpack.js.org/loaders/file-loader/) 将文件发送到输出目录，用于处理图片、音视频等资源

现在 Webpack5 已经将这些loader的功能内置到 Webpack 里了，我们只需要配置资源模块就可以处理资源文件。

资源模块类型(asset module type)，通过添加 4 种新的模块类型，来替换所有这些 loader：

- `asset/resource` 发送一个单独的文件并导出 URL。之前通过使用 `file-loader` 实现。
- `asset/inline` 导出一个资源的 data URI。之前通过使用 `url-loader` 实现。
- `asset/source` 导出资源的源代码。之前通过使用 `raw-loader` 实现。
- `asset` 在导出一个 data URI 和发送一个单独的文件之间自动选择。之前通过使用 `url-loader`，并且配置资源体积限制实现。

### Resource 资源

Resource 资源可以是图片、音视频、文本、字体图标等资源，输出的时候直接复制资源文件到dist目录。

```js
module.exports = {
 module: {
   rules: [
     {
       test: /\.(png|jpe?g|gif|webp)$/,
       type: 'asset/resource'
     }
   ]
 },
};
```

### inline 资源

inline 资源可以是图片、HTML等资源，输出的时候会把资源打包到bundle.js文件中，比如图片会转为base64作为url使用，用于优化占用空间比较小的资源优化：

- 使用Resource 资源加载图片文件的时候需要发送http请求，使用inline 资源加载base64的图片不需要发送http请求
- 这样做的好处：加载小图片可以不使用http请求，减少http请求，缺点：打包后的js文件会大一点点
- 一般会把小于 8kb 的文件，将会视为 `inline` 模块类型，否则会被视为 `resource` 模块类型

```js
module.exports = {
  module: {
    rules: [
      {
       	test: /\.(png|jpe?g|gif|webp)$/,
        type: 'asset/inline',
      }
    ]
  },
};
```

### source 资源

```js
module.exports = {
  module: {
    rules: [
      {
       test: /\.txt/,
       type: 'asset/source',
      }
    ]
  },
};
```

所有 `.txt` 文件将原样注入到 bundle 中。

### asset 通用资源类型

现在，webpack 将按照默认条件，自动地在 `resource` 和 `inline` 之间进行选择：小于 8kb 的文件，将会视为 `inline` 模块类型，否则会被视为 `resource` 模块类型。

```js
module.exports = {
  module: {
    rules: [
      {
       	test: /\.(png|jpe?g|gif|webp)$/,
        type: 'asset',
      }
    ]
  },
};
```

可以通过在 webpack 配置的 module rule 层级中，设置 [`Rule.parser.dataUrlCondition.maxSize`](https://webpack.docschina.org/configuration/module/#ruleparserdataurlcondition) 选项来修改此条件：

```js
module.exports = {
  module: {
    rules: [
      {
       	test: /\.(png|jpe?g|gif|webp)$/,
        type: 'asset',
        parser: {
					// 小于 10kb 的文件，将会视为 inline 模块类型，否则会被视为 resource 模块类型。
					dataUrlCondition: {
						maxSize: 10 * 1024, // 10kb
					},
				},
      }
    ]
  },
};
```

### 自定义输出文件名

默认情况下，`asset/resource` 模块以 `[hash][ext][query]` 文件名发送到输出的根目录：

- hash：文件命名为20位的hash值
- ext：文件的后缀名
- query：传递的query类型的参数

默认情况下会在dist根目录输出资源，可以通过在 webpack 配置中设置 [`output.assetModuleFilename`](https://webpack.docschina.org/configuration/output/#outputassetmodulefilename) 来修改输出asset资源的路径及文件名，这种方式是统一设置文件资源的输出：

```js
const path = require('path');

module.exports = {
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
   	assetModuleFilename: 'static/[hash][ext][query]'，// 自定义asset module资源打包后的路径和名字
  },
  module: {
    rules: [
      {
       test: /\.(png|jpe?g|gif|webp)$/,
        type: 'asset'
      }
    ]
  },
};
```

使用此配置配置，所有图片以及其他资源都会输出到static目录中。也可以使用 Rule.generator.filename 来指定某一资源的输出：

```js
module.exports = {
  module: {
    rules: [
      {
       	test: /\.(png|jpe?g|gif|webp)$/,
        type: 'asset',
        generator: {
         	filename: 'images/[hash][ext][query]'
       	}
      }
    ]
  },
};
```

使用此配置，所有图片送到输出目录中的 `images` 目录中。

`Rule.generator.filename` 与 [`output.assetModuleFilename`](https://webpack.docschina.org/configuration/output/#outputassetmodulefilename) 相同，并且仅适用于 `asset` 和 `asset/resource` 模块类型。

### 常用资源配置参考

#### 图片资源配置

```js
module.exports = {
  module: {
    rules: [
      {
				test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
				type: 'asset',
				parser: {
					dataUrlCondition: {
						maxSize: 10 * 1024, // 10kb
					},
				},
				generator: {
					filename: 'images/[hash:10][ext][query]',
				},
			},
    ]
  },
};
```

#### 字体图标配置

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
				type: 'asset/resource',
				generator: {
					filename: 'iconfont/[hash:10][ext][query]', 
				},
			},
    ]
  },
};
```

#### 其他资源配置

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
				type: 'asset/resource',
				generator: {
					filename: 'media/[hash:10][ext][query]', 
				},
			},
    ]
  },
};
```

## Babel配置

Webpack 对 JavaScript 处理是有限的，只能编译 JavaScript  中 ES 模块化语法，不能编译其他语法，导致 JavaScript  不能在 IE 等浏览器运行，所以我们希望做一些兼容性处理。 JavaScript  兼容性处理，我们使用 Babel 来完成。

Babel主要用于将 ES6+ 语法编写的代码转换为向后兼容的 JavaScript 语法，以便能够运行在当前和旧版本的浏览器或其他环境中。

### 下载模块包

```shell
npm i babel-loader @babel/core @babel/preset-env -D
```

### 配置 babel.config.js

在项目根目录创建 babel.config.js，添加如下内容

```js
module.exports = {
	presets: [
		[
			'@babel/preset-env',
			{
				useBuiltIns: 'usage',
				corejs: '3.6.5',
			},
		],
	],
};
```

在package.json添加 browserslist：

```json
{
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not dead"
  ]
}
```

### 修改配置并编译

修改配置文件如下，运行 `npm run dev` 进行编译：

```js
module.exports = {
  module: {
    rules: [
			// 使用babel编译，兼容旧版本浏览器
			{
				test: /\.m?jsx?$/,
				exclude: /node_modules/, // 排除node_modules文件中的模块，node_modules是别人已经处理好的模块包，不需要重复处理
				use: {
					loader: 'babel-loader',
				},
			},
    ],
  },
};
```

# 插件

## 处理 Html 资源

HtmlWebpackPlugin 插件将为你生成一个 HTML5 文件， 在 body 中使用 `script` 标签引入你所有 webpack 生成的 bundle。

开发文档：https://github.com/jantimon/html-webpack-plugin#options

下载模块

```shell
npm i html-webpack-plugin -D
```

添加该插件到你的 webpack 配置中，如下所示：

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
	plugins: [
		// 自动生成一个HTML文件，并添加script标签导入webpack打包生成的出口文件bundle.js
    // 根据自己提供的HTML模板生成HTML文件
    new HtmlWebpackPlugin({
			template: path.resolve(__dirname, 'public/index.html'),
		}),
	],
};
```

## ESLint

开发中，团队对代码格式是有严格要求的，我们不能由肉眼去检测代码格式，需要使用专业的工具来检测。针对代码格式，我们使用 Eslint 来完成

Eslint 可组装的 JavaScript 和 JSX 检查工具：用来检测 js 和 jsx 语法的工具，可以配置各项功能。

使用 Eslint，关键是写 Eslint 配置文件，里面写上各种 rules 规则，将来运行 Eslint 时就会以写的规则对代码进行检查。

### 配置.eslintrc.js

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
  root: true,
};
```

### 在webpack配置ESLint

安装下载模块

```shell
npm i eslint-webpack-plugin eslint -D
```

添加该插件到你的 webpack 配置中，如下所示：

```js
const ESLintWebpackPlugin = require('eslint-webpack-plugin');

module.exports = {
	plugins: [
    // ESLint检查
    new ESLintWebpackPlugin({
			context: path.resolve(__dirname, 'src'), // context指定检查的目录
		}),
	],
};
```

## CSS优化

### 提取 CSS 成单独文件

CSS 文件目前被打包到 js 文件中，当 js 文件加载时，会创建一个 style 标签来生成样式。这样对于网站来说，会出现闪屏现象，用户体验不好。我们应该是生成单独的 CSS 文件，通过 link 标签加载CSS文件性能才好。

`mini-css-extract-plugin`插件会将 CSS 提取到单独的文件中，为每个包含 CSS 的 JS 文件创建一个 CSS 文件，并且支持 CSS 的按需加载。

下载模块

```shell
npm i mini-css-extract-plugin -D
```

添加该插件到你的 webpack 配置中，如下所示：

```js
const MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  module: {
    rules: [
			// 处理css
			{
				test: /\.css$/i,
        use: [
          MiniCssExtractPlugin.loader,// 提取css到单独的文件中
					// 'style-loader', // 将js模块中的css通过创建style标签添加到html文件中
					'css-loader', // 将css编译为commonjs模块到js模块中
				],
			},
			// 处理scss
			{
				test: /\.s[ac]ss$/i,
        use: [
          MiniCssExtractPlugin.loader,// 提取css到单独的文件中
					// 'style-loader', // 将 JS 字符串生成为 style 节点
					'css-loader', // 将 CSS 转化成 CommonJS 模块
					'sass-loader', // 将 Sass 编译成 CSS
				],
			},
    ],
  },
  plugins: [
    // 提取css成单独文件
    new MiniCssExtractPlugin({
      filename: 'css/index.css',// 指定文件的目录，只能是相对路径
    }),
	],
};
```

### PostCSS兼容性控制

PostCSS是一个用 JavaScript 工具和插件转换 CSS 代码的工具。 这些插件可以检查（lint）你的 CSS，支持 CSS Variables 和 Mixins， 编译尚未被浏览器广泛支持的先进的 CSS 语法，内联图片，以及其它很多优秀的功能。

官网：https://www.postcss.com.cn/

中文文档：https://github.com/postcss/postcss/blob/main/docs/README-cn.md

参考：[什么是 PostCSS？如何使用插件自动化 CSS 任务](https://juejin.cn/post/7062717813764390948)

下载模块

```shell
npm i postcss-loader postcss postcss-preset-env -D
```

添加PostCSS到你的 webpack 配置中，如下所示：

```js
module.exports = {
  module: {
    rules: [
			// 处理css
			{
				test: /\.css$/i,
        use: [
          MiniCssExtractPlugin.loader,// 提取css到单独的文件中
          'css-loader', // 将css编译为commonjs模块到js模块中
          {
            loader: 'postcss-loader',
            options: {
              postcssOptions: {
                plugins: ['postcss-preset-env'],
              },
            },
          },
				],
			},
			// 处理scss
			{
				test: /\.s[ac]ss$/i,
        use: [
          MiniCssExtractPlugin.loader,// 提取css到单独的文件中
          'css-loader', // 将 CSS 转化成 CommonJS 模块
          {
            loader: 'postcss-loader',
            options: {
              postcssOptions: {
                plugins: ['postcss-preset-env'],
              },
            },
          },
					'sass-loader', // 将 Sass 编译成 CSS
				],
			},
    ],
  },
};
```

可以给css添加 `display: flex;`，并修改 `browserslist` 如下：

```json
{
  "browserslist": ["ie >= 8"]
}
```

打包后的样式文件会出现如下结果，`display: flex`兼容IE8需要添加前缀：

```css
.box {
  display: -ms-flexbox;
  display: flex;
}
```

# 本地服务dev-server

## 介绍

`webpack-dev-server` 为你提供了一个基本的 web server，并且具有 live reloading(实时重新加载) 功能。

webpack-dev-server 将在 localhost:8080 启动一个 express 静态资源 web 服务器，默认静态资源目录是output配置的path目录，并且会以监听模式自动运行 webpack，在浏览器打开 http://localhost:8080/ 可以浏览项目中的页面和编译后的资源输出。也可以在配置文件中指定端口号和静态目录

## 安装

安装webpack-dev-server模块

```bash
npm i webpack-dev-server -D
```

## 配置webpack

在webpack.config.js添加配置：

```js
module.exports = {
  devServer: {
    host: "localhost", // 启动服务器域名
    port: "3000", // 启动服务器端口号
    open: true, // 是否自动打开浏览器
  }
};
```

## 配置指令

在scripts中添加指令

```json
"scripts": {
  "dev": "npx webpack",
  "serve": "npx webpack serve"
},
```

终端执行 `npm run serve` 命令，开启一个服务器。

当使用开发服务器时，所有代码都会在内存中编译打包，并不会输出到 dist 目录下。开发时我们只关心代码能运行，有效果即可，至于代码被编译成什么样子，并不需要知道，开发完成，再进行打包。

## 指令总结

- `npx webpack` 使用有两种情况：

  - mode设置为development，用于开发环境，是在开发过程中调试使用

  - mode设置为production，用于生产环境 ，是在开发完毕上线项目使用

- `npx webpack serve`：启动本地服务，用在开发环境，并在内存中编译打包。

不管是那种情况，都会编译打包代码，每次打包后然后引入到HTML文件中使用，这样比较麻烦，所以使用webpack-dev-server开启本地服务器，使用 `npx webpack serve`，启动webpack，这样会在内存中编译打包，并且当依赖的文件改动时，会自动刷新页面，开发调试比较方便。

# 生产环境配置

在项目根目录下创建config目录，并创建webpack.dev.js和webpack.prod.js

- webpack.dev.js 用于开发环境的配置
- webpack.dev.js 用于生产环境的配置

```tex
├── webpack_app (项目根目录)
    ├── config (Webpack配置文件目录)
    │    ├── webpack.dev.js(开发模式配置文件)
    │    └── webpack.prod.js(生产模式配置文件)
    ├── node_modules (下载包存放目录)
    ├── src (项目源码目录，除了html其他都在src里面)
    │    └── 略
    ├── public (项目html文件)
    │    └── index.html
    ├── .eslintrc.js(Eslint配置文件)
    ├── babel.config.js(Babel配置文件)
    └── package.json (包的依赖管理配置文件)
```

## 修改 webpack.dev.js

因为文件目录变了，注意所有**绝对路径**需要回退一层目录才能找到对应的文件，**相对路径**不需要修改。

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ESLintWebpackPlugin = require('eslint-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');

module.exports = {
	// 1、开发环境development，生产环境 production 的配置
	mode: 'development',

	// 2、设置入口文件的路径，路径可以是相对路径，也可以是绝对路径，项目打包时运行的入口文件，默认值是 ./src/index.js
	entry: path.resolve(__dirname, '../src/main.js'),

	// 3、指定出口文件，项目打包成功后导出的文件
	output: {
		filename: 'js/bundle.js', // 设置打包后的文件名，将js文件输出到js目录
		path: undefined, // 开发模式没有输出，不需要指定输出目录
		// clean: true, // 开发模式没有输出，不需要清空输出结果
		// 告诉 webpack 在生成的运行时代码中可以使用哪个版本的 ES 特性。
		environment: {
			arrowFunction: false, // 是否支持箭头函数
		},
	},

	// 4、配置loader
	module: {
		// rules 配置loader的解析规则
		// test: 使用正则模式匹配文件。use设置解析文件需要用到的loader，单个loader直接使用字符串，多个loader使用数组
		rules: [
			// 处理css
			{
				test: /\.css$/i,
				// use 数组里面 loader 执行顺序是从右到左，从下到上
				use: [
					// 'style-loader', // 将js模块中的css通过创建style标签添加到html文件中
					MiniCssExtractPlugin.loader, // 提取css到单独的文件中
					'css-loader', // 将css编译为commonjs模块到js模块中
					{
						loader: 'postcss-loader',
						options: {
							postcssOptions: {
								plugins: ['postcss-preset-env'],
							},
						},
					},
				],
			},
			// 处理scss
			{
				test: /\.s[ac]ss$/i,
				use: [
					// 'style-loader', // 将 JS 字符串生成为 style 节点
					MiniCssExtractPlugin.loader, // 提取css到单独的文件中
					'css-loader', // 将 CSS 转化成 CommonJS 模块
					{
						loader: 'postcss-loader',
						options: {
							postcssOptions: {
								plugins: ['postcss-preset-env'],
							},
						},
					},
					'sass-loader', // 将 Sass 编译成 CSS
				],
			},
			// 处理图片
			{
				test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
				type: 'asset',
				parser: {
					// webpack 将按照默认条件，自动地在 resource 和 inline 之间进行选择：小于 10kb 的文件，将会视为 inline 模块类型，否则会被视为 resource 模块类型。
					dataUrlCondition: {
						// 小于10kb的图片转为base64，大于10kb的图片转为文件
						// 加载图片文件的时候需要发送http请求，加载base64的图片不需要发送http请求，这样做的好处就是加载小图片可以不使用http请求，减少http请求，缺点是打包后的js文件会大一点点
						maxSize: 10 * 1024, // 10kb
					},
				},
				generator: {
					filename: 'images/[hash:10][ext][query]', // 输出图片的路径和名字
				},
			},
			// 处理字体图标
			{
				test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
				type: 'asset/resource',
				generator: {
					filename: 'iconfont/[hash:10][ext][query]', // 输出文件的名字
				},
			},
			// 处理其他资源，比如音视频等
			{
				test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
				type: 'asset/resource',
				generator: {
					filename: 'media/[hash:10][ext][query]', // 输出文件的名字
				},
			},
			// 使用babel编译，兼容旧版本浏览器
			{
				test: /\.m?jsx?$/,
				exclude: /node_modules/, // 排除node_modules文件中的模块，node_modules是别人已经处理好的模块包，不需要重复处理
				use: {
					loader: 'babel-loader',
				},
			},
		],
	},

	// 5、插件的配置，扩展webpack的功能
  plugins: [
		// 自动生成一个HTML文件，并添加script标签导入webpack打包生成的出口文件bundle.js
		// 根据自己提供的HTML模板生成HTML文件
		new HtmlWebpackPlugin({
			template: path.resolve(__dirname, '../public/index.html'),
		}),
		// ESLint检查
		new ESLintWebpackPlugin({
			context: path.resolve(__dirname, '../src'), // context指定检查的目录
		}),
		// 提取css成单独文件
    new MiniCssExtractPlugin({
      filename: 'css/index.css',// 指定文件的目录，只能是相对路径
    }),
	],
	// 6、自动开启一个本地的服务
	devServer: {
    host: 'localhost',
		port: 3000,
		open: true,
  },
};
```

运行开发模式的指令：

- --config：提供 webpack 配置文件的路径，例如 `./webpack.config.js`

```shell
npx webpack serve --config ./config/webpack.dev.js
```

## 修改 webpack.prod.js

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ESLintWebpackPlugin = require('eslint-webpack-plugin');
const MiniCssExtractPlugin = require('mini-css-extract-plugin');
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');

module.exports = {
	// 1、开发环境development，生产环境 production 的配置
	mode: 'production',

	// 2、设置入口文件的路径，路径可以是相对路径，也可以是绝对路径，项目打包时运行的入口文件，默认值是 ./src/index.js
	entry: path.resolve(__dirname, '../src/main.js'),

	// 3、指定出口文件，项目打包成功后导出的文件
	output: {
		filename: 'js/bundle.js', // 设置打包后的文件名，将js文件输出到js目录
		path: path.resolve(__dirname, '../dist'), // 设置出口文件的路径，路径必须是一个绝对路径，path是所有文件输出路径
		clean: true, // 每次打包前清空dist目录，再进行打包
		// 告诉 webpack 在生成的运行时代码中可以使用哪个版本的 ES 特性。
		environment: {
			arrowFunction: false, // 是否支持箭头函数
		},
	},

	// 4、配置loader
	module: {
		// rules 配置loader的解析规则
		// test: 使用正则模式匹配文件。use设置解析文件需要用到的loader，单个loader直接使用字符串，多个loader使用数组
		rules: [
			// 处理css
			{
				test: /\.css$/i,
				// use 数组里面 loader 执行顺序是从右到左，从下到上
				use: [
					// 'style-loader', // 将js模块中的css通过创建style标签添加到html文件中
					MiniCssExtractPlugin.loader, // 提取css到单独的文件中
					'css-loader', // 将css编译为commonjs模块到js模块中
					{
						loader: 'postcss-loader',
						options: {
							postcssOptions: {
								plugins: ['postcss-preset-env'],
							},
						},
					},
				],
			},
			// 处理scss
			{
				test: /\.s[ac]ss$/i,
				use: [
					// 'style-loader', // 将 JS 字符串生成为 style 节点
					MiniCssExtractPlugin.loader, // 提取css到单独的文件中
					'css-loader', // 将 CSS 转化成 CommonJS 模块
					{
						loader: 'postcss-loader',
						options: {
							postcssOptions: {
								plugins: ['postcss-preset-env'],
							},
						},
					},
					'sass-loader', // 将 Sass 编译成 CSS
				],
			},
			// 处理图片
			{
				test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
				type: 'asset',
				parser: {
					// webpack 将按照默认条件，自动地在 resource 和 inline 之间进行选择：小于 10kb 的文件，将会视为 inline 模块类型，否则会被视为 resource 模块类型。
					dataUrlCondition: {
						// 小于10kb的图片转为base64，大于10kb的图片转为文件
						// 加载图片文件的时候需要发送http请求，加载base64的图片不需要发送http请求，这样做的好处就是加载小图片可以不使用http请求，减少http请求，缺点是打包后的js文件会大一点点
						maxSize: 10 * 1024, // 10kb
					},
				},
				generator: {
					filename: 'images/[hash:10][ext][query]', // 输出图片的路径和名字
				},
			},
			// 处理字体图标
			{
				test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
				type: 'asset/resource',
				generator: {
					filename: 'iconfont/[hash:10][ext][query]', // 输出文件的名字
				},
			},
			// 处理其他资源，比如音视频等
			{
				test: /\.(mp4|webm|ogg|mp3|wav|flac|aac)(\?.*)?$/,
				type: 'asset/resource',
				generator: {
					filename: 'media/[hash:10][ext][query]', // 输出文件的名字
				},
			},
			// 使用babel编译，兼容旧版本浏览器
			{
				test: /\.m?jsx?$/,
				exclude: /node_modules/, // 排除node_modules文件中的模块，node_modules是别人已经处理好的模块包，不需要重复处理
				use: {
					loader: 'babel-loader',
				},
			},
		],
	},

	// 5、插件的配置，扩展webpack的功能
  plugins: [
		// 自动生成一个HTML文件，并添加script标签导入webpack打包生成的出口文件bundle.js
		// 根据自己提供的HTML模板生成HTML文件
		new HtmlWebpackPlugin({
			template: path.resolve(__dirname, '../public/index.html'),
		}),
		// ESLint检查
		new ESLintWebpackPlugin({
			context: path.resolve(__dirname, '../src'), // context指定检查的目录
		}),
		// 提取css成单独文件
    new MiniCssExtractPlugin({
      filename: 'css/index.css',// 指定文件的目录，只能是相对路径
    }),
	],
	// 6、自动开启一个本地的服务，开发模式不需要启动服务
	// devServer: {
  //   host: 'localhost',
	// 	port: 3000,
	// 	open: true,
	// },
};
```

运行生产模式的指令：

```shell
npx webpack --config ./config/webpack.prod.js
```

## 配置运行指令

为了方便运行不同模式的指令，我们将指令定义在 package.json 中 scripts 里面

```json
// package.json
{
  "scripts": {
    "serve": "npx webpack serve --config ./config/webpack.dev.js",
    "build": "npx webpack --config ./config/webpack.prod.js"
  }
}
```

以后启动指令：

- 开发模式：`npm run serve`
- 生产模式：`npm run build`

# 代码压缩优化

代码的压缩只需要在生产模式下配置，开发模式不需要压缩。

## CSS压缩

CssMinimizerWebpackPlugin插件使用来压缩 CSS。

下载模块

```shell
npm i css-minimizer-webpack-plugin -D
```

添加该插件到你的webpack.prod.js 配置中，代码压缩只需要写在开发环境中，如下所示：

```js
const CssMinimizerPlugin = require("css-minimizer-webpack-plugin");

module.exports = {
  plugins: [
    // css压缩
    new CssMinimizerPlugin(),
	],
};
```

在webpack5中推荐把压缩相关的代码写在optimization中，详见 [optimization.minimizer文档](https://webpack.docschina.org/configuration/optimization/#optimizationminimizer)：

```js
const CssMinimizerPlugin = require('css-minimizer-webpack-plugin');
const TerserWebpackPlugin = require("terser-webpack-plugin");

module.exports = {
  plugins: [
    // css压缩
    // new CssMinimizerPlugin(),
	],
  // 优化
  // 在webpack5中推荐把压缩相关的代码写在optimization中
  optimization: {
    minimize: true, // 开启压缩，默认为true
		minimizer: [
			// 压缩css
      new CssMinimizerPlugin(),
      // 压缩HTML和JS的功能已经内置，生产模式下自动开启TerserWebpackPlugin插件的压缩功能，如果使用了 optimization.minimizer 选项，需要使用 '...' 来访问默认值。
      // '...',
      // 或者显示的使用webpack5内置的TerserWebpackPlugin插件
      new TerserWebpackPlugin(),
		],
	},
};
```

## HTML和JS压缩

生产模式默认开启了html 压缩和 js 压缩，不需要进行额外配置。

# 路径别名@

```js
module.exports = {
  resolve: {
		alias: {
			'@': path.resolve(__dirname, 'src/'),
		},
	},
}
```

# 总结

Webpack 基本使用，要掌握了以下功能：

- 两种开发模式

  - 开发模式：代码能编译自动化运行

  - 生产模式：代码编译优化输出

- Webpack 基本功能

  - 开发模式：可以编译 ES Module 语法

  - 生产模式：可以编译 ES Module 语法，压缩 js 代码

- Webpack 配置文件

  - 5 个核心概念
    - entry
    - output
    - loader
    - plugins
    - mode

  - devServer 配置

- Webpack 脚本指令用法

  - `npx webpack` 直接打包输出

  - `npx webpack serve` 启动开发服务器，内存编译打包没有输出

