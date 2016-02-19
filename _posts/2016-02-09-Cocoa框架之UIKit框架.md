---
layout: post
title: "Cocoa框架之UIKit框架"
date: 2016-02-09 10:01:23
categories: Objective-C
---

在iOS开发当中，`Foundation`框架主要用来处理数据，而`UIKit`用来搭建界面和逻辑交互，两个框架相互结合，就变成了我们能看到的APP。`UIKit`常用的基本框架如下：

- [UIAlertAction]()
- [UIBarItem]()
  - [UIBarButtonItem]() 
  - [UITabBarItem]()
- [UICollectionViewLayout]()
  - [UICollectionViewFlowLayout]()
  - [UICollectionViewTransition]()  
- [UIColor]()
- [UIFont]()
- [UIGestureRecognizer]()
  - [UILongPressGestureRecognizer]()
  - [UIPanGestureRecognizer]()
  - [UIPinchGestureRecognizer]()
  - [UIRotationGestureRecognizer]()
  - [UISwipeGestureRecognizer]()
  - [UITapGestureRecognizer]()
- [UIImage]()
- [UINavigationItem]()
- [UIResponder]()
  - [UIView]()
    - [UIActionSheet]()
    - [UIAlertView]()
    - [UIApplication]()
    - [UICollectionReusableView]()
      - [UICollectionViewCell]()
    - [UIControl]()
      - [UIButton]()
      - [UIDatePicker]()
      - [UIPageControl]()
      - [UISegmentedControl]()
      - [UISlider]()
      - [UISwitch]()
      - [UITextField]()
    - [UIImageView]()
    - [UILabel]()
    - [UINavigationBar]()
    - [UIPickerView]()
    - [UIScrollView]()
      - [UICollectionView]()
      - [UITableView]()
    - [UITabBar]()
    - [UITableViewCell]()
    - [UIWindow]()  
  - [UIViewController]()
    - [UIAlertController]()
    - [UICollectionViewController]()
    - [UINavigationController]()
    - [UITabBarController]()
    - [UITableViewController]()
    
在`UIKit`框架中，有两个非常重要的子类：`UIResponder(响应者类)`和`UIGestureRecognizer(手势识别类)`。   
顾名思义，`UIResponder`用来构建各个可以交互的界面并对此进行逻辑处理，而`UIGestureRecognizer`则根据不同的输入，或者说交互，来进行对应的操作。 

对UI程序来说，一个程序分为三块：`应用程序对象`、`应用程序代理`、`事件循环`。

# 应用程序代理

1.应用程序代理作用,根据应用程序传递过来的状态做出相应的处理。   2.应用程序的状态有很多,比如:程序启动、进入活跃状态、进到后台、内存警告、收到远程消息等等。   3.任何接受了UIApplicationDelegate协议的对象都可以成为应用程 序代理。   4.一旦应用程序的某种状态触发,就会执行相应的代理方法。

- **`application: didFinishLaunchingWithOptions:`**   
告诉delegate程序启动即将完成，程序准备要运行。(delegate实现这个方法时，要创建window对象，将程序内容通过window呈现给用户)
- **`applicationDidBecomeActive:`**   
告诉delegate应用程序已经进入活跃状态(重新执行被暂停的任务)
- **`applicationWillResignActive:`**   
告诉delegate应用程序即将进入非活跃状态(暂停游戏、停止timer等)
- **`applicationDidEnterBackground:`**   
告诉delegate已经进入到了后台(存储用户数据、释放一些共享资源、停止timer等)
- **`applicationWillEnterForeground:`**   
告诉delegate应用程序即将进入前台(恢复所有进入后台时暂停的任务)
- **`applicationWillTerminate:`**   
告诉delegate应用程序即将退出(从内存中清除)，iOS4之后由**`applicationDidEnterBackground:`**替代