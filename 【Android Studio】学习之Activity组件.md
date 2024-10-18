---
title: Android Studio——Activity组件
tags: Android Studio 
---

# Android Studio学习之Activity组件实验报告

实验目标：

1、掌握Activity的注册；

2、掌握Activity的生命周期；

3、掌握Intent，实现Activity之间的跳转；

4、请设计实验验证Activity的生命周期；请设计实验验证跳转时Activity生命周期的状态变化

------

[TOC]

------



## 一：Activity的原理

1.1Activity是安卓系统中负责用户可视化界面交互的关键组件，称为活动组件（也称为界面程序），它会加载布局文件（属于资源文件）例如 setContentView(R.layout.activity_main)

------



## 二：Activity的基本用法

### 2.1创建Activity文件

点击 Empty Activity 创建名为 FirstActivity 的 Activity

![image-20241012132759211](D:\Myblog\source\typora-user-images\image-20241012132759211.png)

新建一个Activity文件

![image-20241012133142016](D:\Myblog\source\typora-user-images\image-20241012133142016.png)

### 2.2 创建布局和加载布局

右键 app/src/main/res 目录->New->Directory，会弹出一个新建目录的窗口，现在这里创建一个名为 layout 的目录，然后对着 layout 目录右键->New->Layout resource file，优惠弹出一个新建布局资源文件的窗口，我们将这个布局文件命名为first_layout，根元素默认选择为 LinearLayout。

![image-20241012134007420](D:\Myblog\source\typora-user-images\image-20241012134007420.png)

可以点击预览窗口看到对应的代码

![image-20241012134117184](D:\Myblog\source\typora-user-images\image-20241012134117184.png)

#### 2.2.1在布局中添加按钮，如图

![image-20241012134447918](D:\Myblog\source\typora-user-images\image-20241012134447918.png)

#### 2.2.2 加载布局文件

项目中每添加一个元素都会在 R 文件中响应的生成一个资源 id，因此刚才添加的布局文件的 id 就已经添加到了 R 文件中，所以可以通过`R.layout.first_layout`找到 first_layout.xml 的 id，然后将这个值传入到 `setContentView()` 方法即可。

![image-20241012135245016](D:\Myblog\source\typora-user-images\image-20241012135245016.png)

### 2.3 在 AndoirdManifest.xml 文件中注册

> [!NOTE]
>
> 如何找到AndroidManifest.xml文件：
>
> 在Project目录下，在项目结构面板中，展开app文件夹，找到src/main目录找到AndroidManifest.xml文件

![image-20241012155547752](D:\Myblog\source\typora-user-images\image-20241012155547752.png)

可以看到，Activity 的注册声明要放在 `<application>` 标签中，这里通过 `<activity>` 标签来对 Activity 进行注册。在`<activity>`中通过 `name` 属性指定具体注册哪一个 Activity，那么这里填入 `.FirstActivity` ，前面加 “.” 是因为最外层已经声明了 `package` 属性，在 `name` 的地方添加`.FirstActivity` 即可通过全类名找到 `FirstActivity`。

此时已经注册了Activity,但是还不能运行程序，因为需要配置 Activity，也就是说需要指定最先启动哪个 Activity。所以需要在 `<activity>` 标签中添加 `intent-filter` 标签，然后在 `intent-filter` 标签中添加 `<action android:name="android.intent.action.MAIN" />` 和 `<category android:name="android.intent.category.LAUNCHER" />` 两行声明即可。

这样操作之后，FirstActivity 就成了这个应用的主 Activity 了，点击应用图标最先打开的就是这个 Activity。但是如果没有在应用中声明任何一个 Activity 作为主 Activity，这个程序依然是可以安装的，只是**无法在启动器中看到这个应用程序**。这种程序通常作为第三方服务供其它应用在内部进行调用。运行成功！

![image-20241012155727600](D:\Myblog\source\typora-user-images\image-20241012155727600.png)

------



## 三.使用 Intent 在 Activity 之间穿梭

Intent 是 Android 中各组件之间进行交互的一种重要方式，它不仅可以置名当前组件想执行的动作，还可以在不同的组件之间传递数据。Intent 一般可用于启动 Activity、启动 Service以及发送广播等场景。Intent 大致分为两种：显式和隐式。

### 3.1使用显式的Intent

#### 3.1.1创建SecondActivity并修改布局，如下图所示：

![image-20241012162229749](D:\Myblog\source\typora-user-images\image-20241012162229749.png)

#### 3.1.2修改FirstActivity文件内容：

![image-20241012192034425](D:\Myblog\source\typora-user-images\image-20241012192034425.png)

#### 3.1.3运行虚拟机完成跳转

![image-20241012193347723](D:\Myblog\source\typora-user-images\image-20241012193347723.png)

![image-20241012193410152](D:\Myblog\source\typora-user-images\image-20241012193410152.png)

------



## 四：Activity的生命周期

### 4.1Activity的生命周期

Activity的生命周期是指Activity从创建到销毁的过程。Android 是使用任务来管理 Activity 的，一个任务就是一组存放在栈里的 Activity 的集合，这个栈也被称为返回栈（back stack）。栈是一种后进先出的数据结构，在默认情况下，每当我们启动新的 Activity，它就会在返回栈中入栈，并处于栈顶的位置。而每当我们按下 Back 键或调用 `finish()` 方法后，处于栈顶的 Activity 就会出栈，前一个入栈的 Activity 就会重新处在栈顶的位置，下图展示了返回栈如何管理 Activity 入栈出栈操作。

![image-20241012195606742](D:\Myblog\source\typora-user-images\image-20241012195606742.png)

### 4.2Activity的状态

（1）**活动状态——Active**：Activity在用户界面中处于最上层（处于栈顶），完全被用户看到，且能与用户进行交互。

（2）**暂停状态——Pause：**Activity在界面上被部分遮挡，不再处于最上层（ 不再处于栈顶的位置，但仍然可见），且不能和用户进行交互。（如弹出消息框）

（3）**停止状态——Stop：**Activity被其他Activity全部遮挡，界面完全被用户看不见，当其它地方需要内存时，处于停止状态的 Activity 有可能会被系统回收。。（如玩游戏时来电显示）

（4）**非活动状态——Dead：**从返回栈中移除后就变成了销毁状态，Activity没有被启动或者被finish（）。

### 4.3Activity的生存期

Activity 类中定义了7个回调方法，覆盖了 Activity 生命周期的每一个环节。

![image-20241012195656442](D:\Myblog\source\typora-user-images\image-20241012195656442.png)

### 4.4Activity生命周期的验证实验

新建一个Activity,并写入以下代码，如图所示：

![image-20241012204224011](D:\Myblog\source\typora-user-images\image-20241012204224011.png)

![image-20241012204444917](D:\Myblog\source\typora-user-images\image-20241012204444917.png)

点击运行键运行虚拟机，打开Android Studio右下方的logcat，就可以看到代码运行的log日志，如下所示：

（1）点击run后虚拟机运行成功且出现Activity窗口时，logcat的显示：

![image-20241012205241791](D:\Myblog\source\typora-user-images\image-20241012205241791.png)

（2）按下主屏幕键，再按下任务视图，点击还原该窗口时，logcat的显示：

![image-20241012205355830](D:\Myblog\source\typora-user-images\image-20241012205355830.png)

（3）按下退出键，Activity结束，logcat显示：

![image-20241012205949647](D:\Myblog\source\typora-user-images\image-20241012205949647.png)

这样一个程序的生命周期就完成啦！*^-^*

### 4.5验证页面跳转时的生命周期变化

利用上面的FirstActivity和SecondActivity完成该实验的验证。

在FirstActivity.java和SecondActivity.java文件中分别写入生命周期的函数。（此处不一一展示，和上面的单个程序的生命周期布局相同）运行APP，在logcat中查看生命周期。

点击运行键，过程如下所示：

（1）首先启动FirstActivity,查看：

![image-20241013102620418](D:\Myblog\source\typora-user-images\image-20241013102620418.png)

（2）然后点击FirstActivity的跳转按钮：

![image-20241013102819473](D:\Myblog\source\typora-user-images\image-20241013102819473.png)

（3）点击SecondActivity的返回按钮回到上一界面：

![image-20241013103012940](D:\Myblog\source\typora-user-images\image-20241013103012940.png)

（4）关闭程序：

![image-20241013103138593](D:\Myblog\source\typora-user-images\image-20241013103138593.png)

可以看到（绿框部分）：FirstActivity先onPause了，SecondActivity执行了onCreate,当SecondActivity的onResume执行了，FirstActivity才stop,这个是不是和我们经常看的activity的生命周期是一样的。由此得到了验证。

总结：耗时两天终于搞懂了Activity的大致用法。昨日之深渊，今日之浅潭。路虽远行则将至，事虽难做则可成。共勉！
