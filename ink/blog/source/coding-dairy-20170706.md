title: "Qt中虚函数的使用和添加自己的信号"
date: 2017-07-06 10:00:00 +0800
update: 2017-07-06 11:00:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
preview: Qt中虚函数的使用；Qt中添加自己的信号。

---

> 2017-07-06 周四 大雨 北京 院里

## Qt中虚函数的使用 ##
虚函数也可以说是C++面向对象编程的一大特点了。C++“封装性、继承性、多态性”中的多态性正是通过虚函数来实现的。虚函数允许子类重新定义成员函数，实现“一个接口，多种方法”。

今天想实现在某个`QWidget`界面关闭时做某种处理，但是Qt的`QWidget`貌似没有类似的`closed()`信号。因此通过虚函数`closeEvent(QCloseEvent *event)`来实现。



1. 声明虚函数：
``` cpp
virtual void closeEvent(QCloseEvent *event);
```

2. 重新定义函数，添加你需要的功能，最后再加入原函数功能。
``` cpp
void Widget::closeEvent(QCloseEvent *event)
{
    //下面添加你的代码

    //再加入原函数的功能
    QWidget::closeEvent(event);
}
```

## Qt中添加自己的信号 ##
有时候Qt自带的信号可能不够用，继续上面的例子，继续上面的例子，如果说希望在某个`QWidget`界面关闭时发出一个信号，那么就需要自己添加了。



1. 首先声明信号：
``` cpp
signals:
    void formClosed();
```

2. 然后在重新定义的虚函数里添加代码，发出信号即可：
``` cpp
void Widget::closeEvent(QCloseEvent *event)
{
    //下面添加你的代码
	emit(formClosed());
    //再加入原函数的功能
    QWidget::closeEvent(event);
}
```

这样就可以连接该信号到需要的槽函数了。