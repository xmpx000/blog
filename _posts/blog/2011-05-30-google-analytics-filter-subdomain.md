---
title: Google Analytics过滤器小常识1：多子域/目录跟踪与处理
author: clarke
layout: post
description: 在实施跟踪计划的时候，常常会碰到多子域/多目录跟踪的问题。比如一个网站有多个子域名：如news.itweb.me、blog.itweb.me、t.itweb.me，不同的域名代表不同的栏目，属于不同的编辑部门管理；或者这样：我的网站有itweb.me/zh与itweb.me/en等目录，不同的目录代表不同的国家，对于以上的网站，为了数据上管理与保密，如何进行实施Google Analytics跟踪方案？
category: blog
tags:
  - GA
  - 过滤器
---
[<img class="alignright" style="display: block; margin-left: auto; margin-right: auto; border: 0pt;" title="Google Analytics 子域名/目录跟踪与配置" src="http://itweb.me/wp-content/uploads/2011/05/TM_thumb.jpg" alt="Google Analytics 子域名/目录跟踪与配置" width="253" height="193" border="0" />][1] *在实施跟踪计划的时候，常常会碰到多子域/多目录跟踪的问题。比如一个网站有多个子域名：如news.itweb.me、blog.itweb.me、t.itweb.me，不同的域名代表不同的栏目，属于不同的编辑部门管理；或者这样：我的网站有itweb.me/zh与itweb.me/en等目录，不同的目录代表不同的国家，对于以上的网站，为了数据上管理与保密，如何进行实施Google Analytics跟踪方案？*  
<!--more-->

处理上面的问题牵涉到3个方面：

1.  <div>
      网站插码采集的部署问题；
    </div>

2.  <div>
      数据访问权限的分配问题；
    </div>

3.  <div>
      数据的汇总计算问题。
    </div>

#### <span style="color: #000080; font-size: medium;">1：网站插码采集的部署问题</span>

网上有很多的资料，首先可以看看Google官方的资料：<a href="http://code.google.com/intl/zh-CN/apis/analytics/docs/concepts/gaConceptsDomains.html#siteDefinition" target="_blank">域和目录</a>、<a href="http://code.google.com/intl/zh-CN/apis/analytics/docs/tracking/gaTrackingSite.html" target="_blank">跨域跟踪</a>.这两篇文章详细的解释。另外也可以看看这些：[Google Analytics功能篇-跨域追踪][2]、<a href="http://www.chinawebanalytics.cn/wa-implementation-trap-1/" target="_blank">警惕网站分析监测实施的陷阱（上）</a>。

#### <span style="color: #000080; font-size: medium;">2：数据访问权限的分配问题</span>

对于这个问题，上面提到的文章也有描述，对于子域名/子目录来说，根据过滤器将不同的域名与栏目过滤出来，再给不同的人分配相关的查看报告权限即可。

#### <span style="color: #000080; font-size: medium;">3：数据的汇总计算问题：</span>

不论是你采用一个配置跟踪全站，再使用过滤器切割报告，或者是一个全站配置文件+N个子域配置文件中的哪种方式，对于子域名/子目录Profile的报告来说，比较明显的计算问题有：

*   访问本子域/栏目的人，如果去往其他子域/栏目，则会反映在退出数据中;
*   访问过其他子域/栏目，如果只在本栏目/子域中访问一个页面，则会反映在跳出率数据中；
*   Landing Page缺失：你的EDM或者PPC的Landing Page可能在其他的子域名/目录下，这样LandingPage为在本栏目/子域中访问的第一个页面。
*   平均页面停留时间数据

<p style="text-align: center;">
  <a href="http://itweb.me/wp-content/uploads/2011/05/knowsite.jpg"><img style="display: block; margin-left: auto; margin-right: auto; border-width: 0px;" title="know-site" src="http://itweb.me/wp-content/uploads/2011/05/knowsite_thumb.jpg" alt="know-site" width="526" height="256" border="0" /></a> <em>在全站Profile的热门内容中搜索”^/know”的结果</em>
</p>

<p style="text-align: center;">
  <img class="aligncenter" style="display: block; float: none; margin-left: auto; margin-right: auto; border-width: 0px;" title="know-directory" src="http://itweb.me/wp-content/uploads/2011/05/knowdirectory_thumb.jpg" alt="know-directory" width="524" height="199" border="0" /> <em>在使用子目录切割后的Profile的热门内容中搜索”^/know”的结果</em>
</p>

总而言之，如果GA中的某个指标是基于页面与页面直接的关系进行计算，那么这个数值在过滤器的影响下可能会有变化。比如退出，它判断是否退出是看这个人的下一个页面/动作是否在分析数据范围之内，如果你对全站的数据进行分析，访客从新闻到博客栏目不算退出，但是如果你只对新闻栏目数据进行分析，因为博客数据不在分析范围之类，所以在这时你去博客栏目就会被算作退出。

过滤器的使用对于来源统计有何影响了？因为GA对来源的统计是基于Cookie计算，所以过滤器的使用不会对来源统计产生影响。

#### <span style="color: #000080; font-size: medium;">4.如果我的统计是基于全站，那么如何查看子域名/目录的跳出、退出率？</span>

**对于子目录：**很简单的操作，在全站的那个Profile里面就可以看到所有的数据，如果你考虑到保密方面的问题，可以利用报告右下角的高级过滤器过滤对应的URL，再使用电子邮件功能发送对应的报告。

[<img style="display: block; float: none; margin-left: auto; margin-right: auto; border-width: 0px;" title="高级过滤器" src="http://itweb.me/wp-content/uploads/2011/05/thumb.jpg" alt="高级过滤器" width="525" height="139" border="0" />][3]

**对于子域名：**因为GA的内容报告中不显示URL的域名，所以你无法分辨哪些URL来自A子域。哪些来自B子域。这里需要你做一点过滤器配置，请参考<a href="http://www.google.com/support/googleanalytics/bin/answer.py?hl=zh-Hans&answer=55511" target="_blank">如何在报告中看到网页的完整网址？</a>看到完整的URL后，就可以像子目录那样进行操作了。另外，还可以读读帮助中心的这篇文章：<a href="http://www.google.com/support/googleanalytics/bin/answer.py?hl=zh-Hans&answer=55534" target="_blank">网址重写过滤器对目标和渠道设置有何影响？</a>

**PS:如果你为你Profile添加过滤器，请先复制一个Profile，再在复制的上面添加。记得永远保存一个原始的Profile配置。**

如果你对上面的描述有什么疑问，可以邮件联系我。:)

 [1]: http://itweb.me/wp-content/uploads/2011/05/TM.jpg
 [2]: http://bluewhale.cc/2010-04-18/google-analytics-cross-domain-tracking.html
 [3]: http://itweb.me/wp-content/uploads/2011/05/ba3ea5eccc0c.jpg