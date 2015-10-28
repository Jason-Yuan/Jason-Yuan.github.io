title: "C#中的var关键字"
date: 2015-06-21 15:00:08
tags: ["C#", "Implicit Type", "Take a Bite"]
categories: "C#"
---

# 声明变量的时候，可以有下面两种方式

>int a = "hello world";  // 显示类型的(explicitly typed)
var a = "hello world";  // 隐式类型的(implicitly typed)

<!-- more -->
* 上面两种形式实际上是**等价的**
* 隐含类型局部变量(Local Variable Type Inference)，是C# 3.0新语言特性和改进
 * `var`只能作为局部变量
 * `var`必须在定义时初始化，`var a;`是错误的
 * `var`在初始化完成后，不能再更改类型
 * 在编译器会自动匹配`var`变量的实际类型，不同于dynamic，编译后是object类型

***

# 建议在能够明确类型的时候，要使用具体类型。那么为什么还要出现`var`呢

首先， 如果我定义了这样一个**类**
```
class Person 
{
    public Person()
    public Person(string lastname, string first name)
    {
        LastName = lastName;
        FirstName = firstName;
    }

    public string LastName { get; set; }
    public string FirstName { get; set; }
}
```
我可以通过下面两种方法创建**实例**
```
// 传统方式
Person person1 = new Person();
person1.FirstName = "Andrew";
person1.LastName = "Chen";
Console.WriteLine(person1.FirstName + " " + person1.LastName);

// 对象初始化器(Object initializer)
Person person2 = new Person {FirstName = "Andrew", LastName = "Chen"};
Console.WriteLine(person2.FirstName + " " + person2.LastName);
```
Note: 对象与集合初始化器(Object and Collection Initializers)，是C# 3.0新语言特性和改进

如果我们要创建一个**匿名类**呢？
```
// 匿名类，我们不知道presenter的类型
// 没有var关键字，数据是无法存贮的
var presenter = new { FirstName = "Andrew", Age = 43, Topic = "Introduction"}；
```
Note: 匿名类型(Anonymous Types)，是C# 3.0新语言特性和改进
 
再举个栗子，来自[MSDN](https://msdn.microsoft.com/en-us/library/bb383973.aspx)
```
// 栗子1: var是可选的，因为我们知道类型是string
string[] words = { "apple", "strawberry", "grape", "peach", "banana" };
var wordQuery = from word in words
                  where word[0] == 'g'
                  select word;
// 因为每个元素都是string，而非匿名类型，所以这里var是可选的 
foreach (string s in wordQuery)
{
    Console.WriteLine(s);
}


//栗子2: var是必选的，因为返回是匿名类型(anonymous type)
var custQuery = from cust in customers
                  where cust.City == "Phoenix" 
                  select new { cust.Name, cust.Phone };
// 因为每个元素都是匿名类型，这里var是必选的
foreach (var item in custQuery)
{
    Console.WriteLine("Name={0}, Phone={1}", item.Name, item.Phone);
}
```