倍增法，通过字面意思来看就是翻倍。这个方法在很多算法中均有应用。其中最常用的就是 RMQ 问题和求 LCA 了。

## RMQ 问题

### 简介

RMQ 是英文 Range Maximum （Minimum） Query 的缩写，表示查询区间最大（最小）值。

解决 RMQ 问题的主要方法有两种，分别是 ST 表和线段树。本文主要讲 ST 表。

### 引入

[ST 表模板题](https://www.luogu.org/problemnew/show/P3865)

题目大意：给定 $n$ 个数，有 $m$ 个询问，对于每个询问，你需要回答区间 $[x,y]$ 中的最大值

考虑暴力做法。每次都对区间 $[x,y]$ 扫描一遍，求出最大值

显然，这个算法会超时。

### ST 表

ST 表基于倍增思想，可以在 $O(n\log{n})$ 的时间复杂度下预处理，用 $O(1)$ 回答每个询问。但是 ** 不支持修改操作 ** 。

暴力跑的慢的原因在于检索了查询区间的每一个点。

但是，如果我们预处理出每一段的最大值，就可以将效率提高很多。

令 $f[i][j]$ 表示 $[i,i+2^j-1]$ 的最大值。

显然，$f[i][0]=a[i]$ 。

根据定义式，写出状态转移方程：$f[i][j]=max(f[i][j-1],f[i+2^{j-1}][j-1])$

我们可以这么理解：将区间 $[i,i+2^j-1]$ 分成左右两个长度相同的两部分 $[i,i+2^{j-1}-1]$ 和 $[i+2^{j-1}+1,i+2^j-1]$。

![](./images/st1.png)

预处理终于完成了！接下来就是查询了。

对于每个询问 $[x,y]$，我们把它分成两个部分：$f[x][s]$ 和 $f[y-2^s+1][s]$ 。

其中 $s=\log_2{(y-x+1)}$

显然，这两个区间会重叠。但是， ** 重叠并不会对区间最大值产生影响 ** 。同时这两个区间刚好覆盖了 $[x,y]$ ，可以保证答案的正确性。

### 模板代码

```cpp
#include<bits/stdc++.h>
using namespace std;
const int logn=21;
const int maxn=2000001;
long long a[maxn],f[maxn][logn],Logn[maxn];
inline int read()
{
    char c=getchar();int x=0,f=1;
    while(c<'0'||c>'9'){if(c=='-')f=-1;c=getchar();}
    while(c>='0'&&c<='9'){x=x*10+c-'0';c=getchar();}
    return x*f;
}
void pre() 
{
    Logn[1]=0;
    Logn[2]=1;
    for(int i=3;i<=maxn;i++)
    {
        Logn[i]=Logn[i/2]+1;
    }
}
int main()
{
    int n=read(),m=read();
    for (int i=1;i<=m;i++)
        f[i][0]=read();
    pre();
    for (int j=1;j<=logn;j++)
        for (int i=1;i+(1<<j)-1<=n;i++)
            f[i][j]=max(f[i][j-1],f[i+(1<<(j-1))][j-1]);
    for (int i=1;i<=m;i++)
    {
        int x=read(),y=read();
        int s=Logn[y-x+1];
        printf("%d\n",max(f[x][s],f[y-(1<<s)+1][s]));   
    }
    return 0;
}
```

### 注意点

1. 输入输出数据一般很多，建议开启输入输出优化

2. 每次用 [std::log](https://en.cppreference.com/w/cpp/numeric/math/log) 重新计算 log 函数值并不值得，建议预处理：

`Logn[i]=Logn[i/2]+1`

初始值 `Logn[1]=0`

### 总结

ST 表能较好的维护区间信息，时间复杂度较低，代码量相对其他算法不大。但是，ST 表能维护的信息非常有限，不能较好地扩展，并且不支持修改操作。

## 树上倍增求 LCA
