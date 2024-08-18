---
title: Digit Generator
date: 2024-08-18 20:49:25
thumbnail: 'https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png'
excerpt: '这是文章摘要 This is the excerpt of the post'
tags:
  - 'code'
  - 'vjudge'
  - 'UVA'
categories: code
---

### 題目來源

- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-1583
- **連結**：[題目連結](https://vjudge.net/problem/UVA-1583)

### 題目描述

> 對於一個正整數 \( N \)，其數字和（digit-sum）定義為 \( N \) 本身加上 \( N \) 的各位數字的和。當 \( M \) 是 \( N \) 的數字和時，我們稱 \( N \) 為 \( M \) 的生成器。
> 例如，245 的數字和是 256（即 \( 245 + 2 + 4 + 5 \)）。因此，245 是 256 的生成器。
> 有些數字可能沒有任何生成器，有些數字可能有多個生成器。例如，216 的生成器有 198 和 207。
> 你的任務是寫一個程序來找出給定整數的最小生成器。

### 思路與解法

#### 分析

1. **數字和定義**：

   - 對於一個數字 \( N \)，數字和定義為 \( N' + N') 的各位數字的和，其中 \( N' \) 是 \( N \) 的生成器。

2. **最大可能數字和**：

   - 對於一個 \( d \) 位數字，最大可能的數字和是 \( 9 \* d \)。
   - 例如，對於 4 位數字，其數字和的最大值是 \( 9 \* 4 = 36 \)。

3. **範圍確定**：
   - 生成器 \( N' \) 的可能範圍是從 \( N - 9 \* d \) 到 \( N \)。
   - 這是因為如果 \( N' \) 小於 \( N - 9 \* d \)，即使加上它的數字和也不可能達到 \( N \)。
   - 而 \( N' \) 大於 \( N \) 的情況，結果會超過 \( N \)，因此不需要考慮。

#### 解法過程

- 對於每個測試用例：
  1.  計算可能的生成器範圍：\( [N - 9 * d, N] \)。
  2.  在這個範圍內遍歷所有數字，檢查它們是否滿足生成器條件，即： N' + N'{數字和} = N

  3.  如果找到符合條件的生成器，選擇其中最小的；如果沒有找到，則輸出 '0'。

### 代碼實現

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ios::sync_with_stdio(0);cin.tie(0);

  int t; cin >> t;
  while(t--){
    int digit; cin >> digit;
    int max_len = to_string(digit).size() * 9;

    bool found = false;
    for(int i = (digit - max_len); i < digit; i++){
      int total = i;
      string temp = to_string(total);

      for(char c: temp) {
        total += c - '0';
      }

      if (total == digit) {
        cout << i << endl;

        found = !found;
        break;
      }
    }

    if(!found) {
      cout << 0 << endl;
    }
  }
}
```
