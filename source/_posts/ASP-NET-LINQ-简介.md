title: "ASP.NET LINQ 简介"
date: 2015-06-21 21:33:57
tags: ["ASP.NET", "LINQ"]
categories: "ASP.NET"
---

***

![LINQ](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-a93a53f1d9cfcd13_zpsxd0ffrfw.jpg)

# 什么是LINQ ?
* **LINQ，集成查询语言(Language Integrated Query)**
 * **Language Integrated**：说明LINQ变成了编程语言的一部分。
 * **Query**：说明了LINQ是用来查询数据的。
* **LINQ主要包含以下三部分:**
 * **LINQ** - LINQ to Objects 主要负责对象的查询 
 * **XLINQ** - LINQ to XML 主要负责XML的查询
 * **DLINQ** - LINQ to ADO.NET 主要负责数据库查询

<!-- more -->
![LINQ Architecture](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-c59cf3c0f3b266b4_zpsxsabmann.gif)

* **LINQ主要解决的问题：**
 * 编程语言的数据类型与数据库类型不一致。 
 * SQL编码体验落后，同以往写sql query字符串相比，我们不需要等到运行时才有可能发现sql query字符串的错误。
 * 对象没有查询语言。

***

# 扩展方法(Extension Method)
* 扩展方法是C# 3.0新语言特性和改进
* 扩展方法就是向现有类型“添加”方法，而无需创建新的派生类型、重新编译或以其他方式修改原始类型
* 扩展方法是一种特殊的静态方法，但可以像扩展类型上的实例方法一样进行调用。
* **最常见的扩展方法是LINQ标准查询运算符**

## 扩展方法的特性
* 扩展方法是一种静态方法
* 扩展方法必须在静态类中定义
* 一定要使用`this`关键词
* 扩展方法优先级低于同名的类方法，如果存在同名，扩展方法不会被调用
* 扩展方法必须出现在相同命名空间，否则调用前使用`using`

## 扩展方法实例
-- 例子来自MSDN
* 首先，定义一个扩展方法
```
namespace ExtensionMethods
{
    public static class MyExtensions
    {
        public static int WordCount(this String str)
        {
            return str.Split(new char[] { ' ', '.', '?' }, 
                             StringSplitOptions.RemoveEmptyEntries).Length;
        }
    }   
}
```

* 调用刚刚写好的扩展方法
```
using ExtensionMethods; 
string s = "Hello Extension Methods";
int i = s.WordCount();
/*
output: 3
*/
```

## `System.Linq`命名空间中的内建扩展方法
 *  Where
 * Select
 * SelectMany
 * OrderBy
 * OrderByDescending
 * ThenBy
 * ThenByDescending
 * GroupBy
 * Join
 * GroupJoin

***

# Lambda 表达式
* Lambda表达式和Lambda表达式树 (Lambda Expression and Lambda Expression Trees)，是C# 3.0新语言特性和改进
* Lambda表达式是一种匿名函数(anonymous function)，比匿名函数具有更加简介的表示形式
* Lambda Operator `=>`
* Lambda表达式的语法形式如下
 `(input parameters) => expression`
* 简单举例
```
n => n % 2 == 0
```
 * n 是输入参数 
 * n % 2 == 0 是表达式
这个匿名函数可以解释为，若输入的n为偶数，则返回true


* ```
List<int> numbers = new List<int>{1, 3, 5, 6, 8};
List<int> evenNumbers = numbers.where(n => n % 2 == 0).ToList();
/*
evenNumbers = {6, 8}
*/
```
***

# LINQ查询的两种语法
* **Query syntax**(查询语句语法)
更接近于SQL的语法
* **Method syntax**(查询方法语法)
主要利用 System.Linq.Enumerable 类中定义的扩展方法和Lambda 表达式方式进行查询
* LINQ可以使用于List<T>, Array, Dictionary<TKey, TValue>, string......

如果没有LINQ，将使用的方法如下
```
public static void OldSchoolSelectOne()
{
    List<string> names = new List<string>
                                {
                                     "Andy", "Bill", "Dani", "Dane"
                                };
    string result = string.Empty;
    foreach (string name in names)
    {
        if (name == "Andy")
        {
            result = name;
        }
    }
    Console.WriteLine("We found " + result);
}
```


使用LINQ，两种方法如下
```
// Method syntax
public static void Single()
{
    List<string> names = new List<string>
                                {
                                     "Andy", "Bill", "Dani", "Dane"
                                };
    string name = names.Single(n => n == "Andy");
    Console.WriteLine(name);
}

// Query syntax
public static void Single()
{
    List<string> names = new List<string>
                                {
                                     "Andy", "Bill", "Dani", "Dane"
                                };
    string name = from name in names
                  where  name = "Andy"
                  select name;
    Console.WriteLine(name);
}
```

***

# 参考资料和扩展阅读
1. [LINQ Introduction Part 1 Of 3](http://www.codeproject.com/Articles/18116/LINQ-Introduction-Part-Of#WhatItsNot)
2. [Introducing LINQ—Language Integrated Query](http://www.codeproject.com/Articles/199060/Introducing-LINQ-Language-Integrated-Query)
3. [LINQ Tutorial for Beginners](http://www.codeproject.com/Tips/590978/LINQ-Tutorial-for-Beginners)
4. [Understanding LINQ (C#)](http://www.codeproject.com/Articles/19154/Understanding-LINQ-C)
5. [What is Linq and what does it do?](http://stackoverflow.com/questions/471502/what-is-linq-and-what-does-it-do)
6. [Linq语法详细](http://blog.csdn.net/ycwol/article/details/42102939)
7. [LINQ tutorial](https://www.youtube.com/watch?v=z3PowDJKOSA&list=PL6n9fhu94yhWi8K02Eqxp3Xyh_OmQ0Rp6) 
8. [101 LINQ Samples](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b)