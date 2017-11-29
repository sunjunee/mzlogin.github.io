---
layout: post
title: python对二维数组按行/列排序
categories: [python]
description: python对二维数组按行/列排序
keywords: python, 排序
---

<h2 align = "center">python对二维数组按行/列排序</h2>

<br/>
通常在python里，我们对list或者array进行排序，都用sort函数，但是它只能实现一行或者一列的排序，其他行/列不会随之变化。
这种情况下，就需要用到numpy里的lexsort函数，对二维数组按照行或者列进行排序。具体使用如下：

       >>> import numpy as np

       >>> a

       array([[ 2,  7,  4,  2],
       [35,  9,  1,  5],
       [22, 12,  3,  2]])

按最后一列顺序排序

       >>> a[np.lexsort(a.T)]

       array([[22, 12,  3,  2],
       [ 2,  7,  4,  2],
       [35,  9,  1,  5]])

按最后一列逆序排序

       >>> a[np.lexsort(-a.T)]

       array([[35,  9,  1,  5],
       [ 2,  7,  4,  2],
       [22, 12,  3,  2]])

按第一列顺序排序

       >>> a[np.lexsort(a[:,::-1].T)]

       array([[ 2,  7,  4,  2],
       [22, 12,  3,  2],
       [35,  9,  1,  5]])

按最后一行顺序排序

       >>> a.T[np.lexsort(a)].T

       array([[ 2,  4,  7,  2],
       [ 5,  1,  9, 35],
       [ 2,  3, 12, 22]])

按第一行顺序排序

       >>> a.T[np.lexsort(a[::-1,:])].T

       array([[ 2,  2,  4,  7],
       [ 5, 35,  1,  9],
       [ 2, 22,  3, 12]])
