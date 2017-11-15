应用API
===

在下一章开始之前，我们先来搭建一下API平台，不仅仅可以提供一些额外的功能，还可以为我们的APP提供API。

博客列表
---

### Django REST Framework

在这里，我们需要用到一个名为Django REST Framework的RESTful API库。通过这个库，我们可以快速创建我们所需要的API。

Django REST Framework 这个名字很直白，就是基于 Django 的 REST 框架。因此，首先我们仍是要安装这个库：

```
pip install djangorestframework
```

然后把它添加到``INSTALLED_APPS``中：

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

接着我们可以在我们的API中创建一个URL，用于匹配它的授权机制。

```
urlpatterns = [
    ...
    url(r'^api-auth/', include('rest_framework.urls', namespace='rest_framework'))
]
```

不过这个API，目前并没有多大的用途。只有当我们在制作一些需要权限验证的接口时，它才会突显它的重要性。

### 创建博客列表API

为了方便我们继续展开后面的内容，我们先来创建一个博客列表API。参考Django REST Framework的官方文档，我们可以很快地创建出下面的Demo:

```
from django.contrib.auth.models import User
from rest_framework import serializers, viewsets
from blogpost.models import Blogpost


class BlogpsotSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Blogpost
        fields = ('title', 'author', 'body', 'slug')

class BlogpostSet(viewsets.ModelViewSet):
    queryset = Blogpost.objects.all()
    serializer_class = BlogpsotSerializer
```

在上面这个例子中，API由两个部分组成：

 - ViewSets，用于定义视图的展现形式——如返回哪些内容，需要做哪些权限处理
 - Serializers，用于定义API的表现形式——如返回哪些字段，返回怎样的格式

我们在我们的URL中，会定义相应的规则到ViewSet，而ViewSet则通过``serializer_class``找到对应的``Serializers``。我们将Blogpost的所有对象赋予queryset，并返回这些值。在BlogpsotSerializer中，我们定义了我们要返回的几个字段：``title``、``author``、``body``、``slug``。

接着，我们可以在我们的``urls.py``配置URL。

```python
...

from rest_framework import routers
from blogpost.api import BlogpostSet

apiRouter = routers.DefaultRouter()
apiRouter.register(r'blogpost', BlogpostSet)

urlpatterns = [,
    ...
    url(r'^api/', include(apiRouter.urls)),
] + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

我们使用默认的Router来配置我们的URL，即DefaultRouter，它提供了一个非常简单的机制来自动检测URL规则。因此，我们只需要注册好我们的url——``blogpost``以及它值``BlogpostSet``即可。随后，我们再为其定义一个根URL即可:

```python
url(r'^api/', include(apiRouter.urls))
```

### 测试 API

现在，我们可以访问[http://127.0.0.1:8000/api/](http://127.0.0.1:8000/api/)来访问我们现在的API。由于Django REST Framework提供了一个UI机制，所以我们可以在网页上直接看到我们所有的API：

![Django REST Framework列表](./images/django-rest-framework-api-lists.png)

然后，点击页面中的[http://127.0.0.1:8000/api/blogpost/](http://127.0.0.1:8000/api/blogpost/)，我们就可以访问博客相关的API了，如下图所示:

![博客API](./images/drf-blogppost-set-list.png)

在页面上显示了所有的博客内容，在页面的下面有一个表单可以先让我们来创建数据：

![创建博客的表单](./images/api-post-form.png)

直接在表单中添加数据，我们就可以完成数据创建了。

当然，我们也可以直接用命令行工具来测试，执行：

```bash
curl -i  http://127.0.0.1:8000/api/blogpost/
```

即可返回相应的结果：

![CuRL API](./images/curl-api.png)

自动完成
---

AutoComplete是一个很有意思的功能，特别是当我们的文章很多的时候，我们可以让读者有机会能搜索到相应的功能。以Google为例，Google在我们输入一些关键字的时候，会向我们推荐一些比较流行的词条可以让我们选择。

![Google AutoComplete](./images/google-autocomplete.png)

同样的，我们也可以实现一个同样的效果用于我们的博客搜索：

![自动完成](./images/autocomplete-example.png)

当我们输入某一些关键字的时候，就会出现文章的标题，随后我们只需要点击相应的标题即可跳转到文章。

### 搜索API

为了实现这个功能我们需要对之前的博客API做一些简单的改造——可以支持搜索博客标题。这里我们需要稍微扩展一下我们的博客API即可：

```python
class BlogpostSet(viewsets.ModelViewSet):
    permission_classes = (permissions.IsAuthenticatedOrReadOnly,)
    serializer_class = BlogpsotSerializer
    search_fields = 'title'

    def list(self, request):
        queryset = Blogpost.objects.all()

        search_param = self.request.query_params.get('title', None)
        if search_param is not None:
            queryset = Blogpost.objects.filter(title__contains=search_param)

        serializer = BlogpsotSerializer(queryset, many=True)
        return Response(serializer.data)
```

我们添加了一个名为``search_fields``的变量，顾名思义就是定义搜索字段。接着我们覆写了ModelViewSet的list方法，它是用于列出(list)所有的结果。我们会尝试在我们的请求中获取搜索参量，如果没有的话我们就返回所有的结果。如果搜索的参数中含有标题，则从所有博客中过滤出标题中含有搜索标题中的内容，再返回这些结果。如下是一个搜索的URL：[http://127.0.0.1:8000/api/blogpost/?format=json&title=test](http://127.0.0.1:8000/api/blogpost/?format=json&title=test)，我们搜索标题中含有``test``的内容。

同时，我们还需要为我们的apiRouter设置一个basename，即下面代码中最后的``Blogpost``

```python
apiRouter.register(r'blogpost', BlogpostSet, 'Blogpost')
```

### 页面实现

接着，我们就可以在页面上实现这个功能。在这里我们使用一个名为[Bootstrap-3-Typeahead](https://github.com/bassjobsen/Bootstrap-3-Typeahead)的插件来实现，下载这个插件以及它对应的CSS：[https://github.com/bassjobsen/typeahead.js-bootstrap-css](https://github.com/bassjobsen/typeahead.js-bootstrap-css)，并添加到``base.html``中，然后创建一个``main.js``文件负责相关的逻辑处理。

```html
<script src="{% static 'js/jquery.min.js' %}"></script>
<script src="{% static 'js/bootstrap.min.js' %}"></script>
<script src="{% static 'js/bootstrap3-typeahead.min.js' %}"></script>
<script src="{% static 'js/main.js' %}"></script>
```

接着我们需要在页面上创建对应的UI，我们可以直接在``登录``后面添加这个搜索按钮：

```
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
        <li><a href="/admin" id="loginLink">登录</a></li>
    </ul>
    <div class="col-sm-3 col-md-3 pull-right">
        <form class="navbar-form" role="search">
            <div class="input-group">
                <input type="text" id="typeahead-input" class="form-control" placeholder="Search" name="search" data-provide="typeahead">
                <div class="input-group-btn">
                    <button class="btn btn-default search-button" type="submit"><i class="glyphicon glyphicon-search"></i></button>
                </div>
            </div>
        </form>
    </div>
</nav>
```      

我们主要是使用input标签，标签上对应有一个id

```
<input type="text" id="typeahead-input" class="form-control" placeholder="Search" 
```  

对应于这个ID，我们就可以开始编写我们的功能了：

```javascript
$(document).ready(function () {
    $('#typeahead-input').typeahead({
        source: function (query, process) {
            return $.get('/api/blogpost/?format=json&title=' + query, function (data) {
                return process(data);
            });
        },
        updater: function (item) {
            return item;
        },
        displayText: function (item) {
            return item.title;
        },
        afterSelect: function (item) {
            location.href = 'http://localhost:8000/blog/' + item.slug + ".html";
        },
        delay: 500
    });
});
```

$(document).ready()方法可以是在DOM完成加载后，运行其中的函数。接着我们开始监听``#typeahead-input``，对应的便是id为``typeahead-input``的元素。可以看到在这其中有五个对象：

 - source，即搜索的来源，我们返回的是我们搜索的URL。
 - updater，即每次更新要做的事
 - displayText，显示在页面上的内容，如在这里我们返回的是博客的标题
 - afterSelect，每用户选中某一项后做的事，这里我们直接中转到对应的博客。
 - delay，延时500ms。

虽然我们使用的是插件来完成我们的功能，但是总体的处理逻辑是：

1. 监听我们的输入文本
2. 获取API的返回结果
3. 对返回结果进行处理——如高亮输入文本、显示到页面上
4. 处理用户点击事件

跨域支持
---

当我们想为其他的网页提供我们的API时，可能会报错——原因是不支持跨域请求。为了方便我们下一章更好的展开，内容我们在这里对跨域进行支持。

### 添加跨域支持

有一个名为``django-cors-headers``的插件用于实现对跨域请求的支持，我们只需要安装它，并进行一些简单的配置即可。

```bash
pip install django-cors-headers
```

安装过程如下：

```
Collecting django-cors-headers
  Downloading django-cors-headers-1.1.0.tar.gz
Building wheels for collected packages: django-cors-headers
  Running setup.py bdist_wheel for django-cors-headers ... done
  Stored in directory: /Users/fdhuang/Library/Caches/pip/wheels/b0/75/89/7b17f134fc01b74e10523f3128e45b917da0c5f8638213e073
Successfully built django-cors-headers
Installing collected packages: django-cors-headers
Successfully installed django-cors-headers-1.1.0
```

我们还需要添加到``django-cors-headers=1.1.0``到``requirements.txt``文件中，以及添加到``settings.py``中：

```
INSTALLED_APPS = (
    ...
    'corsheaders',
    ...
)
```

以及对应的中间件：

```
MIDDLEWARE_CLASSES = (
    ...
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    ...
)
```

同时还有对应的配置：

```
CORS_ALLOW_CREDENTIALS = True
```

现在，让我们进行下一步，开始APP吧！
