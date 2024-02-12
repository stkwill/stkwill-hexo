---
title: AGC015F 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-07 23:43:06
update: 2024-02-08 00:23:44
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc015/tasks/agc015_f) [洛谷](https://www.luogu.com.cn/problem/AT_agc015_f) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc015_f)

## 简要题意

定义 $F(x, y)$ 为执行 $gcd(x, y)$ 所需要的步数. 

$Q \le 3 \times 10^5$ 次询问, 每次询问给定 $X_i, Y_i \le 10^18$, 求满足 $1\leqslant x\leqslant X_i, 1\leqslant y\leqslant Y_i$ 的二元组的 $F(x,y)$ 的最大值和有多少个二元组的 $F(x, y)$ 达到了最大值, 答案对 $10^9 + 7$ 取模.

## 思路

首先第一个答案很明显可以通过最小的对为斐波那契数列这一性质 $O(\log \max(x, y))$ 计算，令答案为 $K$

令 $k$ 层好对 $(x, y)$ 为满足对于任意 $x' \le x, y' \le y, f(x', y') \le f(x, y) = k (x < y)$ 的对子 **（需先排除 $x=y$）**

很显然，满足条件的答案肯定为好对，并且为 $K$ 层好对

好对可以有无限种，只要 $y \gets y + kx (k \in \mathbb{N^*})$ 即可

那么对于这种无限扩展的性质，可以试着通过一轮 gcd 来试着解决

令可以通过一个好对 gcd 一轮得到的 $k$ 层好对为 $k$ 层优对

那么此时 $k$ 层优对 $(i, j)$ 必满足 $i < j \le 2i$

现在又由于此对不比它偏序的所有对答案更劣的性质，可以推出优对数量不多

试着枚举~~（打表）~~ $k$ 较小时的值：

- $1$ 层优对： $(1, 2)$
- $2$ 层优对： $(2, 3), (3, 4)$
- $3$ 层优对： $(3, 5), (4, 7), (5, 7)$
- $4$ 层优对： $(5, 8), (7, 11), (7, 12), (8, 11)$
- $5$ 层优对： $(8, 13), (11, 18), (12, 19), (11, 19), (13, 18)$

可以发现， $k$ 层优对正好有 $k$ 个，其中前 $k-1$ 个由上一层直接继承而来，最后一个 gcd 一轮后为斐波那契数列一项多加一次

那么预处理出所有优对，然后就可以求解第二个答案

时间复杂度： $O(\log^2 \max(x, y))$

## 代码

<https://atcoder.jp/contests/agc015/submissions/50086114>

<https://www.luogu.com.cn/record/146508005>

```cpp
#include <bits/stdc++.h>

#include <modint.hpp>

using MInt = StK::ModInt<int, 1000000007, int64_t>;

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int T = 1;
    std::cin >> T;

    constexpr int64_t V = 1e18;

    std::vector<std::vector<std::pair<int64_t, int64_t>>> a{{}, {{1, 2}}};
    while (true) {
        int n = a.size() - 1;
        if (a[n][0].first + a[n][0].second > V) {
            break;
        }
        a.emplace_back(n + 1);
        for (int i = 0; i < n; ++i) {
            a[n + 1][i] = {a[n][i].second, a[n][i].first + a[n][i].second};
        }
        a[n + 1][n] = {a[n][0].first + a[n][0].second, a[n][0].first + a[n][0].first + a[n][0].second};
    }

    while (T--) [&]() {
        int64_t x, y;
        std::cin >> x >> y;
        if (x > y) {
            std::swap(x, y);
        }
        if (x == 1 || y <= 2) {
            std::cout << 1 << ' ' << MInt().replace(x * y) << '\n';
            return;
        }
        int c = 1;
        while (a[c][0].second <= x && a[c][0].first + a[c][0].second <= y) {
            ++c;
        }
        MInt ans;
        for (int i = 0; i < c; ++i) {
            auto [tx, ty] = a[c][i];
            if (x >= tx && y >= ty) {
                ans += MInt().replace((y - ty) / tx + 1);
            }
            if (x >= ty && y >= tx) {
                ans += MInt().replace((x - ty) / tx + 1);
            }
        }
        std::cout << c << ' ' << ans << '\n';
    }();

    return 0;
}

```
