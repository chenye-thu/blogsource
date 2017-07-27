title: "QCustomPlot通过代码设置数据点的选择"
date: 2017-07-27 17:00:00 +0800
update: 2017-07-27 17:00:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
    - 作图
    - QCustomPlot
preview: 编程记录：QCustomPlot通过代码设置数据点的选择。

---

> 2017-07-27 周四 阴 北京 院里

## QCustomPlot通过代码设置数据点的选择 ##
之前一篇博文讲了QCustomPlot中怎样获取被选中的数据点的位置和值:[QCustomPlot获取被选中的数据点的位置和值](./coding-dairy-20170712.html)。今天讲一下怎样通过代码改变数据点的选择。

利用`qcustomplot->graph()->setSelection()`或者`qcustomplot->plottable()->setSelection()`即可数据点的选择。不过在这之前要先新建一个`QCPDataSelection`对象作为函数的参数。

下面的例子是将选择位置由当前选择数据点左移1：

``` cpp
//新建数据点范围，由当前减1。单数据点选择范围为1
const QCPDataRange dataRange(selectedDataIndex - 1, selectedDataIndex);
//新建QCPDataSelection对象
QCPDataSelection selection(dataRange);
//设置选择：可以用graph()或者plottable()
//qcustomplot->graph()->setSelection(selection);
qcustomplot->plottable()->setSelection(selection);
//让selectedDataIndex减1
selectedDataIndex = selectedDataIndex - 1;
//重新作图即可
qcustomplot->replot();
```
