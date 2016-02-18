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

### 响应者链

iOS中所有能响应事件(触摸、晃动、远程事件)的对象都是响应者。   
当硬件检测到触摸操作时，会将信息交给`UIApplication`，按`UIApplaction`->`window`->`viewController`->`view`->`所有子视图`的顺序开始检测，而处理触摸事件时则从相反的方向去处理，一旦有某一个环节无法响应，那么丢弃该次触摸操作。涉及到以下方法：

- **`touchesBegan: withEvent:`**
- **`touchesMoved: withEvent:`**
- **`touchesEnded: withEvent:`**
- **`touchesCancelled: withEvent:`**
   
因此，可以使用`userInteractionEnabled`来控制某个控件是否可以响应事件。