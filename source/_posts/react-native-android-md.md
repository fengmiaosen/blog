---
title: React Native 搭建Android开发环境踩坑
date: 2016-11-18 14:56
tags: [React,React Native]
categories: 
    - react
    - React Native
---

> 心血来潮想体验下React Native，于是就按照[官方教程](https://facebook.github.io/react-native/docs/getting-started.html#content)和[React Native中文教程](http://reactnative.cn/docs/0.37/getting-started.html#content)搭建完Android开发环境，使用官方脚手架工具新建了一个测试demo。

<!-- more-->

```bash
react-native init AwesomeProject
cd AwesomeProject
react-native run-android
```

结果运行报错

```bash
FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:installDebug'.
> com.android.builder.testing.api.DeviceException: No connected devices!

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output.

BUILD FAILED

Total time: 12.603 secs
Could not install the app on the device, read the error above for details.
Make sure you have an Android emulator running or a device connected and have
set up your Android development environment.
Go to https://facebook.github.io/react-native/docs/getting-started.html
and check the Android tab for setup instructions.
```

以上错误信息中关键部分

```bash
Execution failed for task ':app:installDebug'.
> com.android.builder.testing.api.DeviceException: No connected devices!
```

* 大概意思是 *没有连接到设备*，此时需要我们启动一个安卓虚拟设备*（AVD）*，具体步骤如下：

1. 启动Android Studio —— Tools —— Android —— AVD Manager

![avd](http://7xt6po.com1.z0.glb.clouddn.com/avd.jpg)

2. 选中已有的Virtual Device 或者 Create Virtual Device，然后点击右侧Actions下的启动按钮启动虚拟设备。

3. 如果没有步骤2中默认创建好的Virtual Device，就需要Create Virtual Device
![hardware](http://7xt6po.com1.z0.glb.clouddn.com/hardware.jpg)

4. 再在官方的测试demo中运行`react-native run-android`就可以启动调试。


# 参考资料

* http://reactnative.cn/docs/0.37/getting-started.html#content
* https://facebook.github.io/react-native/docs/getting-started.html#content
* http://csbun.github.io/blog/2015/12/starting-react-native-with-android/



