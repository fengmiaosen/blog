---
title: Mac使用Python HTTP服务共享访问静态文件
date: 2016-05-06 11:54:06
tags: [开发工具,Python]
categories:
    - 开发工具
    - Python
comments: true
---

> 平常我们如果需要在本机共享访问静态资源，通常需要安装一个Web Server，如Apache、Nginx等。业内有很多包含Web Server、MySql的集成框架，如XAMP、WAMP等。
> 但有时候我们只是单纯需要一个简单的HTTP服务器来共享访问静态资源文件、图片等，而Python内置的HTTP服务模块就提供了这个功能，只需要很简单的命令，我们就可以把指定的目录和文件都以HTTP的方式展示出来。

<!-- more -->

# 启用Python HTTP服务器
例如：我们想要基于HTTP URL的方式来访问 `/Users/fengmiaosen/react-study` 

* 首先跳转到要访问的目录

```sh
cd /Users/fengmiaosen/react-study/react-15.0.2
```

* 然后执行如下命令，启动Python的HTTP服务器模块：

```sh
python -m SimpleHTTPServer
```

* 然后就看到执行结果提示，在浏览器中访问 `http://127.0.0.1:8000` 或者`http://localhost:8000` 就可以访问到上述目录，并进一步访问目录内资源。

```sh
Serving HTTP on 0.0.0.0 port 8000 ...
```

* 如果想指定访问端口号，可以使用如下命令：

```sh
python -m SimpleHTTPServer 8900
```

# 设置alias别名

> 为了省去每次输入这么长的命令，我们可以为这段命令设置alias，我使用`pserver`这个别名。

* 打开 `~/.bashrc` 或者 `~/.zshrc`，假如如下这段命令：

```sh
alias pserver='python -m SimpleHTTPServer'
```

* 执行 `source .bashrc` 或者  `source .zshrc` 使这个alias生效

* 然后我们就可以在终端窗口中，打开指定目录，输入 `pserver` 来启动Python的HTTP服务器。

![pserver](http://7xt6po.com2.z0.glb.clouddn.com/blog/pserver.png)

# 参考资料
* [非常简单的Python HTTP服务](http://coolshell.cn/articles/1480.html)
* [Simple HTTP request handler](https://docs.python.org/2/library/simplehttpserver.html)

