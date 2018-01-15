---
layout: post
title:  "使用EditText遇到的问题及解决方案"
category: Android
date:   2017-08-14 20:15:48
categories: Android

---


####S：

在开发中，产品给的要求是，在填写地址的输入框内只能有两行内容，输入框在横屏的Dialog上，软键盘半屏显示且点击软键盘的右下角按钮实现actionDone功能。

####T：

上面的场景可能有点复杂，先一个个问题来解决。
- dialog横屏下，软键盘半屏显示。
- 软键盘右下角按钮为完成，即点击关闭软键盘，而不是换行。
- 只显示两行内容。

####A：

1. dialog横屏下，软键盘半屏显示。
imeOptions="flagNoExtractUi"
设置前

![设置前](http://upload-images.jianshu.io/upload_images/4105122-752e5bd084e89556.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

设置后

![设置后](http://upload-images.jianshu.io/upload_images/4105122-91954d7a3ea14eb5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. 软键盘右下角按钮为完成，即点击关闭软键盘，而不是换行。
imeOptions="actionDone|flagNoExtractUi"
inputType="text"
3. 只显示两行内容。
xml中
```
android:inputType=”text|textCapSentences”
```
java中
```
mEditText.setHorizontallyScrolling(false);
mEditText.setMaxLines(2);
```

####R：

三个步骤实现不同的功能需求。
另外在SDK 23及以后，singleLine过时，以下代码能实现其相同功能。
```
android:maxLines=“1”
android:inputType="text"
```
如有不理解或是写的不对的地方，欢迎留言指出。
如果能帮你解决困难，是我的荣幸！同时也作备忘。

感谢！
