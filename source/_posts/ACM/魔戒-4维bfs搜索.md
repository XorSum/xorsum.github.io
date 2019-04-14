---
title: 魔戒_4维bfs搜索
date: 2017-11-08 13:31:42
tags: ACM
---


## Problem Description

蓝色空间号和万有引力号进入了四维水洼，发现了四维物体--魔戒。
这里我们把飞船和魔戒都抽象为四维空间中的一个点，分别标为 "S" 和 "E"。空间中可能存在障碍物，标为 "#"，其他为可以通过的位置。
现在他们想要尽快到达魔戒进行探索，你能帮他们算出最小时间是最少吗？我们认为飞船每秒只能沿某个坐标轴方向移动一个单位，且不能越出四维空间。

## Input

输入数据有多组（数据组数不超过 30），到 EOF 结束。
每组输入 4 个数 x, y, z, w 代表四维空间的尺寸（1 <= x, y, z, w <= 30）。
接下来的空间地图输入按照 x, y, z, w 轴的顺序依次给出，你只要按照下面的坐标关系循环读入即可。
for 0, x-1
    for 0, y-1
        for 0, z-1
            for 0, w-1
保证 "S" 和 "E" 唯一。

## Output

对于每组数据，输出一行，到达魔戒所需的最短时间。
如果无法到达，输出 "WTF"（不包括引号）。

## Example Input

```
2 2 2 2
..
.S
..
#.
#.
.E
.#
..
2 2 2 2
..
.S
#.
##
E.
.#
#.
..
```
## Example Output
```
1
3
```

“师创杯”山东理工大学第九届ACM程序设计竞赛 正式赛

裸的bfs水题,只是维数从二维变成四维而已，其它的都和二维一样

## 代码
``` c
#include <iostream>
#include <cstdio>
#include <cstdlib>
#include <string>
#include <string.h>
#include <queue>
using namespace std;
int area[35][35][35][35],vis[35][35][35][35];
int x,y,z,w,sx,sy,sz,sw,ex,ey,ez,ew;
struct Node
{
    int x,y,z,w,step;
    Node(int x,int y,int z,int w,int s):x(x),y(y),z(z),w(w),step(s){};

};

int dir[8][4]={{1,0,0,0},{0,1,0,0},{0,0,1,0},{0,0,0,1},{-1,0,0,0},{0,-1,0,0},{0,0,-1,0},{0,0,0,-1}};

int check(int dx,int dy,int dz,int dw)
{
    if (dx<0||dx>=x||dy<0||dy>=y||dz<0||dz>=z||dw<0||dw>=w)
        return 0;
    else return 1;
}
int bfs()
{
    Node tmp(sx,sy,sz,sw,0);
    queue<Node> que;
    que.push(tmp);
    vis[tmp.x][tmp.y][tmp.z][tmp.w]=tmp.step;
    while(!que.empty())
    {
        tmp=que.front();
        que.pop();
        for (int ijk=0;ijk<8;ijk++)
        {
            int dx=tmp.x+dir[ijk][0];
            int dy=tmp.y+dir[ijk][1];
            int dz=tmp.z+dir[ijk][2];
            int dw=tmp.w+dir[ijk][3];
            if (check(dx,dy,dz,dw)==0||area[dx][dy][dz][dw]==1)
                continue;
            if (dx==ex&&dy==ey&&dz==ez&&dw==ew)
                return tmp.step+1;
            if (vis[dx][dy][dz][dw]!=0&&vis[dx][dy][dz][dw]<=tmp.step+1)
                continue;
            vis[dx][dy][dz][dw]=tmp.step+1;
            Node aaa(dx,dy,dz,dw,tmp.step+1);
            que.push(aaa);
        }

    }
    return -1;
}
int main()
{
    while(scanf("%d%d%d%d",&x,&y,&z,&w)!=EOF)
    {
        char tmp;
        memset(area,0,sizeof(area));
        memset(vis,0,sizeof(vis));
        for (int i=0;i<x;i++)
            for (int j=0;j<y;j++)
            for (int k=0;k<z;k++)
            for (int l=0;l<w;l++)
        {
            cin>>tmp;
            if (tmp=='#')
                area[i][j][k][l]=1;
            else if (tmp=='S')
            {
                sx=i;sy=j;sz=k;sw=l;
            }
            else if (tmp=='E')
            {
                ex=i;ey=j;ez=k;ew=l;
            }
        }
        int ans=bfs();
        if (ans!=-1)
            printf("%d\n",ans);
        else printf("WTF\n");
    }
	return 0;
}


/***************************************************
Result: Accepted
Take time: 576ms
Take Memory: 2092KB
Submit time: 2017-06-04 15:20:48
****************************************************/

```