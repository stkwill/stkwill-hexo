---
title: AGC015B 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-07 22:07:22
update: 2024-02-07 22:07:22
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc015/tasks/agc015_b) [洛谷](https://www.luogu.com.cn/problem/AT_agc015_b) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc015_b)

## 简要题意

给定一个长度为 $n(n≤10^5)$ 的字符串，每个字符为$U$ 或 $D$。第 $i$ 位上的字符表示，从第 $i$ 楼乘电梯**出发只能**向上($U$)或向下($D$)。

定义$f(u,v)$表示从第 $u$ 楼到第 $v$ 楼至少需要乘电梯的次数。

求$\sum_{i=1}^n \sum_{j\ne i}^n f(i,j)$。

## 思路

电梯最多乘两次，而且必定可以折返

考虑从每个点开始的贡献

时间复杂度： $O(n)$

## 代码

<https://atcoder.jp/contests/agc015/submissions/50083886>

<https://www.luogu.com.cn/record/146499647>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    std::string s;
    std::cin >> s;

    const int n = s.length();

    int64_t ans = n * (n - 1ll);
    for (int i = 0; i < n; ++i) {
        ans += s[i] == 'U' ? i : n - 1 - i;
    }

    std::cout << ans << '\n';

    return 0;
}

```

