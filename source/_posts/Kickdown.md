---
title: Kickdown
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-25 16:29:38
---
### 題目來源
- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-1588
- **連結**：[題目連結](https://vjudge.net/problem/UVA-1588)

### 題目描述
> 一個研究實驗室需要製造特殊的傳動機構，這要求使用有不均勻齒數的特殊齒輪。這些齒輪的初步實驗階段使用平面齒段，每段長度為 `n` 單位。每個單位要麼是深度為 `h` 的凹槽，要麼是高度為 `2h` 的齒。實驗需要兩個齒段：一個用於主齒輪（齒位於底部），另一個用於從動齒輪（齒位於頂部）。實驗室有一條寬度為 `3h` 的長條，長度足夠用來切割這兩個齒段。需要找到能同時切割兩個齒段的最小長度。

### 思路與解法
#### 分析
- 我們需要找到兩個不相同的齒段（主齒輪和從動齒輪）在寬度為 `3h` 的條上能夠完全匹配的最小長度。這意味著要找到兩段重疊的有效範圍，其中對應的齒和凹槽必須正確對齊。

#### 解法過程
- **方法一**：暴力法
  - 暴力法是通過嘗試所有可能的對齊方式來檢查兩段齒輪的有效重疊。具體來說，對於每一個可能的偏移量，檢查主齒輪和從動齒輪的對齊是否存在非法重疊。這種方法的優點是直觀易懂，適合用於小範圍內的問題。缺點是效率較低，尤其是當齒段長度增大時，計算量會急劇增加。

- **最終選擇的解法**：
  - 我們選擇方法一，因為它能夠保證找到最小長度並且對於小範圍問題能夠提供足夠的準確性。對於每個測試用例，我們嘗試所有可能的偏移量，計算所需的最小長度，然後返回結果。

### 代碼實現
```cpp
#include <bits/stdc++.h>
using namespace std;

// 計算兩段齒輪在給定偏移量下的最小材料長度
int calculateMinLength(string masterSection, string drivenSection) {
    int minLength = 0; // 初始化結果長度為0
    int masterLen = masterSection.size();

    // 嘗試將 drivenSection 對齊 masterSection 的不同起始位置
    for (int offset = 0; offset < masterSection.size(); offset++) {
        bool hasOverlap = false;

        // 檢查從當前 offset 開始的重疊部分
        for (int j = 0; j < min(masterSection.size() - offset, drivenSection.size()); j++) {
            if (masterSection[offset + j] == drivenSection[j] && drivenSection[j] == '2') {
                hasOverlap = true; // 找到不合法的重疊，跳出
                break;
            }
        }

        // 如果沒有不合法重疊，計算所需長度並結束檢查
        if (!hasOverlap) {
            minLength = drivenSection.size() + offset;
            break;
        }
    }

    // 如果所有可能的重疊位置都有不合法重疊，則結果為兩段的總長度
    if (!minLength) {
        minLength = masterSection.size() + drivenSection.size();
    }

    // 返回結果，考慮最大段長的情況
    return max(masterLen, minLength);
}

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);

    string masterSection, drivenSection;

    // 讀取多個測試案例，直到輸入結束
    while (cin >> masterSection >> drivenSection) {
        // 計算兩種可能重疊方式中的最小材料長度
        cout << min(calculateMinLength(masterSection, drivenSection), calculateMinLength(drivenSection, masterSection)) << endl;
    }
}
```
