---
title: python中采用markdown书写mathjax的环境配置
date: 2016-06-28 12:43:40
tags: [python, markdown, mathjax, 题库]
category: 程序
---
### 关于数学题库开发的想法
对每年的广东育苗杯试题想做一个集合，以便于查询起来方便快捷，同时基于对本校历年各年级试卷的保存考虑，打算开发一个数学题库。暂时有以下几个想法：
<!-- more -->

* 程序采用python3.4 及 PyQt4 来开发，后台的增删改要采用`markdown`书写，公式用`mathjax`来书写。
* 程序的数据库暂时用`sqlite3`，进一步考虑用 `mysql` 等。
* 试题的属性应分类别、年级、年份、题型、难度等。
* 程序未来可导出为`pdf`文档，进一步在能够将公式转为图片的情况下，导出为`word`文档。
暂时就想到这些。

### 配置markdown
在python3.4环境中，安装markdown模块：
```
pip install markdown
```
测试：
```
import markdown
In [1]: a = markdown.markdown("hello **world**")
In [2]: a
Out[2]: '<p>hello <strong>world</strong></p>'
```
可以看出`markdown`安装成功。

### 配置python-markdown-mathjax
下载[`python-markdown-mathjax`](https://github.com/mayoff/python-markdown-mathjax)，将其更名为mathjax.py，复制至`C:\Python34\Lib\site-packages\markdown\extensions`目录中。

测试：
```
import markdown
try: import mdx_mathjax
except: pass
mdProcessor = markdown.Markdown(extensions=['mathjax'])
myHtmlFragment = mdProcessor.convert(r"Euler's identity, $e^{i\pi} = -1$, is widely considered the most beautiful theorem in mathematics.")

输出：
<p>Euler's identity, <mathjax>$e^{i\pi} = -1$</mathjax>, is widely considered th
e most beautiful theorem in mathematics.</p>
```

### 建立html文件
将上述输出的代码粘帖在以下格式中的 `body` 部分，如下代码：
```
<!DOCTYPE html>
<html>
<head>
<title>MathJax TeX Test Page</title>
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}});
</script>
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS_CHTML">
</script>
</head>
<body>

<p>Euler's identity, <mathjax>$e^{i\pi} = -1$</mathjax>, is widely considered the most beautiful theorem in

mathematics.</p>

</body>
</html>
```
另存为html文件，浏览器打开即可。
