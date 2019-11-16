---
title: 数据结构题：Tree Traversals Again
date: 2019-07-10 18:39:24
tags: 
    - 算法
    - c/cpp
---

An inorder binary tree traversal can be implemented in a non-recursive way with a stack. For example, suppose that when a 6-node binary tree (with the keys numbered from 1 to 6) is traversed, the stack operations are: push(1); push(2); push(3); pop(); pop(); push(4); pop(); pop(); push(5); push(6); pop(); pop(). Then a unique binary tree (shown in Figure 1) can be generated from this sequence of operations. Your task is to give the postorder traversal sequence of this tree.

![Figure 1](/images/tree-travisals-again.jpg)

Input Specification:
Each input file contains one test case. For each case, the first line contains a positive integer N (≤30) which is the total number of nodes in a tree (and hence the nodes are numbered from 1 to N). Then 2N lines follow, each describes a stack operation in the format: "Push X" where X is the index of the node being pushed onto the stack; or "Pop" meaning to pop one node from the stack.

Output Specification:
For each test case, print the postorder traversal sequence of the corresponding tree in one line. A solution is guaranteed to exist. All the numbers must be separated by exactly one space, and there must be no extra space at the end of the line.

Sample Input:
6
Push 1
Push 2
Push 3
Pop
Pop
Push 4
Pop
Pop
Push 5
Push 6
Pop
Pop
Sample Output:
3 4 2 6 5 1

```c++
#include <iostream>
#include <cstdio>
#define MAX_N 31
using namespace std;
int pre[MAX_N];
int in[MAX_N];
int post[MAX_N];

void solve(int preL, int inL, int postL, int n) {
    if(n == 0)
        return;
    if(n == 1) {
        post[postL] = pre[preL]; //范围为1时，写入对应位置的元素
        return;
    }
    int root = pre[preL];
    post[postL + n - 1] = root; //后序遍历，头节点在该子树的最后位置
    int i;
    // 寻找头节点在 in 数组中的位置
    for (i = 0; i < n; i++) {
        if(in[inL + i] == root)
            break;
    }
    int lCnt = i, rCnt = n - lCnt - 1; // 左、右子树元素个数
    solve(preL + 1, inL, postL, lCnt);
    solve(preL + lCnt + 1, inL + lCnt + 1, postL + lCnt, rCnt);
}

int main() {
    int n, x, tmp;
    string s;
    scanf("%d", &n);
    int outCnt{0}, inCnt{0}; //出栈元素个数总计,入栈元素个数总计. 另：栈中现有元素个数=intCnt-outCnt
    /**
     * 读取操作项，并记录于 pre 数组和 in 数组
     */
    for (int i = 0; i <= n*2; i++) {
        cin >> s;
        if(s[1] == 'u') { //push
            cin >> x;
            pre[inCnt++] = x; //入栈个数加一
            in[n - (inCnt-outCnt)] = x; //栈中元素按序暂存在in数组尾部，尾部相当于一个模拟的栈
        }
        else { //pop
            tmp = in[n - (inCnt-outCnt)]; //取出栈顶的元素
            in[outCnt++] = tmp; //将栈顶元素“出栈”，即按序存储在in数组头部
        }
    }
    /*
     *读取结束，in数组存储了元素出栈顺序！
     */
    /*
     * 根据 in 数组和 pre 数组， 即先序遍历和中序遍历结果，推导后序遍历结果
     */
    solve(0, 0, 0, n);
    
    for (int i = 0; i < n; i++) {
        printf(i == n - 1 ? "%d" : "%d ", post[i]);
    }
    return 0;
}

```