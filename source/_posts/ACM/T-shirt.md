---
title: T-shirt
date: 2018-06-06 15:46:37
tags: ACM
---


时间限制: 1 Sec  内存限制: 64 MB

## 题目描述
JSZKC is going to spend his vacation! 
His vacation has N days. Each day, he can choose a T-shirt to wear. Obviously, he doesn’t want to wear a singer color T-shirt since others will consider he has worn one T-shirt all the time. 
To avoid this problem, he has M different T-shirt with different color. If he wears A color T-shirt this day and B color T-shirt the next day, then he will get the pleasure of `f[A][B]`.(notice: He is able to wear one T-shirt in two continuous days but may get a low pleasure) 
Please calculate the max pleasure he can get. 
## 输入
The input file contains several test cases, each of them as described below. 
The first line of the input contains two integers N,M (2 ≤ N≤ 100000, 1 ≤ M≤ 100), giving the length of vacation and the T-shirts that JSZKC has.   
The next follows M lines with each line M integers. The jth integer in the ith line means `f[i][j](1<=f[i][j]<=1000000)`. 
There are no more than 10 test cases. 
## 输出
One line per case, an integer indicates the answer 
## 样例输入
```
3 2
0 1
1 0
4 3
1 2 3
1 2 3
1 2 3
```
## 样例输出
```
2
9
```

直接套矩阵快速幂的板子就行

``` c++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>
#include <cctype>
#include <queue>
#include <unordered_map>
#include <set>
#include <map>
 
typedef long long ll;
using namespace std;
const ll maxn = 110;
const ll mod = 1e9+7;

struct Mar{
    ll f[maxn][maxn];
    ll m;
    Mar(ll mm){
        m = mm;
        memset(f,0,sizeof f);
    }
    Mar operator*(const  Mar& b){
        Mar ret(m);
        for (int i=1;i<=m;i++){
            for (int j=1;j<=m;j++){
                for (int k=1;k<=m;k++){
                    ret.f[i][j]=max(ret.f[i][j],f[i][k]+b.f[k][j]);
                }
            }
        }
        return ret;
    }
    Mar pow(ll x){
        Mar ret(m);
        Mar a = *this;
        while(x){
            if (x&1ll)
                ret = ret*a;
            a=a*a;
            x>>=1;
        }
        return ret;
    }
};
 
int main(){
    std::ios::sync_with_stdio(false);
    std::cin.tie();
    ll k,m;
    while(cin>>k>>m){
        Mar f(m);
        for (int i=1;i<=m;i++)
            for (int j=1;j<=m;j++)
                cin>>f.f[i][j];
        f = f.pow(k-1);
        ll ans = 0;
        for (int i=1;i<=m;i++)
            for (int j=1;j<=m;j++)
                ans = max(ans,f.f[i][j]);
        cout<<ans<<endl;
    }
    return 0;
}

/**************************************************************
    Problem: 7561
    User: WC011
    Language: C++
    Result: 正确
    Time:416 ms
    Memory:2036 kb
****************************************************************/
```