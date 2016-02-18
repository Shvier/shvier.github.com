---
layout: post
title: "Cocoa框架之Foundation框架"
date: 2016-02-07 14:00:00
categories: Objective-C
---
# 概述
写这篇文章的初衷是想把自己之前在博客园的iOS学习笔记整理归纳到这里，这件事拖了好久，现在既然有空就做吧。   
文章可能有点长，目录如下：   

[TOC]
  
# Cocoa框架简述
Cocoa框架可以看做许多框架的集合，在iOS中包括两个非常重要的框架：`UIKit`和`Foundation`这两个框架。其中`Foundation`主要提供一些基本数据处理API供程序使用，`UIKit`则包括了许多UI绘制方面的API。
# Foundation框架结构
如目录结构所看到的，`Foundation`框架可大致看做如下结构：   
- **Cocoa框架简述**
- **Foundation框架结构**
- **NSObject**
 - **字符串类**
 - **集合类**
 - **数值类**
 - **日期类**
- **属性**
- **点语法**
- **类的扩展**
 - **协议**
 - **延展**
 - **类目**
- **内存管理**
 - **原理**
 - **管理方式**
 - **拷贝**
 - **dealloc方法**
- **KVC** 
# NSObject
`NSObject`几乎是`Objective-C`中所有类的父类，为什么是几乎？因为部分类继承自`NSProxy`，而这些类的应用比较特殊，在Cocoa程序中比较少见。
## 字符串类
在iOS中字符串类分为两种：`可变字符串`和`不可变字符串`，不止是字符串，其它的几种数据类型：`数组`、`字典`、`集合`都分为可变类型和不可变类型。   
不论是哪种字符串，iOS都提供了两个基础的方法**`length`**和**`characterAtIndex`**，其它所有方法都是从这两个基础方法中衍生而来。   
### 初始化
iOS对字符串提供了很多种初始化的方式：`alloc init`、`便利构造器`、`字面量`
### 文件操作
- **`stringWithContentsOfFile: encoding: error:`**读取文件
- **`writeToFile: atomically: encoding: error:`**写入文件，其中`atomically`表示一次性写入，如果写入过程中出错，就全部都不要写入了
### 常用方法
- **`isEqualToString:`**用于比较两个字符串是否相等
- **`compare:`**比较两个字符串的大小，返回一个`NSComparisonResult`枚举类型的值
- **`substringFromIndex:`与`substringToIndex:`**这两个方法都是用来截取字符串，其中**`substringFromIndex`**包含指定的下标位置，而**`substringToIndex`**不包含指定的下标位置。另外还有一个用来截取字符串的方法**`substringWithRange:`**，该方法需要传入一个`NSRange`结构体类型作为参数，截取指定范围的字符串
- **`stringByAppendingFormat:`**该方法用来拼接字符串
- **`stringByReplacingCharactersInRange: withString:`**该方法有两个参数，第一个参数需要一个`NSRange`类型的参数，表示用第二个字符串参数替换指定范围内的字符串
- **`intValue`**该方法可将字符串类型转为`NSInteger`类型
- **`uppercaseString`**将字符串全部转换为大写，与之对应的方法有**`lowercaseString`**将字符串转换为小写以及**`capitalizedString`**将字符串的首字母全部转为大写
- **`hasPrefix:`**传入一个字符串参数，用来判断字符串的头是否与传入的参数匹配，与之对应的方法有**`hasSuffix`**用来判断字符串的尾部是否与传入的参数匹配
### 可变字符串特有方法
- **`appendFormat:`**追加字符串
- **`insertString: atIndex:`**在指定的下标位置插入字符串
- **`deleteCharactersInRange:`**删除指定范围内的子串
- **`replaceCharactersInRange:`**替换指定范围内的子串
- **`setString:`**替换整个字符串
## 集合类
与字符串类似，集合类也都分为可变类型和不可变类型
### 数组
iOS中数组最大的特点就是可以存储不同类型的对象，需要注意的是`必须存放的是对象类型`。数组也有两个基础方法**`containsObject:`**和**`indexOfObject:`**
#### 初始化
#### 常用方法
- **`componentsSeparatedByString:`**该方法由`字符串调用`需要传入一个字符串类型的参数，将字符串按给定的参数分割成数组，并将这个数组返回
- **`componentsJoinedByString:`**该方法将数组用传入的字符串拼接起来，并将拼接后的字符串返回
#### 可变数组特有方法
- **`addObject:`**给数组添加一个对象
- **`insertObject: atIndex:`**在数组中指定的位置插入对象
- **`removeObject:`**移除数组中的对象
- **`removeLastObject`**移除数组中最后一个对象
- **`removeAllObjects`**移除数组中所有对象
- **`removeObjectAtIndex:`**移除数组中指定位置的对象
- **`replaceObjectAtIndex: withObject:`**使用指定的对象替换指定位置的对象
- **`exchangeObjectAtIndex: withObjectIndex:`**交换指定两个下标对应的对象
#### 枚举
```c
//1.for循环
for (int i = 0; i < array.count; i ++) {
	//...
}
//2.for in(需要数组对象类型完全一致)
for (id *obj in array) {
	//...
}
//3.正向枚举器
NSEnumerator *rator = [array objectEnumerator];
id obj = nil;
while (obj = [rator nextObject]) {
	//...
}
//4.反向枚举器
NSEnumerator *reverseRator = [array reverseObjectEnumarator];
while (obj = [reverseRator nextObject]) {
	//...
}
```
#### 排序
```objectivec
//1.NSSortDescriptor
NSSortDescriptor *des = [[NSSortDescriptor alloc] initWithKey:@"self" ascending:YES];//创建一个排序条件，ascending表示升序或者降序
NSArray *newArray = [array sortedArrayUsingDescriptors:@[des]];//对不可变数组来说，排序后得到新的数组，原数组不变，如果是可变数组，不会生成新数组
//2.SEL
/*该方法需要传入SEL规则即一个返回值类型为NSComparisonResult类型的比较函数，这里传入的是系统函数compare:
*/
NSArray *newArray = [array sortedArrayUsingSelector:@selector(compare:)];
```
### 字典
字典在内存中的存储不是连续的，所以无法对字典进行排序。字典也有一些基础方法：
- **`count`**获取字典中键值对的个数
- **`allKeys`**获取字典中所有的键
- **`allValues`**获取字典中所有的值
- **`objectForKey:`**根据键获得对应的值
#### 初始化
#### 可变字典特有方法
- **`setObject: forKey:`**修改某一键值对的值，如果所给的键没有找到，则添加新的键值对
- **`remoevObjectForKey:`**根据键移除指定的键值对
- **`removeAllObjects`**移除字典中所有的键值对
#### 枚举
```objectivec
//1.for in
for (id *obj in dict) {
	//...
}
//2.枚举器
NSEnumerator *rator = [dict keyEnumerator];
id key = nil;
while (key = [rator nextObject]) {
	//...
}
```
### 集合
#### 特点
- **`互异性`**集合中不能同时存在两个相同的对象
- **`无序性`**
- **`经常被用来处理重用问题`**
#### 基础方法
- **`count`**获取集合中对象的个数
- **`anyObject`**从集合中取出一个对象，这个对象是固定的，并不是随机的
#### 常用方法
- **`containsObject:`**判断集合中是否包含某一指定的对象
#### 可变集合特有方法
- **`addObject:`**添加一个对象
- **`removeObject:`**移除一个对象
- **`removeAllObjects`**移除所有对象
#### 枚举
```objectivec
//1.枚举器
NSEnumerator *rator = [set objectEnumerator];
id value = nil;
while (value = [rator nextObject]) {
	//...
}
//2.for in
for (id *obj in set) {
	//...
}
```
#### 计数集合
`NSCountedSet`添加各个对象时会自动统计各个对象的个数
## 数值类
在Objective-C中集合类必须存储对象类型的数组，所以对基本数据类型，需要将其转换为数值对象来存储
### NSNumber
将基本数据类型转换为对象
```objectivec
NSNumber *number = [NSNumber numberWithInt:1024];
int value = [number intValue];
```
### NSValue
主要用来将结构体类型转换为对象
```objectivec
NSValue *value = [NSValue valuehWithRange:NSRangeMake(0, 1)];
NSRangeMake range = [value rangeValue];
```
## 日期类
### NSDate
# block
`<return type>(^blockName)(list of arguments) = ^(arguments) {body};`
# 属性
采用`@property`关键字
## 原子性
- **`atomic`**
默认修饰符。保证了实例变量在多线程访问是安全的，但是会大量消耗性能
- **`nonatomic`**
与上一个相反，不允许进行多线程访问
## 读写性
- **`readwrite`**
默认修饰符。读写状态，通知编译器，既生成属性的`getter`方法，也生成`setter`方法
- **`readonly`**
只读状态，通知编译器，生成属性的`getter`方法，但不生成`setter`方法
- **`setter=`**
为属性的`setter`方法重新命名
- **`getter=`**
为属性的`getter`方法重新命名
## 语义特性
### MRC环境
- **`assign`**
基本数据类型
- **`retain`**
对象类型
- **`copy`**
遵循了`<NSCopying>`协议的对象类型
### ARC环境
- **`assign`**
基本数据类型
- **`strong`**
对象类型，相当于MRC中的retain
- **`weak`**
对象类型，但是内部不会对对象做retain操作
- **`copy`**
遵循了`<NSCopying>`协议的对象类型
## setter/getter
属性赋值和取值的方法，原理如下：
```objectivec
//setter
- (void)setName:(NSString *)name {
	_name = name;
}
//getter
- (NSString *)name {
	return name;
}
```
# 点语法
# 类的扩展
## 协议`protocol`
- 协议是一套标准，只有.h文件
- 接受协议的类实现协议中定义的方法
- 协议中的方法默认是必须实现的，即`@required`，`@optional`修饰的方法是可选的，可实现也可以不实现
## 延展`extension`
- 为能够获得源代码的类添加私有示例变量，即在类的.m里定义各个变量和方法
## 类目`category`
- 扩充的功能会成为原有类的一部分   
- 可以通过原有类或者原有类的对象直接调用，并且可以继承   
- 该方式只能扩充方法，不能扩充实例变量
# 内存管理
## 管理方式
iOS程序采用`引用计数`的内存管理方式，而OSX程序采用`垃圾回收`和`引用计数`的内存管理方式
## 原理
在iOS/OSX程序中，每当对象进行一次`生成`或者`持有`操作时，引用计数加1，每进行一次`释放`操作时引用计数减1，而当引用计数为0`(在实际中并不会为0，而是1，只是为了语言表达方便说成是0)`时，从内存中销毁该对象
- **`生成对象alloc`**对象的引用计数加1
- **`持有对象retain`**对象的引用计数加1
- **`释放对象release/autorelease`**对象的引用计数减1
- **`dealloc`**销毁对象  
## 规则
- alloc创建的必须释放，便利构造器创建的不要释放
- 加入容器中的对象会被执行一次retain操作，引用计数加1
- 容器移除对象，会向对象发送一次release消息，让对象的引用计数减1
- 当容器释放对象时，会向容器中的所有对象发送一次release消息
## 拷贝
- **`伪拷贝`**拷贝地址，相当于retain操作，引用计数加1
```objectivec
- (id)copyWithZone:(NSZone *)zone {
	return [self retain];
}
```
- **`浅拷贝`**为对象开辟新的空间，但是两个对象的实例变量指向同一块空间
```objectivec
- (id)copyWithZone:(NSZone *)zone {
	Student *student = [Student allocWithZone:zone] init];
	student.name = self.name;
	return student;
}
```
- **`深拷贝`**为对象开辟新的空间，并且两个对象的实例变量指向不同的空间
```objectivec
- (id)copyWithZone:(NSZone *)zone {
	Student *student = [Student allocWithZone:zone] init];
	student.name = [self.name mutableCopy];
	return student;
}
```
## dealloc方法
# KVC
`KVC`即`key-value-coding`，中文是键值编码的意思。KVC提供了一种使用字符串(key)而不是访问器的方法，去访问一个对象的实例变量
## 常用方法
- **`valueForKey:`**通过key(实例变量名)获取实例变量的值
- **`setValue: forKey:`**通过key(实例变量名)给实例变量赋值
- **`valueForKeyPath:`**通过keyPath获取实例变量的值
- **`setValue: forKeyPath:`**通过keyPath给实例变量赋值
- **`setValuesForKeysWithDictionary:`**将一个字典对象的键值对给对象的各个实例变量赋值。不过，如果任一key没有找到对象的实例变量，程序便会出错，先看看KVC的内部实现过程(假如给的key是`name`)：
- **`setValue: forKey:`**
 - 去类里面找是否有一个方法叫做setName:，有的话赋值，没有的话执行第二步
 - 去类里面找是否有一个叫做_name的实例变量，有的话赋值，没有的话执行第三步
 - 去类里面找是否有一个叫做name的实例变量，有的话赋值，没有的话执行第四步
 - 查找当前类是否实现了**`setValue: forUndefinedKey:`**，如果有，走方法内部实现，如果没有，就会抛出异常
- **`valueForKey:`**
 - 去类里面找是否有一个方法叫做getName:，有的话取值，没有的话执行第二步
 - 去类里面找是否有一个叫做_name的实例变量，有的话取值，没有的话执行第三步
 - 去类里面找是否有一个叫做name的实例变量，有的话取值，没有的话执行第四步
 - 查找当前类是否实现了**`valueForUndefiendKey:`**，如果又，走方法内部实现，如果没有，就会抛出异常   

所以为了防止崩溃的情况方法，只需要重写以下方法：
- **`setValue: forUndefinedKey:`**当使用KVC给类的属性赋值时，如果未找到该属性名，需要重写该方法来处理未找到属性名的情况，如果不重写该方法，程序会出错
- **`valueForUndefinedKey:`**与上一个方法对应，当访问属性时，如果未找到该属性名，该方法用来处理未找到键值名的情况