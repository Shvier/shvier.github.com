---
layout: post
title: "UIView之UICollectionView"
date: 2016-02-13 16:24:47
categories: Objective-C
---



<a name = "1"></a>

# UICollectionView概述

- UICollection的实现跟tableView不同的地方在于item的布局，需要用`UICollectionViewLayout`类来描述视图的布局

<a name = "1.1"></a>

### UICollectionViewFlowLayout

#### 初始化

{% highlight objc %}
UICollectionViewFlowLayout *flowLayout = [[UICollectionViewFlowLayou alloc] init];
// 设置每个item的大小
flowLayout.itemSize = CGSizeMake(100, 100);
// 设置每个item的最小列间距(默认是10)
flowLayout.minimumInteritemSpacing = 10;
// 设置分区间隔(上，左，下，右)
flowLayout.sectionInset = UIEdgeInsetsMake(10, 10, 10, 10);
// 设置UICollectionView的滑动方向
flowLayout.scrollDirection = UICollectionViewScrollDirectionVertical;
// 头部引用的尺寸
flowLayout.headerReferenceSize = CGSizeMake(100, 100);
// 尾部引用的尺寸
flowLayout.footerReferenceSize = CGSizeMake(100, 100);
{% endhighlight %}

<a name = "1.2"></a>

### 初始化

{% highlight objc %}
UICollectionView *collectionView = [[UICollectionView alloc] initWithFrame:[UIScreen mainScreen].bounds collectionViewLayout:flowLayout];
{% endhighlight %}

<a name = "1.3"></a>

### 设置代理

- UICollectionView和UITableView一样，也需要遵守两个代理协议   
  1. `UICollectionViewDelegate`
  2. `UICollectionViewDataSource`

<a name = "1.3.1"></a>

#### 必须实现的代理方法

{% highlight objc %}
// 返回item的个数
- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section {
	return 100;
}
// 创建item视图对象
- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
	UICollectionViewCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"collectionCell" forIndexPath:indexPath];
	cell.backgroundColor = [UIColor orangeColor];
	return cell;
}
{% endhighlight %}

<a name = "1.3.2"></a>

#### 选择实现的代理方法

{% highlight objc %} 
// 返回有多少个分区
- (NSInteger)numberOfSectionsInCollectionView:(UICollectionView *)collectionView {
	return 2;
}
// item点击方法
- (void)collectionView:(UICollectionView *)collectionView didSelectItemAtIndexPath:(NSIndexPath *)indexPath {
	NSLog(@"clicked!");
}
// 返回头部、尾部视图样式
// UICollectionView不能像UITableView一样直接指定头部和尾部视图，需要注册使用
// 注册头部视图
[collectionView registerClass:[UICollectionReusableView class] forSupplementaryViewOfKind:UICollectionElementKindSectionHeaderwithReuseIdentifier:@"headerView"];
// 注册尾部视图
[collectionView registerClass:[UICollectionReusableView class] forSupplementaryViewOfKind:UICollectionEelementKindSectionFooterwithReuseIdentifer:@"footerView"];
// 返回头部和尾部的视图样式
- (UICollectionReusableView *)collectionView:(UICollectionView *)collectionView viewForSupplementaryElementOfKind:(NSString *)kind atIndexPath:(NSIndexPath *)indexPath {
	if ([kind isEqualToString:UICollectionElementKindSectionHeader]) {
		UICollectionReusableView *headerView = [collectionView dequeueReusableSupplementaryViewOfKind:UICollectionElementKindSecionHeaderwithReuseIdentifier:@"headerView" forIndexPath:indexPath];
		// 设置头部视图的样式
		headerView.backgroundColor = [UIColor orangeColor];
		return headerView;
	} else {
		UICollectionReusableView *footerView = [collectionView dequeueReusableSupplementaryViewOfKind:UICollectionElementKindSecionHeaderwithReuseIdentifier:@"footerView" forIndexPath:indexPath];
		// 设置头部视图的样式
		footerView.backgroundColor = [UIColor greenColor];
		return footerView;
	}
}
{% endhighlight %}

<a name = "1.3.3"></a>

#### 布局协议

- **`UICollectionViewDelegateFlowLayout`**   
对UICollectionViewDelegate的扩展

<a name = "1.4"></a>

### 自定义瀑布流