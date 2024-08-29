---
title: Hangman Judge
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-29 23:14:57
---
### 題目來源
- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-489
- **連結**：[題目連結](https://vjudge.net/problem/UVA-489)

### 題目描述
> 遊戲要求玩家在最多7次錯誤猜測的範圍內，猜出隱藏單詞中的所有字母。輸入包括單詞和一系列的猜測字母。根據這些猜測，程式判斷玩家是贏得比賽、輸掉比賽還是退出比賽。

### 思路與解法

#### 分析
- **遊戲目標**：在最多7次錯誤猜測內猜出單詞中的所有字母。
- **判斷條件**：
  - 當錯誤猜測達到7次時，玩家輸掉遊戲。
  - 當所有字母被猜出並且錯誤猜測次數少於7次時，玩家贏得遊戲。
  - 當猜測結束但並未猜出所有字母且錯誤猜測次數少於7次時，玩家退出。

#### 解法過程

- **使用集合法**：
  使用兩個集合來分別追蹤單詞中的唯一字母和已猜中的字母。遍歷玩家的每次猜測，檢查該字母是否在單詞的集合中。若不在，增加錯誤次數；若在，將字母加入已猜中的集合。判斷條件如下：
  - 如果錯誤猜測達到7次，輸出 "You lose."
  - 如果所有字母都猜出但錯誤次數小於7次，輸出 "You win."
  - 如果猜測結束但未猜出所有字母且錯誤次數小於7次，輸出 "You chickened out."

### 代碼實現
```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios::sync_with_stdio(0);
    cin.tie(0);

    int roundNumber;
    while (true) {
        cin >> roundNumber;
        if (roundNumber == -1) break;
        cout << "Round " << roundNumber << endl;

        string solution, guesses;
        cin >> solution >> guesses;

        int incorrectGuesses = 0;
        unordered_set<char> uniqueSolutionLetters;
        unordered_set<char> guessedLetters;

        // 將solution中的所有字母加入集合
        for (char c : solution) uniqueSolutionLetters.insert(c);

        // 處理玩家的猜測
        for (char c : guesses) {
            if (uniqueSolutionLetters.find(c) == uniqueSolutionLetters.end()) {
                incorrectGuesses++;  // 錯誤猜測
            } else {
                guessedLetters.insert(c);  // 正確猜測
            }

            if (incorrectGuesses >= 7) break;  // 若錯誤次數達到7則遊戲結束
        }

        // 判斷遊戲結果
        if (incorrectGuesses >= 7) {
            cout << "You lose.";
        } else if (guessedLetters.size() != uniqueSolutionLetters.size()) {
            cout << "You chickened out.";
        } else {
            cout << "You win.";
        }

        cout << endl;
    }

    return 0;
}
```
