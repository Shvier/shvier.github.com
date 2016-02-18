---
layout: post
title: "搭建一个属于自己的博客Github+Jekyll(一)Jekyll等准备工作"
date: 2016-01-27 13:43:00
categories: Guide
---

#前言
这几天决心把很久没弄的主页整一下，因为对前端各种css和js毫无感觉，这个过程异常痛苦。其实之前有一个主页，在SAE上，最近发现SAE也开始收费了，又在知乎上看到GitHub有免费的托管空间使用，并且坑爹的答题人说用GitHub Pages建博客特别容易(事实证明也确实容易)，当时信以为真，就入了这个坑，结果谷歌找到的一堆教程使用的jekyll版本不一样导致有些出入(对大牛来说这些都不是问题，但是对小白来说还是很麻烦的)，于是在建好以后决定记录一下搭博客的过程，供跟我一样的小白参考，不正之处欢迎提出(这几天忙完后会开始加评论系统)。
	
	
	
	
#准备工作
在本次操作中需要用到的工具有：ruby、jekyll、python、git



##声明
本机环境为OSX EI Capitan  
Python、git、ruby系统自动集成，需要使用新版本可进行升级



###安装ruby
需要注意的是如果想更新ruby，需要替换源


    $ gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/

添加完成后可以查看一下是否只有taobao镜像    

	$ gem sources -l
	
然后就可以进行安装了

	$ gem install rails

	
	
	
###安装jekyll
	$ gem install jekyll
	
如果提示没有权限，在命令前加上`sudo`




####jekyll简单使用
jekyll安装成功后，可以新建一个jekyll博客，也可以使用现有博客(模板)，我相信看这个的人大多数会选择后者，所以这里只提后者，前者可以参考这篇博客[http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)




#####两条命令
	$ jekyll build  
	
	  
当使用jekyll模板时，不难发现工程目录的结构有一定的规律性  
其中`_config.yml`写入了整个博客的配置；  
`_layouts`存放整个博客页面的文件，有些博客会将页面拆分出来(header、footer)分为多个HTML，存放在`_includes`文件夹里；  
`_posts`则是博客里的文章；  
`_site`是博客系统启动后生成的静态HTML，所以这个文件夹是可以删除的，并且在使用`jekyll build`命令后会再次生成`_site`文件夹。  
  
	
	$ jekyll server
	
	
	  
在下载好别人的jekyll模板后，一定要记得先本地预览看是否能正常使用。  
使用终端cd命令进入模板目录，输入`jekyll server`会自动生成上面提到的`_site`文件夹，同时我们可以在浏览器输入`127.0.0.0:4000`本地预览该模板。  
需要注意的是部分模板由于jekyll版本不一样，会导致`jekyll server`执行时报错，常见的错误有将`pgyments`替换为`highlighter`(pgyments是一个代码高亮的工具)，这是因为在早期的jekyll版本中，启用pgyments的配置命令是`pgyments = true`，而现在的命令写法是`highlighter = pgyments`(至于其他错误，比如pagenite未安装这种，安装一下就好，再其他的我也不知道了囧)




###GitHub Pages的生成在下一节讲到

#参考
[http://www.jianshu.com/p/07064eb79740](http://www.jianshu.com/p/07064eb79740)
