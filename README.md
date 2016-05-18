[全栈增长工程师](https://github.com/phodal/growth-ebook)实战
===

在Growth中我们介绍的只是一系列的实践，而Growth实战则会带领读者去履行这些实践。

你将会看到：

 - 如何去开发一个Web应用（博客）
 - 如何编写单元测试、功能测试、自动化UI测试
 - 搭建并使用持续集成
 - 添加SEO支持——Sitemap、站长工具和Google Analytics
 - 使用API，制作AutoComplete
 - 开发相应的APP及其API——查看文章、用户登录、发表文章
 - 制作单页面应用
 - 自动化部署

欢迎关注我的微信公众号(搜索Phodal)：

![QRCode](https://raw.githubusercontent.com/phodal/growth/master/www/img/wechat.jpg)

Web应用代码: [https://github.com/phodal/growth-in-action-django-code](https://github.com/phodal/growth-in-action-django-code)

APP代码: [https://github.com/phodal/growth-in-action-hybird](https://github.com/phodal/growth-in-action-hybird)

目录
---
*   [Growth In Action Django](#growth-in-action-django)
    *   [准备工作和工具](#准备工作和工具)
*   [深入浅出Django](#深入浅出django)
    *   [Django简介](#django简介)
        *   [Django应用架构](#django应用架构)
    *   [Django hello,world](#django-helloworld)
        *   [安装Django](#安装django)
        *   [创建项目](#创建项目)
        *   [Django后台](#django后台)
        *   [第一次提交](#第一次提交)
*   [Django创建博客应用](#django创建博客应用)
    *   [Tasking](#tasking)
    *   [创建BlogpostAPP](#创建blogpostapp)
        *   [生成APP](#生成app)
        *   [创建Model](#创建model)
        *   [配置URL](#配置url)
    *   [创建View](#创建view)
        *   [创建博客列表页](#创建博客列表页)
        *   [创建博客详情页](#创建博客详情页)
    *   [测试](#测试)
        *   [测试首页](#测试首页)
        *   [测试详情页](#测试详情页)
*   [功能测试与持续集成](#功能测试与持续集成)
    *   [编写自动化测试](#编写自动化测试)
        *   [Selenium与第一个UI测试](#selenium与第一个ui测试)
    *   [搭建持续集成](#搭建持续集成)
        *   [Jenkins创建任务](#jenkins创建任务)
        *   [创建shell](#创建shell)
*   [更多功能](#更多功能)
    *   [静态页面](#静态页面)
        *   [安装 flatpages](#安装-flatpages)
        *   [创建模板](#创建模板)
    *   [评论功能](#评论功能)
    *   [Sitemap](#sitemap)
        *   [站点地图介绍](#站点地图介绍)
        *   [创建首页的Sitemap](#创建首页的sitemap)
        *   [创建静态页面的Sitemap](#创建静态页面的sitemap)
        *   [创建博客的Sitemap](#创建博客的sitemap)
        *   [提交到搜索引擎](#提交到搜索引擎)
*   [前端框架](#前端框架)
    *   [技能选型](#技能选型)
    *   [页面美化](#页面美化)
        *   [添加导航](#添加导航)
        *   [添加标语](#添加标语)
        *   [优化列表](#优化列表)
*   [API](#api)
    *   [自动完成](#自动完成)
    *   [RESTful](#restful)
        *   [Django REST Framework](#django-rest-framework)
    *   [跨域](#跨域)
        *   [CORS](#cors)
        *   [添加跨域支持](#添加跨域支持)
*   [移动应用](#移动应用)
    *   [hello,world](#helloworld)
        *   [构建应用](#构建应用)
    *   [博客列表页](#博客列表页)
        *   [列表页](#列表页)
        *   [详情页](#详情页)
    *   [Profile](#profile)
        *   [Json Web Tokens](#json-web-tokens)
        *   [Profile](#profile-1)
    *   [创建博客](#创建博客)
    *   [TODO](#todo)
*   [前端重构](#前端重构)
    *   [MVVM](#mvvm)
    *   [UX](#ux)
*   [部署](#部署)
    *   [配置管理](#配置管理)
    *   [Fabric](#fabric)
    
License
---

[![Phodal's Article](http://brand.phodal.com/shields/article-small.svg)](https://www.phodal.com/)

© 2015~2016 [Phodal Huang](https://www.phodal.com). This code is distributed under the Creative Commons Attribution-Noncommercial-No Derivative Works 3.0  License. See `LICENSE` in this directory.

[待我代码编成，娶你为妻可好](http://www.xuntayizhan.com/blog/ji-ke-ai-qing-zhi-er-shi-dai-wo-dai-ma-bian-cheng-qu-ni-wei-qi-ke-hao-wan/)
