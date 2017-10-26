---
layout:     post
title:      "redis集群报错Node is not empty "
date:       2016-8-08 00:00:00
author:     "wblearn"
header-img: "img/contact-bg.jpg"
tags:
    - redis
    - 集群
    - 
---

<div data-note-content class="show-content">
          <h1>
<a target="_blank"></a>写在前面</h1><p>继上一篇<a href="http://blog.csdn.net/wudalang_gd/article/details/52121204" target="_blank"><u>redis3.0.x集群搭建</u></a>完成之后，当然要用客户端JedisCluster简单测试一下集群啦，这样就要将redis.conf里bind 127.0.0.1改成bind +真机ip(我的192.168.161.131),下面简单地将测试中遇到的问题及解决办法记录在本篇。</p><h1>
<a target="_blank"></a>遇到的问题及解决办法</h1><p>在redis.conf里bind 真机ip后，接着重新执行每个redis.conf，最后再创建集群，但报错，如下图所示： <br></p><div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-b559fbce0ba22d84?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-b559fbce0ba22d84?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div><p></p><br><p> 图中报的错即：</p><p></p><blockquote>
<p>[ERR] Node 192.168.161.131:7000 is not empty. Either the node already knows other nodes (check with CLUSTER NODES) or contains some key in database 0. <br>这就奇怪了，于是我又去检查了一下redis.conf，ip我确实改过来了 <br></p>
<div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-1e29ead16f84f6c3?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-1e29ead16f84f6c3?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div>
<p></p>
</blockquote><p>想了一会发现这三个文件appendonly.aof dump.rdb nodes.conf是之前执行ip127.0.0.1时生成的，在我改为真机ip后在执行并没有生效。 <br></p><div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-59020893d0043ee6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-59020893d0043ee6?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div><p></p><p>这里解释一下<strong>dump.rdb</strong>文件：</p><blockquote><p>dump.rdb是由Redis服务器自动生成的 默认情况下 每隔一段时间redis服务器程序会自动对数据库做一次遍历，把内存快照写在一个叫做“dump.rdb”的文件里，这个持久化机制叫做SNAPSHOT。有了SNAPSHOT后，如果服务器宕机，重新启动redis服务器程序时redis会自动加载dump.rdb，将数据库状态恢复到上一次做SNAPSHOT时的状态。</p></blockquote><p>知道原因后就好办了，<strong>解决办法：</strong></p><blockquote><p>1)将每个节点下aof、rdb、nodes.conf本地备份文件删除； <br>2)172.168.63.201:7001&gt;  flushdb      #清空当前数据库(可省略) <br>3)之后再执行脚本，成功执行；</p></blockquote><p>问题解决了之后就可以成功从java客户端测试了： <br></p><div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-0d912b98f43d76ca?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-0d912b98f43d76ca?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div><p></p><br><p>ps:</p><p>这里大家不要这样测试，可以将其写在配置文件里，我这里是为了方便。</p><p></p><h1>
<a target="_blank"></a>写在最后</h1><p>其实平时在测试中遇到的很多问题，都可以在网上找到答案，这里只是简单的记录其中的一个。关于关于redis集群的介绍，了解请看 <a href="http://www.redis.cn/topics/cluster-tutorial.html" target="_blank"><u>redis中文介绍</u></a></p>
        </div>