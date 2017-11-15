更完善的博客系统
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


静态页面
---

Django带有一个可选的“flatpages”应用，可以让我们存储简单的“扁平化(flat)”页面在数据库中，并且可以通过Django的管理界面以及一个Python API来处理要管理的内容。这样的一个静态页面，一般包含下面的几个属性：

 - 标题
 - URL
 - 内容(Content)
 - Sites
 - 自定义模板（可选）

为了使用它来创建静态页面，我们需要在数据库中存储对应的映射关系，并创建对应的静态页面。 

### 安装 flatpages

为此我们需要添加两个应用到``settings.py``文件的``INSTALLED_APPS``中：

 - ``django.contrib.sites``——“sites”框架，它用于将对象和功能与特定的站点关联。同时，它还是域名和你的Django 站点名称之间的对应关系所保存的位置，即我们需要在这个地方设置我们的网站的域名。
 - ``django.contrib.flatpages``，即上文说到的内容。

在添加``django.contrib.sites``的时候，我们需要创建一个``SITE_ID``。通过这个值等于1，除非我们打算用这个框架去管理多个站点。代码如下所示：

```
SITE_ID = 1

INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django.contrib.sites',
    'django.contrib.flatpages',
    'blogpost'
)
```

接着，还添加对应的中间件``django.contrib.flatpages.middleware.FlatpageFallbackMiddleware``到``settings.py``文件的``MIDDLEWARE_CLASSES``中。

然后，我们需要创建对应的URL来管理所有的静态页面。下面的代码是将静态页面都放在pages路径下，即如果我们有一个about的页面，那么对应的URL会变成 http://localhost/pages/about/。

```python
url(r'^pages/', include('django.contrib.flatpages.urls')),
```

当然我们也可以将其配置为类似于 http://localhost/about/ 这样的URL：

```
urlpatterns += [
    url(r'^(?P<url>.*/)$', views.flatpage),
]
```

最后，我们还需要做一个数据库迁移：

```
Operations to perform:
  Apply all migrations: contenttypes, auth, admin, sites, blogpost, sessions, flatpages, django_comments
Running migrations:
  Rendering model states... DONE
  Applying flatpages.0001_initial... OK
```

### 创建模板

接着，我们可以在``templates``目录下创建``flatpages``文件，用于存放我们的模板文件，下面是一个简单的模板：

```
{% extends 'base.html' %}
{% block title %}关于我{% endblock %}

{% block content %}
<div>
<h2>关于博客</h2>
    <p>一方面，找到更多志同道合的人；另一方面，扩大影响力。</p>
    <p>内容包括</p>
    <ul>
        <li>成长记录</li>
        <li>技术笔记</li>
        <li>生活思考</li>
        <li>个人试验</li>
    </ul>
</div>
{% endblock %}
```

当我们完成模板后，我们就需要登录后台，并添加对应的静态页面的配置：

![管理员界面创建flatpage](./images/admin-flatpages-create.jpg)

然后从高级选项中填写我们的静态页面的路径，我们就可以完成静态页面的创建。如下图所示：

![flatpage高级选项](./images/flatpages-advance-option.png)

最后，还要有个链接加到首页的导航中：

```html
<li>
    <a href="/pages/about/">关于我</a>
</li>
```                

下面让我们为我们的博客添加一个简单的评论功能吧！

评论功能
---

在早期的Django版本(1.6以前)中，Comments是自带的组件，但是后来它被从标准组件中移除了。因此，我们需要安装comments这个包：

```
pip install django-contrib-comments
```

再把它及它的版本添加到``requirements.txt``，如下所示：

```
django==1.9.4
selenium==2.53.1
fabric==1.10.2
djangorestframework==3.3.3
djangorestframework-jwt==1.7.2
django-cors-headers==1.1.0
django-contrib-comments==1.7.1
```

接着，将``django.contrib.sites``和``django_comments``添加到``INSTALLED_APPS``，如下:

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

然后做一下数据库迁移我们就可以完成对其的初始化：

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

然后再添加URL到urls.py:

```
url(r'^comments/', include('django_comments.urls')),
```

现在，我们就可以登录后台，来创建对应的评论，但是这是时候评论是不会显示到页面上的。所以我们需要对我们的博客详情页的模板进行修改，在其中添加一句:

```html
{% render_comment_list for post %}
```

用于显示对应博客的评论，最近我们的模板文件如下面的内容所示：

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

遗憾的是，当我们刷新页面的时候，页面报错了，原因如下所示：

![SITE_ID报错](./images/site_id_issue.jpg)

我们还需要定义一个``SITE_ID``，添加下面的代码到``settings.py``文件中即可：

```python
SITE_ID = 1
```

然后，我们就可以从后台创建评论：

![后台创建评论](./images/create-comment-backend.jpg)

Sitemap
---

我们在之前的文章中提到过SEO的重要性，这里只是简单地对Sitemap的内容进行展开。

### 站点地图介绍

Sitemap译为站点地图，它用于告诉搜索引擎他们网站上有哪些可供抓取的网页。常见的Sitemap的形式是以xml出现了，如下是我博客的sitemap.xml的一部分内容：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
<url>
  <loc>https://www.phodal.com/blog/mezzanine-add-new-page/</loc>
  <lastmod>2014-08-03</lastmod>
  <changefreq>Monthly</changefreq>
  <priority>0.2</priority>
</url>
</urlset>
```

从上面的内容中，我们可以发现它包含了下面的一些XML标签：

 - urlset，封装该文件，并指明当前协议的标准。
 - url，每个URL实体的父标签。
 - loc，指明页面的URL
 - lastmod（可选），内容最后的修改时间
 - changefreq（可选），内容的修改频率，用于告知搜索引擎抓取频率。它包含的值有：``always``、``hourly``、``daily``、``weekly``、``monthly``、``yearly``、``never``
 - priority（可选），范围是从0.0~1.0，搜索引擎用于对你网站在搜索结果的排序，即内部的优先级排序。需要注意的是如果你把所有页面的优先级设置为1，那么它就和没有设置的效果是一样的。

从上面的内容中，我们可以发现：

 > 站点地图能够提供与其中列出的网页相关的宝贵元数据：元数据是网页的相关信息，例如网页的最近更新时间、网页的更改频率以及网页相较于网站中其他网址的重要程度。 ——内容来自 Google Sitemap帮助文档。


现在，我们一共有三种类型的页面：

 - 首页，通常来说首页的priority应该是最高的，而它的``changefreq``可以设置为``daily``、``weekly``，这取决于你的博客的更新频率。如果你是做一些UGC(用户生成内容)的网站，那么你应该设置为``always``、``hourly``。
 - 动态生成的博客详情页，这些内容一般很少进行改变，所以这的changefreq会比较低，如``yearly``或者``monthly``——并且没有高的必要性，它会导致搜索引擎一直抓取你的内容。这会对服务器造成一定的压力，并且无助于你网站的排名。
 - 静态页面，如About页面，它可以有一个高的``priority``，但是它的``changefreq``也不一定很高。

下面就让我们从首页说起。

### 创建首页的Sitemap

与上面创建静态页面时一样，我们也需要添加``django.contrib.sitemaps``到``INSTALLED_APPS``中。

然后，我们需要指定一个URL规则。通常来说，这个URL是叫sitemap.xml——一个约定俗成的标准。我们需要创建一个sitemaps对象来存储所有的sitemaps:

```
url(r'^sitemap\.xml$', sitemap, {'sitemaps': sitemaps}, name='django.contrib.sitemaps.views.sitemap')
```

由于，我们使用的视图处理方法是``django.contrib.sitemaps.views.sitemap``，代码如下所示:

```

@x_robots_tag
def sitemap(request, sitemaps, section=None,
            template_name='sitemap.xml', content_type='application/xml'):

    req_protocol = request.scheme
    req_site = get_current_site(request)
```

在这个方法里，它指定了默认模板的位置，即在``template``目录中。

现在，我们需要创建几种不同类型的sitemap，如下是首页的Sitemap，它继承自Django的Sitemap类：

```python
class PageSitemap(Sitemap):
    priority = 1.0
    changefreq = 'daily'

    def items(self):
        return ['main']

    def location(self, item):
        return reverse(item)

```

它定义了自己的priority是最高的1.0，同时每新频率为``daily``。然后在items里面去取它所要获取的URL，即``urls.py``中对应的``name``的``main``的URL。在这里我们只返回了``main``一个值，依据于下面的location方法中的``reverse``，它找到了main对应的URL，即首页。

最后结合首页sitemap.xml的``urls.py``代码如下所示：

```python
from sitemap.sitemaps import PageSitemap

sitemaps =  {
    "page": PageSitemap
}

urlpatterns = patterns('',
    url(r'^$', blogpostViews.index, name='main'),
    url(r'^blog/(?P<slug>[^\.]+).html', 'blogpost.views.view_post', name='view_blog_post'),
    url(r'^comments/', include('django_comments.urls')),
    url(r'^admin/', include(admin.site.urls)),
    url(r'^pages/', include('django.contrib.flatpages.urls')),
    url(r'^sitemap\.xml$', sitemap, {'sitemaps': sitemaps}, name='django.contrib.sitemaps.views.sitemap')
) + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

除此，我们还需要创建自己的``sitemap.xml``模板——自带的系统模板比较简单。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
{% spaceless %}
{% for url in urlset %}
<url>
    <loc>{{ url.location }}</loc>
    {% if url.lastmod %}<lastmod>{{ url.lastmod|date:"Y-m-d" }}</lastmod>{% endif %}
    {% if url.changefreq %}<changefreq>{{ url.changefreq }}</changefreq>{% endif %}
    {% if url.priority %}<priority>{{ url.priority }}</priority>{% endif %}
</url>
{% endfor %}
{% endspaceless %}
</urlset>
```

最后，我们访问[http://localhost:8000/sitemap.xml](http://localhost:8000/sitemap.xml)，我们就可以获取到我们的``sitemap.xml``：

```
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    <url>
        <loc>http://www.phodal.com/</loc>
        <changefreq>daily</changefreq>
        <priority>1.0</priority>
    </url>
</urlset>
```

下一步，我们仍可以直接创建出对应的静态页面的Sitemap。

### 创建静态页面的Sitemap

相似的，我们也需要从items方法中，定义出我们所要创建页面的对象。

```python
from django.contrib.sitemaps import Sitemap
from django.core.urlresolvers import reverse
from django.apps import apps as django_apps

class FlatPageSitemap(Sitemap):
    priority = 0.8

    def items(self):
        Site = django_apps.get_model('sites.Site')
        current_site = Site.objects.get_current()
        return current_site.flatpage_set.filter(registration_required=False)
```

只不过这个方法可能会稍微麻烦一些，我们需要从数据库中取出当前的站点。再取出当前站点中的flatpage集合，对过滤那些不需要注册的页面，即代码中的``registration_required=False``。

最后再将这个对象放入sitemaps即可：

```
from sitemap.sitemaps import PageSitemap, FlatPageSitemap

sitemaps =  {
    "page": PageSitemap,
    'flatpages': FlatPageSitemap
}
```

现在，我们可以完成博客的Sitemap了。

### 创建博客的Sitemap

同上面一样的是，我们依然需要在items方法中返回所有的博客内容。并且在lastmod中，返回这篇博客的发表日期——以免他们返回的是同一个日期：

```python
class BlogSitemap(Sitemap):
    changefreq = "never"
    priority = 0.5

    def items(self):
        return Blogpost.objects.all()

    def lastmod(self, obj):
        return obj.posted
```

最近我们的Sitemap.xml，如下所示:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    <url>
        <loc>http://www.phodal.com/about/</loc>
        <priority>0.8</priority>
    </url>
    <url>
        <loc>http://www.phodal.com/</loc>
        <changefreq>daily</changefreq>
        <priority>1.0</priority>
    </url>
    <url>
        <loc>http://www.phodal.com/blog/hello.html</loc>
        <lastmod>2016-03-24</lastmod>
        <changefreq>never</changefreq>
        <priority>0.5</priority>
    </url>
</urlset>
```

### 提交到搜索引擎

这里我们以Google Webmaster为例简单的介绍一下如何使用各种站长工具来提交sitemap.xml。

我们可以登录Google的Webmaster：[https://www.google.com/webmasters/tools/home?hl=zh-cn](https://www.google.com/webmasters/tools/home?hl=zh-cn)，然后点击添加属性来创建一个新的网站:

![添加网站](./images/add-property.png)

这时候Google需要确认这个网站是你的，所以它提供几种方法来验证，除了下面的推荐方法：

![推荐的验证方式](./images/google-add-website.png)

我们可以使用下面的这一些方法：

![备选的难方法](./images/google-addition-method.png)

我个人比较喜欢用HTML Tag的方式来实现

![HTML标签验证](./images/html-tag.png)

在我们完成验证之后，我们就可以在后台手动提交Sitemap.xml了。

![提交Sitemap.xml](./images/google-add-sitemap.png)

点击上方的**添加/测试站点地图**即可。
