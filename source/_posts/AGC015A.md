---
title: AGC015A 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-07 22:00:28
update: 2024-02-07 22:00:28
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc015/tasks/agc015_a) [洛谷](https://www.luogu.com.cn/problem/AT_agc015_a) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc015_a)

## 简要题意

你有 $N$ 个数，最小为 $A$ ，最大为 $B$ ，求这些数的总和的个数。

## 思路

特殊讨论 $A > B$, $N = 1$ 即可

时间复杂度： $O(1)$

## 代码

<https://atcoder.jp/contests/agc015/submissions/50083742>

<https://www.luogu.com.cn/record/146498944>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int64_t n, a, b;
    std::cin >> n >> a >> b;

    if (a > b) {
        std::cout << 0 << '\n';
    } else if (n == 1) {
        std::cout << (a == b) << '\n';
    } else {
        std::cout << (b - a) * (n - 2) + 1 << '\n';
    }

    return 0;
}

```
