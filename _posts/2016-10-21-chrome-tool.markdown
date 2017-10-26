---
layout:     post
title:      "程序员的你是否熟练掌握Chrome开发者工具？"
date:       2016-10-21 00:00:00
author:     "wblearn"
header-img: "img/home-bg-o.jpg"
tags:
    - 程序员
    - chrome
    - 
---

 <div data-note-content class="show-content">
          <div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2556999-252611b15bc8f6ee.gif?imageMogr2/auto-orient/strip" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-252611b15bc8f6ee.gif?imageMogr2/auto-orient/strip"><br><div class="image-caption">提前祝各位程序员节日快乐</div>
</div>
<h1>写在前面</h1>
<p><br><strong>再过几天就是1024程序员节日了，这里提前祝各位程序员同胞们节日快乐哈^_^</strong></p>
<p>回归正题，本文主要是介绍一下Chrome developer tool(开发者工具)的使用，以方便我们的日常开发与调试。其实在没用Chrome开发之前就时不时的听到类似这样的话：“别用IE，IE太low了，用Chrome吧”。如今,我用过Chrome后才切身体会到，Chrome浏览器无疑是最受前端青睐的工具，原因除了界面简洁、大量的应用插件，良好的代码规范支持、强大的V8解释器，javascript执行速度和内存占有率表现非常优秀之外，还因为Chrome开发者工具提供了大量的便捷功能，方便我们前端调试代码，我们在日常开发中是越来越离不开Chrome，<strong>是否熟练掌握Chrome调试技巧恐怕也会成为考量前端技术水平的标杆。</strong></p>
<p>对于前端技术的学习或者开发调试，浏览器developer tool的使用是必不可少的，下面，介绍Chrome开发者工具。</p>
<h1>Chrome 开发者工具介绍及调试、使用技巧</h1>
<h5>1、先打开谷歌浏览器，然后打开调试界面，打开方式有三种 ：<br> 第一：<strong>按F12</strong>，<br> 第二：s<strong>hift+ctrl+i</strong>，<br> 第三：鼠标右键点<strong>审查元素</strong>
</h5><br><h5>2、请看下边的标记</h5>

<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2556999-741a42939ffc9b91.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-741a42939ffc9b91.jpg?imageMogr2/auto-orient/strip%7CimageView2/2"><br><div class="image-caption">Chrome开发者工具分为 8 个大模块</div>
</div>
<blockquote><p>Element 标签页： 用于查看和编辑当前页面中的 HTML 和 CSS 元素。<br>Console 标签页：用于显示脚本中所输出的调试信息，或运行测试脚本等。<br>Source 标签页：用于查看和调试当前页面所加载的脚本的源文件，可以打断点进行调试。<br>Network 标签页：用于查看 HTTP 请求的详细信息，如请求头、响应头及返回内容等。<br>TimeLine 标签页： 用于查看脚本的执行时间、页面元素渲染时间等信息。<br>Profiles 标签页：用于查看 CPU 执行时间与内存占用等信息。<br>Resource 标签页：用于查看当前页面所请求的资源文件，如 HTML，CSS 样式文件，图片等。<br>Audits 标签页：用于优化前端页面，加速网页加载速度等。</p></blockquote>
<p></p><h5>3、使用 Chrome 开发者工具调试</h5>
<p></p><h6>&lt;1&gt;设置(条件)断点</h6><br><strong>与 Java 调试类似，Chrome 开发者工具提供了断点设置、删除与断点存储等基本功能。</strong>同时，开发者工具也提供了设置条件断点的功能，使开发者可以控制该断点只有在满足某一条件时才会被触发。当然，也可以直接单纯地设置非条件断点。<br>在Source标签元素面板中对应的JS文件中的行号处点击右键，选择添加条件断点后，会弹出一个对话框用于输入具体的条件或者没有条件断点。<strong>合理运用好条件断点能够提高调试的效率与准确性，使开发人员能更专注于在期望的场景下进行调试。</strong><br>还有一点就是<strong>可以在Source标签元素面板中查看元素属性</strong>，比如通过ajax返回的数据对象封装到data中，我们设置断点后直接将鼠标放到数据data中可以看到其中返回的是什么样的数据，比如data中是实体对象的每个属性字段值。<br>如图 Source标签元素面板中添加条件断点或断点
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2556999-a4ebcbd1d82f0899.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-a4ebcbd1d82f0899.jpg?imageMogr2/auto-orient/strip%7CimageView2/2"><br><div class="image-caption">设置条件断点或断点</div>
</div>
<p></p><h6>&lt;2&gt;Element 标签页对 CSS 的控制</h6><br>在网页开发过程中，经常需要在脚本中控制不同条件下页面的样式展示，例如<strong>页面中的标签颜色，位置，大小等等，在 Chrome 开发者工具的 Element 标签页中，其实已经提供了包括该功能在内的一系列对样式进行实时修改的功能，并且在修改之后能够立即从页面中看到变化。</strong>如图
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2556999-f539061cffac2670.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-f539061cffac2670.jpg?imageMogr2/auto-orient/strip%7CimageView2/2"><br><div class="image-caption">Element 标签页对 CSS 的控制</div>
</div>
<p></p><h6>&lt;3&gt;修改 JavaScript 文件中的代码</h6><br>这是 Chrome 开发者工具提供的一种非常实用的功能，即使<strong>开发人员可直接对开发者工具的 Source 标签页中的代码进行修改，并将其保存，使浏览器在下次执行该段脚本时，直接加载最新修改的版本。</strong>目前的 Firebug 及 IE 自带的开发者工具都不支持对脚本的直接修改，导致在 Firefox 或 IE 中调试脚本时，如果需要对代码进行修改，需要先去修改脚本源文件，再同步至应用服务器，再清理浏览器缓存，最终再次打开应用程序时，才会看到代码修改后的效果。可见 Chrome 开发者工具提供的这一功能，大大提供了开发者调试脚本的效果。<br>需要注意的是，由于这种修改是保存在浏览器缓存中，因此它不会影响到脚本的源文件。<strong>当开发人员决定采用修改之后的脚本时，需要将其复制到脚本的源文件中。</strong>
<p></p><h6>&lt;4&gt;使用控制台打印变量值或方法的返回结果</h6><br>当断点被触发进入到调试模式时，我们可以将当前任意存在的变量或方法输入到控制台中，按下回车后，控制台便会返回相关的结果。该功能可使开发人员方便了解程序运行至断点处时各个所需要变量或方法的返回值。<br>需要注意的是，<strong>当在控制台中输入的方法名字不带括号时，控制台输出的是该方法所包含的代码信息，而并不是运行结果。</strong>
<h1>写在最后</h1>
<p>我们借助 Chrome 开发者工具的支持，可以提高网页应用程序开发与调试的效率。想了解更多，请参考资料<a href="https://developers.google.com/chrome-developer-tools/docs/scripts" target="_blank">Chrome Developer Tools 官方文档</a></p>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2556999-4afbab8a497115e8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-4afbab8a497115e8.png?imageMogr2/auto-orient/strip%7CimageView2/2"><br><div class="image-caption">明天周六啦，再次提前祝各位猿猿们节日快乐</div>
</div>

        </div>