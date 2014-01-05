---
title: Google Analytics中正则表达式的使用
author: clarke
layout: post
permalink: /?p=225
categories:
  - 网站分析笔记
---
## 1.正则表达式简介：

正则表达式是用来匹配、描述一系列符合某个句法规则的字符串（维基百科），它是文本处理的有利工具。在Google Analytics中，我们会经常碰到查询、过滤符合某些条件的数据。因此正则表达式也有了很大的用武之地。 

<!--more-->

如果你还不了解正则表达式，那么可以先看看这篇文章<a href="http://deerchao.net/tutorials/regex/regex.htm" target="_blank">“正则表达式30分钟入门教程”</a>。它可以快速让你了解正则表达式的相关知识。另外文中提到的正则表达式测试工具十分有用，可以供今后测试使用。  
## 2.Google Analytics中的正则表达式规则

Google Analytics并不支持完整的正则表达式规则，它支持的表达式规则可以参考官方的帮助文档：<https://support.google.com/analytics/answer/1034324?hl=zh-Hans> 。这个文档中包含的例子也覆盖了大部分的需求，如批量匹配IP，关键词等，值得仔细看看。  
## 3.在Google Analytics中使用正则表达式

#### **在报表界面中使用**

![Google Analytics 报表中使用正则表达式][1]

如上图，在报表的过滤器中选择 Matching RegExp就是正则表达式匹配模式。

**举例：使用正则表达式查找频道页面，正则表达式如下：**

<pre class="csharpcode">/.+/.{3,10}$</pre>

“/.+/”匹配多个/xxx/xxx/xx/; 

“.{3,10}$”表示在“/”之后出现在的字符长度为3-10之间。因为内容页的标题很长，基本上不会短于10个字符，所以能够很好的查找出来。（此规则需要根据网站自身的url规则判断） 

#### **在Profile过滤器中使用**

在profile中过滤器中使用正则表达式的规则和报表一致，值得注意的是Advanced模式下的过滤器规则使用，它最多可以利用两个字段（**字段 A** 和**字段 B**）来构建**输出字段**。具体使用可以参考<https://support.google.com/analytics/answer/1034836>。

#### **在API接口中使用**

在某些情况下我们可能会考虑自己开发数据导出程序，同样也会在数据请求的过滤器中使用到正则表达式。关于此部分的详细使用规则可以参考<https://developers.google.com/analytics/devguides/reporting/core/v3/reference#filters>。其中的“Dimension Filters”说明了使用正则表达式的参数符号。*PS：前段时间自己写导出程序时没注意到这个地方，使用正则表达式过滤网页折腾了好久。*

 [1]: http://itweb.me/wp-content/uploads/2013/google_analytics_re.png ""