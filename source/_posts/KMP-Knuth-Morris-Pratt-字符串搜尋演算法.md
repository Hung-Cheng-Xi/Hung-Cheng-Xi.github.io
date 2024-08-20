---
title: KMP (Knuth-Morris-Pratt)
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - 演算法實作
categories: 演算法實作
date: 2024-08-20 16:12:35
---
## KMP (Knuth-Morris-Pratt) 字符串搜尋演算法

### 介紹
**連結**：[參考學習連結](https://www.youtube.com/watch?v=dgPabAsTFa8)
> KMP (Knuth-Morris-Pratt) 演算法是一種用來在字串中搜尋子字串的演算法。它的主要特點是在匹配過程中，利用已經匹配的信息避免不必要的重新比較，從而在最壞情況下達到線性時間複雜度 O(n + m)，其中 n 是主字串的長度，m 是模式字串的長度。

### 演算法概述
KMP 演算法的核心思想是透過預處理模式字串，生成一個稱為 LPS（Longest Prefix Suffix）的輔助陣列。LPS 陣列記錄了在模式字串中每個位置處的部分匹配值，這樣在匹配過程中，當發生不匹配時，我們可以利用 LPS 陣列跳過一些比較，從而提高匹配效率。

### 實作步驟

#### 1. LPS 陣列的計算

LPS 陣列記錄了每個位置處的部分匹配值，即在該位置之前的子字串中，既是前綴又是後綴的最長子字串的長度。

##### 計算步驟：
- 初始化兩個指標 `i` 和 `j`。`i` 用於遍歷模式字串，`j` 用於追蹤部分匹配的長度。
- 當 `pattern[i] == pattern[j]` 時，表示當前字符匹配，則 `lps[i] = j + 1`，然後將 `i` 和 `j` 向右移動。
- 如果不匹配且 `j > 0`，則將 `j` 回溯至 `lps[j - 1]`，這樣可以避免重新從頭比較。
- 如果不匹配且 `j == 0`，則將 `lps[i]` 設為 0 並繼續移動 `i`。

#### 2. 主字串的搜尋
- 初始化指標 `i` 遍歷主字串 `text`。
- 使用一個內部循環比較 `pattern` 和 `text` 中的字符。
- 如果完全匹配，則記錄下起始位置並結束搜索。
- 如果不匹配，根據 `lps` 陣列決定跳過的位置，避免無意義的比較。

#### 3. 輸出匹配結果
- 如果找到匹配，輸出匹配開始和結束的位置。
- 如果沒有匹配，則不輸出任何結果。

### 代碼實現

```cpp
#include <bits/stdc++.h>
using namespace std;

string text = "abaacababcac";
string pattern = "ababc";
vector<int> lps(pattern.size(), 0);

void computeLPSArray(){
  int i = 1, j = 0;
  while(i < pattern.size() - 1){
    if(pattern[i] == pattern[j]){
      lps[i++] = ++j;
    } else if (j > 0) {
      j = lps[j - 1];
    } else {
      lps[i++] = 0;
    }
  }

  lps.pop_back();
  lps.insert(lps.begin(), -1);
}

int main() {
  ios::sync_with_stdio(0);cin.tie(0);

  computeLPSArray();

  int i = 0;
  while(i < text.size()) {
    bool found = true;
    for(int j = 0; j < lps.size(); j++){
      if(text[i + j] != pattern[j]){
        found = false;
        i += j - lps[j];
        break;
      }
    }

    if(found){
      cout << "starting position: " << i << " end position: " << i + lps.size() - 1 << endl;
      break;
    }
  }
}
```
