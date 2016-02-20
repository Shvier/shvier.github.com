---
layout: post
title: "UIView之UIImageView"
date: 2016-02-11 11:24:43
categories: Objective-C
---

# 目录

- [初始化](#1)
- [动态图](#2)

# UIImageView概述

- `UIImageView`是iOS中用来显示图片的类，iOS中几乎所有看到的图片，都是由这个类来显示的
- `UIImageView`相当于一个相框，专门用作显示图片，可以存放一个图片或一组图片

<a name = "1"></a>

### 初始化

{% highlight objc %}
// 图片文件路径
NSString *path = [[NSBundle mainBundle] pathForResource:@"head" ofType:@"jpg"];
// 创建一个UIImage对象，使用initWithContentOfFile:方法
UIImage *image = [UIImage imageWithContensOfFile:path];
// 创建一个UIImageView对象，使用initWithImage:方法
UIImageView *imageView = [[UIImageView alloc] initWithImage:image];
imageView.frame = CGRectMake(90, 100, 100, 100);
[view addSubview:imageView];
[imageView release];
{% endhighlight %}

<a name = "2"></a>

### 动态图(动画)

#### 相关方法

1. `animationImages`   
设置一组动态图片
2. `animationDuration`   
设置播放一组动态图片的时间
3. `animationRepeatCount`   
设置重复次数
4. `startAnimating`   
开始动画
5. `stopAnimating`   
结束动画

#### 示例

{% highlight objc %}
UIImageView *imageView = [[UIImageView alloc] initWithFrame:CGRectMake(90, 100, 100, 100)];
[view addSubview:imageView];
// 将帧动画图片按1.jpg，2.jpg的形式编号放在工程目录下
// 初始化一个数组用来存放帧图片
// 假设图片数量为n
NSMutableArray *images = [[NSMutableArray alloc] initWithCapacity: 0];
for (int i = 0; i < n; i ++) {
	UIImage *image = [UIImage imageNamed:[NSString stringWithFormat:@"%d.jpg", i]];
	[images addObject: image];
	[image release];
}
imageView.animationImages = images;
imageView.animationDuration = 0.5;
imageView.animationRepeatCount = 1;//设置为0表示无限循环
[imageView startAnimating];
{% endhighlight %}