---
title: 分赌本问题的python模拟
date: 2019-07-04 10:25:56
tags:
  - 概率统计
---

```python
#coding=utf-8
from random import randint

def gamble(n, n1, n2): ## 需要赢的局数 赌徒1已经赢的局数 赌徒2已经赢的局数
    for i in range(2*n - n1 - n2): 
        win = randint(1, 2)
        if win == 1:
            n1 = n1 + 1
        else:
            n2 = n2 + 1
        if n1 == n:
            return 1
        if n2 == n:
            return 2

def main():
    N = 10000
    win1 = 0
    for i in range(N):
        win = gamble(3, 2, 1)
        if win == 1:
            win1 = win1 + 1
    print("三局胜制，赌徒1胜两局，赌徒2胜一局，则最终赌徒1赢的概率：%s" % (win1/N))

main()

```