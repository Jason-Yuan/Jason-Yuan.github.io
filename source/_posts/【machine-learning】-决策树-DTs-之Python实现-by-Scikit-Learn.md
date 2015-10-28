title: "【machine learning】- 决策树(DTs)之Python实现 by Scikit-Learn"
date: 2015-05-31 21:21:08
tags: ["Desicion Tree", "Machine Learning", "Python", "Scikit-learn"]
categories: "Python"
---

这学期AI课的最后一次assignment，老师要让大家实践一下decision tree，而且“You can use any existing machine learning tool for this task”，所以就有了我下面这篇文章，不足之处，望指正，共勉！

***
# 环境搭建
## 用Python实现机器学习的基础环境搭建, [click here](http://www.jianshu.com/p/24603369b7d0).

## 安装 pyparsing (1.5.7) 和 pydot 
注：以下针对于 Mac OS 并且使用 Python2.7

* 如果已经安装了pyparsing (2.x.x)，操作如下：
```
$ pip uninstalled pyparsing
$ pip install -Iv https://pypi.python.org/packages/source/p/pyparsing/pyparsing-1.5.7.tar.gz#md5=9be0fcdcc595199c646ab317c1d9a709
$ pip install pydot
```

* 如果没有安装过pyparsing任何版本，操作如下：
```
$ pip install -Iv https://pypi.python.org/packages/source/p/pyparsing/pyparsing-1.5.7.tar.gz#md5=9be0fcdcc595199c646ab317c1d9a709
$ pip install pydot
```

## 安装 GraphViz
![graphviz](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-f7e6a845be923dd9_zpsm8ahaw17.png)

> **Graphviz** (Graph Visualization Software) 是一个由AT&T实验室启动的开源工具包，用于绘制DOT语言脚本描述的图形。

* GraphViz不能通过pip安装。 所以，下载对于Mac的正确版本[(graphviz-2.36.0.pkg)](http://www.graphviz.org/Download_macos.php "GraphViz for Mac")之后，自行安装。
* 你可能还需要安装xlrd
```
$ pip install xlrd
```

* GraphViz [官网](http://www.graphviz.org/)
***
# 什么是Decision Tree？
决策树(Decision Tree)是基于符号的监督学习方法中的一种。更多相关知识，可以看我另外一篇[文章](http://www.jianshu.com/p/5cd71856cb6e)。
***
# 具体Python实现
* 导入library
```
import numpy as np
import pandas as pd
from sklearn import tree
from sklearn.feature_extraction import DictVectorizer
```

* 读取数据
* 使用pandas读取Excel文件，当然pandas还支持多种文件格式的读写，比如：text, sql,  json, csv ......
* 使用*pd.read_excel()* 读取后默认生成 [pandas.DataFrame](http://pandas.pydata.org/pandas-docs/dev/generated/pandas.DataFrame.html)
```
# read data from excel file as DataFrame
raw_train_data = pd.read_excel("/Users/boyuan/Desktop/TrainingData.xlsx", parse_cols=[1,2,3,4,5,6,7,8,9,10,11])
raw_test_data = pd.read_excel("/Users/boyuan/Desktop/TestingData.xlsx", parse_cols=[1,2,3,4,5,6,7,8,9,10,11])
```

* 清洗数据
```
# If the data has missing values, they will become NaNs in the resulting Numpy arrays.
# The vectorizer will create additional column <feature>=NA for each feature with NAs

raw_train_data = raw_train_data.fillna("NA")
raw_test_data = raw_test_data.fillna("NA")

exc_cols = [u'adjGross']
cols = [c for c in raw_train_data.columns if c not in exc_cols]

X_train = raw_train_data.ix[:,cols]
y_train = raw_train_data['adjGross'].values

X_test = raw_test_data.ix[:,cols]
y_test = raw_test_data['adjGross'].values
```

* 使用pandas *to_dict()* 将DataFrame转化成Dict
```
# Convert DataFrame to dict See more: http://pandas.pydata.org/pandas-docs/dev/generated/pandas.DataFrame.to_dict.html#pandas.DataFrame.to_dict
dict_X_train = X_train.to_dict(orient='records')
dict_X_test = X_test.to_dict(orient='records')
```

* [Encoding categorical features](http://scikit-learn.org/stable/modules/preprocessing.html)
* 使用Scikit-learn中的[DictVectorizer](http://scikit-learn.org/stable/modules/generated/sklearn.feature_extraction.DictVectorizer.html)
* DictVectorizer中使用的是[OneHotEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.OneHotEncoder.html#sklearn.preprocessing.OneHotEncoder)来实现
* 对于Label可以使用[LabelEncoder](http://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.LabelEncoder.html)
```
vec = DictVectorizer()
X_train = vec.fit_transform(dict_X_train).toarray()
X_test = vec.fit_transform(dict_X_test).toarray()
```

* 最后，把train data喂给fit()函数，用score()函数检验模型在test data上的表现！
```
clf = tree.DecisionTreeClassifier()
clf = clf.fit(X_train,y_train)

score = clf.score(X_test,y_test)
```

* 当然也可以把tree导出到dot文件中，使用GraphViz画图
```
from sklearn.externals.six import StringIO
with open("文件名称.dot", 'w') as f:
    f = tree.export_graphviz(clf, out_file=f, feature_names= vec.get_feature_names())
```

在CLI上输入如下命令:
`$ dot -Tps tree.dot -o tree.ps`     (PostScript 格式)
`$ dot -Tpng tree.dot -o tree.png`    (PNG 格式)

* 最后附上两张tree图，分别是没有设置max_depth以及设置max_depth=8的情形
![tree_with_8_depth](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-f463e45111f72762_zpsxuf4rwjg.png)
![tree_without_limit_depth](http://i781.photobucket.com/albums/yy93/Jason__Yuan/424375-c562fc489dd63ae1_zpsb4u38csr.png)

* 网上有很多流行的数据集，比如简书上的[《最流行的4个机器学习数据集》](http://www.jianshu.com/p/be23b3870d2e)
* 我使用的是UCI上一个判断年工资是否大于50k的[数据集](http://archive.ics.uci.edu/ml/datasets/Adult)

***

# 结语
* 这是第一次使用Python进行data mining，学习使用Python断断续续也有大半年，从写简单的算法课作业，后来写爬虫，接触Flask写网站， 不断体会到“人生苦短，我用Python”。这当然基于Python非常完善的代码库。
* 没有一种语言是万能的，Python当然也不是，但不得不说在某些领域Python确实作为一种高级语言，可以让你更专注你核心要做的事情， 而非语言本身。