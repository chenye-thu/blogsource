title: "QCustomPlot获取被选中的数据点的位置和值"
date: 2017-07-12 17:00:00 +0800
update: 2017-07-12 17:00:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
    - 作图
    - QCustomPlot
preview: 编程记录：QCustomPlot获取被选中的数据点的位置和值。

---

> 2017-07-12 周三 天气忘了... 北京 院里

## QCustomPlot获取被选中的数据点的位置和值 ##
利用`qcustomplot->graph()->selection()`可以得到一个`QCPDataSelection`对象，利用`QCPDataSelection::dataRange()`可以得到选中数据的范围。因为我每次只选择一个数据，所以利用其`begin()`函数即可得到选中数据的位置。再利用位置得到取值即可。代码如下：

``` cpp
QCPDataSelection selection = qcustomplot->graph()->selection();
int selectedDataIndex = selection.dataRange().begin();
double dataValue = qcustomplot->plottable()->interface1D()->dataMainValue(selectedDataIndex);
```
