配置管理 
===

local settings
---

作为一个开源项目，我们在这方面做得并不是特别好——当然是有意如此的。不过，这里我们还是做一些简单的介绍。对于我们的项目来说，我们需要一些额外的配置，如我们的数据库中的``DATABASES``、``DEFAULT_AUTHENTICATION_CLASSES``、``CORS_ORIGIN_ALLOW_ALL``、``SECRET_KEY``应该在不同的环境中都有不同的配置。

我们可以一个创建``local_settings.py``，在里面放置一些关键的服务器相关的配置，如:

```python

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'hpi!zb8!(j%40)r55@+_5k*^9qcjf9sx0o_it*jlp3=x9^2ak@'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

TEMPLATE_DEBUG = True

# Database
# https://docs.djangoproject.com/en/1.7/ref/settings/#databases

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': 'db.sqlite3',
    }
}

REST_FRAMEWORK = {
    'DEFAULT_PERMISSION_CLASSES': (

    ),
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework.authentication.SessionAuthentication',
        'rest_framework.authentication.BasicAuthentication',
        'rest_framework_jwt.authentication.JSONWebTokenAuthentication',
    ),
}

CORS_ORIGIN_ALLOW_ALL = True
```

接着，我们只需要在我们的主``settiings.py``中引用即可。
