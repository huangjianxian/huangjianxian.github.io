---
title: Hexo+github pages搭建个人独立网站，绑定域名全教程
date: 2019-09-08 16:22:38
postlink: 20190908a1
categories: Hexo
tags: Next主题

---

写日志可以记录我们的学习、有趣的经历等等，作为一个程序员，写博客更是显得尤为重要，这不仅可以记录我们的技术学习过程，还能让我们在写作的过程中梳理自己的知识，如果能够与网友交流，那更是有利于双方的技术成长。
![1-2019-11-26-21-36-45.png](https://file.hjxlog.com/blog/images/1-2019-11-26-21-36-45.png)
<!-- more -->

## 1 为什么要选择Hexo？

以前我们经常在一些知名的博客平台上面写博客，如CSDN，博客园 ，51CTO等等。但是这些平台功能都大同小异，但是有些专业化了，大部分都是写技术博客，如果你想在上面分享个生活上有趣的事或者写写文学文章之类的，就显得有点不太合适了。简书其实也是个不错的平台，但是太偏向文学了。



因此我想搭建一个属于自己的独立网站，可以在上面集中发布和管理自己的日志和其他一些东西，另一方面也可以在后期进行个性化定制。



个人搭建网站基本上有三个选择：

```
自己动手前端后端一步一步开发。
使用成熟的wordpress平台。
托管在其他平台，比如GitHub。
```

如果你是一个大学在读的学生，我觉得你有时间的话可以选择第一个，就是前端后台一起开发，在这个过程中可以学习到很多东西。博主之前就使用 springboot+thymeleaf+bootstrap+mysql 开发过一个个人网站，这个过程还是非常不错的，可以对整个涉及到的技术进行了解学习，增加企业级开发的经验。



但是1、2都需要花一笔钱去租用服务器，也是有点小贵的。因此可以考虑现在比较成熟的Hexo，它是一个快速、简单和强大的博客框架。你可以用Markdown编写文章，Hexo可以在几秒钟内生成具有漂亮主题的静态文件。然后托管在GitHub上面即可完成免费博客啦。

[Hexo官网](https://hexo.io/)

## 2 准备工作（Windows 10系统）

### 2.1 安装nodejs

因为Hexo需要nodejs环境，因此需要先下载安装Nodejs。点击[NodeJs官网](https://nodejs.org/en/)，下载最新版本。
![2-2019-11-26-21-36-58.png](https://file.hjxlog.com/blog/images/2-2019-11-26-21-36-58.png)
下载好一直next，选择一个文件夹位置在一直next即可完成，这个步骤很简单，就不放图了。

### 2.2 安装git

点击[git官网](https://git-scm.com/downloads)，下载安装包。
![3-2019-11-26-21-37-4.png](https://file.hjxlog.com/blog/images/3-2019-11-26-21-37-4.png)

点击next，选择文件夹位置，然后一直next到底就行了，选择默认配置就好，默认配置会将环境变量配置好的，不需要搞得花里胡哨的。安装好后鼠标右击应该有下图这两个选项了，出现就代表安装成功了。
![4-2019-11-26-21-37-10.png](https://file.hjxlog.com/blog/images/4-2019-11-26-21-37-10.png)

## 3 使用Hexo

### 3.1 安装Hexo

上面环境搭建好之后，在桌面点击鼠标右键，点击 “Git Bash Here” ，输入以下两条命令。

```
$ npm install -g hexo-cli
```

提示：输入的时候不要输入 $ 了，因为命令行本来就已经有了。下载需要几分钟，请耐心等待一下。

可以在复制之后在git窗选择 Shift+Insert 粘贴。有一些警告WARN是不影响使用的，放心。
![5-2019-11-26-21-37-18.png](https://file.hjxlog.com/blog/images/5-2019-11-26-21-37-18.png)

### 3.2 初始化Hexo

安装好Hexo之后，新建一个文件夹，如 E:\HEXO ，然后在该文件夹内鼠标右击，选择 "Git Bash Here" ，输入以下命令。

```
$ hexo init
```

稍等片刻即可完成，如图：
![6-2019-11-26-21-37-25.png](https://file.hjxlog.com/blog/images/6-2019-11-26-21-37-25.png)
文件结构如图所示：
![7-2019-11-26-21-37-35.png](https://file.hjxlog.com/blog/images/7-2019-11-26-21-37-35.png)
scaffolds是模版文件夹，当你新建文章时，Hexo 会根据 scaffold 来建立文件。

source文件夹是存放用户资源的地方。

themes是主题文件夹，Hexo 会根据主题来生成静态页面，待会我们会更换成比较流行的nexT主题。

然后再输入命令行进行本地调试，即可看到初始效果啦~

```
$ hexo s --debug
```

访问[http://localhost:4000/](http://localhost:4000/)即可看到效果：
![8-2019-11-26-21-37-43.png](https://file.hjxlog.com/blog/images/8-2019-11-26-21-37-43.png)

## 4 更换主题为NexT

上面虽然本地可以调试成功了，但是默认的主题实在不是特别好看。你可以选择去官网选择自己喜欢的主题，[官网主题链接https://hexo.io/themes/](https://hexo.io/themes/)



本篇教程选择的是当前流行的NexT主题，这个主题是我感觉用过的最好的一个了。

### 4.1 下载NexT主题

进入刚刚你创建的文件夹的themes里，比如我的 E:\HEXO\themes ，鼠标右击选择“Git Bash Here”输入以下两条命令中的一个：（这两个是一样的，只是有同学反应第一条命令不行，第二条就可以。）

```
$ git clone git@github.com:iissnan/hexo-theme-next.git

$ git clone https://github.com/iissnan/hexo-theme-next
```

![9-2019-11-26-21-37-56.png](https://file.hjxlog.com/blog/images/9-2019-11-26-21-37-56.png)
此时 themes 文件夹便多了一个next主题的文件夹。
![10-2019-11-26-21-38-7.png](https://file.hjxlog.com/blog/images/10-2019-11-26-21-38-7.png)

### 4.2 修改配置

打开 E:\HEXO （你的hexo根目录）下的 _config.yml 配置文件
![11-2019-11-26-21-38-13.png](https://file.hjxlog.com/blog/images/11-2019-11-26-21-38-13.png)
找到下面这段代码

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: landscape
```

将langscape替换成hexo-theme-next

```
theme: hexo-theme-next
```

重新在项目根目录下进行本地部署调试

```
$ hexo s --debug
```

即可看到效果
![12-2019-11-26-21-38-23.png](https://file.hjxlog.com/blog/images/12-2019-11-26-21-38-23.png)

## 5 部署到github

### 5.1 注册GitHub

如果你还没有GitHub账户的话，去[GitHub官网](https://github.com/)免费注册一个就好了。
点击sign up
![13-2019-11-26-21-38-30.png](https://file.hjxlog.com/blog/images/13-2019-11-26-21-38-30.png)
尽量取个好听的名字
![14-2019-11-26-21-38-35.png](https://file.hjxlog.com/blog/images/14-2019-11-26-21-38-35.png)

### 5.2 新建仓库

![15-2019-11-26-21-38-46.png](https://file.hjxlog.com/blog/images/15-2019-11-26-21-38-46.png)
新建的仓库名必须要是 yourusername.github.io ，比如我的就是huangjianxian.github.io ，否则等下不能绑定GitHub pages 访问。
![16-2019-11-26-21-38-56.png](https://file.hjxlog.com/blog/images/16-2019-11-26-21-38-56.png)

### 5.3 配置SSH密钥

为了使本地可以跟远程的github建立联系，需要在本地配置SSH密钥，这样我们就可以在本地直接提交代码到GitHub上。
如果你是第一次配置SSH，则配置一下git的username 和 email

```
 $ git config --global user.name "你要设置的名字"
 $ git config --global user.email "你要设置的邮箱"
```

之后生成SSH密钥：

```
$ ssh-keygen -t rsa -C "你刚刚设置的邮箱"
```

如果不需要设置密码的话，连续三个回车就好了。在这之后会得到两个文件： id_rsa 和 id_rsa.pub 
找到id_rsa.pub文件，用记事本打开，复制其内容。路径： C:\Users\J（你的用户名）\\.ssh
![17-2019-11-26-21-39-3.png](https://file.hjxlog.com/blog/images/17-2019-11-26-21-39-3.png)

### 5.4 在GitHub上添加SSH密钥

登录GitHub，在Settings里面选择 SSH and GPG keys ，然后点击 New SSH Key
![18-2019-11-26-21-39-14.png](https://file.hjxlog.com/blog/images/18-2019-11-26-21-39-14.png)
![19-2019-11-26-21-39-23.png](https://file.hjxlog.com/blog/images/19-2019-11-26-21-39-23.png)
 完成之后测试一下，在git bash输入：

```
$ ssh -T git@github.com
```

如果看到了你的用户名，则表示配置成功了。
![20-2019-11-26-21-39-34.png](https://file.hjxlog.com/blog/images/20-2019-11-26-21-39-34.png)

### 5.5 初始化GitHub pages

打开GitHub上面的仓库，点击settings

![21-2019-11-26-21-39-44.png](https://file.hjxlog.com/blog/images/21-2019-11-26-21-39-44.png)

拉到下面，在GitHub Pages那里选择一个主题，确定之后即可通过域名来访问啦~比如我的https://huangjianxian.github.io

![22-2019-11-26-21-39-52.png](https://file.hjxlog.com/blog/images/22-2019-11-26-21-39-52.png)

### 5.6 将本地Hexo文件部署到GitHub上

登录GitHub，打开之前新建好的仓库 username.github.io ，clone，选择SSH类型
![23-2019-11-26-21-40-2.png](https://file.hjxlog.com/blog/images/23-2019-11-26-21-40-2.png)
打开本地站点配置文件，如E:\HEXO （你的hexo根目录）下的 _config.yml 配置文件。



这里整个项目有两个_config.yml，文件。



一个是位于你的hexo根目录下面的，叫做**站点配置**文件。



另一个是位于你的主题文件夹目录下面的，叫做**主题配置**文件。



这里打开**站点配置**文件，找到deploy，比如我的是：

```
# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
  type: git
  repo: git@github.com:huangjianxian/huangjianxian.github.io.git
  branch: master
```

将repo替换成你的GitHub仓库的SSH链接即可。

在你的项目根目录下使用git bash，输入命令部署：

```
$ hexo d -g
```

如果有同学是出现这个报错：

```
ERROR Deployer not found:git
```

![24-2019-11-26-21-40-16.jpg](https://file.hjxlog.com/blog/images/24-2019-11-26-21-40-16.jpg)

则在git bash输入以下命令：

```
$ npm install hexo-deployer-git --save
```

再重新hexo d -g部署一下应该就可以了，如果还不行，可能是node.js版本太低之类的（之前就有人踩过这个坑）



稍等片刻之后，输入你的仓库主页地址访问看看~比如我的是 [https://huangjianxian.github.io](https://huangjianxian.github.io)
![25-2019-11-26-21-40-25.png](https://file.hjxlog.com/blog/images/25-2019-11-26-21-40-25.png)

## 6 绑定自己的域名

有朋友可能觉得上面这种域名太难记了，而且不好看。这时候你可以通过自己购买一个域名，然后绑定到GitHub pages上。



去阿里云购买一个域名
![26-2019-11-26-21-40-33.png](https://file.hjxlog.com/blog/images/26-2019-11-26-21-40-33.png)
查询，选好之后加入清单付款即可。



买好之后去域名控制台，选择解析
![27-2019-11-26-21-40-42.png](https://file.hjxlog.com/blog/images/27-2019-11-26-21-40-42.png)
如下图所示，记录值改为你的 username.github.io ，然后确定。
![28-2019-11-26-21-40-51.png](https://file.hjxlog.com/blog/images/28-2019-11-26-21-40-51.png)
然后在项目文件夹下面的source下面，比如我的 E:\HEXO\source ，新建一个文件 CNAME（注意是文件，不是文件夹） 然后用记事本打开，填写你的域名。比如我的，hjxlog.com

![29-2019-11-26-21-41-0.png](https://file.hjxlog.com/blog/images/29-2019-11-26-21-41-0.png)

再使用git bash部署一次

```
$ hexo d -g
```

这时候应该就可以使用自己的域名访问项目了（如果还不行可能是DNS解析比较慢，稍等几分钟）。

## 7 总结

Hexo+github pages 是我认为个人搭博客比较好的平台了，不需要花费很多钱去维护服务器。只需要搭建好之后安心写博客就行了。

这还是我第一次写这么长的博客，写完长舒了一口气，写了好几个钟头，比我想象中的要久。不过还是坚持了下来，希望以后也经常这样！努力！奋斗！写作不易，大家转载的话请注明出处，谢谢~
如果大家在这个搭建过程中有什么问题，欢迎评论~