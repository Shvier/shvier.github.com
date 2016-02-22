---
layout: post
title: "UIViewController之UITabBarController"
date: 2016-02-12 18:48:24
categories: Objective-C
---

# 目录

- [UITabBarController概述](#1)
  - [初始化](#1.1)
  - [重要属性](#1.2)
  - [协议方法](#1.3)
  - [UITabBar](#1.4)
  - [UITabBarItem](#1.5)
  - [UIAppearance](#1.6)

<a name = "1"></a>

# UITabBarController概述

- 与UINavigationController不同，UITabBarController用来管理没有层级递进关系的视图

<a name = "1.1"></a>

### 初始化

1. 创建UITabBarController对象
2. 将UITabBarController管理的视图控制器放到一个数组中
3. 设置UITabBarController的子视图控制器数组
4. 将根视图控制器设置成UITabBarController

{% highlight objc %}
UITabBarController *tabBarController = [[UITabBarController alloc] init];
NSArray *viewControllers = @[firstNav, secondNav, thirdNav, fourthNav];
tabBarController.viewControllers = viewControllers;
self.window.rootViewController = tabBarController;
{% endhighlight %}

<a name = "1.2"></a>

### 重要属性

- `viewControllers`   
管理的视图控制器(NSArray)
- `taBar`   
标签栏
- `selectedIndex`   
选中的某个tabBarItem
- `delegate`   
代理

<a name = "1.3"></a.

### 协议方法

{% highlight objc %}
// 点击某个标签时(tabBarItem)时触发该方法
- (void)tabBarController:(UITabBarController *)tabBarController didSelectViewController:(UIViewController *)viewController {
	NSLog(@"%ld", tabBarController.selectedIndex);
}
{% endhighlight %}

<a name = "1.4"></a>

### UITabBar

- UITabBar包含多个UITabBarItem，每一个UITabBarItem对应一个UIViewController，高度是49
- 系统最多只显示5个UITabBarItem，当UITabBarItem超过5个时系统会自动增加一个`更多`按钮，点击更多按钮时没有在底部出现的按钮会以列表的形式显示出来

#### 重要属性

- `tintColor`   
设置tabBar选中的颜色
- `barTintColor`   
设置tabBar的背景颜色
- translucent   
是否半透明，默认是YES

<a name = "1.5"></a>

### UITabBarItem

- `UITabBarItem`可以通过属性`title`、`badgeValue`设置标题及提示
- 可以使用系统样式创建UITabBarItem

#### 图片处理

{% highlight objc %}
// 未选中时的图片
UIImage *normalImage = [UIImage imageNamed:@"normal.png"];
// 图片不被渲染，保持图片本身的样子
normalImage = [normalImage imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
// 选中时的图片
UIImage *selectedImage = [UIImage imageNamed:@"selected.png"];
selectedImage = [selectedImage imagewithRenderingNode:UIImageRenderingModeAlwaysOriginal];
{% endhighlight %}

<a name = "1.6"></a>

### UIAppearance