---
layout: post
title:  "如何搭建个人网站"
category: personal website
date:   2017-11-26 14:01:00
categories: personal website

---


##### 写在开头

> 鸿洋 - [http://www.zhanghongyang.com](http://www.zhanghongyang.com)  
郭霖 - [http://www.guolin.tech](http://www.goileolee.tech)  
严振杰 - [http://www.yanzhenjie.com](http://www.yanzhenjie.com)  
...  
goileolee - [http://www.goileo.top](http://www.goileo.top)

这篇文章主要讲如何拥有自己的专属域名。你可以在属于自己的网页上展示你的github、微信、微博、QQ、Twitter、Email等信息，但主要是展示自己的作品。

> 玉刚大神说过：[树立个人品牌：让名企hr们主动来找你](http://blog.csdn.net/singwhatiwanna/article/details/19174597)

有了个人网站，可能就又多了一些学(zhuang)习(bi)的动力吧！
所以，拥有自己的网站，算是迈出了第一步！

##### 域名怎么来的

![注册域名](http://upload-images.jianshu.io/upload_images/4105122-a0963b37924c7e1f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/440)

好吧~ 我的域名是这么来的...
这里我们先选择第二个，BAT中的A。

![查询域名](http://upload-images.jianshu.io/upload_images/4105122-8d031d0e816b6c23.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

输入你想注册的域名，查询这个域名是否被别人注册了。
```
Tips:
我的域名是goileo.top，为了演示注册过程，显示未注册状态，所以这里以goileolee.top为例
```
![域名](http://upload-images.jianshu.io/upload_images/4105122-775774196484002b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里我选择了一个 .top的域名。
什么？
“我为什么选.top后缀的？”
额...
这个问题就触及到我的资产盲区了。
因为它是最“贵”的！！！

接下来，加入清单，结账。
至此，这个域名就是你的了。
域名注册成功后，需要进行实名认证。
不过你还得对它进行解析，这个我们下面详细说。
这时的域名什么都做不了，因为没有服务器，阿里的服务器可不免费。所以接下来介绍GitHub Pages。

##### 免费的服务器 GitHub Pages

- 什么是GitHub Pages？
> GitHub Pages is a static site hosting service

这里有官方的解释 [GitHub Pages - 传送门](https://help.github.com/categories/github-pages-basics/)
简单说，它能提供给你1G空间的存储，让你放东西，然后还能请求到。是不是很6...

![GitHub Pages Usage limits](http://upload-images.jianshu.io/upload_images/4105122-5be2a12c607a5594.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

GitHub Pages也有一些限制条件，我们主要用GitHub Pages来做个人站点，所以一切OK。
不过也有人认为 [用GitHub Pages做个人网站是不道德的 传送门](https://www.zhihu.com/question/20717014)
---
接下来，登录github.com，点击右上方头像旁的加号new repository。仓库名字为固定格式：
> 你的名字.github.io

![创建仓库，你的名字](http://upload-images.jianshu.io/upload_images/4105122-a6f061d341681ea9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

描述... 公开... 初始化README... 
仓库创建好了，这时你可以写一个简单的index.html文件放到仓库里，然后在浏览器中输入：你的名字.github.io/试试。
如：http://goileolee.github.io
有神奇的效果...

##### 域名与GitHub Pages相绑定

- 解析域名：
打开阿里云[https://www.aliyun.com/](https://www.aliyun.com/)
登录账号
打开控制台
点击解析

![域名位置](http://upload-images.jianshu.io/upload_images/4105122-37c686a83306d332.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/540)

![添加解析](http://upload-images.jianshu.io/upload_images/4105122-ecaad0146d9cbc35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

添加一个记录类型为CNAME，主机记录为@，记录值为github的仓库路径。
```
Tips:
记录类型和主机记录都有解释，很好理解。
记录值就是你希望请求域名跳转到的路径。
TTL值可以理解为缓存时间，这里是每10分钟刷新一次域名的记录值。
调试可以设置小点，正常使用后记录值不会轻易发生更改，可以将TTL值设置大一点。
```
![主机记录](http://upload-images.jianshu.io/upload_images/4105122-6a3f2e67b42479f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

看上图，主机记录决定了解析后的域名。如果需要实现带或不带www都能访问到网址，则建议同时添加一个主机记录为www的解析。
```
Tips:
这里注意一下：因为买域名的同时会有一个企业邮箱，Aliyun会默认生成邮箱的解析记录。
其中记录类型有txt和mx的，会影响到CNAME类型的解析。
```
![](http://upload-images.jianshu.io/upload_images/4105122-754ce03fbd4ceef6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![阿里攻城狮的回答](http://upload-images.jianshu.io/upload_images/4105122-31211746b6a6fd99.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

阿里攻城狮大佬讲得很清楚了，所以删除TXT和MX类型的解析就好了。

- 为仓库（你的名字.github.io）添加CNAME文件
在仓库页面点击Create new file，创建一个新文件；
文件名为CNAME；
内容为刚刚申请的域名goileolee.top
```
Tips:
没有www，表示解析到http://goileolee.top
请求流程为：
http://www.goileolee.top --> http://goileolee.github.io --> http://goileolee.top
```
至此，完成绑定！

 ##### 写在后头

现在已经可以通过访问自己的专属域名来打开指定网页。
我们的目的是搭建个人网站，不过，网站如何设计得需要前段技术HTML、CSS、JS，这就触及到我的知识盲区了。。哈哈，别急这里提供两种方式。

- 1、跳转到已有的博客
```
<script>
    href="http://www.jianshu.com/u/0b1304c41418";
</script>
```
- 2、运用GitHub上开源的框架
在GitHub上搜hexo、jekyll即可。

我的站点用到的开源主题是基于Vno-jekyll框架的更改，我自己做了点修改，加了目录和关于两个页面。欢迎Fork，如果还不错记得点Star。
[我的小站 - http://goileo.top](http://goileo.top)  
[我的GitHub - https://github.com/goileolee/goileolee.github.io](https://github.com/goileolee/goileolee.github.io)

感谢您的阅读！欢迎指正！
