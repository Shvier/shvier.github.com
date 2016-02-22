---
layout: post
title: "UIViewController之UINavigationController"
date: 2016-02-12 13:27:42
categories: Objective-C
---

# 目录

- [UINavigationController概述](#1)
  - [初始化](#1.1)
  - [UINavigationBar](#1.2)
  - [UINavigationItem](#1.3)
  - [UIBarButtonItem](#1.4)
  - [页面跳转](#1.5)

<a name = "1"></a>

# UINavigationController概述
- `UINavigationController`导航控制器，主要用来管理其他有层级递进关系的视图控制器
- 继承于UIViewController，**以栈的方式管理所控制的其它视图控制器**，被称为导航控制器的根视图控制器
- 任何继承自UIViewController的类都可以作为根视图控制器

<a name = "1.1"></a.

### 初始化

1. 创建视图控制器
2. 创建导航控制器，并把刚创建的视图控制器作为根视图控制器
3. 设置导航控制器为window的根视图控制器
4. 内存管理

{% highlight objc %}
RootViewController *rootVC = [[RootViewController alloc] init];
UINavigationController *naVC = [[UINavigationController alloc] initWithRootViewController:rootVC];
self.window.rootViewController = naVC:
[rootVC release];
[naVC release];
{% endhighlight %}

<a name = "1.2"></a>

### UINavigationBar

`UINavigationBar`(导航栏)上的设置范围两部分，一是导航栏上的各种导航部件(UINavigationItem)，二为导航栏自身的相关设置

- `UINavigationBar`在iOS 7之后默认是半透明的，iOS 7之前默认是不透明的
- 竖屏下默认高度是44，横屏下默认高度是32
- iOS 7之后，UINavigationBar的背景会延伸到statusBar(状态栏)上，使得导航栏的高度仍然为44，但是显示效果为64
- 每个视图控制器都有一个`navigationItem`属性。`navigationItem`中设置的左按钮、右按钮、标题等，会随着控制器的显示，也显示到navigationBar上

<a name = "1.3"></a>

### UINavigationItem

{% highlight objc %}
// 导航栏标题
self.title = @"Title";
// self.title会同时改变导航栏的标题和tabBar的标题，可以用如下方法单独操作导航栏的标题
self.navigationItem.title = @"Title";
// 左按钮
self.navigationItem.leftBarButtonItem = [[[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemItemSearch target:self action:@selector(leftAction:)] autorelease];
// 右按钮
self.navigationItem.rightBarButtonItem = [[[UIBarButtonItem alloc] initWithImage:[UIImage imageNamed:@"next"] style:UIBarButtonItemStylePlain target:self action:@selector(rightAction:)] autorelease];
{% endhighlight %}

<a name = "1.4"></a>

### UIBarButtonItem

- `initWithImage: Style: target: action:`   
参数1：图片 参数2：barButtonStyle(按钮样式，枚举类型) 参数3：目标 参数4：方法
- `initWithTitle: Style: target: action:`   
参数1：按钮文字 参数2：barButtonStyle(按钮样式，枚举类型) 参数3：目标 参数4：方法
- `initWithBarButtonSystemItem: target: action:`   
参数1：系统按钮样式(枚举类型) 参数2：目标 参数3：方法
- `initWithCustomView:`   
参数：自定义UIView对象

其中，左右item可以指定为数组，同时显示多个按钮

#### 多个按钮

{% highlight objc %}
UIBarButtonItem *item1 = [[[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemAdd target:self action:@selector(firstItemAction:)] autorelease];
UIBarButtonItem *item2 = [[[UIBarButtonItem alloc] initWithBarButtonSystemItem:UIBarButtonSystemCamera target:self action:@selector(secondItemAction:)] autorelease];

self.navigationItem.leftButtonItem = @[item1, item2];
{% endhighlight%}

#### 标题视图

{% highlight objc %}
UISegmentedControl *seg = [[UISegmentedControl alloc] initWithItems:@[@"已接电话", @"未接电话"]];
seg.frame = CGRectMake(0, 0, 100, 30);
self.navigationItem.titleView = seg;
[seg release];
{% endhighlight %}

#### 重要属性

- `navigationBarHidden`   
导航栏的显隐属性
- `navigationBar.barStyle`   
导航栏样式
- `navigationBar.backgroundColor`   
导航栏背景色
- `navigationBar.barTintColor`   
导航栏颜色
- `navigationBar.tintColor`   
导航栏元素颜色
- `navigationBar.translucent`   
导航栏半透明效果(iOS 7以后，默认为YES，当半透明效果开启时，屏幕左上角为坐标原点，关闭时，导航栏左下角为坐标原点)

<a name = "1.5"></a>

### 页面跳转

- `UINavigationController`以**栈**的形式管理控制器的切换，控制入栈和出栈来展示各个视图控制器
- `UINavigationController`的contentView始终显示的是栈顶的视图控制器
- `viewControllers`属性是一个可变数组，存储了栈中所有被管理的视图控制器，入栈的时候，使用`addObject`把新的视图控制器对象添加到数组末尾，出栈时使用`removeLastObject`移除数组末尾的视图控制器对象
- `navigationController属性`，父类中的属性，每个在栈顶的视图控制器，都能通过此属性获取自己所在的UINavigationController对象

#### 推出方式

###### 工作原理

- 先进后出，后进先出
- 栈顶为当前显示的视图控制器

###### 入栈和出栈

- `pushViewController: animated:`   
进入下一视图控制器
- `popViewControllerAnimated:`   
返回上一视图控制器
- `popToViewController: animated:`   
返回到指定的视图控制器
- `popToRootViewController: animated:`   
返回到根视图控制器

#### 模态方式

1. 创建第二页对象
2. 设置过渡动画
3. 模态跳转
4. 内存管理

{% highlight objc %}
- (void)next {
	SecondViewController *secondVC = [[SecondViewController alloc] init];
	secondVC.modalTransitionStyle = UIModalTransitionStyleCoverVertical;
	[self presentViewController:secondVC animated:YES completion:nil];
	[secondVC release];
}
- (void)back {
	[self dismissViewControllerAnimated:YES completion:nil];
}
{% endhighlight %}

#### 对比

- 页面的切换方式主要分为：推出(push)和模态(modal)
- 推出(push)用于一系列有层级递进关系的视图跳转
- 模态(modal)用于单独功能页面的跳转和主要逻辑功能没有联系的跳转