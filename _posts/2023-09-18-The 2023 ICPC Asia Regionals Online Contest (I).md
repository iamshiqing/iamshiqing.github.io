---
layout: post
title: 'The 2023 ICPC Asia Regionals Online Contest (I)'
subtitle: 'The 2023 ICPC Asia Regionals Online Contest (I)题解'
date: 2023-09-18
categories: 网络赛题解
cover: 'assets/img/2023_icpc_online1.png'
tags: ICPC 网络赛 题解
---

## [The 2023 ICPC Asia Regionals Online Contest (I)](https://pintia.cn/market/item/1703381331863785472)

## L KaChang!
### 题意：
<p>给了n个程序的运行时间和标程的运行时间，问时限开到标程的多少倍，所有程序均可通过，并且至少开到二倍</p>

### 思路：
<p>对n个程序的运行时间取max，max/k向上取整，如果小于2，直接输出2</p>

### AC代码：
```c++
#include<bits/stdc++.h>
using namespace std;
int main(){
    int n,T;
    cin>>n>>T;
    int mx=0;
    while(n--){
        int t;
        cin>>t;
        mx=max(mx,t);
    }
    int k=ceil(mx*1.0/T);
    cout<<(k<2?2:k)<<endl;
}
```

