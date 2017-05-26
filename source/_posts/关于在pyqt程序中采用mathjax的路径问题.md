---
title: 关于在pyqt程序中采用mathjax的路径问题
date: 2016-06-29 12:46:33
tags: [python, mathjax, 题库]
category: 程序
---
### 添加绝对路径
由于qwebview在采用`sethtml`方法和`load`方法中加载外部js等文件有区别，前者需要绝对路径，后者相对路径是可以的。为了程序发布的方便，一般采取的方法是将所需要调用的资源文件放在当前工程目录下，在程序中采用`sethtml`方法时，需要构造绝对路径。
<!-- more -->

程序代码如下：
```
class BrowserScreen(QWebView):
    def __init__(self):
        QWebView.__init__(self)

        baseUrl = QUrl.fromLocalFile(QDir.current().absoluteFilePath("dummy.html"));

        self.resize(800, 600)
        self.show()
        sself.setHtml("""
        <script type="text/x-mathjax-config">
            MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
            </script>
            <script type="text/javascript" async src="MathJax2.6/MathJax.js?config=TeX-MML-AM_CHTML"></script>

            <p>$x+y=z \pi$</p>
            <p>$a \div b = z^2$</p>
            <p>$\\frac{2}{3}=5$</p>
            <p>$$\\frac{2}{3}=5$$</p>
            <p>Euler's identity, <mathjax>$e^{i\pi} = -1$</mathjax>, is widely considered the most beautiful theorem in mathematics.</p>
        """, baseUrl)

```

在程序中以`baseUrl = QUrl.fromLocalFile(QDir.current().absoluteFilePath("dummy.html")); ` 产生绝对当前工程目录的绝对路径，并在sethtml中的第二个参数中设置，便可以确保程序正确加载。

以上方法参考[How to get QWebKit to display image?](http://stackoverflow.com/questions/2727080/how-to-get-qwebkit-to-display-image)

### 添加.gitignore文件
为了不将mathjax添加至github中，需要在.gitignore文件中添加`/MathJax2.6/*`，这样就会忽略该目录。

### 将数学公式显示漂亮
起始时，试了很多次，qwebview总是显示宋体，查询很多资料感觉很难解决这个难题，后来在[这篇文章](http://osdir.com/ml/MathJax-Users/2012-02/msg00278.html)提示下，上面提到如果配置本地mathjax的话，最好采用`full`版本，再查询[mathjax的帮助文档](http://docs.mathjax.org/en/latest/config-files.html)，看到`The TeX-MML-AM_HTMLorMML configuration file`这一段文字，于是将程序中加载mathjax的样式改为这个配置，因为上面说:"It loads all the main MathJax components, ...."，再一测试，成功！

### 更改qwebview的字体
```
class BrowserScreen(QWebView):
    def __init__(self):
        QWebView.__init__(self)

        myset = self.settings()
        # myset.setFontSize(QWebSettings.DefaultFontSize, 20)
        myset.setFontSize(QWebSettings.MinimumFontSize, 20)
```
注意如果设置DefaultFontSize，则感觉没效果。
