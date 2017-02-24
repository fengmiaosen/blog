---
title: Mac 使用Beyond Compare执行svn diff和svn merge
date: 2016-06-06 19:18:56
thumbnail: https://tortoisesvn.net/assets/img/tortoisesvn_logo_hor468x64.497712e4.png
tags: [开发工具, svn]
categories:
    - 开发工具
    - svn
---

# 安装 Beyond Compare

[下载安装地址](http://www.scootersoftware.com/download.php)

# 安装 Command Line Tools

打开 Beyond Compare，菜单栏中点击安装命令行工具 Command Line Tools

<!-- more -->

![beyond](http://7xt6po.com1.z0.glb.clouddn.com/beyond.jpg)

# 设置svn

## svn diff
* 创建 **/usr/local/bin/bcdiff.sh** 脚本
* 执行如下命令添加脚本执行权限
```sh
chmod +x /usr/local/bin/bcdiff.sh
```
* 在上述 *bcdiff.sh* 脚本中添加如下执行命令：
```sh
#!/bin/bash
/usr/local/bin/bcompare "$6" "$7" -title1="$3" -title2="$5" -readonly
exit 0
```

* 打开并编辑 ****$HOME/.subversion/config** 配置文件，在 *[Helpers]* 部分添加如下命令行：
```sh
diff-cmd = /usr/local/bin/bcdiff.sh
```

## svn merge
* 创建 **/usr/local/bin/bcmerge.sh** 脚本
* 执行如下命令添加脚本执行权限
```sh
chmod +x /usr/local/bin/bcmerge.sh
```
* 在上述 *bcmerge.sh* 脚本中添加如下执行命令：
```sh
#!/bin/bash
/usr/local/bin/bcompare "$2" "$3" "$1" "$4"
```

* 打开并编辑 **$HOME/.subversion/config** 配置文件，在 *[Helpers]* 部分添加如下命令行：
```sh
merge-tool-cmd = /usr/local/bin/bcmerge.sh
```

# 参考资料
* [Using Beyond Compare with Version Control Systems](http://www.scootersoftware.com/support.php?zz=kb_vcs#svn)

