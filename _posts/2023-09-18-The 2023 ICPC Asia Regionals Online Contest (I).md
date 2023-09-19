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

## D-Transitivity
### 题意：
<p>给定n个点，m条边的一个简单无向图。定义一个图是可传递的，如果两点之间有路径，则两点之间有条边，要求添加最少且至少一条边使图依然是可传递的</p>

### 思路：
<p>先判连通块，如果所有连通块都是可传递的，则连接最小的两个连通块，否则使每个连通块可传递</p>

### AC代码：
```c++
#include<bits/stdc++.h>
using namespace std;
#define int long long
vector<int>a[1000010];
int flag[1000010];
pair<int,int> dfs(int r){
    pair<int,int>t(1,0);
    flag[r]=1;
    for(int i=0;i<a[r].size();i++){
        if(flag[a[r][i]]){
            t.second++;
        }else{
            pair<int,int>p=dfs(a[r][i]);
            t.first+=p.first;
            t.second+=p.second+1;
        }
    }
    return t;
}
signed main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);
    int n,m;
    cin>>n>>m;
    for(int i=0;i<m;i++){
        int u,v;
        cin>>u>>v;
        a[u].push_back(v);
        a[v].push_back(u);
    }
    int ans=0;
    vector<int>b;
    for(int i=1;i<=n;i++){
        if(flag[i]==0){
            pair<int,int>t=dfs(i);
            ans+=(t.first*(t.first-1)/2-t.second/2);
            b.push_back(t.first);
        }
    }
    sort(b.begin(),b.end());
    if(ans==0){
        cout<<b[0]*b[1]<<endl;
    }else{
        cout<<ans<<endl;
    }
}
```

## J-Minimum Manhattan Distance
### 题意：
<p>给定两个圆C1，C2，这两个圆被用直径表示，求C2上或内的一个点到C1内点的最小曼哈顿距离的期望值</p>

### 思路：
<p>如果C1的圆心在C2的左边，肯定是C2左半部分某点到C1圆心的曼哈顿距离，否则则是C2右半部分到C1圆心的曼哈顿距离，通过圆心和半径表示出圆的方程，用方程把x坐标或y坐标表示出，带入曼哈顿距离公式，然后三分求解</p>

### AC代码：
```c++
#include<bits/stdc++.h>
using namespace std;
#define x first
#define y second
void solve(){
    double x1,y1,x2,y2;
    cin>>x1>>y1>>x2>>y2;
    double r1=sqrt((x1-x2)*(x1-x2)+(y1-y2)*(y1-y2))/2;
    double x3,y3,x4,y4;
    cin>>x3>>y3>>x4>>y4;
    double r2=sqrt((x3-x4)*(x3-x4)+(y3-y4)*(y3-y4))/2;
    pair<double,double> o1,o2;
    o1={(x1+x2)/2,(y1+y2)/2};
    o2={(x3+x4)/2,(y3+y4)/2};
    double l=o2.y-r2,r=o2.y+r2;
    double ans=1e8;
    double pos=1e-7;
    if(o1.x<o2.x){
            while(r-l>=1e-5){
            double mid1=(l+r)/2-pos;
            double mid2 = (l + r) / 2 + pos;
            double f1 = abs(0-sqrt(r2 * r2 - (mid1 - o2.y) * (mid1 - o2.y)) + o2.x - o1.x) + abs(mid1 - o1.y);
            double f2=abs(0-sqrt(r2*r2-(mid2-o2.y)*(mid2-o2.y))+o2.x-o1.x)+abs(mid2-o1.y);
            if(f1>=f2){
                l=mid1;
                ans=f1;
            }else{
                r=mid2;
                ans=f2;
            }
        }	
    }else{
        while(r-l>=1e-5){
            double mid1=(l+r)/2-pos;
            double mid2 = (l + r) / 2 + pos;
            double f1 = abs(sqrt(r2 * r2 - (mid1 - o2.y) * (mid1 - o2.y)) + o2.x - o1.x) + abs(mid1 - o1.y);
            double f2=abs(sqrt(r2*r2-(mid2-o2.y)*(mid2-o2.y))+o2.x-o1.x)+abs(mid2-o1.y);
            if(f1>=f2){
                l=mid1;
                ans=f1;
            }else{
                r=mid2;
                ans=f2;
            }
        }
    }
    printf("%.10f\n",ans);
}

int main(){
    int _=1;
    cin>>_;
    while(_--)
        solve();
    return 0;
}
```

