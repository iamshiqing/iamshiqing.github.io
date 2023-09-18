---
layout: post
title: 'Codeforces Round 880 (Div. 2)'
subtitle: 'Codeforces Round 880 (Div. 2) A-C题解'
date: 2023-06-19
categories: CF题解
cover: 'assets/img/codeforces.png'
tags: Codeforces 题解
---

# [Codeforces Round 880 (Div. 2)](https://codeforces.com/contest/1836)

## A-Destroyer
### 题意：
<p>n个人站了几条线，每个人会报出所在线自己前面的人数，判断报数是否合法</p>

### 思路：
<p>数很小在100以内，直接开一个桶，把每次读入放进去，检查一遍只要桶里有i的数量大于i-1的数量，即不合法。</p>

## AC代码：
```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
void solve(){
    int n;
    cin>>n;
    int a[101];
    memset(a,0,sizeof(a));
    for(int i=0;i<n;i++)
    {
        int t;
        cin>>t;
        a[t]++;
    }
    int flag=1;
    for(int i=1;i<=100;i++)
        if(a[i]>a[i-1])
            flag=0;
    if(flag)
        cout<<"YES"<<endl;
    else    
        cout<<"NO"<<endl;
}
signed main(){
    int _;
    cin>>_;
    while(_--)
        solve();
    
    return 0;
}
```
## B-Astrophysicists
### 题意：
<p>公司打算给n个人发放共计k*g个银币，实际发放是打算给某人发x个，如果x%g>=g/2 会把x%g补齐到g,否则就不发放这x%g个了，问最多能减少发放多少银币。</p>

### 思路：
<p>贪心，给每个人都尽可能少发，g为奇时，每个人最多少发g/2,g为偶时，每个人最多少发g/2-1,知道每个人了就可以知道总共少发了多少，但是当总数%g！=0时，需要补齐到g,所以答案为总数/g*g（注意特殊情况，减少的不能大于k*g）。</p>

### AC代码：
```cpp
#include<bits/stdc++.h>
#define int long long 
using namespace std;
void solve(){
    int n,k,g;
    cin>>n>>k>>g;
    int ans;
    if(g%2)
        ans=n*(g/2);
    else
        ans=n*(g/2-1);
    ans=ans/g;
    if(ans>k)
        ans=k;
    cout<<ans*g<<endl;
}

signed main(){
    int _;
    cin>>_;
    while(_--)
        solve();
    
    return 0;
}
```
## C-k-th equality
### 题意：
<p>给出a,b,c,k,a为第一个数的位数，b为第而个数的位数，c为第三个数的位数，要求是找到满足 第一个数+第二个数=第三个数 按字典序排序的第k个式子。</p>

### 思路：
<p>枚举第一个数，根据第三个数的区间和第一个数算出对应的第二个数的区间，统计满足式子的总数，等于k时输出结果，枚举完小于k输出-1。</p>

### AC代码：
```cpp
#include<bits/stdc++.h>
#define int long long
using namespace std;
void solve(){
    int pre[8]={0,1,10,100,1000,10000,100000,1000000};
    int a,b,c,k;
    cin>>a>>b>>c>>k;
    int ans=0;
    int ll=pre[a],rr=pre[a+1]-1;
    int s=pre[b],e=pre[b+1]-1;
    int ansl=pre[c],ansr=pre[c+1]-1;
    for(  ;ll<=rr;ll++){
        int l,r;
        if(ansl-ll>=s&&ansl-ll<=e)
            l=ansl-ll;
        else if(ansl-ll<s){
            if(ansr-ll>=s)
                l=s;
            else
                break;
        }else{
            continue;
        }
        if(ansr-ll>=s&&ansr-ll<=e){
            r=ansr-ll;
        }else if(ansr-ll<s){
            break;
        }else{
            r=e;
        }
        if(r-l+1+ans>=k){
            cout<<ll<<" + "<<l+(k-ans-1)<<" = "<<l+(k-ans-1)+ll<<endl;
            return;
        }else
            ans+=r-l+1;
    }
    cout<<-1<<endl;
}

signed main(){
    int _;
    cin>>_;
    while(_--)
        solve();
    
    return 0;
}
```