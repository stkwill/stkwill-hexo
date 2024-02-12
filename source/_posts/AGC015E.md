---
title: AGC015E 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-07 23:04:34
update: 2024-02-07 23:40:57
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc015/tasks/agc015_e) [洛谷](https://www.luogu.com.cn/problem/AT_agc015_e) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc015_e)

## 简要题意

数轴上有$N$个点，每个点初始时在位置$X_i$，以$V_i$的速度向数轴正方向前进

初始时刻，你可以选择一些点为其染色，之后的行走过程中，染色的点会将其碰到的所有点都染上色，之后被染上色的点亦是如此

在所有$2^N$种初始染色方案中，问有多少种初始染色方案，能使得最终所有的点都被染色？答案对$10^9+7$取模

## 思路

先将点按 $X_i$ 从小到大排序，**之后所有下标均按已排序过的考虑**

考虑被染色的人的集合是否关于起始染色集合独立

**引理一：令初始集合为 $S$ ，最终染色的集合为 $f(S)$ ， $f(S) \cup f(T) = f(S \cup T)$ 一定成立**

证明：易知 $f(f(S)) = f(S)$ ，那么若 $x \not \in f(S \cup T)$ ，染 $x$ 的点 $x'$ 也满足 $x' \not \in f(S \cup T)$ ，如此递归，可以求出不存在一个点属于 $S \cup T$，经过一串染色可以染到 $x$ ，矛盾

那么考虑每个点 $u$ ，首先朝向 $u$ 走的点肯定染色，被 $u$ 追上的也会被染色

那么对于所有满足 $i \le u$, $V_i > \underset{j \ge u}{\mathrm{min}}\ V_j$ 的点会被染色

同样对于所有满足 $i \ge u$, $V_i < \underset{j \le u}{\mathrm{max}}\ V_j$ 的点会被染色

那么对于一个点 $u$ ，上一个染色的点为 $j$ ，下一个染色的点为 $i$ ，可以直接确定是否被染色

而且对于确定 $i$ ，有 $j$ 确下界，下界之上均满足条件，且下界随 $i$ 单调不降

可以直接 dp 上一个初始染色点

时间复杂度 $O(n \log n)$

## 代码

<https://atcoder.jp/contests/agc015/tasks/agc015_e>

<https://www.luogu.com.cn/record/146506424>

```cpp
#include <bits/stdc++.h>

#include <modint.hpp>

using MInt = StK::ModInt<int, 1000000007, int64_t>;

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int n;
    std::cin >> n;

    std::vector<int> v(n + 2);
    {
        std::vector<std::pair<int, int>> a(n);
        for (auto &[x, v] : a) {
            std::cin >> x >> v;
        }

        std::sort(a.begin(), a.end());

        for (int i = 0; i < n; ++i) {
            v[i + 1] = a[i].second;
        }
    }

    std::vector<int> lmax(n + 2), rmin(n + 2);
    v[0] = lmax[0] = INT32_MIN;
    for (int i = 1; i <= n + 1; ++i) {
        lmax[i] = std::max(lmax[i - 1], v[i]);
    }
    v[n + 1] = rmin[n + 1] = INT32_MAX;
    for (int i = n; i >= 0; --i) {
        rmin[i] = std::min(rmin[i + 1], v[i]);
    }

    std::vector<MInt> f(n + 2);
    f[0] = 1;
    MInt sum = 1;
    std::multiset<int> set;
    for (int i = 1, j = 0; i <= n + 1; ++i) {
        while ([&]() -> bool {
            auto it = set.lower_bound(lmax[j]);
            return it != set.end() && *it <= rmin[i];
        }()) {
            sum -= f[j];
            set.erase(set.find(v[++j]));
        }
        f[i] = sum;
        sum += f[i];
        set.insert(v[i]);
    }

    std::cout << f.back() << '\n';

    return 0;
}

```
