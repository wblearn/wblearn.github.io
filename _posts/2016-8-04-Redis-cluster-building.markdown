---
layout:     post
title:      "redis3.0.x集群搭建 "
date:       2016-8-04 00:00:00
author:     "wblearn"
header-img: "img/contact-bg.jpg"
tags:
    - redis
    - 集群
    - 
---

  <div data-note-content class="show-content">
          <h1>
<a target="_blank"></a>写在前面</h1><p>2015年2月，Redis3.0.0 发布，redis3.0版本之后支持Cluster，关于redis集群的介绍，了解请看 <a href="http://www.redis.cn/topics/cluster-tutorial.html" target="_blank"><u>redis中文简介</u></a> 。 <br> 我准备在一台linux中来部署redis集群，因为集群的运行需要6台服务才能正常运行，所以我在一台linux服务上创建6个节点，用来模拟3主3从这种伪分布式集群。redis3.0及之后的releases版本，大家可以直接访问redis.io官网，下载redis.tar.gz。</p><h1>
<a target="_blank"></a>集群搭建</h1><h2>
<a target="_blank"></a>1.下载和解包</h2><p>cd /usr/local/  <br> wget <a href="http://download.redis.io/releases/redis-3.0.3.tar.gz" target="_blank"><u>http://download.redis.io/releases/redis-3.0.3.tar.gz</u></a>  <br> (大家可以选择自己想安装的releases版本)</p><p>tar -zxvf redis-3.0.3.tar.gz  <br> mv redis-3.0.3  redis</p><h2>
<a target="_blank"></a>2.编译安装</h2><p>cd redis  <br> make &amp;&amp; make install  <br>有些人在这里可能会碰到一个错误导致编译不过(如下)</p><blockquote><p>make[1]: Entering directory <code>/redis/src' <br>  CC adlist.o <br>  In file included from adlist.c:34: <br>  zmalloc.h:50:31: error: jemalloc/jemalloc.h: No such file or directory <br>  zmalloc.h:55:2: error: #error "Newer version of jemalloc required" <br>  make[1]: *** [adlist.o] Error 1 <br>  make[1]: Leaving directory</code>/redis/src’ <br>make: <em>*</em> [all] Error 2Error 2</p></blockquote><p>原因是没有安装jemalloc内存分配器，可以安装jemalloc 或 直接 输入make MALLOC=libc  &amp;&amp; make install</p><h2>
<a target="_blank"></a>3. 创建redis节点</h2><hr><p>在一台linux服务上创建6个节点，3主3从。 <br>cd /usr/local/</p><p>mkdir redis_cluster   //创建集群目录</p><p>mkdir 7000 7001 7002 7003 7004 7005 <br>  //分别代表六个节点    其对应端口 7000 7001 7002 7003 7004 7005</p><p>下面创建7000节点为例，其他节点大家按此例操作即可， <br>cd ./7000  <br> cp /usr/local/redis/redis.conf   ./    //拷贝到当前7000目录  <br>vi redis.conf    //编辑配置  主要修改一下几个参数</p><blockquote><p>daemonize    yes                          //redis后台运行 <br>pidfile  /var/run/redis_7000.pid    //pidfile文件对应7000 <br>  port  7000                                  //端口7000 <br>  cluster-enabled  yes                    //开启集群   把注释#去掉 <br>cluster-config-file  nodes.conf      //集群的配置  配置文件首次启动自动生成 <br>cluster-node-timeout   5000       //请求超时  设置5秒够了 <br>appendonly   yes                        //aof日志开启   有需要就开启，它会每次写操作都记录一条日志</p></blockquote><p>配置好了，就相应地把这个修改后的配置文件拷贝到7003 7004 70054 7005 7002 目录，注意要修改监听端口port 7001 7002 7003 7004 7005。</p><p>接下来，启动服务，进入节点目录  <br> 依次执行  redis-server   redis.conf  <br>可以看到生成了appendonly.aof  nodes.conf  <br></p><div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-7cdcb85e5976cfe6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-7cdcb85e5976cfe6?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div><p>ps -ef | grep redis 查看是否启动成功  <br></p><div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-4307d819ea1bded0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-4307d819ea1bded0?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div><p><br></p><br><p>netstat -tnlp | grep redis 可以看到redis监听端口</p><br><div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-5510225a4415f34c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-5510225a4415f34c?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div><h2>
<a target="_blank"></a>4. 创建集群</h2><p>前面已经准备好了搭建集群的redis节点，接下来我们要把这些节点都串连起来搭建集群。创建集群需要ruby的环境，官方提供了一个工具：redis-trib.rb  （/usr/local/redis/src/redis-trib.rb），所以我们还得安装ruby.</p><p>安装rubygems组件 <br>yum -y install ruby ruby-devel rubygems rpm-build</p><p>接着，加载redis，需要redis和ruby的接口，使用gem 安装 <br>gem install redis <br>可是，我却出了一点问题，但按提示操作后再执行gem install redis 就好啦(如下图) <br></p><div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-b0e5017ebdb263d5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-b0e5017ebdb263d5?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div><p>上面的步骤完事了，接下来运行一下redis-trib.rb</p><p>/usr/local/redis/src/redis-trib.rb  <br></p><div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-4713e35534e7750b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-4713e35534e7750b?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div><p>确认所有的节点都启动，接下来使用参数create 创建集群</p><p>/usr/local/redis/src/redis-trib.rb  create  –replicas  1   127.0.0.1:7000  127.0.0.1:7001 127.0.0.1:7002 127.0.0.1:7003  127.0.0.1:7004  127.0.0.1:7005 <br></p><div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-186ca78d50448cb6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-186ca78d50448cb6?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div><p><br></p><br><p>解释下， –replicas  1  表示 自动为每一个master节点分配一个slave节点    上面有6个节点，程序会按照一定规则生成 3个master（主）3个slave(从)</p><p>ps:防火墙一定要开放监听的端口 ，否则会创建失败。 <br> 运行中，提示Can I setthe above configuration? (type’yes’to accept): yes    //输入yes  <br></p><div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-004ab72095f499d7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-004ab72095f499d7?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div><p>ok，查看一下 /usr/local/redis/src/redis-trib.rb check 127.0.0.1:7000  <br></p><div class="image-package">
<img alt="这里写图片描述" src="http://upload-images.jianshu.io/upload_images/2556999-b7907a0f696b516d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-b7907a0f696b516d?imageMogr2/auto-orient/strip"><br><div class="image-caption"></div>
</div><p><br></p><br><p>可以从图中看到7000,7002被选为master了</p><p>至此，集群已经初步搭建好了。</p><h2>
<a target="_blank"></a>5. 测试</h2><p>1）get 和 set数据</p><p>redis-cli -c -p 7000</p><p>进入命令窗口，直接 set   wb  wudalang_gd</p><p>直接根据hash匹配切换到相应的slot的节点上。</p><p>还是要说明一下，redis集群有16383个slot组成，通过分片分布到多个节点上，读写都发生在master节点。</p><p>2）宕机测试</p><p>假如我们把上面的master为7000或7002的其中一个节点down掉，大家可以再去看一下各个节点的状态，测试一下，依然没有问题，会有新的从节点变为master，集群依然能继续工作。</p><p>原因：  redis集群  通过选举方式进行容错，保证一台Server挂了还能跑，这个选举是全部集群超过半数以上的Master发现其他Master挂了后，会将其他对应的Slave节点升级成Master.</p><p>PS： 超过半数挂了那救不了了，整个集群就无法工作了。 <br>3）关于一致性 hash <br>目前还没什么公司用redis来做数据库持久化的吧，我们只是拿来做cache，官网说的很清楚，Redis Cluster is not able to guarantee strong consistency .</p><h1>
<a target="_blank"></a>写在最后</h1><p>网上关于redis集群的资料真的是很乱，我很多次走入别人挖得坑半天爬不出来，有的人居然在redis3.0.0还没releases就忽悠人(气哭~)，不过，我相信随着官方的不断迭代更新和大家的共同努力，Redis Cluster一定会逐渐完善成熟的！</p>
        </div>