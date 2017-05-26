---
title: 在QTextEdit控件中寻找QImage
date: 2016-07-23 12:24:23
tags: [QTextEdit, 题库]
category: 程序
---
### 简述
为了确保QTextEdit控件中传出的图片数据能被准确解析，所以考虑到要将该图片在此控件中显示出来，这样用户只需要对图片大小位置进行鼠标调整，程序将自动生成相关的图片代码。
<!-- more -->

但是这样就需要检测在QTextEdit控件中图片是位于哪里，从
[how-to-resize-an-image-in-a-qtextedit](http://stackoverflow.com/questions/3720502/how-to-resize-an-image-in-a-qtextedit)这个网页中找到了相关的C++代码，稍做更改即可以变成python代码：
```
def resizeImage(self):
        currentBlock = self.textEditImage.textCursor().block()
        it = QTextBlock.iterator()
        it = currentBlock.begin()
        while it != currentBlock.end():            
            currentFragment = it.fragment()
            it += 1

            if currentFragment.isValid():
                if currentFragment.charFormat().isImageFormat ():
                    newImageFormat = currentFragment.charFormat().toImageFormat()
                    size = [newImageFormat.width(), newImageFormat.height()]

                    newImageFormat.setWidth(size[0]*0.8)
                    newImageFormat.setHeight(size[1]*0.8)

                    if  newImageFormat.isValid():
                        helper = self.textEditImage.textCursor()
                        helper.setPosition(currentFragment.position());
                        helper.setPosition(currentFragment.position() + currentFragment.length(), QTextCursor.KeepAnchor);
                        helper.setCharFormat(newImageFormat)
```

其中要用到`QTextFragment.charFormat().isImageFormat ()`来判断当前取得的块是否图片，若该返回值为True，则是。
经过查询，`QTextFragment.charFormat()`返回一个QTextCharFormat对象，该类继承自QTextFormat，这个父类拥有以下几个函数可判断所得到的对象是哪类：
* bool isBlockFormat (self)
* bool isCharFormat (self)
* bool isFrameFormat (self)
* bool isImageFormat (self)
* bool isListFormat (self)
* bool isTableCellFormat (self)
* bool isTableFormat (self)
* bool isValid (self)

**版权所有，欢迎转载，转载请注明出处**
