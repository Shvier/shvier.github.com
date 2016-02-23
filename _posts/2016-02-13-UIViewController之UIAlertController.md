---
layout: post
title: "UIViewController之UIAlertController"
date: 2016-02-13 20:21:38
categories: Objective-C
---

# UIAlertController

- 在iOS 8以后，`UIAlertController`在功能上是和`UIAlertView`以及`UIActionSheet`相同的，UIAlertController以一种模块化替换的方式来代替这两者的功能和作用。是使用对话框(alert)还是使用上拉菜单(action sheet)，就取决于在创建控制器时如何设置首选样式。

### 初始化

{% highlight objc %}
UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"标题" message:@"这个是UIAlertController的默认样式" preferredStyle:UIAlertControllerStyleAlert];
{% endhighlight %}

### UIAlertAction

- 通过创建`UIAlertAction的`实例，可以将动作按钮添加到控制器上
- `UIAlertAction`由标题字符串、样式以及当用户选中该动作时运行的代码块组成
- `通过UIAlertActionStyle`，可以选择如下三种动作样式：
  - 常规(default)
  - 取消(cancel)
  - 警示(destruective)
- 为了实现原来在创建UIAlertView时创建的按钮效果，只需创建这两个动作按钮并将它们添加到控制器上即可

{% highlight objc %}
UIAlertAction *cancelAction = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:nil];
UIAlertAction *okAction = [UIAlertAction actionWithTitle:@"好的" style:UIAlertActionStyleDefault handler:nil];
[alertController addAction:cancelAction];
[alertController addAction:okAction];
{% endhighlight %}

最后，显示这个视图控制器即可

{% highlight objc %}
[self presentViewController:alertController animated:YES completion:nil];
{% endhighlight %}

### 文本对话框

- `UIAlertController`极大的灵活性意味着不必拘泥于内置样式。以前只能在默认视图、文本框视图、密码框视图、登录和密码输入框视图中选择，现在可以对话框中添加任意数目的UITextField对象，并且可以使用所有的UITextField特性。

{% highlight objc %}
UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"文本对话框" message:@"登录和密码对话框示例" preferredStyle:UIAlertControllerStyleAlert];
[alertController addTextFieldWithConfigurationHandler:^(UITextField *textField){
    textField.placeholder = @"登录";
}];
[alertController addTextFieldWithConfigurationHandler:^(UITextField *textField) {
    textField.placeholder = @"密码";
    textField.secureTextEntry = YES;
}];
{% endhighlight %}

### 上拉菜单

- 当需要给用户展示一系列选择的时候，上拉菜单就能够派上大用场了。和对话框不同，上拉菜单的展示形式和设备大小有关。在iPhone上（紧缩宽度），上拉菜单从屏幕底部升起。在iPad上（常规宽度），上拉菜单以弹出框的形式展现。

{% highlight objc %}
UIAlertController *alertController = [UIAlertController alertControllerWithTitle:@"保存或删除数据" message:@"删除数据将不可恢复" preferredStyle: UIAlertControllerStyleActionSheet];
UIAlertAction *cancelAction = [UIAlertAction actionWithTitle:@"取消" style:UIAlertActionStyleCancel handler:nil];
UIAlertAction *deleteAction = [UIAlertAction actionWithTitle:@"删除" style:UIAlertActionStyleDestructive handler:nil];
UIAlertAction *archiveAction = [UIAlertAction actionWithTitle:@"保存" style:UIAlertActionStyleDefault handler:nil];
[alertController addAction:cancelAction];
[alertController addAction:deleteAction];
[alertController addAction:archiveAction];
{% endhighlight %}