title: "Qt中等待一段时间的实现"
date: 2017-07-14 17:00:00 +0800
update: 2017-07-14 17:00:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
preview: 编程记录：Qt中等待一段时间的实现。

---

> 2017-07-14 周五 天气忘了... 北京 院里

## Qt中等待一段时间的实现 ##
有时程序中可能会有等待一段时间的需求。利用`QWaitCondition`类和`QMutex`即可实现。

在需要等待的位置添加示下面代码，即可等待100ms：

``` cpp
QMutex mutex;
QWaitCondition waitCon;
mutex.lock();
waitCon.wait(&mutex, 100);//单位ms
mutex.unlock();
```

