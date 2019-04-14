---
title: Copy-and-Submit-II
date: 2018-04-22 18:54:09
tags: ACM
---

题目链接 https://nanti.jisuanke.com/t/26220

## Description:

``` c++
// Q.cpp
#include <iostream>
using namespace std;
const long long M = 1000000007;
const long long MAXL = 1000000;
long long a[MAXL];
long long Q(int n, long long t)
{
    if(n < 0) return t;
    return (Q(n - 1, t) + Q(n - 1, (t * a[n]) % M)) % M;
}
int main()
{
    int n;
    while(cin >> n)
    {
        for(int i = 0; i < n; ++i)
        {
            cin >> a[i];
            a[i] %= M;
        }
        cout << Q(n - 1, 1) << endl;
    }
    return 0;
}
```
## Input:

Input consists of several test cases. Each test case begins with an integer n. Then it's followed by n integers a[i]. 

0<n<=1000000

0<=a[i]<=10000

There are 100 test cases at most. The size of input file is less than 48MB. 

## Output:

Maybe you can just copy and submit. Maybe not. 

## 样例输入
```
1
233
1
666
```
### 样例输出
```
234
667
```
## 题解
要求写一个与给定的程序功能一样的程序
观察这个递归，然后发现它是求(1+a1)*(1+a2)*(1+a3)*..*(1+an) ，递推的求就可以了
注意内存大小，不能开数组


```  c++
#include<stdio.h>
typedef long long ll;
ll mod = 1000000007;

int main() {
    ll n;
    while(scanf("%lld",&n)!=EOF){
        ll a=1,fac=1;
        for (int i=1;i<=n;i++){
            scanf("%lld",&a);
            fac=(fac+fac*a%mod)%mod;
        }
        printf("%lld\n",fac);
    }
}
```