---
title: 解决brew search报错“Unknown option n”和“Unknown option 1”
date: 2016-11-23 21:38:02
tags: [shell,brew]
categories: 
    - macOS
    - shell
thumbnail: http://brew.sh/img/homebrew-256x256.png
---

# 问题描述

macOS 10.12终端使用brew搜索软件包

```
brew search pip
```
<!--more-->

搜索结果前包含如下一段错误信息：

```bash
Unknown option: n
Unknown option: 1
Usage: head [-options] <url>...
    -m <method>   use method for the request (default is 'HEAD')
    -f            make request even if head believes method is illegal
    -b <base>     Use the specified URL as base
    -t <timeout>  Set timeout value
    -i <time>     Set the If-Modified-Since header on the request
    -c <conttype> use this content-type for POST, PUT, CHECKIN
    -a            Use text mode for content I/O
    -p <proxyurl> use this as a proxy
    -P            don't load proxy settings from environment
    -H <header>   send this HTTP header (you can specify several)

    -u            Display method and URL before any response
    -U            Display request headers (implies -u)
    -s            Display response status code
    -S            Display response status chain
    -e            Display response headers
    -d            Do not display content
    -o <format>   Process HTML content in various ways

    -v            Show program version
    -h            Print this message

    -x            Extra debugging output
```

# 问题原因
* 在stackoverflow上搜到类似的一个问题—— [Head usage unknown option -1 / -n error. Possibly ruby related](http://stackoverflow.com/questions/21498991/head-usage-unknown-option-1-n-error-possibly-ruby-related)，其描述的问题原因是和系统 `PATH` 变量中 `/Applications/XAMPP/bin`有关。

# 解决方法

* 查看当前系统目录下的 `~/.bashrc` 或者 `~/.bash_profile`，有如下一段类似的设置系统`PATH`的命令

```bash
export PATH=/Applications/XAMPP/bin:$PATH
```
* 先把系统 `PATH` 保存到一个临时变量 `path`（名字随意）中

```
path=${PATH}
```
* 删除变量`path`中的 `/Applications/XAMPP/bin`部分，并赋值给系统 `PATH`

```
PATH=${path#/*XAMPP/bin:}
```

* 再次打开`~/.bashrc` 或者 `~/.bash_profile`，修改 `export PATH`部分，将`/Applications/XAMPP/bin`追加在系统`PATH`后面。

```bash
export PATH=/Applications/XAMPP/bin:$PATH
```
修改为

```bash
export PATH=$PATH:/Applications/XAMPP/bin
```

* 保存并退出，执行 `source ~/.bashrc` 或者 `source ~/.bash_profile`使配置立即生效。

# 参考资料
* [Head usage unknown option -1 / -n error. Possibly ruby related](http://stackoverflow.com/questions/21498991/head-usage-unknown-option-1-n-error-possibly-ruby-related)
*  [Linux变量内容的删除、取代与替换](http://blog.csdn.net/zyq522376829/article/details/47335537)

