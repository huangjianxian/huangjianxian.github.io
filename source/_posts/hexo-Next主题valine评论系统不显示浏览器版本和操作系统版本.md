---
title: Hexo-Next主题valine评论系统不显示浏览器版本和操作系统版本
date: 2019-12-03 08:24:46
postlink: 20191203a1
categories: Hexo
tags: Next主题
---

hexo-next主题使用valine评论系统的时候，发现评论头像旁边会显示浏览器版本和操作系统版本，如图：

![微信图片_20191203083259-2019-12-3-8-33-15.png](https://file.hjxlog.com/blog/images/微信图片_20191203083259-2019-12-3-8-33-15.png)

个人觉得很突兀而且没有显示的必要，因此决定取消这两个信息的显示。
<!--more-->

1、进入你的theme目录下面的source/css文件夹，新建一个文件 "custom.styl"，如图

![345-2019-12-3-8-37-42.png](https://file.hjxlog.com/blog/images/345-2019-12-3-8-37-42.png)

2、复制下面这段代码到 custom.styl 文件中

```
.vsys{
    display:none !important;
}
```
3、引入刚刚新建的样式文件

在custom.styl同级的main.styl 中的最后添加以下代码，引入刚刚新建的样式文件


```
// custom
@import "custom.styl";
```

4、重新部署生成，效果如下：

![123-2019-12-3-9-3-28.png](https://file.hjxlog.com/blog/images/123-2019-12-3-9-3-28.png)
