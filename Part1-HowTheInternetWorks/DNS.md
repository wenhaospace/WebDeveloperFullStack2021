# DNS

## 1. 什么是DNS ?

域名系统 (DNS) 是 Internet 电话簿。人们通过例如 nytimes.com 或 espn.com 等域名在线访问信息。Web 浏览器通过 Internet 协议 (IP) 地址进行交互。DNS 将域名转换为 [IP 地址](https://www.cloudflare.com/zh-cn/learning/dns/glossary/what-is-my-ip-address)，以便浏览器能够加载 Internet 资源。

连接到 Internet 的每个设备都有一个唯一 IP 地址，其他计算机可使用该 IP 地址查找此设备。DNS 服务器使人们无需存储例如 192.168.1.1（IPv4 中）等 IP 地址或更复杂的较新字母数字 IP 地址，例如 2400:cb00:2048:1::c629:d7a2（IPv6 中）。

### 1.1 DNS如何工作 ？

DNS 解析过程涉及将主机名（例如 www.example.com）转换为计算机友好的 IP 地址（例如 192.168.1.1）。Internet 上的每个设备都被分配了一个 IP 地址，必须有该地址才能找到相应的 Internet 设备 - 就像使用街道地址来查找特定住所一样。当用户想要加载网页时，用户在 Web 浏览器中键入的内容（example.com）与查找 example.com 网页所需的机器友好地址之间必须进行转换。

为理解 DNS 解析过程，务必了解 DNS 查询必须经过的不同硬件组件。对于 Web 浏览器，DNS 查找在“幕后”进行，除了初始请求外，不需要从用户的计算机进行任何交互。

### 1.2 加载网页涉及4个DNS服务器

- **DNS 解析器** - 该解析器可被视为被要求去图书馆的某个地方查找特定图书的图书馆员。DNS 解析器是一种服务器，旨在通过 Web 浏览器等应用程序接收客户端计算机的查询。然后，解析器一般负责发出其他请求，以便满足客户端的 DNS 查询。
- **根域名服务器** - 根域名服务器是将人类可读的主机名转换（解析）为 IP 地址的第一步。可将其视为指向不同书架的图书馆中的索引 - 一般其作为对其他更具体位置的引用。
- **TLD 域名服务器** - 顶级域服务器（TLD）可被视为图书馆中的特定书架。此域名服务器是搜索特定 IP 地址的下一步，其托管主机名的最后一部分（在 example.com 中，TLD 服务器为 “com”）。
- **权威性域名服务器** - 可将这个最终域名服务器视为书架上的字典，其中特定名称可被转换成其定义。权威性域名服务器是域名服务器查询中的最后一站。如果权威性域名服务器能够访问请求的记录，则其会将已请求主机名的 IP 地址返回到发出初始请求的 DNS 解析器（图书管理员）。

### 1.3 权威性 DNS 服务器与递归 DNS 解析器之间的区别是什么？

这两个概念都是指 DNS 基础设施不可或缺的服务器（服务器组），但各自担当不同的角色，并且位于 DNS 查询管道内的不同位置。考虑二者差异的一种方式是，[递归](https://www.cloudflare.com/zh-cn/learning/dns/what-is-recursive-dns)解析器位于 DNS 查询的开头，而权威性域名服务器位于末尾。

#### 1.3.1 递归 DNS 解析器

递归解析器是一种计算机，其响应来自客户端的递归请求并花时间追踪 [DNS 记录](https://www.cloudflare.com/zh-cn/learning/dns/dns-records)。为执行此操作，其发出一系列请求，直至到达用于所请求的记录的权威性 DNS 域名服务器为止（或者超时，或者如果未找到记录，则返回错误）。幸运的是，递归 DNS 解析器并不总是需要发出多个请求才能追踪响应客户端所需的记录；[缓存](https://www.cloudflare.com/zh-cn/learning/cdn/what-is-caching)是一种数据持久性过程，可通过在 DNS 查找中更早地服务于所请求的资源记录来为所需的请求提供捷径。

![DNS 的工作原理 - DNS 查询的 10 个步骤](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-record-request-sequence-1.png)

#### 1.3.2 权威性 DNS 服务器

简言之，权威性 DNS 服务器是实际持有并负责 DNS 资源记录的服务器。这是位于 DNS 查找链底部的服务器，其将使用所查询的资源记录进行响应，从而最终允许发出请求的 Web 浏览器达到访问网站或其他 Web 资源所需的 IP 地址。权威性域名服务器从自身数据满足查询需求，无需查询其他来源，因为这是某些 DNS 记录的最终真实来源。

![DNS 查询图](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-record-request-sequence-2.png)

值得一提的是，在查询对象为子域（例如 foo.example.com 或 [blog.cloudflare.com](https://blog.cloudflare.com/)）的情况下，将向权威性域名服务器之后的序列添加一个附加域名服务器，其负责存储该子域的 [CNAME 记录](https://www.cloudflare.com/zh-cn/learning/dns/dns-records/dns-cname-record)。

![DNS 查询图](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-record-request-sequence-3.png)

### 1.4 DNS查找的步骤

大多数情况下，DNS 与正被转换为相应 IP 地址的域名有关。要了解此过程的工作方式，在 DNS 查找从 Web 浏览器经过 DNS 查找过程然后再返回时，跟踪 DNS 查找的路径会有所帮助。我们来看一下这些步骤。

注意：通常，DNS 查找信息将本地缓存在查询计算机内，或者远程缓存在 DNS 基础设施内。DNS 查找通常有 8 个步骤。缓存 DNS 信息时，将从 DNS 查找过程中跳过一些步骤，从而使该过程更快。以下示例概述了**不缓存任何内容**时的所有 8 个步骤。

#### 1.4.1 DNS查找的8个步骤

1. 用户在 Web 浏览器中键入 “example.com”，查询传输到 Internet 中，并被 DNS 递归解析器接收。
2. 接着，解析器查询 DNS 根域名服务器（.）。
3. 然后，根服务器使用存储其域信息的顶级域（TLD）DNS 服务器（例如 .com 或 .net）的地址响应该解析器。在搜索 example.com 时，我们的请求指向 .com TLD。
4. 然后，解析器向 .com TLD 发出请求。
5. TLD 服务器随后使用该域的域名服务器 example.com 的 IP 地址进行响应。
6. 最后，递归解析器将查询发送到域的域名服务器。
7. example.com 的 IP 地址而后从域名服务器返回解析器。
8. 然后 DNS 解析器使用最初请求的域的 IP 地址响应 Web 浏览器。
9. DNS 查找的这 8 个步骤返回 example.com 的 IP 地址后，浏览器便能发出对该网页的请求：

10. 浏览器向该 IP 地址发出[ HTTP](https://www.cloudflare.com/zh-cn/learning/ddos/glossary/hypertext-transfer-protocol-http) 请求。
11. 位于该 IP 的服务器返回将在浏览器中呈现的网页（第 10 步）。

![DNS 查询图](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-lookup-diagram.png)

### 1.5 什么是DNS解析器 ？

DNS 解析器是 DNS 查找的第一站，其负责与发出初始请求的客户端打交道。解析器启动查询序列，最终使 URL 转换为必要的 IP 地址。

注意：典型的未缓存 DNS 查找将涉及递归查询和迭代查询。

务必区分[递归 DNS ](https://www.cloudflare.com/zh-cn/learning/dns/what-is-recursive-dns)查询和递归 DNS 解析器。该查询是指向需要解析该查询的 DNS 解析器发出的请求。DNS 递归解析器是一种计算机，其接受递归查询并通过发出必要的请求来处理响应。

![DNS 查询图](https://www.cloudflare.com/img/learning/dns/what-is-dns/dns-recursive-query.png)

### 1.6 DNS查询有哪些类型 ？

典型 DNS 查找中会出现三种类型的查询。通过组合使用这些查询，优化的 DNS 解析过程可缩短传输距离。在理想情况下，可以使用缓存的记录数据，从而使 DNS 域名服务器能够返回非递归查询。

#### 1.6.1 3种DNS查询类型

1. **递归查询** - 在递归查询中，DNS 客户端要求 DNS 服务器（一般为 DNS 递归解析器）将使用所请求的资源记录响应客户端，或者如果解析器无法找到该记录，则返回错误消息。
2. **迭代查询** - 在这种情况下，DNS 客户端将允许 DNS 服务器返回其能够给出的最佳应答。如果所查询的 DNS 服务器与查询名称不匹配，则其将返回对较低级别域名空间具有权威性的 DNS 服务器的引用。然后，DNS 客户端将对引用地址进行查询。此过程继续使用查询链中的其他 DNS 服务器，直至发生错误或超时为止。
3. **非递归查询** - 当 DNS 解析器客户端查询 DNS 服务器以获取其有权访问的记录时通常会进行此查询，因为其对该记录具有权威性，或者该记录存在于其缓存内。DNS 服务器通常会缓存 DNS 记录，以防止更多带宽消耗和上游服务器上的负载。

### 1.7 什么是 DNS 高速缓存？DNS 高速缓存发生在哪里？

缓存的目的是将数据临时存储在某个位置，从而提高数据请求的性能和可靠性。DNS 高速缓存涉及将数据存储在更靠近请求客户端的位置，以便能够更早地解析 DNS 查询，并且能够避免在 DNS 查找链中进一步向下的额外查询，从而缩短加载时间并减少带宽/CPU 消耗。DNS 数据可缓存到各种不同的位置上，每个位置均将存储 DNS 记录并保存由[生存时间（TTL）](https://www.cloudflare.com/zh-cn/learning/cdn/glossary/time-to-live-ttl)决定的一段时间。

#### 1.7.1 浏览器DNS缓存

现代 Web 浏览器设计为默认将 DNS 记录缓存一段时间。目的很明显；越靠近 Web 浏览器进行 DNS 缓存，为检查缓存并向 IP 地址发出正确请求而必须采取的处理步骤就越少。发出对 DNS 记录的请求时，浏览器缓存是针对所请求的记录而检查的第一个位置。

在 Chrome 浏览器中，您可以转到 chrome://net-internals/#dns 查看 DNS 缓存的状态。

#### 1.7.2 操作系统（OS）级 DNS 缓存

操作系统级 DNS 解析器是 DNS 查询离开您计算机前的第二站，也是本地最后一站。操作系统内旨在处理此查询的过程通常称为“存根解析器”或 DNS 客户端。当存根解析器获取来自某个应用程序的请求时，其首先检查自己的缓存，以便查看是否有此记录。如果没有，则将本地网络外部的 DNS 查询（设置了递归标记）发送到 Internet 服务提供商（ISP）内部的 DNS 递归解析器。

与先前所有步骤一样，当 ISP 内的递归解析器收到 DNS 查询时，其还将查看所请求的主机到 IP 地址转换是否已经存储在其本地持久性层中。

根据其缓存中具有的记录类型，递归解析器还具有其他功能：

1. 如果解析器没有 [A 记录](https://www.cloudflare.com/zh-cn/learning/dns/dns-records/dns-a-record)，但确实有针对权威性域名服务器的 [NS 记录](https://www.cloudflare.com/zh-cn/learning/dns/dns-records/dns-ns-record)，则其将直接查询这些域名服务器，从而绕过 DNS 查询中的几个步骤。此快捷方式可防止从根和 .com 域名服务器（在我们对 example.com 的搜索中）进行查找，并且有助于更快地解析 DNS 查询。
2. 如果解析器没有 NS 记录，它会向 TLD 服务器（本例中为 .com）发送查询，从而跳过根服务器。
3. 万一解析器没有指向 TLD 服务器的记录，其将查询根服务器。这种情况通常在清除了 DNS 高速缓存后发生。

----

## 2. DNS安全

### 2.1 DNS安全为什么非常重要 ？

几乎所有 Web 流量都需要标准 [DNS](https://www.cloudflare.com/zh-cn/learning/dns/what-is-dns) 查询，这为 DNS 攻击提供了机会，例如 DNS 劫持和[在途攻击](https://www.cloudflare.com/zh-cn/learning/security/threats/on-path-attack)。这些攻击可将网站的入站流量重定向到伪造的站点，从而收集敏感的用户信息并使企业承担主要责任。防御 DNS 威胁的最知名方式之一是采用 DNSSEC 协议。

### 2.2 什么是DNSSEC ?

与许多 Internet 协议一样，在设计 DNS 系统时并未考虑安全性，并且该系统存在一些设计限制。再加上技术进步，这些限制使攻击者很容易出于恶意劫持 DNS 查找，例如将用户发送到可分发[恶意软件](https://www.cloudflare.com/zh-cn/learning/ddos/glossary/malware)或收集个人信息的欺诈性网站。DNS 安全扩展 (DNSSEC) 是为缓解此问题而创建的安全协议。DNSSEC 通过对数据进行数字签名来防止攻击，以帮助确保其有效性。为确保进行安全查找，此签名必须在 DNS 查找过程的每个级别进行。

此签名过程类似于人们用笔签署法律文件；此人签署别人无法创建的唯一签名，并且法院专家能够查看该签名并验证文件是否由该人签署的。这些数字签名可确保数据未被篡改。

DNSSEC 在 DNS 的所有层中实施分层数字签名策略。例如，在 “google.com”查找中，[根 DNS 服务器](https://www.cloudflare.com/zh-cn/learning/dns/glossary/dns-root-server)将为 [.COM 域名服务器](https://www.cloudflare.com/zh-cn/learning/dns/dns-server-types#tld-nameserver)签写一个密钥，然后 .COM 域名服务器将为 google.com 的[权威性域名服务器](https://www.cloudflare.com/zh-cn/learning/dns/dns-server-types#authoritative-nameserver)签写一个密钥。

尽管更高的安全性始终是首选的，但 DNSSEC 旨在向后兼容，以确保传统 DNS 查找仍可正确解析，尽管这没有提高安全性。作为整体 Internet 安全策略的一部分，DNSSEC 应与其他安全措施配合使用，例如 [SSL/TLS](https://www.cloudflare.com/zh-cn/learning/cdn/cdn-ssl-tls-security)。

DNSSEC 创建了一个父子信任链，该链一直行进到[根区域](https://www.cloudflare.com/zh-cn/learning/dns/glossary/dns-zone)。在 DNS 的任何层上此信任链都不能受损，否则请求将受到中间人攻击。

要闭合信任链，需要对根区域本身进行验证（证明没有篡改或欺诈），这实际上是通过人工干预来完成的。有趣的是，在所谓的“根区域签名仪式”上，来自世界各地的某些人聚集在一起，以公开且经审核的方式签署根 DNSKEY RRset。

### 2.3 涉及 DNS 的常见攻击有哪些？

DNSSEC 是一种强大的安全协议，但不幸的是，它当前尚未得到普遍采用。缺乏普及再加上其他可能的漏洞，最重要的是 DNS 是大多数 Internet 请求不可或缺的一部分，这些使 DNS 成为恶意攻击的主要目标。攻击者发现了众多针对和利用 DNS 服务器的方法，以下是一些最常见的方法：

- **DNS 欺骗[/缓存中毒](https://www.cloudflare.com/zh-cn/learning/dns/dns-cache-poisoning)：**这是将伪造的 DNS 数据引入 DNS 解析器缓存中的攻击，其将导致解析器返回域的错误 [IP 地址](https://www.cloudflare.com/zh-cn/learning/dns/glossary/what-is-my-ip-address)。流量可能会被转移到恶意计算机或攻击者想要的其他任何位置，而不是前往正确网站；通常是用于恶意目的的原始站点副本，例如分发[恶意软件](https://www.cloudflare.com/zh-cn/learning/ddos/glossary/malware)或收集登录信息。

- **DNS 隧道：**这种攻击使用其他协议通过 DNS 查询和响应建立隧道。攻击者可以使用 SSH、[TCP](https://www.cloudflare.com/zh-cn/learning/ddos/glossary/tcp-ip) 或 [HTTP](https://www.cloudflare.com/zh-cn/learning/ddos/glossary/hypertext-transfer-protocol-http) 在大多数防火墙未察觉的情况下将恶意软件或被盗信息传递到 DNS 查询中。

- **DNS 劫持：**在 DNS 劫持中，攻击者将查询重定向到其他域名服务器。这可通过恶意软件或未经授权的 DNS 服务器修改来实现。尽管其结果与 DNS 欺骗的结果相似，但这是一种截然不同的攻击，因为其目标是域名服务器上网站的 DNS 记录，而不是解析器的高速缓存。

![DNS 劫持](https://www.cloudflare.com/img/learning/dns/dns-security/dns-hijacking.png)

- **NXDOMAIN 攻击：**这是一种 DNS 洪水攻击，攻击者利用请求淹没 DNS 服务器，从而请求不存在的记录，以试图导致合法流量的[拒绝服务](https://www.cloudflare.com/zh-cn/learning/ddos/glossary/denial-of-service)。这可使用复杂的攻击工具来实现，这些工具可为每个请求自动生成唯一子域。NXDOMAIN 攻击还可将递归解析器作为目标，目标是用垃圾请求填充解析器的高速缓存。

- **幻域攻击：**幻域攻击的结果与 DNS 解析器上的 NXDOMAIN 攻击的结果类似。攻击者设置了一堆“幻影”域服务器，这些服务器对请求的响应非常慢，或者根本不响应。然后解析器收到对这些域的大量请求，解析器被束缚起来，只能等待响应，从而导致性能降低和服务拒绝。

- **随机子域攻击：**在这种情况下，攻击者向一个合法站点的几个随机的不存在的子域发送 DNS 查询。其目标是为该域的权威性域名服务器创建拒绝服务，从而使其无法从域名服务器查找网站。其副作用是，为攻击者提供服务的 ISP 也可能会受到影响，因为其递归解析器的高速缓存将被加载错误请求。

- **域锁定攻击：**不良行为者会通过设置特殊域和解析器来与其他合法解析器建立 TCP 连接，从而策划这种攻击形式。当目标解析器发送请求时，这些域会发回缓慢的随机数据包流，从而占用解析器的资源。

- **基于僵尸网络的 CPE 攻击：**这些攻击是使用 CPE 设备（用户终端设备，这是服务提供商提供的供客户使用的硬件，例如调制解调器、路由器、机顶盒等）进行的。攻击者使 CPE 受损，这些设备成为[僵尸网络](https://www.cloudflare.com/zh-cn/learning/ddos/what-is-a-ddos-botnet)的一部分，用于对一个站点或域进行随机子域攻击。

----

## 3. DNS服务器类型

### 3.1 DNS 服务器有哪些不同类型？

所有 DNS 服务器都属于以下四个类别之一：递归解析器、根域名服务器、TLD 域名服务器和权威性域名服务器。在典型 DNS 查找中（当没有正在进行的[高速缓存](https://www.cloudflare.com/zh-cn/learning/cdn/what-is-caching)时），这四个 DNS 服务器协同工作来完成将指定[域](https://www.cloudflare.com/zh-cn/learning/dns/glossary/what-is-my-ip-address)的 [IP 地址](https://www.cloudflare.com/zh-cn/learning/dns/glossary/what-is-a-domain-name)提供给客户端的任务（客户端通常是一个存根解析器 - 内置于操作系统的简单解析器）。

### 3.2 什么是DNS递归解析器 ？

![DNS 递归解析器](https://www.cloudflare.com/img/learning/dns/dns-server-types/recursive-resolver.png)

递归解析器（也称为 DNS 解析器）是 DNS 查询中的第一站。递归解析器作为客户端与 DNS 域名服务器的中间人。从 Web 客户端收到 DNS 查询后，递归解析器将使用缓存的数据进行响应，或者将向根域名服务器发送请求，接着向 TLD 域名服务器发送另一个请求，然后向权威性域名服务器发送最后一个请求。收到来自包含已请求 IP 地址的权威性域名服务器的响应后，递归解析器将向客户端发送响应。

在此过程中，递归解析器将缓存从权威性域名服务器收到的信息。当一个客户端请求的域名 IP 地址是另一个客户端最近请求的 IP 地址时，解析器可绕过与域名服务器进行通信的过程，并仅从第二个客户端的缓存中为第一个客户端提供所请求的记录。

### 3.3 什么是 DNS 根域名服务器？

![DNS 根域名服务器](https://www.cloudflare.com/img/learning/dns/dns-server-types/root-nameserver.png)

每个递归解析器都知道 13 个 DNS 根域名服务器，它们是递归解析器搜寻 DNS 记录的第一站。根服务器接受包含域名的递归解析器的查询，根域名服务器根据该域的扩展名（.com、.net、.org 等），通过将递归解析器定向到 TLD 域名服务器进行响应。根域名服务器由一家名为 Internet 名称与数字地址分配机构（ICANN）的非营利组织进行监督。

注意，尽管有 13 个根域名服务器，但这并不意味着根域名服务器系统中只有 13 台计算机。根域名服务器有 13 种类型，但在世界各地每个类型都有多个副本，它们使用 [Anycast 路由](https://www.cloudflare.com/zh-cn/learning/cdn/glossary/anycast-network)来提供快速响应。如果您将根域名服务器的所有实例加在一起，您将有 632 台不同的服务器（截至 2016 年 10 月）。

### 3.4 什么是 TLD 域名服务器？

![DNS TLD 域名服务器](https://www.cloudflare.com/img/learning/dns/dns-server-types/tld-nameserver.png)

TLD 域名服务器维护共享通用域扩展名的所有域名的信息，例如 .com、.net 或 url 中最后一个点之后的任何内容。例如，.com TLD 域名服务器包含以“.com”结尾的每个网站的信息。如果用户正在搜索 google.com，则在收到来自根域名服务器的响应后，递归解析器将向 .com TLD 域名服务器发送查询，后者将通过针对该域的权威性域名服务器（见下文）进行响应。

TLD 域名服务器的管理由 Internet 编号分配机构（IANA）加以处理，其为 ICANN 的一个分支机构。IANA 将 TLD 服务器分为两个主要组：

- 通用顶级域：这些是非特定国家/地区的域，一些最知名的通用 TLD 包括 .com、.org、.net、.edu 和 .gov。
- 国家/地区代码顶级域：这些包括特定于某个国家/地区或州的任何域。例如，uk、.us、.ru 和 .jp 等。

对于基础设施域实际上存在第三个类别，但该类别几乎从不使用。此类别是为 .arpa 域创建的，该域是在创建新式 DNS 时使用的过渡域；其当今主要是具有历史意义。

### 3.5 什么是权威性域名服务器？

![DNS 权威性域名服务器](https://www.cloudflare.com/img/learning/dns/dns-server-types/authoritative-nameserver.png)

当递归解析器收到来自 TLD 域名服务器的响应时，该响应会将解析器定向到权威性域名服务器。权威性域名服务器通常是解析器查找 IP 地址过程中的最后一步。权威名称服务器包含特定于其服务域名的信息（例如，google.com）权威性域名服务器包含特定于其所服务的域名的信息（例如 google.com），并且它可为递归解析器提供在 [DNS A 记录](https://www.cloudflare.com/zh-cn/learning/dns/dns-records/dns-a-record)中找到的服务器的 IP 地址，或者如果该域具有 [CNAME 记录](https://www.cloudflare.com/zh-cn/learning/dns/dns-records/dns-cname-record)（别名），它将为递归解析器提供一个别名域，这时递归解析器将必须执行全新 DNS 查找，以便从权威性域名服务器获取记录（通常为包含 IP 地址的 A 记录）。[Cloudflare DNS](https://www.cloudflare.com/zh-cn/dns) 分发权威性域名服务器，这些域名服务器带有 Anycast 路由，可使其变得更可靠。

----

## 4. DNS记录

### 4.1 什么是DNS记录 ？

DNS 记录（又名区域文件）是位于权威[ DNS 服务器](https://www.cloudflare.com/zh-cn/learning/dns/dns-server-types)中的指令，提供一个域的相关信息，包括哪些[ IP 地址](https://www.cloudflare.com/zh-cn/learning/dns/glossary/what-is-my-ip-address)与该域关联，以及如何处理对该域的请求。这些记录由一系列以所谓的 DNS 语法编写的文本文件组成。DNS 语法只是用作命令的字符串，这些命令告诉 DNS 服务器执行什么操作。此外，所有 DNS 记录都有一个 [“TTL”](https://www.cloudflare.com/zh-cn/learning/cdn/glossary/time-to-live-ttl)，其代表生存时间，指示 DNS 服务器多久刷新一次该记录。

您可以将一组 DNS 记录想象成 Yelp 上的企业清单，该清单将为您提供有关企业的大量有用信息，例如该企业的位置、营业时间、提供的服务等。所有域都必须至少有一些基本 DNS 记录，以便使用户能够使用[域名](https://www.cloudflare.com/zh-cn/learning/dns/glossary/what-is-a-domain-name)来访问他们的网站，此外还有一些用于其他目的的可选记录。

### 4.2 最常见的DNS记录有哪些 ？

- **A 记录** - 保存域的 IP 地址的记录。
- **CNAME 记录** - 将一个域或子域转发到另一个域，不提供 IP 地址。
- **MX 记录** - 将邮件定向到电子邮件服务器。
- **TXT 记录** - 可使管理员在记录中存储文本注释。
- **NS 记录** - 存储 DNS 条目的名称服务器。
- **SOA 记录** - 存储域的管理信息。
- **SRV 记录** - 指定用于特定服务的端口。
- **PTR 记录** - 在反向查询中提供域名。

### 4.3 一些不太常用的 DNS 记录有哪些？

- **AFSDB 记录** - 此记录用于由 Carnegie Melon 开发的Andrew File System（AFS）的客户端。AFSDB 记录的功能是查找其他 AFS 单元。
- **APL 记录** -“地址前缀列表”是用于指定地址范围列表的实验记录。
- **CAA 记录** - 这是“证书颁发机构授权”记录，其可使域所有者声明哪些证书颁发机构可为该域颁发证书。如果不存在 CAA 记录，则任何人都可为该域颁发证书。子域还会继承这些记录。
- **DNSKEY 记录** -“DNS 密钥记录”包含用于验证[域名系统安全扩展（DNSSEC）](https://www.cloudflare.com/zh-cn/learning/dns/dns-security)签名的[公钥](https://www.cloudflare.com/zh-cn/learning/ssl/how-does-public-key-encryption-work)。
- **CDNSKEY 记录** - 这是 DNSKEY 记录的子副本，应将其转移给父级。
- **CERT 记录** -“证书记录”存储公钥证书。
- **DCHID 记录** -“DHCP 标识符”存储有关动态主机配置协议（DHCP）的信息，该协议是 IP 网络上使用的标准化网络协议。
- **DNAME 记录** -“委托名称”记录创建域别名，就像 CNAME一样，但是此别名也会重定向所有子域。例如，如果“example.com”的所有者购买了域名“website.net”并为其提供了指向“example.com”的 DNAME 记录，则该指针还将扩展到“blog.website.net”以及任何其他子域。
- **HIP 记录** - 此记录使用“主机身份协议”，这是一种分隔 IP 地址角色的方式；此记录最常用于移动计算。
- **IPSECKEY 记录** -“IPSEC 密钥”记录与 Internet 协议安全（IPSEC）、端到端安全协议框架以及 Internet 协议套件（TCP/IP）的一部分配合使用。
- **LOC 记录** -“位置”记录包含域的地理信息，采用经度和纬度坐标形式。
- **NAPTR 记录** - 可将“名称权威指针”记录与 [SRV 记录](https://www.cloudflare.com/zh-cn/learning/dns/dns-records/dns-srv-record)相结合，以便基于正则表达式动态创建指向的 URI。
- **NSEC 记录** -“下一安全记录”是 DNSSEC 的一部分，用于证明所请求的 DNS 资源记录不存在。
- **RRSIG 记录** -“资源记录签名”记录可存储用于根据 DNSSEC 对记录进行身份验证的数字签名。
- **RP 记录** - 这是“负责人”记录，其存储域负责人的电子邮件地址。
- **SSHFP 记录** - 该记录存储“SSH 公钥指纹”；SSH 代表安全外壳，这是一种可在不安全网络上实现安全通信的加密网络协议。

-----

# 参考文档

https://www.cloudflare.com/zh-cn/learning/

