---
title: Puzzle
thumbnail: https://evan.beee.top/img/208184324-f2640ade-587a-4f46-8ad1-7b4c1b31394f.png
tags:
  - code
  - vjudge
  - UVA
categories: code
date: 2024-08-21 18:26:43
---
### 題目來源
- **來源平台**：vjudge 平台收錄 UVA 題目
- **題目編號**：UVA-227
- **連結**：[題目連結](https://vjudge.net/problem/UVA-227)

### 題目描述
> 一個受歡迎的兒童拼圖問題涉及到一個 5×5 的框架，該框架包含 24 個小方塊，每個小方塊上印有一個獨特的字母。由於只有 24 個方塊，因此框架中還有一個空白位置。可以將方塊移動到空白位置，前提是它們在空白位置的上方、下方、左方或右方。目標是將方塊移動到正確的位置，以使字母按字母順序排列。

### 思路與解法
#### 分析
- **理解問題**：
  - 需要處理一個 5x5 的拼圖，其中包含 24 個字母和一個空白位置。
  - 根據給定的移動指令（A: 上移、B: 下移、L: 左移、R: 右移），更新拼圖的配置。
  - 目標是確定是否可以通過這些移動達到字母按字母順序排列的最終配置。
  - 需要檢查移動是否合法，並根據最終結果輸出結果。

- **可能的解法**：
  - **暴力法**：逐步應用每個移動指令，檢查每一步的合法性。這種方法的優點是實現簡單，但在面對大量移動指令時，效率可能較低。
  - **優化方法**：使用一種數據結構（如隊列或堆疊）來記錄每次移動的狀態，並在每步操作後更新拼圖狀態，這樣可以更高效地處理移動和檢查結果。

#### 解法過程
- **方法一**：簡單的暴力法
  - 逐步讀取每個移動指令，根據指令更新拼圖的空白位置和方塊位置。
  - 檢查每次移動是否合法，並在非法移動發生時終止處理並輸出錯誤信息。
  - 最後輸出拼圖的最終配置或錯誤信息。
  - 優點：簡單易實現。
  - 缺點：當移動指令數量很多時，效率可能不高。

- **方法二**：使用數據結構優化處理
  - 使用數據結構來記錄移動狀態，這樣可以在每次操作後快速更新拼圖狀態。
  - 實現時可以引入額外的檢查來確保每次移動都在合法範圍內。
  - 優點：在處理大量移動指令時效率更高。
  - 缺點：實現複雜度較高。

- **最終選擇的解法**：
  - 選擇了簡單的暴力法來解決問題。這種方法足夠應對題目中所提供的測試數據，實現相對簡單且易於理解。對於題目要求的操作和檢查來說，這種方法已經能夠滿足需求。

### 代碼實現
```cpp
#include <bits/stdc++.h>
using namespace std;

void moveChange(int after_row, int after_col, int before_row, int before_col, array<array<char, 5>, 5> &puzzle) {
  char temp = puzzle[after_row][after_col];
  puzzle[after_row][after_col] = puzzle[before_row][before_col];
  puzzle[before_row][before_col] = temp;
}

bool puzzleMove(char &c, pair<int, int> &empty_pos, array<array<char, 5>, 5> &puzzle) {
  int *row = &empty_pos.first, *col = &empty_pos.second;

  switch (c) {
    case 'A':
      if(*row > 0) {
        moveChange(*row, *col, *row - 1, *col, puzzle);
        *row -= 1;
      } else {
        return false;
      }
      break;

    case 'B':
      if(*row < 4) {
        moveChange(*row, *col, *row + 1, *col, puzzle);
        *row += 1;
      } else {
        return false;
      }
      break;

    case 'L':
      if(*col > 0) {
        moveChange(*row, *col, *row, *col - 1, puzzle);
        *col -= 1;
      } else {
        return false;
      }
      break;

    case 'R':
      if(*col < 4) {
        moveChange(*row, *col, *row, *col + 1, puzzle);
        *col += 1;
      } else {
        return false;
      }
      break;

    default:
      return false;
  }

  return true;
}

int main(){
  ios::sync_with_stdio(0);cin.tie(0);

  bool exit_end = false;
  int n = 1;
  int num = 3;
  while(!exit_end){
    array<array<char, 5>, 5> puzzle;
    vector<char> moves;
    pair<int, int> empty_pos;

    for(int i = 0; i < 5; i++){
      string line;
      getline(cin, line);

      if(line[0] == 'Z') {
        exit_end = true;
        break;
      }

      for(int j = 0; j < 5; j++) {
        puzzle[i][j] = line[j];

        if(puzzle[i][j] == ' ' || puzzle[i][j] == '\0') {
          empty_pos = make_pair(i, j);
        }
      }
    }

    if(exit_end) break;

    char move;
    while (cin >> move && move != '0') {
      moves.push_back(move);
    }
    cin.ignore();

    bool illegal = true;
    for(char c: moves){
      illegal = puzzleMove(c, empty_pos, puzzle);
      if(illegal == false) break;
    }

    if(n > 1) cout << endl;
    cout << "Puzzle #" << n << ":" << endl;
    if(!illegal) {
      cout << "This puzzle has no final configuration." << endl;
    } else {
      for(int i = 0; i < 5; i++){
        for(int j = 0; j < 5; j++) {
          if (j > 0) cout << " ";

          puzzle[i][j] ? cout << puzzle[i][j] : cout << " ";
        }
        cout << endl;
      }
    }
    n++;
  }
}
```
