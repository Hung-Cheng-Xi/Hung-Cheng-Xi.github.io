---
title: Box
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-23 00:31:25
---
### 題目來源
- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-1587
- **連結**：[題目連結](https://vjudge.net/problem/UVA-1587)

### 題目描述
Ivan 在一家工廠工作，負責組裝用於運送重型機械的木箱。每個木箱都是長方體，需要六塊矩形木板來組裝。Joe 負責將木板運送給 Ivan，但他經常帶來不匹配的木板。Ivan 請求你寫一個程式，來判斷給定的六塊木板是否能組成一個箱子。

### 思路與解法
#### 分析
這個問題的本質是檢查六塊木板是否能夠組成一個長方體。要形成一個長方體，木板必須成對出現，並且它們的邊長應該符合長方體三個不同面所需的尺寸。具體而言，每個長方體有三個不同的面，因此，給定的六塊木板應該能夠形成三對相同的面。

#### 解法過程
- **方法一：暴力檢查法**
  - 對於所有六塊木板，先將它們的邊長進行排序，使得每塊木板的邊長按照從小到大排列。
  - 接著，將這六對木板邊長按字典序排序，並檢查這六塊木板是否可以配對成三對相同的面。如果成功配對，則可以組成一個長方體；否則，不可能組成長方體。
  - 優點：這種方法直接，便於理解。
  - 缺點：需要對所有可能的配對進行檢查，可能在一些邊界情況下不夠高效。

- **方法二：排序與邊長配對檢查**
  - 先將每塊木板的邊長進行排序（小的在前，大的在後），這樣可以確保在比較時不會因為邊長順序不同而導致錯誤。
  - 接著，將這六塊木板按照邊長對排序，形成三對面，並檢查這三對是否完全相同。如果這三對木板完全相同，那麼這六塊木板就可以組成一個長方體。
  - 此外，我們還需要檢查這些配對的邊長是否能形成一個長方體的三個不同面。為此，我們可以檢查其中一個面的邊長之和是否等於其他兩個面的邊長之和。這部分檢查確保了組成長方體的數學條件滿足。
  - 優點：通過排序來簡化檢查過程，比暴力法更高效且更直觀。同時，檢查邊長之和的方式確保了所有可能的配對都被正確檢查，不會錯過任何組合。
  - 缺點：如果實現細節有錯誤，可能導致不正確的結果。

- **最終選擇的解法**：
  - 我選擇了**方法二**，因為它通過排序來簡化檢查過程，同時結合了數學條件的檢查，能夠更準確且有效地解決這個問題。

### 代碼實現
```cpp
#include <bits/stdc++.h>
using namespace std;

bool sideEqual(vector<pair<int, int>> &sides) {
  if(sides[0] != sides[1] || sides[2] != sides[3] || sides[4] != sides[5]) return false;

  return true;
}

bool sumSide(vector<pair<int, int>> &sides) {
  pair<int, int> a = sides[0];
  pair<int, int> b = sides[2];
  pair<int, int> c = sides[4];

  if(((b.first + b.second) - a.first) == ((c.first + c.second) - a.second)) return true;
  if(((c.first + c.second) - a.first) == ((b.first + b.second) - a.second)) return true;

  return false;
}

int main() {
  ios::sync_with_stdio(0);cin.tie(0);

  while(true) {
    int weight, height;
    vector<pair<int, int>> sides;

    for(int i = 0; i < 6; i++) {
      if(!(cin >> weight >> height)) return 0;
      sides.push_back(make_pair(min(weight, height), max(weight, height)));
    }

    sort(sides.begin(), sides.end());
    cout << (sideEqual(sides) && sumSide(sides) ? "POSSIBLE" : "IMPOSSIBLE") << endl;
  }
}
```
