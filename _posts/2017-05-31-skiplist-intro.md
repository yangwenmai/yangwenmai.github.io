---
layout: post
title: 'skiplist-跳跃链表'
keywords: 跳跃链表, skiplist
date: 2017-05-31 12:30:00
description: '跳跃链表'
categories: [数据结构]
tags: [数据结构]
comments: true
group: archive
icon: file-o
---

	本文耗时60分钟，阅读需要5分钟。

----

今天给大家介绍一下跳跃链表。

跳跃列表由 [William Pugh](https://zh.wikipedia.org/w/index.php?title=William_Pugh&action=edit&redlink=1) 发明。他在 [Communications of the ACM](https://zh.wikipedia.org/w/index.php?title=Communications_of_the_ACM&action=edit&redlink=1) June 1990, 33(6) 668-676 发表了Skip lists: a probabilistic alternative to balanced trees，在其中详细描述了他的工作。[参见](http://citeseer.ist.psu.edu/pugh90skip.html) 引用并[下载文档](ftp://ftp.cs.umd.edu/pub/skipLists/)。

引用发明者的话：

`跳跃列表是在很多应用中有可能替代平衡树而作为实现方法的一种数据结构。跳跃列表的算法有同平衡树一样的渐进的预期时间边界，并且更简单、更快速和使用更少的空间。`

    在计算机科学领域，跳跃链表是一种数据结构，允许快速查询一个有序连续元素的数据链表。快速查询是通过维护一个多层次的链表，且每一层链表中的元素是前一层链表元素的子集。 

基于并联的链表，其效率可比拟于二叉查找树（对于大多数操作需要O(log n)平均时间）。

基本上，跳跃列表是对有序的链表增加上附加的前进链接，增加是以随机化的方式进行的，所以在列表中的查找可以快速的跳过部分列表，因此得名。所有操作都以对数随机化的时间进行。

![跳跃表示例图](https://upload.wikimedia.org/wikipedia/commons/thumb/8/86/Skip_list.svg/940px-Skip_list.svg.png)

跳跃列表是按层建造的。底层是一个普通的有序链表。每个更高层都充当下面列表的“快速跑道”，这里在层 i 中的元素按某个固定的概率 p (通常为0.5或0.25)出现在层 i+1 中。平均起来，每个元素都在 1/(1-p) 个列表中出现，而最高层的元素（通常是在跳跃列表前端的一个特殊的头元素）在 O(log1/p n) 个列表中出现。

```shell
1
1-----4---6
1---3-4---6-----9
1-2-3-4-5-6-7-8-9-10
```

要查找一个目标元素，起步于头元素和顶层列表，并沿着每个链表搜索，直到到达小于或着等于目标的最后一个元素。通过跟踪起自目标直到到达在更高列表中出现的元素的反向查找路径，在每个链表中预期的步数显而易见是 1/p。所以查找的总体代价是 O((log1/p n) / p)，当p 是常数时是 O(log n)。通过选择不同 p 值，就可以在查找代价和存储代价之间作出权衡。

插入和删除的实现非常像相应的链表操作，除了"高层"元素必须在多个链表中插入或删除之外。

跳跃列表不像某些传统平衡树数据结构那样提供绝对的最坏情况性能保证，因为用来建造跳跃列表的扔硬币方法总有可能（尽管概率很小）生成一个糟糕的不平衡结构。但是在实际中它工作的很好，随机化平衡方案比在平衡二叉查找树中用的确定性平衡方案容易实现。跳跃列表在并行计算中也很有用，这里的插入可以在跳跃列表不同的部分并行的进行，而不用全局的数据结构重新平衡。

参考链接
1. [跳跃列表](https://zh.wikipedia.org/wiki/跳跃列表)

----

**茶歇驿站**

一个让你可以在茶歇之余，停下来看一看，里面的内容或许对你有一些帮助。

这里的内容主要是团队管理，个人管理，后台技术相关，其他个人杂想。

![茶歇驿站二维码](http://oqos7hrvp.bkt.clouddn.com/blog/tech_tea.jpg)

当然，你觉得对你有帮助，也可以给我打赏。
![打赏](http://oqos7hrvp.bkt.clouddn.com/blog/wxpay.png)