title: "你能区别Internet和Web吗"
date: 2015-07-25 21:06:40
tags: ["Internet", "Web", "HTTP", "RESTFUL", "DNS"]
categories: "MISC"
---

之前看过一个很火的帖子，说从你在浏览器输入一个网址到网页呈现在你面前都经历了哪些步骤。脑洞大开的geek，技术宅等一批大神给出了不断丰富的答案。我觉得我也有必要借此好好了解一下这里面的细节。

<!-- more -->
请注意，前方大波词汇来袭！下面这些词汇你都了解吗？
`Client` 
`Web Server` 
`DNS` 
`Hosting` 
`IP address` 
`Mail Server` 
`HTTP` 
`HTTPs` 
`URLs` 
`Domain Name` 
`TLD` 
`IPv4` 
`IPv6` 
`TCP/IP` 
`Router` 
`Switch` 
`LAN` 
`VPS` 
`Dedicated Server` 
`FTP` 
`REST`
`RESTFUL`
`A record` 
`CNAME` 
`CDN`

***

如果有一些似曾相识，却又说不清楚，不妨接着往下看，给你一个basic idea。下面这张图大体解释了Internet和Web的不同。

![Internet vs Web](http://s7.computerhistory.org/is/image/CHM/500004969?$re-medium$)

# 先来说说Internet Architecture 
Internet一项最主要的功能就是实现计算机之间的**信息传递**。

> The backbone of the internet is made up of core routers, primary networks, and typically fiber optic cables connecting networks around the globe.

## Internet上的计算机分类
接入Internet的计算机中，有的request信息，有的对request进行反馈，也有的两者兼有。因此，接入Internet的计算机可以大致分为两类：Client & Server

### Client
任何能够在网络上请求资源、信息的设备都可以看做是客户端(Client)，比如手机，台式机，笔记本，平板电脑......

### Server
* 同个人设备或者说客户端相比，server具有以下特点：
  * Faster CPUs
  * Higher Performance RAM
  * More Storage
  * Backup Power
  * Backup Storage

* Server根据功能可以被分为:
 * Database Server
 * Mail Server: send and receive email messages 
 * File Server
 * Web Server: host website
 * Game Server
 * DNS Server (后面具体展开)
 * ......

### Web Hosting
如果自己建站，你还需要把你的网站存放到server上，别人才能看到。
web hosting的几种方式：
* Shared hosting: 经济型，适用于访问量不大的情形。很多网站并行放在一个计算机集群。
* VPS: 即Virtual Private Server，相比第一种提供更多空间，更快速度... 拥有独立虚拟计算机，可配置操作系统，更高灵活性
* Dedicated Server

### FTP
File Transfer Protocol，即文件传输协议，是用于在网络上进行文件传输的一套标准协议。它属于网络传输协议的应用层。

sftp是一种更为安全的传输协议，S表示Security。

常用的FTP客户端：
* Cyberduck
* Filezilla

### TCP/IP
前面说到了Client-Server模型，那么信息是如何在他们之间实现传递的呢？为了能够让网络上的计算机之间都能够相互交流，就需要制定一个标准，就好比大家都说普通话才能顺畅交流一样。这样就出现了TCP/IP协议。
* TCP - Transmissionn Control Protocol 
* IP - Internet Protocol

当我们发送一个请求的时候，根据TCP我们可以把**数据**(网页、视频、邮件...)碎片化即`packets`。这些`packets`通过不同的渠道路径以最快的速度到达目的地，然后再通过TCP按照正确的顺序还原真实数据。所以说TCP就是指导怎样打散数据以及还原数据，而IP则指导`packets`该去向哪里以及返回地址。

## 如何连入Internet
一个一般的过程为：
`Your Computer` =(request)=> `Router` ==> `Modem` ==> `ISP` ==> `Internet`

### Switch
* 交换机是通过电缆将不同设备连接起来，构成网络。这种连接方式称为物理连接。
* 在学校，机房以及办公室通过交换机连接起来的网络就叫做 - 局域网(LAN)

### LAN
* local area network，即局域网。是通过switch连接起来的。

### Router
* 路由器的作用就是把data packets从局域网(local network)传送出去, 比如从A点传送到B点。
* 如同client-server之间的传输一样，路由器之间的传输也要遵循相应的protocol(TCP/IP)。
* 路由器智能的地方就是当你想把数据从A传到B时，他会试图寻找最短的路径。通过一系列路由器之间的传递，到达最终目的地。这也是路由器和交换机的一个区别。

### Modem
Modem中文即调制解调器。顾名思义有两个功能**调制**和**解调**。
* 调制 - 将计算机上的数字信号(digital data)调制成模拟载波信号传送出去
* 解调 - 将收到的模拟信号解调成数字信号返回给计算机

Modem会将`packets`通过同轴电缆(coaxial cable)或者电话线又或者通过无线方式传送给ISP。

两种常见Modem：
* DSL
* Cable

### ISP
Internet Service Provider，即互联网服务供应商。

***

## 再来说说Web
Web(万维网，World Wide Web的简称)是Internet的一部分。我们今天用浏览器上网，浏览网页，就是在用Web。

### URLs
Uniform Resource Locators, 即统一资源定位符，比如下面这个：
`http://www.jianshu.com/p/2b207971c2b1`
想要浏览网页，第一件事就是在地址栏输入URL，它包括：
  * Scheme or Protocol： `http`
  * Basic Destination (Domain Name or IP address)： `www.jianshu.com`
  * Resource Path (for a file)： `/p/2b207971c2b1`

### HTTP
Hypertext Transfer Protocol，即超文本传输协议。是web上客户端和服务器之间沟通要遵守的协议，通过HTTP或者HTTPS协议请求的资源由统一资源标识符(Uniform Resource Identifiers，URI)来标识。

* HTTP状态码
`1xx`: 信息
`2xx`: 成功
`3xx`: 重定向
`4xx`: 客户端错误
`5xx`: 服务器错误

* HTTP最基本的method：
`GET` 获取资源
`POST` 新建资源
`PUT` 更新资源
`DELETE` 删除资源

* HTTP请求有三部分：
  * Request line
 告知服务器**请求类型**(`GET`, `POST`....)以及**需要的资源**
  * Header
 客户端额外的信息
  * Body
对于`GET`方法为空，对于`POST`或者`PUT`方法，body里面即存放相关信息

### REST/RESRFUL
Representational State Transfer，即表现层状态转化。它是一种软件架构风格，一组架构约束条件和原则。满足这些约束条件和原则的应用程序或设计就叫RESTful。REST的名称"表现层状态转化"中，省略了主语。"表现层"其实指的是"资源"(Resources)的"表现层"。

说一个情形，你用google搜索`简书`，google会返回很多结果(hyperlink)，当你点击某一个连接，你就进入了另一个页面，这就是所谓的state transfer。上述过程就遵循了REST原则。

一个RESTful API要遵循：
* 客户端和服务器相分离
* 客户端和服务器之间的交互在请求之间是无状态的
* 使用HTTP和标准HTTP方法

### IP address
Internet Protocol address，即互联网协议地址。它就是给遵循IP协议的互联网上的设备的一个数字编号。

### IPv4
Internet Protocol version 4，由四个数字组成。从`0.0.0.0`到`255.255.255.255`能够表示43亿个不同的地址。

### IPv6
Internet Protocol version 6，由于IPv4已经不能满足需求。[wikipedia](https://zh.wikipedia.org/wiki/IPv6)

### Domain Name
Domain Name，即域名。相比较IP地址，它实际就是为我们提供了一种更易于管理和记忆的形式。如果你想登陆百度你可以输入`www.baidu.com`而不再是一串数字`139.168.2.13`

Domain Name的规则
* LDH Rules: 命名domain或者subdomain时，使用字母(Letter)、数字(Digit)和连接号(Hyphen)，但不能以hyphen开始或者结束。
* Domain Name是从右向左读的。
`subdomain` + `domain` + `TLD`
* Domain Name最多可以有127个level
e.g. ` www.jianshu.com`
e.g. `www.jianshu.example.com`
* Domain Name的总长度最多可以为253个字符，而且必须是ASCII码
* 推荐[Domainr](https://domainr.com/) 可以帮你选名字，查价格

### TLD
Top Level Domain，即顶级域名。由[ICCAN](https://www.icann.org/)管理和维护。
目前，有326个不同的TLD。
* 常见的TLD有：
`.com`  commercial
`.net` network
`.edu` education
`.org` organisation
`.gov` government
* 国家顶级域名
 `.uk` United Kingdom
 `.us` United State
 `.jp` Japan
 `.cn` China 
* 其它组织，品牌甚至虚拟名字
 `.online`
 `.inc`
 `.shopping`
 `.ninja`
 `.name`

## DNS
Domain Name System，即域名系统，它作为将**域名**和**IP地址**相互映射的一个分布式数据库，能够使人更方便的访问互联网。

* 不是每一个DNS都知道每一个域名和IP地址的对应关系
* 你可能通过不同的DNS找到`www.jianshu.com`，比如你在家里上网和在咖啡馆上网，使用的DNS可能就不同。
* DNS Hosting的不同类型
  * Registrar DNS Hosting
  * DNS Hosting Services
  * DNS Self Hosting

To be continued....
* Name Server
* Root Server
* TTL
* A record
* CNAME
* CDN: content delivery network