title: "Qt中Progress Bar控件的使用"
date: 2017-07-02 20:20:00 +0800
update: 2017-07-02 21:00:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
preview: 编程记录:Qt中Progress Bar控件的使用。

---

> 2017-07-02 周日 阴 北京 院里

## Qt中Progress Bar控件的使用 ##
**Progress Bar**是用来显示进度的控件，使用非常简单，掌握`setRange()`和`setValue()`两个函数即可。

其中，`setRange()`函数用来设置**Progress Bar**取值的区间。例如，下面代码即设定最小为0，最大为100，那么取值50就是进度达到一半了。

``` cpp
ui->progressBar->setRange(0, 100);
```

另一个函数`setValue()`用法如下，在进度达到一半时，可以设置**Progress Bar**取值为50。

``` cpp
ui->progressBar->setValue(50);
```