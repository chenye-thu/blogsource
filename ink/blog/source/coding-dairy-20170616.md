title: "Qt中qDebug()函数的使用"
date: 2017-06-16 10:00:00 +0800
update: 2017-06-16 11:40:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
preview: 编程记录:qDebug()函数的使用。

---

> 2017-06-16 周五 晴 北京 院里

## qDebug()函数的使用
1. `qDebug()`是Qt用于调试的函数，可以将所需的信息打印到QtCreator的console中，非常有用。
2. `qDebug()`函数有两种使用方式，一种与C语言的`printf()`类似：
``` cpp
qDebug("Items in list: %d", myList.size());
```

3. 另外一种用法则与C++的`cout`类似，需要`include<QtDebug>`,这一种更为方便：
``` cpp
qDebug() << "Brush:" << myQBrush << "Other value:" << i;
```

4. 与`qDebug()`类似的函数还有：`qWarning()`, `qCritical()`, `qFatal()`, `qInstallMsgHandler()`等，具体可以查阅Qt文档。
