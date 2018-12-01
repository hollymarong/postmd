---
title: python-numpy
date: 2017-11-13 15:12:33
tags: python
---
* zeros(shape)
```
import numpy as np

a = np.zeros(2); // array([0., 0.])

a = np.zeros((2, 2)) 
// array([[0., 0.],
//       [0., 0.]])
```
* tile(A, dep)
在各个维度上重复A
如果A的维度< dep的维度，需要对A增加维度
tile(A, (a, b, c, d))
对数组A最深层的维度重复d次，数组A往外一层，重复c次,如A维度不够，增加维度，依次递推。
```
>>> a = np.array([0, 1, 2])
>>> np.tile(a, 2)
array([0, 1, 2, 0, 1, 2])

>>> a = np.array([0, 1, 2])
>>> np.tile(a, (2, 2))
array([[0, 1, 2, 0, 1, 2], [0, 1, 2, 0, 1, 2]])

>>> a = np.array([0, 1, 2])
>>> np.tile(a, (2, 2, 2))
array([[[0, 1, 2, 0, 1, 2], [0, 1, 2, 0, 1, 2]],[[0, 1, 2, 0, 1, 2], [0, 1, 2, 0, 1, 2]]])
```
