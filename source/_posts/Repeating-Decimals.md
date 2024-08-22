---
title: Repeating Decimals
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-22 14:57:50
---
### 題目來源
- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-202
- **連結**：[題目連結](https://vjudge.net/problem/UVA-202)

### 題目描述
> 對於每一個輸入的分數（分子與分母），輸出它的十進制展開，其中重複循環的小數部分用括號標出。如果循環周期在前 50 個小數位之內出現，則完整顯示；如果超出 50 個小數位，只顯示到第 50 位，並用 `...)` 標記其餘部分。最後輸出該分數的小數展開的循環周期的長度。

### 思路與解法
#### 分析
- 當計算分數的十進制展開時，若有小數部分，小數點後的數字序列會出現循環。這種循環是因為當餘數再次出現相同的值時，已經出現的數字序列將再次重複。
- 我們可以使用一個 `unordered_map` 來記錄每個餘數出現的位置，當我們遇到一個已經出現過的餘數時，意味著開始出現循環。我們可以通過該位置來確定循環的開始點和長度。

#### 解法過程
- **方法一**：暴力法
  - 簡單地模擬除法過程，每次記錄餘數，當餘數重複時標記循環開始位置。這種方法實現起來簡單，但需要注意循環可能會超過 50 個位數，因此需要額外處理。
  - 優點：直觀簡單，容易實現。
  - 缺點：當需要處理大數時，性能可能會下降。

- **最終選擇的解法**：暴力法
  - 我們選擇暴力法進行實現，因為該方法在問題給定的數據範圍內是高效且可行的。透過 `unordered_map` 記錄餘數出現的位置，能夠有效地檢測循環開始的位置，並計算出循環長度。

### 代碼實現
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ios::sync_with_stdio(0);cin.tie(0);

  int dividend, divisor;
  while(cin >> dividend >> divisor) {

    cout << dividend << "/" << divisor << " = ";

    int quotient = dividend / divisor;
    int remainder = dividend % divisor;
    string answer = to_string(quotient) + ".";
    string remainder_answer;

    unordered_map<int, int> remainder_positions;

    int num = 0;
    int repeat_start = -1;

    while(remainder != 0){
      if (remainder_positions.find(remainder) != remainder_positions.end()) {
        repeat_start = remainder_positions[remainder];
        break;
      }

      remainder_positions[remainder] = num++;

      remainder *= 10;
      dividend = remainder;

      quotient = dividend / divisor;
      remainder_answer += to_string(quotient);

      remainder = dividend % divisor;
    }

    if(repeat_start != -1) {
      answer += remainder_answer.substr(0, repeat_start) + "(" + remainder_answer.substr(repeat_start) + ")";
    } else {
      answer += remainder_answer + "(0)";
    }

    if (remainder_answer.size() > 50) {
      answer = answer.substr(0, answer.find("(")) + "(" + remainder_answer.substr(repeat_start, 50 - repeat_start) + "...)";
    }

    cout << answer << endl;
    cout << "   " << (repeat_start != -1 ? remainder_answer.size() - repeat_start : 1) << " = number of digits in repeating cycle" << endl << endl;
  }
}
```
