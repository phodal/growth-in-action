API
===

自动完成
---

AutoComplete是一个很有意思的功能，特别是当我们的文章很多的时候，我们可以让读者有机会能搜索到相应的功能。

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

