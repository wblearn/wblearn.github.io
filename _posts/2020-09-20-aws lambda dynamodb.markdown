---
layout:     post
title:      "AWS lambda and dynamodb with Java"
date:       2020-09-20 00:00:00
author:     "wblearn"
header-img: "img/post-bg-alitrip.jpg"
tags:
    - aws
    - lambda
    - dynamodb
---

<div data-note-content class="show-content">
          <h1>写在前面</h1>
<p>使用aws lambda已经一年多了，下面使用java构建一个简单的lambda服务，大家可以自己扩展想要的功能，废话不多说，开始吧。</p>
<h1>AWS 上 Java Lambda 应用记要</h1>
<pre><code class="java">public class LambdaFunctionHandler implements RequestHandler&lt;Object,Object &gt; {

    int warmNum = 0;
    public GatewayResponse handleRequest(Object input, Context context) {
        Object data = null;
        Map&lt;String, String&gt; headers = new HashMap&lt;&gt;();
        headers.put("Content-Type", "application/json");
        headers.put("X-Custom-Header", "application/json");
        try {
            // 获取传入数据
            Object body = JSON.parseObject(JSONObject.toJSONString(input)).get("body");

            if(body.toString().equals("&lt;body&gt;&lt;action&gt;WARMUP&lt;/action&gt;&lt;/body&gt;")){
                this.timingTask();
                warmNum++;
                context.getLogger().log("启动次数："+warmNum);
                return new GatewayResponse("{}",headers,200);
            }

            JSONObject requestDTO = JSON.parseObject(body.toString());
            String requestMethod = requestDTO.getString("requestMethod");
            String requestData = requestDTO.getString("requestData");

            // 调用对应的方法
            String serviceName = MethodCallEnums.getServiceNameByMethod(requestMethod);
            String methodNameBymethod = MethodCallEnums.getMethodNameBymethod(requestMethod);
            Class aClass = Class.forName(serviceName);
            if (null == requestData || requestData.isEmpty()) {
                try {
                    data = aClass.getMethod(methodNameBymethod).invoke(aClass.newInstance());
                    Result result = ResultUtil.success(data);
                    return new GatewayResponse(JSON.toJSONString(result),headers,200);
                } catch (Exception e) {
                    e.printStackTrace();
                    Result result = ResultUtil.warn(ResultEnum.FAILED);
                    return new GatewayResponse(JSON.toJSONString(result),headers,500);                }
            } else {
                try {
                    data = aClass.getMethod(methodNameBymethod, String.class).invoke(aClass.newInstance(), requestData);
                    Result result = ResultUtil.success(data);
                    return new GatewayResponse(JSON.toJSONString(result),headers,200);
                } catch (IllegalAccessException e) {
                    e.printStackTrace();
                    Result result = ResultUtil.warn(ResultEnum.FAILED);
                    return new GatewayResponse(JSON.toJSONString(result),headers,500);                }
            }
        } catch (Exception e) {
            e.printStackTrace();
            Result result = ResultUtil.warn(ResultEnum.FAILED);
            return new GatewayResponse(JSON.toJSONString(result),headers,500);
        }

    }
</code></pre>
<p>以上是我写的一个简单的不能再简单的lambda例子了，其中的要点我会稍微提一下，完整代码戳这里<a href="https://github.com/wblearn/CopyHistory" target="_blank">github传送门</a>下载。</p>

<p><strong>要点</strong></p>
<ol>
<li>lambda函数的入口是handleRequest()方法，用来处理请求</li>
<li>Context对象是lambda上下文对象，可以将其封装进日志类里打印日志信息</li>
<li>请求体里本例里直接用父类Object接收，当然你也可以自定义接收对象和响应对象，但一定要包含必要的接收变量，比如body，headers，statusCode。</li>
<li>本例通过枚举类和反射来处理路由</li>
<li>从请求获取请求方法的方式有两种：(1)从lambda请求里的proxy获取 (2)用户在请求体body参数里自定义，如本例中的requestMethod ,对于自定义的好处是，当需要配APIConfig的时候，可以一个模块只配置一个API</li>
<li>lambda可以结合aws自身的一些产品来使用，比如本例中的aws dynamodb和aws s3</li>
<li>lambda可以处理get和post请求，根据请求方式不同相应处理即可</li>
<li>首次触发时微服务冷启动有些慢，但一旦启动之后就可以用这个微服务实例接受后续的请求，只有在比较长的一段时间内未被触发 AWS 才会把这个微服务杀掉。<br>
...<br>
lambda还有很多其他的一些特性，这里不一一提了。</li>
</ol>
<h1>写在最后</h1>
<p>AWS 的 Lambda 给了那些不想自己管理 EC2 服务器和配置负载人员很大的便利，所以 Lambda 被描述为 Serverless。真正的只关注业务就行，怎么调度，同时有多少个实例运行交给亚马逊去处理就是了。运行 Lambda 的环境也是亚马逊内部的 EC2 服务器，镜像是 Amazon Linux, 所以如果想运行系统命令，那是 Linux 的。Lambda 支持多种语言 Node.js, Python, C#(.net core), 还有 Java 8，我们就选择了 Java 8, 一开始还担心它与别的语言比起来会多大劣势，其实不然。而且所谓的 Java 8, 并非单指Java 语言，而是指 JVM 平台，所以也可以用 Scala, Clojure, Groovy, Kotlin 来写。</p>
<p>Java 与脚本语言如 Node.js, Python 相比给人一个明显的感觉是启动慢，还有人用统计数据来比划AWS Lambda cold start(pseudeo-)benchmark.不过真不用担心，人家说的是冷启动，也就发生在部署后第一次执行启动会比较慢。要是我们的 Lambda经常被调用，或每天触发比较集中，Lambda 在任务到来之前处理待续状态，就不会有冷启动的耗时过程。或者是每次任务要执行 3分钟左右，又何必在乎毫秒级的冷启动时间。</p>
</div>