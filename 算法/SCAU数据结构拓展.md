# 拓展习题一

## 18711字符串去重

可以用set去重后输出 也可以直接调用C++中的函数 这里给出调用函数的做法

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    string s;
    cin>>s;
    sort(s.begin(),s.end());
    s.erase(unique(s.begin(),s.end()),s.end());
    cout<<s<<"\n";
    return 0;
}
/*
 
*/
```

## **18710 统计不同数字的个数(升级版)**

直接丢set里去重就可以了

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    set<int>st;
    for(int i=0;i<n;i++){
        int t;
        cin>>t;
        st.insert(t);
    }
    cout<<st.size()<<"\n";
    return 0;
}
/*
 
*/
```

## 18927前缀和

模板题 但是要注意会爆int

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<ll>g(n+1);
    for(int i=1;i<=n;i++){
        cin>>g[i];
        g[i]+=g[i-1];
    }
    int m;
    cin>>m;
    while(m--){
        int l,r;
        cin>>l>>r;
        cout<<g[r]-g[l-1]<<"\n";
    }
    return 0;
}
/*
 
*/
```

## 18709魔法

先分别记录黑白奶牛总数 然后可以考虑遍历数组  枚举分界点  比如说遍历到下标为 $i$ 时 用$a$ 和 $b$ 分别记录 $[0，i]$ 这个区间内有 $a$  只白奶牛 $b$ 只黑奶牛   我们可以让 $[0，i]$ 这个区间内全部变成白 ，$[i+1,n-1]$ 全部变成黑 这样的话操作次数就是 $b+white-a$ 答案就是 $min(ans,b+white-a)$                                                                                                             

 那么 $ans$ 应该初始化为多少呢 如果是按$[0，i]$ $[i+1,n-1]$ 这样进行划分的话 就会忽略掉全部变成黑色的情况 所以$ans$应该初始化为全部变成黑色的操作数 

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<int>g(n);
    int black=0,white=0;//记录黑色和白色的总数
    for(int i=0;i<n;i++){
        cin>>g[i];
        if(g[i]==1) white++;
        else black++;
    }

    int a=0,b=0;//记录遍历到i位置时 [0,i]这个区间内有a只白奶牛 b只黑奶牛
    int ans=white;
    for(int i=0;i<n;i++){
        if(g[i]==1) a++;
        else b++;
        ans=min(ans,b+white-a);
    }
    cout<<ans<<"\n";
    return 0;
}
/*
 
*/
```

## 18770差值最大

维护一个最小值$mn$ 即可 遍历数组 如过$g[i]<mn$ 则更新$mn$ 否则执行 $ans=min(ans,g[i]-mn)$ 

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<int>g(n);
    for(int i=0;i<n;i++){
        cin>>g[i];
    }
    int mn=g[0];
    int ans=0;
    for(int i=1;i<n;i++){
        if(g[i]<mn) mn=g[i];
        else ans=max(ans,g[i]-mn);
    }
    cout<<ans<<"\n";
    return 0;
}
/*
 
*/
```

## 18708最大子段和

用$dp[i]$表示以$g[i]$结尾的最大子数组和 状态转移公式就是$dp[i]=max(dp[i-1]+g[i],g[i])$ 要注意的是$ans$不能初始化为0 因为要考虑数组元素全部为负数的情况

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<int>g(n);
    for(int i=0;i<n;i++){
        cin>>g[i];
    }
    vector<int>dp(n);
    int ans=-inf;
    dp[0]=g[0];
    for(int i=1;i<n;i++){
        dp[i]=max(dp[i-1]+g[i],g[i]);
        ans=max(ans,dp[i]);
    }
    cout<<ans<<"\n";
    return 0;
}
/*

*/
```

## 18063圈中的游戏

可以用数组模拟这个过程 一共要淘汰$n-1$个人 所以一共要进行$n-1$轮 注意要特判$n==1$的情况

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    if(n==1){
        cout<<1<<"\n";
        return 0;
    }
    vector<int>g(n);
    for(int i=0;i<n-1;i++){
        g[i]=i+1;
    }
    g[n-1]=0;
    int pre,cur=n-1;
    for(int i=1;i<n;i++){
        for(int j=0;j<3;j++){
            pre=cur;
            cur=g[cur];
        }
        g[pre]=g[cur];
    }
    cout<<pre+1<<"\n";
    return 0;
}
/*

*/
```

## 19079输出链表倒数第K个元素

双指针法。

设链表一共有$n$个元素  倒数$k$个元素前一共有$n-k$个元素  可以先让快指针从虚拟头结点出发走$k-1$步  然后快慢指针一起走$n-k$步  这样的话  如果$K \leq n$  那么当快指针指向空的时候 慢指针必然就是指向倒数第$K$个元素 如果快指针在先走的过程中就已经走到空了 说明$K$是不合法的                                                                                                                                    

注意题目中给的$L$就已经是虚拟头结点了 不用自己另设

```cpp
#include <iostream>//C++
using namespace std;
struct LNode
{
    int data;
    LNode * next;
};
void createList(LNode * &L,int n)
{/**< 尾插法创建单链表 */
    LNode *r, *p;
    r=L=new LNode;/**< 创建头结点 */
    L->next=NULL;
    for(int i=1;i<=n;i++)
    {
        p=new LNode;
        cin>>p->data;
        p->next=NULL;
        r->next=p;
        r=p;
    }
}
void trv(LNode * L)
{ /**< 一个简单的链表遍历函数，供编程过程中测试使用 */
    L=L->next;
    while(L)
    {
        cout<<L->data<<' ';
        L=L->next;
    }
}
int getK(LNode * L,int k)
{
   //L就是虚拟头结点!
   if(!L||k<0) return -1;
   LNode*fast=L,*slow=L;
   for(int i=0;i<k;i++){
    if(!fast) return -1;
    fast=fast->next;        
   }
   if(!fast) return -1;
   while(fast){
    slow=slow->next;
    fast=fast->next;
   }
   return slow->data;
}
int main()
{
    int n,k;
    LNode *L;
    cin>>n>>k;
    createList(L,n);
    //trv(L);
    cout<<getK(L,k);
    return 0;
}
```

## 19084万万没想到之聪明的编辑

这大概是道模拟题。可以用栈来模拟这个过程 大概需要讨论一下几种情况：

1. 如果栈空 那么直接入栈

2. 如果栈不空 且当前元素等于栈顶元素 

   ①如果在此之前 栈顶元素只连续出现了一次 且不是以AAB的形式出现的话 直接入栈

   ②如果在此之前 栈顶元素只连续出现了一次 且是以AAB的形式出现的话 那么就不能入栈 

   ③如果在此之前 栈顶元素连续出现了两次 直接跳过

3. 如果栈不空 且当前元素不等于栈顶元素 直接入栈

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

void solve(){
    string s;
    cin>>s;
    bool f=false;//用于判前面有没有出现AA型
    bool f2=false;//用于判前面是AAB型还是AA型 AA型为true AAB型为false
    int cnt=0;//记录当前元素在栈顶连续出现数量
    stack<char>stk;
    for(auto&e:s){
        if(stk.empty()){
            stk.push(e);
            cnt=1;
        }else if(e==stk.top()){
            cnt++;
            if(cnt==3){
                cnt--;
                f=true;
            }else if(cnt==2&&f){
                cnt--;
                f2=false;//这里变成了AAB型
            }else {
                f2=true;
                f=true;
                stk.push(e);
            }
        }else{
            stk.push(e);
            cnt=1;
            if(!f2) f=false;//！f2说明前面出现了AAB型 加入e后 就变成了AABC型 
            f2=false;//变成了AAB型
        }
    }
    string ans="";
    while(!stk.empty()){
        ans+=stk.top();
        stk.pop();
    }
    reverse(ans.begin(),ans.end());
    cout<<ans<<"\n";
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int t;
    cin>>t;
    while(t--){
        solve();
    }
    return 0;
}
/*

*/
```

## 18936手串

不妨用$g[i][j]$表示第$i$个珠子含不含有第$j$种颜色（下面代码是对所有颜色进行了$-1$的预处理）然后遍历数组 去看$[i,i+m-1]$这个范围内有没有出现相同的颜色 不过要注意的是 同一种颜色可能会在多次不合法的情况 这种情况要去重                                                                                 

PS：这题数据大概率水了  如果不写第三重循环 直接判$g[(i+m-1)\%n][j]$是否等于1甚至都能过

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n,m,c;
    cin>>n>>m>>c;
    vector<vector<int>>g(n,vector<int>(c));
    for(int i=0;i<n;i++){
        int t;
        cin>>t;
        while(t--){
            int x;
            cin>>x;
            x--;
            g[i][x]++;
        }
    }
    vector<int>vis(c,0);
    int cnt=0;
    for(int i=0;i<n;i++){
        for(int j=0;j<c;j++){
            if(!g[i][j]) continue;
            if(vis[j]) continue;
            for(int k=1;k<m;k++){
                if(g[(i+k)%n][j]){
                    vis[j]=1;
                    cnt++;
                    break;
                }
            }
        }
    }
    cout<<cnt<<"\n";
    return 0;
}
/*

*/
```

## 18925试卷排序-双向链表

这题作者尝试使用了手写双向链表，不过是会超时的。标称给出的使用数组模拟的方式。下面给出使用STL中的$list$进行实现的方法

### 前置小知识

首先我们需要一点前置知识。

①首先 STL中的$list$是双向链表 支持增删改查以及访问元素等操作

②★★★ $list$的迭代器非常稳定 稳定说的是 除非删除这个迭代器指向的元素 否则执行其他操作 该迭代器都不会失效 而常用的$vector$在插入/扩容时迭代器会全部失效 也就是说 迭代器指向的元素会变 

那么你或许会有疑惑 如果我在$list$中每次都头插元素 则$list.begin()$指向的元素不是每次都在变吗 事实上并不是这样的 因为$.begin()$和$.end()$都不是迭代器 他们是成员函数 只不过他们的返回值是迭代器而已 

然后我们需要知道的是 $list.insert()$的三种形式：

1°$.insert(iterator,value)$：向iterator迭代器指向元素的前边添加一个元素$value$ 并返回一个迭代器指向新插入的元素

2°$.insert(iterator,num,value)$：向iterator迭代器指向的元素前 添加$num$个元素$value$ 并返回一个迭代器指向新插入的第一个元素

3°$.insert(iterator,iterator1,iterator2)$：向iterator迭代器指向的元素前添加$[iterator1,iterator2)$中的元素 并返回一个迭代器指向新插入的第一个元素

知道上述知识以后 我们其实就可以开一个数组 用于记录指向已经放入链表中的元素的迭代器 
写法就是 $vector<list<int>::iterator>g(n)$ 这样的话 我们可以在$O(1)$的复杂度内查询到要插入的位置 然后再进行插入操作

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    list<int>lst;
    lst.push_back(1);
    vector<list<int>::iterator>g(n+1);
    g[1]=lst.begin();
    for(int i=2;i<=n;i++){
        int a,b;
        cin>>a>>b;
        auto it=g[a];
        if(b) advance(it,1);
        g[i]=lst.insert(it,i);
    }
    for(auto&e:lst){
        cout<<e<<" \n"[e==lst.back()];
    }
    return 0;
}
/*

*/
```



# 扩展习题二

## 18933括号匹配问题

这是道典型的栈的匹配问题。只要对不合法的括号打个标记就可以了

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    string s;
    vector<char>g;
    while(cin>>s){
        stack<pair<char,int>>stk;
        int n=s.size();
        g.assign(n,' ');
        for(int i=0;i<n;i++){
            if(s[i]=='('){
                stk.push({'(',i});
            }else if(s[i]==')'){
                if(!stk.empty()){
                    stk.pop();
                }else{
                    g[i]='?';
                }
            }
        }
        while(!stk.empty()){
            auto it=stk.top();
            stk.pop();
            g[it.second]='$';
        }
        cout<<s<<"\n";
        for(int i=0;i<n;i++){
            cout<<g[i];
        }
        cout<<"\n";
    }
    return 0;
}
/*

*/
```

## 19009后缀表达式

用栈模拟一遍就可以了

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    char c;
    stack<int>stk;
    while(cin>>c){
        if(c=='@') break;
        if(c==' ') continue;
        if(isdigit(c)) stk.push(c-'0');
        else{
            int a=stk.top();
            stk.pop();
            int b=stk.top();
            stk.pop();
            if(c=='+') stk.push(a+b);
            else if(c=='-') stk.push(b-a);
            else if(c=='*') stk.push(a*b);
            else stk.push(b/a);
        }
    }
    cout<<stk.top()<<"\n";
    return 0;
}
/*

*/
```

## 18932出栈序列合法性判定

这也是模拟。具体做法就是如果$a[i]==b[j]$跳过 否则将$a[i]$压入栈 最后再看还在栈内元素的出栈顺序和$b$中剩下元素的顺序一不一样就可以了

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<int>a(n),b(n);
    for(int i=0;i<n;i++) cin>>a[i];
    for(int i=0;i<n;i++) cin>>b[i];
    stack<int>stk;
    int i=0,j=0;//i是a的索引 j是b的索引
    while(i<n&&j<n){
        if(a[i]==b[j]){
            i++;
            j++;
        }else if(!stk.empty()&&stk.top()==b[j]){
            stk.pop();
            j++;
        }else{
            stk.push(a[i]);
            i++;
        }
    }
    bool flag=true;
    while(!stk.empty()){
        if(stk.top()==b[j]){
            j++;
            stk.pop();
        }else{
            flag=false;
            break;
        }
    }
    cout<<(flag?"yes\n":"no\n");
    return 0;
}
/*

*/
```

## 18715出栈序列

可以用一个set来维护还没输入的元素 然后每次从set中删除当前录入元素 如果当前录入元素大于set中的最大值 就说明该出栈了 然后再判栈顶元素是否大于set中最大值   否则就入栈

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<int>g(n);
    int inx=0;
    stack<int>stk;
    set<int,greater<int>>st;
    for(int i=1;i<=n;i++) st.insert(i);
    while(n--){
        int t;
        cin>>t;
        st.erase(t);
        if(t>=*st.begin()){
            g[inx++]=t;
            while(!stk.empty()&&stk.top()>*st.begin()){
                g[inx++]=stk.top();
                stk.pop();
            }
        }else{
            stk.push(t);
        }
    }        
    while(!stk.empty()){
        g[inx++]=stk.top();
        stk.pop();
    }
    for(auto&e:g){
        cout<<e<<" \n"[e==g.back()];
    }
    return 0;
}
/*

*/
```

## 18714迷宫问题

这是道$bfs$模板题。我写的$g[row][col]='1';$这个操作其实是防止$[row,col]$反复入队 这样写还可以少开一个$vis$数组

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int dir[][2]={0,1,0,-1,1,0,-1,0};
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n,m;
    cin>>n>>m;
    vector<string>g(n);
    for(int i=0;i<n;i++){
        cin>>g[i];
    }
    queue<pii>que;
    que.push({0,0});
    g[0][0]='1';
    while(!que.empty()){
        int x=que.front().first,y=que.front().second;
        que.pop();
        if(x==n-1&&y==m-1){
            cout<<"yes\n";
            return 0;
        }
        for(int i=0;i<4;i++){
            int row=x+dir[i][0],col=y+dir[i][1];
            if(row>=0&&row<n&&col>=0&&col<m&&g[row][col]=='0'){
                que.push({row,col});
                g[row][col]='1';
            }
        }
    }
    cout<<"no\n";
    return 0;
}
/*

*/
```

## 19071递归实现指数型枚举

这是道典型递归。如果不会的话就把每层递归结果打印出来，加深理解。

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int n;
vector<int>ans;
void dfs(vector<int>&g,int sum,int cur){
    if(cur>=n) return;

    for(int i=cur;i<n;i++){
        ans.push_back(sum+g[i]);
        dfs(g,sum+g[i],i+1);
    }
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    cin>>n;
    vector<int>g(n);
    for(int i=0;i<n;i++){
        cin>>g[i];
    }
    dfs(g,0,0);
    for(auto&e:ans){
        cout<<e<<"\n";
    }
    return 0;
}
/*

*/
```

## 18712递归实现组合枚举

同样是递归。自己在练习的时候要注意递归终止条件，以及每次递归后的回溯操作，比如下面的代码中$p.pop\_back()$就是回溯。

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int n,k;
vector<vector<int>>ans;
vector<int>p;

void dfs(int cur){
    if(p.size()==k){
        ans.push_back(p);
        return;
    }
    for(int i=cur;i<=n;i++){
        p.push_back(i);
        dfs(i+1);
        p.pop_back();
    }
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    cin>>n>>k;
    dfs(1);
    for(auto&elem:ans){
        for(auto&e:elem){
            cout<<e<<" ";
        }
        cout<<"\n";
    }
    return 0;
}
/*

*/
```

## 18928递归实现排列枚举

先给出递归写法：

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int n;
vector<vector<int>>ans;
vector<int>p;
vector<int>vis;

void dfs(){
    if(p.size()==n){
        ans.push_back(p);
        return;
    }
    for(int i=1;i<=n;i++){
        if(!vis[i]){
            vis[i]=1;
            p.push_back(i);
            dfs();
            vis[i]=0;
            p.pop_back();
        }
    }
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    cin>>n;
    vis.assign(n+1,0);
    dfs();
    sort(ans.begin(),ans.end());
    for(auto&elem:ans){
        for(auto&e:elem){
            cout<<e<<" \n"[e==elem.back()];
        }
    }
    return 0;
}
/*

*/
```

这题当然可以用$next\_permutation()$函数实现：

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<int>g(n);
    for(int i=0;i<n;i++){
        g[i]=i+1;
    }
    do{
        for(int i=0;i<n;i++){
            cout<<g[i]<<" \n"[i==n-1];
        }
    }while(next_permutation(g.begin(),g.end()));
    return 0;
}
/*

*/
```

## 19044平分物品

不妨设两个人分别为$A、B$ 对于每一个物体来说 共有三种选择：给$A$  给$B$ 或者两个都不给 那其实我们可以通过递归枚举所有可能情况 答案就是$A$和$B$相等时 剩余物品价值总和的最小值

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int n;
int ans;
void dfs(vector<int>&g,int a,int b,int c,int cur){
    //a是第一个的物品价值总和 b是第二个人物品价值总和 c是剩余物品价值总和 cur是下标
    if(a==b) ans=min(ans,c);
    if(cur>=n) return;
    dfs(g,a+g[cur],b,c-g[cur],cur+1);
    dfs(g,a,b+g[cur],c-g[cur],cur+1);
    dfs(g,a,b,c,cur+1); 
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int t;
    cin>>t;
    while(t--){
        cin>>n;
        vector<int>g(n);
        int sum=0;
        for(int i=0;i<n;i++){
            cin>>g[i];
            sum+=g[i];
        }
        ans=inf;
        dfs(g,0,0,sum,0);
        cout<<ans<<"\n";
    }
    return 0;
}
/*

*/
```

## 18713整数的分解

可以怎么思考呢 比如说$n=4$ 

那我们可以先放一个1 此时$sum=1<n$ 

然后再放一个1 此时$sum=2<n$ 

再放一个1 此时$sum=3<n$ 

再放一个1 $sum=4==n$ 说明找到一个合法的解 再往下加1就超了 所以直接返回 把最后一个1弹掉 

然后放入一个2 变成$sum=1+1+1+2=5>n$ 不合法 弹掉最后一个2 

放入一个3 变成$sum=1+1+1+3=6>n$ 不合法 弹掉最后一个3

放入一个4 变成$sum=1+1+1+4=7>n$ 不合法 弹掉最后一个4                                            

此时再往下放5的话 5已经比4大 所以显然不合法 我们先弹掉最后一个4 再弹掉最后一个1                

然后放入一个2 变成$sum=1+1+2=4==n$ 合法 直接返回  以此类推

以上其实就是一个递归的过程 每次弹掉最后一个元素 就是回溯的操作  

还有要注意的是 每个解内部顺序是由小到大 所以对二维数组里的没一个一维数组都要排序 如果你是在放入二维数组前就排序的话 不能在递归传递的p数组上进行排序 否则会导致回溯错误 也就是你弹掉的元素 未必是你最后放进去的 这样就不对了 

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int n;
vector<vector<int>>ans;
vector<int>p,q;
void dfs(int sum,int now){
    if(sum==n){
        q=p;
        sort(q.begin(),q.end(),greater<>());
        ans.push_back(q);
        return;
    }else if(sum>n) return;

    for(int i=now;i<=n;i++){
        p.push_back(i);
        dfs(sum+i,i);
        p.pop_back();
    }
}
int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    cin>>n;
    dfs(0,1);
    sort(ans.begin(),ans.end(),greater<>());
    for(auto&ele:ans){
        cout<<n<<"=";
        for(auto i=0;i<ele.size();i++){
            cout<<ele[i]<<"+\n"[i==ele.size()-1];
        }
    }
    return 0;
}
/*

*/ 
```

## 18717舞伴问题

这就没什么好说了 感觉这题不应该放拓展里

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n,m,k;
    cin>>n>>m>>k;
    int a=1,b=1;
    for(int i=1;i<k;i++){
        a=a%n+1;
        b=b%m+1;
    }
    cout<<a<<" "<<b<<"\n";
    return 0;
}
/*

*/ 
```

## 18719填涂颜色

广搜的模板题。具体做法就是 先遍历上下左右四条边 如果遇到0 就广搜 把连通的0全部标记为3    

做完这一步后 如果还出现0 必然在方阵内部 分情况输出即可

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int n;
int dir[][2]={0,1,0,-1,1,0,-1,0};
void bfs(vector<vector<int>>&g,int r,int c){
    queue<pii>que;
    que.push({r,c});
    g[r][c]=3;
    while(!que.empty()){
        int x=que.front().first,y=que.front().second;
        que.pop();
        for(int i=0;i<4;i++){
            int row=x+dir[i][0],col=y+dir[i][1];
            if(row>=0&&row<n&&col>=0&&col<n&&g[row][col]==0){
                g[row][col]=3;
                que.push({row,col});
            }
        }
    }
}
int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    cin>>n;
    vector<vector<int>>g(n,vector<int>(n));
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++) cin>>g[i][j];
    }
    for(int i=0;i<n;i++){
        if(!g[0][i]) bfs(g,0,i);
        if(!g[n-1][i]) bfs(g,n-1,i);
        if(!g[i][0]) bfs(g,i,0);
        if(!g[i][n-1]) bfs(g,i,n-1);
    }
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            if(g[i][j]==3) cout<<0<<" \n"[j==n-1];
            else if(!g[i][j]) cout<<2<<" \n"[j==n-1];
            else cout<<g[i][j]<<" \n"[j==n-1];
        }
    }
    return 0;
}
/*

*/ 
```

## 18720迷宫问题2

这题和 [18714迷宫问题][#18714迷宫问题] 大差不差 只是多了一步计数

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

struct dat{
    int r,c,cnt;
};
int dir[][2]={0,1,0,-1,1,0,-1,0};
int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n,m;
    cin>>n>>m;
    vector<string>g(n);
    for(int i=0;i<n;i++) cin>>g[i];
    queue<dat>que;
    que.push({0,0,0});
    while(!que.empty()){
        int x=que.front().r,y=que.front().c,z=que.front().cnt;
        que.pop();
        if(x==n-1&&y==m-1){
            cout<<z<<"\n";
            return 0;
        }
        for(int i=0;i<4;i++){
            int row=x+dir[i][0],col=y+dir[i][1];
            if(row>=0&&row<n&&col>=0&&col<m&&g[row][col]=='0'){
                que.push({row,col,z+1});
                g[row][col]='1';
            }
        }
    }
    cout<<"-1\n";
    return 0;
}
/*

*/ 
```

## 19118用队列计算杨辉三角

用数组模拟队列跑一遍就可以了

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;
const int mod=1e9+7;
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<int>que((n+3)*n/2);
    que[0]=1,que[1]=0;
    int cur=0,inx=2;//cur指向第n-1行的数 inx是当前入队数索引
    int size=2,tmp;//size是第n-1行的大小（包括后置0） tmp用于每一次内循环结束更新size
    for(int i=1;i<n;i++){
        int s=0,t;//最开始s为0 t是每一行第一个数 即为1
        tmp=0;
        for(int j=0;j<size;j++){
            t=que[cur++];
            que[inx++]=(s+t)%mod;
            tmp++;
            s=t;
        }
        que[inx++]=0;
        size=tmp+1;
    }
    for(cur;cur<que.size()-1;cur++) cout<<que[cur]<<" ";
    cout<<"\n";
    return 0;
}
/*

*/ 
```

## 18718航行

考虑在每一个位置都有两种选择：

①如果当前的飞船能量支持飞到下一个空间站 那么可以不补充能量 直接飞

②如果当前的钱$>=b_i$ 那就可以补满能量再飞 此时应当考虑 如果最大值$M$仍不足以飞到下一个空间站 直接返回

上述过程可以通过搜索实现 如果搜到一个合法的方式能到目的地 直接返回$true$

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int n,l,m,s;
bool dfs(vector<int>&a,vector<int>&b,int inx,int pos,int p,int money){
    //inx是索引 pos是当前坐标 p是当前能量 money是当前剩余钱
    if(pos+p>=l) return true;
    else if(inx>n) return false;

    int next=inx==n?l:a[inx+1];
    if(p>=next-pos){
        if(dfs(a,b,inx+1,next,p-next+pos,money)) return true;
    }
    if(money>=b[inx]&&m>=next-pos){
        if(dfs(a,b,inx+1,next,m-next+pos,money-b[inx])) return true;
    }
    return false;
}
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    cin>>n>>l>>m>>s;
    vector<int>a(n+1),b(n+1);
    a[0]=0,b[0]=0;//分别头插一个0 就可以不用单独讨论飞船到不了第一个空间站的情况
    for(int i=1;i<=n;i++) cin>>a[i]>>b[i];
    if(dfs(a,b,0,0,m,s)) cout<<"Yes\n";
    else cout<<"No\n";
    return 0;
}
/*

*/ 
```

## 18960素数环

首先就是要预处理所有素数 ~~只有四十个数用什么欧拉筛~~

然后就是很正常的去搜一遍就可以了 因为只要一种合法的排列就可以了 所以返回值用$bool$

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int sieve[40]={0,0};
vector<int>p;
int n;
bool dfs(vector<int>&vis){
    if(p.size()==n){
        if(sieve[p.back()+p.front()]){
            for(int i=0;i<n;i++) cout<<p[i]<<" \n"[i==n-1];
            return true;
        }
        return false;
    }

    for(int i=1;i<=n;i++){
        if(!vis[i]&&(p.empty()||sieve[i+p.back()])){
            vis[i]=1;
            p.push_back(i);
            if(dfs(vis)) return true;
            vis[i]=0;
            p.pop_back();
        }
    }
    return false;
}
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    for(int i=2;i<40;i++){
        int cnt=0;
        for(int j=2;j<=sqrt(i);j++){
            if(i%j==0) cnt++;
        }
        sieve[i]=cnt?0:1;
    }//预处理所有质数
    cin>>n;
    vector<int>vis(n+1,0);
    if(!dfs(vis)) cout<<"-1\n";
    return 0;

}
/*

*/ 
```

## 18931分形 

递归好题

首先可以发现，等级为$n$的盒子应该有$3^{n-1}$行，每一级盒子都由五部分上一级盒子组成，每一部分的占据$3^n-2$行，我们可以通过简单计算算出每一部分的起始行位置，然后递归地画出整个盒子

当然，我们发现$n$最大只有5，如果你是打表大师的话，完全可以$O(1)$通过本题！

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;

vector<string>s;

void draw(int layer,int d){
    if(layer==1){
        s[d].push_back('X');
        return;
    }//最小分级

    int x=pow(3,layer-2);//当前等级占据行数
    draw(layer-1,d);//画左上角部分
    for(int i=0;i<x;i++){
        for(int j=0;j<x;j++){
            s[d+i].push_back(' ');
        }
    }//填充左上角和右上角之间的空格
    
    draw(layer-1,d);//画右上角部分

    for(int i=0;i<x;i++){
        for(int j=0;j<x;j++){
            s[d+x+i].push_back(' ');
        }
    }//填充中间部分左边的空格
    
    draw(layer-1,d+x);//画中间部分
    
    for(int i=0;i<x;i++){
        for(int j=0;j<x;j++){
            s[d+x+i].push_back(' ');
        }
    }//填充中间部分右边的空格

    draw(layer-1,d+2*x);//画左下角部分
    
    for(int i=0;i<x;i++){
        for(int j=0;j<x;j++){
            s[d+2*x+i].push_back(' ');
        }
    }//填充左下角部分和右下角部分之间的空格
    
    draw(layer-1,d+2*x);//画右下角部分
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    int m=pow(3,n-1);
    s.resize(m);

    draw(n,0);
    for(int i=0;i<m;i++){
        cout<<s[i]<<"\n";
    }
    return 0;
}


/*

*/
```

## 19070音响外放

本题思路来源于提示。

首先就是要预处理每个人所有可能的播放时长 每个人都有$2^n-1$种情况 可以用深搜解决

然后就是去枚举可能情况 一个可行的剪枝操作就是 如果假定当前最短播放时长为$t$ 如果另外三个人中至少有一个人没有比$t$更大的时长 那么可以同一个人$t$后面的时长就不用尝试了

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int arr[4][1024];
int mx,inx;
void dfs(vector<int>&g,int sum,int now,int&t){
    if(now>=g.size()) return;

    dfs(g,sum,now+1,t);
    arr[t][inx++]=sum+g[now];
    dfs(g,sum+g[now],now+1,t);
}
int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<vector<int>>g(4,vector<int>(n));
    for(int i=0;i<4;i++){
        for(int j=0;j<n;j++) cin>>g[i][j];
    }
    mx=(1<<n)-1;
    for(int i=0;i<4;i++){
        inx=0;
        dfs(g[i],0,0,i);
        sort(arr[i],arr[i]+mx);
    }
    int ans=inf;
    for(int i=0;i<4;i++){
        for(int j=0;j<mx;j++){
            int t=arr[i][j];
            bool ok=true;
            int tmp=0;
            for(int k=0;k<4;k++){
                if(k==i) continue;
                int x=lower_bound(arr[k],arr[k]+mx,t)-arr[k];
                if(x==mx){
                    ok=false;
                    break;
                }
                tmp=max(tmp,arr[k][x]);
            }
            if(ok) ans=min(ans,tmp-t);
            else break;
        }
    }
    cout<<ans<<"\n";
    return 0;
}
/*

*/ 
```

## 19120病毒扩散

虽然放在最后 但这题并不困难。首先题目中说了病毒每秒会向八个方向传递 这其实就说明这要用到广搜，然后最开始病原不唯一  且每一轮都有可能有多个人成为病原 所以是多起点广搜。          

在写的时候 首先是要多判一个时间要小于给定的$x$ 然后呢 我们每一轮都是让上一轮成为所有被感染的人去感染其他人 所以会有一个$for(int\ j=0;j<size;j++)$ ；其次，整个循环里可不能写$j<que.size()$ 因为队列的大小一直在变 这样写会导致$cnt$计数更新不准 （我本轮要取出的只是上一轮加入队列的人）

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int dir[][2]={0,1,0,-1,1,0,-1,0,1,1,1,-1,-1,1,-1,-1};
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n,m,t;
    cin>>n>>m>>t;
    vector<vector<char>>g(n,vector<char>(m));
    queue<pii>que;
    int ans=0;
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            cin>>g[i][j];
            if(g[i][j]=='@'){
                ans++;
                que.push({i,j});
            }
        }
    }
    int cnt=0;
    while(!que.empty()&&cnt<t){
        int size=que.size();
        for(int j=0;j<size;j++){
            int x=que.front().first,y=que.front().second;
            que.pop();
            for(int i=0;i<8;i++){
                int row=x+dir[i][0],col=y+dir[i][1];
                if(row>=0&&row<n&&col>=0&&col<m&&g[row][col]=='*'){
                    ans++;
                    g[row][col]='@';
                    que.push({row,col});
                }
            }
        }
        cnt++;
    }
    cout<<ans<<"\n";
    return 0;
}
/*

*/
```



# 扩展习题三 

## 18939最长单词

从前往后扫一遍就可以了。

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    string s;
    getline(cin,s);
    int start=0;
    int f=0;//标记 看是否在单词内部
    string ans;
    for(int i=0;i<s.size();i++){
        if(!isalpha(s[i])){
            string tmp=s.substr(start,i-start);
            if(tmp.size()>ans.size()) ans=tmp;
            f=0;
            start=i+1;
        }else f++;
    }
    cout<<ans<<"\n";
    return 0;
}
/*

*/ 
```

## 18942偏爱字母

把E看成1 F看成-1 就相当于是$18708最大子段和$ 记得要考虑空串

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;
  
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    vector<int>g(n);
    for(int i=0;i<n;i++){
        char c;
        cin>>c;
        g[i]=c=='E'?1:-1;
    }
    vector<int>dp(n,0);
    dp[0]=g[0];
    int ans=max(0,dp[0]);
    for(int i=1;i<n;i++){
        dp[i]=max(g[i],dp[i-1]+g[i]);
        ans=max(dp[i],ans);
    }
    cout<<ans<<"\n";
    return 0;

}
/*

*/ 
```

## 18940最小循环节 

结论：如果一个字符串含有最小循环节 那么最小循环节必然是 最长公共前后缀所不包含的部分

推导可看 [这个](https://writings.sh/post/algorithm-repeated-string-pattern#%E5%8F%8C%E5%80%8D%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%96%B9%E6%B3%95)

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;
    
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    string s;
    cin>>s;
    int n=s.size();
    vector<int>next(n,0);
    auto getnext=[&](){
        int j=0;
        for(int i=1;i<n;i++){
            while(j&&s[i]!=s[j]){
                j=next[j-1];
            }
            if(s[i]==s[j]) j++;
            next[i]=j;
        }
    };
    getnext();
    int ans;
    if(next.back()&&n%(n-next.back())==0) ans=n/(n-next.back());
    else ans=1;
    cout<<ans<<"\n";
    return 0;
}   
/* 

*/  
```

## 18941压缩算法

递归。具体做法就是当你遇到$[$以后 找循环次数 也就是当前位置和$|$之间的数字 然后去找和当前括号匹配的$]$ 后将$|$和$]$之间部分传入下一层递归 这样处理就可以了 记得要乘循环次数

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

string change(string s,int t){
    string res;
    for(int i=0;i<s.size();i++){
        if(s[i]=='['){
            int inx=find(s.begin()+i+1,s.end(),'|')-s.begin();
            int x=stoi(s.substr(i+1,inx-i-1));
            int a=0,b=0,j=i;
            for(j;j<s.size();j++){
                if(s[j]=='[') a++;
                else if(s[j]==']') b++;
                if(a==b) break;
            }
            res+=change(s.substr(inx+1,j-inx-1),x);
            i=j;
        }else res+=s[i];
    }
    string ans;
    while(t--) ans+=res;
    return ans;
}
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    string s;
    cin>>s;
    cout<<change(s,1)<<"\n";
    return 0;

}
/*

*/ 
```

## 18943小易爱回文

作者想到了一种神奇的做法 

就是把给定字符串当作主串 把给定字符串反转后的字符串当作模式串 去跑一遍$kmp$算法 然后在原字符串末尾补上模式串没有匹配上的部分 即为答案

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

void getnext(string&s,vector<int>&next){
    next[0]=0;
    int j=0;
    for(int i=1;i<s.size();i++){
        while(j&&s[i]!=s[j]) j=next[j-1];
        if(s[i]==s[j]) j++;
        next[i]=j;
    }
}
int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    string s;
    cin>>s;
    string c=s;
    reverse(c.begin(),c.end());
    vector<int>next(c.size());
    getnext(c,next);
    int j=0;
    for(int i=0;i<s.size();i++){
        while(j&&s[i]!=c[j]) j=next[j-1];
        if(s[i]==c[j]) j++;
    }
    s+=c.substr(j,c.size()-j);
    cout<<s<<"\n";
    return 0;
}
/*

*/ 
```

## 18964蛇形方阵

模拟题。注意一下边界问题以及输出格式就可以了

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<vector<int>>g(n,vector<int>(n));
    if(n&1) g[n>>1][n>>1]=n*n;
    int left=0,right=n-1,up=0,down=n-1;
    int row=0,col=0;
    int now=1;
    while(left<right||up<down){
        while(col<right) g[row][col++]=now++;
        while(row<down) g[row++][col]=now++;
        while(col>left) g[row][col--]=now++;
        while(row>up) g[row--][col]=now++;
        right--,up++;
        down--,left++;
        row=up,col=left;
    }
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            if(g[i][j]<10) cout<<"  "<<g[i][j];
            else cout<<" "<<g[i][j];
        }
        cout<<"\n";
    }
    return 0;
}
/*

*/ 
```

## 19072数字字符转化为IP地址 

搜索算法

只需要枚举三个点的位置，再判字符串是否合法即可

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    string s;
    cin>>s;
    vector<string>ans;
    auto isvalid=[&](int start,int end){
        if(start>end){
            return false;
        }

        if(s[start]=='0'&&start!=end){
            return false;
        }

        ll t=stoll(s.substr(start,end-start+1));
        if(t>=0&&t<=255){
            return true;
        }else{
            return false;
        }
    };  

    auto dfs=[&](auto&&self,int start,int cnt) -> void {
        if(cnt==3){
            if(isvalid(start,s.size()-1)){
                ans.push_back(s);
            }
            return ;
        }

        for(int i=start;i<s.size();i++){
            if(isvalid(start,i)){
                s.insert(s.begin()+i+1,'.');
                self(self,i+2,cnt+1);
                s.erase(s.begin()+i+1);
            }else{
                break;
            }
        }
    };
    
    dfs(dfs,0,0);
    for(auto c:ans){
        cout<<c<<"\n";
    }
    return 0;
}
/*

*/ 

```

# 拓展习题四

## 18723FBI树

题目已经说明了构造方式是二分递归建树 那直接写就行了

建完树后再进行后序遍历

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

struct tree{
    tree*left,*right;
    char val;
    tree(char x):val(x),left(nullptr),right(nullptr){};
};
tree*build(string&s,int l,int r){
    if(l==r){
        char c=s[l]=='0'?'B':'I';
        tree*cur=new tree(c);
        return cur;
    }
    int x=count(s.begin()+l,s.begin()+r+1,'0');
    char c;
    if(x==r-l+1) c='B';
    else if(x==0) c='I';
    else c='F';
    int mid=l+r>>1;
    tree*cur=new tree(c);
    cur->left=build(s,l,mid);
    cur->right=build(s,mid+1,r);
    return cur;
}
string ans;
void dfs(tree*cur){
    if(!cur) return ;
    dfs(cur->left);
    dfs(cur->right);
    ans.push_back(cur->val);
}
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    string s;
    cin>>s;
    tree*root=build(s,0,(1<<n)-1);
    dfs(root);
    cout<<ans<<"\n";
    return 0;
}
/*

*/ 
```

## 17263计算二叉树的第K层中所有叶子节点个数

把树建好然后跑层序遍历就行了

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

struct tree{
    char val;
    tree*left,*right;
    tree(char x):val(x),left(nullptr),right(nullptr){};
};
int inx=0;
tree*construct(string&s){
    if(inx>=s.size()) return nullptr;
    char c=s[inx++];
    if(c=='#') return nullptr;
    tree*cur=new tree(c);
    cur->left=construct(s);
    cur->right=construct(s);
    return cur;
}
int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    string s;
    int k;
    cin>>s>>k;
    tree*root=construct(s);
    int cnt=1;
    queue<tree*>que;
    que.push(root);
    while(!que.empty()&&cnt<k){
        int size=que.size();
        for(int i=0;i<size;i++){
            auto cur=que.front();
            que.pop();
            if(cur->left) que.push(cur->left);
            if(cur->right) que.push(cur->right);
        }
        cnt++;
    }
    int ans=0;
    while(!que.empty()){
        auto cur=que.front();
        que.pop();
        if(!cur->left&&!cur->right) ans++;
    }
    cout<<ans<<"\n";
    return 0;
}
/*

*/ 
```

## 18959二叉树的之字形遍历

模拟题 因为完全二叉树第$i$层共有$2^{i-1}$个节点 那么直接遍历一遍即可 对于偶数层则要进行一次反转

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;


int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    string s;
    cin>>s;
    int cur=0;
    int p=1;
    int id=0;
    while(cur<s.size()){
        string ss;
        for(int i=0;i<p&&cur<s.size();i++){
            if(s[cur]!='#') ss+=s[cur];
            cur++;
        }
        if(id) reverse(ss.begin(),ss.end());
        for(auto c:ss) cout<<c<<" ";
        cout<<"\n";
        id^=1;
        p*=2;
    }
    return 0;
}
/*

*/ 

```

## 19069二叉树的右视图

这题由于头文件限制用不了STL中的queue 所以我们要用数组模拟队列进行层序遍历 每次取每一层最后一个节点

```cpp
#include <stdio.h>
#include <malloc.h>
typedef long long ll;
using namespace std;
typedef struct BiTNode
{
    char data;
    struct BiTNode *lchild,*rchild;//左右孩子指针
} BiTNode,*BiTree;
void  CreateBiTree(BiTree &T)
{ /**< 先序建树算法 */
    char ch;
    scanf("%c",&ch);
    if (ch=='#') T = NULL;
    else
    {
        T = (BiTNode *)malloc(sizeof(BiTNode));
        T->data=ch;
        CreateBiTree(T->lchild);
        CreateBiTree(T->rchild);
    }
}
void Rview(BiTree T)
{/**< 右视图算法，用队列作为辅助存储结构 */
    BiTNode*que[1000];
    que[0]=T;
    int inx=1;//队列数组的索引 指向可以填充的第一个位置
    int cur=0;//用于遍历的索引
    int siz=1,tmp=0;//siz记录当前层节点数 tmp记录下一层节点数
    char c;
    while(siz){
        for(int i=0;i<siz;i++){
            c=que[cur]->data;
            if(que[cur]->lchild){
                tmp++;
                que[inx++]=que[cur]->lchild;
            }
            if(que[cur]->rchild){
                tmp++;
                que[inx++]=que[cur]->rchild;
            }
            cur++;
        }
        printf("%c",c);
        siz=tmp,tmp=0;
    }
}
int main()
{
    BiTree T;
    CreateBiTree(T);
    Rview(T);
    return 0;
}

```

## 19081树上摘樱桃

简单搜索

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;
 
struct tree{
    int left,right;
    tree():left(-1),right(-1){};
};
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n,m;
    cin>>n>>m;
    vector<tree>t(n);
    for(int i=0;i<m;i++){
        int u,v;
        string s;
        cin>>u>>s>>v;
        u--,v--;
        if(s=="left") t[u].left=v;
        else t[u].right=v;
    }
    int ans=0;
    auto dfs=[&](auto&&self,int u) -> bool {
        if(t[u].left==-1&&t[u].right==-1) return true;
        if(u==-1) return false;
        bool ok=self(self,t[u].left)&self(self,t[u].right);
        if(ok) ans++;
        return false;
    };
    dfs(dfs,0);
    cout<<ans<<"\n";
    return 0;
}   
/* 

*/  
```

## 19083二叉树的最长路径

不难发现 最长路径必然是某一个节点的左右子树高度之和 那么搜索一遍即可

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;

int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    string s;
    cin>>n>>s;
    int ans=0;
    auto dfs=[&](auto&&self,int u) -> int {
        if(u>=s.size()) return 0;
        else if(s[u]=='#') return 0;
        int left=self(self,2*u+1);
        int right=self(self,2*u+2);
        ans=max(ans,left+right);
        return max(left,right)+1;
    };
    dfs(dfs,0);
    cout<<ans<<"\n";
    return 0;
}   
/* 

*/  
```

## 19482嘤嘤的新平衡树 

首先肯定是先对每个节点赋值$1$，然后进行深搜，找左子树的权值和右子树的权值，记录他们的最大值为$mx$，那么为了保持平衡，这棵子树的权值必然是$2*mx+cur->weight$，$cur$是指向当前节点的指针。

感觉最困难的地方在于建树，作者至今不懂其中原理，被卡了$n$次然后对着错误提示改就对了？

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

constexpr int mod=1e9+7;

struct tree{
    char val;
    int weight;
    tree*left;
    tree*right;
    tree(char x):val(x),weight(0),left(nullptr),right(nullptr){};
};

tree*construct(string&s){
    int i=1;
    tree*root=new tree(s.front());
    root->weight=1;
    tree*fake=new tree('#');
    deque<tree*>que;
    que.push_back(root);
    while(!que.empty()&&i<s.size()){
        int size=que.size();
        int cnt=0;
        for(int j=0;j<size;j++){
            auto it=que.front();
            que.pop_front();
            if(it==fake){
                i+=2;
            }else{
                char c1=s[i++];
                if(c1=='#'){
                    it->left=nullptr;
                    if(que.back()!=fake&&cnt) que.push_back(fake);
                }else{
                    it->left=new tree(c1);
                    que.push_back(it->left);
                    it->left->weight=1;
                    cnt++;
                }
                char c2=s[i++];
                if(c2=='#'){
                    it->right=nullptr;
                    if(que.back()!=fake&&cnt) que.push_back(fake);
                }else{
                    it->right=new tree(c2);
                    que.push_back(it->right);
                    it->right->weight=1;
                    cnt++;
                }
            }
        }

    }
    return root;
}

int dfs(tree*cur){
    if(!cur){
        return 0;
    }

    int l=dfs(cur->left);
    int r=dfs(cur->right);
    int mx=max(l,r);
    return (2*mx+cur->weight);
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    string s;
    cin>>s;
    tree*root=construct(s);
    if(s.front()=='#'){
        cout<<0<<"\n";
        return 0;
    }

    int t=dfs(root);
    cout<<t<<"\n";
    return 0;
}


/*
ABCDE##FG####HI####
31
15

A
B           C
D        E   ##
F   G   ## ##
H I ## #  #
*/
```

## 18946小美的送花线路

到各个点的路程之和是好算的 我们主要关注最短线路

可以发现 最后一个到达的点必然是 距离根节点最远的点

最短路径就是 $（所有主路的路程+所有支路的路程）*2 -最远距离$ 

这里主路指的是 从根节点出发 到第一个出度大于1的节点的路径

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;
   
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<vector<pii>>adj(n);
    for(int i=1;i<n;i++){
        int u,v,w;
        cin>>u>>v>>w;
        u--,v--;
        adj[u].push_back({v,w});
    }
    ll mx=0;
    ll res=0;
    auto dfs=[&](auto&&self,int u,ll sum) -> ll {
        res+=sum;
        if(adj[u].empty()){
            mx=max(mx,sum);
            return 0LL;
        }
        ll tmp=0;
        for(auto c:adj[u]){
            tmp+=self(self,c.first,sum+c.second)+c.second;
        }
        return tmp;
    };
    ll ans=2*dfs(dfs,0,0LL);
    ans-=mx;
    cout<<res<<" "<<ans<<"\n";
    return 0;
}   
/* 

*/  
```

## 18929最优二叉树 

首先给定的是中序遍历，我们不知道应该以谁为根，因此需要枚举所有点作为根，然后根据选定的点的位置，左侧数作为左子树，右侧数作为右子树，发现仍需要枚举左儿子和右儿子，于是可以通过递归实现。

在搜索过程中，不难发现会搜到很多种重复情况，因此采用记忆化搜索，定义$dp[fa][l][r]$为以$fa$索引为根，$[l,r]$这个索引区间内的数作为子树的最小代价，然后递归搜索即可

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    vector<int>g(n+1);
    for(int i=1;i<=n;i++){
        cin>>g[i];
    }
    vector<vector<vector<ll>>>dp(n+2,vector<vector<ll>>(n+2,vector<ll>(n+2,-1)));
    auto dfs=[&](auto&&self,int fa,int l,int r) -> ll {
        if(l>r){
            return 0LL;
        }
        if(dp[fa][l][r]!=-1){
            return dp[fa][l][r];
        }
        
        ll ans=INF;
        for(int i=l;i<=r;i++){
            ll lhs,rhs;
            if(dp[i][l][i-1]!=-1){
                lhs=dp[i][l][i-1];
            }else{
                lhs=self(self,i,l,i-1);
                dp[i][l][i-1]=lhs;
            }
            if(dp[i][i+1][r]!=-1){
                rhs=dp[i][i+1][r];
            }else{
                rhs=self(self,i,i+1,r);
                dp[i][i+1][r]=rhs;
            }
            ans=min(ans,lhs+rhs+g[i]*g[fa]);
        }
        dp[fa][l][r]=ans;
        return ans;
    };
    dfs(dfs,0,1,n);
    cout<<dp[0][1][n]<<"\n";
    return 0;
}
/*

*/ 

```



# 拓展习题五

## 18726查找最接近的元素

笨蛋题

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;

int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<int>g(n);
    for(int i=0;i<n;i++) cin>>g[i];
    int m;
    cin>>m;
    while(m--){
        int x;
        cin>>x;
        int res=lower_bound(g.begin(),g.end(),x)-g.begin()+1;
        cout<<res<<"\n";
    }
    return 0;
}   
/* 

*/  
```

## 18935贪吃的小Q 

简单二分

由于$n$不大，我们的$check$函数可以完整循环$n$天，看会消耗多少巧克力，如果消耗巧克力小于等于初始值$m$，说明检查的这个$mid$值是有效的

注意如果某天吃的巧克力数量$x$为奇数，那么下一天至少得吃$⌈\frac{x}{2}⌉$这么多（$⌈\;⌉$为上取整符号）

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n,m;
    cin>>m>>n;
    int l=1,r=n;
    int ans;
    while(l<=r){
        int mid=l+r>>1;
        auto check=[](int n,int m,int mid){
            int sum=0;
            while(m--){
                sum+=mid;
                mid=mid+1>>1;
            }
            return sum<=n;
        };
        if(check(n,m,mid)){
            ans=mid;
            l=mid+1;
        }else{
            r=mid-1;
        }
    }
    cout<<ans<<'\n';
}
/*

*/ 

```

## 19023砍树 

答案具有单调性 于是考虑二分答案

$check$函数也很好写 对于每个$mid$值 只需要遍历一遍数组看能得到多少木材 最后判$sum$是否大于等于$m$

时间复杂度$O(nlogN)$

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n,m;
    cin>>n>>m;
    vector<int>g(n);
    for(int i=0;i<n;i++){
        cin>>g[i];
    }
    ll l=0,r=2e9+10;
    ll ans;
    while(l<=r){
        ll mid=l+r>>1;
        auto check=[&](){
            ll sum=0;
            for(int i=0;i<n;i++){
                sum+=max(0LL,g[i]-mid);
            }
            return sum>=m;
        };
        if(check()){
            ans=mid;
            l=mid+1;
        }else{
            r=mid-1;
        }
    }
    cout<<ans<<"\n";
    return 0;
}
/*

*/ 

```

## 18725宇宙跃迁 

这种最小值/最大值问题往往可以考虑二分，因为具有单调性

考虑二分答案，$check$函数只需要判到达终点时所需天数是否小于等于$k$即可

显然$check$函数可以在$O(n)$范围内实现，具体操作就是对于当前索引$inx$，定义一个快指针$j$往后跳，保证$g[j]-g[inx]<=mid$即可

注意要判$j==inx$的情况

总复杂度$O(nlogn)$

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n,k;
    cin>>n>>k;
    vector<int>g(n+1);
    for(int i=1;i<=n;i++){
        cin>>g[i];
    }
    int l=0,r=g.back();
    int ans;
    while(l<=r){
        int mid=l+r>>1;
        auto check=[&](){
            int inx=0;
            int cnt=0;
            while(inx<n){
                cnt++;
                int j=inx;
                while(j+1<=n&&g[j+1]-g[inx]<=mid){
                    j++;
                }
                if(j==inx){
                    return false;
                }
                inx=j;
            }
            return cnt<=k;
        };  
        if(check()){
            ans=mid;
            r=mid-1;
        }else{
            l=mid+1;
        }
    }
    cout<<ans<<"\n";
    return 0;
}
/*

*/ 

```

## 19050牛牛打气球

可以首先考虑如下两种情况：①如果所有数字都小于$b$ 那么只需要一轮就可以让所有数变成0
②如果每一轮都不使用额外技能 那么需要的次数就是$⌈mx/b⌉$（$⌈\ \ ⌉$符号代表上取整 $mx$是数组中的最大值）                                                                                                                                      可以发现 答案一定就在这个区间范围内 那么可以采取二分答案的方式
那么$check$函数应该怎么写呢 其实我们只要看额外使用技能的次数是否小于当前的$mid$值就可以了

这里有一个额外小知识点补充 我们都知道 如果有两个整型变量$a$、$b$ 直接写$a/b$的话实际上是下取整 那么如果要上取整有没有什么通用写法呢 是有的：$⌈a/b⌉=(a+b-1)/b$

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int n,a,b;
bool check(vector<ll>&g,ll mid){
    ll cnt=0;
    for(int i=0;i<n;i++){
        if(g[i]-b*mid>0){
            cnt+=(g[i]-b*mid+a-1)/a;
        }
        if(cnt>mid) return false;
    }
    return cnt<=mid;
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    cin>>n>>a>>b;
    vector<ll>g(n);
    for(int i=0;i<n;i++) cin>>g[i];
    ll l=1,mid;
    ll r=(*max_element(g.begin(),g.end())+b-1)/b;
    while(l<r){
        mid=l+r>>1;
        if(check(g,mid)) r=mid;
        else l=mid+1;
    }
    cout<<l<<"\n";
    return 0;
}
/*

*/ 
```

## 18730涂色问题

这是个数学问题。

首先$n$间房子 $m$种颜色 总的情况数就是$m^n$ 种 然后正难则反 我们可以去找不让奶牛发疯的情况 那么就是除了第一间屋子有$m$种颜色可选 其他屋子都只有$m-1$种情况 总数就是$m*(m-1)^{n-1}$种情况 两者相减就是答案

由于幂过大 我们可以用快速幂优化

要注意的是 两者相减完一定要再加一个$mod$再%$mod$ 因为取完模以后本来大的数未必还是更大的

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;
const int mod=1e9+7;

ll ksm(ll a,ll b){
    ll res=1;
    a%=mod;
    while(b){
        if(b&1) res=res*a%mod;
        a=a*a%mod;
        b>>=1;
    }
    return res;
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    
    ll m,n;
    cin>>n>>m;
    ll ans=(ksm(m,n)-m*ksm(m-1,n-1)%mod+mod)%mod;
    cout<<ans<<"\n";
    return 0;
}
/*

*/
```

## 18727数对问题一

排完序以后只用找比当前数大C的数字个数就行了

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n,k;
    cin>>n>>k;
    map<int,int>mp;
    vector<int>g(n);
    for(int i=0;i<n;i++){
        cin>>g[i];
        mp[g[i]]++;
    }
    sort(g.begin(),g.end());
    int ans=0;
    for(int i=0;i<n;i++){
        ans+=mp[g[i]+k];
    }
    cout<<ans<<"\n";
    return 0;
}
/*

*/ 
```

## 18728数对问题二

和上题思路一样 不过为了防止 $g[i]+k$ 超过int范围溢出 可以从后往前找比当前数字小C的数字个数
PS：实际上把上题代码交上去也没啥问题

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n,k;
    cin>>n>>k;
    map<int,int>mp;
    vector<int>g(n);
    for(int i=0;i<n;i++){
        cin>>g[i];
        mp[g[i]]++;
    }
    sort(g.begin(),g.end());
    int ans=0;
    for(int i=n-1;i>=0;i--){
        ans+=mp[g[i]-k];
    }
    cout<<ans<<"\n";
    return 0;
}
/*

*/ 
```

## 19145最长无重复子数组 

滑动窗口模板题 

遍历右端点 将右端点对应值放入multiset中

然后如果集合中右端点对应值数量超过一 说明$[l,i]$这个子数组不合题意 应当收缩左端点 直到集合中右端点对应值的数量为1

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;
     
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<int>g(n);
    for(int i=0;i<n;i++) cin>>g[i];
    int ans=1;
    multiset<int>st;
    int l=0;
    for(int i=0;i<n;i++){
        st.insert(g[i]);
        while(st.count(g[i])>1){
            st.erase(st.find(g[l++]));
        }
        ans=max(ans,i-l+1);
    }
    cout<<ans<<"\n";
    return 0; 
}   
/* 

*/  
```

## 18731最接近的值 

只需要用$multiset$去存储左边的所有值然后二分即可

值得注意的是：

如果写的是$lower\_bound(st.begin(),st.end())$的话，复杂度会是$O(n)$的，在这题中会超时

而$set、map$其实有自己的成员函数$lower\_bound$，这个的时间复杂度是$O(logn)$的

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    multiset<int>st;
    for(int i=0;i<n;i++){
        int x;
        cin>>x;
        if(!i){
            cout<<x<<" ";
            st.insert(x);
            continue;
        }
        auto it=st.lower_bound(x);
        if(it==st.end()){
            advance(it,-1);
            cout<<*it<<" \n"[i==n-1];
        }else if(it==st.begin()){
            cout<<*it<<" \n"[i==n-1];
        }else{
            auto itt=it;
            advance(itt,-1);
            if(abs(*itt-x)<abs(*it-x)){
                cout<<*itt<<" \n"[i==n-1];
            }else{
                cout<<*it<<" \n"[i==n-1];
            }
        }
        st.insert(x);
    }
    return 0;
}
/*

*/ 

```

## 18870最佳搭配 

从前往后贪心即可

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    vector<int>a(n);
    multiset<int>st;
    for(int i=0;i<n;i++){
        cin>>a[i];
    }
    for(int i=0;i<n;i++){
        int x;
        cin>>x;
        st.insert(x);
    }
    for(int i=0;i<n;i++){
        auto it=st.lower_bound(n-a[i]);
        if(it==st.end()) it=st.begin();
        a[i]=(a[i]+*it)%n;
        st.erase(it);
    }   
    for(int i=0;i<n;i++){
        cout<<a[i]<<" \n"[i==n-1];
    }
    return 0;
}
/*

*/ 

```

## 19066第K小子串 

由于不需要重复的串，故考虑使用$set$维护

当$set$中串的数量大于$k$时，我们只需要把最后的串（即目前字典序最大的串）去掉即可

最后答案就是$*st.rbegin()$

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    string s;
    int k;
    cin>>s>>k;
    int n=s.size();
    set<string>st;
    for(int i=0;i<n;i++){
        string tmp;
        for(int j=i;j<n;j++){
            tmp+=s[j];
            st.insert(tmp);
            if(st.size()>k){
                st.erase(*st.rbegin());
            }
        }
    }
    cout<<*st.rbegin()<<"\n";
    return 0;
}
/*

*/ 

```

## 18907雪花 雪花 雪花 

题意是说如果两片雪花相同，那么就是第二片雪花通过循环左移或者右移能和第一片雪花完全相同

如果我们暴力匹配所有雪花，复杂度$O(n^2)$显然不能过

于是我们考虑用哈希的方法去判是否有重

相当于要把一个长度为$6$的数组映射为一个哈希值

又因为雪花数组可以不断进行循环移位，因此我们可以找出雪花的字典序最小的排列，这个可以通过正着枚举一遍+反着枚举一遍得到，然后把数组看成一个$P$进制数，从前往后是高位向低位，把他转成一个哈希值，于是就完成了映射

当然，这题的数组长度最多为$6$，完全可以用$map<vector<int>,int>$这种方式进行哈希

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

constexpr int P=1331;
int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int T;
    cin>>T;
    int n=6;
    vector<int>a(n),b(n);
    auto getmin=[&](){
        auto c=a;
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                b[j]=a[(i+j)%n];
            }
            c=min(c,b);
        }

        reverse(a.begin(),a.end());
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                b[j]=a[(i+j)%n];
            }
            c=min(c,b);
        }

        ll h=0;
        for(int i=0;i<n;i++){
            h=h*P+c[i];
        }
        return h;
    };
    bool ok=false;
    map<ll,int>mp;
    for(int i=0;i<T;i++){
        for(int j=0;j<n;j++){
            cin>>a[j];
        }
        ll h=getmin();
        if(mp[h]){
            ok=true;
            break;
        }else{
            mp[h]++;
        }
    }
    if(ok) cout<<"Twin snowflakes found.\n";
    else cout<<"No two snowflakes are alike.\n";
    return 0;
}
/*

*/ 

```

## 18908字符串哈希 

字符串哈希本质上是将字符串看成一个$P$进制数，我们可以算出这个$P$进制数对$mod$取模的结果，再进行哈希，要求$P$和$mod$都是大素数，且$P<mod$

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

constexpr int P=131;
constexpr int mod=1e9+7;
int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    string s;
    cin>>s;
    int n=s.size();
    vector<ll>p(n+1),h(n+1);
    p[0]=1;
    for(int i=1;i<=n;i++){
        h[i]=h[i-1]*P+s[i-1]-'a';
        h[i]%=mod;
        p[i]=p[i-1]*P;
        p[i]%=mod;
    }
    int t;
    cin>>t;
    while(t--){
        int a,b,c,d;
        cin>>a>>b>>c>>d;
        if((h[b]-h[a-1]*p[b-a+1]%mod+mod)%mod==(h[d]-h[c-1]*p[d-c+1]%mod+mod)%mod){
            cout<<"Yes\n";
        }else{
            cout<<"No\n";
        }
    }
    return 0;
}
/*

*/ 

```

## 18922最长回文子串 

中心拓展法可求最长回文字串。如果回文串长度是奇数的话，我们枚举每个中心点$i$进行扩展；如果回文串长度是偶数的话，以中心点$i,i+1$进行扩展。不难发现这个做法的复杂度是$O(n^2)$，并且涉及大量重复计算，$dp$法的本质是拿空间换时间，记录结果，避免重复计算。

一个很有意思的事情是，这题数据范围给到了$1e6$，实测也有$1e6$的数据，那你如果要开个二维$dp$数组必然是会超时的。但是，这题数据弱了！完全没有给出最长回文字串差长度在$1e5$及以上的情况，于是完全可以用中心扩展法硬冲过去！

（根据作者二分的结果，这题数据的最大输出答案只有$11$，甚至样例不在测试用例中，数据之弱令人震惊）

* 正解应该是$Manacher$算法，作者还不会，于是不写。

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    string s;
    cin>>s;
    int n=s.size();
    auto find=[&](int l,int r){
        while(l>=0&&r<n&&s[l]==s[r]){
            l--;
            r++;
        }
        return r-l-1;
    };
    int ans=1;
    for(int i=0;i<n;i++){
        ans=max(ans,find(i,i));
        if(i+1<n) ans=max(ans,find(i,i+1));
    }
    cout<<ans<<'\n';
    return 0;
}
/*

*/ 

```

## 18944小美的仓库整理 

考虑到要多次查询区间内元素和，因此不难想到要维护一个前缀和数组

由于是不放回取出，因此我们可以维护所有被取出的位置，称为断点

每次取出一个数，产生的影响实际上就是将它数值上相邻的两个断点之间的元素和分成了两部分

我们需要快速找到相邻的两个断点，可以考虑维护一个$set$

为了保证处理逻辑的一致性，我将前缀和数组开到了$n+2$，然后最开始放入断点$0$和$n+1$

然后就是处理每堆的和，这个可以通过前缀和快速处理得到，详见代码

要注意的是，在$multiset$中使用$erase(x)$的话，会把所有$x$值全删掉，要只删一个的话应该采用$st.erase(st.find(x))$这种邪恶发

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    vector<int>pre(n+2,0);
    for(int i=1;i<=n;i++){
        int x;
        cin>>x;
        pre[i]=pre[i-1]+x;
    }
    pre[n+1]=pre[n];
    set<int>interval;
    multiset<int>val;
    interval.insert(0),interval.insert(n+1);
    val.insert(pre[n]);
    for(int i=0;i<n;i++){
        int x;
        cin>>x;
        auto next=interval.lower_bound(x);
        auto prev=next;
        advance(prev,-1);
        val.erase(val.find(pre[*next-1]-pre[*prev]));
        val.insert(pre[x-1]-pre[*prev]);
        val.insert(pre[*next-1]-pre[x]);
        interval.insert(x);
        cout<<*val.rbegin()<<"\n";
    }
    return 0;
}
/*

*/ 

```

## 18716出栈序列 

同[拓展习题二](#18715出栈序列)

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<int>g(n);
    int inx=0;
    stack<int>stk;
    set<int,greater<int>>st;
    for(int i=1;i<=n;i++) st.insert(i);
    while(n--){
        int t;
        cin>>t;
        st.erase(t);
        if(t>=*st.begin()){
            g[inx++]=t;
            while(!stk.empty()&&stk.top()>*st.begin()){
                g[inx++]=stk.top();
                stk.pop();
            }
        }else{
            stk.push(t);
        }
    }        
    while(!stk.empty()){
        g[inx++]=stk.top();
        stk.pop();
    }
    for(auto&e:g){
        cout<<e<<" \n"[e==g.back()];
    }
    return 0;
}
/*

*/
```

## 19008哈希表 

用一个$set$储存所有没有被用到的位置下标

然后模拟即可

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    set<int>st;
    for(int i=0;i<n;i++){
        st.insert(i);
    }
    int cnt=0;
    vector<int>res(n);
    for(int i=0;i<n;i++){
        int x;
        cin>>x;
        int pos=x%n;
        auto it=st.lower_bound(pos);
        if(it==st.end()) it=st.begin();
        if(*it==pos) cnt++;
        res[*it]=x;
        st.erase(it);
    }
    cout<<cnt<<"\n";
    for(int i=0;i<n;i++){
        cout<<res[i]<<" \n"[i==n-1];
    }
    return 0;
}
/*

*/ 

```

## 19040序列合并

首先对两个数组进行排序

然后由于两个数组都是单调不降的 那么必然有：

①$a_x+b_y<=a_{x+1}+b_y$

②$a_x+b_y<=a_x+b_{y+1}$

于是考虑用优先队列处理 使用小顶堆 每次弹出堆顶 然后将${a_x+b_{y+1}}$,${a_{x+1}+b_y}$放入队列中 注意要去重

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

	int n;
    cin>>n;
    vector<int>a(n),b(n);
    for(int i=0;i<n;i++) cin>>a[i];
    for(int i=0;i<n;i++) cin>>b[i];
    sort(a.begin(),a.end());
    sort(b.begin(),b.end());
    priority_queue<array<int,3>,vector<array<int,3>>,greater<>>que;
    que.push({a[0]+b[0],0,0});
    int inx=0;
    set<pii>st;
    st.insert({0,0});
    while(inx<n){
        int sum=que.top()[0];
        int x=que.top()[1];
        int y=que.top()[2];
        que.pop();
        if(x+1<n&&!st.count({x+1,y})){
            que.push({a[x+1]+b[y],x+1,y});
            st.insert({x+1,y});
        }
        if(y+1<n&&!st.count({x,y+1})){
            st.insert({x,y+1});
            que.push({a[x]+b[y+1],x,y+1});
        }
        cout<<sum<<" \n"[inx==n-1];
        inx++;
    }
    return 0; 
}   
/* 

*/  
```

## 18873团队实力 

首先如果人数要求变小了，优先踢出的肯定是能力小的，考虑用堆维护

我们可以考虑按照限制的降序对数组排序，优先放限制大的，然后根据限制的变化去动态调整堆的大小

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

struct dat{
    int a,v;

    bool operator<(const dat&d)const{
        if(a==d.a) return v>d.v;
        else return a>d.a;
    }
};
int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    vector<dat>g(n);
    for(int i=0;i<n;i++){
        cin>>g[i].a>>g[i].v;
    }
    sort(g.begin(),g.end(),[](const dat&x,const dat&y){
        if(x.v==y.v) return x.a>y.a;
        else return x.v>y.v;
    });
    ll ans=0,sum=0;
    int mini=inf;
    priority_queue<dat>que;
    for(int i=0;i<n;i++){
        mini=min(mini,g[i].v);
        while(que.size()>mini-1){
            sum-=que.top().a;
            que.pop();
        }
        que.push(g[i]);
        sum+=g[i].a;
        ans=max(ans,sum);
    }
    cout<<ans<<"\n";
    return 0;
}
/*

*/ 

```

* PS：数据应该弱了，我把

```cpp
	for(int i=0;i<n;i++){
        mini=min(mini,g[i].v);
        while(que.size()>mini-1){
            sum-=que.top().a;
            que.pop();
        }
        que.push(g[i]);
        sum+=g[i].a;
        ans=max(ans,sum);
    }
```

写成

```cpp
	for(int i=0;i<n;i++){
        mn=min(mn,g[i].y);
        sum+=g[i].x;
        que.push(g[i]);
        while((int)que.size()>mn){
            sum-=que.top().x;
            que.pop();
        }
        ans=max(ans,sum);
    }
```

都能过。下面这种写法似乎完全有可能出现：$g[i]$刚放进去就被提出队列了，也就是说更新后的堆的大小其实小于堆内元素的最小人数限制。



# 拓展习题六

## 18746逆序对 

树状数组的模板题。

思路大概是：

首先记录初始每个数的位置 然后对数组按值从大到小进行排序 

然后可以看作是 将排序后的数组中的数 按照原来的位置插入新数组 

考虑如果我当前插入的位置位$inx$ 那么如果$[inx+1,n]$这个位置内已经有数插入 说明形成了逆序对

然后求总和即可

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

template<typename T>
struct Fenwick{
    vector<T>a;
    int n;

    Fenwick(int n_=0){
        n=n_+1;
        init();
    }

    void init(){
        a.assign(n,T{});
    }

    void add(int pos,T k){
        for(int i=pos;i<n;i+=i&-i){
            a[i]+=k;
        }
    }

    T sum(int pos){
        T ans{};
        for(int i=pos;i;i-=i&-i){
            ans+=a[i];
        }
        return ans;
    }

    T rangesum(int l,int r){
        if(!l) return sum(r);
        else return sum(r)-sum(l-1);
    }

    int kth(T k){
        int x=0;
        for(int i=1<<__lg(n);i;i>>=1){
            if(x+i<=n&&a[x+i]<k){
                k-=a[x+i];
                x+=i;
            }
        }
        return x+1;
    }
};

struct dat{
    int id;
    int val;
};

int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

	int n;
    cin>>n;
    vector<dat>g(n);
    for(int i=0;i<n;i++){
        cin>>g[i].val;
        g[i].id=i+1;
    }
    sort(g.begin(),g.end(),[](const dat&x,const dat&y){
        if(x.val==y.val) return x.id<y.id;
        else return x.val<y.val;
    });
    map<int,int>pos;
    for(int i=0;i<n;i++){
        pos[i]=g[i].id;
    }
    Fenwick<ll>fen(n);
    ll ans=0;
    for(int i=0;i<n;i++){
        int inx=pos[i];
        ans+=fen.rangesum(inx,n);
        fen.add(inx,1);
    }
    cout<<ans<<"\n";
    return 0; 
}   
/* 

*/  
```

* 注意①：答案范围会爆$int$ 
* 注意②：在排序的时候 如果两个数值相同 一定是$id$小的数字在前面 否则会算错贡献 这个可以自己手推一下看看

## 18967六一儿童节 

考虑大巧克力先满足大胃口的孩子  或者小巧克力先满足小胃口的孩子

局部最优可以推出全局最优 并且举不出反例

于是考虑贪心 从小往大贪或者从大往小贪都是对的

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;
   
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

	int n;
    cin>>n;
    vector<int>h(n);
    for(int i=0;i<n;i++) cin>>h[i];
    int m;
    cin>>m;
    vector<int>w(m);
    for(int i=0;i<m;i++) cin>>w[i];
    sort(h.begin(),h.end());
    sort(w.begin(),w.end());
    int ans=0;
    int j=0;
    for(int i=0;i<m&&j<n;i++){
        if(w[i]>=h[j]){
            ans++;
            j++;
        }
    }
    cout<<ans<<"\n";
    return 0; 
}   
/* 

*/  
```

## 18962区间合并

贪心 首先将所有区间按照升序排序 然后合并

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;
   
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<pii>g(n);
    for(int i=0;i<n;i++){
        cin>>g[i].first>>g[i].second;
    }
    sort(g.begin(),g.end());
    vector<pii>ans;
    int l=g[0].first,r=g[0].second;
    for(int i=1;i<n;i++){
        if(g[i].first<=r){
            r=max(r,g[i].second);
        }else{
            ans.push_back({l,r});
            l=g[i].first,r=g[i].second;
        }
    }
    ans.push_back({l,r});
    for(auto c:ans){
        cout<<c.first<<" "<<c.second<<"\n";
    }
    return 0;
}   
/* 

*/  
```

## 18963最大数字 

排序即可

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;

int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<string>g(n);
    for(int i=0;i<n;i++) cin>>g[i];
    sort(g.begin(),g.end(),[](const string&a,const string&b){
        return a+b>=b+a;
    });
    for(int i=0;i<n;i++) cout<<g[i];
    cout<<"\n";
    return 0;
}   
/* 

*/  
```

## 18965找到K个最接近的元素 

虽然打了四星，实际不难

首先要把字符串处理成数组，考虑在字符串最后加入一个“，”，便于统一处理

这样每次查找下一个逗号位置，然后将中间的字符串转为数字即可

要找到与x差值最小的k个数，可以先对数组进行排序，然后二分找到最接近x的位置，再双指针左右移动取判当前取哪个值即可

注意双指针移动过程中要考虑有越界的情况

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    string s;
    int k,x;
    cin>>s>>k>>x;
    int n=s.size();
    vector<int>g,res;
    s.push_back(',');
    for(int i=0;i<n;i++){
        int inx=find(s.begin()+i,s.end(),',')-s.begin();
        int tmp=stoi(s.substr(i,inx-i));
        g.push_back(tmp);
        i=inx;
    }
    sort(g.begin(),g.end());
    int l=lower_bound(g.begin(),g.end(),x)-g.begin(),r=l+1;
    for(int i=0;i<k;i++){
        if(l<0){
            res.push_back(g[r++]);
        }else if(r>=n){
            res.push_back(g[l--]);
        }else if(abs(g[l]-x)<=abs(g[r]-x)){
            res.push_back(g[l--]);
        }else{
            res.push_back(g[r++]);
        }
    }
    sort(res.begin(),res.end());
    for(int i=0;i<k;i++){
        cout<<res[i]<<",\n"[i==k-1];
    }
    return 0;
}
/*

*/ 

```

## 18966两两配对差值最小 

实际上就是让每组的和尽可能接近

那么考虑让每组的两个加数尽可能接近

于是可以让第一个加数由小到大选，第二个加数由大到小选

使用双指针实现

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    vector<int>g(n);
    for(int i=0;i<n;i++) cin>>g[i];
    sort(g.begin(),g.end());
    int l=0,r=n-1;
    int mn=inf,mx=0;
    while(l<=r){
        int tmp=g[l]+g[r];
        mx=max(mx,tmp);
        mn=min(mn,tmp);
        l++,r--;
    }
    cout<<mx-mn<<"\n";
    return 0;
}
/*

*/ 

```

## 18961最大点的集合 

注意到 如果存在多个这样的“最大点”，那么随着横坐标增大 ，对应纵坐标必然减小

于是我们可以对所有点进行增序排序 然后从后向前查找

最后一个点必然是“最大点”，向前遍历过程中，如果出现纵坐标更大的点，那说明找到了新的“最大点”

最后再倒序输出即可

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    vector<array<int,2>>g(n);
    for(int i=0;i<n;i++){
        for(int j=0;j<2;j++){
            cin>>g[i][j];
        }
    }
    sort(g.begin(),g.end());
    vector<array<int,2>>res;
    res.push_back(g[n-1]);
    int mx=g[n-1][1];
    for(int i=n-2;i>=0;i--){
        if(g[i][1]>mx){
            mx=g[i][1];
            res.push_back(g[i]);
        }
    }
    for(int i=res.size()-1;i>=0;i--){
        cout<<res[i][0]<<" "<<res[i][1]<<"\n";
    }
    return 0;
}
/*

*/ 

```

## 19018正则序列 

一个很显然的做法就是先排序 然后计算差值 时间复杂度为$O(nlogn)$

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<int>g(n);
    for(int i=0;i<n;i++) cin>>g[i];
    sort(g.begin(),g.end());
    int ans=0;
    for(int i=0;i<n;i++) ans+=abs(i+1-g[i]);
    cout<<ans<<"\n";
    return 0;

}
/*

*/ 
```

## 18958有趣的排序 

不难发现 要保留的位置必然是从最小数开始的最长不递减子序列 

数据范围极小 可以直接通过$multiset$模拟实现

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    vector<int>g(n);
    multiset<int>st;
    for(int i=0;i<n;i++){
        cin>>g[i];
        st.insert(g[i]);
    }
    for(int i=0;i<n;i++){
        if(g[i]==*st.begin()){
            st.erase(st.begin());
        }
    }
    cout<<st.size()<<"\n";
    return 0;
}
/*

*/ 

```

## 19065有趣的数字 

首先对数组排序 最大差就是最大数-最小数 最小差通过遍历数组 取相邻元素差的最小值得到

不难发现的是 我们对于相同的数字只需要处理计算一次 因此可以先去重

然后遍历数组求解 可以利用$map$或者是$set$等容器快速确定有没有与当前元素匹配的数

注意要判最大差为0 最小差为0的情况

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;
      
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    map<int,int>cnt;
    vector<int>g(n);
    for(int i=0;i<n;i++){
        cin>>g[i];
        cnt[g[i]]++;
    }
    sort(g.begin(),g.end());
    int mn=inf,mx=g.back()-g.front();
    ll a=0,b=0;
    for(int i=1;i<g.size();i++) mn=min(mn,g[i]-g[i-1]);
    g.erase(unique(g.begin(),g.end()),g.end());
    for(int i=0;i<g.size();i++){
        if(cnt.find(g[i]+mn)!=cnt.end()){
            if(mn) a+=1LL*cnt[g[i]]*cnt[g[i]+mn];
            else a+=1LL*(cnt[g[i]]-1)*cnt[g[i]]/2;
        }
        if(cnt.find(g[i]+mx)!=cnt.end()){
            if(mx) b+=1LL*cnt[g[i]]*cnt[g[i]+mx];
            else b+=1LL*(cnt[g[i]]-1)*cnt[g[i]]/2;
        }
    }
    cout<<a<<" "<<b<<"\n";
    return 0; 
}   
/* 

*/  
```



# 拓展习题七

## 19085图的存储结构（邻接表）

这就没什么好说的了

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n,m;
    cin>>n>>m;
    vector<vector<int>>adj(n+1);
    while(m--){
        int u,v;
        cin>>u>>v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    for(int i=1;i<=n;i++){
        cout<<i<<":";
        for(auto&e:adj[i]){
            cout<<e<<" ";
        }
        cout<<"\n";
    }
    return 0;
}
/*

*/ 
```

## 19082中位特征值

意思就是 从叶子节点开始 返回以当前节点为根的子树价值和 那么跑一遍深搜就可以了

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

vector<ll>ans;
ll dfs(vector<vector<int>>&adj,vector<int>&g,int f,int u){
    ll t=0;
    for(auto e:adj[u]){
        if(e!=f) t+=dfs(adj,g,u,e);
    }
    t+=g[u];
    ans.push_back(t);
    return t; 
}
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int r,n;
    cin>>r>>n;
    vector<int>g(n+1);
    for(int i=1;i<=n;i++) cin>>g[i];
    vector<vector<int>>adj(n+1);
    for(int i=1;i<n;i++){
        int u,v;
        cin>>u>>v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    dfs(adj,g,0,r);
    sort(ans.begin(),ans.end());
    cout<<ans[n/2]<<"\n";
    return 0;
}
/*

*/ 
```

## 19031树的重心 

对于树上的任一节点$u$，要获取它的所有子树大小是容易的，可以通过一次深搜实现

去掉该点$u$后，$u$节点往上到根节点的连通块大小可以用$n-sum-1$得到，其中$sum$为$u$的子树大小

于是只需要枚举每个点即可得到答案，可通过一次深搜实现

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n;
    cin>>n;
    vector<vector<int>>adj(n+1);
    for(int i=1;i<n;i++){
        int u,v;
        cin>>u>>v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    adj[1].push_back(0);
    vector<int>g(n+1,0);
    auto dfs=[&](auto&&self,int f,int cur) -> int {
        if(adj[cur].size()==1){
            g[cur]=n-1;
            return 1;
        }
        int sum=0;
        int mx=-1;
        for(auto c:adj[cur]){
            if(c==f) continue;
            int tmp=self(self,cur,c);
            sum+=tmp;
            mx=max(tmp,mx);
        }
        g[cur]=max(mx,n-sum-1);
        return sum+1;
    };
    dfs(dfs,0,1);
    int mn=*min_element(g.begin()+1,g.end());
    for(int i=1;i<=n;i++){
        if(g[i]==mn){
            cout<<i<<" ";
        }
    }
    cout<<"\n";
    return 0;
}
/*

*/ 

```

## 19011小猿的依赖循环 

题意就是有环输出1 无环输出0

拓扑排序即可

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

void solve(){
    int n;
    cin>>n;
    vector<vector<int>>adj(n);
    vector<int>indeg(n,0);
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            int x;
            cin>>x;
            if(x){
                adj[j].push_back(i);
                indeg[i]++;
            }
        }
    }
    int cnt=0;
    queue<int>que;
    for(int i=0;i<n;i++){
        if(!indeg[i]){
            que.push(i);
            cnt++;
        }
    }
    while(!que.empty()){
        int x=que.front();
        que.pop();
        for(auto c:adj[x]){
            indeg[c]--;
            if(!indeg[c]){
                que.push(c);
                cnt++;
            }
        }
    }
    cout<<(cnt==n?0:1)<<"\n";
}       
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int T=1;
    cin>>T;
    while(T--){
        solve();
    }

    return 0; 
}   
/* 

*/  
```

## 19017编译依赖问题 

拓扑排序模板题

首先就是字符串的处理可以在末尾添加一个逗号 这样就不用单独处理最后一个数

其次要注意的是 这题要求的是在度为0的情况下 优先输出编号小的数字 所以要跑n轮 每轮都要遍历一遍数组

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;
    
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

	string s;
    cin>>s;
    vector<int>a;
    s.push_back(',');
    int qi=0;
    while(qi<s.size()){
        int inx=find(s.begin()+qi,s.end(),',')-s.begin();
        a.push_back(stoi(s.substr(qi,inx-qi)));
        qi=inx+1;
    }
    int n=a.size();
    vector<vector<int>>adj(n);
    vector<int>indeg(n,0);
    for(int i=0;i<n;i++){
        if(a[i]!=-1){
            adj[a[i]].push_back(i);
            indeg[i]++;
        }
    }
    vector<int>ans;
    vector<int>vis(n,0);
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            if(!vis[j]&&!indeg[j]){
                vis[j]=1;
                ans.push_back(j);
                for(auto c:adj[j]){
                    indeg[c]--;
                }
                break;
            }
        }
    }
    for(int i=0;i<n;i++) cout<<ans[i]<<",\n"[i==n-1];
    return 0; 
}   
/* 

*/  
```

## 19032树上上升序列 

最开始我们的想法肯定会是枚举每一个点作为起点 看它的最长合法路径是多少

但如果直接搜索的话 复杂度会爆

然后我们观察发现 这其中其实涉及到了很多重复计算

比如说 如果我们已经搜索得到了$x$为起点的最长路径 然后我们搜索以$y$为起点的最长路径 假设$y$能到达$x$并且路径合法 那我们就会再搜一遍以$x$为起点的最长路径

因此我们考虑记忆化搜索

（节点范围我们固定为$[0,n-1]$）

我们用$dp[i]$表示以节点$i$ 搜索过程中如果遇到已经搜过的节点 直接返回直接搜的结果 否则 往下搜索并且更新沿途节点的$dp$值

由于要求路径是上升的 因此必然不会出现走回头路的情况

这样的话就可以在$O(n)$复杂度内解决这个问题

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;
     
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

	int n;
    cin>>n;
    vector<int>g(n);
    for(int i=0;i<n;i++) cin>>g[i];
    vector<vector<int>>adj(n);
    for(int i=1;i<n;i++){
        int u,v;
        cin>>u>>v;
        u--,v--;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }
    vector<int>dp(n,0);
    auto dfs=[&](auto&&self,int cur) -> int {
        if(dp[cur]) return dp[cur];
        dp[cur]=1;
        int mx=0;
        for(auto c:adj[cur]){
            if(g[c]>g[cur]) mx=max(mx,self(self,c));
        } 
        dp[cur]+=mx;
        return dp[cur];
    };
    for(int i=0;i<n;i++){
        if(!dp[i]) dfs(dfs,i);
    }
    int ans=*max_element(dp.begin(),dp.end());
    cout<<ans<<"\n";

    return 0; 
}   
/* 

*/  
```

## 18733排队

并查集（DSU）的模板题。题意就是 把$x$和$y$合并进同一个集合 然后把$x$的祖先修改为$y$的祖先 最后再输出每个人的祖先分别是谁

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
const ll INF=0x3f3f3f3f3f3f3f3fll;
const int inf=0x3f3f3f3fll;

struct DSU{
    vector<int>f;
    DSU(){};
    DSU(int n){
        init(n+1);
    }

    void init(int n){
        f.resize(n);
        for(int i=0;i<n;i++){
            f[i]=i;
        }
    }
    
    int find(int u){
        while(u!=f[u]){
            u=f[u]=f[f[u]];
        }
        return u;
    }

    // bool same(int u,int v){
    //     return find(u)==find(v);
    // }

    void merge(int u,int v){
        u=find(u);
        v=find(v);
        if(u==v) return;
        f[v]=u;
    }
};
int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n,m;
    cin>>n>>m;
    DSU dsu(n);
    while(m--){
        int x,y;
        cin>>x>>y;
        dsu.merge(y,x);
    }
    for(int i=1;i<=n;i++){
        cout<<dsu.find(i)<<" \n"[i==n];
    }
    return 0;
}
/*

*/ 
```

## 18945小团的配送团队

$a$和$b$同一个小区 等价于将$a$和$b$加入同一个集合

考虑使用并查集 由于题目要求先输出最小订单编号最小的小区 我们可以进行的一个处理是：

将当前集合最小编号设为这个集合的祖先 

合并完以后在跑一遍看每个集合都有谁 这可以用邻接表储存

最后输出即可

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

struct DSU{
    vector<int>f;
    DSU(){};
    DSU(int n){
        init(n);
    }

    void init(int n){
        f.resize(n+1);
        for(int i=0;i<=n;i++) f[i]=i;
    }

    int find(int u){
        while(u!=f[u]){
            u=f[u]=f[f[u]];
        }
        return u;
    }

    bool same(int u,int v){
        return find(u)==find(v);
    }

    bool merge(int u,int v){
        u=find(u);
        v=find(v);
        if(u==v){
            return false;
        }
        if(u>v){
            swap(u,v);
        }
        f[v]=u;
        return true;
    }

}; 
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n,m;
    cin>>n>>m;
    DSU dsu(n);
    for(int i=0;i<m;i++){
        int u,v;
        cin>>u>>v;
        dsu.merge(u,v);
    }
    vector<vector<int>>adj(n+1);
    int cnt=0;
    for(int i=1;i<=n;i++){
        int x=dsu.find(i);
        adj[x].push_back(i);
        if(adj[x].size()==1) cnt++;
    }
    cout<<cnt<<"\n";
    for(int i=1;i<=n;i++){
        if(adj[i].empty()) continue;
        sort(adj[i].begin(),adj[i].end());
        for(auto c:adj[i]) cout<<c<<" ";
        cout<<"\n";
    }
    return 0; 
}   
/* 

*/  
```

## 18946小美的送花线路

同[拓展习题四](#18946小美的送花线路)

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3fll;
   
int main(){ 
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);

    int n;
    cin>>n;
    vector<vector<pii>>adj(n);
    for(int i=1;i<n;i++){
        int u,v,w;
        cin>>u>>v>>w;
        u--,v--;
        adj[u].push_back({v,w});
    }
    ll mx=0;
    ll res=0;
    auto dfs=[&](auto&&self,int u,ll sum) -> ll {
        res+=sum;
        if(adj[u].empty()){
            mx=max(mx,sum);
            return 0LL;
        }
        ll tmp=0;
        for(auto c:adj[u]){
            tmp+=self(self,c.first,sum+c.second)+c.second;
        }
        return tmp;
    };
    ll ans=2*dfs(dfs,0,0LL);
    ans-=mx;
    cout<<res<<" "<<ans<<"\n";
    return 0;
}   
/* 

*/  
```

## 19049仓库配送 

最短路的模板题

```cpp
#include<bits/stdc++.h>

using namespace std;
using ll=long long;
using ull=unsigned long long;
using pii=pair<int,int>;
using pll=pair<ll,ll>;
constexpr ll INF=0x3f3f3f3f3f3f3f3fll;
constexpr int inf=0x3f3f3f3f;

int main(){
    ios::sync_with_stdio(false);    
    cin.tie(nullptr);
    
    int n,k,m;
    cin>>n>>k>>m;
    k--;
    vector<vector<pii>>adj(n);
    for(int i=0;i<m;i++){
        int u,v,w;
        cin>>u>>v>>w;
        u--,v--;
        adj[u].push_back({v,w});
    }
    vector<int>dis(n,inf),vis(n,0);
    dis[k]=0;
    priority_queue<pii,vector<pii>,greater<>>que;
    que.push({0,k});
    while(!que.empty()){
        int diss=que.top().first,cur=que.top().second;
        que.pop();
        if(vis[cur]) continue;
        vis[cur]=1;
        for(auto c:adj[cur]){
            int v=c.first,w=c.second;
            if(!vis[v]&&dis[v]>diss+w){
                dis[v]=diss+w;
                que.push({dis[v],v});
            }
        }
    }
    int ans=0;
    for(int i=0;i<n;i++){
        if(dis[i]==inf){
            ans=-1;
            break;
        }
        ans=max(ans,dis[i]);
    }
    cout<<ans<<"\n";
    return 0;
}
/*

*/ 

```

