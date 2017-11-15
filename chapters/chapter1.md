深入浅出Django
===

Django简介
---

Django是一个高级的Python Web开发框架，它的目标是使得开发复杂的、数据库驱动的网站变得更加简单。

由于Django最初是被开发来用于管理劳伦斯出版集团旗下的一些以新闻内容为主的网站的。所以，我们可以发现在使用Django的很多网站里，都是用于作为CMS（内容管理系统）来使用的。使用Django的一些比较知名的网站如下图所示：

![使用Django的网站](./images/who-use-django.jpg)

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

![Django 应用架构](./images/django_app_arch.jpg)

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

停止使用只需要执行下面的命令即可：

```
$ deactivate
```

####安装Django

准备了这么久我们终于要开始安装Django了，执行：

```
$ pip install django
```

开始下最新版本的Django，如下所示：

```
Collecting django
  Downloading Django-1.9.4-py2.py3-none-any.whl (6.6MB)
    94% |██████████████████████████████▎ | 6.2MB 251kB/s eta 0:00:02
```    

等下载完后，就会开始安装Django。安装完后，我们就可以使用Django自带的django-admin命令。django-admin是Django自带的一个管理任务的命令行工具。

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
4. \_\_init\_\_.py指明了这是一个Python模块。

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

如果我们想使用别的数据库，可以在网上寻找相应的解决方案，如用于支持使用MongoDB的django-nonrel项目。不同的数据库有不同的配置，如下所示的是使用PostgreSQL的配置。

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

![Django后台](./images/django-backend.jpg)

### 第一次提交

在创建完应用后，我们就可以进行第一次提交，通常初次提交的提交信息(commit message)是``init project``。如果在那之前，你没有执行``git init``来初始化git的话，那么我们就需要去执行这个命令。

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

需要注意的是上面的数据库文件不应该添加到项目里，所以我们应该执行reset命令来重置这个状态：

```bash
git reset db.sqlite3
```

这时我们会将其变成下面的状态：

![第一次提交前的reset](./images/first-commit.png)

上面的绿色文件代表这几个文件都被添加了进去，蓝色则代表未添加的文件。为了避免手误产生一些问题，我们需要添加一个名为``.gitignore``文件用于将一些文件名加入忽略名单，如下是常用的python项目的``.gitignore``文件中的内容：

```
*.pyc
*.db
*.sqlite3
```

当我们添加完这个文件，git就会识别这个文件，并忽略原来的那些文件，如下图所示：

![添加完gitignore文件后的效果](./images/git-ignore.png)  

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
