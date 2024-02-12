---
title: AGC016B 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-08 17:05:13
update: 2024-02-08 17:20:41
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests//tasks/) [洛谷](https://www.luogu.com.cn/problem/AT_) [Virtual Judge](https://vjudge.net/problem/Atcoder-)

## 简要题意

有 $N$ 只猫，每只猫带着某种颜色的帽子，给出每只猫能看到（即其他 $N-1$ 只猫）的颜色种数 $a_i$ ，问是否可以构造出合法序列。

$N \le 10^5, 1 \le a_i \le N - 1$

## 思路

假设总共有 $k$ 种颜色，那么每只猫所能看到的颜色满足 $k-1 \le a_i \le k$ ，那么先枚举 $k$ ，接下来判断 $k$ 可不可以达到

令 $a_i = k - 1$ 有 $x$ 个， $a_i = k$ 有 $y = n - x$ 个

那么首先少一种颜色的猫一定戴着其他猫没有的颜色，那么必定多出 $x$ 种

没少颜色的猫戴着的帽子颜色至少与其他的一只猫相同，那么要求 $y \not = 1$ ，这里有 $[y > 0] \sim \lfloor \frac{y}{2} \rfloor$ 种

那么只要判断 $k$ 是否在范围内即可

时间复杂度： $O(n)$

## 代码

<https://www.luogu.com.cn/record/146548637>

<https://atcoder.jp/contests/agc016/submissions/50096639>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int n;
    std::cin >> n;

    int min = n, max = 0;
    std::vector<int> a(n);
    for (auto &x : a) {
        std::cin >> x;
        min = std::min(min, x);
        max = std::max(max, x);
    }

    if (max - min > 1) {
        std::cout << "No" << '\n';
        return 0;
    }

    int c0 = 0;
    for (auto x : a) {
        c0 += x == min;
    }

    if (n - c0 != 1 && min + 1 <= c0 + (n - c0 >> 1) && min + 1 >= c0 + (n - c0 > 0)) {
        std::cout << "Yes" << '\n';
        return 0;
    }

    if (min == max && min <= (n >> 1) && min >= 1) {
        std::cout << "Yes" << '\n';
        return 0;
    }

    std::cout << "No" << '\n';

    return 0;
}

```
