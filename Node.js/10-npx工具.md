# npx

## 补充

使用下述命令，可以在当前目录下初始化一个叫 `package.json` 的文件：

```
npm init -y
```

这个文件可以记录项目的一些配置。

## npx

`npx` 是 `npm5.2` 之后自带的一个命令，它的作用非常多，但是比较常见的是使用它来调用项目中的某个模块的指令。（注意，安装 `npm` 的时候，会默认安装上 `npx` ）

### 安装 webpack

在使用前，先使用 `npm` 来下载一个 `webpack` ：（会下载到 `node_modules` 文件夹下面）

注：从 `4` 版本后，`webpack` 开始依赖 `cli` 。

``` 
npm i webpack webpack-cli -D
```

如果想要在全局下载 `webpack` ，可以使用下述命令：

```
npm i webpack@需要指定的版本 -g
```

