更多功能
===

在Django框架中，内置了很多应用在它的"contrib"包中，这些包括：

 - 一个可扩展的认证系统
 - 动态站点管理页面
 - 一组产生RSS和Atom的工具
 - 一个灵活的评论系统
 - 产生Google站点地图（Google Sitemaps）的工具
 - 防止跨站请求伪造（cross-site request forgery）的工具
 - 一套支持轻量级标记语言（Textile和Markdown）的模板库
 - 一套协助创建地理信息系统（GIS）的基础框架

这意味着，我们可以直接用Django一些内置的组件来完成很多功能，先让我们来看看怎么完成一个简单的评论功能。

评论功能
---

在早期的Django版本(1.6以前)中，Comments是自带的组件，但是后来它被从标准组件中移除了。

```
pip install django-contrib-comments
```

添加到``requirements.txt``:

```
django==1.9.4
selenium==2.53.1
fabric==1.10.2
djangorestframework==3.3.3
djangorestframework-jwt==1.7.2
django-cors-headers==1.1.0
django-contrib-comments==1.7.1
```

添加到``INSTALLED_APPS``，如下:

```python
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django.contrib.sites',
    'django_comments',
    'rest_framework',
    'blogpost'
)
```

```
Operations to perform:
  Apply all migrations: contenttypes, admin, blogpost, auth, sites, sessions, django_comments
Running migrations:
  Rendering model states... DONE
  Applying sites.0001_initial... OK
  Applying django_comments.0001_initial... OK
  Applying django_comments.0002_update_user_email_field_length... OK
  Applying django_comments.0003_add_submit_date_index... OK
  Applying sites.0002_alter_domain_unique... OK
(growth-django)
```

添加urls.py:

```
url(r'^comments/', include('django_comments.urls')),
```

```
{% extends 'base.html' %}
{% load comments %}

{% block head_title %}{{ post.title }}{% endblock %}
{% block title %}{{ post.title }}{% endblock %}

{% block content %}
<div class="mdl-card mdl-shadow--2dp">
    <div class="mdl-card__title">
        <h2 class="mdl-card__title-text"><a href="{{ post.get_absolute_url }}">{{ post.title }}</a></h2>
    </div>
    <div class="mdl-card__supporting-text">
        {{post.body}}
    </div>
    <div class="mdl-card__actions">
        {{post.posted}} - By {{post.author}}
    </div>
</div>

{% render_comment_list for post %}

{% endblock %}
```

报错：

![site_id_issue.jpg](images/site_id_issue.jpg)

定义``SITE_ID``

```python
SITE_ID = 1
```

从后台创建:

![create-comment-backend.jpg](images/create-comment-backend.jpg)

Sitemap与SEO
---

###SEO

###Monthly
