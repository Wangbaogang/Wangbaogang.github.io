---
title: 蒙特卡罗方法算圆周率的python实现
date: 2019-07-03 19:45:21
tags:
  -  概率统计
---

``` python

#coding=utf-8
from random import uniform

def main():
    n = 100000
    k = 0
    for i in range(n):
        x = uniform(-1, 1)
        y = uniform(-1, 1)

        if x**2 + y**2 <= 1.0:
            k = k+1
    pie = 4 * (k / n)
    print("PIE的近似值：%s" % pie)

main()

```
    