title: "Qt添加快捷键的方法"
date: 2017-07-26 12:00:00 +0800
update: 2017-07-26 12:00:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
preview: 编程记录：使用用Qt的`QShortcut`类和`QKeySequence`类添加快捷键。

---

> 2017-07-26 周三 中雨转阴 北京 院里

## Qt添加快捷键的方法 ##
添加快捷键可以方便软件的使用。在Qt中，利用`QShortcut`类和`QKeySequence`类可以很容易地添加快捷键。

### 1.QKeySequence类的使用 ###
`QKeySequence`类是顾名思义，即键盘的输入流。其有三种简单用法如下，示例中这三种用法都得到了打印对应的“Ctrl+P”的快捷键。其中，用法1和用法3使用了Qt自带的枚举类型，具体可查阅Qt文档；而方法2是以字符串形式直接写按键名。经实际使用，还是方法1和方法3有文档可查，好用一些。

``` cpp
QKeySequence(QKeySequence::Print);//用法1
QKeySequence(tr("Ctrl+P"));//用法2
QKeySequence(Qt::CTRL + Qt::Key_P);//用法3
```

### 2.QShortcut类的使用 ###
`QShortcut`类是注册快捷键的类，用法也非常简单。

首先新建一个快捷键对象。例如，添加`ctrl+left`的快捷键如下所示：
``` cpp
QShortcut *shortcutCtrlLeft = new QShortcut(QKeySequence(Qt::CTRL + Qt::Key_Left), this);
```

然后，就是我们熟悉的Qt的信号与槽机制了。每次快捷键触发，都会发射信号`activated()`，利用这个信号，连接快捷键到相应的槽函数即可：

``` cpp
connect(shortcutCtrlLeft, SIGNAL(activated()), this, SLOT(on_CtrlLeft_activated()));
```