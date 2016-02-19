---
layout: post
title: "UIResponder之UIViewController"
date: 2016-02-10 13:43:15
categories: Objective-C
---

# 目录

- [视图控制器概述](#1)
- [视图控制器的功能](#2)
- [根视图控制器](#3)

<a name = "1"></a>

# 视图控制器概述

- 视图控制器是应用**数据**和**视图**之间的重要桥梁，每个iOS应用程序只显示一个用户界面，显示的内容由一个或多个控制器协调管理。所以，**视图控制器提供了一个基本的框架来构建应用程序**。
- UIViewController是**所有视图控制器的父类**
- iOS提供了许多内置的视图控制器以支持标准的用户界面部分，比如导航控制器(UINavigationController)、标签控制器(UITabBarController)、表视图控制器(UITableViewController)

<a name = "2"></a>

# 视图控制器的功能

- 控制视图大小变换、布局视图、响应事件
- 检测以及处理内存警告
- 检测以及处理屏幕旋转
- 检测视图的切换
- 实现模块独立、提高复用性

<a name = "3"></a>

# 根视图控制器

在iOS应用程序中，需要为一个应用程序制定它的根视图控制器，通过这个根视图控制器来管理其它的视图控制器：
{% highlight objc %}
RootViewController *rootVC = [[RootViewController alloc] init];
self.window.rootViewController = rootVC;
{% endhighlight %}