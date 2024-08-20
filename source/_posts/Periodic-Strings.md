---
title: Periodic Strings
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-20 15:10:49
---
### 題目來源
- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-455
- **連結**：[題目連結](https://vjudge.net/problem/UVA-455)

### 題目描述
> 給定多個測試字串，對於每個字串，找出其最小周期。字串的最小周期是指在不改變原字串的情況下，可以通過重複這個周期多次來組成該字串的最小子串的長度。例如，對於字串 "abcabcabc"，最小周期是 "abc"，其長度為 3。

### 思路與解法
#### 分析
- **問題分析**：題目要求找出字串的最小周期。最小周期是字串的最小子串長度，該子串可以重複多次來完全構成原字串。這類問題通常可以使用「前綴函數」（也稱為部分匹配表，常在 KMP 演算法中使用）來解決，因為它可以有效地幫助我們找到子串的重複結構。

- **可能的解法**：
  1. **暴力解法**：直接檢查每個可能的周期長度，並驗證整個字串是否是這個周期的重複。這種方法雖然直觀，但效率低下，尤其在字串長度較大時，會導致時間複雜度過高。
  2. **使用前綴函數的解法**：利用 KMP 演算法的前綴函數（LPS 陣列），可以有效地找到字串的最小周期。通過計算出字串的前綴函數，我們可以判斷字串的最小周期長度。

#### 解法過程
- **方法一**：暴力解法
  - **介紹**：逐個檢查每個可能的周期長度，從 1 到 n/2，看看整個字串是否是這個周期的重複。
  - **優缺點**：此方法的缺點在於時間複雜度較高，為 \(O(n^2)\)，不適合處理大規模的輸入。

- **方法二**：前綴函數解法（KMP 演算法）
  - **介紹**：使用 KMP 演算法中的前綴函數（LPS 陣列）來計算字串的最小周期。具體步驟是：
    1. 計算字串的 LPS 陣列，這個陣列可以幫助我們找到字串中重複的部分。
    2. 最小周期的長度可以通過公式 `differ = len - lps[len - 1]` 來確定。如果 `len % differ == 0`，則 `differ` 是最小周期；否則，整個字串長度 `len` 是最小周期。
  - **優缺點**：此方法的優點在於時間複雜度為 \(O(n)\)，適合處理大規模的輸入，是更高效的解法。

- **最終選擇的解法**：
  - 我最終選擇了「前綴函數解法」，因為它在時間複雜度和空間複雜度上都更優。此方法能夠在線性時間內解決問題，適合處理題目給定的最大輸入範圍（1 ≤ N ≤ 100,000）。

### 代碼實現
```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
  ios::sync_with_stdio(0);cin.tie(0);

  int n; cin >> n;
  cin.ignore();

  vector<string> testcases;
  string line;

  while(getline(cin, line)){
    if(line.empty()) continue;

    testcases.push_back(line);
  }

  for(int index = 0; index < testcases.size(); index++){
    int len = testcases[index].size();
    vector<int> lps(len, 0);

    int i = 1, j = 0;
    while(i < len) {
      if(testcases[index][i] == testcases[index][j]) {
        lps[i++] = ++j;
      } else if (j > 0) {
        j = lps[j - 1];
      } else {
        lps[i++] = 0;
      }
    }

    int differ = len - lps[len - 1];
    if(len % differ == 0){
      cout << differ;
    } else {
      cout << len;
    }

    if(index < testcases.size() - 1) {
      cout << "\n\n";
    } else {
      cout << "\n";
    }
  }
}
```
