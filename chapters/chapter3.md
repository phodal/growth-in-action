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

这时你可能会产生一些疑惑，这些内容我们不是已经测试过了么？

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

搭建持续集成
---



