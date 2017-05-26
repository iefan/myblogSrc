---
title: PyQt窗体间的自定义信号插槽通信
date: 2016-07-07 12:07:36
tags: [PyQt, 信号, 插槽, signal, slot]
category: 程序
---
### 问题
题库程序中，由于程序中题目列表的窗体与添加题目的窗体是分离的，起始目的是添加题目时需要实时预览，而这样分离的结果就使得当点击前者窗体中的“新增”按钮时需要调用后者的窗体显示。这就需要用到`PyQt`中的`signal`和`slot`机制。
<!-- more -->

### 解决问题
经过查询，在[PySide/PyQt Tutorial: Creating Your Own Signals and Slots](http://pythoncentral.io/pysidepyqt-tutorial-creating-your-own-signals-and-slots/)页面找到解决方案，在程序中采取如下方法处理：
```
class QuestionDlg(QDialog):
    # 自定义信号
    jumpNewQuestion = pyqtSignal()
    def __init__(self, parent=None, db="", curuser=""):
        super(QuestionDlg,self).__init__(parent)
```
要注意的是，这个自定义的信号要放在`__init__`的前面。然后在需要调用的按钮响应中将这个信号emit。
```
    def newQuestion(self):
        self.jumpNewQuestion.emit()
```
在需要响应信号的主窗体中将以该类创建的对象中的这个自定义信号与相应的函数进行挂接。
```
widget = QuestionDlg(db=self.db, curuser=self.curuser)
widget.jumpNewQuestion.connect(self.questionModify)
```
在需要连接的函数前面用`@pyqtSlot()`进行标记。
```
@pyqtSlot()
def questionModify(self):
    ......
```
如此即可完成两个窗体间的通信。

### 窗体间的交换数据
如果要用到交换数据，也同样要如上所示宣告自定义信号，只是需要注意的是要带参数：
```
jumpModifyQuestion = pyqtSignal(str, str)
```
在响应该信号时将所调用的参数也包含在`emit`中。
```
self.jumpModifyQuestion.emit(questionstr, answerstr)
```
同时，要在被调用的窗体中，用`@pyqtSlot(str, str)`将所需要调用的函数声明即可，需要注意的是该被调用函数中需要含有与声明中相同数量和类型的变量。

### 小结
两个窗体间的通信本身是非常繁杂的，但是`Qt`提供的信号插槽机制巧妙地解决了这个问题，将顶层的哲学概念“发送-接受”用很优美的代码实现了，程序的简单实现不但增强了程序的健壮性，也增加了程序的可读性。

**版权所有，欢迎转载，转载请注明出处**
