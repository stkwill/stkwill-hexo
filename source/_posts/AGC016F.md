---
title: AGC016F 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-08 19:31:50
update: 2024-02-09 11:54:39
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc016/tasks/agc016_f) [洛谷](https://www.luogu.com.cn/problem/AT_agc016_f) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc016_f)

## 简要题意

给定一个 $n$ 个点 $m$ 条边的 DAG，对于每条边 $(u,v)$ 都满足 $u<v$，$1,2$ 号点各一个石头，每次可以沿 DAG 上的边移动一颗石头，不能移动则输，求所有 $2^{m}$ 个边的子集中，只保留这个子集先手必胜的方案个数。对 $10^9 + 7$ 取模

注意：两个石头可以重合。

- $2 \le N \le 15$
- $1 \le M \le N(N-1)/2$

## 思路

首先观察博弈情况，注意到 `两个石头可以重合` ，那么两个石头的博弈情况有干扰吗？没有

因此可以分解成两个公平博弈，可以考虑 `SG 函数` 的值

令 $f_i$ 为一个石子处于 $i$ 时 `SG 函数` 的值，那么有 $f_{n} = 0$ ，可以从右往左 dp

先手必胜意味着 $f_{1} \oplus f_{2} \not = 0$ ，不如计算它的相反情况 $f_{1} = f_{2}$

但是 $SG$ 函数的情况总数为 $\sum_i {n \brace i}$ ，显然直接顺序 dp 状态数会超

考虑 dp 的顺序，将状态数设为所有 `SG 函数` $\ge x$ 的位置有哪些，进一步可以发现只要预先计算和这些位置有关边的方案数，就不需要关心 $x$ 的具体值

那么对于一个转移 $S \to S \cup T$ （其中 $S \cap T = \varnothing$ ）

对于 $T$ 内部的边，全部都不能选，对于 $T$ 与 $\left[1,n\right] / S / T$ 之间的边，对于每个没选的点至少要有一条边连向 $T$ 集合

对这部分的每个点和每种集合 $T$ 进行预处理，即可完成 dp

**注意：dp 中只包含 $1$, $2$ 中一个的情况是不合法的**

时间复杂度： $O(3^n n)$

### 优化

时间瓶颈在于枚举边的关系

可以发现的是，对于每个点，若 $T$ 相同，答案不会改变

此时改变预处理的方式，原来预处理只花费了 $O(2^n n)$ 的复杂度，考虑点构成的集合进一步预处理

但是全部预处理复杂度还是有点高，且存储十分麻烦，不如接着做

考虑把点分成大小 $\le S$ 的组，共 $O(\frac{n}{S})$ 组，每组任意集合关于每种 $T$ 的贡献可以预处理

复杂度为 $O((3^n + 2^{n+S})\frac{n}{S})$ ，显然在 $3^n \approx 2^{n+S}$ 附近时最优

此时 $S=(\log_2 3 - 1) n \approx 0.5850 n$ 那么取 $S = \frac{n}{2}$ 可以得到 $O(3^n + (2^{\frac{3}{2}})^n)$ 的复杂度

其中 $3 > 2^{\frac{3}{2}} \approx 2.8284$

时间复杂度： $O(3^n)$ 常数略大

## 代码

<https://atcoder.jp/contests/agc016/submissions/50110027>

<https://www.luogu.com.cn/record/146591323>

```cpp
#include <bits/stdc++.h>

#include <modint.hpp>

using MInt = StK::ModInt<int, 1000000007, int64_t>;

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int n, m;
    std::cin >> n >> m;

    if (n == 2) {
        std::cout << 1 << '\n';
        return 0;
    }

    std::vector<MInt> po2(n + 1);
    po2[0] = 1;
    for (int i = 1; i <= n; ++i) {
        po2[i] = po2[i - 1] * 2;
    }

    std::vector<MInt> ppa(1 << n);
    for (int i = 0; i < 1 << n; ++i) {
        ppa[i] = po2[__builtin_popcount(i)];
    }

    std::vector<int> to(1 << n), fr(1 << n);
    for (int i = 0; i < m; ++i) {
        int u, v;
        std::cin >> u >> v;
        u = n - u, v = n - v;
        to[1 << u] |= 1 << v;
        fr[1 << v] |= 1 << u;
    }

    std::vector<MInt> f(1 << n);
    f[0] = 1;
    for (int i = 1; i < 1 << n; i = i == (1 << n - 2) - 1 ? (1 << n) - (1 << n - 2) : i + 1) {
        for (int j = i; j; --j &= i) if (MInt t = f[i ^ j]) {
            for (int k = (1 << n) - 1 ^ i; k; k &= k - 1) {
                t *= (ppa[j & to[k & -k]] - 1) * ppa[j & fr[k & -k]];
            }
            f[i] += t;
        }
    }

    std::cout << MInt(2).pow(m) - f[(1 << n) - 1] << '\n';

    return 0;
}

```
