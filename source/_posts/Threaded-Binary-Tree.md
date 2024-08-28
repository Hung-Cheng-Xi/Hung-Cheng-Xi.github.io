---
title: Threaded Binary Tree
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - 資料結構實作
categories: 資料結構實作
date: 2024-08-28 17:12:55
---

## 引線二叉樹（Threaded Binary Tree）

### 介紹
**連結**：
[參考學習連結 1](https://www.youtube.com/watch?v=i1imRw8tmnk&t=233s)
[參考學習連結 2](https://www.youtube.com/watch?v=9b2GAFGZ6yw&t=266s)
> 引線二叉樹是一種特殊的二叉樹，其中所有空的子樹指針都被用來存儲對樹中其他節點的指針，這樣可以實現高效的中序遍歷，而不需要使用棧或遞歸。

### 資料結構概述
引線二叉樹的核心思想是在普通的二叉樹上進行改造，使得所有空的左指針和右指針指向樹中某些節點，從而實現高效的中序遍歷。這樣的結構不僅可以避免使用遞歸或棧，還能在遍歷過程中節省空間。

- **結構設計**：每個節點除了原有的左子樹和右子樹指針外，還增加了 `hasLeftThread` 和 `hasRightThread` 標記，分別指示是否存在左線引和右線引。
- **主要特性**：引線二叉樹的主要特性是其能夠在中序遍歷時不使用額外的空間。

### 操作和方法

#### 1. 插入節點到二叉樹中
這個操作將新節點插入到普通的二叉樹中，按層次順序插入。

##### 詳細步驟：
- 使用隊列進行層次遍歷，找到第一個空的左子節點位置，插入新節點。
- 如果左子節點位置已滿，則插入到右子節點位置。

#### 2. 將普通二叉樹轉換為引線二叉樹
這個操作將普通的二叉樹轉換為引線二叉樹，設置線引指針。

##### 詳細步驟：
- 遞歸遍歷左子樹，對每個節點設置左線引和右線引。
- 如果節點的左子節點為空，則設置左線引指向前驅節點。
- 如果節點的前驅節點的右子節點為空，則設置右線引指向當前節點。

#### 3. 中序遍歷引線二叉樹
這個操作在引線二叉樹中進行中序遍歷，利用線引指針提高效率。

##### 詳細步驟：
- 從樹的最左節點開始，逐步訪問每個節點。
- 如果節點有右線引，則直接跳到右子樹的節點。
- 否則，移動到右子樹，並找到右子樹的最左節點。

### 代碼實現

```cpp
#include <bits/stdc++.h>
using namespace std;

// 定義引線二叉樹的節點結構
struct ThreadedBinaryTreeNode {
    int hasLeftThread; // 標記是否有左線引
    ThreadedBinaryTreeNode* leftChild; // 指向左子節點或前驅
    char data; // 節點數據
    ThreadedBinaryTreeNode* rightChild; // 指向右子節點或後繼
    int hasRightThread; // 標記是否有右線引

    // 節點構造函數，默認為空節點
    ThreadedBinaryTreeNode(char data = '\0')
        : data(data), hasLeftThread(0), leftChild(nullptr), hasRightThread(0), rightChild(nullptr) {}
};

// 插入節點到二叉樹中，按層次順序
void insertIntoBinaryTree(ThreadedBinaryTreeNode*& root, char currData) {
    queue<ThreadedBinaryTreeNode*> nodeQueue;
    nodeQueue.push(root);

    while (!nodeQueue.empty()) {
        ThreadedBinaryTreeNode* currentNode = nodeQueue.front();
        nodeQueue.pop();

        if (currentNode->leftChild == nullptr) {
            // 在左子節點位置插入新節點
            currentNode->leftChild = new ThreadedBinaryTreeNode(currData);
            break;
        } else {
            nodeQueue.push(currentNode->leftChild);
        }

        if (currentNode->rightChild == nullptr) {
            // 在右子節點位置插入新節點
            currentNode->rightChild = new ThreadedBinaryTreeNode(currData);
            break;
        } else {
            nodeQueue.push(currentNode->rightChild);
        }
    }
}

// 將普通二叉樹轉換為引線二叉樹
void convertToThreadedBinaryTree(ThreadedBinaryTreeNode*& root) {
    static ThreadedBinaryTreeNode* previousNode = nullptr;

    if (!root) return;

    convertToThreadedBinaryTree(root->leftChild);

    if (root->leftChild == nullptr) {
        // 為空的左子節點設置前驅
        root->leftChild = previousNode;
        root->hasLeftThread = 1; // 標記為線引
    }

    if (previousNode && previousNode->rightChild == nullptr) {
        // 為空的右子節點設置後繼
        previousNode->rightChild = root;
        previousNode->hasRightThread = 1; // 標記為線引
    }

    // 更新前驅節點為當前節點
    previousNode = root;
    convertToThreadedBinaryTree(root->rightChild);
}

// 中序遍歷引線二叉樹
void inorderTraversalThreadedBinaryTree(ThreadedBinaryTreeNode*& root) {
    if (!root) return;

    ThreadedBinaryTreeNode* currentNode = root;

    // 找到中序遍歷的起點（最左節點）
    while (currentNode && !currentNode->hasLeftThread) {
        currentNode = currentNode->leftChild;
    }

    // 開始中序遍歷
    while (currentNode) {
        cout << currentNode->data << " ";

        // 使用線引直接跳到後繼節點
        if (currentNode->hasRightThread) {
            currentNode = currentNode->rightChild;
        } else {
            // 移動到右子樹，然後找到右子樹的最左節點
            currentNode = currentNode->rightChild;
            while (currentNode && !currentNode->hasLeftThread) {
                currentNode = currentNode->leftChild;
            }
        }
    }
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);

    vector<char> nodes = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
    ThreadedBinaryTreeNode* root = nullptr;

    // 建立普通二叉樹
    for (char c : nodes) {
        if (root == nullptr) {
            root = new ThreadedBinaryTreeNode(c);
        } else {
            insertIntoBinaryTree(root, c);
        }
    }

    // 將二叉樹轉換為引線二叉樹並進行中序遍歷
    convertToThreadedBinaryTree(root);
    inorderTraversalThreadedBinaryTree(root);
}
```
