# Node.js 

## Node.js 是什么？

官方定义：`Node.js` 是一个基于（V8）`JavaScript` 引擎的 `JavaScript` 运行时环境。它本身也是一个应用程序，它的存在建立在V8引擎之上。

也就是说，`Node.js` 基于 V8 引擎来执行 `JavaScript` 代码，但不仅仅只有 V8 引擎：

- V8 可以嵌入到任何的 C++ 应用程序中，无论是 Chrome 还是 Node.js ，事实上都是==嵌入了 V8引擎来执行JavaScript代码==；
- 在 Chrome 浏览器中，还需要解析，渲染HTML，CSS等相关渲染引擎，另外需要提供支持浏览器操作的 API，浏览器自己的时间循环等等；
- 另外，在 Node.js 中我们也需要进行一些额外的操作，比如文件系统读/写，网络IO，加密，压缩解压文件等。



## Node.js 架构 

- javascript 代码会经过 V8 引擎，再通过 Node.js 的 Bindings，将任务放在 ==Libuv== 的事件循环中；
- libuv 是使用 C 语言编写的库；
- libuv 提供了时间循环，文件系统的读写，网络IO，线程池等等；

![image.png](https://s2.loli.net/2023/09/05/zyEiYXQg2ktOWwe.png)



## Node.js 应用场景

1. 目前前端开发的库==都是以 Node 包的形式进行管理==（之前的库都是使用 script 标签来引入的，但是现在不需要了，可以直接使用 ==npm==，==yarn==，==pnpm==，来直接下载包，然后再需要使用的地方 ==import== 来引入。）；
2. ==npm==，==yarn==，==pnpm==等工具是前端开发中使用最多的工具；
3. 越来越多的公司使用 Node 作为 Web 服务器的开发，中间件，代理服务器；
4. 大量项目需要借助 Node 完成前后端渲染的同构应用；
5. ==编写脚本工具==；
6. 很多企业在使用 ==Electron== 来开发桌面应用程序（Electron 也是借助于 Node）；

所以，前端的开发，是离不开 Node 的。



## Node.js 安装

Node 有两个版本，一个是 ==LTS== 版本，一个是 ==Current== 版本；

- LTS：相对稳定一些，是长期支持的版本；
- Current：最新的 Node 版本，包含了很多新的特性，随时会变，而且一些新的特性不是很稳定；



