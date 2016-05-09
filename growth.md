
Growth In Action Django
===

准备工作和工具
---

在开始写代码之前你需要保证你有一些Python基础，如果没有的话，请参阅其他相关书籍来一起学习。

并且你还需要在你的计算机上安装：

 - Python环境及其包管理工具pip。
 - Firefox浏览器——用于运行功能测试。
 - Git版本控制器——用于代码版本控制。
 - 一个开发工具。（PS: 在这里笔者使用的是PyCharm的社区版）
 

深入浅出Django
===

Django简介
---

Django是一个高级的Python Web开发框架，它的目标是使得开发复杂的、数据库驱动的网站变得更加简单。

由于Django最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的。所以，我们可以发现在使用Django的很多网站里，都是用于作为CMS（内容管理系统）来使用的。使用Django的一些比较知名的网站如下图所示：

![使用Django的网站](images/who-use-django.jpg)

Django是一个MTV框架，其架构模板看上去与传统的MVC架构并没有太大的区别。其对比如下表所示：

传统的MVC架构| Django 架构
-----------|-----------
Model      | Model(Data Access Logic)
View       |Template(Presentation Logic)
View       | View(Business Logic)
Controller | Django itself

在Django中View只用来描述你要看到的内容，Template才是最后用于显示的内容。而在MVC架构中，这只相当于是View层。它的核心包含下面的四部分：

 - 一个 对象关系映射，作为数据模型和关系性数据库间的媒介（Model层）；
 - 一个基于正则表达式的URL分发器（即MVC中的Controller)；
 - 一个用于处理HTTP请求的系统，含web模板系统(View层)；

其核心框架还包含：

 - 一个轻量级的、独立的Web服务器，只用于开发和测试。
 - 一个表单序列化及验证系统，用于将HTML表单转换成适用于数据库存储的数据。
 - 一个缓存框架，并且可以从几种缓存方式中选择。
 - 中间件支持，能对请求处理的各个阶段进行处理。
 - 内置的分发系统允许应用程序中的组件采用预定义的信号进行相互间的通信。
 - 一个序列化系统，能够生成或读取采用XML或JSON表示的Django模型实例。
 - 一个用于扩展模板引擎的能力的系统。

### Django应用架构

Django的每一个模块在内部都称之为APP，在每个APP里都有自己的三层结构。如下图所示：

![Django 应用架构](images/django_app_arch.jpg)

这样做不仅可以在开发的时候更容易理解系统，而且可以提高代码的可复用性——因为每一个APP都是独立的应用，在下次使用时我们只需要简单的复制和粘贴。

说了这么多，还不如从一个hello,world开始。

Django hello,world
---

###安装Django

安装Django之前，我们可以用virtualenv工具来创建一个虚拟的Python运行环境。环境问题是一个很复杂的问题，在我们使用Python的过程中，我们会不断地安装一些库，而这些库可能会有不同的版本。并且在安装Python库的过程中，我们会遇到权限问题——即我们需要超级用户的权限才能将库安装到系统的环境之下。随后在这个软件的生涯中，我们还需要保证这个项目所依赖的模块不会发生变动。而这些都是很棘手的一些事，这时候我们就需要创建一个虚拟的运行环境，而virtualenv就是这样的一个工具。

####virtualenv

安装Python包我们需要用到pip命令，它是Python语言中的一个包管理工具。如果你没有安装的话，可以使用下面的命令来安装：

```
curl https://bootstrap.pypa.io/get-pip.py | python
```

在不同的Python环境中，我们可能需要使用不同的pip，如下所示是笔者使用的Python3的pip命令pip3

```
$ pip3 install virtualenv
```

如果是Python2.7的话，对应会有:

```
$ pip install virtualenv
```

需要注意的是这将会安装到Python所在的目录，如我的目录是:

```
$ /usr/local/bin/virtualenv
```

有的可能会是：

```
$ /usr/local/share/python3/virtualenv
```

在创建我们的这个虚拟环境之前，我们可以创建一个存储所有virtualenv的目录：

```
$ mkdir somewhere/virtualenvs
```

现在，我们就可以创建一个新的虚拟环境：

```
$ virtualenv somewhere/virtualenvs/<project-name> --no-site-packages
```

如果你想使用不同的Python版本的话，那么需要指定Python版本的路径

```
$ virtualenv --distribute -p /usr/local/bin/python3.3 somewhere/virtualenvs/<project-name>
```

通过到相应的目录下执行激活就可以使用这个虚拟环境了：

```
$ cd somewhere/virtualenvs/<project-name>/bin
$ source activate
```

停止使用只使用执行下面的命令即可：

```
$ deactivate
```

####安装Django

准备了这么久我们终于安装Django了，执行：

```
$ pip install django
```

那么我们并会开始下最新版本的Django，如下所示：

```
Collecting django
  Downloading Django-1.9.4-py2.py3-none-any.whl (6.6MB)
    94% |██████████████████████████████▎ | 6.2MB 251kB/s eta 0:00:02
```    

等下载完后，就会开始安装Django。安装这完后，我们就可以使用Django自带的django-admin命令。django-admin是Django自带的一个管理任务的命令行工具。

通过这个命令，我们不仅仅可以用它来创建项目、创建app、运行服务、数据库迁移，还可以执行各种SQL工具等等。django-admin用法如下：

```
$ django-admin <command> [options]
```

下面是django-admin自带的一些命令：

```
[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    runfcgi
    runserver
    shell
    sql
    sqlall
    sqlclear
    sqlcustom
    sqldropindexes
    sqlflush
    sqlindexes
    sqlinitialdata
    sqlmigrate
    sqlsequencereset
    squashmigrations
    startapp
    startproject
    syncdb
    test
    testserver
    validate
```

现在，让我们来看看这个强大的工具。

###创建项目

在这些命令中startproject可以用于创建项目，在这里我们的项目名是blog，那么我们的命令如下：

$ django-admin startproject blog

这个命令将创建下面的文件内容，而这些是Django项目的一些必须文件。

```
.
├── blog
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── manage.py
```

blog目录对应的就是blog这个项目，将会放置这个项目的一些相关配置：

1. settings.py包含了这个项目的相关配置。如数据库环境、启用的插件等等。
2. urls.py即URL Dispatcher的配置，指明了某个URL应该指向某个函数来处理。
3. wsgi.py用于部署。WSGI（Python Web Server Gateway Interface，Web服务器网关接口）是为Python语言定义的Web服务器和Web应用程序或框架之间的一种简单而通用的接口。
4. __init__.py指明了这是一个Python模块。

manage.py 会在每个Django项目中自动生成，它可以和django-admin做类似的事。如我们可以用manage.py来启动测试环境的服务器：

$ python manage.py runserver

```
Performing system checks...

System check identified no issues (0 silenced).

You have unapplied migrations; your app may not work properly until they are applied.
Run 'python manage.py migrate' to apply them.

March 24, 2016 - 03:07:34
Django version 1.9.4, using settings 'blog.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
Not Found: /
[24/Mar/2016 03:07:35] "GET / HTTP/1.1" 200 1767
Not Found: /favicon.ico
[24/Mar/2016 03:07:36] "GET /favicon.ico HTTP/1.1" 404 1934
```

现在，我们只需要在浏览器中打开[http://127.0.0.1:8000/](http://127.0.0.1:8000/)，便可以访问我们的应用程序。

###Django后台

Django很适合CMS的另外一个原因，就是它自带了一个后台管理系统。为了启用这个后台管理系统，我们需要配置我们的数据库，并创建相应的超级用户。如下所示的是settings.py中的默认数据库配置：

```
# Database
# https://docs.djangoproject.com/en/1.7/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

上面的配置中我们使用的是SQLite3作为数据库，并使用了当前目录下的``db.sqlite3``作为数据库文件。Django内建支持下面的一些数据库：

```
'django.db.backends.postgresql_psycopg2'
'django.db.backends.mysql'
'django.db.backends.sqlite3'
'django.db.backends.oracle'
```

如果我们想使用别的数据库，那么可以在网上寻找的解决方案，如用于支持使用MongoDB的django-nonrel项目。不同的数据库有不同的配置，如下所示的是使用PostgreSQL的配置。

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'mydatabase',
        'USER': 'mydatabaseuser',
        'PASSWORD': 'mypassword',
        'HOST': '127.0.0.1',
        'PORT': '5432',
    }
}
```

接着，我们就可以运行数据库迁移，只需要运行相应的脚本即可：

$ python manage.py migrate

```
Operations to perform:
  Apply all migrations: sessions, admin, auth, contenttypes
Running migrations:
  Rendering model states... DONE
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying sessions.0001_initial... OK
(growth-django)
```

在上面的过程中，我们会创建相应的数据库模型，并依据迁移脚本来创建一些相应的数据，如默认的配置等等。

最后，我们可以创建一个相应的超级用户来登陆后台。

$ python manage.py createsuperuser

```
Username (leave blank to use 'fdhuang'): root
Email address: h@phodal.com
Password:
Password (again):
Superuser created successfully.
```

输入相应的用户名和密码，即可完成创建。然后访问 [http://127.0.0.1:8000/admin](http://127.0.0.1:8000/admin)，输入上面的用户名和密码就可以来到后台：

![Django后台](images/django-backend.jpg)

### 第一次提交

在创建完应用后，我们就可以进行第一次提交，通常这样的提交的提交信息(commit message)是``init project``。如果在那之前，你没有执行``git init``来初始化git的话，那么我们就需要去执行这个命令。

```bash
git init
```

它将返回类似于下面的结果

```bash
Initialized empty Git repository in /Users/fdhuang/test/helloworld/.git/
```

即初始化了一个空的Git项目，然后我们就可以执行``add``来添加上面的内容：

```bash
git add .
```

需要注意的是上在的数据库文件不应该添加到项目里，所以我们应该执行reset命令来重置这个状态：

```bash
git reset db.sqlite3
```

这时我们会将其变成下面的状态：

![第一次提交前的reset](images/first-commit.png)

上面的绿色文件代码这几个文件都被添加了进行，蓝色则代表未添加的文件。为了避免手误产生一些问题，我们需要添加一个名为``.gitignore``文件用于将一些文件名入忽略名单，如下是常用的python项目的``.gitignore``文件中的内容：

```
*.pyc
*.db
*.sqlite3
```

当我们添加完这个文件完，git就会识别到这个文件，并忽略原来的那些文件，如下图所示：

![添加完gitignore文件后的效果](images/git-ignore.png)  

我们只需要添加这个文件即可：

```bash
git add .gitignore
```

如果你之前已经不小心添加了一些不应该添加的文件，那么可以执行下面的命令来重置其状态：

```bash
git reset .
```

然后再执行添加命令。

最后，我们就可以在本地提交我们的代码了:

```
git commit -m "init project"
```

如果你是将代码托管在GitHub上的话，那么你就可以执行``git push``来将代码提交到服务器上。

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

功能测试与搭建持续集成
===

在上一章最后，我们写的测试可以算得上是单元测试，接着我们可以写一些自动化测试。

编写自动化测试
---

接着我们就可以用Selenium来做自动化测试。这是ThoughtWorks出品的一个强大的基于浏览器的开源自动化测试工具，它通常用来编写Web 应用的自动化测试。

### Selenium与第一个UI测试

先让我们来看一个自动化测试的例子：

```python
from django.test import LiveServerTestCase
from selenium import webdriver

class HomepageTestCase(LiveServerTestCase):
    def setUp(self):
        self.selenium = webdriver.Firefox()
        self.selenium.maximize_window()
        super(HomepageTestCase, self).setUp()

    def tearDown(self):
        self.selenium.quit()
        super(HomepageTestCase, self).tearDown()

    def test_visit_homepage(self):
        self.selenium.get(
            '%s%s' % (self.live_server_url,  "/")
        )

        self.assertIn("Welcome to my blog", self.selenium.title)
```

在setUp——即开始的时候，我们会用selenium起一个Firefox浏览器的进程，并执行maximize_window来将窗口最大化。在tearDown——即结束的时候，我们就会关闭这个浏览器的进程。我们的主要测试代码就在``test_visit_homepage``这个方法里，我们在里面访问首页，并判断标题是不是``Welcome to my blog``。

运行上面的测试就会启动一个浏览器，并且会在浏览器上进行相应的操作。如下图所示：

![Selenium Demo](images/selenium-demo.jpg)

这时你可能会产生一些疑惑，这些内容我们不是已经测试过了么？两者从测试看是差不多的，但是从流程上看来说并不是如些。下图是页面渲染的时间线：

![Page Timing Overview](images/page-timing-overview.png)

请求从浏览器传到服务器要有一系列的过程，如重定向、缓存、DNS等等，最后直至返回对应的Response。我们用Django的测试框架只能实现到这一步，随后页面请请求对应的静态资料，再对页面进行渲染，在这个过程中页面的内容会发生一些变化。

为了避免页面的内容被替换掉，那么我们就需要对这部分内容进行测试。

```python
class BlogpostFromHomepageCase(LiveServerTestCase):
    def setUp(self):
        Blogpost.objects.create(
            title='hello',
            author='admin',
            slug='this_is_a_test',
            body='This is a blog',
            posted=datetime.now
        )

        self.selenium = webdriver.Firefox()
        self.selenium.maximize_window()
        super(BlogpostFromHomepageCase, self).setUp()

    def tearDown(self):
        self.selenium.quit()
        super(BlogpostFromHomepageCase, self).tearDown()

    def test_visit_blog_post(self):
        self.selenium.get(
            '%s%s' % (self.live_server_url,  "/")
        )

        self.selenium.find_element_by_link_text("hello").click()
        self.assertIn("hello", self.selenium.title)
```

```python
class BlogpostDetailCase(LiveServerTestCase):
    def setUp(self):
        Blogpost.objects.create(
            title='hello',
            author='admin',
            slug='this_is_a_test',
            body='This is a blog',
            posted=datetime.now
        )

        self.selenium = webdriver.Firefox()
        self.selenium.maximize_window()
        super(BlogpostDetailCase, self).setUp()

    def tearDown(self):
        self.selenium.quit()
        super(BlogpostDetailCase, self).tearDown()

    def test_visit_blog_post(self):
        self.selenium.get(
            '%s%s' % (self.live_server_url,  "/blog/this_is_a_test.html")
        )

        self.assertIn("hello", self.selenium.title)
```

搭建持续集成
---




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


Comments
---

Category
---   

Practise
---

###Monthly

###SEO

前端框架
===

技能选型
---

API
===

RESTful
---

###Django REST Framework

> Django REST Framework 这个名字很直白，就是基于 Django 的 REST 框架。

```
pip install djangorestframework
pip install markdown       # Markdown support for the browsable API.
pip install django-filter  # Filtering support
```

```python
INSTALLED_APPS = (
    ...
    'rest_framework',
)
```

如下所示：

```
INSTALLED_APPS = (
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'blogpost'
)
```

```
urlpatterns = [
    ...
    url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```


CORS
---

###Django CORS Header

```
pip install django-cors-headers
```

```
INSTALLED_APPS = (
    ...
    'corsheaders',
    ...
)
```

移动应用
===

Ionic 2
---

Login
---

###Json Web Tokens

```
pip install djangorestframework-jwt
```

```
urlpatterns = patterns(
    '',
    # ...

    url(r'^api-token-auth/', 'rest_framework_jwt.views.obtain_jwt_token'),
)
```

TODO
---


前端重构
===

MVVM
---

UX
---

部署
===

配置管理 
---

local_settings.py

Fabric
---

 - NGINX - public facing web server
 - gunicorn - internal HTTP application server
 - PostgreSQL - database server
 - memcached - in-memory caching server
 - supervisord - process control and monitor
 - virtualenv - isolated Python environments for each project
 - git or mercurial - version control systems (optional)
