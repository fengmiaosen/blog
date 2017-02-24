---
title: 创建github和gitlab帐号ssh key
date: 2016-04-28 18:00:57
tags: [github]
categories:
    - 开发工具
    - github
comments: true
---
> 有时候我们电脑上就有好几套ssh key，如github/gitcafe/gitlab,三者各不一样，为了保证各个服务能正常使用需要配置多个ssh key。可以按照以下的步骤来实现多套SSH Key的共同工作。

<!-- more -->

# 使用自定义名字生成SSH密钥

```sh
ssh-keygen -t rsa -C "YOUR_EMAIL@YOUREMAIL.COM" -f ~/.ssh/github
```

说明：

（1）记得把命令中的`YOUR_EMAIL@YOUREMAIL.COM`改为你的Email地址，-f后面的参数是自定义的SSH Key的存放路径，将来为github生成的ssh key的名字分别是`github.pub`和`github`，其中pub后缀的`github.pub`是我们后面提取ssh key信息的地方
（2）其他的和上面生成密钥的步骤相同，例如要生成`gitlab`的ssh key，只需要将名字改为gitlab

# 配置自定义的公秘钥名称

在SSH用户配置文件 `~/.ssh/config`中指定对应服务所使用的ssh key，如果没有config文件的话就新建一个，并输入以下内容(可以添加多个)：

```sh
Host github.com www.github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/github
Host gitlab.com gitlab.alibaba-inc.com
    IdentityFile ~/.ssh/gitlab
```

# 添加github的ssh key

## 获取ssh key信息
（1）通过文本编辑器打开github.pub，复制里面的所有内容以备下一步使用。
（2）通过cat命令，显示github.pub文件里的内容，复制后在下一步使用
```
cat github.pub
```
## 添加ssh key到github
* 登录github账户，打开账户 setting页面
![setting](http://7xt6po.com2.z0.glb.clouddn.com/blog/github-setting.png)

* 打开 SSH and GPG keys 设置页面
![SSH and GPG keys](http://7xt6po.com2.z0.glb.clouddn.com/blog/github-ssh.png)

* 点击 New SSH Key，来将前面复制好的ssh key信息添加到Key文本框当中，并给一个title

![new ssh](http://7xt6po.com2.z0.glb.clouddn.com/blog/github-new-ssh.png)

![ssh key](http://7xt6po.com2.z0.glb.clouddn.com/blog/github-add-ssh.png)

# 添加ssh key到gitlab

* 登录gitlab账户，打开Profile配置页面，选择SSH Keys
* 按照要求填写Title和Key，其中Title是Key的描述信息，Key是按照上面类似github方法复制的gitlab的SSH key信息，直接粘贴到输入框中保存

# 测试SSH连接
```
ssh -T git@github.com
```

出现如下类似的信息，说明配置成功
```
Hi fengmiaosen! You've successfully authenticated, but GitHub does not provide shell access
```

# 参考资料
* [GitLab的简单使用](http://blog.devzeng.com/blog/simple-usage-of-gitlab.html)
* [how-to-tell-git-which-private-key-to-use](http://superuser.com/questions/232373/how-to-tell-git-which-private-key-to-use)
* [git生成ssh key及本地解决多个ssh key的问题](http://riny.net/2014/git-ssh-key/)
* [多 SSH Key 管理技巧与 Git 多账户登录问题](http://www.barretlee.com/blog/2016/03/09/config-in-ssh-after-troubling-git-connection/)

