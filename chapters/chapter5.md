前端框架
===

我们的前端样式实在是太丑了，让我们想办法来美化一下它们吧——这时候我们就需要一个前端框架来帮助我们做这件事。这里的前端框架并不是指那种MV*框架，而是UI框架。

响应式设计
---

考虑到易学程度，以其响应式设计的问题，我们决定用Bootstrap来作为这里的前端框架。Bootstrap是Twitter推出的一个用于前端开发的开源工具包，似乎也是当前“最受欢迎”的前端框架。它提供了全面、美观的文档。你能在这里找到关于 HTML 元素、HTML 和 CSS 组件、jQuery 插件方面的所有详细文档。并且我们能在 Bootstrap 的帮助下通过同一份代码快速、有效适配手机、平板、PC 设备。

它是一个支持响应式设计的框架，即页面的设计与开发应当根据用户行为以及设备环境(系统平台、屏幕尺寸、屏幕定向等)进行相应的响应和调整。如下图所示：

![响应式设计](images/responsive-design.png)

我们在不同的设计上看到的是不是同的布局，这会依据我们的设备大小做出调整——使用媒体查询(media queries)实现。

页面美化
---

### 添加导航

```html
<header class="navbar navbar-static-top bs-docs-nav" id="top" role="banner">
    <div class="container">
        <div class="navbar-header">
            <button class="navbar-toggle collapsed" type="button" data-toggle="collapse"
                    data-target=".bs-navbar-collapse">
                <span class="sr-only">切换视图</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a href="/" class="navbar-brand">Growth博客</a>
        </div>
        <nav class="collapse navbar-collapse bs-navbar-collapse" role="navigation">
            <ul class="nav navbar-nav">
                <li>
                    <a href="/pages/about/">关于我</a>
                </li>
                <li>
                    <a href="/pages/resume/">简历</a>
                </li>
            </ul>
            <ul class="nav navbar-nav navbar-right">
                <li><a href="/admin" id="loginLink">登入</a></li>
            </ul>

        </nav>
    </div>
</header>
```

### 添加标语

```
<main class="bs-docs-masthead" id="content" role="main">
    <div class="container">
        <div id="carbonads-container">
            THE ONLY FAIR IS NOT FAIR <br>
            ENJOY CREATE & SHARE
        </div>
    </div>
</main>
```

### 优化列表

```
{% extends 'base.html' %}
{% block title %}Welcome to my blog{% endblock %}

{% block content %}
<h2>博客</h2>
<div class="row">
    {% if posts %}
    {% for post in posts %}
    <div class="col-sm-4">
        <h2><a href="{{ post.get_absolute_url }}">{{ post.title }}</a></h2>
        {{post.body | slice:":80"}}
        {{post.posted}} - By {{post.author}}
    </div>
    {% endfor %}
    {% else %}
    <p>There are no posts.</p>
    {% endif %}
</div>
{% endblock %}
```
