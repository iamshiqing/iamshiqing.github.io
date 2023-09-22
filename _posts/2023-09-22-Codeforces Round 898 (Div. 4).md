---
layout: post
title: 'Codeforces Round 898 (Div. 4)'
subtitle: 'Codeforces Round 898 (Div. 4) A-H题解'
date: 2023-09-22
categories: CF题解
cover: 'assets/img/codeforces.png'
tags: Codeforces 题解
---

# [Codeforces Round 898 (Div. 4)](https://codeforces.com/contest/1873)

## A. Short Sort
### 题意：
<p>给你三张写有a，b，c的牌，按一定顺序排列，你最多可以操作一次，选取两张牌交换位置，问是否可以使其按abc顺序排列</p>

### 思路：
<p>三张牌只要有一张在最终位置上就可以使其按abc排列</p>

### AC代码：
```c++
#include<bits/stdc++.h>
#define int long long 
#define pb push_back
#define P pair<int,int>
#define f first 
#define s second 
using namespace std;
 
void solve(){
    string s;
    cin>>s;
    if(s[0]=='a'||s[1]=='b'||s[2]=='c')
        cout<<"yes"<<endl;
    else
        cout<<"no"<<endl;
}
 
signed main(){
    int _=1;
    cin>>_;
    while(_--){
        solve();
    }
    return 0;
}
```

## B. Good Kid
### 题意：
<p>给定n个数，给其中一个数加1，使这n个数的乘积最大</p>

### 思路：
<p>给最小的数加1，即可使其乘积最大</p>

### AC代码：
```c++
#include<bits/stdc++.h>
#define int long long 
#define pb push_back
#define P pair<int,int>
#define f first 
#define s second 
using namespace std;
 
void solve(){
    int n;
    cin>>n;
    vector<int>a(n);
    for(int i=0;i<n;i++)
        cin>>a[i];
    sort(a.begin(),a.end());
    a[0]+=1;
    int ans=1;
    for(auto x:a){
        ans*=x;
    }
    cout<<ans<<endl;
}
 
signed main(){
    int _=1;
    cin>>_;
    while(_--){
        solve();
    } 
    return 0;
}
```

## C. Target Practice
### 题意：
<p>一个10*10的图从外到里依次为1-5分，X为射中了这个点，求总分</p>

### 思路：
<p>设某点坐标x,y，则该点分值为 { x，y，10-x+1，10-y+1 } 取最小值，对所有射中点求和即可<p>

### AC代码：
```c++
#include<bits/stdc++.h>
#define int long long 
#define pb push_back
#define P pair<int,int>
#define f first 
#define s second 
using namespace std;
 
void solve(){
    int ans=0;
    for(int i=0;i<10;i++){
        string s;
        cin>>s;
        for(int j=0;j<10;j++){
            if(s[j]=='X')
                ans+=min({i+1,j+1,10-i,10-j});
        }
    }
    cout<<ans<<endl;
}
 
signed main(){
    int _=1;
    cin>>_;
    while(_--){
        solve();
    }
    return 0;
}
```

## D. 1D Eraser
### 题意：
<p>给你n个连续的单元格，每个格子不是黑色就是白色，你可以选择连续的k个格子使其全部变白，问要使所有格子变白最少需要操作多少次</p>

### 思路：
<p>贪心，从左到右扫，每碰见一个黑格子，便把从它开始的连续k个格子变白</p>

### AC代码：
```c++
#include<bits/stdc++.h>
#define int long long 
#define pb push_back
#define P pair<int,int>
#define f first 
#define s second 
using namespace std;

void solve(){
    int n,k;
    cin>>n>>k;
    int ans=0;
    string s;
    cin>>s;
    for(int i=0;i<s.length();){
        if(s[i]=='B'){
            i+=k;
            ans++;
        }else{
            i++;
        }
    }
    cout<<ans<<endl;
}
 
signed main(){
    int _=1;
    cin>>_;
    while(_--){
        solve();
    }
    return 0;
}
```

## E. Building an Aquarium
### 题意：
<p>给n个柱子，高度为ai，宽为1，选一个高度h>=1,往进注水使柱子高度小于h的达到h，给定注水量为不超过x，找最高h</p>

### 思路：
<p>直接二分答案<p>

### AC代码：
```c++
#include<bits/stdc++.h>
#define int long long 
#define pb push_back
#define P pair<int,int>
#define f first 
#define s second 
using namespace std;
vector<int>a;
bool check(int mid,int x){
    int ans=0;
    for(auto t:a){
        if(t<mid)
            ans+=mid-t;
    }
    return ans<=x;
}
void solve(){
    a.clear();
    int n,x;
    cin>>n>>x;
    for(int i=0;i<n;i++){
        int t;
        cin>>t;
        a.pb(t);
    }
    int l=1,r=1e10;
    while(l<r){
        int mid=(l+r+1)/2;
        if(check(mid,x)){
            l=mid;
        }else{
            r=mid-1;
        }
    }
    cout<<l<<endl;
}
 
signed main(){
    int _=1;
    cin>>_;
    while(_--){
        solve();
    }
    return 0;
}
```

## F. Money Trees
### 题意：
<p>n棵树，第i棵树，有ai个果子，hi高，他可以收集连续几棵树上的果子，但总数不能超过k，这几棵树得满足相邻的两个树之间，前面的树高度能被后面的树的高度整除</p>

### 思路：
<p>双指针扫一遍，满足前面的树高度能被后面的树高度整除时，移动右指针，不满足时左指针直接跳到右指针位置，然后移动右指针，当摘的果子总数超过k时移动左指针，但左指针不能大于右指针</p>

### AC代码：
```c++
#include<bits/stdc++.h>
#define int long long 
#define pb push_back
#define P pair<int,int>
#define f first 
#define s second 
using namespace std;
void solve(){
    int n,k;
    cin>>n>>k;
    vector<P>a(n);
    for(int i=0;i<n;i++)
        cin>>a[i].f;
    for(int i=0;i<n;i++)
        cin>>a[i].s;
    int ans=0;
    int l=0,r=0;
    int t=0;
    while(l<n&&r<n){
        if(l!=r&&a[r-1].s%a[r].s!=0){
            t=a[r].f;
            l=r;
            r=r+1;
        }else{
            t+=a[r].f;
            r++;
            
        }
        while(t>k&&l<r){
            t-=a[l].f;
            l++;
        }
        ans=max(ans,r-l);
        
    }
    cout<<ans<<endl;
}
 
signed main(){
    int _=1;
    cin>>_;
    while(_--){
        solve();
    }
    return 0;
}
```

## G. ABBC or BACB
### 题意：
<p>给一个字符串，只包含字母A和字母B，你可以选择一个AB使其变为BC，也可以选择一个BA使其变为CB，问最多可以操作多少次</p>

### 思路:
<p>很明显对每个B,它的贡献是它前面连续的A的数量，或者它后面连续的A的数量，然后就可以把原串中连续的A堆在一起，并记录数量，然后dp，如果这个B选择它前面的A，那么它的值就是 { 它前面的点选前面时的值+它前面A的数量，它前面选后面时的值 } 取最大值，如果选后面的A，那么它的值就是 { 它前面的点选前面时的值，它前面选后面时的值 } 取最大值+它后面的A的数量</p>

### AC代码：
```c++
#include<bits/stdc++.h>
#define int long long 
#define pb push_back
#define P pair<int,int>
#define f first 
#define s second 
using namespace std;
void solve(){
    string s;
    cin>>s;
    int a[s.length()+2];
    memset(a,0,sizeof(a));
    string ss;
    int l=0;
    int len=0;
    for(int i=0;i<s.length();i++){
        if(s[i]=='A'){
            if(l!=0)
                a[l]++;
            else{
                ss+='A';
                len++;
                l=len;
                a[l]++;
                
            }
        }else{
            ss+='B';
            len++;
            l=0;
        }
    }
    int q=0;
    int dp[ss.length()+2][2];
    memset(dp,0,sizeof(dp));
    ss=" "+ss;
    int ans=0;
    for(int i=1;i<ss.length();i++){
        if(ss[i]=='B'){
            dp[i][0]=max(dp[q][0]+a[i-1],dp[q][1]);
            dp[i][1]=max(dp[q][1],dp[q][0])+a[i+1];
            ans=max({ans,dp[i][0],dp[i][1]});
            q=i;
        }
    }
    cout<<ans<<endl;

}
 
signed main(){
    int _=1;
    cin>>_;
    while(_--){
        solve();
    }
    return 0;
}
```

## H. Mad City
### 题意：
<p>n个点，n条无向边，开始时Marcel在a，Valeriu在b，他们每次可以选择待在原地，或者前往相连的点，他们同时开始并同时结束运动，Valeriu可以预测Marcel的行动，问Valeriu能无限期躲避Marcel吗</p>

### 思路：
<p>先找环，然后算b到环上最近点的距离，然后算a到该点的距离，如果b到该点的距离小于a到该点的距离，则Valeriu可以无限期躲避Marcel。关于找环可以在读边的时候顺便用并查集维护，如果两个点已经相连，在两点间还有条边，则这两个点在环上，可以先不加这条边，读完之后从两点之一dfs搜索另一点，就可以找到环上所有点。，然后bfs求b到环的距离，第一次访问到环上的点的时候，此时的距离就是b到环的距离，并标记这个点，然后bfs求a到该点的距离，最后比较即可</p>

### AC代码：
```c++
#include<bits/stdc++.h>
#define int long long 
#define pb push_back
#define P pair<int,int>
#define f first 
#define s second 
using namespace std;
const int MAX=1000100;
int root[MAX];//存每个点的根节点
int tall[MAX];//存每个节点的高，优化合并
//初始化根节点为自己，高为0
void init(int n){
    for(int i=0;i<=n;i++)
        root[i]=i,tall[i]=0;
}
//递归查找根节点，并更新
int find(int n){
    if(n==root[n])
        return n;
    else 
        return root[n]=find(root[n]);
}
//判断x,y是否在同一个集合
bool issame(int x,int y){
    return find(x)==find(y);
} 
//合并x，y两个集合
void merge(int x,int y){
    x=find(x);
    y=find(y);
    if(x==y)
        return;
    else if(tall[x]<tall[y])
        root[x]=y;
    else{
        root[y]=x;
        if(tall[x]==tall[y])
            tall[x]++;
    }
}
vector<int>a[MAX];//存基环树
int b[MAX];//标记该点是否在环上
int flag=0;//dfs找到后回溯跳过无关点用
void dfs(int r,int t,int f){
    if(r==t){
        flag=1;
        b[r]=1;
        return;
    }
    for(int i=0;i<a[r].size();i++){
        if(a[r][i]!=f){
            dfs(a[r][i],t,r);
            if(flag){
                b[a[r][i]]=1;
                return;
            }
        }
    }
    
}
void solve(){
    flag=0;
    int n,aa,bb;
    cin>>n>>aa>>bb;
    init(n);//并查集初始化
    int s,t;//环上的两个点
    for(int i=0;i<n;i++){
        int u,v;
        cin>>u>>v;
        if(issame(u,v)){
            s=u,t=v;
        }else{
            merge(u,v);
            a[u].pb(v);
            a[v].pb(u);
        }
    }
    dfs(s,t,s);
    b[s]=1;
    a[s].pb(t);
    a[t].pb(s);
    int dis[n+1];//到某点距离距离
    int flag2[n+1];//标记是否访过
    fill(flag2,flag2+n+1,0);
    fill(dis,dis+n+1,MAX);
    dis[bb]=0;
    int f=0;//b距离环最近的点
    queue<int>q;
    q.push(bb);
    flag2[bb]=1;
    while(!q.empty()){
        int tt=q.front();
        q.pop();
        if(b[tt]){
            f=tt;
            break;
        }else{
            for(int i=0;i<a[tt].size();i++){
                if(flag2[a[tt][i]]==0){
                    dis[a[tt][i]]=dis[tt]+1;
                    q.push(a[tt][i]);
                    flag2[a[tt][i]]=1;
                }
            }
        }
    }
    while(!q.empty())
        q.pop();
    int dist=dis[f];//b到环上最近点的距离
    fill(flag2,flag2+n+1,0);
    fill(dis,dis+n+1,MAX);
    dis[aa]=0;
    q.push(aa);
    flag2[aa]=1;
    while(!q.empty()){
        int tt=q.front();
        q.pop();
        for(int i=0;i<a[tt].size();i++){
            if(flag2[a[tt][i]]==0){
                dis[a[tt][i]]=dis[tt]+1;
                q.push(a[tt][i]);
                flag2[a[tt][i]]=1;
            }
        }
    }
    if(dist<dis[f]){
        cout<<"yes"<<endl;
    }else{
        cout<<"no"<<endl;
    }
    for(int i=0;i<=n;i++){
        b[i]=0;
        a[i].clear();
    }
}
 
signed main(){
    int _=1;
    cin>>_;
    while(_--){
        solve();
    }
    return 0;
}
```