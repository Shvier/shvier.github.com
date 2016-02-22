---
layout: post
title: "UIView之UITableView"
date: 2016-02-11 15:01:13
categories: Objective-C
---

# 目录

- [UITableView概述](#1)
  - [初始化](#1.1)
  - [重要属性](#1.2)
     - [外观属性](#1.2.1)
     - [代理相关属性](#1.2.2)
       - [UITableViewDataSource](#1.2.2.1)
       - [UITableViewDelegate](#1.2.2.2)
  - [UITableViewCell](#1.3)
  - [编辑UITableView](#1.4)
  - [# UITableViewController](#1.5)

<a name = "1"></a.

# UITableView概述

- `UITableView`继承于`UIScrollView`，可以滚动
- `UITableView`的每一条数据对应的单元格叫做cell，是UITableViewCell的一个对象，继承与UIView
- `UITableView`可以分区显示，每一个分区称为section，每一行称为row，编号都从0开始
- 系统提供了一个专门的类来整合section和row，叫做NSIndexPath

<a name = "1.1"></a>

### 初始化

{% highlight objc %}
UITableView *tableView = [[UITableView alloc] initWithFrame:self.view.bounds style:UITableViewStylePlain];
[self.view addSubview:tableView];
[tableView release];
{% endhighlight %}

<a name = "1.2"></a>

### 重要属性

<a name = "1.2.1"></a.

#### 外观属性

- `UITableViewStyle`   
枚举类型，` UITableViewStylePlain`和`UITableViewStyleGrouped`
- `rowHeight`   
行高
- `separatorStyle`   
分隔线样式
- `separatorColor`   
分隔线颜色
- `tableHeaderView`   
UITableView的置顶视图
- `tableFooterView`   
UITableView的置底视图

<a name = "1.2.2"></a>

#### 代理相关属性

- UITableView中有两个跟代理协议相关的属性：

{% highlight objc %}
// 显示数据相关的代理
@property (weak, nonatomic) id<UITableViewDataSource> dataSource;
// 视图操作相关的代理
@property (weak, nonatomic) id<UITableViewDelegate> delegate;
{% endhighlight %}

<a name = "1.2.2.1"></a>

###### UITableViewDataSource

- `UITableViewDataSource`是负责给UITableView对象提供数据的协议
- 协议中有两个必须实现的协议方法   

  1. `- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section`   
  UITableView每个分区包含的行数
  2. `- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath`   
  每一行要显示的cell

{% highlight objc %}
- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
	return 10;
}
- (UITableViewCell *)tableView:(UITableView *)tableViewcellForRowAtIndexPath:(NSIndexPath *)indexPath {
	UITableViewCell *cell = [[[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:@"reuse"] autorelease];
	cell.textLabel.text = @"title";
	return cell;
}
{% endhighlight %}

**其他协议方法**

- `- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView`   
UITableView分区个数
- `- (NSString *)tableView:(UITableView *)tableView titleForHeaderInSection:(NSInteger)section`   
分区的顶部标题
- `- (NSString *)tableView:(UITableView *)tableView titleForFooterInSection:(NSInteger)section`   
分区的底部标题
- `- (NSArray *)sectionIndexTitlesForTableView:(UITableView *)tableView`  
UITableView右侧的索引目录 

<a name = "1.2.2.2"></a>

###### UITableViewDelegate

- `- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath`告诉delegate选中了一个cell  - `- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath;`   每一行的高度- `- (CGFloat)tableView:(UITableView *)tableView heighForHeaderInSection:(NSInteger)section`   每个分区的顶部高度- `- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section`   每个分区的顶部定义视图

<a name = "1.3"></a>

### UITableViewCell 

#### 视图属性

- `UIImageView *imageView`   
图片视图
- `UILabel *textLabel`   
标题视图
- `UILabel *detailTextLabel`   
副标题视图

#### 重用机制

1. 当一个cell被滑出屏幕时，这个cell会被系统放到响应的重用池中
2. 当tableView需要显示一个cell，会先去重用池中尝试获取一个cell
3. 如果重用池没有cell，就会创建一个cell
4. 取得cell之后会重新复制进行使用

{% highlight objc %}
[tableView registerClass:[UITableViewCell class] forCellReuseIdentifier:@"reuse"];
- (UITableViewCell *)tableView:(UITableView *)tableViewcellForRowAtIndexPath:(NSIndexPath *)indexPath{	// 从重用池中取得一个cell, 如果重用池中没有cell, 系统会根据注册cell类自动创建一个返回    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"reuse"];
	cell.textLabel.text = @"title"; 	return cell;}{% endhighlight %}

<a name = "1.4"></a>
### 编辑UITableView

{% highlight objc %}//让UITableView处于编辑状态- (void)setEditing:(BOOL)editing animated:(BOOL)animated
//协议设定1.确定Cell是否处于编辑状态 - (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath2.设定Cell的编辑样式(删除、添加) - (UITableViewCellEditingStyle)tableView:(UITableView *)tableView editingStyleForRowAtIndexPath:(NSIndexPath *)indexPath3.编辑状态进行提交 - (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath: (NSIndexPath *)indexPath{% endhighlight %}
#### UITableView移动
{% highlight objc %}
//1.实现协议:告诉UITableView是否能够移动- (BOOL)tableView:(UITableView *)tableView canMoveRowAtIndexPath:(NSIndexPath *)indexPath//2.移动- (void)tableView:(UITableView *)tableView moveRowAtIndexPath:(NSIndexPath *)sourceIndexPath toIndexPath:(NSIndexPath *)destinationIndexPath{% endhighlight %}
<a name = "1.5"></a>### UITableViewController

- `UITableViewController`继承UIViewController, 自带一个tableView
- self.view不是UIView是UITableView
- dataSource和delegate默认都是self(UITableViewController)- 开发中只需要建立UITableViewController类 