---
title: AGC016D 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-08 17:56:54
update: 2024-02-08 18:22:16
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc016/tasks/agc016_d) [洛谷](https://www.luogu.com.cn/problem/AT_agc016_d) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc016_d)x

## 简要题意

一个序列，一次操作可以将某个位置变成整个序列的异或和。 问最少几步到达目标序列。

$N \le 10^5$

## 思路

先将所有序列末尾添加一个值为原序列异或和的数 $e$

**注意：后面所有序列默认已加这个数**

那么简单计算可以发现操作相当于交换 $e$ 和其左边某一个数

那么有解当且仅当目标序列是起始序列的一个排列

考虑离散化以后将起始序列和目标序列对应位置的值之间连一条边（自环不连）

那么一个不包括 $e$ 的大小为 $c$ 的置换环至少需要 $c+1$ 步回归原始位置，包括 $e$ 的需要 $c-1$ 步

又由于同一个连通块内一定有欧拉回路

那么答案为对应位置不同的个数 $+$ 图中连通块个数 $- \left[ a_n \not = b_n \right] - $ $a_n$ 不在任意一个连通块内

使用并查集计算

时间复杂度： $O(n (\log n + \alpha(n)))$

## 代码

<https://atcoder.jp/contests/agc016/submissions/50097703>

<https://www.luogu.com.cn/record/146553755>

```cpp
#include <bits/stdc++.h>

#include <dsu.hpp>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int n;
    std::cin >> n;

    std::vector<int> a(n + 1), b(n + 1);
    for (int i = 0; i < n; ++i) {
        std::cin >> a[i];
        a[n] ^= a[i];
    }
    for (int i = 0; i < n; ++i) {
        std::cin >> b[i];
        b[n] ^= b[i];
    }

    auto c = a;
    {
        auto d = b;
        std::sort(c.begin(), c.end());
        std::sort(d.begin(), d.end());
        for (int i = 0; i <= n; ++i) {
            if (c[i] != d[i]) {
                std::cout << -1 << '\n';
                return 0;
            }
        }
    }
    c.erase(std::unique(c.begin(), c.end()), c.end());
    
    int m = c.size();
    for (auto &x : a) {
        x = std::lower_bound(c.begin(), c.end(), x) - c.begin();
    }
    for (auto &x : b) {
        x = std::lower_bound(c.begin(), c.end(), x) - c.begin();
    }

    std::vector<int> used(m);
    StK::DSU dsu(m);
    int ans = 0;
    for (int i = 0; i <= n; ++i) {
        if (a[i] != b[i]) {
            ++ans;
            used[a[i]] = used[b[i]] = 1;
            dsu.merge(a[i], b[i]);
        }
    }

    for (int i = 0; i < m; ++i) {
        ans += used[i] && dsu(i) == i;
    }

    ans -= (a[n] != b[n]) + used[a[n]];

    std::cout << ans << '\n';

    return 0;
}

```
