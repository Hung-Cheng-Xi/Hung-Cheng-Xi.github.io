---
title: The Dole Queue
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-30 18:54:15
---
### 題目來源
- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-133
- **連結**：[題目連結](https://vjudge.net/problem/UVA-133)
- **參考下方他人程式碼完成**

### 題目描述
> 每天所有的失業救濟申請者將被放置在一個大圓圈中，面向內部。有人被隨機選為編號 1，其餘人按逆時針方向編號至 N。從 1 開始逆時針方向數 k 個申請者，另一個官員從 N 開始順時針方向數 m 個申請者。被選中的兩人將被送去接受再培訓；如果兩位官員選擇了同一個人，她（他）將被送去成為政治家。每位官員然後從下一個可用的人開始再次計數，直到沒有任何人被留下。注意，兩個受害者（對不起，受訓者）同時離開環，因此一位官員可能會選中另一位官員已經選中的人。

### 思路與解法
#### 分析
- 首先，我們需要模擬逆時針和順時針的計數方式。兩個計數器分別從兩個方向開始，一個逆時針數 k 個人，另一個順時針數 m 個人。
- 當兩個計數器遇到同一個人的時候，該人被選中一次並離開隊列；如果計數器選中了不同的人，則兩個人同時被選中並離開隊列。
- 這個過程重複進行直到所有人都被選中。

#### 解法過程
- **方法一**: 使用模擬方法，我們可以使用一個布林數組來表示每個人是否已經被選中，然後按照給定的 k 和 m 來進行計數和移動。每次找到一個人後，更新數組並輸出結果。這種方法的優點是直觀，易於實現和理解；缺點是當 N 變大時，模擬的時間複雜度可能較高。
- **最終選擇的解法**: 我們最終選擇了模擬方法，因為 N 的範圍有限（最大為 19），這使得模擬方法的性能在可接受範圍內。

### 代碼實現
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
  ios::sync_with_stdio(0);cin.tie(0);

  int n, k, m;
  while(true) {
    cin >> n >> k >> m;
    if(n == 0 && k == 0 && m == 0) break;

    vector<bool> applicants(n, false);

    int i = 0, j = n - 1;
    int icount = 0, jcount = 0;
    int count = 0;

    while(count < n) {
      while (true) {
        if(!applicants[i]) icount++;
        if (icount == k) break;
        i = (i + 1) % n;
      }
      while (true) {
        if(!applicants[j]) jcount++;
        if (jcount == m) break;
        j = (j + (n - 1)) % n;
      }

      if(i == j) {
        applicants[i] = true;
        count++;
        cout << setw(3) << i + 1 << (count == n ? "" : ",");
      } else {
        applicants[i] = true; applicants[j] = true;
        count += 2;
        cout << setw(3) << i + 1 << setw(3) << j + 1 << (count == n ? "" : ",");
      }

      icount = 0; jcount = 0;
    }

    cout << endl;
  }
}
```
