title: "QCustomPlot实现坐标缩放的控制"
date: 2017-07-11 17:00:00 +0800
update: 2017-07-11 17:00:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
    - 作图
    - QCustomPlot
preview: 编程记录：QCustomplot实现坐标缩放的控制：y坐标最小值保持为0；坐标缩小范围的控制。

---

> 2017-07-11 周二 天气忘了... 北京 院里

## QCustomPlot实现坐标缩放的控制 ##
最近都在用`QCustomPlot`作图，仅仅依靠所给的实例已经不太满足需求了，于是没事就查查文档。

### 1.QCPAxis类的rangeChanged()信号 ###
之前一直有的需求是：在图的缩放时，让y坐标的最小值一直保持在0，而x坐标上不要无限缩小。但是因为找不到接口，一直没能实现。不过功夫不负有心人，没事查文档的时候，让我找到了方法：`QCPAxis`类（即坐标类）的`rangeChanged()`信号，会在坐标范围变化时发出，其有两种参数形式，声明如下：

``` cpp
//参数只有新的坐标范围
void rangeChanged (const QCPRange &newRange)；
//参数有新的和旧的坐标范围
void rangeChanged (const QCPRange &newRange, const QCPRange &oldRange)；
```

### 2.控制y坐标最小值保持在0 ###
首先添加坐标控制的槽函数,让y坐标的最小值一直保持在0：

``` cpp
void yAxisRange(QCPRange newRange)
{
    qcustomplot->yAxis->setRangeLower(0);
}
```

然后，连接y坐标的信号到槽函数即可：

``` cpp
connect(qcustomplot->yAxis, SIGNAL(rangeChanged(QCPRange)), this, SLOT(yAxisRange(QCPRange)));
```

### 3.控制x坐标的上下界 ###
首先添加坐标控制的槽函数,为x坐标设置上下界,如0~10000：

``` cpp
void xAxisRange(QCPRange newRange)
{
    qcustomplot->xAxis->setRange(newRange.bounded(0, 10000));
}
```

然后，连接x坐标的信号到槽函数即可：
``` cpp
connect(qcustomplot->xAxis, SIGNAL(rangeChanged(QCPRange)), this, SLOT(xAxisRange(QCPRange)));
```