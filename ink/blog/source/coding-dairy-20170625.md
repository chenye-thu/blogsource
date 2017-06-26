title: "Qt中Tree Widget的使用"
date: 2017-06-25 10:00:00 +0800
update: 2017-06-25 14:17:00 +0800
author: me
# cover: "-/images/sangshen.jpg"
tags:
    - 编程
    - Qt
preview: 编程记录:Qt中Tree Widget的使用。

---

> 2017-06-25 周日 晴 北京 清华

## Qt中Tree Widget的使用 ##
**Tree Widget**即`QTreeWidget`类，可以实现树状的分层列表，非常有用。为了实现帮助文档的不同章节目录功能，采用了这一控件。目录只有两级，因此实现较为简单。

### 利用`itemClicked`信号和父节点parent判断点击项 ###
想要确定**Tree Widget**中第几项被点击或选中，但是查阅文档后，并未找到**Tree Widget**带有行参数的信号。这就不好做了，看来只能利用每一项的文本内容了。

参考这篇博客的内容：[Qt QTreeWidget 树形结构实现](http://www.cnblogs.com/Romi/archive/2012/04/16/2452709.html)。

利用`itemClicked(QTreeWidgetItem *item, int column)`信号可以得到被点击的项`item`，再找到`item`的父节点`parent`，通过`parent->indexOfChild(item)`即可得到`item`在`parent`下的序号。

``` cpp
void MainWindow::on_xxx_itemClicked(QTreeWidgetItem *item, int column)
{
    QTreeWidgetItem *parent = item->parent();
    if(NULL==parent) //注意：最顶端项是没有父节点的，双击这些项时注意(陷阱)
        return;
    int col = parent->indexOfChild(item); //item在父项中的节点行号(从0开始)

    if(0==col) //Band1
    {
        //执行对应操作
    }
    if(1==col) //Band2
    {
        //执行对应操作
    }
}
```

利用`parent->text(0)`文本的不同则可以区分第一级目录。

### 让**Tree Widget**保持展开 ###
让目录一开始为展开状态，在使用时比较方便。查阅这篇博文：[Qt如何使QTreeWidget始终保持展开？](http://blog.csdn.net/can3981132/article/details/52273800)。

了解到`QTreeWidget`有两个接口可以用：`void setItemsExpandable(bool enable)`和`void expandAll()`。可以用下面两句代码使**Tree Widget**全部展开，并且不能收起。
``` cpp
ui->treeWidget->setItemsExpandable(false);
ui->treeWidget->expandAll();
```



