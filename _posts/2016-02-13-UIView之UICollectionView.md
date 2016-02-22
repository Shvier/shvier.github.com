---
layout: post
title: "UIView之UICollectionView"
date: 2016-02-13 16:24:47
categories: Objective-C
---

# 目录
- [UICollectionView概述](#1)
  - [UICollectionViewFlowLayout](#1.1)
  - [初始化](#1.2)
  - [设置代理](#1.3)
    - [必须实现的代理方法](#1.3.1)
    - [选择实现的代理方法](#1.3.2)
    - [布局协议](#1.3.3)
  - [自定义瀑布流](#1.4)

<a name = "1"></a>

# UICollectionView概述

- UICollection的实现跟tableView不同的地方在于item的布局，需要用`UICollectionViewLayout`类来描述视图的布局

