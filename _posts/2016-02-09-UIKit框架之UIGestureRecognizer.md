---
layout: post
title: "UIKit框架之UIGestureRecognizer"
date: 2016-02-09 19:42:32
categories: Objective-C
---

# 目录

- [响应者链](#1)
- [使用手势](#2)
- [UILongPressGestureRecognizer](#3)
- [UIPanGestureRecognizer](#4)
- [UIPinchGestureRecognizer](#5)
- [UIRotationGestureRecognizer](#6)
- [UIScreenEdgePanGestureRecognizer](#7)
- [UISwipeGestureRecognizer](#8)
- [UITapGestureRecognizer](#9)

<a name = "1"></a>

### 响应者链

iOS中所有能响应事件(触摸、晃动、远程事件)的对象都是响应者。   
当硬件检测到触摸操作时，会将信息交给`UIApplication`，按`UIApplaction`->`window`->`viewController`->`view`->`所有子视图`的顺序开始检测，而处理触摸事件时则从相反的方向去处理，一旦有某一个环节无法响应，那么丢弃该次触摸操作。涉及到以下方法：

- **`touchesBegan: withEvent:`**
- **`touchesMoved: withEvent:`**
- **`touchesEnded: withEvent:`**
- **`touchesCancelled: withEvent:`**
   
因此，可以使用`userInteractionEnabled`来控制某个控件是否可以响应事件。

<a name = "2"></a>

### 使用手势

 1. 创建满足需求的手势，在创建时关联手势触发时的方法
 2. 配置手势的相关属性
 3. 将手势添加到需要执行操作的视图上面
 4. 实现手势方法，当触摸发生，手势识别器识别到相对应的触摸时，就会执行关联方法

<a name = "3"></a>

### UILongPressGestureRecognizer

{% highlight objc %}
UILongPressGestureRecognizer *longPress = [[UILongPressGestureRecognizer alloc] initWithTarget:self action:@selector(longPressAction:)];
//长按手势触发的最短时间，默认是0.5
longPress.minimunPressDuration = 3.0;
//添加长按手势
[<UIView *view> addGestureRecognizer:longPress];
{% endhighlight %}

<a name = "4"></a>

### UIPanGestureRecognizer

{% highlight objc %}
//设置平移手势
UIPanGestureRecognizer *pan = [[UIPanGestureRecognizer alloc] initWithTarget:self action:@selector(panAction:)];
//添加平移手势
[<UIView *view> addGestureRecognizer:pan];

//设置平移手势事件
- (void)panAction:(UIPanGestureRecognizer *)pan {
	//获取要进行平移的视图
	<UIView *view>
	//获取平移点的位置
	CGPoint point = [pan translationInView:view];
	//改变视图的位置属性
	view.transform = CGAffineTransformTranslate(view.transform, point.x, point.y);
	//平移后同样需要重置
	[pan setTranslation:CGPointZero inView:view];
}
{% endhighlight %}

<a name = "5"></a>

### UIPinchGestureRecognizer

{% highlight objc %}
//设置缩放手势
UIPinchGestureRecognizer *pinch = [[UIPinchGestureRecognizer alloc] initWithTarget:self action:@selector(pinchAction:)];
//添加缩放手势
[<UIView *view> addGestureRecognizer:pinch];

//设置缩放手势事件
- (void)pinchAction:(UIPinchGestureRecognizer *)pinch {
	//获取要进行缩放的视图
	<UIView *view>
	//改变view的形变属性
	//第一个参数是需要发生形变的transform，第二个参数是x方向的缩放规模，第三个参数是y方向的缩放规模
	view.transform = CGAffineTransformScale(view.transform, pinch.scale, pinch.scale);
	//在完成一次缩放后必须把缩放值重置，不然缩放的大小会累加
	pinch.scale = 1;
	//或者也可以使用这种方法
	view.transform = CGAffineTransfromMakeScale(pinch.scale, pinch.scale);
}
{% endhighlight %}

<a name = "6"></a>

### UIRotationGestureRecognizer

{% highlight objc %}
//设置选择手势
UIRotationGestureRecognizer *rotate = [[UIGestureRecognizer alloc] initWithTarget:self action:@selector(rotateAction:)];
//添加缩放手势
[<UIView *view> addGestureRecognizer:rotate];

//设置旋转手势事件
- (void)rotateAction:(UIRotationGestureRecognizer *)rotate {
	//获取需要进行旋转的视图
	<UIView *view>
	//改变view的旋转属性
	view.transform = CGAffineTransformRotate(view.transform, rotate.rotation);
	//与缩放手势一样，完成一次旋转后同样需要把旋转规模重置
	rotate.rotation = 0;
	//这种旋转方式会回弹
	view.transform = CGAffineTransformMakeRotation(rotate.rotation);
}
{% endhighlight %}

<a name = "7"></a>

### UIScreenEdgePanGestureRecognizer

{% highlight objc %}
//设置屏幕边缘手势
UIScreenEdgePanGestureRecognizer *edge =
    [[UIScreenEdgePanGestureRecognizer alloc] initWithTarget:self action:@selector(edgeAction:)];
//设置向左滑动显示
edge.edges = UIRectEdgeLeft;
//添加屏幕边缘手势
[self.view addGestureRecognizer:edge];

//设置屏幕边缘事件
- (void)handleLeftEdgeGesture:(UIScreenEdgePanGestureRecognizer *)gesture {
    // 获取到当前被触摸的view
    UIView *view = [self.view hitTest:[gesture locationInView:gesture.view] withEvent:nil];
    if(UIGestureRecognizerStateBegan == gesture.state || UIGestureRecognizerStateChanged == gesture.state) {
        //根据被触摸手势的view计算得出坐标值
        CGPoint translation = [gesture translationInView:gesture.view];
        //进行设置
        view.center = CGPointMake(_centerX + translation.x, _centerY);
    } else {
        //恢复设置
        [UIView animateWithDuration:.3 animations:^{view.center = CGPointMake(_centerX, _centerY);}];
    }
}
{% endhighlight %}

<a name = "8"></a>

### UISwipeGestureRecognizer

{% highlight objc %}
UISwipeGestureRecognizer *swipe = [[UISwipeGestureRecognizer alloc] initWith:self action:@selector(swipeAction:)];
//手势方向
swipe.direction = UISwipeGestureRecognizerDirectionDown;
//添加轻扫手势
[<UIView *view> addGestureRecognizer:swipe];
{% endhighlight %}

<a name = "9"></a>

### UITapGestureRecognizer

{% highlight objc %}
UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(tapAction:)];
//1.触发手势的手指个数
tap.numberOfTouchesRequired = 1;
//2.触发手势的轻拍次数
tap.numberOfTapsRequired = 2;
//3.添加轻拍手势
[<UIView *view> addGestureRecognizer:tap];
{% endhighlight %}











