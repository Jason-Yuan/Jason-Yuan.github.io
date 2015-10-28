title: "说说OAuth那点事"
date: 2015-06-07 11:30:44
tags: ["OAUTH 2.0", "API"]
categories: "MISC"
---

今年年初，第一份实习，接触了如何使用Facebook API, Twitter API...去获取数据，自动发个Facebook什么的，也在那会听到了这个词-OAuth. 不明觉厉。无奈最近实习中又接触了，实在不想迷迷糊糊了，决心搞清楚，在拜读了各路大牛的文章之后，决定为自己写个总结。

<!-- more -->
***
![OAUTH](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-5eb7ffb79c457857_zpscxbzx7fz.png)

# 什么是OAuth ?
官网这样说...
> An **open protocol** to allow **secure authorization** in a **simple** and **standard** method from web, mobile and desktop applications.

* OAuth 即 **O**pen standard for **Auth**orization
* OAuth就是一个网络开放协议。为保证用户资源的安全授权提供了简易的标准
* 在知乎上看到了一个比较直观的比喻，有助于初学者理解。[知乎话题链接](http://www.zhihu.com/question/19781476)

***

# OAuth 历史版本
* **2007-12** OAuth 1.0发布并迅速成为工业标准。
* **2008-06** OAuth 1.0 Revision A发布，这是个稍作修改的修订版本，主要修正一个安全方面的漏洞。
* **2010-04**，OAuth 1.0 协议发布为 [RFC 5849](http://www.rfcreader.com/#rfc5849)
* **2011-05** OAuth 2.0 草案发布
* **2012-10** OAuth 2.0 协议发布为 [RFC 6749](http://www.rfcreader.com/#rfc6749)

***

# OAuth 1.0 
OAuth 1.0 协议过于复杂，易用性差，所以并没有得到普及，下文中给出了授权的流程图，可以简单了解了解，现在很少有用的了。

![OAuth Authentication Flow](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-7dacd5d8da5b7d87_zpszfxzrbzz.png)

关于OAuth 1.0， 想了解的可以看看这些：
* [RFC 5849](http://www.rfcreader.com/#rfc5849)
* [OAUTH协议简介](http://blog.csdn.net/hereweare2009/article/details/3968582)

***

# OAuth 2.0
OAuth 2.0是目前的最新版本，OAuth 2.0不兼容OAuth 1.0。这篇文章主要讲讲OAuth 2.0，并以此展开。

**先来说一个场景：**比如你第一次打开简书官网，想要注册一个账号。你会看到简书允许你通过新浪微博账号登陆。当你点击之后，需要你登陆微博。之后会出现“是否同意简书获取你的个人信息”等等，如果你选择授权，之后会跳转回简书。你会发现你在简书的用户名就是你微博的用户名。然后，你会发出一条新的微博比如“我加入了简书，一个基于内容分享的社区！”(这只是举个例子，不知道简书有没有这样做)。

好了，这回开始进入正题

## 角色(roles)
OAuth 2.0 定义了**四个角色**：
* **资源拥有者(Resource Owner)**
资源拥有者其实就是用户(user)，用户将会授权一个第三方应用可以获取他们的账户资源。当然第三方应用程序对于用户账户的操作是有限制的(比如，read access, read and write access)！这个限制就是用户授权时给予的权限范围(**scope**)
上面场景中，微博账户就是资源拥有者。read access就比如读取微博用户名，write access就比如以你的名义发了一个微博。

* **客户端(Client)**
客户端就是前面说的第三方应用程序，他们想要获取用户的账户资源，但在这么做之前必须经过授权
上面场景中，简书就是客户端

* **资源服务器(Resource Server)**
资源服务器存放用户账户以及账户信息和资源
上面场景中，新浪微博就是资源服务器，同时也是授权服务器

* **授权服务器(Authorization Server)**
授权服务器验证用户身份，并为第三方应用程序颁发授权令牌(access token)

资源服务器与授权服务器可以是同一台服务器，这里分开主要是便于解释清楚OAuth协议。从程序开发者的角度，这两个都是service's API会执行的事情。

在了解完OAuth中的四个角色之后，我们看看这四个角色之间是如何互动的。下面是基本运行流程。

![Abstract Protocol Flow](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-98fc867cefb75c57_zpscar1pcub.png)

1. 应用程序向用户请求给予授权，以便获取服务器资源
* 如果用户同意授权，应用程序将获得相应授权
* 应用程序向授权服务器提供自己的身份证明(app key和app secret)和已被授权的证明(authorization grant)，并请求访问令牌(access token)
* 如果应用程序的身份被核实，并且授权是有效地，那么授权服务器将会发放访问令牌给应用程序。**此时，授权完成**
* 应用程序向资源服务器出示访问令牌，并请求资源
* 如果访问令牌是有效的(比如：是否伪造，是否越权，是否过期)，资源服务器将会为应用程序提供资源

## 应用程序注册(Application Registration)
对于一个应用程序来说，如果它想要使用OAuth，那么首先它要在服务提供商那里注册。一般出现在网站的“developer”或者“API”部分。

应用程序要提供：
* 应用程序名称(Application Name)
* 应用程序网站(Application Website)
* 回调URL(Redirect URI or Callback URL)

在用户同意授权(或者拒绝)之后，服务提供商会将用户重新导向这个Callback URL，这个Callback URL要来负责处理授权码或者访问令牌。

应用程序注册完成之后，服务提供商会颁发给应用程序一个“客户端认证信息(client credentials)”。Client Credential包括：
* **Client Identifier**(client ID/API key/consumer key)
  * 提供给服务提供商，用于**识别**(identify)应用程序
  * 用于构建提供给用户请求授权的URLs
* **Client Secret**(secret key/API secret/consumer secret)
  * 提供给服务提供商，用于**验证**(authenticate)应用程序
  * 只有应用程序和服务提供商两者可知

## 授权许可类型(Authorization grant types)
### OAuth 2.0 定义了四种授权模式：
* **授权码模式**(Authorization Code)
used with server-side Applications
* **隐式授权模式**(Implicit)
used with Mobile Apps or Web Applications (applications that run on the user's device)
* **资源拥有者密码凭证模式**(Resource Owner Password Credentials)
used with trusted Applications, such as those owned by the service itself
* **客户端模式**(Client Credentials)
used with Applications API access 

## 授权码模式(Authorization Code)
* 授权码模式是最常见的一种授权模式，最为安全和完善。
* 对于服务器端应用程序(server-side application)来说，可以保证Client Secret的安全性。应用程序必须能够和用户代理(user agent)进行交互，获取API授权码。

![Authorization Code Flow](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-b055e7b2acc6c54f_zps2raapzht.png)

### 第一步：客户端把用户代理定向到授权终端(Authorizaiton Endpoint)
```
https://www.example.com/v1/oauth/authorize?response_type=code&client_id=CLIENT_ID&redirect_uri=CALLBACK_URL&scope=read
```

* **www.example.com/v1/oauth/authorize**: API授权的终端(Authorization Endpoint)
* **client_id**: 应用程序的client ID，用于API识别应用程序
* **redirect_uri**：获得授权码之后，服务提供商重定向用户代理(比如浏览器)的地址
* **response_type**：表明授权类型，默认值是code，即授权码模式(Authorization Code Grant)
* **scope**: 即应用程序可以获得的授权级别(access level)，默认值为read
* **state**: 表示客户端的当前状态，可以指定任意值，认证服务器会原封不动地返回这个值，用于抵制CSRF攻击。

### 第二步：用户授权应用程序
用户点击上述URI之后，用户首先要登陆，证明其身份。然后选择同意授权应用程序可以访问他们的账户或者拒绝。

![是否允许**简书**访问你的**微博账户**](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-5269226d7b91780e%201_zps3iqrusph.png)

### 第三步：应用程序获取授权码
如果用户同意授权，服务提供商会将用户代理重定向到第一步中的redirect_uri，并且会包含**授权码**。
```
https://www.jianshu.com/callback?code=AUTHORIZATION_CODE
```
### 第四步：应用程序请求授权令牌
应用程序向API token终端发送刚刚获得的授权码，以及认证信息。
```
https://www.example.com/v1/oauth/token?client_id=CLIENT_ID&client_secret=CLIENT_SECRET&grant_type=authorization_code&code=AUTHORIZATION_CODE&redirect_uri=CALLBACK_URL

```
URI中包括：
* **www.example.com/v1/oauth/token**： API token的终端
* **client_id**：即app key/consumer key，用于验证应用程序
* **client_secret**： 即app secret/consumer secret，用于验证应用程序
* **grant_type**：刚刚获得的授权码
* **redirect_uri**: 重定向URI，与第一步一致

### 第五步：应用程序获得授权令牌
如果上一步验证有效，API将返回一个HTTP回复。
```
{"access_token":"ACCESS_TOKEN","token_type":"bearer","expires_in":2592000,"refresh_token":"REFRESH_TOKEN","scope":"read"}
```

HTTP回复中包含：
* **access_token**：访问令牌
* **token_type**：令牌类型，bearer类型或mac类型。[详细](http://oauth2.thephpleague.com/token-types/)
* **expires_in**：过期时间，单位为秒。
* **refresh_token**：更新令牌，用来获取下一次的访问令牌，可选项。
* **scope**：权限范围，如果与客户端申请的范围一致，此项可省略。

## 隐式授权模式(Implicit)
* 隐式授权模式主要用于客户端应用程序(client-side application)，比如手机应用、桌面客户端应用程序和运行于浏览器上的web应用程序。
* 因为没有server端，client secret的保密性不能得到保证。授权令牌给予用户代理(比如，浏览器)，再由用户代理交给应用程序。所以用户设备上的其他应用程序同样可以得到授权令牌。
* 隐式授权模式中，应用程序不需要认证。
* 隐式授权模式不支持refresh token。

![Implicit Flow](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-434f3f6abe72b94f_zpskcsnxgfc.png)

### 第一步：客户端把用户代理定向到授权终端(Authorizaiton Endpoint)
* 授权终端是`https://www.example.com/authorize`
* 与授权码链接类似，只不过response_type=`token`而不是`code`

```
https://www.example.com/authorize?response_type=token&client_id=CLIENT_ID&redirect_uri=CALLBACK_URL&scope=read
```

### 第二步：用户授权应用程序
用户点击上述URI之后，用户首先要登陆，证明其身份。然后选择同意授权应用程序可访问他们的账户或者拒绝。

![是否允许**简书**访问你的**微博账户**](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-5269226d7b91780e%201_zps3iqrusph.png)

### 第三步：用户代理收到授权令牌
假设用户同意授权，授权服务器重定向用户代理到第一步提到的redirect_uri。并在URI fragment中包含授权令牌(但不能查看)。
```
https://www.example.com/callback#token=ACCESS_TOKEN
```

### 第四步：用户代理向资源服务器发出请求
用户代理依照重定向的指令，向资源服务器发出请求，但并不包含上一步中得到的授权令牌(#后面的部分)。用户代理将完整的重定向URI保存在本地。

### 第五步：资源服务器返回一个网页
资源服务器会返回一个网页(通常是一个HTML文件内嵌一段脚本)。这段内嵌的脚本(script)可以访问第三步中用户代理保存在本地的完整的重定向URI，并从中提取授权令牌。

### 第六步：用户代理提取授权令牌
用户代理执行上面提到的脚本，提取出授权令牌。然后将授权令牌传递给应用程序。

## 资源拥有者密码凭证模式(Resource Owner Password Credentials)
* 在资源拥有者密码凭证模式中，用户直接向应用程序提供其认证信息(即用户名和密码)。应用程序依此向授权服务器获取授权令牌。
* 这种模式适用于用户对应用程序高度信任的情况。比如是用户操作系统的一部分。
* 认证服务器只有在其他授权模式无法执行的情况下，才能考虑使用这种模式。

![Password Credentials Flow](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-ad727f7be653cf87_zpsuxr2dzgu.png)

### 第一步：用户传递认证信息
用户将用户名和密码交给应用程序。

### 第二步：应用程序请求授权令牌
* 应用程序得到用户认证信息后，向授权服务器请求授权令牌。
* 请求授权令牌终端`https://www.example.com/token`
* response_type=`password`，前面提到的两种分别是`code`和`token`。

```
https://www.example.com/token?grant_type=password&username=USERNAME&password=PASSWORD&client_id=CLIENT_ID
```

### 第三步：授权服务器返回授权令牌
如果用户的认证信息得到验证，授权服务器将向应用程序返回授权令牌。

## 客户端模式(Client Credentials)
* 客户端模式应用于应用程序想要以自己的名义与授权服务器以及资源服务器进行互动。
* 比如应用程序想要修改自身注册信息或者redirect URI。又或者想要获取资源服务器中不与具体用户相关的信息。比如一个应用程序想要获取新浪微博中含有#happy的微博。
* 严格意义上说，客户端模式并不是OAuth协议要解决的问题，因为并未涉及授权。

![Client Credentials Flow](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-baa71d6dae6b9059_zpsedziqvc2.png)

### 第一步：应用程序请求授权令牌
* 应用程序向授权服务器提供自身认证信息(client ID和client secret)，并请求授权令牌。
* 请求授权令牌终端`https://www.example.com/token`
* response_type=`client_credentials`，前面提到的两种分别是`code`、`token`和`password`。

```
https://www.example.com/token?grant_type=client_credentials&client_id=CLIENT_ID&client_secret=CLIENT_SECRET
```

### 第二步：授权服务器返回授权令牌
授权服务器验证认证信息，向应用程序返回授权令牌。

## 更新令牌(Refresh Token)
* 如果授权令牌过期，在进行API请求时会报错`Invalid Token Error`
* 如果在获取授权令牌时，同时获得了refresh token，那么我们可以向授权服务器申请更新令牌。
* grant_type = `refresh_token`
* scope：表示申请的授权范围，不可以超出上一次申请的范围，如果省略该参数，则表示与上一次一致。
```
https://www.example.com/v1/oauth/token?grant_type=refresh_token&client_id=CLIENT_ID&client_secret=CLIENT_SECRET&refresh_token=REFRESH_TOKEN
```

***

# 总结与参考资料
之后可能会写一个在Python中具体如何使用Twitter API或者Facebook API的实例，加深对OAuth的理解。
随着理解的深入，文章也会随时更新。
1. [An Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2)
2. [理解OAuth 2.0](http://www.ruanyifeng.com/blog/2014/05/oauth_2_0.html)
3. [帮你深入理解OAuth2.0协议](http://blog.csdn.net/seccloud/article/details/8192707)