---
layout: post
title: Pandas：数据选择
description:  Pandas：数据选择方法
category: blog
---

### 介绍

pandas主要有2种数据选择选择格式：基于label的`.loc`与基于位置的`.iloc`。

`.loc`主要基于label,当所选择的数据查找不到时，`.loc`将抛出KeyError。`.loc`在选择时，格式为`loc`[行,列],行标签在前，列标签在后。

#### `.loc`支持4种方式选择数据：
1. 单一的标签，比如'a'或'5';注意此处的'5'是索引中的数字标签，而不是位置；
2. 标签数组['a','b','c']；
3. 切片：'a':'f'。**注意此处逻辑与python切片有差异，它会将开始的a与结束的f都包含进去**；
4. boolean数组；


`.iloc`是基于位置进行查找(从0到length-1)，当所选择的数据查找不到时，`.iloc`将抛出IndexErrro。`.iloc`在选择时，格式为`iloc`[行,列],行位置在前，列位置在后。
#### `.iloc`支持四种方式选择数据：
1. 单一的位置数字，比如5；
2. 数字数组：[1,3,5]
3. 切片：比如1:7。**注意此处逻辑与python切片有差异，它会将开始的a与结束的f都包含进去**；
4. boolean数组


### 注意事项：
对比以下两种取值方法：

    1.df['B']['C']
    2.df_g.loc[:,['B','C']]

方法一会导致返回的数据是一份copy，你对它进行操作，比如赋值：`df['B']['C']=0`，可能不会对dataframe造成影响，并会提示：SettingWithCopyWarning。所以推荐使用方法二进行取值。

*具体的官方文档解释，见:http://pandas.pydata.org/pandas-docs/version/0.15.2/indexing.html#indexing-view-versus-copy*


### 示例
以下示例均在ipython中操作：

    df= DataFrame({'A' : ['foo', 'bar', 'foo', 'bar',
                          'foo', 'bar', 'foo', 'foo'],
                    'B' : ['one', 'one', 'two', 'three',
                          'two', 'two', 'one', 'three'],
                     'C' : np.random.randn(8), 'D' : np.random.randn(8)})
    df

 输出：
|-|A|B|C|D|
|---|---|---|---|---|
|0|foo|one|1.065729|1.681778|
|1|bar|one|0.240644|0.717240|
|2|foo|two|0.844402|-1.756049|
|3|bar|three|-1.098014|-0.080866|
|4|foo|two|-1.110899|-0.469254|
|5|bar|two|0.739063|1.727154|
|6|foo|one|0.087585|0.554248|
|7|foo|three|-0.558808|0.000000|


    df.loc[3]
输出：

    A           bar
    B         three
    C     -1.098014
    D   -0.08086613
    Name: 3, dtype: object

-----------------

    df_g.loc[0:2,('A','B')]
输出
|-|A|B|
|---|---|
|0|foo|one|
|1|bar|one|
|2|foo|two|


-----------------
    df.iloc[:1,3:]

输出：
-|D
:-----|:-----
0|	1.681778


----------------------
    df_g.iloc[2]>0.5
输出：

    A     True
    B     True
    C     True
    D    False
    Name: 2, dtype: bool

--------------------------------
    #使用boolean数组，将结果为True的label返回。
    df_g.loc[1:3,df_g.iloc[2]>0.5]
输出
|A|B|C|
|---|---|---|
|1|bar|one|0.240644|
|2|foo|two|0.844402|
|3|bar|three|-1.098014|


值|描述 
:---------------|:---------------
baseline|默认元素放置在父元素的基线上
sub|垂直对齐文本的下标
super|垂直对齐文本的上标
top|把元素的顶端与行中最高元素的顶端对齐
text-top|把元素的顶端与父元素字体的顶端对齐
middle|把此元素放置在父元素的中部
bottom|把元素的顶端与行中最低的元素的顶端对齐
text-bottom|把元素的底端与父元素字体的底端对齐
length|相对基准线的偏移
%|使用 "line-height" 属性的百分比值来排列此元素允许使用负值
inherit|规定应该从父元素继承 vertical-align 属性的值*（所有的IE都不支持？！）*


[It'web]:    http://itweb.me  "It’web"
