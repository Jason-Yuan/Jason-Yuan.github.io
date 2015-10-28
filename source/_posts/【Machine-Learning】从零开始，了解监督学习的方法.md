title: "【Machine Learning】从零开始，了解监督学习的方法"
date: 2015-05-05 15:23:19
tags: ["Machine Learning", "监督学习", "AI"]
categories: "AI"
---

***
# 概念学习 

## 一种常见的学习方法 -- 泛化(generalization)
* 泛化的定义
 * 从集合的角度：表达式P比表达式Q更泛化，当且仅当P ⊇ Q
 * 比如我们可以将 
排球，篮球，足球 ==(泛化为)==>球类或者运动
* 机器学习中主要的泛化操作有：
 * 变量替换常量
 * 从合取表达式中去掉一些条件
 * 对表达式增加一个析取式
 * 用属性的超类替换属性

<!-- more -->
## 通过泛化进行概念学习

* 什么是覆盖(covering)？
如果说概念P比概念q更泛化，我们就说p覆盖q

* 概念空间(concept space)的定义
 * 概念空间是一些**潜在的概念**的**集合**
 * 潜在概念(potential concept / candidate concept)是由泛化、特化等学习方法产生的
下图就是一个拥有如下属性和值的object的概念空间
Size = {small, large}
Color = {red, white, blue}
Shape = {ball, brick, cube}

![概念空间](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-41e4013ad497c87c_zpsxdpmth8s.png)

**从下至上是一个泛化的过程**，比如Obj(X, Y, ball)就可以覆盖Obj(X, red, ball)和Obj(small, X, ball)等等，这也是通过泛化就行概念学习的体现。
***
# 变形空间搜索
> Version space search (Mitchell 1978, 1979, 1982) illustrates the implementation of inductive learning as search through a concept space.

说白了就是从训练实例可以生成一个**概念空间**，比如上图。然后再从概念空间中**搜索**一个能**覆盖**所有概念的概念。 比如上图的Obj(X, Y, Z)。

## 变形空间(version space)的定义
## 三种搜索概念空间的算法
```
特殊到一般 (specific to general)
一般到特殊 (general to specific)
候选解排除 (candidate elimination)
```
* 这些算法依赖于**变形空间**的概念，在有更多**实例**时，可以缩减变形空间的大小。
* 目标：**学习到的概念不仅足以覆盖所有正例，而且能排除所有的反例**。上面讲的Obj(X, Y, Z)虽然可以覆盖所有正例，但可能太泛化了。
* 避免超泛化(overgeneralization)的方法：
  * 采用尽可能小得泛化，使之只覆盖正例
  * 用反例排除超泛化了得概念
![反例在防止超泛化中的作用](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-a1be0adc572d3c96_zpsgpj8tkqm.png)

### 特殊到一般
* 维护一个假设集S (即候选概念定义集)
* 最特殊的泛化(Maximally specific generalization)
一个概念c是最特殊的,如果：
① 覆盖所有正例，而不覆盖反例
② 对于所有其他覆盖正例的概念c', c ≤ c'

![由特殊到一般的搜索](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-cc45e8be3c858e1f_zpsm4nz7xfm.png)

### 一般到特殊
* 维护一个假设集G(即候选概念集合)
* 最一般概念(Maximally general concept)
一个概念c是最一般的，如果：
① 覆盖所有正例，而不覆盖反例
② 对于任意其他不覆盖反例的概念c', c ≥ c'

下图的背景为：
size = {large, small}
color = {red, white, blue}
shape = {ball, brick, cube}
所以由第一个反例我们可以特化出：
size不能是small => obj(large, Y, Z)
color不能是red => obj(X, white, Z) 和 obj(X, blue, Z)
shape不能是brick =>obj(X, Y, ball) 和 obj(X, Y, cube)

![由一般到特殊的搜索](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-e5859ccb592c95b2_zpse2afwlpg.png)

### 候选解排除

* 候选解排除法综合上面两种办法，**双向搜索**
* 维护两个候选概念集合S和G
* 算法特化G并泛化S直到它们收敛在目标概念上

![双向搜索](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-6e08cf2b0f8985a8_zpskuqjf18t.png)


## 评估候选解排除算法 

### 优点
  * 候选解排除算法是增量式的(incremental)，所以不同于其他的算法需要在学习之前给出所有训练实例

### 缺点
* 像其他搜索问题一样，基于搜索的学习必须处理问题空间的合并问题
* 候选解排除算法是不能有噪声(noise)的 

***
# 决策树
## 什么是决策树？
> 机器学习中，**决策树**是一个预测模型；他代表的是对象属性(property)与对象值(value)之间的一种映射关系。树中每个**节点**表示某个**对象**，而每个**分叉路径**则代表的某个可能的**属性值**，而每个**叶结点**则对应从根节点到该叶节点所经历的路径所表示的对象的值。决策树仅有单一输出，若欲有复数输出，可以建立独立的决策树以处理不同输出。
-来自 Wikipedia

* 决策树可以分为**分类树**和**回归树**，分别针对于离散变量和连续变量。
* 再简单点说就是，建立一棵能把所有训练数据进行正确分类的树型结构。

下面举个简单的例子助于理解。对于估计个人**信用风险**(risk)问题，要基于这样一些属性，比如**信用历史**(credit history)、**当前债务**(debt)、**抵押**(collateral)和**收入**(income)。下表列出了已知信用风险的个人的样本。

![已知信用风险的个人的样本](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-eda13a963a184b37_zps3tddcbny.png)

基于上面的信息，我们可以得到下面**两个不同的**决策树。

![决策树 A](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-3c37a63a87058eda_zpsljxj0nml.png)

![决策树 B](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-605d7e7cfc809a73_zpslvbmboj6.png)

我们可以发现，虽然两棵决策树都能对给定实例集进行正确分类，但是**决策树B**要比**决策树A**简单得多。可见，对给定实例集分类所必需的树的大小，**随测试属性的顺序**而不同。

## 常见的建立决策树的算法
### ID3
ID3 was developed in 1986 by Ross Quinlan. The algorithm creates a multiway tree, finding for each node (i.e. in a greedy manner) the categorical feature that will yield the largest information gain for categorical targets. Trees are grown to their maximum size and then a pruning step is usually applied to improve the ability of the tree to generalise to unseen data.

下文会重点介绍ID3算法

### C4.5
C4.5 is the successor to ID3 and removed the restriction that features must be categorical by dynamically defining a discrete attribute (based on numerical variables) that partitions the continuous attribute value into a discrete set of intervals. C4.5 converts the trained trees (i.e. the output of the ID3 algorithm) into sets of if-then rules. These accuracy of each rule is then evaluated to determine the order in which they should be applied. Pruning is done by removing a rule’s precondition if the accuracy of the rule improves without it.

### C5.0
C5.0 is Quinlan’s latest version release under a proprietary license. It uses less memory and builds smaller rulesets than C4.5 while being more accurate.

### CART
CART (Classification and Regression Trees) is very similar to C4.5, but it differs in that it supports numerical target variables (regression) and does not compute rule sets. CART constructs binary trees using the feature and threshold that yield the largest information gain at each node.

## ID3算法详解

### 奥卡姆剃刀(Occam's Razor)
奥卡姆剃刀最早是由逻辑数学家William of Occam于1324年提出的：
>It is vain to do with more what can be done with less. . . . Entities should not be multiplied beyond necessity.

简单点说，找到能够符合数据的**最简单的**解！

### ID3算法的基本思路
给定训练实例集和能对它们正确分类的一组不同的决策树，我们想要知道哪棵树对未来实例正确分类的可能性最大。ID3算法**假定**可能性最大的树是能够覆盖所有训练实例的**最简单的决策树**。
注：ID3不能保证每次都生成最小的树，只是一种**启发式算法**。

ID3采用自顶向下决策树归纳(Top-Down Decision Tree Induction)：
* 首先确定哪一个属性作为**根节点**(root node)的测试
* 选择分类能力最好的(信息增益最大)属性，作为**当前节点**(current node)的测试
* 用这个测试来划分实例集，该属性的每一个可能值都成为一个**划分**(partition)
* 对于每一个划分重复上述过程，建立其子树
* 直到一个划分中的所有成员在同一类别中，这个类别成为树的**叶节点**(leaf node)

注：我们可以把所有可能的决策树集合看成是定义一个变形空间(version space)。ID3在所有的可能树的空间中实现一种**贪心搜索**，对当前树增加一个子树，并继续搜索，而且**不回溯**。

### 如何判断最佳分类属性
ID3算法是由Quinlan首先提出的，该算法是以**信息论**(Information Theory)为基础的，ID3通过把每个属性当作当前树的根节点来度量信息增益，然后算法选取提供最大信息增益的属性。

① 信息增益的度量标准 - **熵**(Entropy)
熵主要是指信息的混乱程度，变量的不确定性越大，熵的值也就越大。
变量的不确定性主要可以体现在两个方面：
* **可能信息的数量**
简单地说，掷硬币有两种可能信息(正面或者反面)，掷筛子有六种可能信息(1,2,3,4,5,6)，所以正确预测筛子的信息对我们更有价值：掷筛子游戏赢钱更多。
* **每条信息出现的概率**
简单地说，如果我们假设对掷硬币作弊使它正面出现的概率为3/4。那么既然我已经知道猜正面的概率为3/4，告诉我掷硬币结果的消息就不如关于未作弊的硬币的消息更有价值。(后面讲了具体计算)

综上，给定消息空间M = {m1, m2, .....}以及相应的概率P(mi)，熵的公式为：

![熵的公式](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-04c43ffa9e383182_zpsk0pa3ta8.png)

未作弊和作弊的熵计算如下：

![未作弊的熵值计算](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-08aa2be0f6c4b77f_zpsrap7f3zj.png)

![作弊后的熵值计算](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-a54653f9550874a2_zpsyckuzuun.png)

为作弊熵值更大，掷硬币的消息更有价值！！！

② 信息增益(Information Gain)
假设有训练实例集C。如果我们通过属性P作为当前树的根结点，将把C分成子集{C1, C2, C3 .....}。再把P当作跟结点完成树所需的信息的期望为：
![完成树所需的信息的期望](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-1030ca182c9e17fd_zpsqdhfsv1d.png)

所以从从属性P得到的增益通过树的**总信息量**减去**完成树的信息期望**来计算：
![信息增益](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-123992830bcb60e1_zpssg1lv8vb.png)

还是举信用风险的例子，P(low)=5/14, P(moderate)=3/14，P(high)=6/14。所以总信息量计算如下：
![总信息量](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-8c3dd871c9c3d934_zpsqdfwpuq9.png)

如果把收入(income)作为树的根结点，表中的实例被划分为C1 = {1,4,7,11}、C2 = {2,3,12,14}和C3 = {5,6,8,9,10,13}。
![决策树的一部分](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-8337a8373b8f84e1_zpshapaqkin.png)

完成树所需的期望值为：
![完成树所需的期望值](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-cfd0cceac8d17bab_zpsgihrpbya.png)

**最后，gain(income) = 1.531 - 0.564 = 0.967 bits**
类似的，可以得到：

| 属性| 信息增益(bits)|
| :-----: |:------:|
| gain(credit history) | 0.266 | 
| gain(debt) | 0.063 | 
| gain(collateral) | 0.206 | 

由于收入提供了最大的信息增益，所以ID3会选择它作为根结点。

### 评价ID3
虽然ID3算法产生简单的决策树(包括根结点，决策结点和叶结点)，但这种树对预测未知实例的分类不见得一定有效。

## 评估决策树 
* 决策树适用于离散型数据，变量的结果是有限集合。
* 优点
  * 决策树计算复杂度不高，便于使用，高效！
  * 决策树可以处理具有不相关特征的数据。
  * 决策树可以很容易的构造出一系列易于理解的规则。
* 缺点
  * 处理缺失数据，坏数据的以及连续型数据的困难。
  * 大的数据集可能会产生很大的决策树。
  * 忽略了数据集中属性之间的关系。
  * **过度拟合**(涉及到剪枝)

# 参考文献
1. [Artificial Intelligence，6th Edition]()
2. [从决策树学习谈到贝叶斯分类算法、EM、HMM](http://blog.csdn.net/v_july_v/article/details/7577684)
3. [机器学习经典算法详解及Python实现--决策树（Decision Tree）](http://blog.csdn.net/suipingsp/article/details/41927247)
4. [Scikit-learn 文档](http://scikit-learn.org/stable/documentation.html)