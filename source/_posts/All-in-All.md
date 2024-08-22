---
title: All in All
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-22 15:49:53
---
### 題目來源
- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-10340
- **連結**：[題目連結](https://vjudge.net/problem/UVA-10340)

### 題目描述
> 給定兩個字串 `s` 和 `t`，需要檢查 `s` 是否為 `t` 的子序列。子序列的定義是：可以通過從 `t` 中刪除一些字元而不改變剩餘字元的相對順序來得到 `s`。對於每個測試用例，輸入兩個以空格分隔的字串，並在 EOF 結束輸入。輸出是對於每個測試用例，如果 `s` 是 `t` 的子序列，輸出 "Yes"，否則輸出 "No"。

### 思路與解法
#### 分析
- 子序列問題要求我們判斷一個字串是否可以通過刪除另一個字串中的某些字元而形成。這裡我們可以通過線性掃描 `t` 字串來找到 `s` 中的每個字元，並檢查是否存在這樣的順序。
- 如果在 `t` 中找到 `s` 的每個字元，且它們的相對順序保持不變，則 `s` 是 `t` 的子序列。

#### 解法過程
- **方法一**：遍歷查找法
  - 我們逐字元掃描 `s`，並在 `t` 中查找每個字元。如果找到，則將 `t` 字串截斷至該字元之後並繼續查找下一個字元。如果 `s` 中的所有字元都能依次在 `t` 中找到，則 `s` 是 `t` 的子序列，否則不是。
  - 優點：簡單易懂，對於小範圍的字串運算速度很快。
  - 缺點：每次查找後對 `t` 進行截斷可能會影響效率，特別是對於長字串。

- **最終選擇的解法**：遍歷查找法
  - 該方法易於實現，並且對於給定問題範圍內的數據可以有效工作。因此選擇此方法作為解決方案。

### 代碼實現

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
  ios::sync_with_stdio(0);cin.tie(0);

  string s, t;
  while(cin >> s >> t){
    bool is_subsequence = true;

    for(char c: s) {
      int index = t.find(c);
      if(index == string::npos) {
        is_subsequence = false;
        break;
      }

      t = t.substr(index + 1);
    }

    cout << (is_subsequence ? "Yes" : "No") << endl;
  }
}
```
