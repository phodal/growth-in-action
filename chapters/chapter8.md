Mobile Web
===

为了实现在移动设备上的访问，这里就以riot.js为例做一个简单的Demo。不过，首先我们需要在后台判断用户是来自于某种设备，再对其进行特殊的处理。

移动设备处理
---

幸运的是我们又找到了一个库名为``django_mobile``，可以根据用户的User-Agent来区别设备，并为其分配一个移动设备专用的模板。因此，我们需要安装这个库：

```
pip install django_mobile
```

并将``'django_mobile.middleware.MobileDetectionMiddleware'``和``'django_mobile.middleware.SetFlavourMiddleware'``添加MIDDLEWARE_CLASSES中：

```
MIDDLEWARE_CLASSES = (
    'django.contrib.sessions.middleware.SessionMiddleware',
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.auth.middleware.SessionAuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
    'django.contrib.flatpages.middleware.FlatpageFallbackMiddleware',
    'django_mobile.middleware.MobileDetectionMiddleware',
    'django_mobile.middleware.SetFlavourMiddleware'
)
```

修改Template配置，添加对应的loader和context_processor，如下所示的内容即是修改完后的结果：

```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
            'templates/'
        ],
        'LOADERS': {
                'django_mobile.loader.Loader',
                'django.template.loaders.filesystem.Loader',
                'django.template.loaders.app_directories.Loader'
        },
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                'django_mobile.context_processors.flavour'
            ],
        },
    },
]
```      

我们在LOADERS中添加了``'django_mobile.loader.Loader'``，在``context_processors``中添加了``django_mobile.context_processors.flavour``。

然后在template目录中创建``template/mobile/index.html ``文件，即可。

RIOT
---
