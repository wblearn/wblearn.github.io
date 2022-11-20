---
layout:     post
title:      "我的Web开发实战总结(一)"
date:       2016-11-26 00:00:00
author:     "wblearn"
header-img: "img/post-bg-2015.jpg"
tags:
    - Web
    - 
    - 
---



<div data-note-content class="show-content">
          <h1>写在前面</h1>
<p>最近一直在做项目，感觉没什么分享的所以一直没写。<br>今天不上班，就把最近做的一个demo做个简单的总结。</p>
<h1>截图</h1>
<div class="image-package">
<img src="http://wblearn.github.io/img/in-post/web1/1.webp" data-original-src="http://wblearn.github.io/img/in-post/web1/1.webp"><br><div class="image-caption">上半部分截图</div>
</div>
<h1>快速查询与快递单号</h1>
<blockquote><p>快速查询</p></blockquote>
<p>之前写过的一篇文章<a href="http://www.jianshu.com/p/8f971a78b808" target="_blank">简书搜索自动匹配功能</a>其实就是这个功能，只不过我这里查出来的数据是动态的而已，并且点击可以跳转到不同的模块查看。大家去看那篇文章就可以了，这里不再赘述了。</p>
<blockquote><p>快递单号</p></blockquote>
<p>这个功能其实跟快速查询的功能差不多，无非就是js，css，ajax这些基本的web前端知识，只不过多了一些判断，样式的排版，监听事件而已。物流信息使用li标签实现的，需要注意的一点是：物流信息左侧的线条是要计算的，每个运单号物流信息量是不同的，不然线条不完整了，因为每条物流信息都是追加上去的，所以可以这样计算：</p>
<blockquote><p>var h = $("ul li:first-child").height();//第一个li的高度<br>$(".line").css("top",h/2);<br>$(".line").height($("ul").height()-h);</p></blockquote>
<p>大概实现就是下图这个样子：</p>
<div class="image-package">
<img src="http://wblearn.github.io/img/in-post/web1/2.webp" data-original-src="http://wblearn.github.io/img/in-post/web1/2.webp"><br><div class="image-caption"></div>
</div>
<h1>我的待办</h1>
<p>我的待办也是通过ajax获取数据，用li标签显示，有具体数字表示待办事件的数量，数字为红色，点击进入到具体的事项处理界面，显示具体数据（数据已经自动查询加载），”0“表示无待办事件，数字为黑色，点击进入相关界面，无数据显示；每处理一条数据减1，每新增一条数据加1。然后就是加了权限控制，不同的人看到的待办数量自然不同。</p>
<div class="image-package">
<img src="http://wblearn.github.io/img/in-post/web1/3.webp" data-original-src="http://wblearn.github.io/img/in-post/web1/3.webp"><br><div class="image-caption"></div>
</div>
<h1>事物办理与信息查询</h1>
<p>刚开始以为里面的都是写死的，用a标签写几个菜单就完事了。做完之后，需求改成：菜单要从第三方库获取，于是傻不拉几的写了个SDK，只有坐等数据再改了。。。。而且做完之后发现li标签在不同设备适配有问题，于是索性改成了表格。</p>
<h1>公告通知</h1>
<p>公告通知，顾名思义即通知信息的传达处理。目的是为了让用户获得需要得到的消息及提醒并进行处理。实现的功能基本就两个：</p>
<blockquote><p>未查看的公告，能够提示“new”图标，查看过一次之后“new”图标消失。<br>点击每条公告通知，可进入该报告的“公告通知查询（管理人员））”界面，查看详情。</p></blockquote>
<div class="image-package">
<img src="http://wblearn.github.io/img/in-post/web1/4.webp" data-original-src="http://wblearn.github.io/img/in-post/web1/4.webp"><br><div class="image-caption"></div>
</div>
<p>关于web网站系统通知设计更多详细的说明，<a href="http://www.jianshu.com/p/a89b61b6944d" target="_blank">请戳这里。</a></p>
<h1>销售业绩与新客户业绩</h1>
<p>这是用iframe从第三方引入进来的数据曲线图，报表。我并没有做什么特别的工作。唯一一点就是先通过ajax在后台获取第三方库的账号和密码，然后在请求的时候传过去就可以获取页面了。</p>
<h1>工作看板</h1>
<p>刚开始在网上找了一个只是查看日期的简单日历，之后用着才发现里面到处都是bug，于是改啊改啊，改的过程中真有点恶心到我了，改好了这里，那里又出问题了。改好了之后，要把他变成类似那种schedule日历的形式。就是添加几个功能：</p>
<blockquote><p>日历上加个添加功能，点击”添加“，弹出添加任务计划的窗口；<br>点击各天，在下方显示当天最早的三条需要处理的计划；当天的计划提前30分钟提醒，点击“查看详情”，显示计划的详情界面，点击“知道了”，关闭弹出框，本条计划提醒消失，后续计划前移。</p></blockquote>
<p>在页面加载的时候把后台需要处理的计划全部都查出来并初始化日历，让有任务的计划日期追加不同样式，比如颜色变灰，以便用户知道并可以点击查看任务详情。完成的效果如下：<br></p><div class="image-package">
<img src="http://wblearn.github.io/img/in-post/web1/5.webp" data-original-src="http://wblearn.github.io/img/in-post/web1/5.webp"><br><div class="image-caption"></div>
</div>
<h1>排行榜</h1>
<p>实现的功能主要有两个:</p>
<blockquote><p>上月排行：点击”上月排行“，显示上个月的相关排行榜（当前表格刷新）；<br>下月排行：点击“下月排行”，显示下个月的相关排行（当前表格刷新）;</p></blockquote>
<p>排行榜的数据也是从第三方库获取的，于是在第三方库写好接口后,利用sdk获取数据显示。效果如下：</p>
<div class="image-package">
<img src="http://wblearn.github.io/img/in-post/web1/6.webp" data-original-src="http://wblearn.github.io/img/in-post/web1/6.webp"><br><div class="image-caption"></div>
</div>
<p>ps：因为数据库没加图片，测试数据不够完整，所以图片啥的没有出来，而且sql也没有去重。后续再弄了。</p>
<div class="image-package">
<img src="http://wblearn.github.io/img/in-post/web1/7.webp" data-original-src="http://wblearn.github.io/img/in-post/web1/7.webp"><br><div class="image-caption">改版后</div>
</div>
<div class="image-package">
<img src="http://wblearn.github.io/img/in-post/web1/8.webp" data-original-src="http://wblearn.github.io/img/in-post/web1/8.webp"><br><div class="image-caption">改版后</div>
</div>
<h1>写在最后</h1>
<p>还有一些就不一一写了，总之，在做的过程中发现自己的前端有点薄弱，有待提高。whatever，<strong>勇敢的去尝试，从失败中去学习。人都是做自己原本不能胜任的事情中，才能快速成长。</strong></p>

        </div>
