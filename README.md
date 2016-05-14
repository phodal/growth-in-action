Growth In Action Django
===

> 这本书是[全栈增长工程师指南](https://github.com/phodal/growth-ebook)的Python(Django)实战版。

在Growth中我们介绍的只是一系列的实践，而Growth实战则会带领读者去履行这些实践。

你将会看到：

 - 如何去开发一个Web应用（博客）
 - 如何编写单元测试
 - 如何编写功能测试、自动化UI测试
 - 搭建并使用持续集成
 - 添加SEO支持
 - 支持APP使用的API
 - 开发相应的APP——登录、注册、发现文章、查看文章
 - 添加单页面应用的前端
 - 自动化部署

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
*   [功能测试与搭建持续集成](#功能测试与搭建持续集成)
    *   [编写自动化测试](#编写自动化测试)
        *   [Selenium与第一个UI测试](#selenium与第一个ui测试)
    *   [搭建持续集成](#搭建持续集成)
        *   [Jenkins创建任务](#jenkins创建任务)
        *   [创建shell](#创建shell)
*   [更多功能](#更多功能)
    *   [评论功能](#评论功能)
    *   [Sitemap与SEO](#sitemap与seo)
        *   [SEO](#seo)
        *   [Monthly](#monthly)
*   [前端框架](#前端框架)
    *   [技能选型](#技能选型)
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

© 2015~2016 [Phodal Huang](https://www.phodal.com). This code is distributed under the MIT LICENSE in this directory.

[待我代码编成，娶你为妻可好](http://www.xuntayizhan.com/person/ji-ke-ai-qing-zhi-er-shi-dai-wo-dai-ma-bian-cheng-qu-ni-wei-qi-ke-hao-wan/)
