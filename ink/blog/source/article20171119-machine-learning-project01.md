title: "机器学习课第一次作业记录"
date: 2017-11-19 13:00:00 +0800
update: 2017-11-20 14:00:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 机器学习
    - Python
    - scikit-learn
    - 编程
preview: 机器学习课第一次作业记录：Bagging与Random Forest；MultiBoosting与Iterative Bagging。

---

> 2017-11-19 周日 阴 北京 清华大学

## 0 作业内容 ##
本次作业是周志华老师的《机器学习》第八章课后题的7、8小题。在网上找到了一个简单的描述性答案：[机器学习(周志华) 参考答案 第八章 集成学习](http://blog.csdn.net/icefire_tyh/article/details/52194771)。这里面是这样说的（也不知道对不对）：

- 7.试述随即森林为什么比决策树Bagging集成的训练速度快。答：随机森林不仅会随机样本，还会在所有样本属性中随机几种出来计算。这样每次生成分类器时都是对部分属性计算最优，速度会比Bagging计算全属性要快。

- 8.MultiBoosting算法与Iterative Bagging的优缺点。答：MultiBoosting由于集合了Bagging，Wagging，AdaBoost，可以有效的降低误差和方差，特别是误差。但是训练成本和预测成本都会显著增加。Iterative Bagging相比Bagging会降低误差，但是方差上升。由于Bagging本身就是一种降低方差的算法，所以Iterative Bagging相当于Bagging与单分类器的折中。

## 1 决策树Decision Tree ##

1. Bagging和RF的基学习器
2. AdaBoost的基学习器
3. 决策树的深度对其性能有很大影响。在sklearn中，默认设置是将决策树训练到最深、最细，在训练误差率上表现好，但是实际上是过拟合，这样在训练误差率上反而不一定好。
4. 接上一条，尤其是在训练AdaBoost分类器时，如果每个决策树都过拟合，会使AdaBoost分类器的性能受到限制。在这时候，取小一点的决策树深度，反而能够使得最后的AdaBoost分类器的错误率更低。
5. 研究决策树的深度与训练时间之间的关系

## 2 Bagging 与 Random Forest ##
1. RF在训练速度上，收敛速度上要比Bagging快，与理论符合。这一点其实已经把第一题完成了。
2. 在题目给的国际象棋训练数据集上，RF并没有比Bagging的误差率好；这可能是因为每两个特征代表一个棋子的位置，RF抽取特征训练反而效果不好。这一猜测已经经过验证。
3. 决策树的深度对Bagging 与 Random Forest的性能是由影响的，需要研究。
4. 11.24日：确实，在减小决策树的深度后，体现出了随机森林比Bagging的优势，在深度为5时，出现了与书上一致的结果。这以结果在之前没有预料到。我的结论是，对于chess数据集，RF的机制对于误差的降低，没能达到弥补随机选择特征带来的误差增大；只有在决策树深度较低时，RF机制的误差降低才能比带来的误差多。**需要查一下，RF的机制。**

## 3 AdaBoost ##
1. AdaBoost在后面的MultiBoosting和Iterative Bagging中都会用到。
2. AdaBoost不能并行学习，它比Bagging慢。
3. 研究决策树的深度对AdaBoost性能的影响。包括训练集误差率和测试集误差率。
4. AdaBoost的训练时间会随着基决策树的深度先增大后减小。这是因为在决策树深度达到一定值后，基学习器的训练错误率非常低，甚至等于1，AdaBoost的收敛速度非常快，根本达不到预定的机学习器个数。实际上此时，基决策树已经开始出现比较严重的过拟合情况，在这种情况下，AdaBoost分类误差也开始出现增大的情况。
5. 而Bagging就不同了，它的训练时间会一直增加。相对应的，Bagging的分类错误率也会一直降低，这与Iterative Bagging文献中所说的情况一致。


## 4 MultiBoosting ##
1. MultiBoosting使用Wagging时比使用Bagging的性能要好，已经过验证，与原文结论一致。
2. 在所给训练集上，MultiBoosting与AdaBoost的性能不相上下，而Bagging的性能比前两者要差。原文结论中也说了，只是说MultiBoosting只是在较多的训练集上性能比AdaBoost和Bagging好，并不是所有的都好，在这个训练集上可能就是差。
3. 自己写的MultiBoosting训练时间比AdaBoost略长，但是在采用sklearn封装好的bagging测试MultiBoosting算法时，训练时间是短的。这是因为bagging可以并行学习，而我们自己写的MultiBoosting没有使用并行学习。这一结果也与原文的结论相一致。
4. 我的任务是比较MultiBoosting和后面的Iterative Bagging，但是目前到底怎么比才好？
5. 文章里的偏差bias、方差variance、误差error，我对error是有直观认识的，但是对偏差、方差该怎样计算和评价？有时间的话可以研究下。比如sklearn的这个例子：[Single estimator versus bagging: bias-variance decomposition](http://scikit-learn.org/stable/auto_examples/ensemble/plot_bias_variance.html)。貌似是对一个模型来说，重复训练多次，以训练多次的结果来求error、bias和variance。
6. 还有这篇文章：[使用sklearn进行集成学习——理论](https://www.cnblogs.com/jasonfreak/p/5657196.html)。以及这篇：[SCIKIT-LEARN : BIAS-VARIANCE TRADEOFF](http://www.bogotobogo.com/python/scikit-learn/scikit_machine_learning_Bias-variance-Tradeoff.php)。偏差和方差的折中（bias-variance tradeoff）：方差大（偏差小）的往往是对训练集处理得很好，但是有可能是过拟合，对于噪声或干扰敏感，对于测试集可能效果不好。偏差大（方差小）的往往模型比较简单，但是倾向于拟合程度不够，不能从数据中找到足够的规律。
7. 原文中指出对于偏差、方差，有些定义只适用于回归。因此有人提出了一些其它的能用于分类的定义。所以说，在分类和回归对于偏差和方差的定义是不一样的。
8. 实验结果看出，对于chess数据集，MultiBoost有最低的错误率。最低的错误率并不来自最大深度的基决策树，而是存在一个最佳的决策树深度。
9. chess数据集：MultiBoost的分类错误率与AdaBoost错误率的走势是一致的。在机学习器个数相同时，MultiBoost的分类错误率与AdaBoost错误率的最佳值差别不大，略少一点。
10. chess数据集：但是在基学习器数目达到较大时，MultiBoost相比于AdaBoost的优势就体现出来了，AdaBoost会很快收敛(没有误差就没有再增加基学习器的必要了)，导致有效的基学习器个数不够。而MultiBoost则能够更好地增加基学习器个数，减低分类的错误率。
10. chess数据集：MultiBoost使用Wagging比Bagging要好。特别是在基决策树的深度达到最佳值附近后，使用Wagging得到的错误率明显比Bagging低。与文献一致。
11. 人工数据集：有着类似的结果，只不过最佳决策树深度从15左右变为25左右。
12. 对不同的数据集，不同学习模型的分类效果是不一样的。
13. 还可以看出一点：对于AdaBoost算法，训练时间开始减少的时候，往往意味着分类错误率开始增加。这是因为算法过快收敛，导致有效的基学习器不够多。
14. 从实验结果可以看出，Bagging需要小的bias，大的variance，因为Bagging只能够降低variance，不能降低bias；而AdaBoost能够同时降低bias和variance，它需要的是bias和variance大小都适中的基学习器。

## 5 Iterative Bagging ##
1. 文章题目《Using Iterated Bagging to Debias Regressions》。看上去应该是做回归的。
2. 摘要中：Bagging可以有效降低方差，同时偏差相对不变。这篇文章的重点在于通过迭代降低bias。尤其是小树的bias较高时，单纯bagging起不到debiasing效果。
3. P12，第8章：不剪枝的CART具有最低的bias和最高的variance。因此**不剪枝的CART最适合用在Bagging上，因为Bagging只降低方差**。这个信息可以用在第一题上。
4. P12，第8章：小的树可以减少方差，但是提高偏差。在树的大小和提高的除偏能力上可能存在折中。在使用了debiasing后，小的树只比未剪枝树的精度小一点点。
5. P12，第8章：一颗“笨树”（two-terminal-tree）的计算量只有一颗完全数的1/log2(N),其中N是样本个数。
6. P12，第8章：未剪枝树的结点个数和样本个数处于同一量级。
7. P12，第8章：对小树，不debiasing，只bagging的效果不好。
8. P13，第9章：用在了二分类问题上，提供了数据集名称，结果显示错误率比完全树更低。
9. P16，结论：可以同时减少方差和偏差。
10. 看完了算法部分：Iterative Bagging的思路就是，每一次Bagging之后，计算残差，再用残差作为训练样本，再对这个样本集Bagging，并与之前的一个Bagging结果相加，再算残差……
11. 所以Iterative Bagging其实用不到sklearn的bagging。想实现的话，只能从基学习器开始，这里还是使用决策树吧。原文里也用过。bagging的过程需要自己实现，并且还要利用包外样本计算残差。
12. 完全决策树确实是这样：对于训练集，score基本上0.99，但是对于包外数据，只有0.2多。
13. Iterative Bagging比起Bagging来说，不仅仅是简单的Bagging的迭代。预测也是有成本的，在每一个基学习器训练完成后，都要用它对包外数据进行预测，用来计算最后的残差，以更新训练样本的y值。而在每一个阶段（即每一个bagging）训练完成后，都要对训练样本进行预测，预测结果用于判断是否停止迭代。
14. 初步试验结果表明，最起码比决策树是要更加准确的。数的深度为12时，一棵树score为0.39，而Iterative Bagging的score为0.63。数的深度为`None`（即未剪枝树）时，一棵树score为0.27，而Iterative Bagging的score为0.67。
15. 但是与一棵树的比较意义不大，还是要看与一次bagging比较的结果。目前感觉，差别不明显，其中一次结果为：bagging的决定系数0.660，Iterative Bagging的决定系数0.665。想要更具体结果的话，估计需要做多次，取平均结果。但是这个程序跑一次太久了。
16. 文章的第4部分有对bias和variance的解释。error = noise + bias + variance。他们怎么求呢，应该做多次试验，好好看看这个：[Single estimator versus bagging: bias-variance decomposition](http://scikit-learn.org/stable/auto_examples/ensemble/plot_bias_variance.html)。这里面发现bagging比单棵树的bias大，但是variance小。bias要看平均效果。variance看分散程度。11.23日：通过仔细研究这个例子，终于明白了bias和variance的分解。
17. 文章的4.2可以看出，相比于bagging，Iterative Bagging的bias明显减小，但是variance略有增加。
18. 已经验证，在限制树的大小时，bagging明显比Iterative Bagging差。在完全树时，Iterative Bagging只好一点点。
19. 利用第16条的例子，已经验证了Iterative Bagging的降低bias的能力（树的深度为2，bias较大）。但是在bias已经很低时，Iterative Bagging的效果就不好了。这些都已经与原文的结论一致。
20. Iterative Bagging的迭代次数随着基决策树的深度增大呈现出先增加后减小的趋势。这是因为，迭代停止的条件？巧合的是，转折点的位置恰好是误差等于完全树误差的位置。这里面有什么内在联系吗？树深度小时，本身误差大，对于残差的估计不够准确，残差估计的抖动较大，因此容易满足停止条件；在数深度大时，因为基学习器本身准确度足够高，所以残差较小，也容易满足迭代停止条件。