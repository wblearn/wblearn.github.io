---
layout:     post
title:      "我的Web开发实战总结(二)"
date:       2017-2-26 00:00:00
author:     "wblearn"
header-img: "img/post-bg-2015.jpg"
tags:
    - Web
    - 
    - 
---



 <div data-note-content class="show-content">
          <h1>写在前面</h1>
<p>这篇是继<a href="http://www.jianshu.com/p/185fd6942e7b" target="_blank">我的Web开发实战总结(一)</a>的第二篇文章，在此篇里，我主要总结一下如何把Web页面上的报表或列表数据转换成pdf文件下载到本地。其中涉及到的知识我也会提出来供大家交流学习。ok，开始吧~</p>
<h1>先来看看效果</h1>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2556999-d4f04ff0f1b3e564.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-d4f04ff0f1b3e564.png?imageMogr2/auto-orient/strip%7CimageView2/2"><br><div class="image-caption">Web页面上的列表数据</div>
</div>
<p>上图就是Web页面上的列表数据，将其右侧生成pdf之后的效果如下：</p>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2556999-566937ade0ec943e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-566937ade0ec943e.png?imageMogr2/auto-orient/strip%7CimageView2/2"><br><div class="image-caption">生成的pdf文件</div>
</div>
<h1>实现思路</h1>
<p>这里我提出两种实现思路：</p>
<blockquote><p>1.利用Jacob将EXCEL转成PDF<br>2.利用iText将HTML 转为 PDF</p></blockquote>
<p><strong>1.利用Jacob将EXCEL转成PDF</strong><br>一开始我用的这种思路，主要是因为有生成EXCEL的功能了，想着只要利用jacob再讲EXCEL转成PDF即可，但是后来放弃了。虽然jacob可以生成pdf，word，excel等，但经过本人的实操，问题多多，还要放dll文件到bin目录下。</p>
<p><strong>首先</strong>，Dispatch.call(sheet,"Activate");指定活动sheet，这个是没有问题，设置哪一个就打哪一个，但是也只是当前的一个，其他的没有显示，对于有多个SHEET的EXCEL 怎么能这一次全部转到一个PDF上？实现是可以实现：遍历sheet保存多个pdf文件，通过itextpdf再将这多个PDF合成一个，不过效率偏低。</p>
<p><strong>其次</strong>，jacob是对EXCEL中的每个单元格操作的，像上面的PDF中有图片读取很不方便，就算能打出图片也可能会很模糊，而且复杂的EXCEL更是无能为力。所以我建议大家使用第二种利用iText将HTML 转为 PDF，我也是用的第二种思路实现的。</p>
<p><strong>2.利用iText将HTML 转为 PDF</strong><br>这个思路就是我此篇要重点要讲的，将html转成PDF，首先html有图片，还有各种数据，那么怎么将图片和各种数据填充到html里面呢？这个问题非常好，有童鞋会说，将他们追加拼接到html里，我只想说：大兄弟，别呀，这样太蠢了。这里我们可以利用 freemarker，<strong>首先创建一个FreeMarker模板文件（*.ftl），在这个文件中加入FreeMarker表达式，这些表达式就好比jsp中的jstl标签一样，我们在程序中将数据传递给此文件中即可，在客户端显示时会被真实的数据替换。</strong>下面开始实现过程了。</p>
<h1>利用iText将HTML 转为 PDF</h1>
<p><strong>1.准备好生成pdf所需的jar包</strong></p>
<ul>
<li>CORE 包：主要是itext相关的一些核心itext.jar</li>
<li>
<p>XML  包：xmlworker是一个基于iText的xml生成pdf工具</p>
</li>
<li>
<p>freemarker包：将模板转换成html的jar包(此jar包也能将模板转换成excel，word等)</p>
</li>
</ul>
<p>这里我将它们打包免费分享出来，下载地址：<a href="http://download.csdn.net/detail/wudalang_gd/9764273" target="_blank">itext生成pdf所需的jar包</a></p>
<p><strong>2.创建ftl模板文件</strong></p>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2556999-35a459345a7a545c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-35a459345a7a545c.png?imageMogr2/auto-orient/strip%7CimageView2/2"><br><div class="image-caption"></div>
</div>
<p><em>创建一个FreeMarker模板文件（</em>.ftl），在这个文件中加入FreeMarker表达式，这些表达式就好比jsp中的jstl标签一样，我们在程序中将数据传递给此文件中即可，在客户端显示时会被真实的数据替换。说白了，ftl模板文件就是在html里加入了FreeMarker表达式，所以里面的内容基本跟html一样，我们可以先创建html文件，修改完成后再将文件后缀改成.ftl即可。*本文.ftl模板如下：</p>
<blockquote><p>arDraftBillPreview.ftl</p></blockquote>
<pre><code>&lt;!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"&gt;
&lt;html&gt;
  &lt;head&gt;
    &lt;#--ftl模板--&gt;
    &lt;meta http-equiv="Content-Type" content="text/html; charset=UTF-8" /&gt;
  &lt;/head&gt;
  &lt;#--一定要特别注意字体，很多字体部分中文不支持--&gt;
  &lt;body screen_capture_injected="true" ryt11773="1"&gt;
     &lt;div id="header"&gt;
        &lt;div&gt;![](${imgPath})&lt;/div&gt;
        &lt;div id="text" sytle="font-size:30px;width:445px;height:45px;margin:0px auto;border:0px solid #ccc;"&gt;&lt;p&gt;STATEMENT OF ACCOUNT(Draft)&lt;/p&gt;&lt;/div&gt;
    &lt;/div&gt;

    &lt;div&gt;
        &lt;table style="width: 90%;margin:0px auto;font-size:11px;"&gt;
        &lt;tr&gt;
            &lt;td&gt;&lt;font style="font-weight:bold"&gt;Customer:&lt;/font&gt;&lt;span style="font-family:宋体"&gt;${data.name}&lt;/span&gt;&lt;/td&gt;
            &lt;td&gt;&lt;font style="font-weight:bold"&gt;SOA#:&lt;/font&gt;${data.number}&lt;/td&gt;
            &lt;td style="width: 2%;"&gt;&lt;/td&gt;
            &lt;td&gt;&lt;font style="font-weight:bold"&gt;Currency:&lt;/font&gt;${data.bill}&lt;/td&gt; 
        &lt;/tr&gt;
        &lt;tr&gt;
            &lt;td&gt;&lt;font style="font-weight:bold"&gt;Issud by:&lt;/font&gt;UNI-TOP ALRLINES CO.,LTD&lt;/td&gt;
            &lt;td&gt;&lt;font style="font-weight:bold"&gt;Period:&lt;/font&gt;${data.stDate} ~ ${data.edDate}&lt;/td&gt;
        &lt;/tr&gt;
        &lt;/table&gt;
    &lt;/div&gt;
    &lt;div&gt;
     &lt;table border="0" style="font-size:10px;text-align:center;border-collapse:collapse;"&gt;
        &lt;tr style="background-color:#FDCDCB"&gt;
               &lt;td style="border:1px solid #ccc;"&gt;NO&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Flight Date&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Flight No&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Prefi x No&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;AWB NO&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Origin&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;VIA&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Dest&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Gross Weight(KG)&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Chargeable Weight(KG)&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Unit Price&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Air Freight Charge&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Transfer Charge&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Other Charge&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Total&lt;/td&gt;
               &lt;td style="border:1px solid #ccc;"&gt;Other Charge Remark&lt;/td&gt;
        &lt;/tr&gt;
        &lt;tr style="background-color:#FDCDCB"&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;序号&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;航班日期&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;航班号&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;货单前缀&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;货单号&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;货源地&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;经停站&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;目的地&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;始发地(重)&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;结算重量&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;单价&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;空运费&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;国外转运费&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;其他杂费&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;共计&lt;/td&gt;
            &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;备注&lt;/td&gt;
        &lt;/tr&gt;

    &lt;#list dataList as bill&gt;  
          &lt;tr&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.no}&lt;/td&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.flightDate}&lt;/td&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.flightNo}&lt;/td&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.prefixNo}&lt;/td&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.aWBNO}&lt;/td&gt; 
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.origin}&lt;/td&gt; 
                    &lt;td style="border:1px solid #ccc;"&gt;&lt;/td&gt; 
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.dest}&lt;/td&gt; 
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.grossWeight}&lt;/td&gt; 
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.chargeableWeight}&lt;/td&gt; 
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.unit}&lt;/td&gt; 
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.airFreightCharge}&lt;/td&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.transferCharge}&lt;/td&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.otherCharge}&lt;/td&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;${bill.total}&lt;/td&gt;  
                    &lt;td style="font-family:宋体;border:1px solid #ccc;"&gt;${bill.remark!}&lt;/td&gt;  
           &lt;/tr&gt;
    &lt;/#list&gt;
    &lt;#if total??&gt;  
           &lt;tr&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;&lt;/td&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;&lt;/td&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;&lt;/td&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;&lt;/td&gt;  
                    &lt;td style="border:1px solid #ccc;"&gt;&lt;/td&gt; 
                    &lt;td style="border:1px solid #ccc;"&gt;&lt;/td&gt; 
                    &lt;td style="border:1px solid #ccc;"&gt;&lt;/td&gt; 
                    &lt;td style="font-family:宋体;color:red;border: 1px solid #ccc;"&gt;总计：&lt;/td&gt; 
                    &lt;td style="color:red;border:1px solid #ccc;"&gt;${total.grossWeight}&lt;/td&gt; 
                    &lt;td style="color:red;border:1px solid #ccc;"&gt;${total.chargeableWeight}&lt;/td&gt; 
                    &lt;td style="color:red;border:1px solid #ccc;"&gt;&lt;/td&gt; 
                    &lt;td style="color:red;border:1px solid #ccc;"&gt;${total.airFreightCharge}&lt;/td&gt;  
                    &lt;td style="color:red;border:1px solid #ccc;"&gt;${total.transferCharge}&lt;/td&gt;  
                    &lt;td style="color:red;border:1px solid #ccc;"&gt;${total.otherCharge}&lt;/td&gt;  
                    &lt;td style="color:red;border:1px solid #ccc;"&gt;${total.total}&lt;/td&gt;  
                    &lt;td style="color:red;border:1px solid #ccc;"&gt;&lt;/td&gt;  
            &lt;/tr&gt;
    &lt;/#if&gt;  
        &lt;/table&gt;
        &lt;/div&gt;
  &lt;/body&gt;
&lt;/html&gt;</code></pre>
<p><em>以上代码在myeclipse中预览的效果如下：</em></p>
<div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2556999-c6416e4d6ef17365.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-c6416e4d6ef17365.png?imageMogr2/auto-orient/strip%7CimageView2/2"><br><div class="image-caption"></div>
</div>
<p><strong>注意：如果使用不存在的freemarker指令，FreeMarker不会使用模板输出，而是产生一个错误消息。其次，在写ftl模板的时候，因为xmlworker支持的CSS样式极少，所以模板内容要尽量简单。对于DOCTYPE和html标签的约束页比较严格。对于一个标签中含有中文、数字或英文的时候，很可能会出现问题。这是因为xmlworker在渲染PDF的时候是以html的标签为单位的。我发现有些字体下部分中文生成pdf不会显示。</strong>另外，对于freemarker模板语言不熟悉的童鞋，我会在文末贴出一些参考资料。</p>
<p><strong>3.向ftl模板文件中填充数据，同时将其生成html</strong><br>在业务处理层，将数据传递个ftl ，同时解析ftl模板生成html</p>
<pre><code>//将需要在客户端浏览器中显示的业务数据放在一个map中，传递给FreeMarker   
                Map&lt;String, Object&gt; map = new HashMap&lt;String, Object&gt;();  
                map.put("imgPath", imgPath);
                map.put("data",data);
                map.put("dataList", dataList);  
                map.put("total", total);  
                TemplateParseUtil.parse(templatePath, "arDraftBillPreview.ftl", htmlPath, map);</code></pre>
<p>解析ftl模板生成html(此方法与生成Excel,xml等通用)，这里我写了一个工具类<br><em>TemplateParseUtil.java</em></p>
<pre><code>public class TemplateParseUtil {
    /**
     * 解析模板生成html(此方法与生成Excel,xml等通用)
     * @param templateDir  ftl模板目录
     * @param templateName ftl模板名称
     * @param htmlPath 生成的html文件路径
     * @param data 数据参数
     * @throws IOException
     * @throws TemplateException
     */
    public static void parse(String templateDir,String templateName,String htmlPath,Map&lt;String,Object&gt; data) throws IOException, TemplateException {
        //初始化工作
        Configuration cfg = new Configuration();
        //设置默认编码格式为UTF-8
        cfg.setDefaultEncoding("UTF-8"); 
        //全局数字格式
        cfg.setNumberFormat("0.00");
        //设置模板文件位置
        cfg.setDirectoryForTemplateLoading(new File(templateDir));
        cfg.setObjectWrapper(new DefaultObjectWrapper());
        //加载模板
        Template template = cfg.getTemplate(templateName,"utf-8");
        OutputStreamWriter writer = null;
        try{
            //填充数据至html
            writer = new OutputStreamWriter(new FileOutputStream(htmlPath),"UTF-8");
            template.process(data, writer);
            writer.flush();
        }finally{
            writer.close();
        }    
    }
}</code></pre>
<p><strong>4.利用itext将生成的html渲染生成PDF</strong></p>
<p>步骤基本如下：</p>
<pre><code> // 1.新建document对象
        Document document = new Document();
// 2.建立一个书写器(Writer)与document对象关联，通过书写器(Writer)可以将文档写入到磁盘中。
// 创建 PdfWriter 对象 第一个参数是对文档对象的引用，第二个参数是文件的实际名称，在该名称中还会给出其输出路径。
        PdfWriter writer = PdfWriter.getInstance(document, new       FileOutputStream("D:/test.pdf"));
// 3.打开文档
        document.open();
 // 4.添加一个内容段落
      document.add(new Paragraph("Hello World!"));
 // 5.关闭文档
     document.close();</code></pre>
<p>本文中的利用itext生成PDF的代码如下：</p>
<pre><code>                Document document = new Document(PageSize.A4, 10, 50, 10, 50);
                // step 2
                PdfWriter writer = PdfWriter.getInstance(document, new FileOutputStream(servletPath+DEST));
                // step 3
                document.open();
                // step 4
                XMLWorkerHelper.getInstance().parseXHtml(writer, document,
                        new FileInputStream(htmlPath), Charset.forName("UTF-8"));
                // step 5
                document.close();</code></pre>
<p><strong>5.完整处理逻辑</strong><br>可能写到这里代码有点分散，这里将上面3、4步骤的代码完整逻辑贴出来，让大家看的清晰明白点：</p>
<pre><code>/**
     * 
     * 草稿账单pdf导出
     */
    @RequestMapping("/exportPDFDraftPreview")
    public @ResponseBody ControllerResult&lt;?&gt; exportPDFDraftPreview(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ExecuteController e = new ExecuteControllerHandle() {
            @Override
            public ControllerResult&lt;?&gt; dowith(HttpServletRequest request,HttpServletResponse response, Object... params) throws Exception {
                /**
                 * 具体导出步骤：先查询出具体的表头信息，然后再查询出具体的列表数据。最后整合放在具体的类里
                 */
                // 请求参数绑定
                DraftBillResponse info = ControllerUtils.bindParams(request,DraftBillResponse.class);//返回一个草稿账单号
                info.setMoneytype(info.get币种());
                DraftBill topInfo = new DraftBill();
                topInfo.set币种(info.get币种());
                topInfo.set草稿账单编号(info.get草稿账单编号());
                // 业务层调用
                IFinancialManagementMgr mgr = FinancialManagementFactory.createIFinancialManagementMgrMgrImpl();

                List&lt;DraftBill&gt; bill =  mgr.queryManuscriptPreview(topInfo);//查询表头

                List&lt;DraftBillResponse&gt; list = mgr.queryDraftPreview(info); //查询列表数据


                String servletPath = request.getSession().getServletContext().getRealPath("/");//请求的服务器
                String templatePath = servletPath+"template";//模板目录
                String htmlPath = servletPath+"tempFile\\"+info.get草稿账单编号()+".html";//生成的html地址
                String imgPath = servletPath+"images\\exlTop.png";//模板中的图片
                String DEST = "tempFile\\"+info.get草稿账单编号()+".pdf";//将要生成的pdf


                /*DraftBill data = new DraftBill();
                data = bill.get(0);*/
                //进行英文转换，防止无法识别

                List&lt;BillList&gt;  dataList = new ArrayList&lt;BillList&gt;();
                BillList listData = null;
                BillList total = new BillList();
                BillExport data = new BillExport(bill.get(0).get代理人(),bill.get(0).get草稿账单编号(),bill.get(0).get币种(),bill.get(0).get账单周期起(),bill.get(0).get账单周期止());

                double num1 = 0,num2 = 0,num3 = 0,num4 = 0,num5 = 0,num6 = 0;  //地重，始发重量，空运费，转运费，杂费，共计


                for(DraftBillResponse res : list){//集合

                  double 地重 =     Double.valueOf(res.get始发地重量());  //设置统计数据
                  double 始发重 =     Double.valueOf(res.get结算重量());
                  double 空运费 =     Double.valueOf(res.get运费());
                  double 转运费 =     Double.valueOf(res.get国外联运费());
                  double 杂费 =     Double.valueOf(res.get杂费());
                  double 统计 =     Double.valueOf(res.get运费())+ Double.valueOf(res.get国外联运费())+Double.valueOf(res.get杂费());
                  num1 = num1 + 地重;
                  num2 = num2 + 始发重;
                  num3 = num3 + 空运费;
                  num4 = num4 + 转运费;
                  num5 = num5 + 杂费;
                  num6 = num6 + 统计;
                    listData = new BillList(res.get序号(), res.get航班日期(), res.get航班号(), res.get货单前缀(),  //设置列表数据
                            res.get货单号(), res.get货源地(), "", res.get目的站(), res.get始发地重量(),
                            res.get结算重量(), res.get空运费费率(), res.get运费(), 
                              res.get国外联运费(),res.get杂费(), String.valueOf(CommonUtil.toDecimalFormat(统计)),
                            res.get备注());

                    dataList.add(listData);
                }

                //将统计的数据放进实体
                total.setGrossWeight(String.valueOf(CommonUtil.toDecimalFormat(num1)));
                total.setChargeableWeight(String.valueOf(CommonUtil.toDecimalFormat(num2)));
                total.setAirFreightCharge(String.valueOf(CommonUtil.toDecimalFormat(num3)));
                total.setTransferCharge(String.valueOf(CommonUtil.toDecimalFormat(num4)));
                total.setOtherCharge(String.valueOf(CommonUtil.toDecimalFormat(num5)));
                total.setTotal(String.valueOf(CommonUtil.toDecimalFormat(num6)));

              //将需要在客户端浏览器中显示的业务数据放在一个map中，传递给FreeMarker   
                Map&lt;String, Object&gt; map = new HashMap&lt;String, Object&gt;();  
                map.put("imgPath", imgPath);
                map.put("data",data);
                map.put("dataList", dataList);  
                map.put("total", total);  
                TemplateParseUtil.parse(templatePath, "arDraftBillPreview.ftl", htmlPath, map);

                Document document = new Document(PageSize.A4, 10, 50, 10, 50);
                // step 2
                PdfWriter writer = PdfWriter.getInstance(document, new FileOutputStream(servletPath+DEST));
                // step 3
                document.open();
                // step 4
                XMLWorkerHelper.getInstance().parseXHtml(writer, document,
                        new FileInputStream(htmlPath), Charset.forName("UTF-8"));
                // step 5
                document.close();

                return ControllerUtils.buildSimpleResult(true, DEST);
            }
        };</code></pre>
<p>在上面的程序中，包括PDF上的图片，表头及表身数据都传给ftl模板中了，在生成PDF之前，都会先生成一个.html的文件到tempFile的文件夹下，如下：<br></p><div class="image-package">
<img src="http://upload-images.jianshu.io/upload_images/2556999-19f0aadaaaa89ebb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" data-original-src="http://upload-images.jianshu.io/upload_images/2556999-19f0aadaaaa89ebb.png?imageMogr2/auto-orient/strip%7CimageView2/2"><br><div class="image-caption"></div>
</div>
<h1>写在最后</h1>
<p>其实整个过程都比较简单，难就难在一开始你不知道用那种方式去实现，这种时候我建议你都试试，毕竟一个东西你试过之后才知道好不好，适不适合。还有一点就是，对于你不知道的东西，网上一般都有很多参考资料，一定要善于利用搜索引擎学习。关于学习，就三点：坚持，坚持，坚持。</p>
<p>下面列出一些相关链接供大家参考：<br><a href="http://piperzero.iteye.com/blog/1660540" target="_blank">iText入门</a><br><a href="http://www.cnblogs.com/Iran1112/p/6013474.html" target="_blank">动态jsp页面转PDF输出到页面</a><br><a href="http://www.open-open.com/lib/view/open1402448471822.html" target="_blank">最简单 iText 的 PDF 生成方案（含中文解决方案）HTML 转为 PDF</a><br><a href="http://carolli.iteye.com/blog/1387838" target="_blank">ftl 入门</a><br><a href="http://lavasoft.blog.51cto.com/62575/716825/" target="_blank">Freemarker 最简单的例子程序</a><br><a href="http://format-me.iteye.com/blog/543905" target="_blank">FreeMarker 例子</a><br><a href="http://blog.csdn.net/u010722643/article/details/41732607" target="_blank">freemarker生成excel、word、html、xml实例教程</a><br><a href="http://lauy.iteye.com/blog/1774880" target="_blank">freemarker判断对象是否为空</a></p>
<p> 阅读我的其他文章</p>
<blockquote><ul>
<li><a href="http://www.jianshu.com/p/292da3de5bcd" target="_blank">1024程序员节，向改变世界的程序员致敬</a></li>
<li><a href="http://www.jianshu.com/p/bee295965800" target="_blank">程序员的你是否熟练掌握Chrome开发者工具？</a></li>
<li><a href="http://www.jianshu.com/p/83afa19c5096" target="_blank">UML学习归纳整理</a></li>
</ul></blockquote>

        </div>
