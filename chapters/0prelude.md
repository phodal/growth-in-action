序：如何成为全栈增长工程师？
===

Phodal's Idea实战指南
===

关于作者
---

黄峰达（Phodal Huang）是一个创客、工程师、咨询师和作家。他毕业于西安文理学院电子信息工程专业，现作为一个咨询师就职于 ThoughtWorks 深圳。长期活跃于开源软件社区 GitHub，目前专注于物联网和前端领域。

作为一个开源软件作者，著有 Growth、Stepping、Lan、Echoesworks 等软件。其中开源学习应用 Growth，广受读者和用户好评，可在 APP Store 及各大 Android 应用商店下载。

作为一个技术作者，著有《自己动手设计物联网》（电子工业出版社）、《全栈应用开发：精益实践》（电子工业出版社，正在出版）。并在 GitHub 上开源有《Growth: 全栈增长工程师指南》、《GitHub 漫游指南》等七本电子书。

作为技术专家，他为英国 Packt 出版社审阅有物联网书籍《Learning IoT》、《Smart IoT》，前端书籍《Angular 2 Serices》、《Getting started with Angular》等技术书籍。

他热爱编程、写作、设计、旅行、hacking，你可以从他的个人网站：[https://www.phodal.com/](https://www.phodal.com/) 了解到更多的内容。

其它相关信息：

 - 微博：[http://weibo.com/phodal](http://weibo.com/phodal)
 - GitHub： [https://github.com/phodal](https://github.com/phodal)
 - 知乎：[https://www.zhihu.com/people/phodal](https://www.zhihu.com/people/phodal)
 - SegmentFault：[https://segmentfault.com/u/phodal](https://segmentfault.com/u/phodal)

当前为预览版，在使用的过程中遇到任何问题请及时与我联系。阅读过程中的问题，不妨在GitHub上提出来： [Issues](https://github.com/phodal/fe/issues)

阅读过程中遇到语法错误、拼写错误、技术错误等等，不妨来个Pull Request，这样可以帮助到其他阅读这本电子书的童鞋。

我的电子书：

 * 《[GitHub 漫游指南](https://github.com/phodal/github-roam)》
 * 《[我的职业是前端工程师](https://github.com/phodal/fe)》
 * 《[Serverless 架构应用开发指南](https://github.com/phodal/serverless)》
 * 《[Growth: 全栈增长工程师指南](https://github.com/phodal/growth-ebook)》
 * 《[Phodal's Idea实战指南](https://github.com/phodal/ideabook)》
 * 《[一步步搭建物联网系统](https://github.com/phodal/designiot)》
 * 《[RePractise](https://github.com/phodal/repractise)》
 * 《[Growth: 全栈增长工程师实战](https://github.com/phodal/growth-in-action)》

我的微信公众号:

![作者微信公众号：phodal-weixin](./images/wechat.jpg)

支持作者，可以加入作者的小密圈:

![小密圈](./images/xiaomiquan.jpg)

或者转账：

![支付宝](./images/alipay.png) ![微信](./images/wechat-pay.png)


记得我们在《[RePractise前端篇: 前端演进史](http://mp.weixin.qq.com/s?src=3&timestamp=1463835081&ver=1&signature=z1onJvKn4TSrUmXm384CQUF1IZBVsLShsQ4DpmumN6xY0Gm5RR9XKdbf6ELzdRqg-mxdtxceTg-4-KrhYHZQC6wiSEWsP64vh0sl2Je4G16hnS6MsuZaD-u01HAENCSKoMhQiw0tu2y3-tSJsOML0w==)》中提到技术在最近十几年的飞速发展，当然最主要的就是：技术的复杂度不断地从应用层抽象到了框架层。虽说：

> 技术的复杂度同力一样不会消失，也不会凭空产生，它总是从一个物体转移到另一个物体或一种形式转为另一种形式。

然而这也意味着成为一个全栈工程师，比以往的任何一个时间要容易得多。这也意味着一个全栈工程师也可以很快地成为一个Growth Hacking（中文：增长黑客）。所以，我们开始谈论如何成为一名``全栈增长工程师``。

先成为全栈工程师
---

在电子书《[全栈增长工程师指南](http://mp.weixin.qq.com/s?src=3&timestamp=1463835463&ver=1&signature=z1onJvKn4TSrUmXm384CQUF1IZBVsLShsQ4DpmumN6xzPP-WG-vZxJgzeXdGcPSFn9Erm6laV3FgnEMuiqMnHP0TadjpLl4tYHPhFr-yKWi35U*tGi-RKIdwGc2ylN9bA2Ph*KAl5w5CJRlw2LI9*g==)》中，我们提到过成为全栈增长工程师的技术基础，但是没有并没有谈论到如何成为这样的全栈工程师——这是一个漫长的过程。

早期，当我们有一个想法的时候，我们会去搭建一个网站——如以WordPress作为CMS，以RoR、Django来开发应用等等。随后，我们将我们的网站推向市场，发现市场有点反应。

接着，我们不断地开发出一些新的功能——如CMS的留言、Sitemap等等。在这个过程中，我们会开发一些API来满足我们的需求。

在一个新的阶段里，我们开始推出移动应用。基于先前的API，我们不断地构建出了不同的API。或以单体应用的形式出现，或以微服务的形式产生出新的API。

然后，我们发现并不是所有的移动用户都愿意去下载我们的API。于是，我们推出了SPA（单页面应用），以此来迎接那些移动设备用户。

最后，我们的业务逐渐稳定了下来。我们开始了一些优化工作，或者如Facebook一样优化PHP，推出HHVM。或者如Netflix一样使用微服务解耦系统。又或者，我们使用新的架构对我们的系统进行重新的设计。

在整个过程中，我们将学习到如何去做网站后台、移动应用、API设计、前端单页面应用等等。从这种意义上来说，全栈工程师非常match初创企业所需要的技术要求。

再成为增长工程师
---

Growth整一个系列：APP、社区、电子书《全栈增长工程师指南》、电子书《全栈增长工程师实战》算是我对Growth Hacking的一个研究。不过，对于一个人来说这工作量还是蛮大的——在完成两本电子书后，我们将继续研究。在这一个过程中，我发现一些很有意思的东西——只有开发出用户想要的东西，这个过程才容易实践起来的。

增长可以分为两部分：一个是自身的增长，一个是用户的增长。两者实际上是一种相互促进的关系，当我们的能力增长到一定的程度，我们才能推进用户的增长。相用户增长到一定的程度，也会推进我们的技能增长。

只是要在技术、数据分析、用户分析、创新等等有所突破，看上去好像不是一件容易的事。只是对于大部分的全栈工程师来说，实现技术、数据抓取和分析是一件容易的事。要实现对数据的敏感是一种很难的事，但是可视化过后的数据就一样了。对于用户的行为分析也是类似的，只是因为我们缺乏一些有效的练习。

更让人惊讶的是创新也是可以练习的，每次我们遇到一个问题的时候，就是我们离创新最近的时候——难道不是吗？当你遇到一个难解的问题，就是你开拓一个新的能力的时候。

好好享受这个学习的过程吧！



