title: "ASP.NET Razor 简介"
date: 2015-06-16 21:28:04
tags: ["Razor", "ASP.NET", "C#"]
categories: "ASP.NET"
---

***

 ![razor](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-c0c761294e344088_zpsw9p8g9ez.png)

# 什么是Razor ?
* Razor 不是一种编程语言，而是一种标记语法，可以将基于服务器的代码（Visual Basic 和 C#）嵌入到网页中。
* Razor 是基于 ASP.NET 的，是为创建 Web 应用程序而设计的。
* Razor支持代码混写。
* 带 Razor 语法的 ASP.NET 网页有特殊的文件扩展名cshtml(Razor C#)或者vbhtml(Razor VB)。

<!-- more -->
***

# Razor C#基本语法规则
## ① 使用`@`将代码块添加到页面中
* 内联表达式(Inline expressions)
* 单语句块(Single statement blocks)
* 多语句块(Multi-statement block)

```
<!-- Inline expressions -->
<p>You are using @Request.Broswer.Broswer!</p>

<!-- Single statement blocks  -->
@{ ViewBag.title = "Home Page"; }
@{ var myMessage = "Hello World"; }

<!-- Multi-statement block -->
@{
    var name = "Jason";
    var greeting = "Nice to meet you, ";
    var greetingMessage = greeting + name;
}
<p>The greeting is: @greetingMessage</p>
```

## ② 代码块括在大括号中，代码语句用分号结束

## ③ 使用 `var` 关键字，声明变量存储值
```
<!-- Storing a string -->
@{ var welcomeMessage = "Welcome, new members!"; }
<p>@welcomeMessage</p>

<!-- Storing a date -->
@{ var year = DateTime.Now.Year; }
```

## ④ 字符串要用引号括起来
```
@{ var myString = "This is just an example"; }
```
## ⑤ C#代码是区分大小写

## ⑥ 空格和换行符不影响语句
* 可以通过增加空格或者换行符提高代码的可读性。
* 但是对于字符串，不可以
```
@{ var test = "This is a long
    string"; }  // Does not work!
```

## ⑦ 内联的helper方法
```
@helper formatAmount(decimal amount)
{
    var color = "green";
    if (amount < 0)
    {
        color = "red";
    }
    <span style="color:@color">@String.Format("{0:c}", amount)</span>
}
```
然后可以在其他地方使用helper方法，比如：
```
@{var amounts = new List<decimal> {100, 25.50m, -40, 276.99m}
}

<ul>
    @foreach(decimal amount in amounts)
    {
        <li>@formatAmount(amount)</li>
    }
</ul>
```

## ⑧ `@{}`中的内容都会被视为C#代码
* 如果想要添加纯文本，两种方法
```
@ {
      //方法1
      <text>djskfadsfhadsjfk</text>
      //方法2
      @: fhdshfjskhfksfs
}
```

* 输出`@`符号
```
@ { <p>Have a good weekend @@LA</p> }
//output: Have a good weekend @LA
```

## ⑨ 注释
* 使用`@**@`
```
@*  A one-line code comment. *@
@*
    This is a multiline code comment.
    It can continue for any number of lines.
*@	
```

* 在`@{}`中使用C#的注释格式
```
@{
    // This is a comment.
    var myVar = 17;
    /* This is a multi-line comment
    that uses C# commenting syntax. */
}
```

***

# 逻辑条件与循环 
* If-else, else if 语句
```
@ { var price = 25; }
<body>
@if (price >= 30)
{
    <p>The price is high.</p>
}
else if (price > 20 && price < 30) 
{
    <p>The price is OK.</p>
}
else
{
    <p>The price is low.</p>
} 
</body>
```

* Switch 语句
```
@ { var day = "Monday"; }
<body>
@switch(day)
{
case "Monday":
    message="This is the first weekday.";
    break;
case "Thursday":
    message="Only one day before weekend.";
    break;
case "Friday":
    message="Tomorrow is weekend!";
    break;
default:
    message="Today is " + day;
    break;
}
```

* For 循环
```
<!-- 方式1 -->
@for (int i = 0; i < 10; i++)
{
    @:@i
}
<!-- 方式2 -->
@{
    for (int i = 0; i < 10; i++)
    {
        //do something
    }
}
```

* While 循环
```
<body>
@{
    var i = 0;
    while (i < 5)
    {
        i += 1;
        <p>Output is: @i</p>
   }
}
</body>
```

* Foreach 循环
```
//定义一个数组
@{var amounts = new List<decimal> {100, 25.50m, -40, 276.99m}
}
//使用foreach遍历数组
<ul>
    @foreach(decimal amount in amounts)
    {
        <li>@amount</li>
    }
</ul>
```


***

# ASP.NET MVC 中Razor布局
![Views folder](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-381d116b54e076fb_zps547wl2c0.png)
* 在_ViewStart.cshtml中， 可以定义所有view的默认layout模板
```
@{
    Layout = "~/Views/Shared/_Layout.cshtml";
}
```
* _Layout.cshtml即模板页，起到页面整体框架重用的目的
```
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title>@ViewBag.Title</title> 
    @Styles.Render("~/Content/css")
    @Scripts.Render("~/bundles/modernizr")
</head>
<body>
    @Html.Partial("_header")
    <div class="navbar navbar-inverse navbar-fixed-top">
        <div class="container">
            <div class="navbar-collapse collapse">
                <ul class="nav navbar-nav">
                    <li>@Html.ActionLink("Home", "Index", "Home")</li>
                    <li>@Html.ActionLink("About", "About", "Home")</li>
                    <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
                </ul>
                @Html.Partial("_LoginPartial")
            </div>
        </div>
    </div>
    <div class="container body-content">
        <div class="row">
            <div class="col-md-12">
                <img src="~/Content/Images/logo.png" class="img-responsive item-center"/>
            </div>
        </div>
        @RenderBody()
    </div>
    @Scripts.Render("~/bundles/jquery")
    @RenderSection("scripts", required: false)
    @Html.Partial("_footer")
</body>
</html>
```
* @Html.Partial()
HtmlHelper.Partial()，可以将页头、页脚、登陆等局部视图加载进来
* @RenderBody()
将对应View页面的主内容替换到此
* @RenderSection()
将对应View页面的相应的section部分替换到此

***

# 相关资料
1. Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)
2. [C# Razor Syntax Quick Reference](http://haacked.com/archive/2011/01/06/razor-syntax-quick-reference.aspx/)