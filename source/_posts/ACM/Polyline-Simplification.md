---
title: Polyline-Simplification
date: 2018-04-25 08:35:42
tags: ACM
---

时间限制: 5 Sec  内存限制: 128 MB

## 题目描述
Mapping applications often represent the boundaries of countries, cities, etc. as polylines, which are connected sequences of line segments. Since fine details have to be shown when the user zooms into the map,these polylines often contain a very large number of segments. When the user zooms out, however, these fine details are not important and it is wasteful to process and draw the polylines with so many segments. In this problem, we consider a particular polyline simplification algorithm designed to approximate the original polyline with a polyline with fewer segments.
A polyline with n segments is described by n + 1 points p0 = (x0, y0),... , pn = (xn, yn), with the ith line segment being  . The polyline can be simplified by removing an interior point pi (1≤i≤n-1),so that the line segments  are replaced by the line segment . To select the point to be removed, we examine the area of the triangle formed by pi-1, pi, and pi+1 (the area is 0 if the three points are colinear), and choose the point pi such that the area of the triangle is smallest. Ties are broken by choosing the point with the lowest index. This can be applied again to the resulting polyline, until the desired number m of line segments is reached. 
Consider the example below.

The original polyline is shown at the top. The area of the triangle formed by p2, p3, and p4 is considered (middle), and p3 is removed if the area is the smallest among all such triangles. The resulting polyline after p3 is removed is shown at the bottom.
## 输入
The first line of input contains two integers n (2≤n≤200 000) and m (1≤m < n). The next n+1 lines specify p0,..., pn. Each point is given by its x and y coordinates which are integers between -5000 and 5000 inclusive. You may assume that the given points are strictly increasing in lexicographical order. That is, xi < xi+1, or xi = xi+1 and yi < yi+1 for all 0≤i < n.
## 输出
Print on the kth line the index of the point removed in the kth step of the algorithm described above (use the index in the original polyline).
## 样例输入
```
10 7
0 0
1 10
2 20
25 17
32 19
33 5
40 10
50 13
65 27
75 22
85 17
```
## 样例输出
```
1
9
```

## 题解
题目大意是： 给n+1个点，每个点可以与它左右的两个点形成一个三角形，每次删去最小的那个三角形的顶点，直到剩下m+1个点，输出每次删除的点的编号。
这是一道模拟题，按照题意写就行。

- 求三角形面积时用叉积求出它的面积的两倍，这样可以用int而不是double保存
- 为了找到最小的三角形，需要用一个set进行排序
- 删除一个点后要更新它左右两个点的数据
- 0和n要特殊处理


``` c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <cmath>
#include <set>
using namespace std;
const int maxn=200010;
typedef long long ll;

struct Node{
    int index;
    int area;
    bool operator<(const Node b)const {
        if (area==b.area) return index<b.index;
        return area<b.area;
    }
};

int LEFT[maxn],RIGHT[maxn];
int x[maxn],y[maxn];
int area[maxn];
int n,m,k;
set<Node> ss;

int getArea(int i) {
    int p1=i,p2=LEFT[i],p3=RIGHT[i];
    return abs(x[p1]*y[p2]+x[p2]*y[p3]+x[p3]*y[p1]-x[p1]*y[p3]-x[p2]*y[p1]-x[p3]*y[p2]);
}


int main() {
    scanf("%d%d",&n,&m);
    k=n-m;
    for (int i=0;i<=n;i++){
        scanf("%d%d",&x[i],&y[i]);
    }
    LEFT[n]=n-1;
    RIGHT[0]=1;
    for (int i=1;i<n;i++) {
        LEFT[i]=i-1;
        RIGHT[i]=i+1;
        area[i]=getArea(i);
        ss.insert(Node{i,area[i]});
    }
    for (int i=1;i<=k;i++) {
        set<Node>::iterator fir = ss.begin();
        int p = (*fir).index;
        ss.erase(fir);
        fir=ss.end();
        LEFT[RIGHT[p]]=LEFT[p];
        RIGHT[LEFT[p]]=RIGHT[p];
        if (LEFT[p]!=0) {
            fir = ss.find(Node{LEFT[p], area[LEFT[p]]});
            ss.erase(fir);
            area[LEFT[p]] = getArea(LEFT[p]);
            ss.insert(Node{LEFT[p], area[LEFT[p]]});
        }
        if (RIGHT[p]!=n) {
            fir = ss.find(Node{RIGHT[p], area[RIGHT[p]]});
            ss.erase(fir);
            area[RIGHT[p]] = getArea(RIGHT[p]);
            ss.insert(Node{RIGHT[p], area[RIGHT[p]]});
        }
        printf("%d\n",p);
    }
    return 0;
}
```