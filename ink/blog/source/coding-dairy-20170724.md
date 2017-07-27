title: "QCustomPlot通过代码实现坐标的放缩和移动"
date: 2017-07-24 17:00:00 +0800
update: 2017-07-24 17:00:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
    - 作图
    - QCustomPlot
preview: 编程记录：QCustomPlot通过代码实现坐标的放缩和移动。

---

> 2017-07-24 周一 阴 北京 院里

## QCustomPlot通过代码实现坐标的放缩和移动 ##
QCustomPlot可以通过鼠标滚轮和拖拽实现坐标的缩放和移动。今天讲一下怎样通过代码实现这一功能。

### 1.坐标缩放 ###

利用`QCPAxis`类（即坐标类）的`scaleRange()`函数即可实现很方便的实现坐标缩放。

下面的例子是将x坐标范围放大了1.25倍（图形被缩小了）：

``` cpp
//坐标范围放大1.25倍
qcustomplot->xAxis->scaleRange(1.25);
//重新作图即可
qcustomplot->replot();
```

### 2.坐标移动 ###

利用`QCPAxis`类（即坐标类）的`moveRange()`函数即可实现很方便的实现坐标移动。

下面的例子是将x坐标右移十分之一（图形左移了）：

``` cpp
//获取当前坐标范围
QCPRange curXRange = qcustomplot->xAxis->range();
//移动值为当前范围的十分之一
double diff = 0.1 * (curXRange.upper - curXRange.lower);
//移动坐标
qcustomplot->xAxis->moveRange(diff);
//重新作图即可
qcustomplot->replot();
```