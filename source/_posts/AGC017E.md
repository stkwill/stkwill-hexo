---
title: AGC017E 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-11 21:54:09
update:
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc017/tasks/agc017_e) [洛谷](https://www.luogu.com.cn/problem/AT_agc017_e) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc017_e)

## 简要题意

你有 $N$ 块拼图,每块拼图分为左 中 右三个部分,其中中间部分高度恒为 $H$ ,左右部分的形状将由 $A_i,B_i,C_i,D_i$ 指定, $A_i,B_i$ 指定左右部分长度, $C_i,D_i$ 指定左右部分离地高度.

现在,你需要将这 $N$ 块拼图拼成一条直线,使得每块拼图中间部分接地,左右部分不悬空。

![](https://cdn.luogu.com.cn/upload/vjudge_pic/AT_agc017_e/085913e2ea706e9f5a234d65bf3ad02f7f07f135.png)

- $1\ \le\ N\ \le\ 100000$
- $1\ \le\ A_i + C_i, B_i + D_i\ \le\ H\ \le\ 200$

## 思路

考虑拼接的位置一定由一个接地和一个悬空组成，并且可以拼接

那么把拼接出的状态记为左 $D_i=0,B_i$ 或右 $C_i=0,A_i$

把这些状态作为节点对每块拼图连边，每条路径必须从 $C_i=0$ 开始，到 $D_i=0$ 结束

需要判定欧拉路径拆分，那么即为每个连通块至少有一个入度出度不同点，度数不同点必须为对应的出度或入度更大

时间复杂度： $O(N + H\mathop{\alpha}(H))$

## 代码

<https://atcoder.jp/contests/agc017/submissions/50211792>

<https://www.luogu.com.cn/record/146702548>

```cpp
#include <bits/stdc++.h>

#include <dsu.hpp>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int n, h;
    std::cin >> n >> h;
    
    std::vector<int> din(h + h), dout(h + h);
    StK::DSU dsu(h + h);
    
    auto get_idl = [&](int x, int y) {
        return y ? h + y - 1 : x - 1;
    };
    auto get_idr = [&](int x, int y) {
        return y ? y - 1 : h + x - 1;
    };
    auto link = [&](int u, int v) {
        ++dout[u], ++din[v];
        dsu.merge(u, v);
    };

    for (int i = 0; i < n; ++i) {
        int a, b, c, d;
        std::cin >> a >> b >> c >> d;
        link(get_idl(a, c), get_idr(b, d));
    }

    std::vector<int> dif(h + h);
    for (int i = 0; i < h + h; ++i) if (din[i] || dout[i]) {
        int t = dsu(i);
        if (din[i] != dout[i]) {
            if ((dout[i] > din[i]) != (i < h)) {
                std::cout << "NO" << '\n';
                return 0;
            }
            dif[t] = true;
        }
    }

    for (int i = 0; i < h + h; ++i) if ((din[i] || dout[i]) && dsu(i) == i && !dif[i]) {
        std::cout << "NO" << '\n';
        return 0;
    }

    std::cout << "YES" << '\n';

    return 0;
}

```
