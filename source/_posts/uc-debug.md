---
title: UC浏览器开发者版（Android）调试页面
date: 2016-05-11 16:37:05
tags: [开发工具,Android]
categories: 
    - 开发工具
    - Android
comments: true
toc: true  //在此处设定是否开启目录，需要主题支持
---

# 准备工作

## 手机端

进入UC官方网站开发者中心（[网站地址](http://www.uc.cn/business/developer/)），下载Android平台的UC浏览器开发者版（[UCBrowser_Dev_RI.apk，下载地址](http://wap.uc.cn/index.php?action=PackageDown&do=ByPfid&product=UCBrowser&pfid=145&lang=zh-cn&bid=999&direct=true&from=dev-slp-dir-pc))，安装到手机中。

## 电脑端

* 安装 Chrome 或 Safari（推荐使用 Chrome）。支持 Chrome15+，以及 Safari5.1.4 以上版本。

<!-- more -->

# 连接手机与电脑端

## Wi-Fi 连接模式
* Wi-Fi模式下，保证手机与 PC 处于同一个无线网段即可。**推荐使用此模式**。

## USB 连接模式
* USB 连接模式需要搭建 Android SDK 开发环境或安装 adb工具，具体方法[参照](http://www.uc.cn/business/download/developer.pdf)。

# 调试方式
* 在手机上启动 UC 浏览器开发者版，并打开需要调试的页面。在 PC 上打开 Chrome 或 Safari
* 若是 Wi-Fi 连接模式，则在地址栏输入：`手机 IP + :9998`

> 例如手机 IP 为 `192.168.112.244`，则输入 `192.168.112.244:9998`。

此时手机端的 UC浏览器开发者版会弹出对话框，如下图所示：

![uc-debug](http://7xt6po.com2.z0.glb.clouddn.com/blog/uc-debug.jpeg)

选择`确定`，允许调试。

* 若是 USB连接模式，则在地址栏输入：http://localhost:9998

* 成功访问该网址后，即可看到 UC 浏览器开发者版已打开索引页面：
![uc-pc](http://7xt6po.com2.z0.glb.clouddn.com/blog/uc-pc.png)

* 接下来，点击任一需要调试的页面即可进行调试。调试方法与 PC 上 Chrome 或Safari 开发者工具的调试方法类似。

	* 浏览器端如图所示：
![uc-chrome](http://7xt6po.com2.z0.glb.clouddn.com/blog/uc-pc-console.png)

	* 手机端如图所示：
![uc-b](http://7xt6po.com2.z0.glb.clouddn.com/blog/uc-b.jpeg)






