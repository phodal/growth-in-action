[全栈增长工程师](https://github.com/phodal/growth-ebook)实战
===

在Growth中我们介绍的只是一系列的实践，而Growth实战则会带领读者去履行这些实践。你将会看到：

 - 如何开发一个Web应用（博客）
 - 如何编写测试——单元测试、功能测试、自动化UI测试
 - 搭建并使用持续集成
 - 添加SEO支持——Sitemap、站长工具和Google Analytics
 - 创建API，制作AutoComplete
 - 开发相应的APP及其API——查看文章、用户登录、发表文章
 - 制作单页面应用
 - 可配置管理

下载地址: [https://github.com/phodal/growth-in-action/releases](https://github.com/phodal/growth-in-action/releases)

Web应用代码: [https://github.com/phodal/growth-in-action-python-code](https://github.com/phodal/growth-in-action-python-code)

APP代码: [https://github.com/phodal/growth-in-action-hybird](https://github.com/phodal/growth-in-action-hybird)

欢迎关注我的微信公众号(搜索Phodal)：

![QRCode](https://raw.githubusercontent.com/phodal/growth/master/www/img/wechat.jpg)


目录
---

*   [序：如何成为全栈增长工程师？](http://growth-in-action.phodal.com/#序如何成为全栈增长工程师)
    *   [先成为全栈工程师](http://growth-in-action.phodal.com/#先成为全栈工程师)
    *   [再成为增长工程师](http://growth-in-action.phodal.com/#再成为增长工程师)
*   [全栈增长工程师实战](http://growth-in-action.phodal.com/#全栈增长工程师实战)
    *   [准备工作和工具](http://growth-in-action.phodal.com/#准备工作和工具)
*   [深入浅出Django](http://growth-in-action.phodal.com/#深入浅出django)
    *   [Django简介](http://growth-in-action.phodal.com/#django简介)
        *   [Django应用架构](http://growth-in-action.phodal.com/#django应用架构)
    *   [Django hello,world](http://growth-in-action.phodal.com/#django-helloworld)
        *   [安装Django](http://growth-in-action.phodal.com/#安装django)
        *   [创建项目](http://growth-in-action.phodal.com/#创建项目)
        *   [Django后台](http://growth-in-action.phodal.com/#django后台)
        *   [第一次提交](http://growth-in-action.phodal.com/#第一次提交)
*   [Django创建博客应用](http://growth-in-action.phodal.com/#django创建博客应用)
    *   [Tasking](http://growth-in-action.phodal.com/#tasking)
    *   [创建BlogpostAPP](http://growth-in-action.phodal.com/#创建blogpostapp)
        *   [生成APP](http://growth-in-action.phodal.com/#生成app)
        *   [创建Model](http://growth-in-action.phodal.com/#创建model)
        *   [配置URL](http://growth-in-action.phodal.com/#配置url)
    *   [创建View](http://growth-in-action.phodal.com/#创建view)
        *   [创建博客列表页](http://growth-in-action.phodal.com/#创建博客列表页)
        *   [创建博客详情页](http://growth-in-action.phodal.com/#创建博客详情页)
    *   [测试](http://growth-in-action.phodal.com/#测试)
        *   [测试首页](http://growth-in-action.phodal.com/#测试首页)
        *   [测试详情页](http://growth-in-action.phodal.com/#测试详情页)
*   [功能测试与持续集成](http://growth-in-action.phodal.com/#功能测试与持续集成)
    *   [编写自动化测试](http://growth-in-action.phodal.com/#编写自动化测试)
        *   [Selenium与第一个UI测试](http://growth-in-action.phodal.com/#selenium与第一个ui测试)
    *   [搭建持续集成](http://growth-in-action.phodal.com/#搭建持续集成)
        *   [Jenkins创建任务](http://growth-in-action.phodal.com/#jenkins创建任务)
        *   [创建shell](http://growth-in-action.phodal.com/#创建shell)
*   [更多功能](http://growth-in-action.phodal.com/#更多功能)
    *   [静态页面](http://growth-in-action.phodal.com/#静态页面)
        *   [安装 flatpages](http://growth-in-action.phodal.com/#安装-flatpages)
        *   [创建模板](http://growth-in-action.phodal.com/#创建模板)
    *   [评论功能](http://growth-in-action.phodal.com/#评论功能)
    *   [Sitemap](http://growth-in-action.phodal.com/#sitemap)
        *   [站点地图介绍](http://growth-in-action.phodal.com/#站点地图介绍)
        *   [创建首页的Sitemap](http://growth-in-action.phodal.com/#创建首页的sitemap)
        *   [创建静态页面的Sitemap](http://growth-in-action.phodal.com/#创建静态页面的sitemap)
        *   [创建博客的Sitemap](http://growth-in-action.phodal.com/#创建博客的sitemap)
        *   [提交到搜索引擎](http://growth-in-action.phodal.com/#提交到搜索引擎)
*   [前端框架](http://growth-in-action.phodal.com/#前端框架)
    *   [响应式设计](http://growth-in-action.phodal.com/#响应式设计)
        *   [引入前端框架](http://growth-in-action.phodal.com/#引入前端框架)
    *   [页面美化](http://growth-in-action.phodal.com/#页面美化)
        *   [添加导航](http://growth-in-action.phodal.com/#添加导航)
        *   [添加标语](http://growth-in-action.phodal.com/#添加标语)
        *   [优化列表](http://growth-in-action.phodal.com/#优化列表)
        *   [添加footer](http://growth-in-action.phodal.com/#添加footer)
*   [API](http://growth-in-action.phodal.com/#api)
    *   [博客列表](http://growth-in-action.phodal.com/#博客列表)
        *   [Django REST Framework](http://growth-in-action.phodal.com/#django-rest-framework)
        *   [创建博客列表API](http://growth-in-action.phodal.com/#创建博客列表api)
        *   [测试 API](http://growth-in-action.phodal.com/#测试-api)
    *   [自动完成](http://growth-in-action.phodal.com/#自动完成)
        *   [搜索API](http://growth-in-action.phodal.com/#搜索api)
        *   [页面实现](http://growth-in-action.phodal.com/#页面实现)
    *   [跨域支持](http://growth-in-action.phodal.com/#跨域支持)
        *   [添加跨域支持](http://growth-in-action.phodal.com/#添加跨域支持)
*   [移动应用](http://growth-in-action.phodal.com/#移动应用)
    *   [hello,world](http://growth-in-action.phodal.com/#helloworld)
        *   [构建应用](http://growth-in-action.phodal.com/#构建应用)
    *   [博客列表页](http://growth-in-action.phodal.com/#博客列表页)
        *   [列表页](http://growth-in-action.phodal.com/#列表页)
        *   [详情页](http://growth-in-action.phodal.com/#详情页)
    *   [Profile](http://growth-in-action.phodal.com/#profile)
        *   [Json Web Tokens](http://growth-in-action.phodal.com/#json-web-tokens)
        *   [登录表单](http://growth-in-action.phodal.com/#登录表单)
        *   [Profile](http://growth-in-action.phodal.com/#profile-1)
    *   [创建博客](http://growth-in-action.phodal.com/#创建博客)
*   [Mobile Web](http://growth-in-action.phodal.com/#mobile-web)
    *   [移动设备处理](http://growth-in-action.phodal.com/#移动设备处理)
    *   [前后端分离](http://growth-in-action.phodal.com/#前后端分离)
        *   [Riot.js](http://growth-in-action.phodal.com/#riot.js)
        *   [ReactiveJS构建服务](http://growth-in-action.phodal.com/#reactivejs构建服务)
        *   [创建博客列表页](http://growth-in-action.phodal.com/#创建博客列表页-1)
        *   [博客详情页](http://growth-in-action.phodal.com/#博客详情页)
        *   [添加导航](http://growth-in-action.phodal.com/#添加导航-1)
*   [配置管理](http://growth-in-action.phodal.com/#配置管理)
    *   [local settings](http://growth-in-action.phodal.com/#local-settings)
    
License
---

[![Phodal's Article](http://brand.phodal.com/shields/article-small.svg)](https://www.phodal.com/)

© 2015~2016 [Phodal Huang](https://www.phodal.com). This code is distributed under the Creative Commons Attribution-Noncommercial-No Derivative Works 3.0  License. See `LICENSE` in this directory.

[待我代码编成，娶你为妻可好](http://www.xuntayizhan.com/blog/ji-ke-ai-qing-zhi-er-shi-dai-wo-dai-ma-bian-cheng-qu-ni-wei-qi-ke-hao-wan/)
