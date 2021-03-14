---
title: VScode+Picgo+阿里云OSS上传博客图片
postlink: VScode-Picgo-Alibaba-Cloud-OSS-upload-blog-pictures
date: 2019-12-30 13:34:39
categories: Tools
tags: VSCode
---
在写博客的过程中，采用是markdown格式，图片是博客中重要的组成部分。由于本身博客采用的是静态网页，如果直接把图片放在文件夹下面，以后会变得越来越臃肿。在网上搜索了一番后，最后确定上传图片的方案为：VSCode为markdown编辑器+VSCode插件Picgo，阿里云OSS为图床。

在配置的过程中遇到了一些坑，这里记录下，免得踩坑。

![微信图片_201912301347512019-12-30-13-49-2](https://file.hjxlog.com/blog/images/微信图片_201912301347512019-12-30-13-49-2)
<!--more-->
这里面的Area填的是你仓库的地区，比如深圳的是"oss-cn-shenzhen"。

Bucket就是仓库名称了。

Custom Url是自定义域名：这里要写开头的http/https，写完整了。

Path是路径，比如我想放在我的仓库的images文件夹下面。这里有个坑：在开头不用写"/"，比如"blog/images/"，否则会报错。