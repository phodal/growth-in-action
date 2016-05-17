API
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

urlpatterns = patterns('',
    ...
    url(r'^api/', include(apiRouter.urls)),
) + static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)
```

我们使用默认的Router来配置我们的URL，即DefaultRouter，它提供了一个非常简单的机制来自动检测URL规则。因此，我们只需要注册好我们的url——``blogpost``以及它值``BlogpostSet``即可。随后，我们再为其定义一个根URL即可:

```python
url(r'^api/', include(apiRouter.urls))
```

### 测试 API

现在，我们可以访问[http://127.0.0.1:8000/api/](http://127.0.0.1:8000/api/)来访问我们现在的API。由于Django REST Framework提供了一个UI机制，所以我们可以在网页上直接看到我们所有的API：

![Django REST Framework列表](images/django-rest-framework-api-lists.png)

然后，点击页面中的[http://127.0.0.1:8000/api/blogpost/](http://127.0.0.1:8000/api/blogpost/)，我们就可以访问博客相关的API了，如下图所示:

![博客API](images/drf-blogppost-set-list.png)

在页面上显示了所有的博客内容，在页面的下面有一个表单可以先让我们来创建数据：

![创建博客的表单](images/api-post-form.png)

直接在表单中添加数据，我们就可以完成数据创建了。

当然，我们也可以直接用命令行工具来测试，执行：

```bash
curl -i  http://127.0.0.1:8000/api/blogpost/
```

即可返回相应的结果：

![CuRL API](images/curl-api.png)

自动完成
---

AutoComplete是一个很有意思的功能，特别是当我们的文章很多的时候，我们可以让读者有机会能搜索到相应的功能。以Google为例，Google在我们输入一些关键字的时候，会向我们推荐一些比较流行的词条可以让我们选择。

![Google AutoComplete](images/google-autocomplete.png)

同样的，我们也可以实现一个同样的效果用于我们的博客搜索：

![自动完成](images/autocomplete-example.png)

当我们输入某一些关键字的时候，就会出现文章的标题，随后我们只需要点击相应的标题即可跳转到文章。

跨域支持
---

### CORS

### 添加跨域支持

```bash
pip install django-cors-headers
```

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

添加到``django-cors-headers=1.1.0``到``requirements.txt``文件中。

添加到``settings.py``中：

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

对应的配置：

```
CORS_ALLOW_CREDENTIALS = True
```

