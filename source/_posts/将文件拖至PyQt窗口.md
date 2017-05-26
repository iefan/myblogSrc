---
title: 将文件拖至PyQt窗口
date: 2016-07-10 08:08:44
tags: [PyQt, 文件拖放, 题库]
category: 程序
---
### 概述
在题库开发过程中，如果要将题目或答案中的图片添加进来，单纯依靠手工填写代码，并不是一件十分友好的事情，尽管灵活，但是对于非专业人员操作而言，不是十分便利，最简单的方法是直接将选中的图片拖放至题目或答案的文本框中，然后程序自动进行转换并存储图片。
<!-- more -->

### 实现
为了实现这个功能，需要创建一个继承自`QTextEdit`的类，并将该类中的`dragEnterEvent`、`dragMoveEvent`、`dropEvent`三个方法重新实现一下，自定义的文本框代码如下所示：
```
class DragImgTextEdit(QTextEdit):
    def __init__(self, type, parent=None):
        super(DragImgTextEdit, self).__init__(parent)
        self.setAcceptDrops(True)        

    def dragEnterEvent(self, event):
        if event.mimeData().hasUrls:
            event.accept()
        else:
            event.ignore()

    def dragMoveEvent(self, event):
        if event.mimeData().hasUrls:
            event.setDropAction(Qt.CopyAction)
            event.accept()
        else:
            event.ignore()

    def dropEvent(self, event):
        if event.mimeData().hasUrls:
            event.setDropAction(Qt.CopyAction)
            event.accept()
            links = []
            for url in event.mimeData().urls():
                links.append(str(url.toLocalFile()))
            self.emit(SIGNAL("dropped"), links)
        else:
            event.ignore()
```
然后再需要调用该类的主程序中添加：
```
self.questionEditor = DragImgTextEdit(self)
self.connect(self.questionEditor, SIGNAL("dropped"), self.pictureDropped)
```
即该控件响应上述继承类中的信号`dropped`，然后将其与自定义函数`self.pictureDropped`相联接，这个地方又充分利用了`PyQt`的信号插槽机制。
自定义函数如下：
```
def pictureDropped(self, l):
    for filename in l:
        if os.path.exists(filename):
            newImgName = strftime("%Y-%m-%d-%H-%M-%S", gmtime()) + filename[-4:]
            newImgAllPath = self.curdir + QDir.separator() + "images" + QDir.separator() + newImgName
            shutil.copyfile(filename, newImgAllPath) ##复制文件

            tmpstr = self.questionEditor.toPlainText()
            tmpstr += '''<img src="images/''' + newImgName + '''" alt="Smiley face" width="100" height="100" align="right"> '''
            self.questionEditor.setPlainText(tmpstr)
```
至此，文件拖动目标实现。

**版权所有，欢迎转载，转载请注明出处**。
