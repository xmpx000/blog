---
title: 使用Google Analytics进行注册用户分析思路
author: clarke
layout: post
category: blog
tags:
  - google
  - 用户分析
---
正是因为我们希望了解用户，所以才有了网站分析需求。而Google Analytics的默认数据中，等价于用户概念的量值应该是“唯一身份访问者”，而对于拥有注册用户的网站而言，如果能够在Google&nbsp; Analytics 中增加注册用户的信息，统计区分真实的注册用户行为，进行深层次的数据分析与挖掘，那对分析工作会有很大的帮助，对公司而言也是很节省资源的一件事情。谈到这里，不得不感慨Google Analytics的开放性，让这种想法成为了可能。接下来我们看看如何利用Google&nbsp; Analytics 的自定义变量及Reporting API来实现这个方案。

<!--more-->

说明：因为Google Analytics对数据规模的限制（导出数据的抽样），本文内容对应的数量级为日登陆用户在十万级别，百万级别的日登陆用户网站应该有能力自己搭建独立的平台或者买一套Google Analytics Pre版了。 
1.数据的采集： 
数据的采集需要使用到Google Analytics的自定义变量。关于自定义变量的相关介绍，可以看看我这篇文章：<http://itweb.me/?p=189>。在这里，我们使用Visitor级别的自定义变量。因为我们需要统计注册用户信息，所以要在变量中存储用户的ID编码（考虑到用户隐私问题，不建议存放ID昵称，只存放ID）。同样我们还对用户的注册日期比较感兴趣，因为这个可以用来计算生命周期。最终我们的代码格式如下： 
*\_gaq.push(['\_setCustomVar',  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1,&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'Member ID',&nbsp;&nbsp;&nbsp; //  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; '121212_20130101',&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //   
]);*&nbsp; 
之所以将ID与注册日期用&#8217;_'连接，是为了节约自定义变量资源，将两个字段放到一个位置存储。在今后做数据处理时，再将字段拆分开。 
数据分组的设置： 
以上的设置试用于小规模网站，对于每天登录用户操过5万的网站而言，就需要进一步优化数据了。因为Google Analytics每天的Table Size只能有5万行记录，对于超出部分的数据会归纳到Other中，所以还需要对数据进一步细分。方法很简单，依据用户类别对用分组，将&#8217;Member ID&#8217;改为对应的组名，如下：

*\_gaq.push(['\_setCustomVar',  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1,&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'GroupName',&nbsp;&nbsp;&nbsp; // 组别名称  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; '121212_20130101',&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //   
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 1&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //   
]);*&nbsp;

这样就可以避免other的出现。 
2.数据的导出： 
有了用户ID数据后，接下来的任务是将原始数据导出来，进行进一步的分析。比如统计用户生命周期，访问频次，地域分布、常见互动行为等等。考虑到Google Analytics对数据抽样的处理，所以采用API每天导出一次数据，避免或者降低抽样比例。而自定义的API可以通过多个维度将你感兴趣的用户数据导出到本地，再进行进一步的分析。 
Google Analytics的Reporting api有完整的文档，开发也非常方便，我自己是使用Python进行的开发，将数据导出为CSV格式的文件，存储到本地。在数据导出的过程中，我们还需要对数据进行清洗与处理，比如将ID_注册日期拆分为2个字段。将异常的ID值数据排除掉。另外你如果有其他的需求，可以通过Python进行预处理。 
3.数据入库分析 
将数据导入数据库，可以依据自己的需求使用SQL查询相关数据，建立起用户统计分析框架。不过随着时间增长，用户ID数据会变的很庞大，一个日均10万用户登录网站，一个月就有300万行的记录。那么接下来的工作你或许就要考虑如何提高复杂SQL语句的查询效率问题了。