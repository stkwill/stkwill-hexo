---
title: AGC017B 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-11 21:03:49
update: 2024-02-11 21:12:32
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc017/tasks/agc017_b) [洛谷](https://www.luogu.com.cn/problem/AT_agc017_b) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc017_b)

## 简要题意

有 $N$ 个格子排成一排，最左边的里面写了数字 $A$ ，最右边的写了数字 $B$ ，中间的 格子都是空的。 你需要在中间的每个格子里填上一个数字，使得这个序列中，任意相邻两个数 的差的绝对值在 $\left[C,D\right]$ 之间。 问是否存在这样一种可行的填数方案，输出 `YES` 或者 `NO`

- $3\ \le\ N\ \le\ 500000$
- $0\ \le\ A\ \le\ 10^9$
- $0\ \le\ B\ \le\ 10^9$
- $0\ \le\ C\ \le\ D\ \le\ 10^9$

## 思路

枚举相邻两个数之差为 $\left[C, D\right]$ 或 $\left[-D, -C\right]$ 各有几次

然后可直接得出 $B-A$ 范围，判断即可

时间复杂度： $O(n)$

## 代码

<https://atcoder.jp/contests/agc017/submissions/50209729>

<https://www.luogu.com.cn/record/146697473>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int64_t N, A, B, C, D;
    std::cin >> N >> A >> B >> C >> D;
    --N;
    
    for (int i = 0; i <= N; ++i) {
        if (B - A >= C * i - D * (N - i) && B - A <= D * i - C * (N - i)) {
            std::cout << "YES" << '\n';
            return 0;
        }
    }

    std::cout << "NO" << '\n';

    return 0;
}

```
