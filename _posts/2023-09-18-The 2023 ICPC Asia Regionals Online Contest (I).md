---
layout: post
title: 'The 2023 ICPC Asia Regionals Online Contest (I)'
subtitle: 'The 2023 ICPC Asia Regionals Online Contest (I)部分题解'
date: 2023-09-18
categories: 网络赛题解
cover: 'assets/img/2023_icpc_online1.png'
tags: ICPC 网络赛 题解
---

## [The 2023 ICPC Asia Regionals Online Contest (I)](https://pintia.cn/market/item/1703381331863785472)

## L-KaChang!
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

## A-Qualifiers Ranking Rules
### 题意：
<p>对两场比赛的学校排名进行归并,每场比赛每个学校只记最高队伍排名，排名相同时第一场优先</p>

### 思路：
<p>先对两场的排名进行去重，然后依次从第一场和第二场各取出一所学校，如果未加入最终排名，则加入，否则跳过，有一场取完后就只从另一场取，直到取完所有学校</p>

### AC代码：
```c++
#include<bits/stdc++.h>
using namespace std;
int main(){
    int n,m;
    cin>>n>>m;
    vector<string>a,b;
    map<string,int>mp;
    string s;
    for(int i=0;i<n;i++){
        cin>>s;
        if(mp[s]==0){
            mp[s]=1;
            a.push_back(s);
        }
    }
    mp.clear();
    for(int i=0;i<m;i++){
        cin>>s;
        if(mp[s]==0){
            mp[s]=1;
            b.push_back(s);
        }
    }
    vector<string>ans;
	n=a.size(),m=b.size();
	mp.clear();
    int l=0,r=0;
    while(l<n||r<m){
        if(l<n&&mp[a[l]]==0){
            mp[a[l]]=1;
            ans.push_back(a[l]);
        }
        if(r<m&&mp[b[r]]==0){
            mp[b[r]]=1;
            ans.push_back(b[r]);
        }
        l++,r++;
    }
    for(auto s:ans){
        cout<<s<<endl;
    }
}
```


