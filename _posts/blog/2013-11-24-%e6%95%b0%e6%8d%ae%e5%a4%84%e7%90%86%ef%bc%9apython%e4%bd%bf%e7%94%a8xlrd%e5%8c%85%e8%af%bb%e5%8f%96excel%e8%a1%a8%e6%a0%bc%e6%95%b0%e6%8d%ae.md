---
title: 数据处理：Python使用xlrd包读取Excel表格数据
author: clarke
layout: post
permalink: /?p=231
categories:
  - python
tags:
  - 数据处理
---
在日常的数据处理工作中，我们经常会有批量处理Excel文件的需求。比如历史上定期的Excel报价表格，或者是会员信息等等。针对多个文件的数据处理，VBA宏是一种方案，而用Python进行处理也是一种好的办法。

<!--more-->

这两天看见人力忙着核对绩效，手动工作量十分大，于是我主动提出帮她们减轻负担啦！她们的需求是每月有一批绩效考核文件，需要打开每个核对与汇总绩效成绩是否一致。&nbsp; 
*说明：Python的Excel包有很多，我使用的是xlrd包，地址： https://pypi.python.org/pypi/xlrd/0.9.2 。* 
首先我们要明白excel文件的组织结构：  
> cell:单元格。就是excel表中一个个的小格子，我们在里面输入数据，也可以通过行与列来表示这个小格子的位置； 
> sheet:工作表。我们通常新建一个新的excel，会在excel界面的底部看到有个3个新的工作表-”sheet1,sheet2,sheet3“。一个工作表是由N多个cell单元格组成，他是excel文件的中层结构。 
> Excel文件:office官方文档称呼为”工作簿“，它由多个sheet组成，是excel的最高层。</blockquote> 
> 
> 理解了上面excel组织结构后，我们就能很好的理解excel的相关函数了。 
> 从最简单的一段代码说起： <div class="csharpcode">
>   <pre class="alt"><span class="lnum">   1:  </span>import xlrd #引入包</pre>
>   
>   <pre><span class="lnum">   2:  </span>book = xlrd.open_workbook(<span class="str">"myfile.xls"</span>) </pre>
>   
>   <pre class="alt"><span class="lnum">   3:  </span>#创建一个book <span class="kwrd">class</span>，打开excel文件</pre>
>   
>   <pre><span class="lnum">   4:  </span>print <span class="str">"The number of worksheets is"</span>, book.nsheets </pre>
>   
>   <pre class="alt"><span class="lnum">   5:  </span># nsheets属性记录一个excel文件中有多少个sheet</pre>
>   
>   <pre><span class="lnum">   6:  </span>print <span class="str">"Worksheet name(s):"</span>, book.sheet_names() </pre>
>   
>   <pre class="alt"><span class="lnum">   7:  </span>#返回sheet name的list</pre>
>   
>   <pre><span class="lnum">   8:  </span>sh = book.sheet_by_index(0)   </pre>
>   
>   <pre class="alt"><span class="lnum">   9:  </span>#获取一个sheet对象</pre>
>   
>   <pre><span class="lnum">  10:  </span>print sh.name, sh.nrows, sh.ncols   </pre>
>   
>   <pre class="alt"><span class="lnum">  11:  </span># sheet的属性：name,rows,cols</pre>
>   
>   <pre><span class="lnum">  12:  </span>print <span class="str">"Cell D30 is"</span>, sh.cell_value(rowx=29, colx=3) </pre>
>   
>   <pre class="alt"><span class="lnum">  13:  </span>#定位一个cell</pre>
>   
>   <pre><span class="lnum">  14:  </span><span class="kwrd">for</span> rx <span class="kwrd">in</span> range(sh.nrows):</pre>
>   
>   <pre class="alt"><span class="lnum">  15:  </span>    print sh.row(rx)  #按照row输出excel数据</pre>
>   
>   <pre><span class="lnum">  16:  </span>&nbsp;</pre>
> </div>
> 
> 上面这段代码打开了一个叫”myfile.xls“的文件，使用的方法是xlrd.open_workbook()。我们知道一个excel文件中可以有多个sheet，所以我们可以用book.nsheets属性知道这个文件有多少个sheet.
> 
> 接下来是获取sheet的操作：使用book.sheet\_by\_index(0)获取第一个sheet对象。我们获取sheet对象，可以使用index定位，也可以使用name定位，比如sheet\_by\_name（xxx）。
> 
> 获取完sheet对象后，就可以读取具体的单元格数据啦。按行读取，按列读取等可以按需选择。
> 
> 附：之前需求的处理代码，其实很简单：
> 
> <div class="csharpcode">
>   <pre class="alt"><span class="lnum">   1:  </span>def get_kpi(file):</pre>
>   
>   <pre><span class="lnum">   2:  </span>    book = xlrd.open_workbook(file)    </pre>
>   
>   <pre class="alt"><span class="lnum">   3:  </span>    #print <span class="str">"The number of worksheets is"</span>, book.nsheets</pre>
>   
>   <pre><span class="lnum">   4:  </span>    <span class="kwrd">for</span> shnum <span class="kwrd">in</span> range(0,book.nsheets):</pre>
>   
>   <pre class="alt"><span class="lnum">   5:  </span>        rowx=0</pre>
>   
>   <pre><span class="lnum">   6:  </span>        sh = book.sheet_by_index(shnum)</pre>
>   
>   <pre class="alt"><span class="lnum">   7:  </span>        <span class="kwrd">if</span> re.search(<span class="str">'he'</span>,sh.name)!=None:</pre>
>   
>   <pre><span class="lnum">   8:  </span>            pass</pre>
>   
>   <pre class="alt"><span class="lnum">   9:  </span>        <span class="kwrd">else</span>:</pre>
>   
>   <pre><span class="lnum">  10:  </span>            rowx=sh.col_values(0).index(u<span class="str">'总评得分：'</span>)</pre>
>   
>   <pre class="alt"><span class="lnum">  11:  </span>            <span class="kwrd">for</span> rVaule <span class="kwrd">in</span> sh.row_slice(rowx,1,3):</pre>
>   
>   <pre><span class="lnum">  12:  </span>                <span class="kwrd">if</span> rVaule.ctype==2:</pre>
>   
>   <pre class="alt"><span class="lnum">  13:  </span>                    print sh.name,rVaule.value</pre>
> </div>