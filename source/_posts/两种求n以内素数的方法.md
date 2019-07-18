---
title: 两种求n以内素数的方法
date: 2019-07-09 10:41:31
tags:
  - 算法
---
求n以内的素数有两种常用方法
* 方法一： 对任意数n>=2，用2到根号n范围内的数除以n，如果都无法整除，说明n是素数
* 方法二： 用数组元素标记n是否为素数，对于任意n>=2，n在要求的最大值范围内的倍数都是素数，以此做循环标记

方法二又称为筛法，在n较大时，方法二明显节省时间

```cpp
#include <iostream>
#include <cstdio>
#include <ctime>
#include <cmath>
#define MAX_N 10000
clock_t begin, end;
double duration;
double douration2;
char isPrime[MAX_N + 1]; //char类型节省空间
int main() {
    begin = clock();
    for (int i = 2; i <= MAX_N; i++) {
        isPrime[i] = 1;
    }
    for(int i = 2; i<= MAX_N; i++) {
        if(isPrime[i]) {
            for(int j = 2*i; j <= MAX_N; j+=i ) {
                isPrime[j] = 0;
            }
        }
    }
    for (int i = 2; i<=MAX_N; i++) {
        if(isPrime[i])
            printf("%d\n", i);
    }
    end = clock();
    duration = static_cast<double>(end - begin)/CLOCKS_PER_SEC; //CLOCKS_PER_SEC表示计算机每秒的时钟打点数

    begin = clock();
    for (int i = 2; i <= MAX_N; i++) {
        isPrime[i] = 1;
    }
    for (int i = 2; i <= MAX_N; i++) {
        int sqrt_n = static_cast<int>(sqrt(i));
        int flag = 1;
        for(int j = 2; j<= sqrt_n; j++) {
            if(i % j == 0) {
                flag = 0;
                break;
            }
        }
        if(!flag) {
            isPrime[i] = 0;
        }
    }
    for (int i = 2; i<=MAX_N; i++) {
        if(isPrime[i])
            printf("%d\n", i);
    }
    end = clock();
    douration2 = static_cast<double>(end - begin)/CLOCKS_PER_SEC;
    printf("筛法求n(n=%d)以内素数的时间：%.18lf s\n", MAX_N, duration);
    printf("2到根号n法求n(n=%d)以内素数的时间：%.18lf s\n", MAX_N, douration2);

}

```