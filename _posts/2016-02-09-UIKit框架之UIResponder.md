---
layout: post
title: "UIKit框架之UIResponder"
date: 2016-02-09 13:24:41
categories: Objective-C
---

# 目录

- [MVC](#1)
- [UIView](#2)
  - [alpha](#2.1)
  - [backgroundColor](#2.2)
  - [bounds](#2.3)
  - [center](#2.4)
  - [frame](#2.5)
  - [transform](#2.6)
- [UIViewController](#3)
  - [管理视图](#3.1)
    - [isViewLoaded](#3.1.1)
    - [loadView](#3.1.2)
    - [viewDidLoad](#3.1.3)
  - [视图间跳转](#3.2)
    - [presentViewController: animated: completion:](#3.2.1)
    - [dismissViewControllerAnimated: completion:](#3.2.2)
  - [响应视图事件](#3.3)
    - [viewWillAppear:](#3.3.1)
    - [viewDidAppear:](#3.3.2)
    - [viewWillDisappear:](#3.3.3)
    - [viewDidDisappaer:](#3.3.4)

<a name = "1"></a>

# MVC
`UIResponder`下有两个非常重要的子类：`UIView`和`UIViewController`。iOS开发中的MVC也由此而来，`UIView`主要负责界面的展现，`UIViewController`用来负责界面的跳转和数据呈现。

<a name = "2"></a>

# UIView

在iOS中，所有展示的控件都继承自`UIView`。   
`UIView`具有以下重要属性：
  
- **alpha** 
- **backgroundColor**
- **bounds**
- **center**
- **frame**
- **transform**

<a name = "2.1"></a>

### alpha

UIView的不透明度，设置该属性可以改变UIView显示的透明程度。

<a name = "2.2"></a>

### backgroundColor

UIView的背景颜色，设置该属性可以改变UIView的背景颜色。

<a name = "2.3"></a>

### bounds

UIView相对自身坐标系的位置以及大小。

<a name = "2.4"></a>

### center

UIView中心点的位置，设置该点`(CGPoint类型)`可以设置UIView的中心位置。

<a name = "2.5"></a>

### frame

UIView相对父视图坐标系的位置以及大小。

<a name = "2.6"></a>

### transform

`CGAffineTransform`类型，通过该属性可以对UIView进行矩阵变换，包括旋转、平移、缩放等。

<a name = "3"></a>

# UIViewController

在iOS中，`UIViewController`主要用来处理界面间的跳转和数据的处理。   
`UIViewController`具有以下重要方法：

- **管理视图**
  - **`isViewLoaded`**
  - **`loadView`**
  - **`viewDidLoad`**
- **视图间跳转**
  - **`presentViewController: animated: completion:`**
  - **`dismissViewControllerAnimated: completion:`** 
- **响应视图事件**
  - **`viewWillAppear:`**
  - **`viewDidAppear:`**
  - **`viewWillDisappear:`**
  - **`viewDidDisappear:`**
- **编辑状态**
  - **`setEditing: animated:`**
  - **`editButtonItem`**
  
<a name = "3.1"></a>  
  
### 管理视图

<a name = "3.1.1"></a>

#### isViewLoaded

当视图已经载入到内存时执行此方法，会返回一个`BOOL`类型的变量来告诉系统视图已经载入到内存中。

<a name = "3.1.2"></a>

#### loadView

当视图加载其内容但是这些内容还没有被载入到内存中时执行此方法，但是官方文档表示不要直接执行此方法，如果需要加载视图内容、设置属性，在**`viewDidLoad`**方法中实现。

<a name = "3.1.3"></a>

#### viewDidLoad

当视图上内容已经加载到内存中时执行此方法。

<a name = "3.2"></a>

### 视图间跳转

<a name = "3.2.1"></a>

#### presentViewController: animated: completion:

用执行该方法的视图控制器跳转到作为参数的视图控制器，`animated:`表示是否使用跳转动画，`completion:`表示跳转完成后执行的操作。

<a name = "3.2.2"></a>

#### dismissViewControllerAnimated: completion:

将目前的视图控制器返回到跳转来的视图控制器，换句话说，该方法是**`presentViewController: animated: completion:`**的逆操作，`completion:`依然表示完成后进行的操作。

<a name = "3.3"></a>

### 响应视图事件

<a name = "3.3.1"></a>

#### viewWillAppear:

前面说过，**`viewDidLoad`**用来加载视图内容，而如果当我们从一个视图返回到一个视图时是不会执行**`viewDidLoad`**方法的，所以需要用该方法来执行加载操作。该方法表示视图将要推出时执行的操作。

<a name = "3.3.2"></a>

#### viewDidAppear:

该方法紧接上一步操作，表示视图完全推出后的需要执行的操作。

<a name = "3.3.3"></a>

#### viewWillDisappear:

该方法是上述过程的逆操作，表示视图将要跳转到另一个视图时执行的操作。