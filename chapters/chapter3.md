功能测试与搭建持续集成
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

在setUp——即开始的时候，我们会用selenium起一个Firefox浏览器的进程，并执行maximize_window来将窗口最大化。在tearDown——即结束的时候，我们就会关闭这个浏览器的进程。我们的主要测试代码就在``test_visit_homepage``这个方法里，我们在里面访问首页，并判断标题是不是``Welcome to my blog``。

运行上面的测试就会启动一个浏览器，并且会在浏览器上进行相应的操作。如下图所示：

![Selenium Demo](images/selenium-demo.jpg)

这时你可能会产生一些疑惑，这些内容我们不是已经测试过了么？两者从测试看是差不多的，但是从流程上看来说并不是如些。下图是页面渲染的时间线：

![Page Timing Overview](images/page-timing-overview.png)

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



