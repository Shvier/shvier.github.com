---
layout: post
title: "搭建一个属于自己的博客Github+Jekyll(二)开始搭建Github"
date: 2016-01-28 17:14:29
categories: Guide
---
#设置GitHub   
##申请账号
直接上GitHub官网申请 [https://github.com/](https://github.com/)  
由于是国外网站，速度慢请耐心等待  
##搭建本地环境
首先安装GitHub客户端，如果是OSX直接启用即可，Windows环境需要下载一个客户端 
##配置ssh
GitHub为了保证本地和远端是同一人操作，采用了RSA数字签名的认证机制，所以为了保证后面的工程不出错，需要配置一下ssh   
首先打开终端      

	$ cd ~/.ssh/   
	
进入保存密钥的文件目录

	$ ls
	
可以使用`rm`命令删除多余的密钥文件，随后输入

	$ ssh-keygen -t rsa -C "email@example.com"
	
这里的邮箱表示GitHub的注册邮箱，随后要求输入密钥保存在哪个文件内，可以根据提示输入`id_rsa`，生成密钥后，还要将其添加到GitHub远端上。所以再次进入密钥目录

	$ cd ~/.ssh/
	$ ls
	$ cat xxx
	
`cat`可以直接查看文件内容(需要注意的是这里查看的是公钥，即`id_rsa.pub`而不是私钥)   
然后将内容拷贝(包括开头的`ssh-rsa`和结尾的`邮箱地址`)   
接着打开网页端GitHub，点击头像选择`Settings`，接着选择`SSH keys`，然后点击`Add SSH key`，将刚刚复制的密钥信息粘贴到`Key`文本框中，`Title`可以不用填，然后点击下方的`Add key`
##初始化GitHub Pages
###分支(repository)
`分支`是GitHub中一个重要的概念，可以把整个GitHub看做一棵树，这棵树的树干叫做`master`，树上的树枝叫做`repository`，一个`repository`可以看做一个工程。    
因此，在这里需要新建一个分支用来存放博客工程，点击`+New repository` ，随后要求输入`Repository name`，`Repository name`可以看做访问的连接，可以随意填写，为了方便最好采用这种形式`XXXX.github.com`。   
public和private不需要理会。private是收费的，既然选择了GitHub，就得要有分享精神嘛;-)，然后点击`Create repository`
##设置Pages
建立好分支后，进入刚刚建立好的分支，点击`Settings`，在这里我们可以看到刚刚设置的分支名，往下拖动，可以看到一项`Launch automatic page generator`，点击，随后点击`Continue to layouts`，选择一个主题(随意选择，因为稍后会用jekyll模板覆盖)，选择好后点击`Publish page`即完成了Pages的初始化。   
等待一段时间后，就可以通过之前创建的链接访问啦。       
###注意
- 如果忘记了创建的访问链接，可以在`Settings`选项里找到
- 如果发现分支仍然是空的，可以尝试重复刚刚的操作，也可以先看下面的(一开始我搭建的时候工程目录也是空的，在克隆工程后无意中执行了`$ git fetch origin`和`$ git checkout gh-pages`这两条指令，发现工程目录正常了，所以可以在后面的步骤中尝试执行这两条命令，如果还是不行的话再重复刚刚的操作)   
 
##建立本地与远端的链接
首先选择一个文件路径作为工程的保存路径   
使用终端`cd`命令进入目录后输入`git init`进行初始化   
接着输入   
	
	$ git clone git@github.com:Shvier/shvier.github.com.git   
	
这里是我的分支路径，选择自己的`SSH`路径就可以将远端分支与本地建立对应关系      
`clone`的作用是将远端的工程全部拷贝到本地
#使用模板
将下载好的jekyll拷贝到`clone`好的工程目录，接着输入

	$ git status
	
`git status`的作用是查看本地和远端的文件有哪些不一样，我觉得这也是git被设计出来的初衷吧(比较各个版本的信息)，在每一次进行`commit`(将本地需要修改的文件先放到缓存池后才可以进行提交)操作之前，都需要把`git status`列举出的冲突解决   
一般来说`delete`冲突使用`git rm *`(*表示文件名)   
`untracked`文件使用`git add *`   
   
###注意
- 可以使用*匹配关键字
- 如果嫌麻烦可以使用`git add *`解决所有冲突，但是对于部分冲突不管用    

解决完所有冲突后可以再次使用`git status`命令，这是发现所有文件变成了绿色即可开始`commit`

	$ git commit
	
这是会进入版本改动日志记录，与vim操作一样，按`i`可以开始编辑，主要记录改动的地方和修复的问题即可，编辑好后按`ESC`会退出`i`(编辑)状态，接着输入`:`，可以看到光标跳刀了最下面，这是进入了命令模式，输入`wq`，`wq`表示保存并退出的意思    
以上工作都准备好后即可将本地文件提交到远端

	$ git push
	
    
###注意
如果一开始不是从远端`clone`下来的，在`push`时会提示需要设置远端目标
   
#结尾
以上操作完成后，稍等片刻即可登入之前设置的GitHub链接访问上传的博客啦。接下来的工作就是自己DIY，把模板慢慢改成自己的口味，记录一些有趣的日志。
   
#最后
如果看着这篇叙述的不太通顺的教程建好了自己的博客，那当然是极好的，如果遇到一些问题也不要轻易放弃，有问题解决了，才是真正的学习。
  
#参考
[http://www.cnblogs.com/fanyong/p/3518176.html](http://www.cnblogs.com/fanyong/p/3518176.html)