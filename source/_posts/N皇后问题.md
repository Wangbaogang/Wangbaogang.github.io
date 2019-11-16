---
title: N皇后问题
date: 2019-07-22 10:56:20
tags:
    - 程序设计与算法
---

```cpp
#include <iostream>
#include <cstdio>
using namespace std;
#define MAX_N 100
int QueenList[MAX_N];
int N;
void NQueen(int k);

int main() {
    cin >> N;
    NQueen(0);
    return 0;
}

int Cnt = 0;
void NQueen(int k) { // k -> row
    if(k == N) {
        cout << ++Cnt << ": ";
        for (int i = 0;i < k; i++) {
            cout << QueenList[i] + 1 << " ";
        }
        cout << endl;
        return;
    }
    for (int i = 0; i < N; i++) { // i -> column
        int j;
        for (j = 0; j < k; j++) { // j -> row
            if(QueenList[j] == i || abs(QueenList[j] - i) == abs(k - j)) { // (k, i) (j, QueenList[j])
                break;
            }
        }
        if(j == k) {
            QueenList[k] = i;
            NQueen(k+1);
        }
    }
}
```
