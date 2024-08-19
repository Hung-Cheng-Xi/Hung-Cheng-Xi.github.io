---
title: Molar mass
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-19 19:52:08
---
### 題目來源
- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-1586
- **連結**：[題目連結](https://vjudge.net/problem/UVA-1586)

### 題目描述
> 在有機化合物是分子中含有碳元素的化學化合物。這道題目要求給定有機化合物的分子式，計算該分子式的摩爾質量。分子式由元素符號及其對應的原子數量組成，如 `C3H4O3`。元素符號可能有四種：`C`（碳）、`H`（氫）、`O`（氧）、`N`（氮）。每個元素的標準原子量如下：

- 碳 (C): 12.01 g/mol
- 氫 (H): 1.008 g/mol
- 氧 (O): 16.00 g/mol
- 氮 (N): 14.01 g/mol

例如，分子式 `C6H5OH` 的摩爾質量為 94.108 g/mol，計算方式為：
6 × (12.01 g/mol) + 6 × (1.008 g/mol) + 1 × (16.00 g/mol)


### 思路與解法
#### 分析
1. **解析分子式**：從左到右遍歷分子式，識別每個元素及其對應的原子數量。
   - 如果數字部分缺失，則默認該元素的原子數量為 1。

2. **計算元素質量**：根據標準原子量和解析出的原子數量計算該元素的質量。

3. **計算總摩爾質量**：將所有元素的質量加總得到最終的摩爾質量。

#### 解法過程
1. **初始化**：創建一個字典來存儲每個元素的標準原子量，並初始化總質量變數 `total` 為 0。

2. **遍歷分子式**：
   - 逐字符遍歷分子式，識別元素符號。
   - 尋找元素符號後的數字並解析出原子數量。如果沒有數字則默認為 1。
   - 根據標準原子量和原子數量計算該元素的質量，並加到總質量中。

3. **輸出結果**：遍歷結束後，輸出總質量 `total`。

### 代碼實現
```cpp
#include <bits/stdc++.h>
using namespace std;

map<char, float> atomic_map = {
  {'C', 12.01},
  {'H', 1.008},
  {'O', 16.00},
  {'N', 14.01}};

int main()
{
  ios::sync_with_stdio(0);
  cin.tie(0);

  int t;
  cin >> t;
  while (t--)
  {
    string atomic;
    cin >> atomic;

    double number = 0.0, total = 0.0;
    int length = atomic.size();
    for(int i = 0; i < length; i++) {
      number = 0.0;
      char letter = atomic[i];


      while (i + 1 < length && isdigit(atomic[i + 1]))
      {
        number = number * 10 + (atomic[++i] - '0');
      }

      number = number <= 0 ? 1 : number;
      total += number * atomic_map[letter];
    }

    printf("%.3f\n", total);
  }
}
```
