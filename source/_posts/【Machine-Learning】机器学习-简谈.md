title: "【Machine Learning】机器学习-简谈"
date: 2015-05-02 15:06:48
tags: ["Machine Learning", "AI"]
categories: "AI"
---

***

# What is machine learning ?
* Herbert Simon 定义学习为:
> 能够让系统在执行统一任务或相同规模任务时比前一次执行的更好的任何改变 - (Simon, 1983).

* Learning can be also seen as a search problem.

* machine learning 之于 AI
![AI 领域](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-9e85e8cbda2f19f6_zpsbe27uyy0.png)

<!-- more -->
* Machine learning [wiki 定义](http://zh.wikipedia.org/wiki/机器学习)

* [一个通俗易懂的例子](http://www.zhizhihu.com/html/y2012/4124.html) Original from [Quora](http://www.quora.com/How-do-you-explain-Machine-Learning-and-Data-Mining-to-non-Computer-Science-people)

***

# Why machine learning ?
* 重要的AI问题之一
* “Knowledge engineering bottleneck”, 这个瓶颈是指传统知识获取技术构建expert system代价巨大。所以需要机器自身具备学习能力。
* 机器学习在现实生活中有诸多应用(Applications)：
 * 数据挖掘 data mining
 * 模式识别 pattern recognition
 * 语音识别 speech recognition
 * 天气预报 forecasting
 * 无人机 unmanned vehicles
 * 大数据分析 big data analysis
 * 金融建模 Financial modeling
 * 游戏 Games
 * 生物医药 Medicine and biology
 * 等等。。。等等。。。

***

# Four approaches to ML problem

## 基于符号的方法 (symbol-based)

### 几种分类方式

```
* 基于相似性的学习 (Similarity-based learning)
* 基于解释的学习 (Explanation-based learning)
```
**基于相似性的学习**依赖大量的例子(data-driven)，从中泛化出某个概念的必要属性。
**基于解释的学习**可以通过类比或运用领域中的先验知识(prior knowledge-driven)指导泛化。人类学习就不需要依赖大量的例子。比如，如果我告诉你这是苹果，你可以从你以前的知识*(水果是什么？)*中对苹果加深理解。
```
* 监督学习 (Supervised learning)
* 无监督学习 (Unsupervised learning)
* 强化学习 (Reinforcement learning)
```
![Based on learning method](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-94b9e2a1fca28a4f_zps0937nq34.png)

**监督学习**是指对**有**label的的train dataset进行学习
* **分类**是指预测需要输出的是离散型变量，比如：iris的类别，人的血型
* **回归**是指预测需要输出的是连续型变量，比如：房价，温度

**无监督学习**是指对**没有**label的的train dataset进行学习
* **聚类**的目标是从dataset中发现具有相似性的groups, 常见算法如K-means 和 Mean Shift
* **密度估计**的目标是判断dataset中数据的分布


**强化学习**主体处在环境之中，并从环境中得到反馈。主体必须创建一种策略来解释所有的反馈，从中学习。

[注：如果既可以使用监督学习又可以使用无监督学习，一般情况下选择监督学习。]()
```
* 预测 (Prediction)
* 数据挖掘 (Data mining)
```
![Based on porpose](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-efdcee5624b7a92c_zpsg6vt6par.png)

* **预测**重视算法的预测能力，比如天气预报，我们更关心的是一个算法预测温度的准确率。
* **数据挖掘**则更重视算法的可解释性，比如Market Basket Analysis中的一个经典案例，当{尿布+奶粉} -->{啤酒+ 刮胡刀} 这种奇怪的组合被Walmart发现时，数据挖掘可以帮助人们理解其背后的原因：年轻的妻子在家坐月子，年轻的父亲们出来购物。因此，我们可以把更多年轻父亲可能会买的东西也摆在一起。

### 基于符号学习的框架
![学习过程的总体模型](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-6cd1c95839551c0e_zpsci6tdapi.png)
我们可以从下面几个方面分析一下：
* **学习任务的数据和目标** (Data and goals of the learning task)
 * 数据类型
   * 正例或反例(主要是针对上面说的**基于相似的学习**)
   * 单一实例和特定领域的知识库(主要针对上面说的**基于解释的学习**)
   * 高水平指导
   * 类比 (比如： 电流 vs. 水)
 * 数据的属性和质量
   * 数据可能来自外界环境中的teacher，或者程序自身产生
   * 数据可能是可信赖的，也可能是包含噪声(noise)的
   * 数据可能是well-structured，也可能是unorganized
   * 数据可能包含正例反例，也可能只含有正例
 * 目标(goal or target)
   * 对一个概念或者类的描述
   * 获取计划
   * 求解问题的启发式信息
   * 其他形式的过程性知识

|         | 概念学习| 基于解释的学习|聚类|
| :-----: |:----------:| :----------:|:----------:|
| 数据 | 对于某个**目标类**大量的正例和反例 | 单一实例和特定领域的知识库 | 未分类实例集 |
| 目标 | 推断一个一般定义 | 推断一个一般概念 | 发现类别 |

  
* **所学知识的表示** (The representation of learned knowledge)
 * 谓词演算(predicare calculus)的表达式
 * 结构化表示(structured representation)比如: 框架(frame)或者对象(object)
 * 计划可以通过操作序列或者三角表(triangle table)来描述
 * 启发式信息(heuristics)可以用问题求解规则来表示

        size(obj1, small) ∧ color(obj1, red) ∧ shape(obj1, round)
        size(obj2, large) ∧ color(obj2, red) ∧ shape(obj2, round)

        从上面两个实例，我们可以把“ball”的概念泛化为：
        size(X, Y) ∧ color(X, Z) ∧ shape(X, round)

* **操作集合** (A set of operations)
给定训练实例集，学习器要有对representations有操作能力，典型的操作能力包括：
 * 对于符号表达式的泛化(generalizing)或者特化(specializing)
 * 调整神经网络的权值(weights)
 * 其他方式对representation的修改

        Replacing constants with variables:
            Obj(round,red,ball) generalizes to Obj(round,X,ball)
        Dropping conditions from a conjunctive expression:
            Obj(round) ∧ Obj(small) ∧ Obj(red) generalizes to Obj(round) ∧ Obj(red) 
        Adding a disjunct to an expression:  
            Obj(round) ∧ Obj(small) ∧ Obj(red) generalizes to Obj(round) ∧ Obj(small) ∧ ((Obj(red) v Obj(blue))
        Replacing a property with its parent in a class hierarchy
            Obj(red) generalizes to Obj(primary_color) if primary_color is a superclass of red (following the color class hierarchy)

* **概念空间** (The concept space)
上述的表述语言和操作定义了潜在的概念空间，学习器(learner)必须搜索这个空间来寻找期望的概念。

* **启发式搜索** (Heuristic search)
 * 利用训练数据和启发式信息进行有效地搜索
 * 下面举一个归纳学习的经典例子 from **Patrick Winston**

首先看到的是“拱门”的两个正例，描述如下图
![第一个正例：长方形顶](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-f7c033da182b6435_zpssrfveztf.png)
![第二个正例：锥形顶](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-dbb0769a13f5b409_zpsnh1rbmdj.png)

然后进行**泛化**，这里的**超类**(supertype)则是多边形(polygon)
![多边形包括：长方形和锥形](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-cc3e6331bd996afa_zpseitr02of.png)

泛化后我们可以得到对拱门新的描述
![泛化后：拱门的新描述](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-5c2166a76a9d0d1f_zpsf7cmswna.png)

最后根据**小差别**(near miss)的描述，我们特化得到了排除小差别的拱门描述
![小差别的描述](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-11addb85406e4ea9_zpsxr10br3v.png)
![特化后：拱门的新描述](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-5c2166a76a9d0d1f_zpsf7cmswna.png)

### 一些常见的基于符号的方法

*  监督学习 [Learn More](http://www.jianshu.com/p/5cd71856cb6e)
 * 概念学习 (Concepts learning)
 * 变形空间搜索 (Version space learning)
 * 决策树 (Decision Tree) [决策树之Python实现]()
* 无监督学习 [Learn More](http://www.jianshu.com/p/6c097c4d376b)
 * 发现程序(discovery program)比如：AM, BACON, SCAVENGER...
 * 类别发现(pattern discovery)
* 强化学习
 * 不同于监督学习，学习器并未告知要做什么，学习主体自身通过训练、误差和反馈来学习在环境中实现目标的最佳策略(optimal policy)。
 * 在强化学习中，主体通过发现在特定环境中做什么动作获得的**奖励最多**就采取什么动作。主体动作的影响不只是立即得到的奖励，而且还影响接下来的动作和最终的奖励。
 * 强化学习中最重要的两个特性：
   * 试错搜索(trial-and-error search)
   * 延期强化(delayed reinforcement)
 * Reinforcement learning is most successful when the environment is big or can not be precisely described.

### 归纳偏置 (Inductive Bias) 
* 归纳学习依赖于先验知识和对要学习的概念本质的假定。
* 归纳偏置是指学习器用来限制概念空间或者在这个概念空间选择概念的任何标准(criteria)。
* 归纳偏置的原因
 * 学习空间太大，基于搜索的学习就没有实用性。
 * 归纳泛化自身的本质-泛化并不保真。
 * 训练数据只是域中所有实例的一个子集，任意的一个训练集可以支持很多不同的泛化。所以仅有数据是不充分的，学习器必须对“可能的”概念做进一步假设。比如以启发式信息的形式出现。


## 连接的方法 (connectionist) [Learn More]()
Non-symbolic Learning Approach
* 神经网络(Neural network)或者连接网络(Connectionist network)学习知识的方法**不是基于符号化的**。
* 就如同我们人类的大脑一样，Neural network是由相连的人工神经元(Neuron)组成的系统。
* 神经网络并不是通过增加representations来学习，而是通过修改自身结构来学习。

## 遗传或进化的方法 (genetic/evoltionary)
Non-symbolic Learning Approach

更多内容，以后补充
## 动态或随机的方法 (dynamic/stochastic)

* 应用于很多复杂领域，例如人类语言的产生及理解。

更多内容，以后补充