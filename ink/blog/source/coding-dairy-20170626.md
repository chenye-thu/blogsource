title: "Qt中List Widget的使用"
date: 2017-06-26 10:00:00 +0800
update: 2017-06-26 11:13:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
preview: 编程记录:Qt中List Widget的使用。

---

> 2017-06-26 周一 晴 北京 院里

## Qt中List Widget的使用 ##
**List Widget**即`QListWidget`类，可以实现列表功能，很有用。为了实现材料数据库查看功能，采用了这一控件。

### 添加列表项 ###
因为使用了Qt设计师，所以可以在设计界面直接双击**List Widget**控件，并添加相应的列表项。

如果需要动态添加或删减列表内容，那就要代码实现了。使用函数`addItem()`可以添加列表项：

``` cpp
void QListWidget::addItem(QListWidgetItem *item)
```

可以这样使用：

``` cpp
listWidget->addItem(new QListWidgetItem( "列表项名称" ));
```

### 删除列表项 ###
使用函数`addItem()`可以添加列表项，其参数`row`代表列表中第几行。

``` cpp
QListWidgetItem *QListWidget::takeItem(int row)
```

### 使用`currentRowChanged`信号 ###
为了实现点击不同列表项查看不同材料内容的功能，选择使用`currentRowChanged`信号，该信号在当前选中项改变时会发出，其参数为新当前项所在的行，即其序号。利用这一参数便可以知道选中了哪一项了。

``` cpp
[signal] void QListWidget::currentRowChanged(int currentRow)
```