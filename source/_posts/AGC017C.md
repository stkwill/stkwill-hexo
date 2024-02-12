---
title: AGC017C 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-11 21:14:48
update: 2024-02-11 21:26:15
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc017/tasks/agc017_c) [洛谷](https://www.luogu.com.cn/problem/AT_agc017_c) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc017_c)

## 简要题意

$N$ 个球排在一起，每个球上有一个数 $a_i$。接下来会进行若干轮删除。设现在还有 $k$ 个球，则 $a_i=k$ 的球会被删除。

最终可能球不会被删完，你需要求出最少修改几个球上的数后可以让球全部被删完。

同时还有 $M$ 次修改，每次修改第 $X_i$ 个球的数为 $Y_i$，你需要求出每次修改后上述问题的答案。

- $1\ \le\ N, M\ \le\ 200000$
- $1\ \le\ A_i, X_j, Y_j\ \le\ N$

## 思路

考虑先把特殊的多个球处理掉

把多个同种球拆成多个球，转化为等价操作

例如 `7 7 7` 可以转化为 `5 6 7` ，操作等价的拆成了三份

那么题目可以等价的变为转化后的序列正好为 $1 \sim N$

那么只要数空位即可，容易根据这里的空位与重复构造出操作序列

时间复杂度： $O(n)$

## 代码

<https://atcoder.jp/contests/agc017/submissions/50210086>

<https://www.luogu.com.cn/record/146698380>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int n, m;
    std::cin >> n >> m;

    std::vector<int> a(n);
    for (int i = 0; i < n; ++i) {
        std::cin >> a[i];
        --a[i];
    }

    int empty_pos = n;
    std::vector<int> num(n), pos(n);
    auto pos_inc = [&](int x) {
        if (x >= 0) {
            empty_pos -= !pos[x]++;
        }
    };
    auto pos_dec = [&](int x) {
        if (x >= 0) {
            empty_pos += !--pos[x];
        }
    };
    auto num_inc = [&](int x) {
        pos_inc(x - num[x]++);
    };
    auto num_dec = [&](int x) {
        pos_dec(x - --num[x]);
    };

    for (int i = 0; i < n; ++i) {
        num_inc(a[i]);
    }

    while (m--) {
        int x, y;
        std::cin >> x >> y;
        --x, --y;
        num_dec(a[x]);
        num_inc(a[x] = y);
        std::cout << empty_pos << '\n';
    }
    

    return 0;
}

```
