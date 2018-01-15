---
layout: post
title:  "Android开发中用Fiddler抓包，更改后台接口数据"
category: Android
date:   2017-08-12 20:15:48
categories: Android

---

![1434282bz51izk5yf45y85.jpg](http://upload-images.jianshu.io/upload_images/4105122-6b5f220bfe7bae0d.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/320)

#### 1.什么是Fiddler

- **Fiddler**是一个使用 C# 编写的 **http 抓包工具**。它使用灵活，功能强大，支持众多的 http 调试任务，是 web、移动应用的开发调试利器。
- 同 **Httpwatch**、**Firebug** 这些抓包工具一样，Fiddler 够记录客户端和服务器之间的所有 http 请求，可以针对特定的 http 请求，分析请求数据、设置断点等。
- 但 Fiddler 更为强大的是，它还可以修改请求的数据，甚至可以实现请求自动重定向，从而修改服务器返回的数据。
- **Fiddler** 使用也十分方便。在打开 **Fiddler** 的时候，它就自动设置好了浏览器的代理，通过改写 http 代理，让数据从它那通过，来监控并且截取到数据。当关闭 **Fiddler** 的时候，它又自动帮你把代理还原。
- 下载地址：[https://www.telerik.com/download/fiddler](https://www.telerik.com/download/fiddler)


#### 2.走进Fiddler

![Fiddler主界面说明](http://upload-images.jianshu.io/upload_images/4105122-8cdcefe462e34be9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 前面介绍了Fiddler是web、移动应用的开发调试利器。所以，打开Fiddler请求网页，左边的接口列表区域就会显示你所请求的每个接口。
- 点击左侧列表中的某个请求，在右侧的Inspectors功能菜单下就可以看到该接口非常详细的请求、返回数据。

![Fiters介绍](http://upload-images.jianshu.io/upload_images/4105122-a4e4ea97eaa496a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 如果只想要看到自己app请求的接口信息，则可以使用Fiters进行过滤。
Zone：指定只显示内网（Intranet）或互联网（Internet）的内容
Host：指定显示某个域名下的会话
这里以jianshu.com为例。

![过滤器](http://upload-images.jianshu.io/upload_images/4105122-61e4889964ac3e11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 如果想修改服务器返回的接口数据，则可以使用AutoResponder功能。


#### 3.改变接口返回的数据*

- AutoResponder 允许你拦截指定规则的请求，并返回本地资源或 Fiddler 资源，从而代替服务器响应。通常我们在开发App项目的初期，如果出现App端前期工作已经做完了，但服务器端接口联调通过但数据不够完善。这时我们就可以通过Fiddler来修改服务器返回的接口数据了。当然Fiddler还有很多用处，比如某个App在某种情况下限制你进入，那么可以尝试用Fiddler修改接口数据。
- 配置

![Tools-Options](http://upload-images.jianshu.io/upload_images/4105122-c9f5bee3ad6b52cc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![允许远程连接](http://upload-images.jianshu.io/upload_images/4105122-8e17e9076eed6ef7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里默认端口号是8888，重启Fiddler。然后要求手机和电脑连接到同一网络，Windows系统可以通过cmd-ipconfig查看到ip地址。假设电脑的IP是192.168.1.1，将手机中wifi设置为如下图所示

![代理-手动](http://upload-images.jianshu.io/upload_images/4105122-c0775c03b730c159.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接着打开手机浏览器请求http://192.168.1.1:8888，在打开的页面中点击“FiddlerRoot certificate”。

![下载安装证书](http://upload-images.jianshu.io/upload_images/4105122-0ab44eaa6e2d5c56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后在AutoResponder 项中勾选以下两个。第一个复选框的作用是开启或禁用自动重定向功能。
第二个复选框的作用是不理会那些没满足我们处理条件的请求。

![勾选项](http://upload-images.jianshu.io/upload_images/4105122-6fcf8754b72dce35.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

配置完成，现在开始对接口的数据进行修改替换。从左边选择一个你想要修改返回数据的接口，点击Add Rule就会自动显示在右边区域，直接按住左边不放拖过来也行。

![Add Rule](http://upload-images.jianshu.io/upload_images/4105122-8e758f2f30416f14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 接着添加一个你希望返回的json数据文件，用于替换掉服务器接口返回的数据。用Notepad++创建一个uesrinfo.txt文件作为接口返回数据。

![userinfo](http://upload-images.jianshu.io/upload_images/4105122-d8e4bfe776baa96b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

点击Find a file，选中userinfo.txt。到此，大功告成！

![Find a file](http://upload-images.jianshu.io/upload_images/4105122-7a4a34ae82ffc1c2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 最后，打开App，请求接口，看下Android Studio中返回的数据是否发生了改变。

![response](http://upload-images.jianshu.io/upload_images/4105122-b09aec05412a4fe8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

欢迎加入Android交流群155495090

感谢！

文中部分内容来自：[http://www.hangge.com/blog/cache/detail_1697.html](http://www.hangge.com/blog/cache/detail_1697.html)
