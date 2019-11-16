---
title: Saving James Bond - Hard Version
date: 2019-08-02 16:49:48
tags:
---

```c++
#include <iostream>
#include <cstdio>
#include <cmath>
using namespace std;
#define INFINITY_D 65535
#define ISLAND_DIAMETER 15.0
#define SHORE_POSITION 50.0
#define MAX_N 101

typedef struct Croc* Position;
struct Croc {
    double x;
    double y;
};
Position CreatCroc(double x, double y) {
    Position P = new Croc();
    P->x = x;
    P->y = y;
    return P;
}
double GetDistance(Position P1, Position P2) {
    double d1 = pow(P1->x - P2->x, 2);
    double d2 = pow(P1->y - P2->y, 2);
    return sqrt(d1 + d2);
}
double FirstJump(Position P) {
    int d = GetDistance(P, CreatCroc(0, 0));
    return d - ISLAND_DIAMETER/2;
}
bool AbleJumpFromIsland(Position P, double maxJump) {
    return FirstJump(P) <= maxJump;
}
bool AbleJumpShore(Position P, double maxJump) {
    double x = fabs(P->x);
    double y = fabs(P->y);
    double d = SHORE_POSITION - (x > y ? x : y);
    return  d <= maxJump;
}
bool EffectiveCroc(Position P) {
    if(fabs(P->x) >= SHORE_POSITION || fabs(P->y) >= SHORE_POSITION)
        return false;
    if(FirstJump(P) <= 0)
        return false;
    return true;
}

Position Crocs[MAX_N];
int dist[MAX_N][MAX_N];
int path[MAX_N][MAX_N];
bool shore[MAX_N];

void Print(int v) {
    printf("%.0lf %.0lf\n", Crocs[v]->x, Crocs[v]->y);
}
void PrintData(int v1, int v2) {
    if(v1 == v2)
        return;
    int mid = path[v1][v2];
    if(path[v1][mid] != mid)
        PrintData(v1, mid);
    Print(mid);
    if(path[mid][v2] != v2)
        PrintData(mid, v2);
}

int main() {
    int n, jump;
    double x, y;
    cin >> n >> jump;
    for (int i = 0; i < n; i++) {
        shore[i] = -1;
        for (int j = 0; j < n; j++) {
            path[i][j] = -1;
            if(i == j)
                dist[i][j] = 0;
            else
                dist[i][j] = INFINITY_D;
        }
    }
    for (int i = 0; i < n; i++) {//初始化鳄鱼
        cin >> x >> y;
        Crocs[i] = CreatCroc(x, y);
        if(!EffectiveCroc(Crocs[i])) {
            Crocs[i]->x = (double)INFINITY_D;
            Crocs[i]->y = (double)INFINITY_D;
        }
        for (int j = 0; j < i; j++) {
            if(GetDistance(Crocs[i], Crocs[j]) < jump) {
                dist[i][j] = 1;
                dist[j][i] = 1;
                path[i][j] = j;
                path[j][i] = i;
            }
        }
    }
    for(int k = 0; k < n; k++) {
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if(dist[i][k] + dist[k][j] < dist[i][j]) {
                    dist[i][j] = dist[i][k] + dist[k][j];
                    path[i][j] = k;
                }
            }
        }
    }
    int min_step = INFINITY_D, min_first = -1, min_end = -1;
    if(AbleJumpShore(CreatCroc(ISLAND_DIAMETER/2, 0), jump)) {
        min_step = -1;
    } else {
        for (int i = 0; i < n; i++) {
            if(AbleJumpFromIsland(Crocs[i], jump)) {
                for (int j = 0; j < n; j++) {
                    if(AbleJumpShore(Crocs[j], jump)) {
                        if(dist[i][j] < min_step) {
                            min_step = dist[i][j];
                            min_first = i;
                            min_end = j;
                        } else if(dist[i][j] == min_step && min_step < INFINITY_D ) {
                            if(FirstJump(Crocs[i]) < FirstJump(Crocs[min_first])) {
                                min_step = dist[i][j];
                                min_first = i;
                                min_end = j;
                            }
                        }
                    }
                }
            }
        }
    }
    if(min_step == -1) {
        cout << 1 << endl;
    } else if(min_step < INFINITY_D) {
        cout << min_step+2 << endl;
        Print(min_first);
        PrintData(min_first, min_end);
        if(min_first != min_end)
            Print(min_end);
    } else {
        cout << 0 << endl;
    }
    
    return 0;
}
```c++
