Django创建博客应用
===

Tasking
---

在我们不了解Django的时候，要对这样一个任务进行Tasking，有点困难。不过，我们还是可以简单地看看是应该如何去做：

 - 生成APP。对于大部分主流的Web框架来说，它们都可以手动地生成一些脚手架，如Ruby语言中的Ruby On Rails、Node.js中的Express等等。
 - 创建对应的Model，即其在数据库中存储的模型与我们在代码中要使用的模型。
 - 创建程序对应的View，用于处理数据。
 - 创建程序的Template，用于显示数据。
 - 编写测试来保证功能。

对于其他应用来说也是差不多的。 

创建BlogpostAPP
---

### 生成APP

现在我们可以开始创建我们的APP，使用下面的代码来创建：

$ django-admin startapp blogpost

会在blogpost目录下，生成下面的文件：


```
.
├── __init__.py
├── admin.py
├── apps.py
├── migrations
│   └── __init__.py
├── models.py
├── tests.py
└── views.py
```

### 创建Model

现在，我们需要来创建博客的Model即可。对于一篇基本的博客来说，它会包含下在面的几部分内容：

 - 标题
 - 作者
 - 链接（中文更需要一个好的链接）
 - 内容
 - 发布日期

我们就可以按照上面的内容来创建我们的Blogpost model：

```python
from django.db import models
from django.db.models import permalink


class Blogpost(models.Model):
    title = models.CharField(max_length=100, unique=True)
    author = models.CharField(max_length=100, unique=True)
    slug = models.SlugField(max_length=100, unique=True)
    body = models.TextField()
    posted = models.DateField(db_index=True, auto_now_add=True)

    def __unicode__(self):
        return '%s' % self.title

    @permalink
    def get_absolute_url(self):
        return ('view_blog_post', None, { 'slug': self.slug })
```

上面的``get_absolute_url``方法就是用于返回博客的链接。之所以使用手动而不是自动生成，是因为自动生成不靠谱，而且不利

然后在Admin注册这个Model

```python
from django.contrib import admin
from blogpost.models import Blogpost

class BlogpostAdmin(admin.ModelAdmin):
    exclude = ['posted']
    prepopulated_fields = {'slug': ('title',)}

admin.site.register(Blogpost, BlogpostAdmin)
```

接着进入后台，我们就可以看到BLOGPOST的一栏里，就可以对其进行相关的操作。

![Django后台界面](images/django-admin-ui.png)

点击Blogpost的Add后，我们就会进入如下的添加博客界面：

![Django添加博客](images/admin-blog.png)

实际上，这样做的意义是将删除(Delete)、修改(Update)、添加(Create)这些内容将给用户后台来做，当然它也不需要在View/Template层来做。在我们的Template层中，我们只需要关心如何来显示这些数据。

现在，我们可以执行一次新的代码提交——因为现在的代码可以正常工作。这样出现问题时，我们就可以即时的返回上一版本的代码。

```
git add .
git commit -m "create blogpost model"
```

然后再进行下一步地操作。

### 配置URL

现在，我们就可以在我们的``urls.py``里添加相应的route来访问页面，代码如下所示：

```python
from django.conf import settings
from django.conf.urls import patterns, include, url
from django.conf.urls.static import static
from django.contrib import admin

apiRouter = routers.DefaultRouter()
apiRouter.register(r'blogpost', BlogpostSet)

urlpatterns = patterns('',
    (r'^$', 'blogpost.views.index'),
    url(r'^blog/(?P<slug>[^\.]+).html', 'blogpost.views.view_post', name='view_blog_post'),
    url(r'^admin/', include(admin.site.urls))
) + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

在上面的代码里，我们创建了两个route：

 - 指向首页，其view是index
 - 指向博客详情页，其view是view_post

指向博客详情页的URL正规``r'^blog/(?P<slug>[^\.]+).html``，会将形如blog/hello-world.html中的hello-world提取出来作为参数传给view_post方法。

接着，我们就可以创建两个view。 

创建View
---

### 创建博客列表页

对于我们的首页来说，我们可以简单的只显示五篇博客，所以我们所需要做的就是从我们的Blogpost对象中，取出前五个结果即可。代码如下所示：

```python
from django.shortcuts import render, render_to_response, get_object_or_404
from blogpost.models import Blogpost

def index(request):
    return render_to_response('index.html', {
        'posts': Blogpost.objects.all()[:5]
    })
```

Django的render_to_response方法可以根据一个给定的上下文字典渲染一个给定的目标，并返回渲染后的HttpResponse。即将相应的值，如这里的Blogpost.objects.all()[:5]，填入相应的index.html中，再返回最后的结果。

因此，在我们的index.html中，我们就可以拿到前五篇博客。我们只需要遍历出posts，拿出每个post相应的值，就可以完成列表页。

```html
{% extends 'base.html' %}
{% block title %}Welcome to my blog{% endblock %}

{% block content %}
<h1>Posts</h1>
{% for post in posts %}
<h2><a href="{{ post.get_absolute_url }}">{{ post.title }}</a></h2>
<p>{{post.posted}} - By {{post.author}}</p>
<p>{{post.body}}</p>
{% endfor %}
{% endblock %}
```

在上面的模板里，我们还取出了博客的链接用于跳转到详情页。

### 创建博客详情页

依据上面拿到的slug，我们就可以创建对应的详情页的view，代码如下所示：

```python
def view_post(request, slug):
    return render_to_response('blogpost_detail.html', {
        'post': get_object_or_404(Blogpost, slug=slug)
    })
```

这里的``get_object_or_404``将会根据slug来获取相应的博客，如果取不出相应的博客就会返回404。因此，我们的详情页和上面的列表页也是类似的。

```html
{% extends 'base.html' %}
{% block head_title %}{{ post.title }}{% endblock %}
{% block title %}{{ post.title }}{% endblock %}

{% block content %}
<h2>{{ post.title }}</a></h2>
<p>{{post.posted}} - By {{post.author}}</p>
<p>{{post.body}}</p>
{% endblock %}
```

随后，我们就可以再提交一次代码了。

测试
---

TDD虽然是一个非常好的实践，但是那是对于那些已经习惯写测试的人来说。如果你写测试的经历非常小，那么我们就可以从写测试开始。

在这里我们使用的是Django这个第三方框架来完成我们的工作，所以我们并不对这个框架的功能进行测试。虽然有些时候正是因为这些第三方框架的问题而导致的Bug，但是我们仅仅只是使用一些基础的功能。这些基础的功能也已经在他们的框架中测试过了。

### 测试首页

先来做一个简单的测试，即测试我们访问首页的时候，调用的函数是上面的index函数

```python
from django.core.urlresolvers import resolve
from django.http import HttpRequest
from django.test import TestCase

from blogpost.views import index, view_post


class HomePageTest(TestCase):
    def test_root_url_resolves_to_home_page_view(self):
        found = resolve('/')
        self.assertEqual(found.func, index)
```

但是这样的测试看上去没有多大意义，不过它可以保证我们的route可以和我们的URL对应上。在编写完测试后，我们就可以命令提示行中运行:

```bash
python manage.py test
```

来查看测试的结果：

```
Creating test database for alias 'default'...

.
----------------------------------------------------------------------
Ran 1 test in 0.031s

OK
Destroying test database for alias 'default'...
(growth-django)
```

运行通过，现在我们可以进行下一个测试了——我们可以测试页面的标题是不是我们想要的结果：

```python
    def test_home_page_returns_correct_html(self):
        request = HttpRequest()
        response = index(request)
        self.assertIn(b'<title>Welcome to my blog</title>', response.content)
```

这里我们需要去请求相应的页面来获取页面的标题，并用assertIn方法来断言返回的首页的html中含有``<title>Welcome to my blog</title>``。

### 测试详情页

同样的我们也可以用测试是否调用某个函数的方法，来看博客的详情页的route是否正确？

```python
class BlogpostTest(TestCase):
    def test_blogpost_url_resolves_to_blog_post_view(self):
        found = resolve('/blog/this_is_a_test.html')
        self.assertEqual(found.func, view_post)
```

与上面测试首页不一样的是，在我们的Blogpost测试中，我们需要创建数据，以确保这个流程是没有问题的。因此我们需要用``Blogpost.objects.create``方法来创建一个数据，然后访问相应的页面来看是否正确。

```python  
def test_blogpost_create_with_view(self):
    Blogpost.objects.create(title='hello', author='admin', slug='this_is_a_test', body='This is a blog',
                            posted=datetime.now)
    response = self.client.get('/blog/this_is_a_test.html')
    self.assertIn(b'This is a blog', response.content)
```

或许你会疑惑这个数据会不会被注入到数据库中，请看运行测试时返回的结果的第一句：

```
Creating test database for alias 'default'...
```

Django将会创建一个数据库用于测试。

同理，我们也可以为首页添加一个相似的测试：

```python
def test_blogpost_create_with_show_in_homepage(self):
    Blogpost.objects.create(title='hello', author='admin', slug='this_is_a_test', body='This is a blog',
                            posted=datetime.now)
    response = self.client.get('/')
    self.assertIn(b'This is a blog', response.content)
```

我们用同样的方法创建了一篇博客，然后在首页测试返回的内容中是否含有``This is a blog``。
