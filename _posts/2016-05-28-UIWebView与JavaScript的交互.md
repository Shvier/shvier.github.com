---
layout: post
title: "UIWebView与JavaScript的交互"
date: 2016-05-28 08:42:42
categories: Objective-C
---

## 引言

最近学习到了这方面的知识，花了几天时间完成了一个小需求，记录一下。   

- 首先要说的是iOS 8.0以后推出新的Web框架WKWebView，与之前的UIWebView相比优化了内存和效率，如果iOS 8.0以后的项目强烈推荐使用，至于为什么这里没有使用WKWebView，那是因为...在我写完本文中的需求后才看到WKWebView，当时真是...(╯‵□′)╯︵┻━┻
- 其次是关于UIWebView与JavaScript之间的通信，系统原声就是支持的。在GitHub上有一个开源框架[WebViewJavaScriptBridge](https://github.com/marcuswestin/WebViewJavascriptBridge)对此进行了封装，在这里我没有用到，因为觉得用不习惯，最近该开源项目也添加了对WKWebView的支持。

## UIWebView中的方法

在进行Objective-C和JavaScript通信时主要依靠UIWebView的一个方法`stringByEvaluatingJavaScriptFromString`。该方法需要传入一个JavaScript脚本作为参数，即可在调用该方法的UIWebView上执行该脚本。

#### 需求功能

在这里，需要给UIWebView上的图片添加一个点击事件，当点击该图片以后就可以使用轮播图跟查看本地文件一样来查看网页上的图片。

#### 分析

针对这个需求，需要先使用一段JavaScript去抓取网页上所有需要添加点击事件的图片，接着根据对应的图片添加正确的点击事件即可。所以第一步首先需要抓取所有图片。

* 抓取图片

```JavaScript
var allImage = document.querySelectorAll("img");
allImage = Array.prototype.slice.call(allImage, 0);
var imageUrlsArray = new Array();
allImage.forEach(function(image) {var esrc = 
	image.getAttribute("data-original");
	if (esrc != null) { 
		imageUrlsArray.push(esrc.replace(/^(https|http|\/\/)/,'svheader'));
		}
	});
	document.location=imageUrlsArray.toString();
```

实现的思路非常简单，首先通过`img`标签抓取所有的图片并存放在数组中，然后对该数组进行遍历，将图片对应的url添加数组中，然后通过`document.location`方法返回。   
需要说明的是这里用了一个很巧妙的方式(要感谢[@Kitten](http://kittenyang.com/webview-javascript-bridge/)提供的该思路)，熟悉前端的同学应该知道`document.location`其实是一个网页跳转的方法，比如返回`www.google.com`这个方法，UIWebView即会开始load这个链接，也就是会执行UIWebView的`shouldStartLoadWithRequest`这个代理方法。但是，在load这个request之前，可以看到我们使用一个正则表达式`/^(https|http|\/\/`将http/https协议头替换成了一个自定义的头`svheader`，这样做的目的是为了不让网页自动进行跳转，而拼接在`svheader`头之后的链接数组，即是我们需要的url。   

* 添加事件

拿到url之后，接下来的事情就很简单了，我们只需要再次遍历url数组，然后为每个`img`元素添加一个lisenter事件，具体如下：

```JavaScript
var allImage = document.querySelectorAll("img");
allImage = Array.prototype.slice.call(allImage, 0);
allImage.forEach(function(image) { 
	if (image.getAttribute("src") == "%@" || image.getAttribute("data-original") == "%@" {
		image.addEventListener('click',function() { 
		document.location="%@://openImageAtIndex%ld";
		});
	}
});
``` 

这里依然用到的是上面的实现思路使用`document.location`重定位一个url，然后根据url拿到协议头后传递过来的消息。

## 结语

一开始以为混合编程是一件比较复杂的事情，在进行了这次的学习后感觉还是可以接受的。不过对于一些复杂的交互还需要深入去研究，并且WKWebView要开始了解和学习。   

[GitHubDemo](https://github.com/Shvier/UIWebView-JavaScript-)   

### 参考链接

[http://liuyanwei.jumppo.com/2015/10/17/ios-webView.html](http://liuyanwei.jumppo.com/2015/10/17/ios-webView.html)





