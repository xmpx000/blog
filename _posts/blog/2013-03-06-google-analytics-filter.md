---
title: Google Analytics的数据过滤规则探讨
author: clarke
layout: post
category: blog
tags:
  - GA
  - 过滤器
---
**1.过滤级别的介绍** 
**2.Google Analytics的过滤级别** 
**3.如何实现UV级的过滤**

<!--more-->

**1.过滤级别的介绍** 
过滤这个概念在生活中很常见，比如天气质量不好戴口罩，就是把空气中某些你不想要的物质排除在呼吸道范围之外。在网站数据分析中，我们所提到的过滤也类似，将某些你不想要的数据排除在分析报表之外，或者只让某些数据进入分析报表。空气不好有口罩，根据污染物还有PM10，PM 2.5的过滤级别区分。同样在网站分析中，过滤也是有级别的。 
对应PV、Visits、UV三个层级的数据粒度，我们也希望有这样三个级别的数据过滤级别： 
*   基于Hits级别的过滤（PV，事件跟踪均属于Hits级别）； 
    *   基于Visits的过滤； 
        *   基于UV级别的过滤； </ul> 
        同样，基于过滤器的影响范围，我们又希望有这样的级别： 
        *   应用于一系列的报告（整个配置文件） 
            *   应用于部分报告（只在对部分报告进行过滤） </ul> 
            这样一来，过滤器可能就存在3×2=6种了，当然，肯定不是所有的分析工具都有这么多的过滤级别。我们可以看看Google Analytics有哪些。 
            
            <table border="0" cellspacing="0" cellpadding="2" width="400">
              <tr>
                <td valign="top" width="100">
                  &nbsp;
                </td>
                
                <td valign="top" width="100">
                  Hit
                </td>
                
                <td valign="top" width="100">
                  Visits
                </td>
                
                <td valign="top" width="100">
                  UV
                </td>
              </tr>
              
              <tr>
                <td valign="top" width="100">
                  配置文件级别
                </td>
                
                <td valign="top" width="100">
                  &nbsp;
                </td>
                
                <td valign="top" width="100">
                  &nbsp;
                </td>
                
                <td valign="top" width="100">
                  &nbsp;
                </td>
              </tr>
              
              <tr>
                <td valign="top" width="100">
                  报告级
                </td>
                
                <td valign="top" width="100">
                  &nbsp;
                </td>
                
                <td valign="top" width="100">
                  &nbsp;
                </td>
                
                <td valign="top" width="100">
                  &nbsp;
                </td>
              </tr>
            </table>
            
            **2.Google Analytics的过滤级别** 
            在Google Analytics中，目前有的级别有： 
            *   配置文件级别的Hit（部分Visits）； 
                *   报告级别的Hit,Visits。 </ul> 
                **a.配置文件级别的Hit（部分Visits）** 
                这个主要就是配置文件中的过滤器。不过这个过滤器比较特别，还可以利用hit过滤来实现visits级别的过滤。其实这个比较好理解，一个Visits是由多条Hit记录组成的。比如一个Visit的所有Hit记录的来源应该是一样的，或者IP地址是一样。这样的话，就可以将所有来自某个IP或者来源的Hit记录进行过滤操作，进而达到Visits级过滤的目的。 
                **b.报告级别中的hit ,visits** 
                报告上的搜索框：对Langding page、Source这种类型的报表而言，属于访次级别；对于Page报表而言，属于page级别。这里要注意下，高级细分是基于访次进行过滤的。 
                **3.如何实现UV级过滤** 
                首先我们知道：**高级细分基于访次，过滤访问者时存在问题，****唯一身份访问者(UV )在两个细分中可能存在累加现象。** 
                *以下来自Google 帮助文档：* 
                高级细分依据的标准是用户的会话（访问）数据。如果某个访问者在指定日期范围内的任意一次访问中达到了您的高级细分条件，该访问者就会被加入该细分中。 
                例如，假设您创建了一个高级细分来找出从未转化过的访问者（与该访问者对应的收入等于 0）。在这种情况下，如果某个访问者在指定日期范围内有 4 次访问带来的收入高于 100 元，同时有 1 次访问带来的收入为 0 元，就会被错误地归类为从未转化过的访问者。 
                为避免这种错误归类的情况，您可以使用访问者级的自定义变量来汇总多个会话的行为，然后再根据该自定义变量创建高级细分。请参考：<https://support.google.com/analytics/bin/answer.py?hl=zh-Hans&answer=2481996>