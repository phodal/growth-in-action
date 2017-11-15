创建移动应用
===

依照国际惯例，我们还将用Ionic 2继续创建hello,world。

hello,world
---

开始之前我们需要先安装Ionic的命令行工具，后面我们需要用这个工具来创建工程。

```bash
npm install -g ionic@beta
```

如果没有意外，我们将安装成功，然后可以使用``ionic``命令:

它自带了一系列的工具来加速我们的开发，这些工具可以在后面的章节中学习到。

```
Available tasks: (use --help or -h for more info)

   start  ..........  Starts a new Ionic project in the specified PATH
   serve  ..........  Start a local development server for app dev/testing
   platform  .......  Add platform target for building an Ionic app
   run  ............  Run an Ionic project on a connected device
   emulate  ........  Emulate an Ionic project on a simulator or emulator
   build  ..........  Build (prepare + compile) an Ionic project for a given platform.

   plugin  .........  Add a Cordova plugin
   resources  ......  Automatically create icon and splash screen resources (beta)
		      Put your images in the ./resources directory, named splash or icon.
		      Accepted file types are .png, .ai, and .psd.
		      Icons should be 192x192 px without rounded corners.
		      Splashscreens should be 2208x2208 px, with the image centered in the middle.

   upload  .........  Upload an app to your Ionic account
   share  ..........  Share an app with a client, co-worker, friend, or customer
   lib  ............  Gets Ionic library version or updates the Ionic library
   setup  ..........  Configure the project with a build tool (beta)
   io  .............  Integrate your app with the ionic.io platform services (alpha)
   security  .......  Store your app's credentials for the Ionic Platform (alpha)
   push  ...........  Upload APNS and GCM credentials to Ionic Push (alpha)
   package  ........  Use Ionic Package to build your app (alpha)
   config  .........  Set configuration variables for your ionic app (alpha)
   browser  ........  Add another browser for a platform (beta)
   service  ........  Add an Ionic service package and install any required plugins
   add  ............  Add an Ion, bower component, or addon to the project
   remove  .........  Remove an Ion, bower component, or addon from the project
   list  ...........  List Ions, bower components, or addons in the project
   info  ...........  List information about the users runtime environment
   help  ...........  Provides help for a certain command
   link  ...........  Sets your Ionic App ID for your project
   hooks  ..........  Manage your Ionic Cordova hooks
   state  ..........  Saves or restores state of your Ionic Application using the package.json file
   docs  ...........  Opens up the documentation for Ionic
   generate  .......  Generate pages and components
```

现在，我们就可以用第一个命令``start``来创建我们的项目。


```bash
ionic start growth-blog-app --v2
```

在这个过程中，它将下载Ionic 2项目的基础项目，并执行安装命令。

```
Creating Ionic app in folder /Users/fdhuang/repractise/growth-blog-app based on tabs project
Downloading: https://github.com/driftyco/ionic2-app-base/archive/master.zip
[=============================]  100%  0.0s
Downloading: https://github.com/driftyco/ionic2-starter-tabs/archive/master.zip
[=============================]  100%  0.0s
Installing npm packages...
```

然后到``growth-blog-app``目录，我们会看到类似于下面的内容：

```
.
├── README.md
├── app
│   ├── app.js
│   ├── pages
│   │   ├── page1
│   │   │   ├── page1.html
│   │   │   ├── page1.js
│   │   │   └── page1.scss
│   │   ├── page2
│   │   │   ├── page2.html
│   │   │   ├── page2.js
│   │   │   └── page2.scss
│   │   ├── page3
│   │   │   ├── page3.html
│   │   │   ├── page3.js
│   │   │   └── page3.scss
│   │   └── tabs
│   │       ├── tabs.html
│   │       └── tabs.js
│   └── theme
│       ├── app.core.scss
│       ├── app.ios.scss
│       ├── app.md.scss
│       ├── app.variables.scss
│       └── app.wp.scss
├── config.xml
├── gulpfile.js
├── hooks
│   ├── README.md
│   └── after_prepare
│       └── 010_add_platform_class.js
├── ionic.config.json
├── package.json
└── www
    └── index.html
```

在这2.0版本的Ionic，页面开始以目录来划分，一个页面路径下有自己的``html``、``js``、``scss``。

 - ``tabs``负责这些页面间跳转
 - ``theme``则负责系统相应样式的修改
 - ``config.xml``带有相应的Cordova配置
 - ``hooks``则对系统添加和编译时进行一些预处理
 - ``ionic.config.json``则是ionic的一些相关配置选项
 - ``package.json``则存放相应的node.js的包的依赖
 - ``www``目录用于存放出最后构建出来的内容，以及一些静态资源

由于Angular 2.0使用的是Typescript，所以在这里我们将用typescript进行展示，因此我们的执行命令变成~~：

```
ionic start growth-blog-app --v2 --ts
```

``--ts``表示使用的是``typescript``来创建项目，安装的过程是一样的，不一样的是后面写的代码。

执行相应的启动serve命令，我们就可以开始我们的项目了：

```bash
ionic serve
```

这时候Ionic将做一些额外的事，才能启动我们的服务，如：

 - 删除``www/build``目录下的文件
 - 编译SASS到CSS
 - 编译文件到HTML
 - 编译字体
 - 等等

最后，它将启动一个Web服务，URL为[http://localhost:8100](http://localhost:8100)

```bash
  Running 'serve:before' gulp task before serve
  [20:59:16] Starting 'clean'...
  [20:59:16] Finished 'clean' after 6.07 ms
  [20:59:16] Starting 'watch'...
  [20:59:16] Starting 'sass'...
  [20:59:16] Starting 'html'...
  [20:59:16] Starting 'fonts'...
  [20:59:16] Starting 'scripts'...
  [20:59:16] Finished 'scripts' after 43 ms
  [20:59:16] Finished 'html' after 51 ms
  [20:59:16] Finished 'fonts' after 54 ms
  [20:59:16] Finished 'sass' after 738 ms
  7.6 MB bytes written (5.62 seconds)
  [20:59:22] Finished 'watch' after 6.62 s
  [20:59:22] Starting 'serve:before'...
  [20:59:22] Finished 'serve:before' after 3.87 μs

  Running live reload server: http://localhost:35729
  Watching: www/**/*, !www/lib/**/*
  √ Running dev server:  http://localhost:8100
  Ionic server commands, enter:
    restart or r to restart the client app from the root
    goto or g and a url to have the app navigate to the given url
    consolelogs or c to enable/disable console log output
    serverlogs or s to enable/disable server log output
    quit or q to shutdown the server and exit
  ionic $
```

接着，就可以打开相应的Web页面，如下图所示：

![Ionic Web预览界面](./images/ionic-web-view.jpg)

### 构建应用

由于Ionic是基于Cordova的，我们需要安装Cordova业完成后续的工作。

```bash
sudo npm install -g cordova
```

为了构建不同的平台的应用，我们就需要添加不同的平台，如:

```bash
ionic platform add android
```

上面的命令可以为项目添加Android平台的支持，过程如下面的日志所示：

```
Adding android project...
Creating Cordova project for the Android platform:
	Path: platforms/android
	Package: io.ionic.starter
	Name: V2_Test
	Activity: MainActivity
	Android target: android-23
Android project created with cordova-android@5.1.1
Running command: /Users/fdhuang/repractise/growth-blog-app/hooks/after_prepare/010_add_platform_class.js /Users/fdhuang/repractise/growth-blog-app
```

最后，再执行``run``就可以在对应的平台上运行，如:

```
ionic run android
```

博客列表页
---

现在，让我们来结合我们的博客APP，做一个相应的展示博客的APP。在这一步我们所要做的事情比较简单：

 - 获取博客列表API
 - 渲染博客列表

### 列表页

在上一个章节里我们已经有了一个博客详细的API，我们只需要获取这个API并显示即可。不过，让我们简单地熟悉一下显示数据的这部分内容：

```html
<ion-navbar *navbar>
  <ion-title>博客</ion-title>
</ion-navbar>

<ion-content class="blog-list">
  <ion-item *ngFor="#blogpost of blogposts">
    <h1 *ngIf="blogpost">
      {{blogpost.title}}
    </h1>

    <p *ngIf="blogpost">
      {{blogpost.body}}
    </p>
  </ion-item>
</ion-content>
```

上面是一个基本的详情页的模板，其中定义了一系列的Ionic自定义标签，如：

 - <ion-navbar> 显示在导航栏中的内容
 - <ion-content> 显示APP的内容
 - <ion-item> 即将博客展示成每一项

而从上面的内容中，我们可以看到：我们在ngFor中遍历了blogposts，然后显示每篇文章的标题和内容。对应的代码也就比较简单了:

```javascript
import {Page} from 'ionic-angular';

@Page({
  templateUrl: 'build/pages/blog/list/index.html',
  providers: [BlogpostServices]
})
export class BlogList {
  public blogposts;

  constructor() {

  }
}
```

但是我们要去哪里获取博客的值呢，我们先看看改造后的BlogList的Controller：


```javascript
import {Page} from 'ionic-angular';
import {BlogpostServices} from '../../../services/BlogpostServices';

@Page({
  templateUrl: 'build/pages/blog/list/index.html',
  providers: [BlogpostServices]
})
export class BlogList {
  private blogListService;
  public blogposts;

  constructor(blogpostServices:BlogpostServices) {
    this.blogListService = blogpostServices;
    this.initService();
  }

  private initService() {
    this.blogListService.getBlogpostLists().subscribe(
      data => {this.blogposts = JSON.parse(data._body);},
      err => console.log('Error: ' + JSON.stringify(err)),
      () => console.log('Get Blogpost')
    );
  }
}

```

我们初始化了一个blogListService，然后我们调用这个服务去获取博客列表。

```javascript
    this.blogListService.getBlogpostLists().subscribe(
      data => {this.blogposts = JSON.parse(data._body);},
      err => console.log('Error: ' + JSON.stringify(err)),
      () => console.log('Get Blogpost')
    );
```    

当我们获取到数据的时候，我们就解析这个数据，并将这个值赋予blogposts。如果这其中遇到什么错误，就会显示相应的错误信息。

现在，让我们创建一个获取博客的服务：

```
import {Inject} from 'angular2/core';
import {Http} from 'angular2/http';
import 'rxjs/add/operator/map';

export class BlogpostServices {
  private http;

  constructor(@Inject(Http) http:Http) {
    this.http = http
  }

  getBlogpostLists() {
    var url = 'http://127.0.0.1:8000/api/blogpost/?format=json';
    return this.http.get(url).map(res => res);
  }
}
```

我们将通过这个API来获取相关的数据，并将数据返回到BlogList类中。接着将更新blogposts的值，并重新渲染页面。

### 详情页

在我们的博客API中，每个内容都对应有一个id，如下所示:

```
{
    "title": "这是一个标题",
    "author": "Phodal2",
    "body": "这是一个测试的内容",
    "slug": "this-is-a-test",
    "id": 3
}
```

我们只需要访问这个id，就可以获取这个结果，如：[http://localhost:8000/api/blogpost/3/](http://localhost:8000/api/blogpost/3/)

因此，我们所需要做的就是：

 - 在渲染博客列表的时候，为每一项赋予一个ID
 - 点击某一项时，将跳转到详情页，并去获取相应的API的数据，并渲染到页面上。

好了，我们可以用ionic的生成命令来创建博客详情页。    

```bash
ionic g page blog-detail --ts
```

它将在app/pages目录下，生成下面的内容：

```bash
app/pages/blog-detail/
├── blog-detail.html
├── blog-detail.ts
└── blog-detail.scss
```

我们可以遵循之前添加Django App的习惯，先添加Router。因此我们可以在``app.ts``添加新的Route:

```
const ROUTES = [
  {path: '/app/blog/:id', component: BlogDetailPage}
];

@App({
  template: '<ion-nav [root]="rootPage"></ion-nav>',
  config: {} 
})
@RouteConfig(ROUTES)
export class MyApp {
  rootPage:any = TabsPage;

  constructor(platform:Platform) {
    this.rootPage = TabsPage;
    this.initializeApp(platform)
  }


  private initializeApp(platform:Platform) {
    platform.ready().then(() => {
      StatusBar.styleDefault();
    });
  }
}
```

我们用RouteConfig来关联我们的URL和App Component。

同上面的博客列表页面一样，我们也可以直接添加我们的API服务。有所区别的是，我们需要依据id去获取我们的博客内容。

```javascript

  getBlogpostDetail(id) {
    var url = 'http://localhost:8000/api/blogpost/' + id + '?format=json';
    return this.http.get(url).map(res => res);
  }
```  

和之前的博客列表一样，我们需要几乎一样的方法来获取数据：

```javascript
import {Page, NavController, NavParams} from 'ionic-angular';
import {BlogpostServices} from "../../services/BlogpostServices";

@Page({
  templateUrl: 'build/pages/blog-detail/blog-detail.html',
  providers: [BlogpostServices]
})
export class BlogDetailPage {
  private navParams;
  private blogServices;
  private blogpost;

  constructor(public nav:NavController, navParams:NavParams, blogServices:BlogpostServices) {
    this.nav = nav;
    this.navParams = navParams;
    this.blogServices = blogServices;

    this.initService();
  }

  private initService() {
    let id = this.navParams.get('id');
    this.blogServices.getBlogpostDetail(id).subscribe(
      data => {
        this.blogpost = JSON.parse(data._body);
        console.log(this.blogpost);
      },
      err => console.log('Error: ' + JSON.stringify(err)),
      () => console.log('Get Blogpost')
    );
  }
}
```

现在我们几乎已经完成了博客详情页的工作，我们可以直接通过URL来访问博客详情页：[http://localhost:8100/#/app/blog/1](http://localhost:8100/#/app/blog/1)。结果如下图所示：

![访问博客详情页](./images/blog-detail-page.png)

不过，这时候我们的列表页并没有和详情页关联到一起。我们还需要做一些额外的工作：

 - 在列表页的每一项中添加对点击事件的处理

在我们的模板页里ion-item里添加一个click事件，这个事件将调用navigate函数，并把博客id传到这个函数里。

```html
<ion-item *ngFor="#blogpost of blogposts" (click)="navigate(blogpost.id)">
  <h1 *ngIf="blogpost">
    {{blogpost.title}}
  </h1>

  <p *ngIf="blogpost">
    {{blogpost.body}}
  </p>
</ion-item>
```

随后在我们的博客详情页的初始化里，我们要初始化一个NavController:

```javscript
constructor(nav: NavController, blogpostServices:BlogpostServices) {
    this.blogListService = blogpostServices;
    this.nav = nav;
    this.initService();
  }
```

接着，在navigate里我们只需要将BlogDetailPage页面及参数push给navController，并交由它来渲染页面。

```javascript
navigate(id){
  this.nav.push(BlogDetailPage, {
    id: id
  });
}
```  

现在，我们可以试试从首页跳转到这个博客详情页。

Profile
---

现在，我们要做一个更有意思的东西了。不过这个内容是为后面的创建文章提供一个技术基础。在用户授权这一部分，我们使用不同的技术来实现，如Cookies、HTTP基本认证等等。而在手机端继续Cookie来进行用户授权，不是一件简单的事。因此我们就需要JSON Web Tokens，这是一种基于token 的认证方案。

###Json Web Tokens

同样，为了实现这部分功能，我们仍然可以使用其他框架来帮助我们完成基础功能。这里我们就用到了一个名为``djangorestframework-jwt``的库，从它的名字上我们就可以知道，它就是基于Django REST Framework之上的JWT实现。还是继续使用pip来安装这个库，记得把它添加到``requirements.txt``中。

```
pip install djangorestframework-jwt
```

接着，我们需要在我们的URL中配置用于获取token的API即可使用。

```
urlpatterns = patterns(
    '',
    # ...

    url(r'^api-token-auth/', 'rest_framework_jwt.views.obtain_jwt_token'),
)
```

在我们完成了上面的步骤之后，我们可以用``curl``命令或者Chrome浏览器的Postman来做测试：

 - 向服务器发送我们的用户名和密码，获取对应的Token。

如下是``curl``创建的请求，在这其中我们发送了我们的用户和密码。

```
curl -H "Content-Type: application/json" -X POST -d '{"username":"admin","password":"root"}' http://localhost:8000/api-token-auth
``` 

然后服务端给我们返回了对应的Token，它可以用于后面的创建文章、获取用户信息等等的功能。下面是一个Token的示例：

```
{"token":"eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJlbWFpbCI6ImhAcGhvZGFsLmNvbSIsInVzZXJfaWQiOjIsInVzZXJuYW1lIjoiYWRtaW4iLCJleHAiOjE0NjQ4NzQ1MDZ9.B5LEeIlGDTGggD6dh9akGRKx0Hk09wjylQRLas6kjGM"}
```

### 登录表单

现在，我们要先做的一件事就是，创建一个用于登录的表单。

```
<form #loginCreds="ngForm" (ngSubmit)="login(loginCreds.value)">
  <ion-item>
    <ion-label>Username</ion-label>
    <ion-input type="text" ngControl="username"></ion-input>
  </ion-item>

  <ion-item>
    <ion-label>Password</ion-label>
    <ion-input type="password" ngControl="password"></ion-input>
  </ion-item>

  <div padding>
    <button block type="submit">登录</button>
  </div>
</form>
```

我们创建一个名为``loginCreds``的ngForm，在我们提交的时候我们就调用login方法，并把其中的值（username、password）传过去。而在我们的代码里，我们所要做的就是和上面一样将数据post到之前的Auth API的地址：

```
constructor(http: Http, nav:NavController) {
  this.nav = nav;
  this.http = http;
  this.local.get('id_token').then(
    (data) => {
      this.user = this.jwtHelper.decodeToken(data).username;
    }
  );
}

login(credentials) {
  this.contentHeader = new Headers({"Content-Type": "application/json"});
  this.http.post(this.LOGIN_URL, JSON.stringify(credentials), {headers: this.contentHeader})
    .map(res => res.json())
    .subscribe(
      data => this.authSuccess(data.token),
      err => console.log(err)
    );
}

authSuccess(token) {
  this.local.set('id_token', token);
  this.user = this.jwtHelper.decodeToken(token).username;
}
```

在我们成功的获取到Token的时候，保存这个Token，并调用jwtHelper来解码Token，并从中获取我们的username。

同时，对于我们来说要登出就是一件容易的，删除这个token，将用户名清空。
```
logout() {
  this.local.remove('id_token');
  this.user = null;
}
```

### Profile

当我们获取到这个Token，我们也可以顺便获取用户的用户名、邮件等等的信息给用户。我们所要做的就是再获取一次API，但是在获取这次API的时候，我们需要上传我们的Token。因此我们需要一个简单的AuthHelper来帮助我们。

####AuthHttp

虽然我们要做的仅仅只是在我们的Header中，添加一个字段，它的值就是Token的值。但是这部分的逻辑交给Angular2-JWT来做可能会好一点，它提供了一个AuthHTTP方法可以让每次请求都带上这个Header。首先我们需要安装这个库：

```
npm install angular2-jwt
```

然后在我们``app.ts``中添加这个provider，并指明它的header前缀是JWT。

```javascript
@App({
  template: '<ion-nav [root]="rootPage"></ion-nav>',
  config: {}, // http://ionicframework.com/docs/v2/api/config/Config/
  providers: [
    provide(AuthHttp, {
      useFactory: (http) => {
        var authConfig = new AuthConfig({headerPrefix: 'JWT'});
        return new AuthHttp(authConfig, http);
      },
      deps: [Http]
    })
  ]
})
```

现在，我们就可以用和Http一样的方式去获取用户信息。

#### 获取用户信息

现在我们所需要做的就是发出我们的API去获取用户的信息：

```javascript
authSuccess(token) {
  this.local.set('id_token', token);
  this.user = this.jwtHelper.decodeToken(token).username;
  let params:URLSearchParams = new URLSearchParams();
  params.set('username', this.user);

  this.authHttp.request('http://localhost:8000/api/user/', {
      search: params
    })
    .map(res => res.text())
    .subscribe(
      data => this.local.set('user_info', JSON.stringify(JSON.parse(data)[0])),
      err => console.log(err)
    );
}
```

只是我们的API似乎还不支持这样的功能。它的实现方式和我们之前的AutoComplete是一样的，也是搜索用户名：

```python
def list(self, request):
    search_param = self.request.query_params.get('username', None)
    if search_param is not None:
        queryset = User.objects.filter(username__contains=search_param)

    serializer = UserSerializer(queryset, many=True)
    return Response(serializer.data)
```


然后再显示这些数据：

```
<div *ngIf="auth.authenticated()">
  <div padding>
    <h1>Welcome, {{ user }}</h1>
    <h2 *ngIf="user_info">Last login {{user_info.last_login}}</h2>
    <button block (click)="logout()">Logout</button>
  </div>
</div>
```  

不过由于我们Django自带的用户管理模块只有这点信息，我们也就只能显示这些信息了。下一步，我们就可以实现在我们的APP里去创建博客。

创建博客
---

在我们开始在APP端实现这个功能之前，我们先要实现一个高级点的用户授权管理——即只有用户登录或者用户的请求中带有Token的时候，我们才能创建博客。于是在这里我们所要做的就是实现一个``IsAuthenticatedOrReadOnly``，来判断用户是否有权限，如果没有的话，那么只让用户看到博客的内容。代码如下所示：

```python
SAFE_METHODS = ['GET', 'HEAD', 'OPTIONS']

class IsAuthenticatedOrReadOnly(BasePermission):
    """
    The request is authenticated as a user, or is a read-only request.
    """

    def has_permission(self, request, view):
        if (request.method in SAFE_METHODS or
            request.user and
            request.user.is_authenticated()):
            return True
        return False

class BlogpsotSerializer(serializers.HyperlinkedModelSerializer):
    class Meta:
        model = Blogpost
        fields = ('title', 'author', 'body', 'slug', 'id')


# ViewSets define the view behavior.
class BlogpostSet(viewsets.ModelViewSet):
    permission_classes = (permissions.IsAuthenticatedOrReadOnly,)
    queryset = Blogpost.objects.all()
    serializer_class = BlogpsotSerializer

```        

接着，我们可以创建一个Modal来做这个工作。对于我们的博客表单来说，和登录没有太大的区别。

```
  <form #blogpostForm="ngForm" (ngSubmit)="create(blogpostForm.value)">
    <ion-item>
      <ion-label>标题</ion-label>
      <ion-input type="text" ngControl="title"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label>作者</ion-label>
      <ion-input type="text" ngControl="author"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label>URL</ion-label>
      <ion-input type="text" ngControl="slug"></ion-input>
    </ion-item>

    <ion-item>
      <ion-label>内容</ion-label>
      <ion-textarea type="text" ngControl="body"></ion-textarea>
    </ion-item>

    <div padding>
      <button block type="submit">创建</button>
    </div>

  </form>
```  

稍有不同的是在我们的标题栏里会有一个关闭按钮。

```
<ion-toolbar>
  <ion-title>
    创建博客
  </ion-title>
  <ion-buttons start>
    <button (click)="close()">
      <span primary showWhen="ios">取消</span>
      <ion-icon name="md-close" showWhen="android,windows"></ion-icon>
    </button>
  </ion-buttons>
</ion-toolbar>
```

对于我们的实现代码来说，也是类似的，除了我们在发表成功的时候做的事情不一样——关闭这个Modal。

```
  close() {
    this.viewCtrl.dismiss();
  }

  create(value) {
    this.contentHeader = new Headers({"Content-Type": "application/json"});
    this.authHttp.post('http://127.0.0.1:8000/api/blogpost/', JSON.stringify(value), {headers: this.contentHeader})
      .map(res => res.json())
      .subscribe(
        data => this.postSuccess(data),
        err => console.log(err)
      );
  }

  postSuccess(data) {
    this.close()
  }
```  

同时，我们需要在我们的首页里添加这样的一个入口。

```
  <button fab fab-bottom fab-right (click)="createBlog()" calm>
    <ion-icon name="add"></ion-icon>
  </button>
```

以及它的处理逻辑：

```
  createBlog() {
    let modal = Modal.create(CreateBlogModal);
    this.nav.present(modal)
  }
```
  


