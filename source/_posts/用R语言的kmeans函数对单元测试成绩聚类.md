---
title: 用R语言的kmeans函数对单元测试成绩聚类
date: 2016-06-12 17:37:53
tags: 聚类
category: 教学研究
---
1、在ubuntu下安装：采用命令行 `sudo apt-get R-base`，具体可参见官方网站；在windows下有二进制安装包可下载。
2、在R环境中用 `source()` 加载运行r脚本时，在windows下脚本编码需要用gbk，在linux下，用utf8，否则汉字编码在运行中会出现错误提示。
3、用 `kmeans` 函数对数据集分别聚类，将结果提取并保存，而后用柱状图绘制单元测试成绩结果。
<!-- more -->
程序如下：
``` python:n
grade = read.table("grade1to5.dat", header=TRUE)
g_l = length(names(grade))
histdata = matrix(0, 6, g_l)

for(indx in 1:5) {
  a = kmeans(grade[indx],6)
  bb = cbind(a$centers, a$size)
  cc = as.data.frame(bb)   #必须转换为data.frame，否则不能用order排序
  dd = cc[order(cc$unit),] #此时以cc第一列进行排序，第一列名称为unit*
  histdata[,indx] = dd[,2]
  print(dd)
}
print(histdata)
namearg <- c('差','较差','中下','中等','较好','好')
legendtxt <- c('1单元', '2单元','3单元','4单元','5单元')
barplot(t(histdata), beside=TRUE, names.arg=namearg, legend.text=legendtxt)
```
需要注意的是，`kmeans` 方法并非每次聚类结果都一样，对于分类而言，这种情况并不是最优，只能起到一定的参考作用。

**版权所有，欢迎转载，转载请注明出处**。
