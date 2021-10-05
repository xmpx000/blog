---
layout: post
title: 微信公众号的RSS订阅解决方案
description: 微信公众号的RSS订阅解决方法。
category: blog
---
### 介绍

对于日常的资讯及博客更新阅读，我都喜欢用RSS订阅来解决，当年Google Reader，现在是feedly。自从微信公众号出来后，不知不觉自己就订阅了一堆的微信公众号，但是手机的阅读及检视实在是效率过低。以前采用的方案是用每天扫扫微信，顺便将感兴趣的用pocket保存起来。但这样还不方便，直到看到有朋友在朋友圈介绍了公众号保存为RSS订阅的方案，但是那个RSS烧制的地址为个人作者维护，稳定性有所欠缺。所以寻找一个稳定的RSS抓取源成了解决这个问题的关键点。前段时间发现小众软件有篇文章介绍如何利用 [Feed43制作RSS源](http://www.appinn.com/feed43/)的文章，利用Feed43.com可以很好的解决RSS抓取问题。

#### 1.获取公众号地址
首先我们第一步需要找到我们想订阅的公众号地址。目前公众号的文章列表没有网页官方地址可以访问，因此只能通过曲线救国的方式，通过sogou.com中的微信搜索，搜索你要的订阅的公众号，查找到公众号的地址，比如[小道消息]的地址就是：http://weixin.sogou.com/gzh?openid=oIWsFt86NKeSGd_BQKp1GcDkYpv0。

*注意：因为目前搜狗搜索对抓取也会有延迟，因此如果搜索结果页中的文章没有更新，那么我们制作的RSS也不会更新。比如小道消息有篇文章出现了4天的延迟。*

在chrome浏览器下，用F12查看网页请求，可以看到找到一条gzhjs的请求，地址类似：http://weixin.sogou.com/gzhjs?cb=sogou.weixin.gzhcb&openid=oIWsFt86NKeSGd_BQKp1GcDkYpv0&eqs=tUsYo8OgE6twomWXaQdVku7NKxnk2wEmdNj6ZkctDPsSYcedigs324KsshDZJ5q7I%2FMZr&ekv=7&page=1&t=1439470598227，把这条请求保存下来。

#### 2.制作RSS
访问feed43.com,注册登录不多说，操作步骤可以参考前面提到的小众软件的方法，这里只写下step2的参数获取方式,如果想深入了解参数的配置，可以访问help文档查看。

```html
<title><![CDATA[{%}]{*}<\/title><url><![CDATA[{%}]{*}<\/url>{*}<content168><![CDATA[{%}]{*}<\/content168>

```
#### 3.RSS订阅
选择什么工具，就看各自爱好了。

附：配置截图
step1:
![步骤1](http://itweb.me/wp-content/uploads/2015/step1.png)

step2:
![步骤2](http://itweb.me/wp-content/uploads/2015/step2.png)

step3:
![步骤3](http://itweb.me/wp-content/uploads/2015/step3.png)


[It'web]:    http://itweb.me  "It’web"
