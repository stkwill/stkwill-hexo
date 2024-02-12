---
title: AGC015D 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-07 22:28:25
update: 2024-02-07 22:53:40
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc015/tasks/agc015_d) [洛谷](https://www.luogu.com.cn/problem/AT_agc015_d) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc015_d)

## 简要题意

从 $\ge A$ 且 $\le B$ 的整数中选择一个或多个，把这些整数按位或，求一共有多少种可能的结果。

$1\le A\le B \le 2^{60}$

## 思路

特判掉 $A = B$ 情况

**下列数位情况默认为二进制**

首先去除 $A, B$ 相同的最高位

此时 $A,B$ 的首位一定分别为 $0, 1$ ，令 $B$ 首位为 $2^c$

$2^c \sim B$ 的所有数可以表示出 $0 \sim 2^{\lceil \log (B - 2^c + 1) \rceil} - 1$ 的所有数

$A \sim 2^c-1$ 能表示的数比较复杂：

不断枚举 $A$ 中的 $0$ ，此时能表示出 $A$ 到这个 $0$ 的前缀，且此位为 $1$ ，其他位置任意数 的所有数

$2^c$ 位为 $0$ 直接枚举， $2^c$ 为 $1$ 需要考虑两种数的交

时间复杂度： $O(\log B)$

## 代码

<https://atcoder.jp/contests/agc015/submissions/50084805>

<https://www.luogu.com.cn/record/146500631>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int64_t A, B;
    std::cin >> A >> B;

    if (A == B) {
        std::cout << 1 << '\n';
        return 0;
    }
    
    while (std::__lg(A) == std::__lg(B)) {
        A ^= 1ll << std::__lg(A);
        B ^= 1ll << std::__lg(B);
    }

    const int c = std::__lg(B);
    const int d = std::__lg((B ^ 1ll << c) << 1 | 1);

    int64_t ans = 0;

    for (int i = c - 1; i >= 0; --i) {
        if ((A >> i & 1) == 0) {
            ans += 1ll << i;
        }
    }
    ++ans;

    if ((1ll << c) - (1ll << d) & A) {
        ans = ans + ans + (1ll << d);
    } else {
        ans = ans + (1ll << c);
    }

    std::cout << ans << '\n';

    return 0;
}

```
