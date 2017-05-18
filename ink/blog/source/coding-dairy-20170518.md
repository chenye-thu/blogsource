title: "coding记录-20170518"
date: 2017-05-18 10:40:00 +0800
update: 2017-05-18 22:40:00 +0800
author: me
# cover: "-/images/xxx.jpg"
tags:
    - 编程
    - Qt
    - PanEngine
preview: 编程记录:在Qt中使用PanEngine记录（续）。

---

> 2017-05-18 周四 晴 北京 院里

## 在Qt中使用PanEngine记录
续之前一篇文章：[coding记录-20170516](./coding-dairy-20170516.html "coding记录-20170516")

仍然想继续研究一下为什么PanEngine用在Qt里不行。因为毕竟编译是通过的，而且可以运行，只不过是在执行具体计算的例子时出错，程序崩溃。（在执行到`PanEngine`类内的虚函数时出错）

1. 百度查到一篇博文：[Qt调用DLL动态库接口函数程序崩掉](http://blog.csdn.net/jackzhaoyuxiang/article/details/41550073)。说到可能与`_stdcall`有关。简单试了一下，貌似不太行。
2. 查到有人说是编译器的问题，但是之前在VS里应该用的不是mingw了，还是不行啊，回头再试一下？又试了一下，还是不行。
3. 又查到一篇博文：[Qt中隐式调用VS建立的dll动态库](http://blog.csdn.net/fantasy999999999/article/details/40979581)。于是使用VS2010编译器的Qt5.5.1，结果还是不行。
4. 又安装了**Qt5.8.0**，这已经是我查到的最新版本了，还是同样的问题，跪了……

***我尽力了，真的尽力了，真的不行啊……TT***
