---
title: webpack之plugin内部运行机制
date: 2017-03-21
tags: [javascript,webpack]
categories: 
    - webpack
---

# 简介
[webpack](https://webpack.js.org/)作为当前最为流行的模块打包工具，几乎所有的主流前端开发框架（React、Vue等）都会将其作为默认的模块加载和打包工具。通过简单的配置项，使用各种相关的loader和plugin，我们就可以实现自动的模块依赖分析并打包，从而大大降低了前端项目的开发复杂度，明显提高了前端项目的开发效率。

其中，[plugin](https://webpack.js.org/plugins/)是webpack核心支柱功能，通过plugin（插件）webpack可以实现loader所不能完成的复杂功能，使用plugin丰富的自定义API以及生命周期事件，可以控制webpack编译流程的每个环节，实现对webpack的自定义功能扩展。

<!--more-->

# webpack基础概念

## entry

`entry`表示webpack编译的入口文件，通常由html通过script标签引入。

entry的配置参见[官方文档说明](https://webpack.js.org/concepts/entry-points/)。

## loader

`loader` 用于对模块的源代码进行转换。其类似于其他构建工具中“任务(task)”，并提供了处理前端构建步骤的强大方法。loader 可以将文件从不同的语言（如 TypeScript）转换为 JavaScript，或将内联图像转换为 data URL，或者将CSS文件、markdown等非js文件转换为js文件并`require`进来！

### loader特性

![loader特性](http://7xt6po.com1.z0.glb.clouddn.com/loader特性-1.png)

### loader使用方法

* webpack.config.js

```js
module.exports = {
  module: {
    rules: [
      {test: /\.css$/, use: 'css-loader'},
      {test: /\.ts$/, use: 'ts-loader'}
    ]
  }
};
```

* `require` 语句中使用

```js
require('style-loader!css-loader?modules!./styles.css');
```

## module

`Module`是webpack的中的核心实体，要加载的一切和所有的依赖都是Module，总之一切都是Module。

* webpack支持的模块类型：
    * [ ES2015模块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import)，使用`import`来加载
    * [CommonJS模块](http://www.commonjs.org/specs/modules/1.0/)，使用 `require() `加载。例如：Node.js模块
    * [AMD模块](https://github.com/amdjs/amdjs-api/blob/master/AMD.md)，使用`require` 加载。例如：require.js支持的异步加载模块
    * css/sass/less 模块文件
    * image等其它非js模块文件

webpack 通过 `loader` 可以支持各种语言和预处理器编写模块。loader 描述了webpack 如何处理非js模块，可将这些非js模块进行转换，然后可以通过普通的js模块加载方式来使用。

## chunk

`chunk` 是webpack使用[code splitting](https://webpack.js.org/guides/code-splitting/)后的产物，也就是按需加载的分块，装载了不同的 `module`。

###  module 和 chunk 的关系图：

![module_chunk](http://7xt6po.com1.z0.glb.clouddn.com/module_chunk.jpg)

### webpack将chunk分为三种类型

* entry chunk
   入口代码块包含了 webpack 运行时需要的一些函数，如 `webpackJsonp`, `__webpack_require__` 等以及依赖的一系列模块
   
* normal chunk
   普通代码块没有包含运行时需要的代码，主要指代那些应用运行时`动态加载`的模块，其结构有加载方式决定，如基于异步的方式可能会包含 `webpackJsonp` 的调用。
    
* initial chunk
`initial chunk`本质上还是`normal chunk`，不过其会在应用初始化时完成加载，往往这个类型的chunk由`CommonsChunkPlugin`生成。
~~与入口代码块对应的一个概念是`入口模块（`module 0），如果入口代码块中包含了入口模块 webpack 会立即执行这个模块，否则会等待包含入口模块的代码块，包含入口模块的代码块其实就是` initial chunk`。~~

### code splitting

利用webpack提供的code splitting功能可生成不同类型的chunk，具体请参照另外一篇文章[《基于webpack实现react组件的按需加载》](https://fengmiaosen.github.io/2017/01/08/webpack-react-lazy-load/)

# 如何编写一个webpack plugin

## plugin创建

plugin是一个具有 `apply`方法的 js对象。 apply方法会被 webpack的 `compiler`（编译器）对象调用，并且 `compiler` 对象可在整个 `compilation`（编译）生命周期内访问。

webpack插件的组成：

* 一个JavaScript`函数`或者`class`（ES6语法）。
* 在它的原型上定义一个`apply`方法。
* 指定挂载的webpack`事件`钩子。
* 处理webpack内部实例的特定数据。
* 功能完成后调用webpack提供的`回调`。

这里以我们常用的 `UglifyJsPlugin` 为分析示例。

```js
class UglifyJsPlugin {

	apply(compiler) {
		const options = this.options;
		options.test = options.test || /\.js($|\?)/i;
		
		......
		
		//绑定compilation事件
		compiler.plugin("compilation", (compilation) => {
			if(options.sourceMap) {
				compilation.plugin("build-module", (module) => {
					// to get detailed location info about errors
					module.useSourceMap = true;
				});
			}
			//绑定optimize-chunk-assets事件
			compilation.plugin("optimize-chunk-assets", (chunks, callback) => {
				const files = [];
				chunks.forEach((chunk) => files.push.apply(files, chunk.files));
            
            ......
            
				callback();
			});
		});
	}
}

module.exports = UglifyJsPlugin;

```

## plugin用法

由于plugin可以携带参数/选项，你必须在wepback配置中，向plugins属性传入 new实例。这里介绍两种常用的使用plugin的方式。

### webpack.config.js

```js
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');

module.exports = {
  
  module: {
    loaders: [
      {
        test: /\.(js|jsx)$/,
        loader: 'babel-loader'
      }
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(), //访问内置的插件
    new HtmlWebpackPlugin({template: './src/index.html'}) //访问第三方插件
  ]
};
```

### Node.js

> 目前webpack官方强烈建议在配置文件中使用plugins属性配置项来使用各种plugin，而在Node.js中通过`compiler.apply`来使用plugin这种方式并不推荐。

```js
  const webpack = require('webpack'); //运行时(runtime)访问 webpack
  const configuration = require('./webpack.config.js');

  let compiler = webpack(configuration);
  compiler.apply(new webpack.optimize.UglifyJsPlugin());

  compiler.run(function(err, stats) {
    // ...
  });
```

# webpack整体运行流程图

> TODO

![webpack整体流程](http://7xt6po.com1.z0.glb.clouddn.com/webpack整体流程.png)

# Tapable

webpack整体是一个插件架构，所有的功能都以插件的方式集成在构建流程中，通过发布订阅事件来触发各个插件执行。webpack核心使用[Tapable](https://github.com/webpack/tapable) 来实现插件(plugins)的binding（绑定）和applying（应用）。

## tapable介绍

tapable是webpack官方开发维护的一个小型库,能够让我们为javascript模块添加并应用插件。 它可以被其它模块继承或混合。它类似于NodeJS的 `EventEmitter` 类,专注于`自定义事件`的`发射`和`操作`。 除此之外, Tapable 允许你通过回调函数的参数访问事件的生产者。

## tapable核心函数

![Tapable](http://7xt6po.com1.z0.glb.clouddn.com/Tapable.png)

# compiler(编译器)和compilation(编译)

在webpack插件开发中最重要的两个核心概念就是 `compiler` 和 `compilation` 。理解他们是扩展webpack功能的关键。

## compiler

webpack官方对[compiler](https://webpack.js.org/development/how-to-write-a-plugin/#compiler-and-compilation)的解释如下：

> The compiler object represents the fully configured webpack environment. This object is built once upon starting webpack, and is configured with all operational settings including options, loaders, and plugins. When applying a plugin to the webpack environment, the plugin will receive a reference to this compiler. Use the compiler to access the main webpack environment

compiler对象代表的是配置完备的Webpack环境。 compiler对象只在Webpack启动时构建一次，由Webpack组合所有的配置项构建生成。

### 功能核心

`Compiler` 继承自前面我们介绍的`Tapable`类，其混合了 Tapable 类以吸收其功能来注册和调用自身的插件。 大多数面向用户的插件，都是首先在 Compiler 上注册的。 

![Compiler核心](http://7xt6po.com1.z0.glb.clouddn.com/Compiler核心-1.png)

### compiler事件钩子（Event Hooks）

webpack官方列出了Compiler的所有事件钩子，[点击查看](https://webpack.js.org/api/plugins/compiler/#event-hooks)

我们这里只介绍一些关键性的事件，大致按照webpack流程的执行顺序：

![webpack关键事件](http://7xt6po.com1.z0.glb.clouddn.com/webpack关键事件-1.png)

### compiler源码

* [点击查看](https://github.com/webpack/webpack/blob/master/lib/Compiler.js)

## compilation

webpack[官方解释](https://webpack.js.org/development/how-to-write-a-plugin/#compiler-and-compilation)如下：

> A compilation object represents a single build of versioned assets. While running webpack development middleware, a new compilation will be created each time a file change is detected, thus generating a new set of compiled assets. A compilation surfaces information about the present state of module resources, compiled assets, changed files, and watched dependencies. The compilation also provides many callback points at which a plugin may choose to perform custom actions.

`compilation` 对象代表了一次单一的版本构建和生成资源。当运行 webpack 开发环境中间件时，每当检测到一个文件变化，一次新的编译将被创建，从而生成一组新的编译资源。一个编译对象表现了当前的模块资源、编译生成资源、变化的文件、以及被跟踪依赖的状态信息。编译对象也提供了很多关键点`事件回调`供插件做自定义处理时选择使用。

### 功能核心

`Compilation`对象负责组织整个编译过程，包含了每个构建环节所对应的方法。前面提到的`Compiler`（编译器）的`run`方法中调用`compiler`方法开始编译，在编译过程中创建了一个`Compilation`对象。

![Compilation核心](http://7xt6po.com1.z0.glb.clouddn.com/Compilation核心.png)

### Compilation事件钩子（Event Hooks）

* 获取Compilation对象

```js
compiler.plugin("compilation", function(compilation) {
    //主要的编译实例
    //随后所有的方法都从 compilation.plugin 上得来
});
```

* Compilation（编译）事件钩子

![Compilation事件](http://7xt6po.com1.z0.glb.clouddn.com/Compilation事件.png)

* Compilation绑定事件示例：

```js
//这里一般只有一个块，除非你在配置中指定了多个入口
compilation.plugin('optimize-chunks', function(chunks) {
    //这里一般只有一个块，除非你在配置中指定了多个入口
    chunks.forEach(function (chunk) {
        //块含有模块的循环引用
        chunk.modules.forEach(function (module){
            //module.loaders, module.rawRequest, module.dependencies 等。
        });
    });
});
```

### compilation源码

* [点击查看](https://github.com/webpack/webpack/blob/master/lib/Compilation.js)

## 二者关系

* `compiler` 对象代表的是不变的webpack环境，是针对webpack的
* `compilation` 对象针对的是随时可变的项目文件，只要文件有改动，compilation就会被重新创建。

# Module

module 是 webpack 构建的核心实体，也是所有 module 的 父类，它有几种不同子类：`NormalModule` , `MultiModule` , `ContextModule` , `DelegatedModule` 等。但这些核心实体都是在构建中都会去调用对应方法，也就是` build() `。

build方法核心源代码[点击查看](https://github.com/webpack/webpack/blob/master/lib/NormalModule.js#L259)

# Template

`Template`是用来生成结果代码的。入口是`compilation`的`createChunkAssets`方法。

```js
//如果是入口，则使用MainTemplate生成结果，否则使用ChunkTemplate.
if(chunk.entry) {
   source = this.mainTemplate.render(this.hash, chunk, this.moduleTemplate, this.dependencyTemplates);
} else {
   source = this.chunkTemplate.render(chunk, this.moduleTemplate, this.dependencyTemplates);
}
```

## Template子类

* MainTemplate.js 用于生成项目入口文件
* ChunkTemplate.js 用于生成异步加载的js代码
* ModuleTemplate.js 用于生成某个模块的代码
* HotUpdateChunkTemplate.js 热更新chunk

## Template源代码

[ModuleTemplate源代码](https://github.com/webpack/webpack/blob/master/lib/ModuleTemplate.js#L13)

# 参考资料

* [命令行输入webpack的时候都发生了什么](https://github.com/DDFE/DDFE-blog/issues/12)
* [Webpack 源码（一）—— Tapable 和 事件流](https://segmentfault.com/a/1190000008060440)
* [webpack 源码解析](https://lihuanghe.github.io/2016/05/30/webpack-source-analyse.html)
* [细说 webpack 之流程篇](http://taobaofed.org/blog/2016/09/09/webpack-flow/)
* [从 Bundle 文件看 Webpack 模块机制](https://zhuanlan.zhihu.com/p/25954788)
* [如何写一个webpack插件（一）](https://github.com/lcxfs1991/blog/issues/1)
* [How to write a plugin?](https://webpack.js.org/development/how-to-write-a-plugin/)
* [Webpack中hash与chunkhash的区别，以及js与css的hash指纹解耦方案](http://www.cnblogs.com/ihardcoder/p/5623411.html)
* [插件 API](https://doc.webpack-china.org/api/plugins/)


