title: "C#中的System-Threading"
date: 2015-06-28 21:48:16
tags: ["C#", "ASP.NET", "Operating System", "Thread & Process", "Take a Bite"]
categories: "ASP.NET"
---

![MultiThreading ? ? ?](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-7fd36207e95438db_zpsomzwjb0k.png)

在说C#中的`System.Threading`之前，先来简单说说`Process`, `Thread`, `CPU`, 和`OS`，算是一个铺垫，说的比较浅，都算是OS课的范畴。话说这是我转专业上的第一门课，当时几乎没有任何编程基础和计算机基础知识，可谓相当痛苦。学习效果也比较渣，现在算是回头慢慢补救。

<!-- more -->
# Process 和 Thread 的比较
* **Process**(进程)
Process是正在运行的应用程序的实例(executing instance)。比如我们双击Microsoft Word图标，就启动了一个Process。所以说，一台计算机上可以运行多个Process

* **Thread**(线程)
 * 线程，有时被称为轻量级进程(Lightweight Process，LWP)，是程序执行流的最小单元。
 * 线程是进程中的一个实体，是被系统独立调度和分派的基本单位。
 * 一个进程可以有很多线程，每条线程**并行**执行不同的任务，称为多线程(multi-threading)。
> 比如， 你在word中单击**打印**就开启了一个新的线程(backgroud thread)，但同时你可以返回主页面继续进行**文字编辑**(这是另外一个thread accept user input)。

推荐当年看过的一篇[@阮一峰](http://www.ruanyifeng.com/blog/)大神的blog，浅显易懂 - [进程与线程的一个简单解释](http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html)

# 单核CPU的运行方式
* 运行一个thread
> * thread首先加载到RAM中
 * 通过bus转移到CPU的pipeline中(一个queue)
 * 指令集(instruction set)中的指令依此被执行 
* 运行多个threads
> * 比如，两个threads加载到RAM中
 * 此时，在CPU和RAM中多了一个**scheduler**，负责把thread转移到CPU的pipeline中，scheduler会在两个threads之间相互转换，分别给每个thread一定的time slice
 * 对于我们而言，当time slice足够小，看起来两个threads就像是同时多线程(Simultaneous multithreading)

# 发展
伴随着`Multi-task OS`和`Multi-thread Application`的出现，CPU设计也开始适应这种需求，出现了：
* Hyper Threading
* Multi-Core CPU
* Multi CPU

# C#中的 System.Threading 命名空间
* 第一个例子，不使用`System.Threading`
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
// 第一个控制台程序
namespace LearnThread
{
    class Program
    {
        static void subTask()
        {
            for (int i = 0; i < 5; i++)
            {
                Console.WriteLine("subTask is running");
            }
        }
        static void Main(string[] args)
        {
            subTask();
            for (int i = 0; i < 5; i++ )
            {
                Console.WriteLine("main is running");
            }
            Console.ReadLine();
        }
    }
}
```
`subTask`运行结束之后才会运行`main`中的for循环，运行结果为
```
subTask is running
subTask is running
subTask is running
subTask is running
subTask is running
main is running
main is running
main is running
main is running
main is running
```

* 第二个例子，使用`System.Threading`，建立一个新thread
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading;
using System.Threading.Tasks;
// 第二个控制台程序
namespace LearnThread
{
    class Program
    {
        static void subTask()
        {
            for (int i = 0; i < 5; i++)
            {
                Console.WriteLine("subTask is running");
            }
        }
        static void Main(string[] args)
        {
            // create one thread
            Thread t = new Thread(new ThreadStart(subTask)); 
            // invoke this thread
            t.Start();
            for (int i = 0; i < 5; i++ )
            {
                Console.WriteLine("main is running");
            }
            Console.ReadLine();
        }
    }
}
```
在运行`subTask`这个thread的时候，并不是运行到结束再执行`main`中的for循环，而是交替执行，**类似Simultaneous multithreading**，运行结果为(结果随机)
```
subTask is running
main is running
main is running
subTask is running
subTask is running
main is running
subTask is running
subTask is running
main is running
main is running
```
可见，`System.Threading`命名空间中的`Thread`类可以帮助我们实现程序中的并行。

* `System.Threading.Thread`类中常见的方法
**Thread.Start():** 启动线程的执行
**Thread.Suspend():** 挂起线程，或者如果线程已挂起，则不起作用；
**Thread.Resume():** 继续已挂起的线程
**Thread.Interrupt():** 中止处于 Wait或者Sleep或者Join 线程状态的线程
**Thread.Join():** 阻塞调用线程，直到某个线程终止时为止
**Thread.Sleep():** 将当前线程阻塞指定的毫秒数
**Thread.Abort():** 终止此线程的过程。如果线程已经在终止，则不能通过Thread.Start()来启动线程

# Thread的两种类型
* **Foreground thread**
如果主程序结束了，foreground thread依然存在或者继续运行。正如下面的例子：
```
namespace LearnThread
{
    class Program
    {
        static void Main(string[] args)
        {
            // A thread will be created to run
            // function1 parallely
            Thread obj1 = new Thread(Function1);
            obj1.IsBackground = true;
            obj1.Start();

            // the control will come here and exit the main function
            Console.WriteLine("The main application has exited");
        }

        static void Function1()
        {
            Console.WriteLine("Function 1 entered");
            // wait here until the user put any input
            Console.ReadLine();
            Console.WriteLine("Function 1 is exited");
        }
    }
}
```
程序的运行结果为：
```
The main application has exited
Function 1 entered
hello
Function 1 exited
Press any key to continue . . . 
```
可见，虽然主程序结束了，foreground thread依然会等待用户输入，在用户输入`hello`之后，执行最后一句，退出

* **Background thread**
如果主程序结束了，background thread也随即结束。正如下面例子：
```
namespace LearnThread
{
    class Program
    {
        static void Main(string[] args)
        {
            // A thread will be created to run
            // function1 parallely
            Thread obj1 = new Thread(Function1);
            obj1.IsBackground = true;
            obj1.Start();

            // the control will come here and exit the main function
            Console.WriteLine("The main application has exited");
        }

        static void Function1()
        {
            Console.WriteLine("Function 1 entered");
            // wait here until the user put any input
            Console.ReadLine();
            Console.WriteLine("Function 1 is exited");
        }
    }
}
```
改程序的输出结果为：
```
The main application has exited
Function 1 entered
Press any key to continue . . . 
```
可见，主程序退出后，background thread也结束了，没有再等待用户输入

## 关于 Thread Safe Object
* 在两个thread中运行下面的程序，如果我们不考虑thread safe的问题，程序会报错，因为会出现**零**作为除数的情况
```
Random o = new Random();
for (long i = 0; i < 10000; i++)
{
    Num1 = o.Next(1, 2);
    Num2 = o.Next(1, 2);
    int result = Num1/Num2;
    Num1 = 0;
    Num2 = 0;
}
```
* 在.NET中提供的解决方案：
 * Lock
 * Mutex
 * Semaphore

* 比如，使用lock修改后的程序
```
Random o = new Random();
for (long i = 0; i < 10000; i++)
{
    lock (this)
    {
        Num1 = o.Next(1, 2);
        Num2 = o.Next(1, 2);
        int result = Num1/Num2;
        Num1 = 0;
        Num2 = 0;
    }
}
```

***

# 更多资源
1. [C# Multithreading Example](http://resources.infosecinstitute.com/multithreading/)
2. Thread Class (MSDN)