title: "通过Github Pages和Jekyll搭建个人博客"
date: 2015-06-24 21:39:01
tags: ["Jekyll", "Github Pages"]
categories: "MISC"
---

***

# 什么是Jekyll ?

![Jekyll](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-f98becf54ff9fabc_zpstbnjl7ae.png)

* Jekyll 是目前非常流行的静态网站生成器(static website generator)。静态网站即只包含HTML, CSS 和 Javascript
```
Jekyll(raw_text_files, templates)
{
    return Beautiful_web_pages;
} 
```

<!-- more -->
* 不同于WordPress, Ruby on Rails, CakePHP, Flask等这些需要web server的复杂应用程序。Jekyll不依赖于数据库或CMS。所以最适合用来建立个人博客，个人portfolio等等
* Github Pages即靠Jekyll实现的

# 使用静态网站生成器的好处
* 轻量级网站，响应速度快
* 网站更安全，无数据库，无动态数据
* 无需通入过多精力维护

# 环境搭建
本文只说明在Mac上的实现过程
## 安装 Jekyll
* 首先打开terminal，输入
```
jason: ~ $ gem install jekyll
```
* 如果提示错误，一般是读写权限问题，可尝试`sudo`，然后根据提示输入密码
```
jason: ~ $ sudo gem install jekyll
```
* 输入`jekyll -v`，确认安装成功。若成功会提示版本信息，例如
```
jekyll 2.5.3
```

## 创建 Jekyll 项目
* 首先cd到你想创建项目的目录下，比如我想在桌面创建
```
jason: ~ $ cd Desktop
```
* 例如我想创建一个项目，叫`Jason_Blog`
```
jason: ~/Desktop $ jekyll new Jason_Blog
```
* 成功会收到提示信息
```
New jekyll site installed in /Users/boyuan/Desktop/Jason_Blog.
```
* Jason_Blog文件夹内容如下
![Jason_Blog](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-33f225c3ee0ef7c7_zpsun9wzhfy.png)

 * _config.yml 是配置文件，最为重要，包含了所有配置信息
 * _includes 文件夹包含了将被反复利用的文件，比如footer，header
 * _layouts 文件夹包含了主页面的排版布局
 * _posts 文件夹将包含所有的日志文件，Markdown格式

* Jekyll为我们提供了一篇默认博客，我们可以通过运行下面的命令，然后再本地查看
```
boyuan: ~/Desktop $ cd Jason_Blog_2/
boyuan: ~/Desktop/Jason_Blog $ jekyll serve
```
**注意**：此时Jason_Blog文件夹中会增加`_site`文件夹，包含了生成网站的所有结构。同时，原来文件夹中开头**没有下划线**的文件夹会被复制到`_site文件夹`中，比如`CSS文件夹`

* 停止本地运行，敲击`CTR+C`

## 更改配置文件`_config.yml`
详细内容参考官网 - [Jekyll configuration](http://jekyllrb.com/docs/configuration/)
想了解更多YAML文件 - [YAML](http://yaml.org/)

* 打开`_config.yml`文件，可以看到一些基础设置

* 更改URL显示
原始URL: `http://127.0.0.1:4000/jekyll/update/2015/06/22/welcome-to-jekyll.html`
```
# Build settings
markdown: kramdown
permalink: pretty
```
  URL变更为：`http://127.0.0.1:4000/jekyll/update/2015/06/22/welcome-to-jekyll/`
```
# Build settings
markdown: kramdown
permalink: /:title
```
  URL变更为：`http://127.0.0.1:4000/welcome-to-jekyll/`

## 深入了解Jekyll模板
Jekyll实际使用的是模板语言Liquid来操作模板和输出文件的。Liquid现在是[Github上的开源项目](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers)，有兴趣可以多了解一下，很简单
* 在每一个`_post`文件夹的post文件中，在`---`中间的内容叫做**YAML Front Matter**，Liquid可以将这里的数据传送到模板中相应的位置。

```
---
layout: post                       // 表示选择post模板
title:  "Welcome to Jekyll!"       // 博客的标题
date:   2015-06-23 01:54:33        // 博客日期
author: Jason                      // 作者
categories: jekyll update          // 添加类别，空格分开
permalink: "Welcome to Jekyll"     // 确定显示的URL
---
```
注：反复使用的变量，比如`layout`, `author`等可以移到`_config.yml`中

* 从下面这张图我们可以看到，Jekyll中模板相关的文件在红框中，我们具体以`default.html`简述一下

![default.html](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-4bfde4bb8f5a55b0_zps0eextqau.png)

* 首先是Liquid的两种格式
```
// 格式一   output markup
{% include head.html %}
// 格式二   tag markup
{{ content }}
```
 * 第一个的意思就是把`head.html`文件添加到相应位置，同理`header.html`和`footer.html`。
 * 第二个的意思就是具体某个post的内容会添加到相应位置，当然在相应的post中我们要指定layout是什么，比如下面就是指定layout为default。这个post的内容会添加到content的位置。
**其实上述内容与ASP.NET 中的Razor布局很像** - [ASP.NET Razor 简介](http://www.jianshu.com/p/f28e04c187be)

* 下面这张图我们可以看到我们指定了这个post使用default，同时注意到`page.title`，title数据就来自于上面第一点说的**YAML Front Matter**
![post.html](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-43d2f0f0e50c8ad3_zpswgqgczss.png)

* 说完了post，我们再说说怎么增加新页面，比如About, Contact......
新页面直接添加在项目根目录下，前面**没有下划线**，比如新建`contact.md`

```
// 下面是about.md 中的 YAML Front Matter
---
layout: page                       // 表示选择post模板
title: About                       // 标题为About
permalink: /about/                 //about后面添加一个 "/" 可是_site中生成同名文件夹
---
```

* 在项目根目录下可以创建文件夹`_drafts`，然后再其中添加草稿，预览可输入
```
boyuan: ~/Desktop/Jason_Blog $ jekyll serve --drafts
```

# 主题
有了上面的了解，网络上有一大波主题在等着你，直接下个主题，轻松简单。

最后，发布到Github吧， Free Free Free
请戳 => [如何使用Github Pages免费搭建网站](http://www.jianshu.com/p/6cabb41495c8)