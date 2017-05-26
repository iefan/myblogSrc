---
title: 用键盘上下键来对QTableView导航并获取其响应
date: 2016-07-13 20:36:19
tags: [QTableView, 题库]
category: 程序
---
### 起因
在题库预览时，用键盘的方向键对题库列表进行导航是很方便的操作方法，但是`QTableView`本身没有提供数据改变的方法，在查询很多材料后，在[PyQt4 Selected Item Text in QTableView delayed by one click](http://stackoverflow.com/questions/12575896/pyqt4-selected-item-text-in-qtableview-delayed-by-one-click)这个页面的回答中找到了答案。
<!-- more -->

### 解决方法
由于QTableView是对QSqlTableModel的绑定，所以在可以调用后者的`currentChanged`信号来响应键盘，并填写相应的slot函数。代码如下：

```
self.QuestionView.selectionModel().currentChanged.connect(self.viewDataCursorChanged)
```
该代码用`selectionModel()`方法找到QTableView的相关Model绑定，然后再调用后者的`currentChanged`去响应相应的函数。

再填写slot函数：
```
def viewDataCursorChanged(self, curindx, preindx):
    questionhtml = curindx.sibling(curindx.row(),0).data()
    answerhtml = curindx.sibling(curindx.row(),1).data()
    self.questionDisp.setHtmlString(questionhtml)
    self.answerDisp.setHtmlString(answerhtml)
```

问题解决。
