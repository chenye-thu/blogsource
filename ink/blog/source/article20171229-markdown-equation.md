title: "Markdown中加入公式"
date: 2017-12-29 10:00:00 +0800
update: 2017-12-29 16:00:00 +0800
author: me
# cover: "-/images/scikit-learn-example01.png"
tags:
    - web
preview: Markdown中加入公式。

---
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

> 2017-12-29 周五 阴 北京 清华大学

## 1 加入公式方法 ##
参考[：Markdown中插入数学公式的方法](http://blog.csdn.net/xiahouzuoxin/article/details/26478179)。

使用[MathJax](https://www.mathjax.org/)的方法很简单，在Markdown文件中添加以下代码：

```html
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
```

然后可以使用下面两种方法，分别添加居中的公式和段内的公式：

### 1.1 居中公式 ###
```latex
$$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$$
```
得到居中的公式：
$$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$$

### 1.2 段内公式 ###
```latex
\\(x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}\\)
```
得到段内公式：\\(x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}\\)