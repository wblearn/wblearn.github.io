---
layout:     post
title:      "redis应用的总结 "
date:       2016-7-26 17:14:34
author:     "wblearn"
header-img: "img/contact-bg.jpg"
tags:
    - redis
    - 集群
    - 总结
---

## 写在前面

对最近项目应用redis做一个简单总结，项目中的营业网点资料和客户资料等模块以后的资料量势必会随着业务的扩张会越来越大，可能会造成系统性能瓶颈及用户体验不佳等，所以根据老大的建议，把相应模块对应的表名+表关键字作为key，优先从redis缓存中拿数据，减少对数据库CRUD操作避免负载过大。



## 这里，我们会专门写一个接口来实现redis处理的逻辑



查询的逻辑：根据我们生成的key，来判断redis里是否存在这样的key，若存在，直接从redis里面取，不存在，从数据库(ORACLE)里面取。注意：在从redis里取得时候，会做这样一个操作：就是我们定义了一个缓存对象CacheObject，缓存对象有两个属性，一个布尔值用来判断redis是否需要同步更新oracle最新数据,另一个是泛型的数据集合，至于为什么是泛型，是因为这样我们查询哪个模块的数据就返回哪个模块的数据而不必为每个模块重新去定义。当缓存对象里的布尔值为true时，从redis里获得数据后把布尔值从新设置为false，并更新缓存对象到redis里。



这部分的代码如下：

定义的缓存对象：



![img](http://wblearn.github.io/img/in-post/redis/1s.webp)

查询缓存处理：



![img](http://wblearn.github.io/img/in-post/redis/2s.webp)

通用查询接口：



![img](http://wblearn.github.io/img/in-post/redis/3s.webp)

更新的逻辑：

一旦我们修改或者增加资料到数据库(oracle)，我们同时设置缓存对象的布尔值为true，并将其存到redis中，当下次我们查询的时候会根据这个布尔值同步最新数据到redis(见上面的查询逻辑)

更新逻辑代码：





![img](http://wblearn.github.io/img/in-post/redis/4s.webp)



删除逻辑：

删除数据库数据时，同时删除redis中的数据

删除代码：



![img](http://wblearn.github.io/img/in-post/redis/5s.webp)



## 写在最后

以上只是让大家知道redis缓存的处理逻辑，如果大家有更好的意见，欢迎到博客左侧的小窗骚扰我(...呸呸呸...联系我)。