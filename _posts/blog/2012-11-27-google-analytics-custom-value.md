---
title: Google Analytics自定义变量的使用小结
author: clarke
layout: post
category: blog
tags:
  - 自定义变量，ga
---
## 1.自定义变量的概念；

顾名思义，自定义变量就是可以根据自己需要标识的变量，它的推出极大的丰富了Google Analytics的数据采集范围和采集方式，也让我们深入分析用户和流量有了一把好工具。

<!--more-->

详细介绍可以参考：Google Analytics的官方帮助文档介绍以及Justin的博客： 
1.<https://developers.google.com/analytics/devguides/collection/gajs/gaTrackingCustomVariables?hl=zh-CN> 
2.<https://support.google.com/analytics/bin/answer.py?hl=zh-Hans&answer=2481996> 
3.<http://cutroni.com/blog/2011/05/18/mastering-google-analytics-custom-variables/> 
关于Justin的博客，有肖庆同学的翻译版:<http://xiaoq.in/google-analytics/mastering-google-analytics-custom-variables/> 
以上3个文档可以很好的帮助你体会自定义变量的含义，所以不再赘述.话说回来我还是很喜欢Justin那个关于停车位的比喻:).  
## 2.自定义变量的注意事项；

a).页面级的自定义变量和事件差不多，考虑到自定义变量只有5个位置，所以页面级变量可以考虑采用事件跟踪； 
b).必须放到放到trackpageview前面，其他代码之后面。曾经有人这样设置了自定义变量，如下图： 
[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="Image(14)" border="0" alt="Image(14)" src="http://itweb.me/wp-content/uploads/2012/11/Image14_thumb.png" width="498" height="259" />][1] 
结果导致新增搜索引擎的代码全部失效。所以不管怎样，将setCustomvar紧贴在trackPageview之前是妥善的做法。 
c).自定义变量的设置与更新都需要向Google Analytics发出请求才能起作用。这就意味着你如果更新了用户的自定义变量，就需要向Google Analytics 发出一条请求，但是\_gaq.push(['\_setCustomVar',1,'VisitorType','Member', 1]);这条语句是不会发送请求的，所以你还得跟着执行一条trackPageview或者trackEvent语句。 
d).关”访问开始次数”的概念： 
在Google Analytics 的自定义变量报表中，有一个字段为“访问开始次数”，官方定义为：The number of visits in which the session started with a custom variable. 这个定义对Visit、Vistor 级别比较好理解，可以直接视为访次，但是对Page级别变量如何理解了？我们做个实验看看。 
下图为Page级别自定义变量的系统默认报告： 
[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="Image(13)" border="0" alt="Image(13)" src="http://itweb.me/wp-content/uploads/2012/11/Image13_thumb.png" width="569" height="136" />][2] 
下图为自定义报告： 
[<img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top: 0px; border-right: 0px; padding-top: 0px" title="Image(12)" border="0" alt="Image(12)" src="http://itweb.me/wp-content/uploads/2012/11/Image12_thumb.png" width="569" height="129" />][3] 
对于Page级别的报告而言，访问开始次数是指Page页面做为目标网页时的访次， 
Google Analytics的进入次数是指的访次，而对网页而言，访次只在作为目标网页时才有存在，**所以系统默认的报告只展示了网页级变量作为目标网页时所产生的数据，而真实的浏览数据，还需要我们通过自定义报告去获取。** 
报表中的时间为Page级别变量，是文章的发布时间。我们可以看到2012116这天的访问开始次数和20091118这天差不多，从系统默认的数据来看我们可能认为这一样。但是通过自定义报告，我们发现了巨大的差异。16号发布文章的浏览量远高于18号的文章浏览量。 
关于这个问题更详细的阅读可以参考：<http://www.lunametrics.com/blog/2012/09/25/page-level-custom-variables-reports/>。

 [1]: http://itweb.me/wp-content/uploads/2012/11/Image14.png
 [2]: http://itweb.me/wp-content/uploads/2012/11/Image13.png
 [3]: http://itweb.me/wp-content/uploads/2012/11/Image12.png