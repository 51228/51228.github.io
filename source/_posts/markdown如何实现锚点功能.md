---
title: markdown如何实现锚点功能
categories:
  - 应用
  - 知识点
tags:
  - markdown
  - 锚点
date: 2021-06-20 10:04:51
update: 2021-06-20 10:04:51
---

之前看了几片文章， markdown 里面设置锚点步骤及用法都比较模糊，经过实验，将markdown锚点方法做如下分享。

<!-- more -->

参考链接：

> https://blog.csdn.net/wangzhibo666/article/details/88731227

> https://blog.csdn.net/weixin_41910694/article/details/91629999

[前言](#qianyan)

[方法](#fangfa)

[参考](#cankao)

## <a id="qianyan">前言</a>

之前看了几片文章， markdown 里面设置锚点步骤及用法都比较模糊，经过实验，将markdown锚点方法做如下分享。

## <span id="fangfa">方法</span>

MarkDown页面内跳转语法

方法一:

- 第一步:在需要跳转到的位置添加锚点，语法如下：

~~~
<span id="jump">跳转到的地方</span>
~~~

- 第二步:在需要点击跳转的位置，使用上面的id，格式类似超链接的形式：

~~~
[点击跳转](#jump)
~~~

方法二:

第一步:在需要跳转到的位置添加锚点，语法如下：

~~~
<a href="#测试2">测试2</a>
~~~

第二步:在需要点击跳转的位置，使用上面的id，格式类似超链接的形式：

~~~
<a id="测试2">测试2</a>
~~~


## <a id="cankao">参考</a>
[Markdown]页面添加锚点,跳到本页指定位置

## 标题语法


```
# 标题

## 目录
1. [目录1](#jump1)
2. [目录2](#jump2)


### <span id="jump1">1. 目录1</span>

### <span id="jump2">2. 目录2</span>

```
