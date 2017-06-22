title: "Qt类声明中Q_OBJECT的作用与报错解决"
date: 2017-06-22 10:00:00 +0800
update: 2017-06-22 11:13:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
    - 作图
    - QCustomPlot
preview: 编程记录:新建作图类继承自QCUstomPlot类，自带单坐标缩放功能；Qt类声明中Q_OBJECT的作用；加入Q_OBJECT后报错的解决。

---

> 2017-06-22 周五 大雨 北京 院里

## 新建作图类，继承自QCUstomPlot类 ##
因为需要同时作8张图，都要单坐标缩放的功能，因此想干脆新建一个类，继承自`QCUstomPlot`，把需要的功能都加上。类名取为`QCUstomPlotPlus`，最终成功版类代码如下：

``` cpp
//声明。explicit是为了禁止隐式转换。
class QCustomPlotPlus : public QCustomPlot
{
    Q_OBJECT  //重要！
public:
    explicit QCustomPlotPlus(QWidget *parent = 0);
private slots:
    void mousePressFun();
    void mouseWheelFun();
};
//函数定义
//构造函数：继承自QCustomPlot，所以用QCustomPlot(parent)。
QCustomPlotPlus::QCustomPlotPlus(QWidget *parent) :
  QCustomPlot(parent)
{
  //设置单坐标方向缩放和拖拽
  bool t1 = connect(this, SIGNAL(mousePress(QMouseEvent*)), SLOT(mousePressFun()));
  bool t2 = connect(this, SIGNAL(mouseWheel(QWheelEvent*)), SLOT(mouseWheelFun()));
}
//鼠标点击槽函数
void QCustomPlotPlus::mousePressFun()
{
  // if an axis is selected, only allow the direction of that axis to be dragged
  // if no axis is selected, both directions may be dragged
  if (xAxis->selectedParts().testFlag(QCPAxis::spAxis))
    axisRect()->setRangeDrag(xAxis->orientation());
  else if (yAxis->selectedParts().testFlag(QCPAxis::spAxis))
    axisRect()->setRangeDrag(yAxis->orientation());
  else
    axisRect()->setRangeDrag(Qt::Horizontal|Qt::Vertical);
}
//鼠标滚轮槽函数
void QCustomPlotPlus::mouseWheelFun()
{
  // if an axis is selected, only allow the direction of that axis to be zoomed
  // if no axis is selected, both directions may be zoomed
  if (xAxis->selectedParts().testFlag(QCPAxis::spAxis))
    axisRect()->setRangeZoom(xAxis->orientation());
  else if (yAxis->selectedParts().testFlag(QCPAxis::spAxis))
    axisRect()->setRangeZoom(yAxis->orientation());
  else
    axisRect()->setRangeZoom(Qt::Horizontal|Qt::Vertical);
}
```
## Qt类声明中Q_OBJECT的作用 ##
一开始在类声明时，我没有加入`Q_OBJECT`这一句代码，结果信号和槽函数的连接总是不成功，无法实现单坐标缩放。

经过对比自动生成的类，我发现每一个类声明都有`Q_OBJECT`，因此百度了一下。原来只有加入`Q_OBJECT`之后，才能正常使用信号和槽机制，囧TT……

## 加入Q_OBJECT后报错的解决 ##
在类声明中加入`Q_OBJECT`后，程序构建报错：

``` cpp
undefined reference to `vtable for QCustomPlotPlus'
```

没有办法，只好再百度，查到了这篇博文：[Qt 出现“undefined reference to `vtable for”原因总结"](http://blog.csdn.net/chenlong12580/article/details/7431104)。

在这篇文章里找到了解决方法：

> 问题:某一个类中如果加入Q_OBJECT后, 则link时提示:undefined reference to vtable for "xxx::xxx".删掉它则没有任何问题.
> 
> 解决:尝试(1):把所有的obj文件和uic文件删除,重新编译.仍然失败.去trolltech的 mail lists找到原因: 因为qmake生成Makefile的时候,这个类的头文件中并没有Q_OBJECT,所以在相应的Makefile里面并没有用moc xxx.h命令,最终导致链接失败.重新运行qmake,问题解决.在查找解决方法的时候,附带发现一点:
> 
> qmake 不会处理.cpp文件里的Q_OBJECT,所以,如果在.cpp文件中有它的话,也会产生undefined reference to vtable for "xxx::xxx". 这时,需要先用moc xxxx.cpp生成相应的moc文件,再包含到.cpp里面去,才能解决这个问题.

按照博文说法，我重新运行了***qmake***，成功构建，功能正确！

具体***qmake***的作用是什么我还不太懂，有待后面继续学习！