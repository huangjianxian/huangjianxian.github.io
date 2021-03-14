---
title: 将Hexo部署到阿里云轻量服务器（保姆级教程）
date: 2019-11-30 13:22:45
postlink: 20191130a1
categories: Hexo
tags: 
    - 阿里云 
    - Next主题
---
![02020-1-3-17-53-57](https://file.hjxlog.com/blog/images/02020-1-3-17-53-57)
<!--more-->
## 1 前言
作为有梦想的，有追求的程序员，有一个自己的个人博客简直就是必须品。你可以选择wordpress这种平台，直接使用，在任何地方只要有网络就能写博客。还可以选择hexo这种静态博客，但是发文章就没有那么随心所欲了，要在你部署的电脑上写才行。

本人之前使用的是hexo+github pages这种方式搭建博客，简单方便，感兴趣的同学可以看我之前写的这篇文章：[Hexo+github pages搭建个人独立网站，绑定域名全教程](http://hjxlog.com/19090816.html)

由于github是在国外的，访问速度确实慢了一些，体验有些不好。正巧上学的时候用学生优惠价格购买了几年阿里云轻量服务器，闲着也是闲着，就用来部署hexo好了，加快访问速度。

因为我是先部署了github pages，所以git安装、NodeJs安装等等都已经实现了，还没安装的有需要可以看我上面那个教程链接。

在部署的过程中，看了网上很多教程，大多是抄来抄去的，看了很多篇照着做也没有成功过，所以这篇博文是综合网上很多篇博文，加上自己的探索写出来的。网上普遍没有贴图片，看着头大，为了照顾纯小白，我这里贴上图片，一步一步来，保姆级教程。

## 2 环境介绍
我本地电脑安装的是win10(64)位。

服务器使用的是阿里云轻量服务器，配置是：

```
1核、2GB内存，系统盘 40GB SSD云盘
```

操作系统是CentOS 7.3。

## 3 本地环境部署

这部分在我之前写的[Hexo+github pages搭建个人独立网站，绑定域名全教程](http://hjxlog.com/19090816.html)这里也有，为了方便我直接拿过来这边了。

### 3.1 安装nodejs
因为Hexo需要nodejs环境，因此需要先下载安装Nodejs。点击[NodeJs官网](https://nodejs.org/en/)，下载最新版本。

![2-2019-11-26-21-36-58.png](https://file.hjxlog.com/blog/images/2-2019-11-26-21-36-58.png)

下载好一直next，选择一个文件夹位置在一直next即可完成，这个步骤很简单，就不放图了。

### 3.2 安装git

点击[git官网](https://git-scm.com/downloads)，下载安装包。
![3-2019-11-26-21-37-4.png](https://file.hjxlog.com/blog/images/3-2019-11-26-21-37-4.png)

点击next，选择文件夹位置，然后一直next到底就行了，选择默认配置就好，默认配置会将环境变量配置好的，不需要搞得花里胡哨的。安装好后鼠标右击应该有下图这两个选项了，出现就代表安装成功了。
![4-2019-11-26-21-37-10.png](https://file.hjxlog.com/blog/images/4-2019-11-26-21-37-10.png)

## 4 使用Hexo

### 4.1 安装Hexo

上面环境搭建好之后，在桌面点击鼠标右键，点击 “Git Bash Here” ，输入以下两条命令。

```
$ npm install -g hexo-cli
```

提示：输入的时候不要输入 $ 了，因为命令行本来就已经有了。下载需要几分钟，请耐心等待一下。

可以在复制之后在git窗选择 Shift+Insert 粘贴。有一些警告WARN是不影响使用的，放心。

![5-2019-11-26-21-37-18.png](https://file.hjxlog.com/blog/images/5-2019-11-26-21-37-18.png)

### 4.2 初始化Hexo

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

### 4.3 更换主题为NexT

上面虽然本地可以调试成功了，但是默认的主题实在不是特别好看。你可以选择去官网选择自己喜欢的主题，[官网主题链接https://hexo.io/themes/](https://hexo.io/themes/)

本篇教程选择的是当前流行的NexT主题，这个主题是我感觉用过的最好的一个了。

### 4.4 下载NexT主题

进入刚刚你创建的文件夹的themes里，比如我的 E:\HEXO\themes ，鼠标右击选择“Git Bash Here”输入以下两条命令中的一个：（这两个是一样的，只是有同学反应第一条命令不行，第二条就可以。）

```
$ git clone git@github.com:iissnan/hexo-theme-next.git

$ git clone https://github.com/iissnan/hexo-theme-next
```

![9-2019-11-26-21-37-56.png](https://file.hjxlog.com/blog/images/9-2019-11-26-21-37-56.png)

此时 themes 文件夹便多了一个next主题的文件夹。

![10-2019-11-26-21-38-7.png](https://file.hjxlog.com/blog/images/10-2019-11-26-21-38-7.png)

### 4.5 修改配置

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

## 5 配置SSH密钥

为了使本地可以跟远程的github建立联系，需要在本地配置SSH密钥，这样我们就可以在本地直接提交代码到GitHub上或者远端git仓库。
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

## 6 服务器部署
注意：服务器是 centOS 7.3

阿里云截图如下：

![1-2019-11-30-16-22-18.png](https://file.hjxlog.com/blog/images/1-2019-11-30-16-22-18.png)

点击右上角的"远程连接"。

![2-2019-11-30-16-26-8.png](https://file.hjxlog.com/blog/images/2-2019-11-30-16-26-8.png)

输入以下命令，切换到root账号

```
sudo su root
```

![3-2019-11-30-16-26-31.png](https://file.hjxlog.com/blog/images/3-2019-11-30-16-26-31.png)

### 6.1 git配置

1、安装git

在刚刚的黑框里输入，然后回车：

```
yum install git
```
等待一下就安装好了。中途会出现
```

Is this ok [y/d/N]: 

```
输入 ： y 回车即可

![4-2019-11-30-16-27-42.png](https://file.hjxlog.com/blog/images/4-2019-11-30-16-27-42.png)

此时git已经安装成功。

2、创建git账户

在命令框输入（下面不做重复提示了）

```
adduser git
```

3、添加git账户权限

```
chmod 740 /etc/sudoers
vim /etc/sudoers
```
![5-2019-11-30-16-28-12.png](https://file.hjxlog.com/blog/images/5-2019-11-30-16-28-12.png)

输入上面的命令，回车之后，进入编辑界面。

![6-2019-11-30-16-28-30.png](https://file.hjxlog.com/blog/images/6-2019-11-30-16-28-30.png)

这里要先点击 "i" 键，进入编辑模式，然后找到一下内容的地方：

```
## Allow root to run any commands anywhere
root    ALL=(ALL)     ALL
```

![7-2019-11-30-16-28-45.png](https://file.hjxlog.com/blog/images/7-2019-11-30-16-28-45.png)

添加以下内容：
```
git     ALL=(ALL)     ALL
```

然后按 "Esc" 键，此时最底下的--INSERT--消失，再输入 ":wq"，即保存退出。

4、改回权限
```
chmod 400 /etc/sudoers
```

5、设置git账户密码

```
sudo passwd git
```

输入两次密码就设置成功了。注意，linux下输入密码是不显示****的，你直接输入，输完回车就行了。

![8-2019-11-30-16-29-3.png](https://file.hjxlog.com/blog/images/8-2019-11-30-16-29-3.png)

6、切换至git用户，创建 ~/.ssh 文件夹和 ~/.ssh/authorized_keys 文件，并赋予相应的权限

```
su git
mkdir ~/.ssh
vim ~/.ssh/authorized_keys
```

按"i"进入编辑模式，将我们在win10中生成的id_rsa.pub文件中的公钥复制到authorized_keys中，按"esc"，然后按":wq"，保存退出。

![9-2019-11-30-16-29-28.png](https://file.hjxlog.com/blog/images/9-2019-11-30-16-29-28.png)

接着，输入一下命令，赋予权限

```
chmod 600 /home/git/.ssh/authorized_keys
chmod 700 /home/git/.ssh
```

在本地Git终端中测试是否能免密登录git，其中SERVER为填写自己的云主机IP，执行输入yes后输入你之前配置的git密码，无报错就说明好了。

在电脑本地桌面，右键"Git Bash Here"，输入一下命令，其中SERVER填写自己的云主机ip，执行输入yes后不用密码说明配置成功了。

```
ssh -v git@SERVER
```

如果你之前配置过git，可能你会出现以下这种错误。

![微信图片_20191203082833-2019-12-3-8-28-59.png](https://file.hjxlog.com/blog/images/微信图片_20191203082833-2019-12-3-8-28-59.png)

可以看到

```
Offending ECDSA key in /c/Users/jonty/.ssh/known_hosts:2
```

是.ssh/known_hosts 这个文件第二行出现冲突了。只要用笔记本打开，将这个文件的第二行删掉即可。

![11-2019-11-30-16-29-57.png](https://file.hjxlog.com/blog/images/11-2019-11-30-16-29-57.png)

重新执行刚刚的命令

```
ssh -v git@你的服务器ip
```

![12-2019-11-30-16-30-12.png](https://file.hjxlog.com/blog/images/12-2019-11-30-16-30-12.png)

这说明已经连接成功了。

### 6.2 创建仓库目录及相关配置

1、创建目录
在var目录下创建repo作为Git仓库目录，返回服务端命令行切换到root账户，然后输入：

```
mkdir /var/repo
```

![13-2019-11-30-16-30-43.png](https://file.hjxlog.com/blog/images/13-2019-11-30-16-30-43.png)

赋予权限：

```
chown -R git:git /var/repo
chmod -R 755 /var/repo
```

接下来创建hexo目录作为网站根目录，并赋予权限：

```
mkdir /var/hexo
chown -R git:git /var/hexo
chmod -R 755 /var/hexo
```

接下来创建一个空白的git仓库

```
cd /var/repo
git init --bare hexo.git
```
![14-2019-11-30-16-31-7.png](https://file.hjxlog.com/blog/images/14-2019-11-30-16-31-7.png)

创建一个新的 Git 钩子，用于自动部署.

在 /var/repo/hexo.git 下，有一个自动生成的 hooks 文件夹。我们需要在里边新建一个新的钩子文件 post-receive。

```
vim /var/repo/hexo.git/hooks/post-receive
```

进入编辑模式，然后将下面那两行代码粘贴进去，保存退出。

```
#!/bin/bash
git --work-tree=/var/hexo --git-dir=/var/repo/hexo.git checkout -f
```

修改权限：

```
chown -R git:git /var/repo/hexo.git/hooks/post-receive
chmod +x /var/repo/hexo.git/hooks/post-receive
```

到这里Git仓库已经搭建完毕了。
## 7 配置Nginx
为了部署和维护，我们使用宝塔面板来一键部署Nginx

```
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && bash install.sh
```

中途输入"y"回车等待一会就好了。在执行结果最后会出现地址，用户名，密码等。

![15-2019-11-30-16-31-40.png](https://file.hjxlog.com/blog/images/15-2019-11-30-16-31-40.png)

复制这个地址打开，输入账号密码即可进入宝塔面板。

![16-2019-11-30-16-31-50.png](https://file.hjxlog.com/blog/images/16-2019-11-30-16-31-50.png)

注意：这里也有可能你进不去面板页面，是因为你的服务器没有开8888这个端口（具体看你的宝塔面板连接的端口），去阿里云轻量服务器控制台中的“安全”->“防火墙”，右上角的"添加规则"，添加相应的端口即可。看下面的第二张图。

![17-2019-11-30-16-32-0.png](https://file.hjxlog.com/blog/images/17-2019-11-30-16-32-0.png)

![18-2019-11-30-16-32-7.png](https://file.hjxlog.com/blog/images/18-2019-11-30-16-32-7.png)

另外如果忘记了宝塔用户名密码，可以去你服务器终端输入：

```
 cd /www/server/panel && python tools.py panel testpasswd
```
 下图为宝塔解决方案，链接[忘记Linux 3.X/4.x/5.x/6.x 宝塔面板密码的解决方案](https://www.bt.cn/bbs/thread-1172-1-1.html)
 
 ![19-2019-11-30-16-32-26.png](https://file.hjxlog.com/blog/images/19-2019-11-30-16-32-26.png)

 进入面板之后，会提示叫你修改端口，点击"立即修改",可以看到"面板端口"这时候是8888，自己选一个值，然后先去服务器防火墙上开放这个端口，跟刚刚的"添加规则"操作一样。

 再回到宝塔面板页面将"面板端口"的值修改成你刚刚开放的端口值。

 然后需要用新端口，重新进入宝塔面板，就是将原有的链接":"后面的值改成你的端口即可。
 
 ![20-2019-11-30-16-32-37.png](https://file.hjxlog.com/blog/images/20-2019-11-30-16-32-37.png)

 在宝塔面板，进入软件商店，输入"Nginx"，然后搜索，安装免费的那个。

![21-2019-11-30-16-32-50.png](https://file.hjxlog.com/blog/images/21-2019-11-30-16-32-50.png)

 等待部署完成。

 部署完成之后，点击网站，添加站点，填写你的域名，没有的话写你的服务器ip地址。其他的不要改。

![22-2019-11-30-16-33-1.png](https://file.hjxlog.com/blog/images/22-2019-11-30-16-33-1.png)

填写完之后提交，然后点击”设置"

![23-2019-11-30-16-33-9.png](https://file.hjxlog.com/blog/images/23-2019-11-30-16-33-9.png)

 点击"配置文件"

 ```
server
{
    listen 80;
    # server_name填写你自己的域名，没有的话填ip
    server_name hjxlog.com;
    index index.php index.html index.htm default.php default.htm default.html;
    # 这里root填写自己的网站根目录，修改为/var/hexo
    root /var/hexo;
```
保存，然后选择“设置”-“网站目录”，将网站目录修改成以下，保存。
```
/var/hexo
```
![24-2019-11-30-16-33-18.png](https://file.hjxlog.com/blog/images/24-2019-11-30-16-33-18.png)

回到服务器终端，重启宝塔服务，使之生效。
```
service bt restart
```

## 8 修改hexo配置
进入本地电脑hexo博客的根目录，编辑**站点配置文件 _config.yml**，找到deploy，修改成以下

```
deploy:
  type: git
  #repo改为repo: git@你的域名:/var/repo/hexo.git
  repo: git@hjxlog.com:/var/repo/hexo.git
  branch: master
```

最后在本地电脑hexo博客的根目录右击，Git Bash Here，输入以下命令部署

```
hexo clean
```

```
hexo d -g
```

这时候可能出现权限问题，导致部署到git失败。

![25-2019-11-30-16-33-43.png](https://file.hjxlog.com/blog/images/25-2019-11-30-16-33-43.png)

在服务器终端输入以下命令即可：

```
chown -R git:git /var/repo/
chown -R git:git /var/hexo/
```

最后再hexo d -g部署，应该就可以看到部署成功了。

![26-2019-11-30-16-34-0.png](https://file.hjxlog.com/blog/images/26-2019-11-30-16-34-0.png)

自此，已经将博客从GitHub pages搬到阿里云服务器了。

![27-2019-11-30-16-34-7.png](https://file.hjxlog.com/blog/images/27-2019-11-30-16-34-7.png)

## 9 将网站添加https访问
去阿里云申请一个免费的SSL证书，好像要备案，有点忘记了，很快就申请下来了。

进入宝塔面板，"网站"，"设置"，"SSL，"其他证书"

![28-2019-11-30-16-34-22.png](https://file.hjxlog.com/blog/images/28-2019-11-30-16-34-22.png)

将你申请的证书*.key以及*.pem内容，粘贴进去然后保存即可。

![29-2019-11-30-16-34-28.png](https://file.hjxlog.com/blog/images/29-2019-11-30-16-34-28.png)

## 10 持续更新 
> 2020-03-15 根据评论和邮件反馈遇到的常见问题更新

![12020-3-15-11-17-2](https://file.hjxlog.com/blog/images/12020-3-15-11-17-2)

根据有同学遇到上图这种情况，如果提示

```
Could not resolve hostname ***.com: Name or service not known
Please make sure you have the correct access rights
```

那么看一下自己的域名是否实名验证成功了，如果已验证。可以ping一下域名看能不能ping得通，有可能是你没有把域名解析到服务器ip地址上了。

另外，Mac OS可能在配置方面和Windows不太一样。具体问题要具体分析，具体解决了。

## 11 总结
在部署网站的时候，由于之前对Linux没有经验，网络上看到几篇博客，还是没能部署成功，有些博客比如命令中的单词拼写错误，或者前后不衔接，导致部署一直有问题。

还好在部署的同时去网上学一些Linux基本命令，学到了进入编辑模式，编辑完怎么退出等等。之前看了一篇博客，可能作者默认大家都会了，结果我卡在这里了一下。

总的来说，部署还是比较简单的，第一次没成功的同学不要轻易放弃，多去网上找找答案，努力解决问题，加油！

如果因为看我的博客，在部署过程当中出现任何问题，请私信我或者邮箱联系我，帮助大家一起解决问题。