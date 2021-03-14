---
title: Hexo Next主题中使用gitment评论功能
date: 2019-09-07 18:24:10
postlink: 20190907a1
categories: Hexo
tags: Next主题
---
这几天在折腾hexo搭建自己的博客，这个过程很简单，但是搭建完发表两篇文章之后发现没有评论功能体验就很不好了。后来看了一下配置文章发现hexo next主题已经内置了很多评论功能。有Disqus，畅言，valine，gitment等，对比之后发现还是使用gitment比较靠谱，毕竟是托管在GitHub上的，用起来比较稳。<!-- more -->其他第三方会因为各种需要实名、备案等问题，比较麻烦，也不稳定，比如畅言就关闭了。

[gitment地址](https://github.com/imsun/gitment)

## 1 在Hexo next主题中使用gitment
因为我用的是比较新的5.1.4版本，因此gitment已经集成好了，只需在主题文件夹下面的_config.yml中修改配置即可。如果你使用的是比较低的版本，可以选择升级一下版本或者在你的博客根目录下用git bash输入下面的命令安装：

```
npm i --save gitment
```

（低版本）执行完之后便在_config.yml中找到gitment的开关配置了。
![1-2019-11-26-21-42-41.png](https://file.hjxlog.com/blog/images/1-2019-11-26-21-42-41.png)

## 2 在github中申请应用
申请地址：[Register a new OAuth application](https://github.com/settings/applications/new)
![2-2019-11-26-21-42-50.png](https://file.hjxlog.com/blog/images/2-2019-11-26-21-42-50.png)
以上根据你的情况填写好之后，点击“Register application”既可以了。之后会看到**ClientID**和**Client Secret**,这两个后面会在hexo next主题配置中使用到。

## 3 配置gitment
在主题文件夹下面的_config.yml文件（路径：themes/next/_config.yml）找到gitment的配置，修改配置。如下图。
![3-2019-11-26-21-42-56.png](https://file.hjxlog.com/blog/images/3-2019-11-26-21-42-56.png)
gitment是把评论放在对应仓库的issue中的。
这时候应该就可以开通gitment评论功能了，发布测试一下。

## 4 效果
![4-2019-11-26-21-43-1.png](https://file.hjxlog.com/blog/images/4-2019-11-26-21-43-1.png)

## 5 遇到的问题

在测试的时候，出现了gitment无法登陆评论的问题（Object ProgressEvent）。
好像是因为原作者的网站证书挂了，于是网上找了一下解决方法。

```
\themes\next\layout\_third-party\comments\gitment.swig
```
找到上面路径的文件，找到以下代码：

```
<link rel="stylesheet" href="https://imsun.github.io/gitment/style/default.css">
<script src="https://imsun.github.io/gitment/dist/gitment.browser.js"></script>
```
替换成：

```
<link rel="stylesheet" href="https://jjeejj.github.io/css/gitment.css">
<script src="https://jjeejj.github.io/js/gitment.js"></script>
```
解决方案原地址：[https://github.com/imsun/gitment/issues/170](https://github.com/imsun/gitment/issues/170)
## 总结
经过以上设置，你应该就可以愉快的使用gitment了，有什么问题欢迎给我发邮件~