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
            'context_processors':    [
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


前后端分离
---

为了方便我们讲述模块化，也不改变系统原有架构，我决定挖个大坑使用Riot.js来展示这一部分的内容。

### Riot.js

Riot拥有创建现代客户端应用的所有必需的成分:

 - “响应式” 视图层用来创建用户界面
 - 用来在各独立模块之间进行通信的事件库
 - 用来管理URL和浏览器回退按钮的路由器（Router）

等等。

接着让我们引入riot.js这个库，顺便也引入rxjs吧:

```
<script src="{% static 'js/mobile/riot+compiler.min.js' %}"></script>
<script src="{% static 'js/mobile/rx.core.min.js' %}"></script>
```

###ReactiveJS构建服务

由于我们所要做的服务比较简单，并且我们也更愿意使用Promise来加载API服务，因此我们引入了这个库来加速我们的开发。下面是我们用于获取博客API的代码：

```
var responseStream = function (blogId) {
    var url = '/api/blogpost/?format=json';

    if(blogId) {
        url = '/api/blogpost/' + blogId + '?format=json';
    }

    return Rx.Observable.create(function (observer) {
        jQuery.getJSON(url)
            .done(function (response) {
                observer.onNext(response);
            })
            .fail(function (jqXHR, status, error) {
                observer.onError(error);
            })
            .always(function () {
                observer.onCompleted();
            });
    });
};
```

当我们想访问特定博客的时候，我们就传博客ID进去——这时会使用``'/api/blogpost/' + blogId + '?format=json'``作为URL。接着我们创建了自己定制的事件流——使用jQuery去获取API：

 - 成功的时候(done)，我们将用onNext()来通知观察者
 - 失败的时候(fail)，我们就调用onError()来通知观察者
 - 不论成功或者失败，都会执行always

在使用的时候，我们只需要调用其``subscribe``方法即可：

```
responseStream().subscribe(function (response) {

})
```

### 创建博客列表页

现在，我们可以修改原生的博客模板，将其中的container内容变为：

```
<div class="container" id="container">
    <blog></blog>
</div>
```

接着，我们可以创建一个``blog.tag``文件，添加加载这个文件:`

```
<script src="{% static 'riot/blog.tag' %}" type="riot/tag"></script>
```

为了调用这个tag的内容，我们需要在我们的``main.js``加上一句：

```javascript
riot.mount("blog");
```

随后我们可以在我们的tag文件中，来对blog的内容进行操作。

```html
<blog class="row">
    <div class="col-sm-4" each={ opts }>
        <h2><a href="#/blogDetail/{id}" onclick={ parent.click }>{ title }</a></h2>
        { body }
        { posted } - By { author }
    </div>
    <script>
        var self = this;
        this.on('mount', function (id) {
            responseStream().subscribe(function (response) {
                self.opts = response;
                self.update();
            })
        })

        click(event)
        {
            this.unmount();
            riot.route("blogDetail/" + event.item.id);
        }
    </script>
</blog>
```

在Riot中，变量默认是以opts的方式传递起来的，因此我们也遵循这个方式。在模板方面，我们遍历每个博客取出其中的内容：

```
<div class="col-sm-4" each={ opts }>
    <h2><a href="#/blogDetail/{id}" onclick={ parent.click }>{ title }</a></h2>
    { body }
    { posted } - By { author }
</div>
```

而博客的数据需要依赖于我们监听``mount``事件才会去获取——即我们加载了这个tag。

```
this.on('mount', function (id) {
    responseStream().subscribe(function (response) {
        self.opts = response;
        self.update();
    })
})
```

在这个页面中，还有一个单击事件``onclick={ parent.click }``，即当我们点击某个博客的标题时执行的函数:

```
click(event)
{
    this.unmount();
    riot.route("blog/" + event.item.id);
}
```
我们将卸载当前的tag，然后加载blogDetail的内容。

### 博客详情页

在我们加载之前，我们需要先配置好blogDetail。我们仍然使用正则表达式``blogDetail/*``来获取博客的id:

```
riot.route.base('#');

riot.route('blog/*', function(id) {
    riot.mount("blogDetail", {id: id})
});

riot.route.start();
```

然后将由相应的tag来执行：

```
<blogDetail class="row">
    <div class="col-sm-4">
        <h2>{ opts.title }</h2>
        { opts.body }
        { opts.posted } - By { opts.author }
    </div>
    <script>
        var self = this;

        this.on('mount', function (id) {
            responseStream(this.opts.id).subscribe(function (response) {
                self.opts = response;
                self.update();
            })
        })
    </script>
</blogDetail>
```
同样的，我们也将去获取这篇博客的内容，然后显示。

### 添加导航

在上面的例子里，我们少了一部分很重要的内容就是在页面间跳转，现在就让我们来创建``navbar.tag``吧。

首先，我们需要重新规则一下route，在系统初始化的时候我们将使用的路由是blog，在进入详情页的时候，我们用blog/*。

```javascript
riot.route.base('#');

riot.route('blog/*', function (id) {
    riot.mount("blogDetail", {id: id})
});

riot.route('blog', function () {
    riot.mount("blog")
});

riot.route.start();

riot.route("blog");
```

然后将我们的navbar标签放在``blog``和``blogDetail``中，如下所示：

```
<blogDetail class="row">
    <navbar title="{ opts.title }"></navbar>
    <div class="col-sm-4">
        <h2>{ opts.title }</h2>
        { opts.body }
        { opts.posted } - By { opts.author }
    </div>
</blogDetail>      

当我们到了博客详情页，我们将把标题作为参数传给title。接着，我们在navbar中我们就可以创造一个breadcrumb导航了:

```
<navbar>
    <ol class="breadcrumb">
        <li><a href="#/" onclick={parent.clickTitle}>Home</a></li>
        <li if="opts.title">{ opts.title} </li>
    </ol>
</navbar>
```

最后可以在我们的blogDetail标签中添加一个点击事件来跳转到首页：

```
clickTitle(event) {
    self.unmount(true);
    riot.route("blog");
}
```
        
