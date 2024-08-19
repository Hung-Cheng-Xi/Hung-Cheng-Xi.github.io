---
title: Score
thumbnail: https://imgur.com/fufoJmK.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-19 13:53:48
---

### 題目來源

- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-1585
- **連結**：[題目連結](https://vjudge.net/problem/UVA-1585)

### 題目描述

> 給定一個測驗結果的字串，例如 "OOXXOXXOOO"。'O' 表示該題答對，'X' 表示該題答錯。每題的分數計算是基於該題本身以及之前連續的 'O' 數量。例如，第 10 題的分數是 3，因為該題本身和前面連續的兩個 'O' 都是正確的。因此，"OOXXOXXOOO" 的總分為 10，其計算方法為 "1+2+0+0+1+0+0+1+2+3"。

### 思路與解法

#### 分析

1. **分數計算邏輯**：對於每一個字符，我們需要計算它應該得多少分。當遇到 'O' 時，我們需要考慮它之前連續的 'O' 的數量，這樣才能計算出它應該得的分數。

2. **變數設置**：我們可以使用一個變數 `current` 來記錄當前連續的 'O' 的數量。每次遇到 'O'，這個變數加 1；遇到 'X' 時，這個變數重置為 0。然後，我們將 `current` 的值加到總分 `score` 中。

3. **最小化循環**：由於每個字符只需要處理一次，因此可以使用一次循環來完成所有計算，這保證了我們的算法是線性的 \(O(n)\)。

#### 解法過程

1. **初始化**：設置總分變數 `score` 為 0，以及當前連續 'O' 的計數器 `current` 為 0。

2. **遍歷字串**：

   - **檢查每個字符**：逐個檢查字串中的每個字符：
     - 如果字符是 'O'，則 `current` 增加 1，並將 `current` 加到 `score`。
     - 如果字符是 'X'，則將 `current` 重置為 0。

3. **輸出結果**：在遍歷結束後，`score` 即為該測驗結果的最終分數，將其輸出。

### 代碼實現

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ios::sync_with_stdio(0);cin.tie(0);

  int t; cin >> t;
  while(t--){
    string temp; cin >> temp;
    int score = 0;
    int current = 0;

    for(char c: temp){
      if(c == 'O') current++;
      if(c == 'X') current = 0;

      score += current;
    }

    cout << score << endl;
  }
}
```
