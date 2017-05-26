---
title: 将mathml导出至msword的思索
date: 2016-07-19 20:02:19
tags: [mathml, 题库, msword]
category: 程序
---
### 思索一
将题库导出至word文档是一个很不错的想法，对于文字的导出是比较容易的，可以用pywin32中的相关功能，并对word进行宏录制后，一一对应进行填写是可以实现自动化插入的。但是数学题目相对于其它题目而言至少有两个比较特别的地方，第一当然是公式，第二则是有较多的图片。后者是容易的，可是对于公式的转换则比较困难。
<!-- more -->

最开始第一个想法就是自然地采用 mathjax 的相关功能，因为 mathjax 在生成公式后，可以将当前 DOM 中的公式转换成相应的 mathml 语言，只要将该段代码复制粘贴至 word 中，则自动可转换为公式，这里指的 word 是 MSWord2007 以后的版本（含2007）。从 mathjax 中得到 mathml 可以用以下代码得到，下面代码是用 javascript 函数实现的。
```
var jax = MathJax.Hub.getAllJax();  
    alert(jax.length)
    for (var i = 0; i < jax.length; i++) {
        alert(jax[i]);
        alert(jax[i].root.toMathML(""))                    
    }
```
剩下的就是要利用 Qt 中的信号插槽功能来将得到的该 mathml 语句导出至 python 中，然后进行操作，这种转换是比较麻烦的（尚需进一步研究）。

### 思索二
第二个思路是直接将录入的题目中的公式用 python 转换为 mathml ,这就用到一个 python 包，即 latex2mathml ，这个包是针对 python2 系列的版本，对于 python3 来说，需要做一些修改，这部分已完成，在上篇博客中有述。

步骤是这样的：
* 查找题目中是否含有 `$` ，如果有则要检查是否奇数，对于公式而言，该符号应该是成对出现，这需要用 re 模块中的 split 函数。
* 将该字符串以 `$` 分割为字符串数组，位于偶数位置的则是公式字符串，可将该字符串传至 latex2mathml ，转换至 mathml 。
* 打开 word 文档，将该字符串数组分步写入，当遇到偶数位时，需要用`格式化粘贴`的方式，只有这样才可以将 mathml 转换至 word 中的公式，这种方法很笨，但是目前还没有找到合适的其它方法。操作剪切板需要用到 python 中的 pyperclip 包。
