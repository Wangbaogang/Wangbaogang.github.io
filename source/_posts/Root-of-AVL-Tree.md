---
title: Root of AVL Tree
date: 2019-07-18 09:44:19
tags:
---

An AVL tree is a self-balancing binary search tree. In an AVL tree, the heights of the two child subtrees of any node differ by at most one; if at any time they differ by more than one, rebalancing is done to restore this property. Figures 1-4 illustrate the rotation rules.

![](/images/root-of-avl-1.jpg) ![](/images/root-of-avl-2.jpg) ![](/images/root-of-avl-3.jpg) ![](/images/root-of-avl-4.jpg)
Now given a sequence of insertions, you are supposed to tell the root of the resulting AVL tree.

Input Specification:
Each input file contains one test case. For each case, the first line contains a positive integer N (≤20) which is the total number of keys to be inserted. Then N distinct integer keys are given in the next line. All the numbers in a line are separated by a space.

Output Specification:
For each test case, print the root of the resulting AVL tree in one line.

Sample Input 1:
5
88 70 61 96 120
Sample Output 1:
70
Sample Input 2:
7
88 70 61 96 120 90 65
Sample Output 2:
88

```cpp
#include <iostream>
#include <cstdio>
using namespace std;
typedef int ElementType;
typedef struct AVLNode* PrtToNode;
struct AVLNode {
    PrtToNode Left;
    PrtToNode Right;
    int Height;
    ElementType Data;
};
typedef PrtToNode AVLTree;

PrtToNode CreateNode(ElementType X) {
    PrtToNode P = (PrtToNode)malloc(sizeof(struct AVLNode));
    P->Data = X;
    P->Height = 0;
    P->Left = nullptr;
    P->Right = nullptr;
    return P;
}

int GetHeight(AVLTree T) {
    if(!T)
        return 0;
    return T->Height;
}
int max(int a, int b) {
    return a > b ? a : b;
}

AVLTree SingleLeftRotation(AVLTree A) {
    AVLTree B = A->Left;
    A->Left = B->Right;
    B->Right = A;
    A->Height = max(GetHeight(A->Left), GetHeight(A->Right)) + 1;
    B->Height = max(A->Height, GetHeight(B->Left)) + 1;
    return B;
}

AVLTree SingleRightRotation(AVLTree A) {
    AVLTree B = A->Right;
    A->Right = B->Left;
    B->Left = A;
    A->Height = max(GetHeight(A->Left), GetHeight(A->Right)) + 1;
    B->Height = max(A->Height, GetHeight(B->Right)) + 1;
    return B;
}

AVLTree DoubleLeftRightRotaion(AVLTree A) {
    A->Left = SingleRightRotation(A->Left);
    A = SingleLeftRotation(A);
    return A;
}

AVLTree DoubleRightLeftRotation(AVLTree A) {
    A->Right = SingleLeftRotation(A->Right);
    A = SingleRightRotation(A);
    return A;
}

AVLTree Insert(AVLTree T, ElementType X) {
    if(!T) {
        T = CreateNode(X);
    }
    else if(X < T->Data) {
        T->Left = Insert(T->Left, X);
        if(GetHeight(T->Left) - GetHeight(T->Right) == 2) {
            if(X < T->Left->Data)
                T = SingleLeftRotation(T);
            else
                T = DoubleLeftRightRotaion(T);
        }
            
    }
    else if(X > T->Data) {
        T->Right = Insert(T->Right, X);
        if(GetHeight(T->Left) - GetHeight(T->Right) == -2){
            if(X > T->Right->Data)
                T = SingleRightRotation(T);
            else
                T = DoubleRightLeftRotation(T);
        }
    }
    
    T->Height = max(GetHeight(T->Left), GetHeight(T->Right)) + 1;
    return T;
}

int main() {
    int N, X;
    cin >> N;
    AVLTree T = nullptr;
    for (int i = 0; i < N; i++) {
        cin >> X;
        T = Insert(T, X);
    }
    cout << T->Data;
    return 0;
}

```