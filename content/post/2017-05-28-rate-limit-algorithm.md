---
layout: post
title: '限流:漏桶算法和令牌桶算法'
keywords: 限流, 漏桶算法, 令牌桶算法
date: 2017-05-28 17:50:00
description: '什么是限流，限流有哪些可用算法？'
categories: [技术, 算法]
tags: [算法]
comments: true
group: archive
icon: file-o
---

	本文耗时120分钟，阅读需要15分钟。

----

互联网产品动不动就搞一个活动，虽然现在搞多了，用户活跃度降低了很多，但是依然会在短时间内带来巨大的流量，我们如何让系统在处理高并发的同时还是保证自身系统的稳定，

有人说可以加机器啊，我的架构是分布式的，只需要加机器就好了。但是如果你加机器也不够怎么办？
这个时候我们就必须需要要做业务降级或系统限流了。

怎么做呢？？？

## 什么是限流?

限流，归根结底就是在一定频率上进行量的限制。

一般用来控制服务请求的速率，比如双十一的限流，12306的抢票等等。

比如家里的“保险丝”，还有 Java 线程池，数据库连接池等也是类似的概念， 一旦超过连接池的容量，程序就可以根据预先配置的策略进行处理。

## 两种常见的限速/限流算法

有时人们将漏桶算法与令牌桶算法错误地混淆在一起。而实际上，这两种算法具有截然不同的特性并且为截然不同的目的而使用。

它们之间最主要的差别在于：漏桶算法能够强行限制数据的传输速率，而令牌桶算法能够在限制数据的平均传输速率的同时还允许某种程度的突发传输。

### 令牌桶(Token Bucket)

令牌桶算法通过发放令牌，根据令牌的rate频率做请求频率限制，容量限制等。

令牌桶算法的原理是系统会以一个恒定的速度往桶里放入令牌，而如果请求需要被处理，则需要先从桶里获取一个令牌，当桶里没有令牌可取时，则拒绝服务。

令牌桶算法的基本过程如下:
1. 每秒会有 r 个令牌放入桶中，或者说，每过 1/r 秒桶中增加一个令牌；
2. 桶中最多存放 b 个令牌，如果桶满了，新放入的令牌会被丢弃；
3. 当一个 n 字节的数据包到达时，消耗 n 个令牌，然后发送该数据包；
4. 如果桶中可用令牌小于 n，则该数据包将被缓存或丢弃。

![令牌桶算法示意图](http://oqos7hrvp.bkt.clouddn.com/blog/token-bucket.jpg)

优缺点:
1. 令牌桶的另外一个好处是可以方便的改变速度。 一旦需要提高速率，则按需提高放入桶中的令牌的速率。
1. 可以限制总请求大小，还限制平均频率大小；
2. 还是容易导致误判等问题

优化扩展:

还有些变种算法则实时的计算应该增加的令牌的数量, 比如华为的专利"采用令牌漏桶进行报文限流的方法"(CN 1536815 A),提供了一种动态计算可用令牌数的方法，相比其它定时增加令牌的方法， 它只在收到一个报文后，计算该报文与前一报文到来的时间间隔内向令牌漏桶内注入的令牌数， 并计算判断桶内的令牌数是否满足传送该报文的要求。

### 漏桶(leaky bucket)

漏桶算法可以很好地限制容量池的大小，从而防止流量暴增。

漏桶算法思路很简单，水（请求）先进入到漏桶里，漏桶以一定的速度出水，当水流入速度过大会直接溢出，可以看出漏桶算法能强行限制数据的传输速率。

![漏桶算法示意图](http://oqos7hrvp.bkt.clouddn.com/blog/leaky-bucket.png)

漏桶算法实现上来说，就是建立一个队列，队列上实现FIFO，在请求前实现插入，请求完成后实现删除。

优缺点:
1. 漏桶算法优点很明显，简单、高效，能恰当拦截容量外的暴力流量。
2. 缺点也明显，无法对流量做频率处理。比如:桶大小设置范围内，进行并发攻击依然能产生大流量并发效果，桶容量又不可以设置的过小，否则容易卡死正常用户。

### 漏桶和令牌桶比较

“漏桶算法”能够强行限制数据的传输速率，而“令牌桶算法”在能够限制数据的平均传输数据外，还允许某种程度的突发传输。

在“令牌桶算法”中，只要令牌桶中存在令牌，那么就允许突发地传输数据直到达到用户配置的上限，因此它适合于具有突发特性的流量。

- - - - 

## 参考资料
1. [令牌桶(Token Bucket)](https://en.wikipedia.org/wiki/Token_bucket)
2. [漏桶(Leaky Bucket)](https://en.wikipedia.org/wiki/Leaky_bucket)
3. [接口限流实践](http://www.cnblogs.com/LBSer/p/4083131.html)
4. [流量调整和限流技术](http://colobu.com/2014/11/13/rate-limiting/)

----

**茶歇驿站**

一个可以让你停下来看一看，在茶歇之余给你帮助的小站。

这里的内容主要是后端技术，个人管理，团队管理，以及其他个人杂想。

![茶歇驿站二维码](http://oqos7hrvp.bkt.clouddn.com/blog/tech_tea.jpg)

当然，你觉得对你有帮助，也可以给我打赏。
![打赏](http://oqos7hrvp.bkt.clouddn.com/blog/wxpay.png)