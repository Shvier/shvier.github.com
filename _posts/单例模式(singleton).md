
# CoreData

* 显示SQL语句
  * 打开`Product`，点击`EditScheme`
  * 点击`Arguments`，在`ArgumentsPassed On Launch`中添加
  	* com.apple.CoreData.SQLDebug
  	* 1
 * 分页查询   
 
 	   // 分页的起始索引
 	   request.fetchOffset = 0;
 	   // 分页的条数
 	   request.fetchLimit = 6;
 
 * 模糊查询
 
    	NSPredicate *predicate = [NSPredicate predicateWithFormat:@"name like %@", no];
    	
# 网络编程

* `NSURLConnection`和`NSURLSession`   
在iOS 7之后，`NSURLSession`作为系统推荐使用的HTTP请求框架，在进行前台请求的情况下，`NSURLSession`与`NSURLConnection`并无太大差异，对于后台的请求，`NSURLSession`更加灵活。
	* `NSURLSession`任务的类型
		1. 数据任务：使用NSData对象进行数据的发送和获取，一般用于短数据的任务
		2. 下载任务：从文件下载数据，支持后台下载
		3. 上传任务：以文件的形式上传数据，支持后台上传

# Tips

* SEL:Selector表示方法的存储位置
	* 寻找方法的过程 
		1. 首先把这个方法名包装成类型的数据；
		2. 根据数据找到对应的方法地址；
		3. 根据方法地址调用相应的方法；
		4. 在这个操作过程中有缓存，第一次找的时候是一个一个的找，非常耗性能，之后再用到的时候就直接调用
	* SEL其实是对方法的一种包装，将方法包装成一个类型的数据，去寻找对应的方法地址，找到方法地址后就可以调用方法，这些都是运行时特性。
* 变量作用域
	一个类继承了另一个类，那么就拥有了父类的所有成员变量和方法，注意所有的成员变量都拥有，只是有的它不能直接访问
* 类的加载和初始化
	1. 当程序启动时，就会加载项目中所有的类和分类，而且加载后会调用每个类和分类的`load`方法，只会调用一次
	2. 当第一次使用某个类时，就会调动当前类的`initialize`
	3. 先加载父类，再加载子类(先调用父类的`load`方法，再调用子类的`load`，最后调用分类的`load`)，先初始化父类，再初始化子类(先调用父类的`initialize`方法，再调用子类的`initialize`)
	4. 注意：在初始化的时候，如果在分类中重写了`initialize`方法，则会覆盖掉父类的
	5. 重写`initialize`方法可以监听类的使用情况
* `#include`和`#import`的区别
	1. `import`和`include`一样，完完全全的拷贝文件的内容
	2. 可以自动防止文件的重复拷贝(即使文件被多次包含，也只拷贝一份) 
* 视图控制器的功能：
	1. 控制视图大小变换、布局视图、响应事件
	2. 检测以及处理内存警告
	3. 检测以及处理屏幕旋转
	4. 检测视图的切换
	5. 实现模块独立，提高复用性 
* 改变状态栏样式
   	
   		- (UIStatusBarStyle)preferredStatusBarStyle {
   			return UIStatusBarStyleLightContent;
   		}
   
* 计算文字高度(根据文字长度改变文本框大小)

   		/*
   		@param text 需要计算尺寸的文字
   		@param font 文字的字体
   		@param maxSize 文字的最大尺寸
   		*/
   		- (CGSize)sizeWithText:(NSString *)text font:(UIFont *)font maxSize:(CGSize)maxSize {
   			NSDictionary *attrs = @{NSFontAttributeName:font};
   			return [text boundingRectWithSize:max options:NSStringDrawingUsesLineFragmentOrigin attributes:attrs context:nil].size];
   		}

* 通过代码自定义cell(cell的高度不一致)
	1. 新建一个继承自UITableViewCell的类
	2. 重写initWithStyle: reuseIdentifier:方法
		* 添加所有需要显示的子控件(不需要设置子控件的数据和frame，子控件要添加到contentView中)
		* 进行子控件一次性的属性设置(有些属性需要设置一次，比如字体\固定的图片)
	3. 提供2个模型
		* 数据模型：存放文字数据\图片数据
		* frame模型：存放数据模型\所有子控件的frame\cell的高度
	4. cell拥有一个frame模型(不要直接拥有数据模型)
	5. 重写frame模型属性的setter方法：在这个方法中设置子控件的显示数据和frame
	6. frame模型数据的初始化已经采取懒加载的方式(每一个cell对应的frame模型数据只加载一次)
	
* tableView的设置一定要及时更新，之前用过的cell的某些属性可能并不适用于新的cell，一定要处理冗余数据，将多次计算cell属性的方法封装，提高性能
	* `scrollToRowAtIndexPath: atScrollPosition: animated:`点击加载更多数据后，滚到新数据的那一行
* `readonly`修饰的属性，只能通过`_`方式赋值，因为没有生成setter方法所以不能适用点语法
* 在通知中心注册过的对象，必须在该对象释放前取消注册   
	* 通知和代理
		1. 共同点
			* 利用通知和代理都能完成对象之间的通信
		2. 不同点
			* 代理：一对一关系
			* 通知：多对多关系 
* 弹出键盘时滚动视图
   
   		- (void)keyboardWillChangeFrame:(NSNotification *)notification {
   			// 设置窗口的颜色，这样当键盘弹出时不会看到底部黑色
   			self.view.window.backgroundColor = self.tableView.backgroundColor;
   			// 取出键盘动画的时间
   			CGFloat duration = [notification.userInfo[UIKeyboardAnimationDurationUserInfoKey] doubleValue];
   			// 取出键盘最后的frame
   			CGRect keyboardFrame = [notification.userInfo [UIKeyboardFrameEndUserInfoKey] CGRectValue];
   			// 计算控制器的view需要移动的距离
   			CGFloat transformY = keyboardFrame.origin.y - self.view.frame.size.height;
   			// 执行动画
   			[UIView animateWithDuration:duration animations:^{
   				self.view.transform = CGAffineTransformMakeTranslation(0, transformY);
   			}];
   			/*
   			// 动画执行节奏(速度)，枚举
   			UIKeyboardAnimationCurveUserInfoKey = 7;
   			// 键盘弹出\隐藏动画所需要的时间
   			UIKeyboardAnimationDurationUserInfoKey = "0.25";
   			UIKeyboardBoundsUserInfoKey = "NSRect: {{0, 0}, {375, 258}}";
   			UIKeyboardCenterBeginUserInfoKey = "NSPoint: {187.5, 559}";
   			UIKeyboardCenterEndUserInfoKey = "NSPoint: {187.5, 538};
   			UIKeyboardFrameBeginUserInfoKey = "NSRect: {{0, 451}, {375, 216}}";// 键盘刚出来的那一刻的frame
   			UIKeyboardFrameEndUserInfoKey = "NSRect: {{0, 409}, {375, 258}}";// 键盘显示完毕后的frame
   			UIKeyboardIsLocalUserInfoKey = 1;
   			*/
   		}
   		
* 当一个控件被添加到父视图时调用

	  - (void)didMoveToSuperView {
	  	// 刷新视图用，因为存在重用池机制
	  }

* tableViewCell右侧的小箭头
    
      cell.accessoryType = UITableViewCellAccessoryNone;
      cell.accessoryType = UITableViewCellAccessoryDisclosureIndicator;
      cell.accessoryType = UITableViewCellAccessoryDetailDisclosureButton;
      cell.accessoryType = UITableViewCellAccessoryCheckmark;
   	  
* `clipsToBounds`裁剪图片边框，默认为YES
* `layoutSubviews`一定要调用父类布局`[super layoutSubviews];`
* `friend`(友元函数)是C++关键字，不能用friend作为属性名
* `UIApplication`对象是应用程序的象征，每个应用都有自己的UIApplication对象，而且是单例的
* `makeKeyAndVisible`的作用是让某个window成为keyWindow并且课件，`makeKey`只是让某个window成为keyWindow，而不会使其可见，keyWindow等于最新设置的window，键盘打开时也是`[UIApplicaiton sharedApplication].windows里的对象

# TableViewController编辑

`EditButtonItem`每一个TableViewController都有自己的一个编辑按钮，因为项目中编辑的应用场景非常多，所以系统预留了一个编辑按钮与`setEditing: animated:`方法关联。   

	// 1.让将要执行删除、添加操作的表视图处于编辑状态
	- (void)setEditing:(BOOL)editing animated:(BOOL)animated {
		[super setEditing:editing animated:animated];
		[self.tableView setEditing:editing aniamted:animated];
	}
	
	// 2.指定表视图中哪些行可以处于编辑状态
	- (BOOL)tableView:(UITableView *)tableView canEditRowAtIndexPath:(NSIndexPath *)indexPath {
		// 默认全部行都可以进行编辑
		return YES:
	}
	
	// 3.指定编辑样式，到底是删除还是添加
	- (UITableViewCellEditingStyle)tableView:(UITableView *)tableView editingStyleForRowAtIndexPath:(NSIndexPath *)indexPath {
		// 此方法如果不重写，默认是删除样式
		return UITableViewCellEditingStyleInsert;
	}
	
	// 4.不管删除还是添加，这个方法才是操作的核心方法
	- (void)tableView:(UITableView *)tableView commitEditingStyle:(UITableViewCellEditingStyle)editingStyle forRowAtIndexPath:(NSIndexPath *)indexPath {
		// 表视图开始更新
		[tableView beginUpdates];
		
		if (editingStyle == UITableViewCellEditingStyleDelete) {
			// 将该位置下的单元格删除
			[tableView deleteRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationFade];
			// 删除数据数组中与该表格绑定的数据
			[_dataArray removeObjectAtIndex:indexPath.row];
		} else if (editingStyle == UITableViewCellEditingStyleInsert) {
			Student *student = _dataArray[indexPath.row];
			// 构建一个位置信息
			NSIndexPath *index = [NSIndexPath indexPathForRow:0 inSection:)];
			[tableView insertRowsAtIndexPaths:@[indexPath] withRowAnimation:UITableViewRowAnimationMiddle];
			[_dataArray insertObject:student atIndex:index.row];
		}
		// 表视图更新结束
		[tableView endUpdates];
	}
	
# TableViewController移动

    // 1.开启编辑状态
    - (void)setEditing:(BOOL)editing animated:(BOOL)animated {
    	[super setEditing:editing animated:animated];
    	[self.tableView setEdtiting:editing animated:animated];
    }
    
    // 2.指定可以移动的行
    - (BOOL)tableView:(UITableView *)tableView canMoveRowAtIndexPath:(NSIndexPath *)indexPath {
    	// 默认都可以移动
    	return YES:
    }
    
    // 3.移动完成后要做什么事，怎么完成移动
    - (void)tableView:(UITableView *)tableView moveRowAtIndexPath:(NSIndexPath *)sourceIndexPath toIndexPath:(NSIndexpath *)destinactionIndexPath {
    	// 先记录原有位置下的模型数据
    	Student *student = _dataArray[sourceIndexPath.row];
    	// 删除原位置下的模型数据
    	[_dataArray removeObjectAtIndex:sourceIndexPath.row];
    	// 在新位置将记录的模型数据添加到数据数组中
    	[_datarray insertObject:student atIndex:destinationIndexPath.row];
    }

# 选取照片

	- (IBAction)click:(id)sender {
		// 创建提醒视图
		UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"提醒" message:nil preferredStyle:UIAlertControllerStyleActionSheet];
		UIAlertAction *action1 = [UIAlertAction actionWithTitle:@"相机" style:UIAlertActionStyleDestructive handler:^(UIAlertAction *_Nonnull action) {
			// 判断设备是否存在摄像头，有调用摄像机，没有则提醒用户
			if ([UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera]) {
				// 创建相机
				UIImagePickerController *picker = [[UIImagePickerController alloc] init];
				// 指定数据来源为相机
				picker.sourceType = UIImagePickerControllerSourceTypeCamera;
				// 指定代理
				picker.delegate = self;
				// 允许编辑
				picker.allowsEditing = YES;
				[self presentViewController:picker animated:YES completion:nil];
			} else {
				// 没有摄像头提醒用户
			}
		}];
		UIAlertAction *action2 = [UIAlertAction actionWithTile:@"相册" style:UIAlertActionStyleDefault handler:^(UIAlertAction *_Nonnull action) {
			UIImagePickerController *picker = [[UIImagePickerController alloc] init];
			// 指定数据来源为相册
			picker.sourceType = UIImagePickerControllerSourceTypePhotoLibrary;
			picker.delegate = self;
			picker.allowsEditing = YES:
			[self presentViewController:picker animated:YES completion:nil];
		}];
		[alertController addAction:action1];
		[alertController addAction:action2];
		[self presentViewController:alertController animated:YES completion:nil];
	}
	
	- (void)imagePickerController:(UIImagePickerController *)picker didFinishPickingMediaWithInfo:(NSDictionary<NSString *, id> *)info {
		NSLog(@"%@", info);
		UIImage *image = [info objectForKey:UIImagePickerControllerOriginalImage];
		self.imageView.image = image;
		[picker dismissViewControllerAnimated:YES completion:nil];
	}


# 可视化编程(storyboard&xib)

* 通过`storyboard`加载UIViewController

      UIStoryboard *storyboard = [UIStoryboard storyboardWithName:@"Main" bundle:nil];
      UIViewController *vc = [storyboard instantiateInitialViewController];// instantiateInitialViewControllerWithIdentifier:@""
      
* 通过xib加载ViewController
	1. 设置xib的`File's Owner`及ViewController
	2. `initWithNibName`
	
创建view顺序为：优先调用`loadView`方法，没有查找`storyboard`加载，没有，查找`xib`加载，没有查找`alloc init`

* Segue的属性
	1. 唯一标识
	`@property (nonatomic, readonly) NSString *identifier;`
	2. 来源控制器
	`@property (nonatomic, readonly) id sourceViewContorller;`
	3. 目标控制器
	`@property (nonatomic, readonly) id destinationViewController;` 

# 单例模式(SingleTon)

单例就是单个实例对象，保证对象不管创建多少次，都是唯一一个   
[UIScreen mainScreen]   
[UIDevice currentDevice]   
[NSFileManager defaultManager]   
[NSUserDefaults standardUserDefaults]   

	id _tool;
	+ (id)allocWithZone:(struct _NSZone *)zone {
		@synchronized(self) {
			if (!_tool) {
				_tool = [[Tool alloc] init];
			}
		}
		return _tool;
	}
	
	+ (instancetype)sharedTool {
		return [[self alloc] init];
	}
	
	// copy有可能会产生新的对象
	// copy方法内部会调用copyWithZone:
	- (id)copyWithZone:(struct _NSZone *)zone {
		return _tool;
	}
static修饰变量   
1. 修饰局部变量   
局部变量的生命周期跟全局变量类似，但是不能改变作用域，能保证局部变量永远只初始化1次，在程序运行过程中，永远只有1份内存   
2. 修饰全局变量   
外部文件无法访问，编译时就不会通过    

1.懒汉式:使用时再创建单例   

	static id _instance;
	// alloc方法内部会调用这个方法
	+ (id)allocWithZone:(struct _NSZone *)zone {
		if (!_instance) { // 防止频繁加锁
			@synchronized(self) {
				if (!_instance) {
					_instance = [super allocWithZone:zone];
				}
			}
		}
		return _instance;
	}
	
	+ (instancetype)sharedMusicTool {
		/*
		if (!_instance) {
			@synchronized(self) {
				if (!_instance) {
					_instance = [[self alloc] init];
				}
			}
		}
		*/
		statice dispatch_once_t onceToken;
		dispatch_once(&onceToken, ^{
			_instance = [[self alloc] init];
		});
		return _instance;
	}
	
	- (id)copyWithZone:(NSZone *)zone {
		return _instance;
	}
	
2.饿汉式:进入内存就创建单例   
`+ (void)load`一进入内存就会调用此方法，第一次使用这个类会调用initialize(包括子类初始化时，也会调用父类的initialize)如果想某个类永远只执行1次某个操作，那么这个操作放在`load`方法中最合适   
	
	static id _instance;
	
	+ (id)allocWithZone:(struct _NSZone *)zone {
		if (!_instance) {
			@synchronized(self) {
				if (!_instance) {
					_instance = [super allocWithZone:zone];
				}
			}
		}
		return _instance;
	}
	
	+ (instancetype)sharedDataTool {
		static dispatch_once_t onceToken;
		dispatch_once(&onceToken, ^{
			_instance = [[self alloc] init];
		});
		return _instance;
	}
	
	+ (void)load {
		_instance = [[self alloc] init];
	}
	
	- (id)copyWithZone:(NSZone *)zone {
		return _instance;
	}
	
	// 非ARC环境下需要重写release和retain方法(ARC环境下不允许重写)
	- (oneway void)release {
	
	}
	
	- (id)retain {
		return self;
	}
	
	- (NSUInteger)retainCount {
		return 1;
	}
	
	- (id)autorelease {
		return self;
	}
	
# 宏定义

用\连接多行定义   
用##连接需要替换的字符   

	#if __has_feature(objc_arc)
		// 编译器是ARC环境
	#else
		// 编译器是MRC环境
	#endif
	
	#ifdef DEBUG
	#define Log(...) NSLog(__VA_ARGS__)
	#else
	#define Log(...)
	#endif
	
	#ifdef __OBJC__
	#import <UIKit/UIKit.h>
	#import <Founcation/Foundation.h>
	#endif
	
# @synthesize

@synthesize是对属性的实现，实际就是指定setter和getter操作的实例变量名   
@synthesize musicArray默认操作就是实例变量和属性同名   
Xcode4.5之后@synthesize可以省略，系统默认实现了setter和getter，但是如果setter和getter都重写，必须使用@synthesize   

# 内存区(从上到下越来越小)

栈区:与函数相关的   
堆区:必须有alloc函数分配内存   
全局静态区:static或者全局变量   
常量区:字符串(int a = 5这样的不在内存区，在寄存器里，称为立即数)   
代码区：存放编译后的二进制代码(函数名、if else这些字)   

#### 内存管理

* Objective-C内存管理方式：垃圾回收(OSX)和引用计数
* iOS内存管理方式：引用计数
* 字符串是特殊的对象，但不需要使用release手动释放，这种字符串对象默认就是autorelease的，不要额外去管理内存

#### 循环引用

`@class 类名`解决循环引用问题(#import)，提高性能   
`@class`仅仅告诉编译器，在进行编译的时候把后面的名字作为一个类来处理

#### autorelease

* 使用注意：
	1. 占用内存较大的对象，不要随便使用autorelease，应该使用release来精确控制
	2. 占用内存较小的对象，没有太大影响
* 自动释放池
	* 在程序运行过程中，会创建无数个池子，以栈结构存在
	* 当一个对象调用autorelease时，会将这个对象放到位于栈顶的释放池中
		1. 5.0以前
	 
		  	  NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
		  	  [pool release];
		2. 5.0以后
		
			  @autorelease {
			  
			  }

# 动画

在创建UIView对象时，UIView内部会自动创建一个图层(即CALayer对象)，通过UIView的layer属性可以访问这个层。   
当UIView需要显示到屏幕上时，会调用`drawRect:`方法进行绘图，并且会将所有内容绘制在自己的图层上，绘制完毕后，系统会将图层拷贝到屏幕上，于是就完成了UIView的显示   
UIView本身不具备显示的功能，是它内部的层才有显示功能。   
通过`CALayer`，能做出跟UIImageView一样的界面效果。   
对比`CALayer`，UIView多了一个事件处理的功能，也就是说，CALayer不能处理用户的触摸事件，而UIView可以，所以，如果显示出来的东西需要交互，用UIView；不需要交互，用UIView或者CALayer都可以。   
`CALayer更轻量`   
CALayer定义在`QuartzCore`框架中   
`CGImageRef`、`CGColorRef`两种数据类型定义在`CoreGraphics`框架中   
`UIColor`、`UIImage`定义在`UIKit`框架中   
`QuartzCore`框架和`CoreGraphics`框架是可以跨平台使用的，在iOS和Mac OSX上都能使用，但是`UIKit`只能在iOS中使用。

#### view的完整显示流程

1. view.layer会准备一个Layer Graphics Context(图层类型的上下文)
2. 调用view.layer.delegate(view)的`drawLayer: inContext:`，并传入刚才准备好的上下文
3. `drawLayer: inContext:`方法内部又会调用`drawRect:`方法
4. view可以在`drawRect:`方法中实现绘图代码，所有东西最终都绘制到view.layer上面
5. 系统再将view.layer的内容拷贝到屏幕，于是完成了view的显示
   
* 如果两个UIView是父子关系，那么它们内部的CALayer也是父子关系。
* 图层动画都是假象，在动画执行过程中，图层的position属性一直没有变过。


# 语义设置
* `strong`和`weak`   
	ARC下的强指针和弱指针
	* 强指针：默认情况下，任何指针都是强指针
	* 弱指针：使用__weak修饰的指针   
	ARC环境下系统的判断准则：只要没有任何强指针指向对象，这个对象就会被销毁   
	当循环引用发生时，必须有一个对象为弱指针才可以解决该问题
* `weak(assign)`:代理\某些UI控件
* `strong(retain)`:其他对象(除代理\UI控件\字符串以外的对象)
* `copy`:字符串
* `assign`:非对象类型(基本数据类型int\float\BOOL\枚举\结构体)

# __block

在MRC中使用__block来修饰对象类型，不会对对象类型进行持有造成引用计数增加   
__weak只能在ARC中使用，__block在ARC和MRC中都能使用