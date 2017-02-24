---
title: npm常用命令学习
date: 2016-11-21 16:13:06
thumbnail: http://7xt6po.com1.z0.glb.clouddn.com/blog/npm.png
tags: [Node.js, npm]
categories: 
    - Node.js
---

> npm作为Node.js的模块包管理器，可用来安装任何发布在npmjs系统上的模块。

<!--more-->

# npm简介

在我们安装Node.js的时候npm就一起安装好，不过有时候我们本机上的`npm`不一定是最新版本，可以使用以下命令安装更新。

```bash
npm install npm@latest -g
```
运行下面的命令，查看各种信息。

```bash
# 查看 npm 命令列表
$ npm help

# 查看各个命令的简单用法
$ npm -l

# 查看 npm 的版本
$ npm -v

# 查看 npm 的配置
$ npm config list -l
```
# npm设置

* 镜像设置

由于在国内npm官方镜像地址经常被墙，建议设置npm镜像以加速模块下载过程，在这里推荐使用[淘宝npm镜像](https://npm.taobao.org/)。

```bash
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global
```

* 安装cnpm

如果不想修改npm官方镜像，也可以使用淘宝提供的[cnpm](https://npm.taobao.org/)来代替`npm`下载安装npm模块。

```bash
npm install -g cnpm --registry=https://registry.npm.taobao.org
```
使用cnpm的命令来安装npm模块

```bash
cnpm install [name]
```

# 常用npm命令

## npm init

`npm init`用来初始化生成一个新的`package.json`文件。使用 `-y` 参数表示你能接受` package.json` 文件的一堆默认值：

```bash
npm init
```
## npm set

`npm set`用来设置环境变量，等于为`npm init`设置了默认值。

例如使用以下命令设置了默认用户名、用户邮箱等信息

```bash
$ npm set init-author-name 'fengmiaosen'
$ npm set init-author-email 'fengmiaosen@gmail.com'
```
再使用`npm config list -l`查看配置信息就能看到所设置的。

```bash
init-author-email = "fengmiaosen@gmail.com"
init-author-name = "fengmiaosen"
```

## npm config

以下命令使得`npm install --save`和`npm install --save-dev`安装新模块时，允许的版本范围从上尖括号 `^` 改成波浪号`~`，即从允许小版本升级，变成只允许补丁包的升级。

```bash
$ npm config set save-prefix ~
```

* 波浪线 `~`会匹配小版本号，忽略更高的版本。

> 比如：~1.2.3 will match all 1.2.x versions but will miss 1.3.0.
 
* 上尖括号 `^` 会匹配中版本号，忽略更高的版本。

> 比如：^1.2.3 will match any 1.x.x release including 1.3.0, but will hold off on 2.0.0.

* 官方文档

 [npm config](https://docs.npmjs.com/cli/config)

## npm info

`npm info` 可以查看每个模块的具体信息。比如，查看`express`模块的信息

```bash
$ npm info express

{ name: 'express',
  description: 'Fast, unopinionated, minimalist web framework',
  'dist-tags': { latest: '4.14.0', rc: '4.0.0-rc4' },
  maintainers:
   [ 'dougwilson <doug@somethingdoug.com>',
     'hacksparrow <captain@hacksparrow.com>',
     'jasnell <jasnell@gmail.com>',
     'mikeal <mikeal.rogers@gmail.com>' ],
     
     ......
```
这个对象的每个成员，都可以直接从info命令查询。

```bash
$ npm info express description
$ npm info express dependencies
```

## npm search

`npm search`命令用于搜索npm仓库，它后面可以跟字符串，也可以跟正则表达式。

```bash
$ npm search 
```

* 目前存在一个问题是当配置使用淘宝npm镜像后，MacOS上 `npm search`命令报错，可以使用 `cnpm search` 命令来暂时替代。

## npm list

`npm list` 命令以树型结构列出当前项目安装的所有模块，以及它们依赖的模块

* 默认列出当前项目中安装的模块

```bash
npm list
```

* 加上global参数，会列出全局安装的模块

```bash
npm list -global
```

## npm install

### 全局安装

将一个模块安装到系统目录中，各个项目都可以调用。一般来说，全局安装只适用于工具模块，比如`eslint`和`gulp`

```bash
npm install -g gulp
```

### 本地安装

将一个模块下载到当前项目的`node_modules`子目录，只有当前项目中才能调用这个模块。

```bash
npm install <package name>
```

### 安装版本

* 在当前项目根目录下直接使用 `npm install`命令直接安装 `package.json`中所配置的依赖模块。

* `npm install`也支持直接输入Github代码库地址。 

* `npm install`命令总是安装模块的最新版本，如果要安装模块的特定版本，可以在模块名后面加上`@`和`版本号`

```bash
$ npm install sax@latest
$ npm install sax@0.1.1
$ npm install sax@">=0.1.0 <0.2.0"
```

### 安装参数

* `--save`：模块名将被添加到`dependencies`，可以简化为参数-S
* `--save-dev`: 模块名将被添加到`devDependencies`，可以简化为参数-D
* `--save-exact`参数，会在`package.json`文件指定安装模块的确切版本

## npm outdated

该命令用来查询使用模块是否已经更新。

* 查找全局模块是否已经更新

```
npm outdated -g 
```

* 也可以查看检验当前和历史版本：

```
npm view <package> versions
```

## npm update

`npm update` 命令可以更新本地安装的模块

* 升级当前项目的指定模块

```
npm update [package name]
```

* 升级全局安装的模块

```
npm update -global [package name]
```

* 使用`-S`或`--save`参数，可以在安装的时候更新`package.json`里面模块的版本号

## npm run

`npm run` 是 `npm run-script`的别名用法，用来执行 `package.json`中`script`对象中配置的脚本命令。

```
{
  "name": "demo",
  "devDependencies": {
    "mocha": "latest"
  },
  "scripts": {
    "test": "mocha test/"
  }
}
```
例如对以上项目执行 `npm run test`命令，即可调用执行`mocha test/`命令行。

* `npm run`命令会自动在环境变量`$PATH`添加`node_modules/.bin`目录，所以`scripts`字段里面调用命令时不用加上路径，这就避免了全局安装NPM模块，可以直接运行本地模块。

* 直接运行`npm run`不给出任何参数，就会列出`scripts`属性下所有命令

### 参数

npm run命令还可以添加参数。将参数传到所指定的命令行，需要参数之前要加上两个连词线 `--` 。


```
"scripts": {
  "test": "mocha test/"
}
```

```
$ npm run test -- anothertest.js
```

等同于

```
$ mocha test/ anothertest.js
```

### pre- 和 post- 脚本

* `npm run`为每条命令提供了`pre-`和`post-`两个钩子（hook）。

* 以`npm run test`为例，执行这条命令之前，npm会先查看有没有定义pretest和posttest两个钩子，如果有的话，就会先执行`npm run pretest`，然后执行`npm run test`，最后执行`npm run posttest`。


```  
"scripts": {
    "test": "karma start --log-leve=error karma.config.js --single-run=true",
    "pretest": "npm run lint",
    "posttest": "echo 'Finished running tests'"
  }
```

## [npm link](https://docs.npmjs.com/cli/link)

`npm link`命令会在npm的全局模块node_modules目录内，生成一个符号链接，指向模块的本地目录。这样我们在开发npm模块的时候，就可以边开发边试用。

1.在模块目录`src/myModule`下运行`npm link`命令，会在npm的全局模块目录内，生成一个符号链接文件，该文件的名字就是`package.json`文件中指定的文件名。这个时候，已经可以全局调用myModule模块了。但是，如果我们要让这个模块安装在项目内，还要下一步操作。

```bash
$ cd src/myModule
$ npm link
```

2.切换到项目目录`src/myProject`，再次运行`npm link`命令，并指定模块名。然后，就可以在你的项目中加载该模块了


```
$ cd src/myProject
$ npm link myModule
```

上面命令等同于生成了本地模块的符号链接

3.在js脚本中就可以通过require的方式来引用该模块了。

```
var myModule = require('myModule');
```

这样一来，myModule的任何变化，都可以直接反映在myProject项目之中。但是，这样也出现了风险，任何在myProject目录中对myModule的修改，都会反映到模块的源码中。

4.如果你的项目不再需要该模块，可以在项目目录内使用`npm unlink`命令，删除符号链接

```bash
$ cd src/myProject
$ npm unlink myModule
```

## npm bin

显示相对于当前目录的，Node模块的可执行脚本所在的目录（即.bin目录）。

## npm publish

用于将当前模块发布到npmjs.com

1. 申请用户名 `npm adduser`
2. 登录 `npm login`
3. 发布 `npm publish`

# 内置命令

* npm test

 内置命令，等同于执行`npm run test`

* npm start

 内置命令，等同于执行`npm run start`

* npm stop

 内置命令，等同于执行`npm run stop`

* npm restart

 内置命令，等同于执行`npm run restart`

# 命令实践推荐

## npm-run-all

模块用于运行多个`scripts`脚本命令，[点击查看](https://www.npmjs.com/package/npm-run-all)。

* 继发执行

```
$ npm-run-all build:html build:js
```

等同于

```
$ npm run build:html && npm run build:js
```

* 并行执行

```
$ npm-run-all --parallel watch:html watch:js
```

等同于

```
$ npm run watch:html & npm run watch:js
```

## live-server

此模块启动一个HTTP服务器，添加自动刷新功能，具体查看[文档](https://www.npmjs.com/package/live-server)


# 参考资料

* http://javascript.ruanyifeng.com/nodejs/npm.html
* https://docs.npmjs.com/
* https://npm.taobao.org/
* [npm命令解析](https://great-tiger.gitbooks.io/-/content/npm/npm%E5%91%BD%E4%BB%A4%E8%A7%A3%E6%9E%90.html)
* [如何使用NPM来管理你的Node.js依赖](http://www.infoq.com/cn/articles/msh-using-npm-manage-node.js-dependence)


