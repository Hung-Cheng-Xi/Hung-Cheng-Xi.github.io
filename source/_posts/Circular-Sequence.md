---
title: Circular Sequence
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-19 00:51:00
---

### 題目來源

- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-1584
- **連結**：[題目連結](https://vjudge.net/problem/UVA-1584)

### 題目描述

> 給定一個圓形 DNA 序列，要求找出其所有可能的線性序列中字典序最小的序列。

### 思路與解法

#### 分析

- 由於圓形序列可以從任意位置開始，因此對於一個給定的序列，我們可以將其與自身拼接，得到一個新的序列。
- 通過截取不同位置的子串，可以模擬所有可能的線性序列。

#### 解法過程

- 拼接序列本身，以方便模擬圓形旋轉。
- 逐個截取每個可能的線性序列，並記錄當前字典序最小的序列。
- 輸出最小的線性序列作為結果。

### 代碼實現

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ios::sync_with_stdio(0);cin.tie(0);

  int t; cin >> t;
  while(t--){
    string temp; cin >> temp;

    string double_temp = temp + temp;
    for(int i = 0; i < temp.size(); i++){
      string temp2 = double_temp.substr(i, temp.size());

      if(temp > temp2) {
        temp = temp2;
      }
    }

    cout << temp << endl;
  }
}
```
