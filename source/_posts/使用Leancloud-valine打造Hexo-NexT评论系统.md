---
title: 使用Leancloud+valine打造Hexo-NexT评论系统
date: 2019-09-11 23:00:46
postlink: 20190911a1
categories: Hexo
tags: Next主题
---
之前使用gitment用作评论系统，每次都需要初始化评论页面，一开始搭博客，文章不多还好，要是文章很多，一篇一篇点击初始化，还是有点头疼的。GitHub上有人开发了插件可以批量初始化，感觉还是有点麻烦。

![1-2019-11-26-21-26-20.png](https://file.hjxlog.com/blog/images/1-2019-11-26-21-26-20.png)

<!--more-->

如果没有登录github，还显示"未开放评论"

![2-2019-11-26-21-26-26.png](https://file.hjxlog.com/blog/images/2-2019-11-26-21-26-26.png)

gitment需要用户使用GitHub账号登陆，可能有些游客刚想简单评论一下，看到登录，授权啥的，就失去了评论的兴趣，默默关掉了网页…经过一番比较之后，还是Valine比较适合，不需要登录，还可以是匿名的。

## 1 注册LeanCloud

点击 [LeanCloud官网](<https://leancloud.cn/>) ，注册一个账号，这里分为国内和国际版的。国内的在2019-10-1之后需要自定义已备案域名才能继续提供服务了，觉得麻烦的同学可以移步去国际版[LeanCloud国际版](<https://leancloud.app/>)。

## 2 创建应用

注册好之后，进行实名验证、邮箱验证等等。

点击创建应用

![3-2019-11-26-21-26-36.png](https://file.hjxlog.com/blog/images/3-2019-11-26-21-26-36.png)



给新应用起一个名称，选择开发版，创建即可。

![4-2019-11-26-21-26-43.png](https://file.hjxlog.com/blog/images/4-2019-11-26-21-26-43.png)

结果如图：

![5-2019-11-26-21-26-50.jpg](https://file.hjxlog.com/blog/images/5-2019-11-26-21-26-50.jpg)

## 3 设置Web安全域名，填入自己的域名

进入控制台，在“设置” -> "安全中心" -> "Web安全域名"，填写自己的博客网站域名。

![6-2019-11-26-21-26-58.jpg](https://file.hjxlog.com/blog/images/6-2019-11-26-21-26-58.jpg)

## 4 获取APP ID 和 APP Key

![7-2019-11-26-21-27-7.png](https://file.hjxlog.com/blog/images/7-2019-11-26-21-27-7.png)

接下来打开博客目录的next主题配置文件 _config.yml ，找到Valine，将上图的APP ID 和 APP Key复制到对应位置。

![8-2019-11-26-21-27-13.png](https://file.hjxlog.com/blog/images/8-2019-11-26-21-27-13.png)

如果之前使用了gitment，要找到gitment的配置位置，将enable改成false。避免出错。

## 5 测试

结果如图：

![9-2019-11-26-21-27-21.png](https://file.hjxlog.com/blog/images/9-2019-11-26-21-27-21.png)