---
layout: post
title: "UIView之UIScrollView"
date: 2016-02-11 11:24:43
categories: Objective-C
---

# 目录

- [UIScrollView概述](#1)
  - [初始化](#1.1)
  - [重要属性](#1.2)
  - [协议方法](#1.3)
  - [与UIPageControl结合使用](#1.4)

<a name = "1"></a>

# UIScrollView概述

- `UIScrollView`是UIView的子类，
- `UIScrollView`是`所有滚动视图的基类`(UITableView，UICollectionView)
- `UIScrollView`主要使用在滚动头条、相册等常见的功能里

<a name = "1.1"></a>

### 初始化

1. 创建一个任意尺寸的UIScrollView
2. 设置背景颜色
3. 把UIScrollView放到self.view上显示
4. 内存管理

{% highlight objc %}
// 获取屏幕的宽高
#define kWidth self.view.frame.size.width
#define kHeight self.view.frame.size.height

// 创建一个和屏幕等尺寸的UIScrollView
UIScrollView *scrollView = [[UIScrollView alloc] initWithFrame:CGRectMake(0, 0, kWidth, kHeight)];
scrollView.backgroundColor = [UIColor orangeColor];
[self.view addSubview:scrollView];
[scrollView release];
{% endhighlight %}

`UIScrollView`虽然创建出来了，但是不具备滚动功能，所以还要对其他一些重要的属性进行设置。

<a name = "1.2"></a>

### 重要属性

#### contentSize(滚动范围)

- **控制滚动范围**   
`contentSize`有两个参数：`width`和`height`，分别表示横向滚动范围和纵向滚动范围。通过该属性，可以设置滚动视图的滚动范围。比如让其可以横向滚动四个屏幕宽度

{% highlight objc %}
scrollView.contentSize = CGSizeMake(kWidth * 4, 0);
{% endhighlight %}

又或是让其纵向滚动五个屏幕高度

{% highlight objc %}
scrollView.contentSize = CGSizeMake(0, kHeight * 5);
{% endhighlight %}

设置好滚动范围后，发现界面依然是空白的，所以添加几张图片吧:)

#### 添加滚动图片

将图片命名为1.jpg，2.jpg...的形式放在工程目录下(图片张数为n)

{% highlight objc %}
for (NSInteger i = 1; i < n; i ++) {
	UIImageView *imageView = [[UIImageView alloc] initWithFrame:CGRectMake((i - 1) * kWidth), 0, kWidth, kHeight];
	imageView.image = [UIImage imageNamed:[NSString stringWithFormat:@"%ld.jpg", i]];
	[scrollView addSubview:imageView];
	[imageView release];
}
{% endhighlight%}

如果想默认显示第二张图片(或者第n张图片)，如何去做？

#### contentOffset偏移量

- 通过`contentOffset`可以显示指定偏移量的视图

{% highlight objc %}
// 设置默认显示第二张图片
scrollView.contentOffset = CGPointMake(kWidth, 0);
{% endhighlight %}

移动过程中我们发现，当移动停止时，图片也停在了一半，这样非常难看，那么如何使其一页一页的滚动呢？

#### pagingEnabled整页滚动

{% highlight objc %}
scrollView.pagingEnabled = YES:
{% endhighlight %}

#### 不显示滚动条

{% highlight objc %}
// 不显示水平滚动条
scrollView.showHorizontalScrollIndicator = NO;
// 不显示垂直滚动条
scrollView.showVerticalScrollIndicator = NO;
{% endhighlight %}

#### bounces回弹效果

{% highlight objc %}
// 关闭回弹效果
scrollView.bounces = NO;
{% endhighlight %}

<a name = "1.3"></a>

### 协议方法

`UIScrollView`的协议方法分为两类：

- 监控滚动状态
- 控制视图缩放

#### 监控滚动状态

- **`- (void)scrollViewWillBeginDragging:(UIScrollView *)scrollView`**   
开始拖拽时触发此方法
- **`- (void)scrollViewDidScroll:(UIScrollView *)scrollView`**   
拖拽过程中触发此方法   
- **`- (void)scrollViewDidEndDragging:(UIScrollView *)scrollView`**   
结束拖拽时触发此方法
- **`- (void)scrollViewBeginDecelerating:(UIScrollView *)scrollView`**   
滚动减速时触发此方法
- **`- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView`**   
滚动彻底停止时触发此方法

#### 控制视图缩放

在查看控制视图缩放的协议之前，先看看有关UIScrollView缩放的属性：

- `maximumZoomScale`   
设置最大的缩放比例
- `minimumZoomScale`   
设置最小的缩放比例
- `zoomScale`   
设置当前的比例

在视图控制器的`viewDidLoad`方法里设置好最大和最小的缩放比例后，即可对scrollView进行缩放操作

- **`(UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView`**

{% highlight objc %}
// 设置最大缩放比例
scrollView.maximumZoomScale = 2;
// 设置最小缩放比例
scrollView.minimumZoomScale = 0.5;

// 指定需要缩放的视图
- (UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView {
	return [scrollView.subViews firstObject];
}
{% endhighlight %}

但是进行缩放后发现，其他视图好像也发生了变化，这是因为协议方法不仅会改变视图的尺寸，同样会改变scrollView的contentSize属性，所以在构建滚动视图时，应该把一张大的scrollView作为画册，另外给小的scrollView设置一个imageView，作为缩放的图片。   
即大的scrollView负责滚动，小的scrollView负责缩放。

{% highlight objc %}
// 小的scrollView上三个子视图，一个是添加到上面的imageView，另外两个是水平和垂直两个滚动条，所以需要找到imageView，进行缩放操作
- (UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView {
	UIImageView *imageView = [scrollView.subviews objectAtIndex:0];
	return imageView;
}

// 在滚动到下一张视图时，恢复到原有尺寸
- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView {
	// 从大的scrollView找到小的scrollView
	for (UIScrollView *smallScrollView in scrollView.subviews) {
		if ([smallScrollView isKindOfClass:[UIScrollView class]]) {
			// 把视图尺寸恢复到原有尺寸
			smallScrollView.zoomScale = 1.0;
		}
	}
}
{% endhighlight %}

<a name = "1.4"></a>

### 与UIPageControl结合使用

- 可以通过UIPageControl的点击事件和UIScrollView的偏移量实现二者的关联使用

{% highlight objc %}
[page addTarget:self action:@selector(pageControlAction:) forControlEvents:UIControlEventValueChanged];
// 在pageControl的点击⽅方法⾥里对ScrollView进⾏行关联,通过操作scrollView的偏移量ContentOffset进⾏行视图上得切换- (void)pageControlAction:(UIPageControl *)page {	// 通过tag值先找到要操作的scrollView对象	UIScrollView *scrollView = (UIScrollView *)[self.view viewWithTag:101];	// 根据pageControl当前的页数计算scrollView当前的偏移量	[scrollView setContentOffset:CGPointMake(page.currentPage * kWidth, 0) animated:YES];}

// 实现scrollView对pageControl的操作
//先找到scrollView的滚动结束触发的协议⽅方法- (void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView {	// 先通过tag值找到要操作的视图	UIPageControl *page = (UIPageControl *)[self.view viewWithTag:102];	// 根据当前scrollView的偏移量计算pageControl应显示的页数	page.currentPage = scrollView.contentOffset.x / kWidth;}{% endhighlight%}