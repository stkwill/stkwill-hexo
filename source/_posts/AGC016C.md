---
title: AGC016C 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-08 17:21:58
update: 2024-02-08 17:55:40
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc016/tasks/agc016_c) [洛谷](https://www.luogu.com.cn/problem/AT_agc016_c) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc016_c)

## 简要题意

给定整数 $H,W,h,w$，你需要判断是否存在满足如下条件的矩阵，如果存在，则输出任意一种可能的方案。
- 矩阵是 $H$ 行 $W$ 列
- 矩阵的每个元素的权值在 $[-10^9,10^9]$ 之间
- 矩阵的所有元素权值和为正
- 任意大小为 $h \times w$ 的子矩阵的元素权值和为负

- $1 \le h \le H \le 500$
- $1 \le w \le W \le 500$

## 思路

考虑什么时候不存在答案

一种很显然的情况：当 $h$ 整除 $H$ 且 $w$ 整除 $W$ 时，可以将大矩阵不重不漏划分为若干小矩阵，由于每个小矩阵权值和非负，那么大矩阵权值和显然为负

进一步讨论其他情况，还是把矩阵向左上角对齐划分，右边下边显然会有一些边角料，那么这些区域的和必须为正

考虑到一个数最多可以给 $h \times w$ 个矩阵贡献，那么我们考虑把负数放在所有完整小矩阵的最右下角，以达到效率最大化

令所有正数为 $x$ ，那么为满足矩阵贡献为负的条件，所有负数 $y < -(wh - 1)x$ ，将其设为 $y = -(wh - 1)x - 1$ 不会比其他更劣

所有负数共有 $Y = \lfloor \frac{H}{h} \rfloor \times \lfloor \frac{W}{w} \rfloor$ 个 ，正数有 $X = HW - Y$ 个

大矩阵要求总和为正，即为 $xX + yY > 0$ ，展开为

$$
x(HW - Y) + (-(wh - 1)x - 1)Y > 0
$$

$$
xHW > (whx + 1) Y
$$

$$
(HW - whY)x > Y
$$

由于 $h$ 整除 $H$ 与 $w$ 整除 $W$ 至少有一个不成立，有 $HW > whY$

所以

$$
x \ge \lceil \frac{Y + 1}{HW - whY} \rceil
$$

时答案成立

显然上式取等号时 $0 < x \le \frac{Y + 1}{\max(W, H)} \le \frac{1}{2} \max(W, H) + 1 \le 251$ ，$0 > y \ge -whx \ge -\max{W, H} \ge -500$ 在题设范围之内

## 代码

<https://atcoder.jp/contests/agc016/submissions/me>

<https://www.luogu.com.cn/record/146551676>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int H, W, h, w;
    std::cin >> H >> W >> h >> w;

    if (H % h == 0 && W % w == 0) {
        std::cout << "No" << '\n';
        return 0;
    }

    int Y = (H / h) * (W / w);
    int x_div = H * W - h * w * Y;
    int x = Y / x_div + 1;
    int y = -(w * h - 1) * x - 1;

    std::cout << "Yes" << '\n';
    for (int i = 1; i <= H; ++i) {
        for (int j = 1; j <= W; ++j) {
            std::cout << (i % h == 0 && j % w == 0 ? y : x) << " \n"[j == W];
        }
    }

    return 0;
}

```
