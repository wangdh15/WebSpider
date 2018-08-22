## 解析库bs4

用很多的对HTML文档进行解析的库，本文档就`beautifulsoup4`这个库进行说明。

### Install

```python
pip install beautifulsoup4
conda install beautifulsoup4
```

### 解析器

bs4在解析的时候依赖解析器，比较好用的解析器是lxml。

`pip install lxml`


### 简单使用
在使用的时候首先将要解析的HTML文档做成一个bs，在初始化的时候可以指定解析器，如下所示
```python 

from bs4 import BeautifulSoup

html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>

<p class="story">...</p>
"""
soup = BeaautifulSoup(html_doc , 'lxml')

```

这样就做了一个bs。

- prettity()方法   这个方法可以将要解析的字符串一标准的缩进格式输出。
- title.string    输出title节点的文本内容

可以直接使用节点来获得节点的名称和属性值。

获取节点的属性可以直接通过属性的名称，如果属性的值只有一个，那么返回这个属性值，如果属性值有多个，返回一个列表。

同时可以使用attrs来获得所有属性名称及其值，得到的是一个字典，每一项对应一个属性和属性值。

通过节点的额string属性可以获得节点的内容。例子中获取到的是第一个a节点的内容。

```python
soup.a.attrs
Out[16]: {'href': 'http://example.com/elsie', 'class': ['sister'], 'id': 'link1'}

soup.a['class']
Out[17]: ['sister']

soup.a.string
Out[18]: 'Elsie'
```
### 方法选择器

#### find_all()

API:`find_all(narne , attrs , recursive , text , **kwargs)`

查询所有符合条件的元素。

- name : 节点名，可以进行嵌套查询
- attrs ：属性，字典类型
- text：匹配节点中的文本，可以使字符串或者正则表达式，返回结果是匹配上的节点中的文本，而不是节点

#### find()

和find_all()类似，但是返回的是第一个节点，find_all返回的是一个列表，内容是所有满足条件的节点。

