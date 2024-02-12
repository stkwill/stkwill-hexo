---
title: AGC016A 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-08 16:57:27
update: 2024-02-08 17:02:26
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc016/tasks/agc016_a) [洛谷](https://www.luogu.com.cn/problem/AT_agc016_a) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc016_a)

## 简要题意

给出一个字符串，每次操作可以使得字符串缩短一位，新的字符串的第i位可以是原字符串的第i或者第i+1位。
问使得整个字符串全相同最少的操作次数

## 思路

枚举最后字符串相同的颜色

那么一步操作相当于所有这种颜色向左扩展一位，将末尾后一个元素视为任意颜色

取最小值即可

时间复杂度： $O(n + C)$

## 代码

<https://atcoder.jp/contests/agc016/submissions/50096379>

<https://www.luogu.com.cn/record/146546998>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    std::string s;
    std::cin >> s;

    constexpr char CHAR_BEG = 'a';
    constexpr int C = 26;

    std::array<std::vector<int>, C> a;
    for (int i = 0; i < s.size(); ++i) {
        a[s[i] -= CHAR_BEG].push_back(i);
    }
    
    int ans = s.size();
    for (int i = 0; i < C; ++i) {
        a[i].push_back(s.size());
        int t = -1, max = 0;
        for (auto x : a[i]) {
            max = std::max(max, x - t - 1);
            t = x;
        }
        ans = std::min(ans, max);
    }

    std::cout << ans << '\n';

    return 0;
}

```
