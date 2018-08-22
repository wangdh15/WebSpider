## Ajax

在使用requests抓取网页的时候，得到的结果可能和在浏览器中看到的不一样，这是因为requests得到的都是原始的HTML文档，但是浏览器中的页面则是经过JS处理数据后的效果。

Ajax是一种异步加载方式，原始页面最初不会包含某些数据，原始页面加载完之后，回想服务器请求某个接口获取数据，然后数据才被处理从而呈现在网页。

Ajax不是一门编程语言，而是利用JS在保证页面不被刷新，页面链接不改变的情况下与服务器交换数据并更新部分网页的技术。

有了Ajax，页面在后台与服务器进行了数据交互，获取到数据之后，利用JS改变网页，从而更新网页内容。

### 基本原理

- 发送请求
- 解析内容
- 渲染页面

真实的数据是一次次Ajax请求得到的，想要抓取这些数据，需要知道这些请求是怎么发送的，发往哪里，发了哪些参数。知道了这些，就可以使用Python模拟这个操作。

### Ajax分析方法

#### 查看请求

借助浏览器的开发者工具，筛选xhr,然后可以看到这个请求的详细信息。

这样就可以各具请求头，和响应来进行数据的处理。一般需要观察Ajax请求的参数是怎么变化的，找到规律，并分析响应的规律，才能够进行处理。

## 动态渲染页面爬取

通过JS动态渲染出来的网页在原始HTML中几乎不包含任何数据，是通过后来JS渲染出来的。所以有些并不是通过Ajax实现的，需要其他方法进行爬取。

为了解决上述问题，我们可以直接使用模拟浏览器运行的方式来实现，这样就可以做到在浏览器中看到的是什么样，抓到的源码就是什么样，可见即可爬。这样就可以不用管页面内部的JS用了什么算法渲染页面。

Python提供了许多模拟浏览器运行的库，如`Selenium`,`Splash`

### 环境配置

先安装Chrome ,然后安装ChromeDriver.然后需要安装selenium库
`pip install selenium`

然后执行下面代码测试，应该能够输出一个空白的Chrome页面
```python
from selenium import webdriver
browser = webdriver.Chrome()
```

### 使用

#### 声明浏览器对象

selenium支持很多浏览器，Chrome、Firefox等
可以先初始化一个浏览器对象，然后调用浏览器对象，就可以执行各个动作来模拟浏览器操作。

```python
from selenium import webdriver

browser = webdriver.Chrome()
browser = webdriver.Firefox()
```

#### 访问页面

可以使用`get()`方法来请求页面，参数传入链接URL即可。

```python
from selenium import webdriver
browser = webdriver.Chrome()
browser.get('https://www.taobao.com')
print('browser.page_source')
browser.close()
```
#### 查找节点

selenium提供了一系列查找节点的方法，可以通过这些方法找到想要的节点，然后执行下一步的动作或提取信息。

- 单个节点

可以根据属性值来查找某一个节点，如`find_element_by_id('q')`就是查找id属性值为q的节点。还有各种通过各个属性的方法 如下所示：


```python
find_element_by_id
find_element_by_name
find_element_by_xpath
find_element_by_link_text
```

此外，还可以通过通用的方法`find_element()`，需要传入两个参数：查找方式By和值。实际上就是上面多种方法的通用版本。
```python
browser.find_element(By.ID , 'q')
```

- 多个节点

如果查找的目标在网页中只有一个，那么可以用`find_element()`方法，但是，如果有多个节点，那么需要使用`find_elements()`方法。使用方法和上述类似，但是返回值是列表类型，每一项都是一个节点。

#### 节点交互

Selenium可以驱动浏览器来执行一些操作，可以让浏览器模拟执行一写动作。常见的用法有：

- 输入文字时用`send_keys()`方法
- 清空文字时用`clear()`方法
- 点击按钮时用`click()`方法

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait
import time


browser = webdriver.Chrome()
browser.get('https://www.taobao.com')
input = browser.find_element_by_id('q')
input.send_keys('iPhone')
time.sleep(1)
input.clear()
input.send_keys('iPad')
button = browser.find_element_by_class_name('btn-search')
button.click()
```

先驱动浏览器打开淘宝，然后使用`find_element_by_id()`方法获取输入框，然后使用`send_keys()`方法输入文字，然后等待一秒后清空输入框，然后调用`seng_keys()`方法输入iPad，然后使用`find_element_by_class_name()`方法获取搜索按钮，最后调用`click()`方法来完成搜索动作。

#### 动作链

上面一些动作都是针对某一个节点执行的。但是还有一些操作是没有特定的执行对象，比如鼠标拖拽，键盘按键等，这些动作通过另一种方式来执行，就是动作链。

#### 执行JS

有些操作，Selenium API没有提供，如下拉进度条，这些可以通过直接模拟运行JS来完成，通过使用`execute_script()`方法即可实现。

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait
import time


browser = webdriver.Chrome()
browser.get('https://www.zhihu.com/explore')
browser.execute_script('window.scrollTo(0,document.body.scrollHeight)')
browser.execute_script('alert("To Bottom")')
```
利用`execute_script()`方法将进度条下拉倒最底部，然后弹出alert提示框。所以说有了这个方法，基本上所有的API没有的功能可以用执行JS的方式来实现了。

#### 获取节点信息

通过`page_source`属性可以获取网页的源代码，然后可以使用解析库来提取信息。

但是Selenium提供了节点选择的方法，返回类型是`WebElement`类型，也有相关的方法和属性来直接提取节点信息，如属性、文本等。

- 获取属性值

通过`get_attribute()`方法，然后传入要获取的属性名，就可以得到他的值。

- 获取文本值

每一个WebElement节点都有text属性，直接调用这个属性就可以得到节点内部的文本信息。

- 获取id、位置、标签名和大小

WebElement节点还有一些其他的属性，id属性可以获取节点的id，location属性可以获取节点在页面中的相对位置，tag_name属性可以获取标签名称，size属性可以获取节点的大小，也就是宽高。

####延时等待

在selenium中，`get()`方法会在网页框架加载结束后结束执行，此时如果获取page_source，可能并不是浏览器完全加载完成的页面，如果某些页面右额外的Ajax请求，我们在网页源码中也不一定能获得成功。所以需要延时等待一段时间，确保节点已经加载出来。

两种方式，隐式等待和显示等待

#### 前进后退

使用浏览器有前进和后退功能，selenium也可以完成。使用`back()`方法后退，`forward()`方法前进。

#### Cookies

可以对Cookies进行操作。如获取，添加，删除等。

```python
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait
import time


browser = webdriver.Chrome()
browser.get('https://www.zhihu.com/explore')
print(browser.get_cookies())
browser.add_cookie({'name':'name', 'domain':'www.zhihu.com', 'value':'germey'})
print(browser.get_cookies())
browser.delete_all_cookies()
print(browser.get_cookies())
```

先访问知乎，然后调用`get_cookies()`方法来获取多有的Cookies，然后添加一个Cookie,传入一个字典，然后调用`delete_all_cookies()`方法删除所有的Cookies

#### 选项卡管理

在访问网页的时候，开启一个个的选项卡，在Selenium中，也能够对选项卡进行操作。不过是通过执行JS脚本实现的。

