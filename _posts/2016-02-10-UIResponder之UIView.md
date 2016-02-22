---
layout: post
title: "UIResponder之UIView"
date: 2016-02-10 09:31:21
categories: Objective-C
---

# 目录

- [UIView概述](#1)
  - [重要属性](#1.1)
    - [alpha](#1.1.1)
    - [center](#1.1.2)
    - [frame](#1.1.3)
    - [hidden](#1.1.4)
    - [subViews](#1.1.5)
    - [superView](#1.1.6)
    - [tag](#1.1.7)
  - [重要方法](#1.2)
    - [addSubView:](#1.2.1)
    - [insertSubview: aboveSubview:](#1.2.2)
    - [insertSubview: atIndex:](#1.2.3)
    - [insertSubview: belowSubview:](#1.2.4)
- [UIWindow](#2)
- [UIControl](#3)
  - [常用方法](#3.1)
  - [事件响应](#3.2)
  - [事件处理](#3.3)
    - [基于触摸](#3.3.1)
    - [基于值](#3.3.2)
    - [基于编辑](#3.3.3)
    - [其他](#3.3.4)

<a name = "1"></a>

# UIView概述

UIView表示屏幕上的一块矩形区域。在iOS中几乎所有可视化控件都是UIView的子类。它负责渲染区域的内容，并且响应该区域内发生的触摸事件。

<a name = "1.1"></a>

### 重要属性

<a name = "1.1.1"></a>

#### alpha

不透明度

<a name = "1.1.2"></a>

#### center

视图的中心点，可以更改视图位置。

<a name = "1.1.3"></a>

#### frame

- `frame`是UIView的重要属性
- 决定了视图的大小和位置
- CGRect类型
- 基于父视图的坐标系而言

<a name = "1.1.4"></a>

#### hidden

控制视图显示或隐藏

<a name = "1.1.5"></a>

#### subViews

获取本视图所有子视图

<a name = "1.1.6"></a>

#### superView

获取本视图的父视图

<a name = "1.1.7"></a>

#### tag

给视图标记，用于找到该视图

<a name = "1.2"></a>

### 重要方法

<a name = "1.2.1"></a>

#### addSubView:

添加子视图

<a name = "1.2.2"></a>

#### insertSubview: aboveSubview:

在指定的视图上添加子视图

<a name = "1.2.3"></a>

#### insertSubview: atIndex:

在指定的index处插入子视图

<a name = "1.2.4"></a>

#### insertSubview: belowSubview:

在指定的视图下面添加子视图

<a name = "2"></a>

# UIWindow

- 管理和协调应用程序的显示
- UIWindow类是UIView的子类，可以看作是特殊的UIView
- 一般应用程序只有一个UIWindow对象

<a name = "3"></a>

# UIControl

- UIControl是有控制功能的视图(比如UIButton、UISlider、UISegmentedControl)的父类
- 只要跟控制有关的控件都是继承于该类
- UIControl通常不直接使用，而是使用其子类

<a name = "3.1"></a>

### 常用方法

- **`addTarget: action: forControlEvents:`**   
添加一个事件
- **`removeTarget: action: forControlEvents:`**   
移除一个事件

<a name = "3.2"></a>

### 事件响应

事件响应的三种形式：基于触摸、基于值、基于编辑

<a name = "3.3"></a>

### 事件处理

<a name = "3.3.1"></a>

#### 基于触摸
- 当触摸从控件内部拖到外部时触发   
`UIControlEventTouchDragExit`
- 当控件之内触摸抬起时触发   
`UIControlEventTouchUpInside`
- 控件之外触摸抬起时触发   
`UIControlEventTouchUpOutside`
- 触摸取消事件，设备被上锁或者电话呼叫打断   
`UIControlEventTouchCancel`

<a name = "3.3.2"></a>

#### 基于值

- 用户按下时触发   
`UIControlEventTouchDown` 
- 点击计数大于1时触发   
`UIControlEventTouchDownRepeat` 
- 当触摸在控件内拖动时触发   
`UIControlEventTouchDragInside`
- 当触摸在控件之外拖动时触发   
`UIControlEventTouchDragOutside` 
- 当触摸从控件之外拖动到内部时触发    
`UIControlEventTouchDragEnter`

<a name = "3.3.3"></a>

#### 基于编辑

- 当控件的值发生变化时。用于滑块、分段控件等控件   
`UIControlEventValueChanged` 
- 本控件中开始编辑时   
`UIControlEventEditingDidBegin`
- 本控件中的文本被改变  
`UIControlEventEditingChanged` 
- 本控件中编辑结束时  
`UIControlEventEditingDidEnd`
- 本控件内通过按下回车键结束编辑时   
`UIControlEventEditingDidOnExit`

<a name = "3.3.4"></a>

#### 其他

- 所有触摸事件   
`UIControlEventAllTouchEvents`
- 本编辑的所有事件   
`UIControlEventAllEditingEvents`
- 所有事件  
`UIControlEventAllEvents`