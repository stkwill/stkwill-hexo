---
title: AGC017A 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-11 20:39:14
update: 2024-02-11 21:03:12
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc017/tasks/agc017_a) [洛谷](https://www.luogu.com.cn/problem/AT_agc017_a) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc017_a)

## 简要题意

有 $N$ 包饼干，第 $i$ 包里有 $a_i$ 个饼干，你将会选择一些包饼干然后将所有你选择的包里面的饼干吃掉（当然你也可以一包也不选），你想要选择一些包饼干使得包里面饼干的总数在模 $2$ 意义下与 $P$ 同余，问有多少种方法？

- $1\ \le\ N\ \le\ 50$
- $P\ =\ 0,\ 1$

## 思路

dp 前 $i$ 个饼干模 $2$ 为 $c$ 方案数

时间复杂度： $O(n)$

## 代码

<https://atcoder.jp/contests/agc017/submissions/50209487>

<https://www.luogu.com.cn/record/146696873>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int n, P;
    std::cin >> n >> P;

    int64_t f0 = 1, f1 = 0;
    for (int i = 0; i < n; ++i) {
        int x;
        std::cin >> x;
        int t0 = 1 + (x & 1 ^ 1), t1 = x & 1;
        std::tie(f0, f1) = std::make_pair(
            f0 * t0 + f1 * t1,
            f1 * t0 + f0 * t1
        );
    }

    std::cout << (P ? f1 : f0) << '\n';

    return 0;
}

```
