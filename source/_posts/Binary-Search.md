---
title: Binary Search
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - 演算法實作
categories: 演算法實作
date: 2024-08-21 01:02:25
---
## 二分查找演算法

### 介紹
**連結**：[Binary Search - GeeksforGeeks](https://www.geeksforgeeks.org/binary-search/)
> Binary Search 是一種高效的搜尋演算法，特別適用於已排序的數列。它每次將搜尋範圍縮小一半，以此快速找到目標元素或確定目標元素不存在。這種演算法常用於需要高效查找的應用場景，如數據庫查詢和搜索引擎。

### 演算法概述
Binary Search 的核心思想是通過將已排序數列的搜尋範圍逐步縮小，直到找到目標元素。其主要步驟包括設置初始搜尋範圍、計算中點、比較目標值與中點值、調整搜尋範圍以及最終確定目標元素的位置或返回未找到的訊息。

### 實作步驟

#### 1. 設定初始範圍
此步驟的目的是定義搜尋範圍的起始點與終止點。
##### 詳細步驟：
- 初始化 `left` 為 0，表示數列的起始索引。
- 初始化 `right` 為數列的最後一個索引，即 `temp.size() - 1`。
- 這些邊界確定了初始搜尋範圍。

#### 2. 計算中點並比較
此步驟的目的是計算當前搜尋範圍的中點，並比較該中點值與目標值。
##### 詳細步驟：
- 使用 `mid = left + (right - left) / 2` 計算中點，避免溢位。
- 比較中點值 `temp[mid]` 與目標值 `target`：
  - 如果相等，則返回中點索引，結束搜尋。
  - 如果中點值大於目標值，將右邊界 `right` 調整為 `mid - 1`。
  - 如果中點值小於目標值，將左邊界 `left` 調整為 `mid + 1`。

#### 3. 終止條件與返回結果
此步驟的目的是確保搜尋範圍正確縮小並返回正確結果。
##### 詳細步驟：
- 當 `left` 超過 `right` 時，表示目標值不在數列中，返回 `-1`。
- 返回的結果 `result` 將表示目標值的索引或未找到訊息。

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<int> temp{1, 2, 3, 5, 7, 8, 11, 12, 15, 19, 23, 25};
int target = 30;

int binarySearch(){
  int left = 0, right = temp.size() - 1;
  while(left <= right) {
    int mid = left + (right - left) / 2;
    if(temp[mid] == target){
      return mid;
    } else if (temp[mid] > target){
      right = mid - 1;
    } else{
      left = mid + 1;
    }
  }
  return -1;
}

int main() {
  ios::sync_with_stdio(0);cin.tie(0);

  int result = binarySearch();
  result > -1 ? cout << result << endl : cout << -1 << endl;
}
```
