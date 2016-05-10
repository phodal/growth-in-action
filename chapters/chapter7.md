移动应用
===

Ionic 2
---

开始之前我们需要先安装Ionic的命令行工具，后来我们需要用这个工具来创建工程。

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

Login
---

###Json Web Tokens

```
pip install djangorestframework-jwt
```

```
urlpatterns = patterns(
    '',
    # ...

    url(r'^api-token-auth/', 'rest_framework_jwt.views.obtain_jwt_token'),
)
```

TODO
---

