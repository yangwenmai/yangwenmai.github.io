---
layout: post
title: '解决ios5.0.1之苹果推送APNs失败'
date: 2014-01-01 16:15:00
comments: true
categories: [push]
tags: [Apple, iOS, APNs, 苹果]
---

说来也巧，我在去年的最后一天晚上（2013年12月31日），成功的修复了一个强劲的bug--小恩爱应用在iOS5.0.1设备无法成功进行消息推送。

不知道这是上天给我一次机会，还是因为我的孜孜不倦的努力，感动了上天！！！或许“两者皆有吧”。现在来看这次重构是价值连城的，不管重构的结果如何，至少现在我们已经因此而获益了，解决了可能已为期一年的bug。

同事们都对我的发现欢呼雀跃，也对我表示赞许和膜拜。对此我也非常开心，毕竟这样我们就能够让这部分的小恩爱用户能正常使用到我们的消息推送服务了。我们整个团队必将加倍努力，服务好每一对情侣爱人。

一通闲扯之后，我们现在还是进入正题吧~O(∩_∩)O

**iOS5.0.1苹果推送APNs失败**
- 现象： 自己搭建的苹果推送APNs后台服务器显示发送成功，并且也无任何错误提示反馈，但是推送的苹果手机未收取到任何推送相关的消息
- 排查： 根据其他正常可推送的消息，分析payload参数的不同，发现是自定义参数有所不同，所以自己尝试了不同的自定义参数名称以及值，最后很明显的得出一个结论就是，当我的自定义参数值为负值时，能够重现此问题。
- 根本原因是自定义字典(自定义参数)中，我们所传递的参数是负值。

根据测试的结论，跟CTO讨论得知，原来此问题我们一直都未解决，所以我就这样误打误撞的解决了公司尘封已久的死bug了。高兴之余大家也在感叹，没想到修复bug也是要讲究缘分的啊~~~
