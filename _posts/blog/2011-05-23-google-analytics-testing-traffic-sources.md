---
title: Google Analytics测试案例：流量来源分析
author: clarke
layout: post
permalink: /?p=28
Views:
  - 20
categories:
  - 网站分析笔记
tags:
  - GA
  - 测试
---
前两天看到蓝鲸的博客《<a href="http://bluewhale.cc/2011-05-09/google-analytics-question-and-answer-1.html" target="_blank">Google Analytics使用中的常见问题（一）</a>》，在问题2中提到了GA中流量来源的级别问题，并推荐了郑海平的博客。翻了下郑海平以前的文章<a href="http://www.wachina.net/index.php/googleanalyticsmarketingchannnellogics/" target="_blank">《如何解读Google Analytics渠道流量数据(一)：流量是怎么算出来的》</a>，发现这个问题挺有意思的，博主在博文中对GA的四种流量做了详细的解释，<!--more-->在此不重述，我们直接看看结论(先感谢原文博主)：

> 对于这四类流量来源，Google Analytics是如下来定义规则的：
> 
> 一、同一次访问内覆盖规则：
> 
> 1.投放活动永远能覆盖别的渠道
> 
> 2.自然搜索永远能覆盖别的渠道
> 
> 3.推介网站永远覆盖不了别的渠道
> 
> 4.直接输入永远覆盖不了别的渠道
> 
> 二、对于访客回访时的覆盖规则：
> 
> 1.投放活动永远能覆盖别的渠道
> 
> 2.自然搜索永远能覆盖别的渠道
> 
> 3.推介网站永远能覆盖别的渠道
> 
> 4.直接输入永远覆盖不了别的渠道

第一次看到这个总结时，我很想知道能否在GA的官方帮助中找到对应的介绍，可惜能力有限，只找到这个文档：Google Code中的<a href="http://code.google.com/intl/zh-CN/apis/analytics/docs/tracking/gaTrackingTraffic.html" target="_blank">流量来源</a>。里面提到了2个规则：1.新的付费广告系列可以覆盖老的广告系列的cookie信息；2.直接流量永远不会覆盖现有的广告系列。

对于上述结论，如果你有困惑，但又找不到相关文档，就只能测试验证了。其实测试很简单，方法如下：

1.找个一个使用GA的网站，比如你本地利用IIS搭建的一个简单网站；

2.打开浏览器中的http请求监测工具，我使用的是Firefox下的HttpFox。

3.先使用网址构建器生产一个带广告参数的访问地址，如下：

<pre class="csharpcode">访问地址：

http://www.XXXXXX.com/html/index.html?utm_source=test-ref-index-2&#038;utm_medium=test&#038;utm_campaign=testname

请求信息：
utmwv    4.9.4
utms    14
utmn    1954966713
utmhn    www.XXXXXX.com
utmcs    x-gbk
utmsr    1400x1050
utmsc    24-bit
utmul    zh-cn
utmje    1
utmfl    10.3 r181
utmdt    GIS_å°çä¿¡æ¯ç³»ç»_GISè®ºå_GISåå®¢-ALL FOR GIS
utmhid    156617965
utmr    -
utmp    /html/index.html?utm_source=test-ref-index-2&utm_medium=test&utm_campaign=testname
utmac    UA-8235932-1
utmcc    __utma=108883860.1012660805.1299825094.1306076989.1306078695.9;+__utmz=108883860.1306079890.9.7.utmcsr=test-ref-index-2|utmccn=testname|utmcmd=test;
utmu    D~</pre>

4.打开新的标签页，直接输入网址访问：

<pre class="csharpcode">直接输入：

utmwv    4.9.4
utms    9
utmn    1534785708
utmhn    www.XXXXXX.com
utmcs    x-gbk
utmsr    1400x1050
utmsc    24-bit
utmul    zh-cn
utmje    1
utmfl    10.3 r181
utmdt    GIS_å°çä¿¡æ¯ç³»ç»_GISè®ºå_GISåå®¢-ALL FOR GIS
utmhid    417979899
utmr    -
utmp    /html/index.html
utmac    UA-8235932-1
utmcc    __utma=108883860.1012660805.1299825094.1306076989.1306078695.9;+__utmz=108883860.1306079223.9.4.utmcsr=test-ref-index-2|utmccn=testname|utmcmd=test;
utmu    D~</pre>

5.从其他网站点击链接访问：

<pre class="csharpcode">从其他网站过来

utmwv    4.9.4
utms    15
utmn    1253340026
utmhn    www.XXXXXX.com
utmcs    x-gbk
utmsr    1400x1050
utmsc    24-bit
utmul    zh-cn
utmje    1
utmfl    10.3 r181
utmdt    GIS_åœ°ç†ä¿¡æ¯ç³»ç»Ÿ_GISè®ºå›_GISåšå®¢-ALL FOR GIS
utmhid    1836347421
utmr    http://www.esrichina-bj.cn/
utmp    /html/index.html
utmac    UA-8235932-1
utmcc    __utma=108883860.1012660805.1299825094.1306076989.1306078695.9;+__utmz=108883860.1306079890.9.7.utmcsr=test-ref-index-2|utmccn=testname|utmcmd=test;
utmu    D~</pre>

<pre class="csharpcode">6.再在Google中搜索此网站，点击关键词访问：</pre>

<pre class="csharpcode">Google搜索后请求信息：
utmwv    4.9.4
utms    16
utmn    501066610
utmhn    www.XXXXXX.com
utmcs    x-gbk
utmsr    1400x1050
utmsc    24-bit
utmul    zh-cn
utmje    1
utmfl    10.3 r181
utmdt    GIS_å°çä¿¡æ¯ç³»ç»_GISè®ºå_GISåå®¢-ALL FOR GIS
utmhid    425062692
utmr    http://www.google.com/search?sourceid=navclient&hl=zh-CN&q=gisall
utmp    /html/index.html
utmac    UA-8235932-1
utmcc    __utma=108883860.1012660805.1299825094.1306076989.1306078695.9;+__utmz=108883860.1306080143.9.8.utmcsr=google|utmccn=(organic)|utmcmd=organic|utmctr=gisall;
utmu    D~</pre>

7.再次生产一个新的广告版本，进行访问：

<pre class="csharpcode">输入

http://www.XXXXXX.com/html/index.html?utm_source=test-ref-index&#038;utm_medium=test&#038;utm_campaign=testname

请求信息：
utmwv    4.9.4
utms    13
utmn    1193996599
utmhn    www.XXXXXX.com
utmcs    x-gbk
utmsr    1400x1050
utmsc    24-bit
utmul    zh-cn
utmje    1
utmfl    10.3 r181
utmdt    GIS_å°çä¿¡æ¯ç³»ç»_GISè®ºå_GISåå®¢-ALL FOR GIS
utmhid    1460648580
utmr    -
utmp    /html/index.html?utm_source=test-ref-index&utm_medium=test&utm_campaign=testname
utmac    UA-8235932-1
utmcc    __utma=108883860.1012660805.1299825094.1306076989.1306078695.9;+__utmz=108883860.1306079777.9.6.utmcsr=test-ref-index|utmccn=testname|utmcmd=test;
utmu    D~</pre>

**GA是通过utmcc参数中的utmcsr进行来源判别的，我们可以看到utmcsr的变化：**

**第一个广告版本：utmcsr=test-ref-index-2；**

**打开新标签页直接访问：utmcsr=test-ref-index-2**

**通过其他网站链接访问：utmcsr=test-ref-index-2**

**通过搜索引擎访问：utmcsr=google**

**通过第二个广告版本访问：utmcsr=test-ref-index**

> 可以看出：在同一个访次中，投放活动与自然流量处于同一个级别，可以覆盖其他来源，推荐网站与直接流量处于同一个级别，不能覆盖其他来源（当然从上述几次测试还不能得出全部的结论，但是你可以列表将所有组合情况测试一次）。

一个看上去比较神奇的问题，可以很容易就通过测试验证，这就是测试的作用:)

写了这么多，其实过程很简单，如果你对GA的测试还有什么疑问就邮件与我联系吧。