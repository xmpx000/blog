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

*具体的官方文档解释，见[此处](http://pandas.pydata.org/pandas-docs/version/0.15.2/indexing.html#indexing-view-versus-copy)*


### 示例
以下示例均在ipython中操作：

    df= DataFrame({'A' : ['foo', 'bar', 'foo', 'bar',
                          'foo', 'bar', 'foo', 'foo'],
                    'B' : ['one', 'one', 'two', 'three',
                          'two', 'two', 'one', 'three'],
                     'C' : np.random.randn(8), 'D' : np.random.randn(8)})
    df

 输出：
 
 
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
      <th>C</th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>foo</td>
      <td>one</td>
      <td>0.149304</td>
      <td>0.242362</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bar</td>
      <td>one</td>
      <td>0.665781</td>
      <td>0.382197</td>
    </tr>
    <tr>
      <th>2</th>
      <td>foo</td>
      <td>two</td>
      <td>0.266864</td>
      <td>-0.846936</td>
    </tr>
    <tr>
      <th>3</th>
      <td>bar</td>
      <td>three</td>
      <td>-1.456214</td>
      <td>0.600248</td>
    </tr>
    <tr>
      <th>4</th>
      <td>foo</td>
      <td>two</td>
      <td>-1.137853</td>
      <td>-1.007400</td>
    </tr>
    <tr>
      <th>5</th>
      <td>bar</td>
      <td>two</td>
      <td>2.312738</td>
      <td>-0.528008</td>
    </tr>
    <tr>
      <th>6</th>
      <td>foo</td>
      <td>one</td>
      <td>-1.069947</td>
      <td>0.420484</td>
    </tr>
    <tr>
      <th>7</th>
      <td>foo</td>
      <td>three</td>
      <td>0.786922</td>
      <td>-0.956687</td>
    </tr>
  </tbody>
</table>

--------------------------------
<p></p>
输入：

    df.loc[3]

输出：

    A          bar
    B        three
    C    -1.456214
    D    0.6002484
    Name: 3, dtype: object

-----------------
<p></p>
输入：

    df.loc[0:2,('A','B')]

输出


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>foo</td>
      <td>one</td>
    </tr>
    <tr>
      <th>1</th>
      <td>bar</td>
      <td>one</td>
    </tr>
    <tr>
      <th>2</th>
      <td>foo</td>
      <td>two</td>
    </tr>
  </tbody>
</table>
--------------------------------
<p></p>
输入：

    
    df.iloc[:1,3:]

输出：


<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>D</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.242362</td>
    </tr>
  </tbody>
</table>

--------------------------------
<p></p>
输入：
    
    df.iloc[2]>0.5
    
    
输出：

    A     True
    B     True
    C    False
    D    False
    Name: 2, dtype: bool

--------------------------------
<p></p>
输入：
    
    #使用boolean数组，将结果为True的label返回。
    df.loc[1:3,df.iloc[2]>0.5]

输出

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>A</th>
      <th>B</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>bar</td>
      <td>one</td>
    </tr>
    <tr>
      <th>2</th>
      <td>foo</td>
      <td>two</td>
    </tr>
    <tr>
      <th>3</th>
      <td>bar</td>
      <td>three</td>
    </tr>
  </tbody>
</table>


<p></p>
<p></p>
<p></p>

[It'web]:    http://itweb.me  "It’web"
