---
title: AGC016E 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-08 18:23:08
update: 2024-02-08 19:22:37
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc016/tasks/agc016_e) [洛谷](https://www.luogu.com.cn/problem/AT_agc016_e) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc016_e)

## 简要题意

有 $N(2<=N<=400)$ 只火鸡, 编号为 $1$ 到 $N$ , 有 $M(1<=M<=10^5)$ 个人, 每人指定了两只火鸡 $x$ 和 $y$ .  

1. 若 $x$ 和 $y$ 都活着, 那么这个人将会等概率地随机吃掉一只  
2. 若 $x$ 和 $y$ 恰好活着一只, 那么这个人将会吃掉活着的这只  
3. 若 $x$ 和 $y$ 都已经死亡, 那么只好什么都不做  

注意，第 $1$ 个人到第 $M$ 个人每个人依次行动  

求有多少个 $(i,j)(1<=i<j<=N)$ 满足在最终时刻第 $i$ 只火鸡和第 $j$ 只火鸡**可能**都还活着

## 思路

先分析下每个人的行动

对 $(x,y)$ 行动一次意味着除非它们都死亡，存活数目减一

随便手玩几组数据，发现情况十分复杂，几乎无法讨论

尝试倒着考虑

如果要求 $x$ 最终存活，那么对于一个操作 $(x, y)$ ，要求这个操作前 $x, y$ 均 存活

这个看起来方便量化，那么去考虑传递性，即如果要求 $x$ 存活，需要要求哪些点在要求 $x$ 存活前必须存活，记为集合 $S_x$

显然，开始时 $S_x = \left\{ x \right\}$

那么此时顺序考虑每个操作 $(x, y)$ ，那么如果之中有点在操作之后还存活，那么两个点在操作前都必须存活

存在一种情况使得两个点在操作之前存活的充要条件是： $x, y$ 可能存活且 $S_x \cap S_y = \varnothing$

那么操作之后 $S_x' = S_y' = S_x \cup S_y$

显然最后 $(i,j)$ 可能都存活的条件为： $i, j$ 可能存活且 $S_i \cap S_j = \varnothing$

时间复杂度：

- 正常做法： $O(nm + n^3)$
- 使用 `bitset` 优化： $O(\frac{n}{w}(m + n^2))$

## 代码

<https://atcoder.jp/contests/agc016/submissions/50098640>

<https://www.luogu.com.cn/record/146557907>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int n, m;
    std::cin >> n >> m;

    std::vector<int> ban(n);
    std::vector<std::vector<int>> need(n, std::vector<int>(n));
    for (int i = 0; i < n; ++i) {
        need[i][i] = 1;
    }
    for (int i = 0; i < m; ++i) {
        int u, v;
        std::cin >> u >> v;
        --u, --v;
        if (ban[u] || ban[v]) {
            ban[u] = ban[v] = 1;
            continue;
        }
        for (int j = 0; j < n; ++j) {
            if (need[u][j] && need[v][j]) {
                ban[u] = ban[v] = 1;
                break;
            }
            need[u][j] = need[v][j] = need[u][j] + need[v][j];
        }
    }

    int ans = 0;
    for (int i = 0; i < n; ++i) if (!ban[i]) {
        for (int j = i + 1; j < n; ++j) if (!ban[j]) {
            ++ans;
            for (int k = 0; k < n; ++k) {
                if (need[i][k] && need[j][k]) {
                    --ans;
                    break;
                }
            }
        }
    }

    std::cout << ans << '\n';

    return 0;
}

```
