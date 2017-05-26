---
title: 在Mathjax中调用宏包来实现约分过程
date: 2016-07-17 11:23:36
tags: [Mathjax, 题库]
category: 程序
---
### 起因
因为要给学生讲述约分的过程，而约分时会将原来的数字划掉，那么如何在公式中显示这样的情形呢？
<!-- more -->

### 方法一
在`latex`中，可以用宏包 `\use{cancel}`，那么在 `Mathjax` 中该如何应用呢？经过查询资料，在[mathjax-basic-tutorial-and-quick-reference](http://meta.math.stackexchange.com/questions/5020/mathjax-basic-tutorial-and-quick-reference)找到相应的答案。

即首先要在准备划线的公式前引用 `\require{cancel}` 这句宏代码，然后再继续往后写，如：
```
$\require{cancel}y+\bcancel{x+y}$
```
即显示：$\require{cancel}y+\bcancel{x+y}$
对于分数则可以这样：
```
$\frac{1\cancel9}{\cancel95} = \frac15$
```
即显示：$\frac{1\cancel9}{\cancel95} = \frac15$

**这里需要注意的是要将Mathjax的引用方式中的配置写为`config=TeX-AMS-MML_SVG`的形式，即以svg方式输出。**

同时，为了显示美观，需要将svg的scale调整为180，以下代码要单独添加，不要添加至原有的`Hug.Config`中。
```
MathJax.Hub.Config({
          SVG: {
            scale: 180
          }
        });
```

### 方法二
用 `\require{enclose}`这个宏也可以：
```
$\require{enclose} \enclose{downdiagonalstrike}{x+y}\\
\enclose{horizontalstrike,updiagonalstrike}{x+y}$
```
显示为：
$\require{enclose} \enclose{downdiagonalstrike}{x+y}\\\
\enclose{horizontalstrike,updiagonalstrike}{x+y}$

**版权所有，欢迎转载，转载请注明出处**。
