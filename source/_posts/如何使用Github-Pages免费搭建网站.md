title: "如何使用Github Pages免费搭建网站"
date: 2015-05-21 16:37:36
tags: ["Github Pages", "Free Website"]
categories: "MISC"
---

![github cat](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-7f7a6a5949061599_zpscrjr1xfp.png)

<!-- more -->
***
# 什么是Github ？

![Github 官方主页](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-014db44d0bb6f0f7_zpsexwsc3ui.png)

简单说，Github是一个基于git的社会化代码分享社区。
* 你可以在Github上创建**免费的**远程仓库(remote repository)，分享你的代码，当然也可以关注其他人的代码
* 你也可以建立公司账户，创建私有的远程仓库，与开发团队共同协作开发
* 想要使用Github Pages，你首先要创建一个Github账户

***

# 谁在使用Github免费托管网站 ？
* [Bootstrap](http://getbootstrap.com)
* [NODESCHOOL](http://nodeschool.io)
* [WebComponents](http://webcomponents.org)
......

***

# Github pages的两种类型
##  Project Pages(Repository Pages)

![URL for Project Pages.png](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-e071f96267a1e69d_zpsewqfh7fn.png)

* 在Github上我们可以给不同的project分别创建相应的repository，对于某一个repository，你可以在其中创建一个小网站，向人们展示你的项目，提供项目的相关信息等等。这就是所谓的project pages。例如上面说的[bootstrap.com](http://getbootstrap.com)
* 在一个repo的**gh-pages**分支中的所有文件将出现在github.io上。
* Project Pages How-To
 * 创建一个**gh-pages**分支
 * **编辑**相应的**html/css/js文件**，用于展示在github.io上
 * **push gh-pages**分支到Github上面
```
//下面是一些会用到的git command
git checkout -b gh-pages          //create a gh-pages branch 
git branch                        //check all branches and which branch you are currently working on
git push origin gh-pages          //push gh-pages branch to github
git checkout --orphan go-pages    //you can create a new empty branch
git push origin :gh-pages         //delete a remote branch
```
* 最简单地方法是从Github上直接自动生成页面，还可以选择模板。[移步这里](https://help.github.com/articles/creating-pages-with-the-automatic-generator/)

## User Pages

![URL for User Pages](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-9f56ef2d90636bf1_zps64yharth.png)

* 每一个Github账户只能有一个User Pages，主要用来展示一个账户中最最重要的项目。
* 命名为**username.github.io**的repo中的内容将会出现在username.github.io上。
* User Pages How-To
 * 创建一个新的repo，名字必须是username.github.io
![创建新的repo](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-f4b4ec14803d7031_zpslghja8zm.png)

 * 创建你的网站，包括：HTML文件，CSS文件...
```
my_website_folder
    |- index.html
    |- main.css
    |- bootstrap.js
    |...
```
 * 创建本地git repo
```
~ $ cd my_website_folder   //进入你的网站所在的文件夹
~ $ git init
~ $ git add .
~ $ git commit -m "Initial commit"
```
 * 添加remote repo到本地，push到Github
```
~ $ git remote add origin https://github.com/Jason-Yuan/Jason-Yuan.github.io.git
~ $ git remote -v           //可以查看是否添加成功，及其详细信息
~ $ git push origin master
```
 * 设置个性域名
   * 创建一个CNAME文件，包含你的个性域名
```
example.com
```
   * 把你个性域名的A record指向Github DNS 
 
* 如果想要搭建博客，下面列了一些非常流行的framework，可自动生成静态页面：
 * Octopress (基于Ruby)
 * Jekyll (基于Ruby) - [通过Github Pages和Jekyll搭建个人博客](http://www.jianshu.com/p/3f355c7872d5)
 * Hexo (基于NodeJS)
 * Pelican (基于Python)

***

# Github Pages的限制(Limitations)
* Github Pages只是静态网站(HTML, CSS, JavaScript)
* 没有服务端，所以不支持服务端的语言(没有ruby, php, python)


#参考资料
1. [Github官网]()
2. [Using GitHub Pages To Host Your Website](http://blog.teamtreehouse.com/using-github-pages-to-host-your-website)
3. [Repositories to build blogs on GitHub pages](http://www.jianshu.com/p/48d58d16920d)