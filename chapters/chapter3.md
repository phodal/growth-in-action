自动化测试与持续集成
===

在上一章最后，我们写的测试可以算得上是单元测试，接着我们可以写一些自动化测试。

编写自动化测试
---

接着我们就可以用Selenium来做自动化测试。这是ThoughtWorks出品的一个强大的基于浏览器的开源自动化测试工具，它通常用来编写Web 应用的自动化测试。

### Selenium与第一个UI测试

先让我们来看一个自动化测试的例子：

```python
from django.test import LiveServerTestCase
from selenium import webdriver

class HomepageTestCase(LiveServerTestCase):
    def setUp(self):
        self.selenium = webdriver.Firefox()
        self.selenium.maximize_window()
        super(HomepageTestCase, self).setUp()

    def tearDown(self):
        self.selenium.quit()
        super(HomepageTestCase, self).tearDown()

    def test_visit_homepage(self):
        self.selenium.get(
            '%s%s' % (self.live_server_url,  "/")
        )

        self.assertIn("Welcome to my blog", self.selenium.title)
```

在setUp——即开始的时候，我们会用selenium启动一个Firefox浏览器的进程，并执行maximize_window来将窗口最大化。在tearDown——即结束的时候，我们就会关闭这个浏览器的进程。我们的主要测试代码就在``test_visit_homepage``这个方法里，我们在里面访问首页，并判断标题是不是``Welcome to my blog``。

运行上面的测试就会启动一个浏览器，并且会在浏览器上进行相应的操作。如下图所示：

![Selenium Demo](./images/selenium-demo.jpg)

这时你可能会产生一些疑惑，这些内容我们不是已经测试过了么？两者从测试看是差不多的，但是从流程上看来说并不是如些。下图是页面渲染的时间线：

![页面渲染时间线](./images/page-timing-overview.png)

请求从浏览器传到服务器要有一系列的过程，如重定向、缓存、DNS等等，最后直至返回对应的Response。我们用Django的测试框架只能实现到这一步，随后页面请请求对应的静态资料，再对页面进行渲染，在这个过程中页面的内容会发生一些变化。

为了避免页面的内容被替换掉，那么我们就需要对这部分内容进行测试。

如下的代码也是可以用于测试页面内容的代码：

```python
class BlogpostDetailCase(LiveServerTestCase):
    def setUp(self):
        Blogpost.objects.create(
            title='hello',
            author='admin',
            slug='this_is_a_test',
            body='This is a blog',
            posted=datetime.now
        )

        self.selenium = webdriver.Firefox()
        self.selenium.maximize_window()
        super(BlogpostDetailCase, self).setUp()

    def tearDown(self):
        self.selenium.quit()
        super(BlogpostDetailCase, self).tearDown()

    def test_visit_blog_post(self):
        self.selenium.get(
            '%s%s' % (self.live_server_url,  "/blog/this_is_a_test.html")
        )

        self.assertIn("hello", self.selenium.title)
```

虽然在这里我们要测试的只是页面的标题，而实际上我们要测试的是页面的元素是否存在。

同样的，我们也可以对博客的内容进行测试。这些稍有不同的是，我们更多地是要测试用户的行为，如我们在首页点击某个链接，那么我应该中转到对应的博客详情页，如下代码所示：

```python
class BlogpostFromHomepageCase(LiveServerTestCase):
    def setUp(self):
        Blogpost.objects.create(
            title='hello',
            author='admin',
            slug='this_is_a_test',
            body='This is a blog',
            posted=datetime.now
        )

        self.selenium = webdriver.Firefox()
        self.selenium.maximize_window()
        super(BlogpostFromHomepageCase, self).setUp()

    def tearDown(self):
        self.selenium.quit()
        super(BlogpostFromHomepageCase, self).tearDown()

    def test_visit_blog_post(self):
        self.selenium.get(
            '%s%s' % (self.live_server_url,  "/")
        )

        self.selenium.find_element_by_link_text("hello").click()
        self.assertIn("hello", self.selenium.title)
```

需要注意的是，如果我们的单元测试如果可以测试到页面的内容——即没有使用JavaScript对页面的内容进行修改，那么我们应该使用单元测试即可。如测试金字塔所说，越底层的测试越快。

在我们编写完这些测试后，我们就可以搭建好相应的持续集成来运行这些测试了。

搭建持续集成
---

这里我们将使用Jenkins来完成这部分的工作，它是一个用Java编写的开源的持续集成工具。

> 它提供了软件开发的持续集成服务。它运行在Servlet容器中（例如Apache Tomcat）。它支持软件配置管理（SCM）工具（包括AccuRev SCM、CVS、Subversion、Git、Perforce、Clearcase和和RTC），可以执行基于Apache Ant和Apache Maven的项目，以及任意的Shell脚本和Windows批处理命令。

要使用Jenkins，只需要从Jenkins的主页上([https://jenkins.io/](https://jenkins.io/))下载最新的 jenkins.war文件。然后运行

```bash
java -jar jenkins.war
```

便可以启动:

```bash
Running from: /Users/fdhuang/repractise/growth-ci/jenkins.war
webroot: $user.home/.jenkins
May 12, 2016 10:55:18 PM org.eclipse.jetty.util.log.JavaUtilLog info
INFO: Logging initialized @489ms
May 12, 2016 10:55:18 PM winstone.Logger logInternal
INFO: Beginning extraction from war file
May 12, 2016 10:55:20 PM org.eclipse.jetty.util.log.JavaUtilLog warn
WARNING: Empty contextPath
May 12, 2016 10:55:20 PM org.eclipse.jetty.util.log.JavaUtilLog info
INFO: jetty-9.2.z-SNAPSHOT
May 12, 2016 10:55:20 PM org.eclipse.jetty.util.log.JavaUtilLog info
INFO: NO JSP Support for /, did not find org.eclipse.jetty.jsp.JettyJspServlet
Jenkins home directory: /Users/fdhuang/.jenkins found at: $user.home/.jenkins
May 12, 2016 10:55:21 PM org.eclipse.jetty.util.log.JavaUtilLog info
INFO: Started w.@68c34b0{/,file:/Users/fdhuang/.jenkins/war/,AVAILABLE}{/Users/fdhuang/.jenkins/war}
May 12, 2016 10:55:21 PM org.eclipse.jetty.util.log.JavaUtilLog info
INFO: Started ServerConnector@733a9ac6{HTTP/1.1}{0.0.0.0:8080}
```

接着，打开[http://0.0.0.0:8080/](http://0.0.0.0:8080/)就可以进行后续的安装，如下图所示：

![Jenkins安装过程](./images/jenkins-install.jpg)

慢慢等其安装完成：

![Jenkins安装完成](./images/jenkins-getting-started.jpg)

等安装完成后，我们就可以开始使用Jenkins来创建我们的任务了。

### Jenkins创建任务

在首页，我们会看到“开始创建一个新任务”的提示，点击它。

源码管理中选择Git，并填入我们代码的地址：

```
[https://github.com/phodal/growth-in-action-python-code](https://github.com/phodal/growth-in-action-python-code)
```

如下图所示:

![Jenkins设计Repo](./images/jenkins-repo-setup.jpg)

然后就是构建触发器，一共有五种类型的触发器，意思也很容易理解：

 - 触发远程构建 (例如,使用脚本)
 - Build after other projects are built 
 - Build periodically
 - Build when a change is pushed to GitHub
 - Poll SCM

在这里，我们要使用的是GitHub这个，它的原理是:

> This job will be triggered if jenkins will receive PUSH GitHub hook from repo defined in scm section

即Jenkins在监听GitHub上对应的PUSH hook，当发生代码提交时，就会运行我们的测试。

由于，我们暂时不需要一些特殊的``构建环境``配置，我们就可以将这个放空。接着，我们就可以配置``构建``了。

### 创建shell

在这里我们需要添加的构建步骤是：``execute shell``，先让我们写一个简单的安装依赖的shell

```bash
virtualenv --distribute -p /usr/local/bin/python3.5 growth-django
source growth-django/bin/activate
pip install -r requirements.txt
```

然后在保存后，我们可以尝试立即构建这个项目：

![控制台输出](./images/build-console-ouput.jpg)

在编写shell的过程中，我们要经过一些尝试，在这其中会经历一些失败的情形——即使是大部分有相关经验的程序员。如下图就是一次编写构建脚本引起的构建失败的例子：

![Jenkins失败的构建](./images/jenkins-failure-setup.jpg)

最后，我们就得到下面的一个shell脚本，我们就可以将其变成相应的运行CI的脚本。以便于它可以在其他环境中使用：

```bash
#!/usr/bin/env bash
virtualenv --distribute -p /usr/local/bin/python3.5 growth-django
source growth-django/bin/activate
pip install -r requirements.txt
python manage.py test
python manage.py test test
```

记得给你的shell文件，加上执行的标志：

```
chmod u+x ./scripts/ci.sh
```

最后，我们就可以修改CI上相应的构建环境的配置。
