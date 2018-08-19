## 爬虫基础

### HTTP基本原理


URL (Unnform Resource Locater)： 统一资源定位符。

超文本(hypertext) ： 网页源吗HTML。

HTTP和HTTPS是访问资源需要的协议类型。

HTTP : Hyper Text Transfer Protocol.超文本传输协议。用于从网络传输超文本数据到本地浏览器的船宿协议。

HTTPS：HTTP的安全版，在HTTP下加入SSL层。传输内容通过SSL加密。

- 建立一个信息安全通道来保证数据传输的安全。
- 确认网站的真实性。

#### HTTP请求过程

在浏览器中输入一个URL，然后浏览器向网站发送了一个请求，网站服务器接收到了这个请求之后，进行处理和解析，然后返回对应的响应，传回浏览器，包含页面的源码等内容，然后浏览器进行解析，就呈现出了网页。



请求是客户端向服务器发出，包含四个部分

- 请求方法
- 请求的网址 即URL，唯一确定我们想要请求的资源。
- 请求头
- 请求体 : 一般承载的内容是POST请求中的表单数据，GET请求为空。

##### 请求方法

常见请求方法右两种：GET和POST

GET请求中的参数包含在URL中，数据可以在URL中看到，在浏览器中输入一个URL回车便是一个GET请求。提交的最所只有1024个字节。

POST请求大多数在表单提交时发起，数据通常以表单的形式传输，不会包含在URL中，包含在请求体中。如提交用户名和密码以及上传文件等。提交的数据没有限制。

##### 请求头

请求头是用来说明服务器要使用的附加信息，是重要的组成部分，在写爬虫的时候一般需要设定请求头。

- Accept ： 指定客户端可以接受那些信息。
- Accept-Language ：  客户端可接受的语言类型
- Accept-Encoding : 客户端可接受的内容编码
- Host : 请求资源的主机IP和端口号
- Cookie : 网站为了辨别用户进行会话跟踪而存储在用户本地的数据。主要功能是维持当前访问会话。如我们在访问网站输入用户名和密码之后，每次浏览器请求改站点的页面时，会在请求头上加上Cookie，并发动给服务器，服务器通过Cookie识别出是我们自己，并查出当前装填是登录状态，就返回登录之后才能够看到的内容。
- Referer : 标记这个请求是哪个页面发动过来的
- User-Agent : 使服务器识别客户使用的操作系统及版本，浏览器记版本信息。做爬虫加上此信息，尅伪装成流量奶器。
- Content-Type ; 表示具体请求中的媒体信息。如text/html代表HTML、image/gif代表GIF图片 等。在构造POST请求，需要使用正确定的Content-Type。

#### 响应

响应是服务端返回个客户端，可分为三部分：

- 响应状态码： 200代表服务器响应正常，404表示页面未找到，500表示服务器内部发生错误
- 响应头 ： 包含服务器对请求的应答信息
- 响应体： 响应的正文在响应体中。

**响应头**：

- Date ： 时间
- Last-Modified : 资源的最后修改时间
- Content-Encoding ： 响应内容的编码格式
- Server : 服务器的信息，名称版本号等
- Content-Type：文档类型
- Set-Cookie：设置Cookies。告诉浏览器将此内容放在Cookies中，下次请求携带Cookies请求

----

### 网页基础

网页可分为三大部分HTML、CSS和JavaScript。HTML定义了网页的内容和结构，CSS描述了网页的布局，JS定义了网页的行为。

- HTML：超文本标记语言
- CSS：层叠样式表。网页页面排版样式标准。规定整个网页的样式规则。
- JavaScript：简称JS。脚本语言，是网站能够呈现一些动态和交互的效果。

**节点和节点树** ： HTML中所有内容都是节点。可以将HTML文档看做树结构。

### 爬虫基本原理

爬虫是获取网页并提取保存信息的自动化程序。

但是有些时候爬虫爬取的内容和浏览器看到的内容不一样，这是因为整个网站是用js渲染的。HTML只是一个空壳。但是爬虫只会请求HTML。所以看不到网站的内容。

### 代理的基本原理

代理也叫作代理服务器(proxy server)，功能是代理网络用户去取得网络信息，相当于中转站。在请求一个网站的时候，如果设置了代理服务器，那么先向代理服务器发动请求，然后代理服务器将请求要到，再发送给本机。这样能够实现IP伪装。

在使用爬虫的过程中，如果由于爬虫爬去速度过快，那么可能会遇到一个IP被封锁的问题，这时候可以使用代理来隐藏IP。

