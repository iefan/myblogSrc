---
title: 关于python3中自定义包的导入问题
date: 2016-07-18 18:25:58
tags: [python3, 导入问题, init目录, 题库]
category: 程序
---
### 起因
因为要计划将题库中的`latex`公式转换为`mathml`，经查询后，`python`中的`latex2mathml`包似乎比较有用，但是在用`pip`安装后，导入时提示出错。
<!-- more -->

```
In [1]: import latex2mathml
---------------------------------------------------------------------------
ImportError                               Traceback (most recent call last)
<ipython-input-1-63539f436375> in <module>()
----> 1 import latex2mathml

c:\python34\lib\site-packages\latex2mathml\__init__.py in <module>()
      1 #!/usr/bin/python
      2
----> 3 from aggregator import aggregate
      4 from converter import convert
      5 from element import Element

ImportError: No module named 'aggregator'
```
### 解决问题
根据提示，却无法查询到`python`中有提示中的包，于是打开安装目录，发现提示的导入文件竟然就在包安装目录下，但是为什么在`__init__`中却无法找到呢？
在[init-py-cant-find-local-modules](http://stackoverflow.com/questions/34753206/init-py-cant-find-local-modules)中找到了答案，原来在`python3`的自建包中，`__init__`文件中需要导入包自身拥有的文件时，要在该文件名前加`.`，无奈，只能根据提示一步步修改，将几个文件的导入均修改后，导入成功。

根据文章介绍：
```
Put in the following codes in the __init__.py inside the Animals directory.

Python 3.x :

from .Mammals import Mammals
from .Birds import Birds

On 2.x:

from __future__ import absolute_import
from .Mammals import Mammals
from .Birds import Birds
```

### 应用中的问题
在按照示例时，有以下两个问题
* `xrange`的问题
由于题库开发采用的是 `python3`，所以没有 `xrange`函数，而该包中用的是这个函数，这就需根据提示出错的问题中重新定义 `xrange` 函数：
```
import sys

if sys.version_info >= (3, 0):
    def xrange(*args, **kwargs):
        return iter(range(*args, **kwargs))
```

由于本用的就是 `python3`，所以就只需要重新定义就可。

* 该包导出的是 `mathml`，为了能在粘帖至word中时，能自动识别公式，则需要将这个包转换出来的结果用字符串替换一下即可：
```
import latex2mathml
latex_input = "x"
mathml_output = latex2mathml.convert(latex_input)
a = mathml_output.replace("<math>", '<math xmlns="http://www.w3.org/1998/Math/MathML">')

In [9]: print(a)
<math xmlns="http://www.w3.org/1998/Math/MathML">
    <mrow>
        <mi>x</mi>
    </mrow>
</math>
```
这样就可以了。
