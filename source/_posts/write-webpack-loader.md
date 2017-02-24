---
title: 如何编写一个webpack loader
date: 2017-01-07 10:56:01
tags: [webpack, Node.js]
categories: 
    - webpack
---

![webpack](http://7xt6po.com1.z0.glb.clouddn.com/webpack-1.png)

<!--more-->


## loader是什么

### webpack[官方定义](https://webpack.js.org/api/loaders/)

> _A loader is just a JavaScript module that exports a function_

* 从语法角度看，loader就是一个普通的Node.js模块，只是必须以函数格式导出来供使用。如果有必要可以使用一切Node.js功能模块。

* 从功能角度看，一个loader是在应用中作用于指定格式的资源文件并将其按照一定格式转换输出。例如：sass-loader将scss文件转换为标准css文件输出。

```js
// base loader
var fs = require("fs");

module.exports = function(source) {
  return source;
};
```

### loader 的种类

* `preloaders` 就是在调用loader之前需要调用的loader, 他不做任何代码的转换，只是进行检查
* `loaders` 就是我们本文章重点讲述的用来处理文件，做代码转换的loader，例如将 scss文件转换为css文件。
* `postloaders` 就是在调用loader之后需要调用的loader，比如进行`代码覆盖率测试`。

### loader如何使用

* loader 是支持链式执行的，如处理 sass 文件的 loader，可以由 sass-loader、css-loader、style-loader 组成，由 compiler 编译器对其由右向左执行，第一个 loader 将会拿到需处理的原内容，上一个 loader 处理后的结果回传给下一个接着处理，最后的 Loader 将处理后的结果以 String 或 Buffer 的形式返回给 compiler。

* 每个 loader 只做该做的事，纯粹的事，而不希望一箩筐的功能都集成到一个 loader 中。

* loader函数还可以选择使用 JSON 格式来传递数据。

### loader 调用语法

* 命令行调用

```
webpack --module-bind jade --module-bind 'css=style!css'
```

* require调用

在需要作用的资源文件名前面加上`loader`名称和 `！`感叹号。

```js
require('style-loader!css-loader?modules!./styles.css');
```

* webpack配置文件

> webpack 2.x语法中需要写全loader全名，不再允许省略` -loader`

**语法格式一：**

> webpack 1.x语法

```js
{
    module: {
        loaders: [{
            test: /\.scss$/,
            loader: 'style!css!sass'
        }]
    }
}；
```
> webpack 2.x语法

```js
{
    module: {
        loaders: [{
            test: /\.scss$/,
            loader: 'style-loader!css-loader!sass-loader'
        }]
    }
}；
```

**语法格式二：**

> webpack 1.x

```js
{
    module: {
        loaders: [{
            test: /\.scss$/,
            loader: ['style','css','sass']
        }]
    }
}；

```
**语法格式三：**

> webpack 2.x 语法

```js
{
    module: {
        rules: [{
            test: /\.scss$/,
            use: [
                'style-loader',
                'css-loader',
                {
                    loader: "sass-loader",
                    options: {
                        /* ... */
                    }
                }
            ]
        }]
    }
}；
```

### loader 执行顺序

当一个文件从文件系统中被读取进入到loader执行序列中，其执行次序如下：

1. webpack配置文件中 `preloaders` 配置项 `（webpack2.x移除）`
2. webpack配置文件中 `loaders` 配置项
3. request请求中指定的loaders，例如：
```
require('raw!./file.js')
```
4. webpack配置文件中 `postloaders` 配置项`（webpack2.x移除）`

> 在某些情况下，我们需要在js模块请求中覆盖webpack配置文件中的loaders顺序来实现一些特定需求。具体规则如下：

* 模块请求参数最前面添加感叹号 `!`来禁用配置文件中的`preLoaders`

```
require("!raw!./script.coffee")
```

* 模块请求参数最前面添加双感叹号 `!!`来禁用配置文件中所有的`loaders`

```
require("!!raw!./script.coffee")
```

* 模块请求参数最前面添加 `-!`来禁用配置文件中`preLoaders`和`loaders`，除了`postLoaders`

```
require("-!raw!./script.coffee")
```

> 官方的建议

* 对于结果为js文件的情况，建议在`loaders`中处理
* 对于预先编译的非js（如coffee等）转换js文件，建议在`preloader`中处理，如果不是全局应用也可以在 `loaders`中处理
* 对于相同的语言，建议`preloader` 和 `postloader` 过程中处理


## 如何编写一个loader

### loader 输出处理结果

* 同步模式（sync mode）

直接返回loader的处理结果。


```js
module.exports = function(content) {
    return someSyncOperation(content);
};
```

* 异步模式（async mode）

调用 `this.async()`来获取`this.callback()`方法，然后在异步调用的回调函数中通过`callback`返回`null`以及`处理结果`。

```js
module.exports = function(content) {
    var callback = this.async();
    if(!callback) return someSyncOperation(content);
    someAsyncOperation(content, function(err, result) {
        if(err) return callback(err);
        callback(null, result);
    });
};
```

### Raw loader

默认情况下，不同loader之间是以UTF-8格式的文本字符串来传递数据。我们可以通过设置`module.exports.raw = true;` 来选择使用`Buffer`数据格式来传递数据，而编译器会负责相邻loader之间的格式转换。

```js
module.exports = function(content) {
    assert(content instanceof Buffer);
    return someSyncOperation(content);
    // return value can be a `Buffer` too
    // This is also allowed if loader is not "raw"
};
module.exports.raw = true;
```

### Pitching Loader

多个loader的链式调用默认都是遵循从右到左的顺序。但在某些情况下，loaders并不关心前一个loader的处理结果或者资源文件数据，它仅仅关心元数据`metadata`即原始输入内容。

而`pitch`方法则支持从左到右的执行顺序，当pitch方法执行完毕，loaders调用流程反转并跳过剩余未执行的loaders，继续调用当前loader更左侧的loaders。并且通过 `data`属性来在当前loader的`pitch`方法调用和 `normal`方法调用之间来传递数据。

```js
module.exports = function(content) {
    return someSyncOperation(content, this.data.value);
};
module.exports.pitch = function(remainingRequest, precedingRequest, data) {
    if(someCondition()) {
        // fast exit
        return "module.exports = require(" + JSON.stringify("-!" + remainingRequest) + ");";
    }
    data.value = 42;
};
```

## 如何做好loader开发

### 善用loader中的this

loader函数作用域内的 `this` 变量挂载了一些很有用的方法属性来实现诸如异步调用、query参数获取等功能。

* query则能获取到 Loader 上附有的参数。 如 `require("./somg-loader?ls")`; 通过 query 就可以得到 "ls" 了。
* `emitFile` 能够让开发者更方便的输出一个 file 文件，这是 webpack 特有的方法，使用的方法也很直接

```
emitFile(name: string, content: Buffer|String, sourceMap: {...})
```
### 启用结果缓存 cacheable

```js
module.exports = function(source) {
    this.cacheable();
    return source;
};
```

### 调用`exec` 方法执行一些代码片段。

```js
exec(code: string, filename: string)
```

### loader-utils

loader开发中经常用到的一个工具模块，其中提供了一些很有用的解析方法。[点击查看文档](https://github.com/webpack/loader-utils)

## code split

require.ensure使得我们可在所有的dependencies项加载完毕后，再执行回调

> require.ensure仅仅是加载组件，并不会执行，若要执行，需要借助传进去的require参数

```
require.ensure(["module-a", "module-b"], function(require) {
    var a = require("module-a");
    // ...
});
```

## 参考资料
* [如何开发一个 Webpack Loader ( 一 )](https://github.com/joeyguo/blog/issues/4)
* [Loader API](https://webpack.js.org/api/loaders/)
* [loaders](https://webpack.github.io/docs/loaders.html)
* [using-loaders](http://webpack.github.io/docs/using-loaders.html)
* [loader concepts](https://webpack.js.org/concepts/loaders/)
* [webpack: require.ensure与require AMD的区别](http://blog.csdn.net/zhbhun/article/details/46826129)



