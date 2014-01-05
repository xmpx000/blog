---
title: 如何本地测试Google Analytics跟踪脚本
author: clarke
layout: post
permalink: /?p=181
categories:
  - 网站分析笔记
tags:
  - GA
  - 日常经验
---
很多时候在写Google Analytics的跟踪脚本（GATC）时，需要进行脚本测试，看看是否能够满足的自己的采集需求。但这样一来，又可能需要走很多流程把这个工作拖的很久（原因你懂的），这里向大家介绍一种简单方便的方式：搭建本地测试环境进行测试。 
本文分为以下几部分：  
> 1.安装本地的Web服务器 
> 2.修改Host名称 
> 3.新建网页，插入代码 
> 4.测试：检测采集数据</blockquote> 
> 
> <!--more-->
> 
>   
> > 
> ## 1.安装本地服务器。
> 
> 高版本的windows系统一般都带有IIS安装程序。但是现在大部分的正版用户一般都是基础版，不带有此功能，所以推荐使用xampp。 
> “许多人通过他们自己的经验认识到安装 Apache 服务器是件不容易的事儿。如果您想添加 MySQL、PHP 和 Perl，那就更难了。 
> XAMPP 是一个易于安装且包含 MySQL、PHP 和 Perl 的 Apache 发行版。XAMPP 的确非常容易安装和使用：只需下载，解压缩，启动即可。”&#8211;摘自xampp主页（<http://www.apachefriends.org/zh_cn/xampp.html>）。 
> 你可以访问下载自己需要的版本，如果是小白用户，就下载安装包进行安装吧。 
> 在安装完这个软件后，启用Apache服务即可。(我也用xampp搭建博客的本地环境，这就需要启动Mysql服务了）。 
> [<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="Image(10)" border="0" alt="Image(10)" src="http://itweb.me/wp-content/uploads/2012/10/Image10_thumb.png" width="244" height="206" />][1]  
> ## 2.修改本地Host。
> 
> 在“C:\Windows\System32\drivers\etc”（windows7 系统）目录下，可以看到“hosts”文件。你可以复制到桌面（windows 7有权限管理，所以需要此操作，xp无需），使用记事本打开。添加你想测试的域名，格式如下：  
> > 127.0.0.1&nbsp;&nbsp;&nbsp;&nbsp; itweb.me
> 
> 点击保存，再复制到“C:\Windows\System32\drivers\etc”目录下，替换原有文件。这样你访问itweb.me，就可以访问到本地的网页了。 
> 打开浏览器，输入你刚刚配置的域名试试看。  
> ## 3.新建网页，插入代码
> 
> 访问你的软件安装目录，在安装目录下有个文件夹，叫做“htdocs”，可以新建一个文件夹，如命名为“webtest”，在里面新建网页aaa.html. 
> 在浏览器中输入ww.itweb.me/webtest/aaa.html，你就可以访问到刚刚新建的网页。这样你就可以将计划测试的GATC代码插入到网页中，进行测试了。  
> ## 4.测试：检测采集数据
> 
> 我一般使用Firefox进行测试查看。Firefox下的插件HttpFox可以很好的显示请求的参数信息。如下图： 
> 另外在使用时在过滤器中输入google-可以过滤你不需要的请求信息。 
> [<img style="background-image: none; border-right-width: 0px; padding-left: 0px; padding-right: 0px; display: inline; border-top-width: 0px; border-bottom-width: 0px; border-left-width: 0px; padding-top: 0px" title="Image(11)" border="0" alt="Image(11)" src="http://itweb.me/wp-content/uploads/2012/10/Image11_thumb.png" width="244" height="70" />][2] 
> 本方法也试用于测试百度统计的数据。如果你有任何问题，欢迎给我留言讨论。

 [1]: http://itweb.me/wp-content/uploads/2012/10/Image10.png
 [2]: http://itweb.me/wp-content/uploads/2012/10/Image11.png