title: "基于Python的机器学习框架scikit-learn学习笔记02"
date: 2017-11-20 18:00:00 +0800
update: 2017-11-22 19:00:00 +0800
author: me
# cover: "-/images/scikit-learn-example01.png"
tags:
    - 机器学习
    - Python
    - scikit-learn
    - 编程
preview: 基于Python的机器学习框架scikit-learn学习笔记02。

---

> 2017-11-20 周一 晴 北京 清华大学 开始<br>
> 2017-11-20 周一 晴 北京 清华大学 完成

## 1 scikit-learn的使用 ##
### 1.1 DecisionTreeRegressor ###
基于决策树的回归器[DecisionTreeRegressor](http://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeRegressor.html)位于`sklearn.tree`。官方文档地址：[http://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeRegressor.html](http://scikit-learn.org/stable/modules/generated/sklearn.tree.DecisionTreeRegressor.html)。在文档的最后，有几个使用了DecisionTreeRegressor的例子。

DecisionTreeRegressor定义：

```python
class sklearn.tree.DecisionTreeRegressor(criterion=’mse’, splitter=’best’, max_depth=None, min_samples_split=2, min_samples_leaf=1, min_weight_fraction_leaf=0.0, max_features=None, random_state=None, max_leaf_nodes=None, min_impurity_decrease=0.0, min_impurity_split=None, presort=False)
```

其比较重要的几个参数意义如下。其实默认的参数值应该就是我们需要的bagging，我们最多改改基学习器个数`n_estimators`。

| 参数          | 意义         | 默认            |       备注   |
| ------------- |-------------| -----          |         -----|
|base_estimator | 基学习器     | None，对应决策树 |             |
|n_estimators   | 基学习器个数  |   10           |     int型   |
|max_samples    | 样本抽取个数或比例  |   1.0     | 整数对应个数，小数则对应比例 |
|max_features   | 特征抽取个数或比例  |   1.0     | 整数对应个数，小数则对应比例 |
|bootstrap      |样本抽取是否放回|   True        |    bool型    |
|bootstrap_features|特征抽取是否放回|   false    |    bool型    |
|oob_score      |是否用bag外样本估计误差|        |    bool型    |


使用下面的代码训练了一个DecisionTreeRegressor，真的非常简单。声明分类器，`fit`用于训练，`predict`用于分类。关于加载样本数据相关的内容，在本文的后面。
```python
import numpy as np
from sklearn.ensemble import BaggingClassifier

# 加载样本数据
dataset = np.loadtxt("krkopt.txt", delimiter=",")
X = dataset[:,0:6]
y = dataset[:,6]

# 训练分类器
bagging = BaggingClassifier()
bagging.fit(X, y)

# 测试：用训练好的分类器分类
xt = [[ 3., 2., 5.,7., 8., 6.],
      [ 3., 2., 5.,7., 8., 7.]]
yt = bagging.predict(xt)
print(yt)
```

另外，如果想要计算错误率的话，不用自己费事计算理论，直接用`Classifier.score(XTest, yTest)`就可以计算预测正确率。其中`XTest`是测试输入，`yTest`是与之对应的正确结果。

### 1.4 利用sklearn生成训练样本数据 ###

因为没有时间系统学习sklearn，只能一边做一边学。比如`sklearn.datasets`这个东西，我已经在很多例子中看过到过了，貌似是里面有很多现成的数据，而且还有可以生成样本数据的函数。

因为我想要一组可以测试算法的样本数据，恰巧就在[RandomForestClassifier](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html)的文档中看到了`sklearn.datasets.make_classification(...)`这个函数。它可以用来生成一些简单的分类训练样本。

`make_classification`函数定义如下。其中比较重要的参数是`n_samples`、`n_features`、`n_classes`、`n_informative`、`n_redundant`。具体我也还没太弄明白，不过反正是能用了。有机会的话后面再研究。

```python
sklearn.datasets.make_classification(n_samples=100, n_features=20, n_informative=2, n_redundant=2, n_repeated=0, n_classes=2, n_clusters_per_class=2, weights=None, flip_y=0.01, class_sep=1.0, hypercube=True, shift=0.0, scale=1.0, shuffle=True, random_state=None)
```

使用方法如下，生成6个特征、17个类的分类训练样本，样本个数为10000。
```python
from sklearn.datasets import make_classification
X, y = make_classification(n_samples=10000, n_features=6, n_classes=17, n_informative=6, n_redundant=0, random_state=0, shuffle=False)
```

## 2 Python相关 ##
### 2.1 随机抽样与bootstrap抽样 ###
对于保存在向量中的数据，随机抽出它们的序号即可。

- 随机抽样，不重复：从nSamples个数据中抽出nSamples_train个数据。

```python
import random
indexs = random.sample( xrange(nSamples), nSamples_train )
```

- bootstrap抽样，有重复从nSamples个数据中有重复地抽出nSamples个数据。注意：`numpy.random.randint( 0, nSamples, nSamples )`最大抽到`nSamples-1`。

```python
import numpy
indexs = numpy.random.randint( 0, nSamples, nSamples )
```

### 2.2 range()与xrange()的区别 ###
range()生成一个向量（list），xrange()返回一个生成器。xrange()更节省空间，特别是长度比较大时，使用xrange()更好。
但是要注意的是，Python 3里已经没有xrange()了，所以以上只有在Python 2管用。

### 2.3 numpy删除向量中的元素 ###
使用`numpy.delete()`，第二个参数为要删除元素的序号，重复只起一次作用。

```python
aaa = np.array( range(10) )
bbb = np.delete(aaa, [1, 2, 1, 3, 1])
```

得到结果：
```
bbb = [0, 4, 5, 6, 7, 8, 9]
```

### 2.4 numpy.loadtxt() ###
从文本中加载样本数据用到`numpy.loadtxt()`，如果样本中第一行是各个数据的名称，我们不需要。这时候设置参数`skiprows`即可，其意义是忽略文本的前`skiprows`行。比如我们想忽略第一行，设置`skiprows=1`即可。

```python
dataset = np.loadtxt("CASP.csv", delimiter=",", skiprows=1)
```