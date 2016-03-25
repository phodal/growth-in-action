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
