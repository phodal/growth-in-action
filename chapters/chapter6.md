API
===

在下一章开始之前，我们先来搭建一下API平台，不仅仅可以提供一些额外的功能，还可以为我们的APP提供API。

RESTful
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

### 创建博客列表API



### 自动完成

AutoComplete是一个很有意思的功能，特别是当我们的文章很多的时候，我们可以让读者有机会能搜索到相应的功能。

跨域
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

