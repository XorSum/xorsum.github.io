---
title: 第k大数_二分法
date: 2017-11-29 22:49:59
tags: ACM
---

时间限制: 10 Sec  内存限制: 128 MB
## 题目描述
有两个序列a，b，它们的长度分别为n和m，那么将两个序列中的元素对应相乘后得到的n*m个元素从大到小排列后的第k个元素是什么？

## 输入
输入的第一行为一个正整数T (T<=10)，代表一共有T组测试数据。

每组测试数据的第一行有三个正整数n，m和k(1<=n, m<=100000,1<=k<=n*m)，分别代表a序列的长度，b序列的长度，以及所求元素的下标。第二行为n个正整数代表序列a。第三行为m个正整数代表序列b。序列中所有元素的大小满足[1,100000]。

## 输出
对于每组测试数据，输出一行包含一个整数代表第k大的元素是多少。

## 样例输入
```
3
3 2 3
1 2 3
1 2
2 2 1
1 1
1 1
2 2 4
1 1
1 1
```
## 样例输出
```
3
1
1
```

## 来源
首届全国中医药院校大学生程序设计竞赛


## 分析

用二分法，每次猜一个数x，求出有多少个数比x大，直到有k个时停止
最后有可能会大1，所以特判一下

## 代码
```
#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;

typedef long long ll;
const int MAXN = 100010;
ll a[MAXN],b[MAXN];
ll n,m,k,t;

ll check(ll x)
{
    ll cnt=0,j=0;
    for (int i=n-1;i>=0;i--)
    {
        while(a[i]*b[j]<x&&j<m-1) j++;
        if (a[i]*b[j]>=x) cnt+=m-j;
    }
    return cnt;
}
int main()
{
    #ifndef ONLINE_JUDGE
        freopen("in.txt","r",stdin);
    #endif // ONLINE_JUDGE

    scanf("%lld",&t);
    while(t--)
    {
        scanf("%lld%lld%lld",&n,&m,&k);
        for (int i=0;i<n;i++) scanf("%lld",&a[i]);
        for (int i=0;i<m;i++) scanf("%lld",&b[i]);
        sort(a,a+n);
        sort(b,b+m);
        ll l=a[0]*b[0],r=a[n-1]*b[m-1];
        while (l<r)
        {
            ll mid=(l+r)/2;
            if (check(mid)>=k) l=mid+1;
            else r=mid-1;
        }
        while (check(l)<k) l--;
        printf("%lld\n",l);
    }
    return 0;
}
```