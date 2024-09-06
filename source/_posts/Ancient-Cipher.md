---
title: Ancient Cipher
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-09-06 15:39:38
---
### 題目來源
- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-1339
- **連結**：[題目連結](https://vjudge.net/problem/UVA-1339)

### 題目描述
古羅馬帝國有一個強大的政府系統，包括一個秘密服務部門。重要文件以加密形式在省份和首都之間傳送，以防止竊聽。當時最流行的加密方法是所謂的替代加密和置換加密。

替代加密將每個字母的所有出現都更改為其他字母。所有字母的替代必須不同。對於某些字母，替代字母可以與原始字母相同。例如，應用將字母從‘A’到‘Y’的替代為字母表中的下一個字母，並將‘Z’替代為‘A’，對於消息“VICTORIOUS”得到“WJDUPSJPVT”。

置換加密對消息的字母進行某種置換。例如，對消息“VICTORIOUS”應用置換 ⟨2, 1, 5, 4, 3, 7, 6, 10, 9, 8⟩，得到消息“IVOTCIRSUO”。

顯然，僅僅使用替代加密和置換加密單獨應用，兩者都是相對脆弱的。但是當兩者結合時，足夠強大。最重要的消息首先使用替代加密進行加密，然後再使用置換加密進行加密。因此，最重要的消息使用上述描述的兩種加密方法進行加密後，得到消息“JWPUDJSTVP”。

考古學家最近在一塊石板上發現了刻有消息。乍一看它似乎完全沒有意義，因此猜測該消息可能使用了某種替代加密和置換加密。他們提出了可能的原始消息文本，並希望檢查他們的猜測。他們需要一個計算機程序來進行檢查，因此你必須編寫一個程序來實現這一點。

### 思路與解法
#### 分析
- 題目要求檢查給定的加密消息是否可能是猜測的原始消息經過替代加密和置換加密後的結果。這需要考慮替代加密和置換加密的組合效果。

- 我們可以先進行替代加密的驗證，然後再進行置換加密的驗證。

#### 解法過程
- **替代加密驗證**：
  1. 先創建兩個長度為 26 的整數數組 `sub1` 和 `sub2`，用來存儲加密消息和原始消息中每個字母的頻率。
  2. 遍歷加密消息和原始消息，更新 `sub1` 和 `sub2` 中對應字母的計數。
  3. 將 `sub1` 和 `sub2` 排序，然後比較排序後的結果。如果兩者完全匹配，則說明可以通過某種替代加密得到相同的字母頻率。

- **最終選擇的解法**：
  - 直接使用上述替代加密驗證函數 `validSubstitution` 來檢查是否可能有某種替代加密使得原始消息和加密消息的字母頻率匹配。若匹配則輸出“YES”，否則輸出“NO”。該方法充分考慮了替代加密的效果，並能有效地驗證是否存在可能的加密方案。

### 代碼實現
```cpp
#include <bits/stdc++.h>
using namespace std;

bool validSubstitution(array<int, 26> &sub1, array<int, 26> &sub2, string en, string on) {
  for(int i = 0; i < en.size(); i++) {
    sub1[en[i] - 'A']++;
    sub2[on[i] - 'A']++;
  }
  sort(sub1.begin(), sub1.end());
  sort(sub2.begin(), sub2.end());

  for(int i = 0; i < sub1.size(); i++) {
    if (sub1[i] != sub2[i]) return false;
  }

  return true;
}

int main() {
  ios::sync_with_stdio(0);cin.tie(0);

  string encrypted, original;
  while(cin >> encrypted >> original) {

    array<int, 26> sub1 = {0};
    array<int, 26> sub2 = {0};

    cout << (validSubstitution(sub1, sub2, encrypted, original) ? "YES" : "NO") << endl;
  }
}
```
