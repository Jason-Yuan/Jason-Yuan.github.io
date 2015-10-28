title: "Python中的*arg和**kwarg"
date: 2015-07-28 21:58:13
tags: ["Python", "*arg", "**kwarg"]
categories: "Python"
---

# 一个简单的函数
首先我们可以定一个简单的函数, 函数内部只考虑`required_arg`这一个形参(**位置参数**)
```
def exmaple(required_arg):
    print required_arg

exmaple("Hello, World!")

>> Hello, World!
```

<!-- more -->
那么，如果我们调用函数式传入了**不止一个位置参数**会出现什么情况？当然是会报错！

```
exmaple("Hello, World!", "another string")

>> TypeError: exmaple() takes exactly 1 argument (2 given)
```

# 定义函数时，使用\*arg和\**kwarg
\*arg和\**kwarg 可以帮助我们处理上面这种情况，允许我们在调用函数的时候传入多个实参
```
def exmaple2(required_arg, *arg, **kwarg):
    if arg:
        print "arg: ", arg

    if kwarg:
        print "kwarg: ", kwarg

exmaple2("Hi", 1, 2, 3, keyword1 = "bar", keyword2 = "foo")

>> arg:  (1, 2, 3)
>> kwarg:  {'keyword2': 'foo', 'keyword1': 'bar'}
```
从上面的例子可以看到，当我传入了更多实参的时候
* \*arg会把多出来的位置参数转化为`tuple`
* \**kwarg会把关键字参数转化为`dict`

再举个例子，一个不设定参数个数的加法函数
```
def sum(*arg):
    res = 0
    for e in arg:
        res += e
    return res

print sum(1, 2, 3, 4)
print sum(1, 1)
>> 10
>> 2
```
当然，如果想控制关键字参数，可以单独使用一个*，作为特殊分隔符号。限于`Python 3`，下面例子中限定了只能有两个关键字参数，而且参数名为`keyword1`和`keyword2`
```
def person(required_arg, *, keyword1, keyword2):
    print(required_arg, keyword1, keyword2)

person("Hi", keyword1="bar", keyword2="foo")
>> Hi bar foo
```
如果不传入参数名`keyword1`和`keyword2`会报错，因为都会看做**位置参数**！
```
person("Hi", "bar", "foo")

>> TypeError: person() takes 1 positional argument but 3 were given
```

# 调用函数时使用\*arg和\**kwarg
直接上例子，跟上面的情况十分类似。反向思维。
```
def sum(a, b, c):
    return a + b + c

a = [1, 2, 3]

# the * unpack list a 
print sum(*a)
>> 6
```

```
def sum(a, b, c):
    return a + b + c

a = {'a': 1, 'b': 2, 'c': 3}

# the ** unpack dict a
print sum(**a)
>> 6
```
