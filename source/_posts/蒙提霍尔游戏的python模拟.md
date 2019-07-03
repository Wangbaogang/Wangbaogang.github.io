---
title: 蒙提霍尔游戏的python模拟
date: 2019-07-03 19:32:37
tags:
  -  概率统计
---

基于python3对蒙提霍尔游戏进行模拟

``` python
#coding=utf-8
from random import randint

def MontyHall(doorOfSelected, changed):
    doorOfCar = randint(1, 3)
    if doorOfCar == doorOfSelected:
        if changed:
            return 0
        else:
            return 1
    else:
        if changed:
            return 1
        else: 
            return 0

def main():
    n = 10000
    win = 0
    for i in range(n):
        doorOfSelected = randint(1, 3)
        changed = 0
        win += MontyHall(doorOfSelected, changed)
    print("参赛者不改变TA的选择，中奖概率：%f" %(win/n))

    win = 0
    for i in range(n):
        doorOfSelected = randint(1, 3)
        changed = randint(0, 1)
        win += MontyHall(doorOfSelected, changed)
    print("参赛者随机改变或不改变TA的选择，中奖概率：%f" %(win/n))

    win = 0
    for i in range(n):
        doorOfSelected = randint(1, 3)
        changed = 1
        win += MontyHall(doorOfSelected, changed)
    print("参赛者改变TA的选择，中奖概率：%f" %(win/n))

main()
```
