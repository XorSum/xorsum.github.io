---
title: Connect-Three
date: 2018-12-24 14:50:58
tags: ACM
---

[http://codeforces.com/contest/1087/problem/C](http://codeforces.com/contest/1087/problem/C)

The Squareland national forest is divided into equal 1×1 square plots aligned with north-south and east-west directions. Each plot can be uniquely described by integer Cartesian coordinates (𝑥,𝑦) of its south-west corner.

Three friends, Alice, Bob, and Charlie are going to buy three distinct plots of land 𝐴,𝐵,𝐶 in the forest. Initially, all plots in the forest (including the plots 𝐴,𝐵,𝐶) are covered by trees. The friends want to visit each other, so they want to clean some of the plots from trees. After cleaning, one should be able to reach any of the plots 𝐴,𝐵,𝐶 from any other one of those by moving through adjacent cleared plots. Two plots are adjacent if they share a side.

For example, 𝐴=(0,0), 𝐵=(1,1), 𝐶=(2,2). The minimal number of plots to be cleared is 5. One of the ways to do it is shown with the gray color.
Of course, the friends don't want to strain too much. Help them find out the smallest number of plots they need to clean from trees.

### Input

The first line contains two integers 𝑥𝐴 and 𝑦𝐴 — coordinates of the plot 𝐴 (0≤𝑥𝐴,𝑦𝐴≤1000). The following two lines describe coordinates (𝑥𝐵,𝑦𝐵) and (𝑥𝐶,𝑦𝐶) of plots 𝐵 and 𝐶 respectively in the same format (0≤𝑥𝐵,𝑦𝐵,𝑥𝐶,𝑦𝐶≤1000). It is guaranteed that all three plots are distinct.

### Output

On the first line print a single integer 𝑘 — the smallest number of plots needed to be cleaned from trees. The following 𝑘 lines should contain coordinates of all plots needed to be cleaned. All 𝑘 plots should be distinct. You can output the plots in any order.

If there are multiple solutions, print any of them.

### Examples

input
```
0 0
1 1
2 2
```
output
```
5
0 0
1 0
1 1
1 2
2 2
```
input
```
0 0
2 0
1 1
```
output
```
4
0 0
1 0
1 1
2 0
```

## 题解

先只看横坐标，把三个x值从左到右分别叫做Xl,Xm,Xr,，因为要把三个点连起来而且代价尽量小，所以我们只需要从xl到xr连起来就可以了，而且每列只使用一个点，外面的区间就不需要点了。同理，纵坐标也是从Yh到Yl连起来。

所以，我们画三个线段。第一个是(Xm,Yl)到(Xm,Yh),这一条竖线能把三个点的纵向的连起来。为了横向的连起来，还要画两个横线，记Xl,Xr对应的Y值分别是Yl,Yr,那我们就可以画出(xl,Yl)到(Xm,Yl)和(Xr,Yr)到(Xm,Yr)两个线段就可以了。当然，要注意重复的情况。

## 代码

``` cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
typedef long long ll;
typedef pair<ll, ll> pll;

int main() {
    ios::sync_with_stdio(false);
    pll p[5];
    ll maxY = -1e18, minY = 1e18;
    for (int i = 1; i <= 3; i++) {
        ll x, y;
        cin >> x >> y;
        p[i] = pll(x, y);
        maxY = max(maxY, y);
        minY = min(minY, y);
    }
    sort(p + 1, p + 4, [](pll a, pll b) {
        return a.first < b.first;
    });
    vector<pll> ans;
    for (ll i = minY; i <= maxY; i++) {
        ans.push_back(pll(p[2].first, i));
    }
    if (p[1].first < p[2].first) {
        for (ll i = p[1].first; i < p[2].first; i++) {
            ans.push_back(pll(i, p[1].second));
        }
    } else if (p[1].first > p[2].first) {
        for (ll i = p[2].first + 1; i <= p[1].first; i++) {
            ans.push_back(pll(i, p[1].second));
        }
    }
    if (p[3].first < p[2].first) {
        for (ll i = p[3].first; i < p[2].first; i++) {
            ans.push_back(pll(i, p[3].second));
        }
    } else if (p[3].first > p[2].first) {
        for (ll i = p[2].first + 1; i <= p[3].first; i++) {
            ans.push_back(pll(i, p[3].second));
        }
    }
    cout << ans.size() << endl;
    for (pll p:ans) {
        cout << p.first << " " << p.second << endl;
    }
    return 0;
}
```