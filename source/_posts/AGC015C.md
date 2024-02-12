---
title: AGC015C 题解
top: 50
mathjax: true
categories:
  - solution
  - AtCoder
tags:
  - solution
  - AtCoder
date: 2024-02-07 22:18:47
update: 2024-02-07 22:18:47
---

[导航 - 题解](/guide-solution/)

**题面链接：** [AtCoder](https://atcoder.jp/contests/agc015/tasks/agc015_c) [洛谷](https://www.luogu.com.cn/problem/AT_agc015_c) [Virtual Judge](https://vjudge.net/problem/Atcoder-agc015_c)

## 简要题意

$Nuske$ 现在有一个$N*M(N,M<=2000)$ 的矩阵$S$ , 若$S_i,j=1$ , 那么该处为蓝色, 否则为白色, 保证所有蓝色格子构成的连通块都是树.  
给出$Q(Q<=200000)$ 次询问, 每次询问一个子矩阵中蓝色连通块的个数  

## 思路

无环连通块个数 = 点 - 边

二位前缀和三遍即可

## 代码

<https://atcoder.jp/contests/agc015/submissions/50084143>

<https://www.luogu.com.cn/record/146500631>

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios::sync_with_stdio(false);
    std::cin.tie(nullptr), std::cout.tie(nullptr);

    int n, m, q;
    std::cin >> n >> m >> q;

    std::vector<std::vector<int>> tp(n + 2, std::vector<int>(m + 2)), tx(tp), ty(tp);
    for (int i = 1; i <= n; ++i) {
        std::string s;
        std::cin >> s;
        for (int j = 0; j < m; ++j) {
            tp[i][j + 1] = s[j] - '0';
        }
    }

    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            tx[i][j] = tp[i + 1][j] && tp[i][j];
            ty[i][j] = tp[i][j + 1] && tp[i][j];
        }
    }
    
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            tp[i][j] += tp[i - 1][j];
            tx[i][j] += tx[i - 1][j];
            ty[i][j] += ty[i - 1][j];
        }
    }

    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            tp[i][j] += tp[i][j - 1];
            tx[i][j] += tx[i][j - 1];
            ty[i][j] += ty[i][j - 1];
        }
    }

    for (int qi = 1; qi <= q; ++qi) {
        int ax, ay, bx, by;
        std::cin >> ax >> ay >> bx >> by;
        --ax, --ay;
        int cx = bx - 1, cy = by - 1;
        std::cout << (tp[ax][ay] - tp[ax][by] - tp[bx][ay] + tp[bx][by]) - 
                     (tx[ax][ay] - tx[ax][by] - tx[cx][ay] + tx[cx][by]) - 
                     (ty[ax][ay] - ty[ax][cy] - ty[bx][ay] + ty[bx][cy]) << '\n';
    }

    return 0;
}

```

