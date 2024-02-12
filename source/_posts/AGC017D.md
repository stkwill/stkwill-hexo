---
title: AGC017D 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-11 21:26:25
update: 2024-02-11 21:47:44
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc017/tasks/agc017_d) [洛谷](https://www.luogu.com.cn/problem/AT_agc017_d) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc017_d)

## 简要题意

有一棵 $N$ 个节点的树，节点标号为 $1,2,⋯,N$，边用 $(x_i,y_i)$表示。 Alice 和 Bob 在这棵树上玩一个游戏，Alice先手，两人轮流操作：

选择一条树上存在的边，把它断开使树变成两个连通块。然后把不包含 $1$ 号点的联通块删除

当一个玩家不能操作时输，你需要算出：假如两人都按最优策略操作，谁将获胜。

- $2\ \le\ N\ \le\ 100000$
- $1\ \le\ x_i,\ y_i\ \le\ N$

## 思路

考虑整棵树的博弈情况，不难发现根节点的每个子树是互相独立的

由于题设是公平博弈，可直接联想到 `SG` 函数，总函数为每个独立博弈的 `SG` 函数异或和

现在考虑在当前子树上加一条边， `SG` 函数会如何变化

打表可以发现变化为在原有基础上加一

**引理一：在子树根节点向加一条边， `SG` 函数值加一**
- 证明：
- 设原 `SG` 函数为 $x$ ，那么根据定义（ `SG` 函数值为能走一步到达的所有情况的 `SG` 函数的 `MEX`），$0 \sim x-1$ 的所有函数值均能到达，且无法到达 $x$
- 尝试使用第二数学归纳法，假设所有节点数更少的情况均满足题设，则 $1 \sum x$ 的情况均能到达，且无法到达 $x+1$
- 断掉新连的边后为 $\mathop{SG} = 0$ 的情况，结合起来就是 $\mathop{MEX} = x+1$

由上述性质可以直接数位 dp 得出答案

时间复杂度： $O(n)$

## 代码

<https://atcoder.jp/contests/agc017/submissions/50210692>

<https://www.luogu.com.cn/record/146700015>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int n;
    std::cin >> n;

    std::vector<std::vector<int>> to(n);
    for (int i = 1; i < n; ++i) {
        int u, v;
        std::cin >> u >> v;
        --u, --v;
        to[u].push_back(v);
        to[v].push_back(u);
    }

    auto _F_dfs = [&](auto self, int u, int fa) -> int {
        int x = 0;
        for (auto v : to[u]) if (v != fa) {
            x ^= self(self, v, u) + 1;
        }
        return x;
    };

    std::cout << (_F_dfs(_F_dfs, 0, 0) ? "Alice" : "Bob") << '\n';

    return 0;
}

```
