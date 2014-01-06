---
layout: post
title: 如何使用Google Analytics统计网页加载时长？
description: 分析的需求是无穷尽的。每当我打开老王的那满是图片的博客时，我就想：大部分访问者的打开时长是多少？有多少访问者是因为打开时间过长而走掉的了？现在大家都在讨论用户体验，如果让你的上帝在刷个页面上花时间都发个微博都长，那人家走你都不好意思说啥:).
category: blog
---

分析的需求是无穷尽的。每当我打开老王的那满是图片的博客时，我就想：大部分访问者的打开时长是多少？有多少访问者是因为打开时间过长而走掉的了？现在大家都在讨论用户体验，如果让你的上帝在刷个页面上花时间都发个微博都长，那人家走你都不好意思说啥:).

有需求就有解决方案，要看到你的网站表现如何，得先有事实数据说话才可以，现在整理下GA中统计页面加载时间方法：

### GA的官方解决方案

5月初，Google官方推出新版GA的一个新报告：页面加载时间分析。打开新版GA的“内容”栏目，你可以有个一个“网站速度”报告。如下图所示:

[<img style="display: block; float: none; margin-left: auto; margin-right: auto; border-width: 0px;" title="report-1" src="http://itweb.me/wp-content/uploads/2011/05/report1_thumb.png" border="0" alt="report-1" width="471" height="330" />][1]

但是这个报告不是默认生成的，需要对你网页中的GATC，做一下修改增加_tackPageLoadTime()函数，如下所示(本示例为异步GATC脚本)：


    <script type="text/javascript">
      var _gaq = _gaq || [];
      _gaq.push(['_setAccount', 'UA-XXXXXX-1']);
      _gaq.push(['_trackPageview']);
      _gaq.push(['_trackPageLoadTime']);
      (function() {
        var ga = document.createElement('script'); ga.type = 'text/javascript'; ga.async = true;
        ga.src = ('https:' == document.location.protocol ? 'https://ssl' : 'http://www') + '.google-analytics.com/ga.js';
        var s = document.getElementsByTagName('script')[0]; s.parentNode.insertBefore(ga, s);
      })();
    </script>

更多说明请点击访问正统的GA help：<a href="http://www.google.com/support/analyticshelp/bin/answer.py?hl=en&answer=1205784&topic=1120718&utm_source=gablog&utm_medium=blog&utm_campaign=newga-blog&utm_content=sitespeed" target="_blank"><strong>Site Speed</strong></a> 。

官方方案的问题在于这个数据是抽样统计，我在5月初兴匆匆的为我的博客更新了代码，但是等了10多天了还是没有数据，开始我以为是我的配置有问题，后来在网上搜索了下，发现有人反馈这个抽样的频率不高，我这种小博客没几个人访问的，居然这段时间没有一条数据（汗颜，虽然这段时间只有20多个pv，但好歹给我显示一条嘛——昨晚我折腾到2点排查问题）。

### 非官方方案

#### 使用事件跟踪

在网上看到<a href="http://www.optimisationbeacon.com/" target="_blank">Rob Kingston</a>的博客，上面有一篇内容详细介绍了如何使用_trackEvent()跟踪网页加载时长，<a href="http://www.optimisationbeacon.com/analytics/track-page-load-times-with-google-analytics-asynchronous-script/?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+OptimisationBeacon+%28Optimisation+Beacon%29" target="_blank">原文地址请点击我</a>。

其实方法也比较简单，在此简单的把内容翻译加工下：

**准备工作（人家都搞的这么专业，我也学习下）：**

*   网站已经添加最新的异步代码而且正常运行；
*   修改使用的GATC代码，避免跳出率计算异常（这个等会说明如何修改）；
*   新建一个新的profile，采用“为新域添加配置文件”的方式，这样可以获得一个新的UA ID。


**第一步:修改原先的跟踪代码**

最初的的跟踪如下：

    <script type="text/javascript">
      var _gaq = _gaq || [];
      _gaq.push(['._setAccount', 'UA-15373241-1']);
      _gaq.push(['._trackPageview']);
      (function() {
        var ga = document.createElement('script'); ga.type ='text/javascript'; ga.async = true;
        ga.src = ('https:' == document.location.protocol ? 'https://ssl' :'http://www') + '.google-analytics.com/ga.js';
        var s = document.getElementsByTagName('script')[0];s.parentNode.insertBefore(ga, s);})();
    </script>

修改后如下：

    <script type="text/javascript">
      var _gaq = _gaq || [];
      _gaq.push(['pageTracker._setAccount', 'UA-15373241-1']);
      _gaq.push(['pageTracker._trackPageview']); 
      (function() {
         var ga = document.createElement('script'); ga.type ='text/javascript'; ga.async = true;
         ga.src = ('https:' == document.location.protocol ? 'https://ssl' :'http://www') + '.google-analytics.com/ga.js';
         var s = document.getElementsByTagName('script')[0];s.parentNode.insertBefore(ga, s);
      })();
    </script>
<span style="color: #000000; font-family: Consolas; font-size: x-small;">说明：这里使用pageTracker对象，避免后面的冲突。</span>

**<span style="color: #333333; font-family: Consolas;">第二步，在页面顶端head区域添加JS代码，这表示从页面顶端开始计时；</span>**

    <script type="text/javascript">
     var plstart = new Date();
    </script>

**<span style="color: #333333; font-family: Consolas;">第三步，添加事件跟踪代码：</span>**

    <script type="text/javascript">
      window.onload=function() {
      var plend = new Date();
      var plload = plend.getTime() - plstart.getTime();
      if(plload<1000)
      lc = "Very Fast";
      else if (plload<2000)
      lc = "Fast";
      else if (plload<3000)
      lc = "Medium";
      else if (plload<5000)
      lc = "Sluggish";
      else if (plload<10000)
      lc = "Slow";
      else
      lc="Very Slow";
      var fn = document.location.pathname;
      if( document.location.search)
      fn += document.location.search;
      try {
      _gaq.push(['loadTracker._setAccount', 'UA-15373241-2']);
      _gaq.push(['loadTracker._trackEvent','Page Load (ms)',lc + ' Loading Pages',fn,plload]);
      _gaq.push(['loadTracker._trackPageview']);
      } catch(err){}
      }
    </script>


这段代码展示了采集的逻辑及判断依据，其中1000、2000都是以毫秒为单位，所以1000代表1秒。上面代码中设置的逻辑是小于1000毫秒就是Very Fast，你可以根据实际情况进行调整。

注意:

1.这个地方_gaq.push[]中的对象为loadTracker，而不是开始pageTracker，这样就避免的2个JS对象的冲突。
2.这段代码的UA ID为UA-15373241-2，与第一段代码的UA ID不一样，这是为了避免landing Page的跳出率计算错误。

<span style="background-color: #fafafa; color: #333333;">效果如下：</span>

[<img style="display: block; float: none; margin-left: auto; margin-right: auto; border-width: 0px;" title="report-2" src="http://itweb.me/wp-content/uploads/2011/05/report2_thumb.png" border="0" alt="report-2" width="442" height="276" />][2]

#### 使用虚拟页面

<span style="background-color: #fafafa; color: #333333;">此部分可以参考蓝鲸的博客内容：</span>

[Google Analytics中trackPageview函数的5种使用策略][3]
 
Read more: <http://bluewhale.cc/2010-01-12/google-analytics-trackpageview-policy.html#ixzz1MJKrQfci>
 
[使用JS和_trackPageview函数从时间维度监测页面表现][4]
 
Read more: <http://bluewhale.cc/2010-01-12/js-trackpageview-time-dimension-tracking-page.html#ixzz1MJKy7j8d>

####小结

<span style="font-family: 宋体;"> 3种方法都可以采集页面加载时间。但是官方的方法对访问量小的网站来说难以说明实际问题。采用事件跟踪方式和虚拟页面方式都可以获得准确的数据，但是需要新建Profile进行配置，另外十分值得注意的是需要对Landing Page的跳出率统计考虑。（蓝鲸的博客没有写到要新建Profile，但是在回复中他也说到虚拟页面会导致PV虚增，需进行过滤，其实也可以参考上面事件跟踪方式的方法将数据分离。）</span>

<span style="font-family: 宋体;"> 页面加载时间并不需对全站进行，所以推荐先根据GA报告统计哪些页面为热点页面，再单独插码进行处理。</span>

 [1]: http://itweb.me/wp-content/uploads/2011/05/report1.png
 [2]: http://itweb.me/wp-content/uploads/2011/05/report2.png
 [3]: http://bluewhale.cc/2010-01-12/google-analytics-trackpageview-policy.html
 [4]: http://bluewhale.cc/2010-01-12/js-trackpageview-time-dimension-tracking-page.html
