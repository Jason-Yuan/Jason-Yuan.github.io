title: "【Machine Learning】从零开始，了解无监督学习的方法"
date: 2015-05-13 15:37:58
tags: ["无监督学习", "Machine Learning", "AI"]
categories: "AI"
---

***

# 什么是无监督学习
无监督学习没有教师，需要学习器自身形成(form)和评价(evaluate)概念。

科学是人类中无监督学习最好的例子，因为科学家没有教师的指点，他们提出假设来解释现象，并设计实验来验证假设。

hypothesis -> generality -> conclusion

<!-- more -->
***

# 发现和无监督学习(Discovery and unsupervised learning)

##  Automated Mathematician(AM)
* AM是最早的和最成功的发现程序之一
* AM获取了许多有趣的数学概念，比如：集合论的概念。通过搜索这个数学概念空间，AM发现自然数和几个重要的数论的概念，比如质数的存在性。
* AM并不具备学习能力

## BACON
* BACON发展了量化科学定律的形式的计算模型
* 用与行星和太阳间的距离以及行星的旋转周期相关的数据，BACON“重发现”行星运动的开普勒定律。

## SCAVENGER
* 用ID3算法的一个变种来改进它形成类比的能力。
***

# 聚类分析(Clustering analysis)
## 什么是聚类分析
>Cluster analysis or clustering is the task of grouping a set of objects in such a way that objects in the same group (called a cluster) are more similar (in some sense or another) to each other than to those in other groups (clusters).
-from wikipedia

* 聚类是一种**无监督学习**
* 聚类**唯一**需要的信息就是**样品之间的相似性**(similarity between examples)。
* 一个好的聚类要满足：
  * High intra-cluster similarity
  * Low inner-cluster similarity
![聚类](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-e48c24607f94a9dc_zpsm7ralnfd.png)
**注：computationally difficult problem (NP-hard)**

## 相似性(similarity)的定义
![相似性通常很难去定义](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-e1acf6b32d6ba264_zpsa7xtshgc.jpg)

* 相似性**衡量标准**的选择，对于聚类(clustering)十分重要。
* 与**相似性**相对应的就是**差异性**(dissimilarity或者说distance)。
* Proximity通常指的相似性(similarity)或者差异性(dissimilarity)
* 现有的一些对于distance的衡量方法：
  * 欧几里得距离(Euclidean distance)
  * 明氏距离(Minkowski distance) 
  Minkowski distance is a generalization of Euclidean distance
  * 曼哈顿距离(Manhattan distance/City Block distance)
  * Kernelized (non-linear) distance

## 聚类在生活中的应用
类别对于人类如何分析和描述世界起了至关重要的作用，人类其实非常擅长做分类，一个小孩子就可以将熟悉的事物分为建筑、机动车、动物、植物.......

* 生物学(Biology)
生物学家花费多年时间为所有的生物创建了一套生物学分类法(等级结构分类)：界(kingdom), 门(phylum), 纲(class), 目(order), 科(family), 属(genus), 和种(species)。

* 信息检索(Information Retrieval)
万维网包含了数十亿网页，搜索引擎可以将搜索结果进行分类，分成不同的clusters。每个cluster可能代表搜索的一个方面。比如当你搜索“电影”，搜索结果可能被分为“电影预告片”“电影导演”“电影院”......当然也可能会继续向下层级的分类，完善用户体验。

* 气候(Climate)
理解地球气候需要在大气和海洋中寻求模式，聚类分析已经被应用于相应的模式寻找过程，例如海洋对陆地气候的显著影响。

* 心理学和医学(Psychology and Medicine)
疾病会频繁的出现一系列新的变种，聚类分析可以用来鉴定和识别这些新的不同的子类。

* 商业(Business)
聚类分析被用来发现不同的客户群，并且通过购买模式刻画不同的客户群的特征。

## 不同类型的Clustering

### 层次聚类 vs 划分聚类
被讨论的最多的区分不同聚类类型的依据就是看被划分好的这些clusters是嵌套的(nested)还是非嵌套的(untested)，或者更通俗点说，是hierarchical还是partitional.

* 层次聚类(Hierarchical clustering)
![层次聚类](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-a77090dae1d381fc_zpshenomtz0.png)

* 划分聚类(Partitional or Flat clustering)
![划分聚类](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-00511da621a7fad9_zpsvkgqjxvn.png)

### 互斥聚类 vs 重叠聚类 vs 模糊聚类
* 互斥聚类(Exclusive clustering)每个对象都指派到单个簇。
* 重叠聚类(Overlapping clustering)用来反映一个对象同时属于多个簇(类)这一事实。 
* 模糊聚类(Fuzzy clustering)每个对象以一个0(绝对不属于)和1(绝对属于)之间的录属权值属于每个簇。

### 完全聚类 vs 部分聚类
* 完全聚类(Complete clustering)将每一个对象都分配到某一个簇(cluster)
* 部分分类(Partial clustering)有些对象没有被聚类，比如一些噪声(noise)或者离群值(outliers)

## 不同类型的Cluster

在很多实际应用中，**cluster**的概念并没有一个很好的定义。为了更好的理解决定一个cluster由什么构成的困难性，我们在下图展示了同样的20个点，用三种不同的方法去把它们划分到不同的clusters。

![The notion of cluster is important](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-13f8aba27881d42a_zpsd7yommg4.png)

上图阐明了其实一个cluster的定义不是精确的，固定不变的。对于cluster最好的定义依赖于**数据的性质**和**预期结果**。

聚类(Clustering)的目标是要找到一组有意义的对象(object)或者说cluster。 这里所说的有意义或者说有用，是针对数据分析的目标而言的。毫无悬念，在实际当中已经有一些不同的对于cluster的概念，被证明是有意义的，具体如下：

### 明显分离的(Well-Separated)
不同组中的任意两点之间的距离都大于组内任意两点之间的距离。明显分离的簇不必是球形的，可以具有任意形状。
![Well-Seperated cluster](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-a5ee0035d6c51b7c_zpsqavgydrc.png)

### 基于原型的(Prototype-Based /center-based clusters)
簇是对象的集合，其中每个对象到定义该簇的原型的距离比到其他簇的原型的距离更近（或更加相似）。对于具有连续属性的数据，簇的原型通常是质心，即簇中所有点的平均值。这种簇倾向于呈球状。基于原型的聚类技术创建数据对象的单层划分。
![Prototype-Based cluster](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-9149c0129d8c0c90_zpsmifq2cuf.png)

### 基于图的(Graph-Based)
如果数据用图表示，其中节点是对象，而边代表对象之间的联系，则簇可以定义为连通分支，即互相连通但不与组外对象连通的对象组。当簇不规则或缠绕时，簇的这种定义是有用的。但是，当数据具有噪声时就可能出现问题。也存在其他类型的基于图的簇。一种方法是定义簇为团，即图中相互之间完全连接的节点的集合。
![Graph-Based cluster](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-3644715016538939_zpsntvntf00.png)

### 基于密度的(Density-Based)
簇是对象的稠密区域，被低密度的区域环绕。当簇不规则或互相盘绕，并且有噪声和离群点时，常常使用基于密度的簇定义。
![Desity-Based cluster](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-f304c498cdb23db2_zpsfclnvsht.png)

### 共同性质的/概念簇(Shared-Property /Conceptual Clusters)
把簇定义为有某种共同性质的对象的集合。发现这样的簇的过程称作概念聚类。
![Conceptual Cluster](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-68cfe3b307ea2806_zpsyza63czu.png)

## K-means 简介

* K-means 属于 基于中心的(Center-Based)划分(partitional)聚类法方
* Each cluster is associated with a centroid(center point)
* Each point is assigned to the cluster with the closest centroid
* K的值必须事先给定 (K centroids)
* K-means 算法的基本步骤：
 * 从 n个数据对象任意选择 k 个对象作为初始聚类中心
 * **迭代**
   * 通过把每个点分配给最近的聚类中心，从而形成K个类
   * 重新计算每个类的聚类中心
 * **终止** 如果计算后，聚类中心不发生改变
![K-means算法，过程演示(K=2)](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-6ec53c365382a10e_zps86m23wba.png)

* K-means 算法优点
 * 算法框架清晰，简单，容易理解。
 * 本算法确定的k个划分到达平方误差最小。当聚类是密集的，且类与类之间区别明显时，效果较好。
 * 对于处理大数据集，这个算法是相对可伸缩和高效的，计算的复杂度为O(NKt)，其中N是数据对象的数目，t是迭代的次数。一般来说，K<<N，t<<N 。
* K-means 算法缺点
 * K-means算法中k是事先给定的，这个k值的选定是非常难以估计的。
 * 算法的时间开销是非常大的。
 * K-means算法对异常数据很敏感。在计算质心的过程中，如果某个数据很异常，在计算均值的时候，会对结果影响非常大。

***

# 结语和参考文献
1. [Cluster Analysis: Basic Concepts and Algorithms](http://www-users.cs.umn.edu/~kumar/dmbook/ch8.pdf)
2. [Clustering - ccsu](http://www.cs.ccsu.edu/~markov/ccsu_courses/DataMining-10.pdf)
3. [Clustering - Matteo Pardo](http://lectures.molgen.mpg.de/algsysbio10/clustering.pdf)
4. [Data Clustering: K-means and Hierarchical Clustering￼](http://www.cs.utah.edu/~piyush/teaching/4-10-print.pdf)
5. [Cluster analysis wikipedia](http://en.wikipedia.org/wiki/Cluster_analysis)
6. [聚类算法总结](http://blog.csdn.net/yinlili2010/article/details/39452395)
7. [文本聚类算法介绍](http://blog.csdn.net/xiaojimanman/article/details/44977889)
8. [ 漫谈 Clustering (1): k-means](http://blog.csdn.net/aa512690069/article/details/11918401)
9. [聚类分析](http://wiki.mbalib.com/wiki/聚类分析)
10. [聚类算法：K-means](http://shiyanjun.cn/archives/539.html)
11. [Artificial Intelligence，6th Edition]()
12. [数据挖掘技术（四）——聚类](http://blog.sina.com.cn/s/blog_6002b97001014nja.html)