---
layout:     post
title:      "push本地代码到github出错"
date:       2017-1-1 00:00:00
author:     "wblearn"
header-img: "img/post-bg-alitrip.jpg"
tags:
    - git
    - 
    - 
---

![](http://img.blog.csdn.net/20170101121417088?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd3VkYWxhbmdfZ2Q=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**有时候往GitHub上提交东西的时候会因为remote repository上有东西更新了但是local repository 没有更新而造成提交失败**


有如下几种解决方法：

**1.使用强制push的方法：**

```
$ git push -u origin master -f 
```

这样会使远程修改丢失，一般是不可取的，尤其是多人协作开发的时候。

**2.push前先将远程repository修改pull下来**

```
$ git pull origin master
```

```
$ git push -u origin master
```

**3.若不想merge远程和本地修改，可以先创建新的分支：**

```
$ git branch [name]
```

然后push

```
$ git push -u origin [name]
```

关于git的更多用法，请看[这里](http://www.ruanyifeng.com/blog/2014/06/git_remote.html)