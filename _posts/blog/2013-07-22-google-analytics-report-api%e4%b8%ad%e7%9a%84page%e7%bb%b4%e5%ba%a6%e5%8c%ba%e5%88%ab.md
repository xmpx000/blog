---
title: Google Analytics Report API中的Page维度区别
author: clarke
layout: post
permalink: /?p=219
categories:
  - 网站分析笔记
tags:
  - GA
  - 维度
---
Google analytics的report api一直在不停的更新，Page Tracking下的维度也更新了很多个。上周群里在讨论利用api导出页面上下游访问路径时，对几个不同的场景的理解有些混淆。 
其实目前来看，Google analytics默认报表中的上下游页面分析已经可以显示500个页面了，应该可以满足常见需求，如果利用API导出数据再依据栏目进行分组统计，那么可以更进一步的得出不同栏目之间的上下游关系。

<!--more-->

个人觉得几个容易混淆的维度： 
ga:pagePath 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A page on your website specified by path and/or query parameters. Use in conjunction with ga:hostname to get the full URL of the page. 
ga:landingPagePath 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The first page in a user&#8217;s session, or landing page. 
ga:secondPagePath 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; The second page in a user&#8217;s session. 
ga:previousPagePath 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A page on your property that was visited before another page on the same property. Typically used with the ga:nextPagePath dimension. 
ga:nextPagePath 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; A page on your website that was visited after another page on your website. Typically used with the ga:previousPagePath dimension. 
注：以下皆为API导出数据需求。 
场景1： 
**统计访问首页的人接下来访问了哪些页面？** 
正确的方法：  
> 维度：ga:previousPagePath,ga:nextPagePath 
> 过滤器：ga:previousPagePath==‘首页’</blockquote> 
> 
> 之前想着是用**ga:pagePath,ga:nextPagePath**进行组合,但这种组合**是错误的**，无法得出数据。ga:previousPagePath,ga:nextPagePath在Google analytics应该是一对组合维度（个人推测）。另外在page的导航页面报告中，是如下图显示的。而在我们用api导出数据时，没有了current selection这个维度，previous Pages就代表了current selection这个概念。 
> <img src="http://itweb.me/wp-content/uploads/2013/google_analytcis_nav.png" width="587" height="141" /> 
> 场景2： 
> **目标页为首页的用户接下来访问了哪些页面。** 
> 正确的方法：  
> > 维度：ga:landingPagePath,ga:secondPagePath 
> > 量值：ga:visits 
> > 过滤器：ga:langdingPagePath==&#8217;首页&#8217;</blockquote> 
> > 
> > 这个报告和目标页中的进入路径报告类似。注意在这个地方不能使用ga:nextPagePath。对上下游页面分析而言，ga:landingPagePath,ga:secondPagePath也应该是一对固定组合，只不过量值为visits. 
> > 另外在做配置导出数据之前，可以利用官方的explorer进行测试下，看看维度组合是否有效。 
> > <http://ga-dev-tools.appspot.com/explorer/> 
> > 另外在使用过滤器时，可以参加下<http://itweb.me/?p=201>，关于过滤器级别的概念。