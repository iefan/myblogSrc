---
title: 用Python打印旋转数字矩阵
date: 2017-05-25 12:35:21
tags:  [python, 旋转矩阵]
category: 程序
---
### 起始
想教孩子学一点小程序，想来想去也没什么特别好玩的，刚好想起可以从1到某一个数的平方数中所有自然数以方阵的方式表示，所以就做了个简单的程序。
<!-- more -->

### 思路
这个小程序看起来不难，只是开始还是有点找不着规律，后来想就按人的最基本想法就是了，先向右，再向下，然后向左，最后向上一直重复直到碰到有数字就开始拐弯。所以就做了这个小程序，还有需要优化的地方，先贴这儿吧。

### 程序

```
def genRotateNums(n):
    a = [[0 for i in range(n)] for j in range(n)]
    # print(a)

    num = 1
    a[0][0] = num
    row = 0
    col = 1

    flag = 1 # right

    while a[row][col] == 0:
        num += 1
        a[row][col] = num

        if flag == 1: #right 向右，列加1，检测右边是否到头和是否已经有数字。
            col += 1
            if col == n:
                col = n-1
                flag = 2 # down 向下，顺时针则第二步要向下
            elif a[row][col] != 0:
                col = col -1
                flag = 2 # down 向下，顺时针则第二步要向下

        if flag == 2: #向下
            row += 1
            if row == n:
                row = n-1
                flag = 3 #向上
            elif a[row][col] != 0:
                row = row -1
                flag = 3 #向上

        if flag == 3: #向左
            col = col -1
            if col == -1:
                col = 0
                flag = 4
            elif a[row][col] != 0:
                col += 1
                flag = 4

        if flag == 4: #向上
            row = row -1
            if row == -1:
                row = 0
                flag = 1
                # print("111==========")
            elif a[row][col] != 0:
                row += 1
                col += 1  #要向右移动一位进行检测
                flag = 1
                # print("111==========222")

    print("==============================================")
    for i in range(n):
        for j in range(n):
            print(str(a[i][j])+ "\t", end="")
        print("\n")
    print("~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~")
```

### 打印结果图

![3的平方](/img/3的平方.PNG)
![5的平方](/img/5的平方.PNG)
![6的平方](/img/6的平方.PNG)

**版权所有，欢迎转载，转载请注明出处**。
