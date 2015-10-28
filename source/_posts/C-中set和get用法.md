title: "C#中set和get用法"
date: 2015-05-20 20:57:43
tags: ["C#", "get & set", "Take a Bite"]
categories: "C#"
---

***

# c#中的域与属性
首先，先来谈一谈c#中的两个概念，域与属性。

## 域(field)
* 类(class)或结构(structure)中的成员变量(Member Variable)或方法称为域。
* 域即是字段，分为实例域和静态域，实例域属于具体的对象，为特定的对象所专有。静态域属于类，为所有对象所共用。
* 域的存取限制集中体现了面向对象编程的封装原则。C#中的存取限制修饰符有5种(Public, Private, Protected, Internal, Protected internal)。
* **域表示存储位置，域的类型可以是c#中的任何数据类型。**

<!-- more -->

## 属性(property)
* 属性是类(class)、结构(structure)和接口(interface)的命名成员。
* 属性是域的扩展，通过使用访问器(accessors)让私有域的值可被读写或操作。
* 属性也有5种存取修饰符，但属性的存取修饰往往为public。
* 属性的背后是两个函数：赋值函数(get)和取值函数(set)。
* **属性不表示存储位置，这是属性和域的根本性的区别。**

***
 
# 为什么会出现set和get
* 在面向对象编程(OOP)中，用户只需要知道对象(object)能做什么，而不需要知道其如何完成的，或者说**不允许**访问其内部，这体现了面向对象的3个基本原则中的**封装**。
>面向对象的基本原则：
封装(Encapsulation)
多态(Polymorphism)
继承(Inheritance)

* 一般来说，域是私有的。对所有有必要在类外可见的域，C#推荐采用属性来表达。属性提供了只读(get)，只写(set)，读写(get和set)三种接口操作。对域的这三种操作，必须在同一个属性名下声明，而不可以将它们分离。

```
public class MyClass
{
    // this is a field.  It is private to your class and stores the actual data.
    private string _myField;

    // this is a property.  When you access it uses the underlying field, but only exposes
    // the contract that will not be affected by the underlying field
    public string MyField
    {
        get
        {
            return _myField;
        }
        set
        {
            _myField = value;
        }
    }
}
```
* 当引用属性时，除非该属性为赋值目标，否则将调用 get 访问器读取该属性的值。(这里解释程序是如何判别应该调用get还是set的)

***

# 使用set和get的好处
* 为赋值与取值增加控制
* 保证属性的安全性，不能直接修改域
* 便于维护，可实现在set访问器中一处更改

```
class Bank
    {
        private int money;//私有字段

        public int Money  //属性
        {
            get { return money;  }
            //增加限制，存钱不能为负数
            set
            {
                if (value >= 0) {
                    money = value;
                }
                else {
                    money = 0; 
                }
            }
        }
    }
```

**自动实现的属性**(Auto-Implemented Properties), 以下两种代码等效
```
public class Student
{
    public string Name { get; set; }
}
```

```
public class Student
{
    private string name; // This is the so-called "backing field"
    public string Name // This is your property
    {
        get {return name;}
        set {name = value;}
    }
}
```

***

# 总结与参考资料
本文是笔者学习c#过程中的读书笔记，鞭策自己深入学习，也方便以后复习参考。内容会持续更新。更多内容可参见下面的参考文献。错误之处，还望指出，共同进步。

1. [C#域与属性](http://blog.sina.com.cn/s/blog_5c4a1a800100gudi.html)
2. [C# 属性（Property）](http://www.w3cschool.cc/csharp/csharp-property.html)
3. [What is the { get; set; } syntax in C#?](http://stackoverflow.com/questions/5096926/what-is-the-get-set-syntax-in-c)
4. [浅析C# get set的简单用法](http://developer.51cto.com/art/200909/151051.htm)

