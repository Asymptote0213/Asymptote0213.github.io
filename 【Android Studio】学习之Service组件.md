---
title:【Android Studio】学习之Service组件
date:2024/10/15
updated:
tags:
categories:
toc:
type:
comments:
description:
keywords:Android Studio,Service组件
top_img:
---

Android Studio 学习之Service组件及应用

一：Service组件服务概念

1. **定义**：Service是Android四大组件之一，用于执行后台任务，如网络请求、音乐播放、文件下载等耗时操作，而不会阻塞用户界面。
2. **特点**：
   - 没有用户界面，运行于后台。
   - 生命周期由系统控制，不由开发者直接控制其实例化。
   - 可以执行耗时操作，但默认不会开启新线程，需要在Service中手动开启线程以避免阻塞主线程。

3.**启动方式**：
