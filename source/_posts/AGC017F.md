---
title: AGC017F 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-11 22:35:17
update:
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc017/tasks/agc017_f) [洛谷](https://www.luogu.com.cn/problem/AT_agc017_f) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc017_f)

## 简要题意

![](https://cdn.luogu.com.cn/upload/vjudge_pic/AT_agc017_f/5db61944d074543e7a00348b55f3a5b5e0696735.png)

- 给定一个 $N$ 层的三角形图，第 $i$ 层有 $i$ 个节点。
- 第 $i$ 层的节点，从左到右依次标号为 $(i, 1), (i, 2), \ldots , (i, i)$（具体如上图所示）。
- 你需要从 $(1, 1)$ 往下画 $M$ 条折线。
- 对于每条折线的每一个小段，你可以从 $(i, j)$ 画到 $(i + 1, j)$ 或者 $(i + 1, j + 1)$。
- 同时你还必须保证第 $i$ 条折线的任何一个位置必须不能处在第 $i - 1$ 条折线的左侧，它们必须按照从左到右的顺序排列。
- 有 $K$ 条限制，每条限制形如 $(A_i, B_i, C_i)$。
- 表示第 $A_i$ 条折线处于位置 $(B_i, j)$ 时，下一小段必须走向 $(B_i + 1, j + C_i)$，也就是当 $C_i = 0$ 时向左，当 $C_i = 1$ 时向右。
- 询问不同的折线画法的方案数，对 ${10}^9 + 7$ 取模。
- $1 \le N, M \le 20$，$0 \le K \le M (N - 1)$。其它变量在合理范围内。

## 思路

考虑最暴力的一条一条折线 dp，记录在哪些位置向右走，每条线状态 $O(2^N)$ ，总时间复杂度 $O(M4^N)$

观察是否能优化，想到轮廓线 dp 其实 dp 过程与之类似

改成折线一步步走，每次记录当前折线哪些位置向右走，和上一条折线距离多少，上一条折线这一层之后哪些位置向右走

这样状态数 $O(N^2M2^N)$ ，时间复杂度即为状态数

虽然快了很多，但是时间复杂度算一下仍有 $8.38 \times 10^9$ ，显然过不去，考虑是否能少一个 $N$

除了横向距离外，其他东西都不可能省，而且这部分确实有点浪费

改成记录上一条折线的限制与当前折线能走到的位置的交，那么这条轮廓就没有断点了，状态数只有 $O(NM2^N)$

时间复杂度： $O(NM2^K)$

## 代码

<https://atcoder.jp/contests/agc017/submissions/50212703>

<https://www.luogu.com.cn/record/146704407>

```cpp
#include <bits/stdc++.h>

#include <modint.hpp>

using MInt = StK::ModInt<int, 1000000007, int64_t>;

int lowbit(int x) { return x & -x; }
int subbit(int x) { return x & x - 1; }

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int n, m, k;
    std::cin >> n >> m >> k;
    --n;

    std::vector<std::vector<int>> allow(m, std::vector<int>(n, 3));
    for (int i = 0; i < k; ++i) {
        int x, y, c;
        std::cin >> x >> y >> c;
        --x, --y;
        allow[x][y] = 1 << c;
    }

    std::vector<MInt> f(1 << n);
    f[0] = 1;
    for (int x = 0; x < m; ++x) {
        for (int y = 0; y < n; ++y) {
            bool t0 = allow[x][y] & 1, t1 = allow[x][y] >> 1 & 1;
            int tg = (1 << y) - 1;
            std::vector<MInt> g(1 << n);
            for (int i = 0; i < 1 << n; ++i) {
                if (t0 && (i >> y & 1) == 0) {
                    g[i] += f[i];
                }
                if (t1) {
                    g[i >> y & 1 ? i : subbit(i >> y) + 1 << y | i & tg] += f[i];
                }
            }
            f.swap(g);
        }
    }

    MInt ans;
    for (auto x : f) {
        ans += x;
    }

    std::cout << ans << '\n';

    return 0;
}

```
