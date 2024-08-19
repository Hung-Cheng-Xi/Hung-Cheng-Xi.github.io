---
title: Digit Counting
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-19 20:49:48
---

### 題目來源

- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-1225
- **連結**：[題目連結](https://vjudge.net/problem/UVA-1225)

### 題目描述

> Trung 厭倦了他的數學作業，他拿起一根粉筆，開始從 1 到 N（1 < N < 10000）寫下一系列連續的整數。寫完後，他數了一下每個數字（0 到 9）在這個序列中出現的次數。例如，當 N = 13 時，序列為 12345678910111213。在這個序列中，數字 0 出現了 1 次，數字 1 出現了 6 次，數字 2 出現了 2 次，數字 3 出現了 3 次，而數字 4 到 9 各出現了 1 次。你的任務是寫一個程序來幫助 Trung 計算這些數字出現的次數。

### 思路與解法

#### 分析

- 題目要求計算數字 0 到 9 在從 1 到 N 的序列中出現的次數。這是典型的數位統計問題。
- 我們可以通過遍歷從 1 到 N 的每個數字，將它們轉換為字符串並統計各個數字出現的次數。

#### 解法過程

1. **字符串拼接**：從 1 到 N 的每個數字都轉換為字符串，並將它們拼接成一個長字符串。這樣可以確保每個數字的每一個字符都能被逐一處理。
2. **計算數字出現次數**：遍歷這個長字符串，使用一個大小為 10 的整數陣列來統計每個數字字符（0 到 9）出現的次數。陣列的索引對應數字，值對應出現次數。
3. **輸出結果**：將這個陣列中的每個值按順序輸出，即數字 0 到 9 的出現次數。

### 代碼實現

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ios::sync_with_stdio(0);cin.tie(0);

  int n; cin >> n;
  while(n--){
    int num; cin >> num;
    string temp;
    array<int, 10> arr;
    arr.fill(0);
    for(int i = 1; i < num + 1; i++){
      temp.append(to_string(i));
    }

    for(char c: temp){
      arr[c - '0']++;
    }

    for(int i = 0; i < arr.size(); i++){
      if(i > 0) cout << " ";
      cout << arr[i];
    }
    cout << endl;
  }
}
```
