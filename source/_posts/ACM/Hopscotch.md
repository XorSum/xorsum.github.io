---
title: Hopscotch
date: 2018-04-21 23:26:39
tags:  ACM
---


时间限制: 5 Sec  内存限制: 128 MB

## 题目描述
You’re playing hopscotch! You start at the origin and your goal is to hop to the lattice point (N, N). A hop consists of going from lattice point (x1, y1) to (x2, y2), where x1 < x2 and y1 < y2.
You dislike making small hops though. You’ve decided that for every hop you make between two lattice points, the x-coordinate must increase by at least X and the y-coordinate must increase by at least Y .
Compute the number of distinct paths you can take between (0, 0) and (N, N) that respect the above constraints. Two paths are distinct if there is some lattice point that you visit in one path which you don’t visit in the other.
Hint: The output involves arithmetic mod 109+ 7. Note that with p a prime like 109+ 7, and x an integer not equal to 0 mod p, then x(xp−2) mod p equals 1 mod p.
## 输入
The input consists of a line of three integers, N X Y . You may assume 1 ≤ X, Y ≤ N ≤ 106.
## 输出
The number of distinct paths you can take between the two lattice points can be very large. Hence output this number modulo 1 000 000 007 (109+ 7).
## 样例输入
```
7 2 3
```
## 样例输出
```
9
```
## 题解
题目大意: 给定三个数`n,x,y`。表示有一个`n*n`的方阵，如果`x1+x<x2`且`y1+y<y2`，那么可以从`(x1,y1)`跳到`(x2,y2)`，问从`(0,0)`跳到`(n,n)`有多少种不同的走法。
先把行列分开考虑。
`a1[i]`表示一列中，从`0`走到`n`使用了`i`步有多少组走法，`a2[i]`表示一行中，从`0`走到`n`使用了`i`步有多少组走法，则使用`i`步从`(0,0)`走到`(n,n)`的方法数为`a1[i]*a2[i]`,所以答案`ans = a1[1]*a2[1]+a1[2]*a2[2]+...a1[i]*a2[i]`。
那么如何求出两个`a`数组呢？我们可以这样考虑，一共有`n`个点，走`i`次，这样相当于把`n`个连续的小球分到`i`个桶里。因为每个桶至少分`x`个球，所以我们可以先给每个桶分`x-1`个球，剩下的`n-(x-1)*i`个球用隔板法分给`i`个人。也就是`a1[i]=C(n-(x-1)*i-1,i-1)`种。

## 代码
``` c++
#include<cstdio>
#include<vector>
#include<algorithm>
#include <iostream>
typedef long long ll;
using namespace std;
ll mod = 1000000007;
const int maxn = 1000010;

ll fac[maxn],inv[maxn];
ll a1[maxn],a2[maxn];

ll qpow(ll a,ll x){
    ll ret=1;
    while (x){
        if (x&1)
            ret = ret*a%mod;
        a=a*a%mod;
        x>>=1;
    }
    return ret;
}

ll init(){
    fac[0]=1;
    for (int i=1;i<maxn;i++)
        fac[i]=fac[i-1]*i%mod;
    inv[maxn-1]=qpow(fac[maxn-1],mod-2);
    for (int i=maxn-2;i>=0;i--)
        inv[i]=inv[i+1]*(i+1)%mod;
    return 0;
}

ll c(ll n,ll m){
    if (n<m) return 0;
    return fac[n]*inv[m]%mod*inv[n-m]%mod;
}

int main() {
    init();
    ll n, x, y;
    scanf("%lld%lld%lld", &n, &x, &y);
    for (int i = 1; i * x <= n; i++) {
        a1[i] = c(n - (x - 1) * i - 1, i - 1);
    }
    for (int i = 1; i * y <= n; i++) {
        a2[i] = c(n - (y - 1) * i - 1, i - 1);
    }
    ll ans = 0;
    for (int i = 1; i * x <= n && i * y <= n; i++)
        ans = (ans + a1[i] * a2[i] % mod) % mod;
    printf("%lld\n", ans);
    return 0;
}
/**************************************************************
    Problem: 5095
    User: DP56
    Language: C++
    Result: 正确
    Time:916 ms
    Memory:32948 kb
****************************************************************/


```



