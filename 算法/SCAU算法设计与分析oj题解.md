

**By 三石吖 2026.2**

* 搜索部分基本都没写
* 递归和分治差最后一题，课后习题差$1$

# 1.一般方法

## 9715 相邻最大矩形面积

最终形成矩形的高度必然不超过给定高度的最大值，于是考虑枚举所有高度作为最终矩形高度

形式化地，对于每个位置$i$，有高度$a[i]$，我们要找在它左侧小于$a[i]$的第一个位置$l[i]$，在它右侧小于$a[i]$的第一个位置$r[i]$ ，那么以$a[i]$为高度的矩形面积就是 $S = a[i] \times (r[i] - l[i] - 1)$

于是考虑单调栈，我们维护一个单调递增的栈，对于每个位置$i$，首先弹出栈内大于等于$a[i]$的位置，此时若栈非空，则找到了第一个小于$a[i]$的位置，正反各跑一遍即可

初始化：若左侧找不到比$a[i]$更小的值，将$l[i]$置为$-1$，若右侧找不到，将$r[i]$置为$n$（上述索引均从$0$开始）

* 时间复杂度$O(n)$

  

* 看半天不知道为啥 $n$ 范围 $1e5$ 时限 $1s$ 推荐$O(n^2)$的做法，测了一下测试数据中$n$不超过70。。

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>a(n);
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
    }
    std::vector<int>l(n, -1), r(n, n), stk;
    for(int i = 0; i < n; i++){
        while(!stk.empty() && a[stk.back()] >= a[i]){
            stk.pop_back();
        } 
        if(!stk.empty()){
            l[i] = stk.back();
        }
        stk.push_back(i);
    }
    stk.clear();
    for(int i = n - 1; i >= 0; i--){
        while(!stk.empty() && a[stk.back()] >= a[i]){
            stk.pop_back();
        } 
        if(!stk.empty()){
            r[i] = stk.back();
        }
        stk.push_back(i);
    }
    i64 ans = 0;
    for(int i = 0; i < n; i++){
        ans = std::max(ans, 1LL * a[i] * (r[i] - l[i] - 1));
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 10345 前缀平均值

记录出现值的和，再除$i$即可，由于不涉及多次询问，不需要单独开一个前缀和数组

* 时间复杂度$O(n)$

* 学校$oj$不能用$std::setprecision()$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    double sum = 0;
    for(int i = 1; i <= n; i++){
        double x;
        std::cin >> x;
        sum += x;
        printf("%.2f%c", sum / i, " \n"[i == n]);
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11075 强盗分赃

假设初始有$n$个金币，第一个强盗拿走$1$个，剩$n - 1$个，分成五份后留下四份，剩$\frac{4(n - 1)}{5}$

第二个强盗拿走一个，剩$\frac{4n - 9}{5}$个，分成五份后留下四份，剩$\frac{16n - 36}{25}$

第三个强盗拿走一个，剩$\frac{16n - 61}{25}$个，分成五份后留下四份，剩$\frac{64n - 244}{125}$

第四个强盗拿走一个，剩$\frac{64n - 369}{125}$个，分成五份后留下四份，剩$\frac{256n - 1476}{625}$

第五个强盗拿走一个，剩$\frac{256n - 2101}{625}$个，分成五份后留下四份，剩$\frac{1024n - 8404}{3125}$

数据范围$1e8$，考虑枚举初始的金币数$n$，满足$(1024n - 8404) \ \% 3125 \ = 0$即合法

* 注意乘法可能爆$int$

* 时间复杂度$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>res;
    for(int i = 1; i <= n; i++){
        if((1024LL * i - 8404) % 3125 == 0){
            res.push_back(i);
        }
    }
    if(res.empty()){
        std::cout << "impossible\n";
        return;
    }
    for(int i = 0; i < res.size(); i++){
        std::cout << res[i] << " \n"[i == res.size() - 1];
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11076 浮点数的分数表达

提示讲的很详细了，这里搬运一下提示：

（1）假设输入为有限小数：$X \ = \ 0.a_1a_2...a_n$，其中$a_1a_2...a_n$都是数字$0-9$

$X \ = \ 0.a_1a_2...a_n \ = \ \frac{a_1a_2...a_n}{10^n}$

然后约分即可

（2）假设输入为无限循环小数：$X \ = \ 0.a_1a_2...a_n(b_1b_2...b_m)$，其中$a_1a_2...a_n$，$b_1b_2...b_m$都是数字$0-9$，括号内为循环节

①先将$X$转化为只有循环部分的纯小数

$X \ = \ 0.a_1a_2...a_n(b_1b_2...b_m)$，左右同乘$10^n$

$10^n \times X \ = \ a_1a_2...a_n \ + \ 0.(b_1b_2...b_m)$

 上式中，$a_1a_2...a_n$是整数部分，可单独处理

②然后考虑循环节部分的转化

令$Y \ = \ 0.(b_1b_2...b_m)$，左右同乘$10^m$

$10^m \times Y \ = \ b_1b_2...b_m \ + \ 0.(b_1b_2...b_m)$，左右同时$- Y$，消去小数点后的循环节

$(10^m \ - \ 1) \times Y \ = \ b_1b_2...b_m$

得$Y \ = \ \frac{b_1b_2...b_m}{10^m \ - \ 1}$

将①②步结果相加：$X \ = \ \frac{a_1a_2...a_n}{10^n} \ + \ \frac{b_1b_2...b_m}{10^m \ - \ 1} \ = \ \frac{(10^m \ - \ 1) \times (a_1a_2...a_n) \ + \ (b_1b_2...b_m)}{(10^m - 1) \times 10^n}$

最后约分即可

* 时间复杂度$O(n)$，$n$为字符串长度
* 代码中的$p1$为$10^n$，$p2$为$10^m$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){   
    std::string s;
    std::cin >> s;
    int n = s.size();
    i64 a = 0, b = 0, p1 = 1, p2 = 1;
    for(int i = 2; i < n && s[i] != '('; i++){
        a = a * 10 + (s[i] - '0');
        p1 *= 10;
    }
    i64 up, down, g;
    if(s.back() == ')'){
        for(int i = n - 2; i >= 0 && s[i] != '('; i--){
            b += p2 * (s[i] - '0');
            p2 *= 10;
        }
        up = (p2 - 1) * a + b, down = (p2 - 1) * p1;
        g = std::__gcd(up, down);
    }else{
        up = a, down = p1;
        g = std::__gcd(up, down);
    }
    std::cout << up / g << " " << down / g << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 17963 完美数

按提示模拟即可

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int a[] = {2, 3, 5, 7, 13, 17, 19, 31};
    for(int i = 0; i < 8; i++){
        i64 res = (1LL << (a[i] - 1)) * ((1LL << a[i]) - 1);
        std::cout << i + 1 << " " << res << "\n";
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 17086 字典序的全排列

法一：直接用$C++$的$STL$模拟

* 时间复杂度$O(n! \times n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::string s;
    std::cin >> n >> s;
    std::sort(s.begin(), s.end());
    int cnt = 1;
    do{
        std::cout << cnt << " " << s << "\n";
        cnt++;
    }while(std::next_permutation(s.begin(), s.end()));
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



法二：写个真的$dfs$，由于字符串不含重复元素，因此可以先对字符串排序，后进行深搜，这样必然是按字典序升序排列的

* 时间复杂度$O(n! \times n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::string s;
    std::cin >> n >> s;
    std::sort(s.begin(), s.end());
    int cnt = 1;
    std::string v;
    std::vector<int>vis(n);
    auto dfs = [&] (auto &&self) -> void {
        if(v.size() == n){
            std::cout << cnt << " " << v << "\n";
            cnt++;
            return;
        }

        for(int i = 0; i < n; i++){
            if(!vis[i]){
                vis[i] = 1;
                v.push_back(s[i]);
                self(self);
                v.pop_back();
                vis[i] = 0;
            }
        }
    };
    dfs(dfs);
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 8593  最大覆盖问题

根据定义，一个合法的区间受到最右侧元素的约束，考虑枚举右端点

题目要求线性的做法，于是考虑双指针

发现正着跑双指针好像很困难，右端点从$i$位置到$i + 1$位置，左端点的移动不具有规律性

于是想到反着跑双指针

首先固定右端点$i$，对于左端点$j$，满足$a[j] < | \ a[i] \ |$即可进行左移，最后合法的区间就是$(j,i]$，即对$j + 1 \le k \le i$，都有$a[k] \le | \ a[i]\ |$

此时若$j \ge 0$，我们移动右端点，找到第一个$| \ a[i'] \ | \ge a[j]$的位置$i'$，这样就找到了一个新的右端点，使得$j$能够被覆盖，此时$[j,i']$这个区间必然是合法的

证明：

设$j \lt k \le i'$，首先必然有$a[k] \le |\ a[i]\ |$，又$| \ a[i] \ | < a[j]$，则$a[j] > a[k]$

由于$| \ a[i'] \ | \ge a[j]$，可推出$| \ a[i'] \ | \ge a[j] \gt a[k]$，说明合法

* 对于每个位置，最多左右指针各经过一次，时间复杂度$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>a(n);
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
    }
    int ans = 1;
    for(int i = n - 1, j = n - 1; i >= 0; i--){
        if(std::abs(a[i]) < a[j]){
            continue;
        }
        while(j >= 0 && a[j] <= std::abs(a[i])){
            j--;
        }
        ans = std::max(ans, i - j);
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



如果不限制复杂度$O(n)$，这题有没有别的做法呢，好像是有的！

法二：

观察到答案显然不超过$n$，并且具有单调性，也就是说，如果存在一个长度为$x$的合法区间，那么长度小于$x$的合法区间必然存在，于是想到二分答案

于是问题变成了，给定一个长度$d$，如何检查是否存在长度为$d$的合法区间

首先对于$i \lt d - 1$，区间长度不足$d$，不考虑

一个合法的区间满足：区间最大值不超过右端点值的绝对值

对于静态区间最值，不难想到用$st$表维护，于是可以枚举右端点询问最值进行检查

* $st$表预处理复杂度$O(logn)$，二分答案复杂度$O(logn)$，每次检查需要$O(n)$，询问最值$O(1)$

* 总复杂度$O(nlogn)$
* 数据范围要是达到$1e7$这个做法会超时

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

template<typename T>
struct SpareTable{
    int n;
    std::vector<std::vector<T>>stmx, stmn;
    
    SpareTable(std::vector<T>a){
        init(a);
    }

    void init(std::vector<T>a){
        n = a.size();
        stmx.resize(n), stmn.resize(n);
        int siz = std::__lg(2 * n - 1);
        for(int i = 0; i < n; i++){
            stmx[i].assign(siz, T{});
            stmn[i].assign(siz, T{});
            stmx[i][0] = stmn[i][0] = a[i];
        }
        for(int j = 1; 1 << j <= n; j++){
            for(int i = 0; i + (1 << j) - 1 < n; i++){
                stmx[i][j] = std::max(stmx[i][j - 1], stmx[i + (1 << j - 1)][j - 1]);
                stmn[i][j] = std::min(stmn[i][j - 1], stmn[i + (1 << j - 1)][j - 1]);
            }
        }
    }

    T askmn(int l, int r){
        int k = std::__lg(r - l + 1);
        return std::min(stmn[l][k], stmn[r - (1 << k) + 1][k]);
    }

    T askmx(int l, int r){
        int k = std::__lg(r - l + 1);
        return std::max(stmx[l][k], stmx[r - (1 << k) + 1][k]);
    }
};
void solve(){
    int n;
    std::cin >> n;
    std::vector<int>a(n);
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
    }
    SpareTable<int>st(a);
    int ans, lo = 1, hi = n;
    auto check = [&] (int mid){
        for(int i = mid - 1; i < n; i++){
            if(st.askmx(i - mid + 1, i) <= std::abs(a[i])){
                return true;
            }
        }
        return false;
    };
    while(lo <= hi){
        int mid = lo + hi >> 1;
        if(check(mid)){
            ans = mid;
            lo = mid + 1;
        }else{
            hi = mid - 1;
        }
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



# 2.递归与分治

## 8594 有重复元素的排列问题

重点是知道有重复元素排列问题的去重方式，至于产生排列的顺序，会一种即可，本题做法了解即可

* 本题做法生成排列的顺序未必是字典序最优的

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){   
    int n;
    std::string s;
    std::cin >> n >> s;
    int tot = 0;
    auto dfs = [&] (auto &&self, int inx) -> void {
        if(inx >= n){
            std::cout << s << "\n";
            tot++;
            return;
        }

        auto check = [&] (int start, int end){
            for(int i = start; i < end; i++){
                if(s[i] == s[end]){
                    return true;
                }
            }
            return false;
        };

        for(int i = inx; i < n; i++){
            if(check(inx, i)){
                continue;
            }
            std::swap(s[i], s[inx]);
            self(self, inx + 1);
            std::swap(s[i], s[inx]);
        }
    };

    dfs(dfs, 0);
    std::cout << tot << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 17088 分治法求众数

不会分治法。

排序法：

先排序然后双指针扫一遍

* 时间复杂度$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){   
    int n;
    std::cin >> n;
    std::vector<int>a(n);
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
    }
    std::sort(a.begin(), a.end());
    int cnt = 0, ans;
    for(int i = 0, j; i < n; i = j){
        j = i;  
        while(j < n && a[j] == a[i]){
            j++;
        }
        if(j - i > cnt){
            cnt = j - i;
            ans = a[i];
        }
    }
    std::cout << ans << " " << cnt << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 9714 圣诞礼物

发现好像可以直接$dp$！

考虑用$dp[i][j]$ 表示将$i$个礼物放进$j$个盒子，且每个盒子都不空的方法数

由于限制了每个盒子都不空，我们可以先往每个盒子中放进一个礼物，那么还剩$i - j$个礼物，这$i - j$个礼物可以放进$1,2...j$个盒子中

于是得到了状态转移方程：$dp[i][j] = \sum_{k=0}^{j}dp[i-j][k]$

初始化$dp[0][0] = 1$，答案就是$\sum_{j=0}^{min(m,n)}dp[m][j]$（因为$m$个礼物最多放入$m$个盒子，因此可以优化掉）

* 时间复杂度$O(m \times n \times n)$，此处的$n$为$min(n, m)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){ 
    int m, n;
    std::cin >> m >> n;
    n = std::min(m, n);
    std::vector<std::vector<int>>dp(m + 1, std::vector<int>(n + 1));
    dp[0][0] = 1;
    for(int i = 1; i <= m; i++){
        for(int j = 1; j <= std::min(n, i); j++){
            for(int k = 0; k <= j ; k++){
                dp[i][j] += dp[i - j][k];
            }
        }
    }
    int ans = 0;
    for(int i = 0 ; i <= n; i++){
        ans += dp[m][i];
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



求和部分可以用前缀和优化，这样每次加法都是$O(1)$的

* 时间复杂度$O(m \times n)$，其中$n = min(n,m)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){ 
    int m, n;
    std::cin >> m >> n;
    n = std::min(m, n);
    std::vector<std::vector<int>>dp(m + 1, std::vector<int>(n + 1));
    std::vector<std::vector<int>>pre(m + 1, std::vector<int>(n + 1));
    dp[0][0] = 1;
    for(int i = 0; i <= n; i++){
        pre[0][i] = 1;
    }
    for(int i = 1; i <= m; i++){
        for(int j = 1; j <= std::min(n, i); j++){
            dp[i][j] += pre[i - j][j];
        }
        for(int j = 1; j <= n; j++){
            pre[i][j] = pre[i][j - 1] + dp[i][j];
        }
    }
    int ans = 0;
    for(int i = 0 ; i <= n; i++){
        ans += dp[m][i];
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 9718 整数因子分解

设$f(n)$为$n$的分解式数量，依次枚举$n$的大于$1$的因数，将它们固定在第一位，问题转化为求$\sum_{i=1}^{m}f(\frac{n}{x_i}) \quad (n \% x_i = 0)$

问题可以拆解为若干子问题，考虑递归求解

其中涉及大量重复计算，可用记忆化搜索优化

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){ 
    int n;
    std::cin >> n;
    std::vector<int>f(n + 1);
    f[1] = 1;

    auto dfs = [&](auto &&self, int cur) -> int {
        if(f[cur]){
            return f[cur];
        }

        for(int i = 2; i <= std::sqrt(cur); i++){
            if(cur % i){
                continue;
            }
            f[cur] += self(self, cur / i);
            if(i * i != cur){
                f[cur] += self(self, i);
            }
        }
        f[cur]++;
        return f[cur];
    };

    int ans = dfs(dfs, n);
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 10302 整数的特殊划分

好像有很好的做法！

由于$2$的幂次增长极快，$n$以内的$2$的幂次其实并不多，就算$n$是$1e5$级别，那也只会有$17$种不同的$2$的幂次

换句话说，我们可以先在常数级的复杂度内找到所有小于$n$的$2$的幂次，设其数量为$m$

现在问题变成了 给定$m$个不同的数，以及一个正整数$n$，每个数可以重复取，问有多少种组成$n$的方法数

或许这还不够直观，那么可以看成：

给定$m$个不同的物品，以及一个容量$n$，每个物品可以重复取，问有多少种容量为$n$的填充方式

发现显然就是 **完全背包** 的板子题，直接$dp$即可

* 时间复杂度$O(nlogn)$，$logn$为物品数量

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

constexpr int mod = 1000000000;
void solve(){
    int n;
    std::cin >> n;
    std::vector<int>a;
    for(int i = 1; i <= n; i *= 2){
        a.push_back(i);
    }
    int m = a.size();
    std::vector<i64>dp(n + 1);
    dp[0] = 1;
    for(int i = 0; i < m; i++){
        for(int j = a[i]; j <= n; j++){
            dp[j] = (dp[j] + dp[j - a[i]]) % mod;
        }
    }
    std::cout << dp[n] << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 10304 平面域着色

首先判掉不可能的情况：

①$k=1$且$n \gt 1$

②$k=2$且$n$为奇数

用$dp[i]$表示用$k$种颜色给$i$个区域染色的方法数

①若第$i-1$个区域和第$1$个区域同色，等价于给前$i-2$个区域染色的方法数，此时第$i$个区域有$k-1$种染色方式，即为$dp[i-2] \times (k-1)$

②若第$i-1$个区域和第$1$个区域不同色，此时第$i$个区域有$k-2$种染色方式，为$dp[i-1] \times (k-2)$

即：$dp[i]=dp[i-2] \times (k-1) + dp[i-1] \times (k-2)$

要初始化前三个数

* 时间复杂度$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, k;
    std::cin >> n >> k;
    if(k == 1 && n > 1){
        std::cout << "0\n";
        return;
    }else if(k == 2 && n % 2){
        std::cout << "0\n";
        return;
    }

    if(n == 1){
        std::cout << k << "\n";
        return;
    }else if(n == 2){
        std::cout << k * (k - 1) << "\n";
        return;
    }else if(n == 3){
        std::cout << k * (k - 1) * (k - 2) << "\n";
        return;
    }

    std::vector<i64>dp(n + 1);
    dp[1] = k, dp[2] = k * (k - 1), dp[3] = k * (k - 1) * (k - 2);
    for(int i = 4; i <= n; i++){
        dp[i] = dp[i - 2] * (k - 1) + dp[i - 1] * (k - 2);
    }
    std::cout << dp[n] << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 10343 划分凸多边形

给每个顶点按顺时针顺序标上$1,2,..n$，顶点$1,n,i$可以将凸多边形划分成一个三角形，一个$i$边形，一个$n-i+1$边形，然后分解成了两个子问题，递归求解即可

* 时间复杂度$O(n^2)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    if(n < 3){
        std::cout << "No answer\n";
        return;
    }
    std::vector<int>f(n + 1 , -1);
    f[2] = f[3] = 1;

    auto dfs = [&](auto &&self, int x) -> int {

        if(f[x] != -1){
            return f[x];
        }

        f[x] = 0;
        for(int i = 2; i < x; i++){
            f[x] += self(self, i) * self(self, x - i + 1);            
        }

        return f[x];
    };  

    int ans = dfs(dfs, n);
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 10344 矩阵连乘积的加括号方式数

卡特兰数太难

可以看看https://oi-wiki.org/math/combinatorics/catalan/

本题就是求卡特兰数

* 时间复杂度$O(n^2)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    if(n <= 2){
        std::cout << "1\n";
        return;
    }else if(n == 3){
        std::cout << "2\n";
        return;
    }

    std::vector<int>f(n + 1);
    f[1] = f[2] = 1;
    f[3] = 2;

    auto dfs = [&](auto &&self, int x) -> int {
        if(f[x]){
            return f[x];
        }

        for(int i = 1; i < x; i++){
            f[x] += self(self, i) * self(self, x - i);
        }
        return f[x];
    };

    int ans = dfs(dfs, n);
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```







## 11073最热门的$K$个搜索串

维护一个大小为$k$的小顶堆，当堆的大小大于$k$时，弹出堆顶

* 时间复杂度$O(nlogk)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){   
    int n, k;
    std::cin >> n >> k;
    std::priority_queue<int, std::vector<int>, std::greater<>>que;
    for(int i = 0; i < n; i++){
        int x;
        std::cin >> x;
        que.push(x);
        while(que.size() > k){
            que.pop();
        }
    }
    std::vector<int>res;
    while(!que.empty()){
        res.push_back(que.top());
        que.pop();
    }
    for(int i = res.size() - 1; i >= 0; i--){
        std::cout << res[i] << " \n"[i == 0];
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



* 如果不要求选中的$k$个元素有序，可以用$std::nth\_element()$

* 时间复杂度$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){   
    int n, k;
    std::cin >> n >> k;
    std::vector<int>a(n);
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
    }
    std::nth_element(a.begin(), a.begin() + k, a.end(), std::greater<>());
    for(int i = 0; i < n; i++){
        std::cout << a[i] << " \n"[i == n - 1];
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11074 平面分割

看提示吧，好困难。

* 时间复杂度$O(1)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    i64 a = 1LL * n * (n + 1) / 2 + 1;
    i64 b = 1LL * n * n - n + 2;
    i64 c = 2LL * n * n - n + 1;
    i64 d = (9LL * n * n - 7 * n) / 2 + 1;
    std::cout << a << " " << b << " " << c << " " << d << "\n"; 
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11081 邮局选址

中位数定理，对横坐标和纵坐标分别选中位数即可

* 时间复杂度$O(nlogn)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>x(n), y(n);
    for(int i = 0; i < n; i++){
        std::cin >> x[i] >> y[i];
    }
    std::sort(x.begin(), x.end());
    std::sort(y.begin(), y.end());
    int r = x[n / 2], c = y[n / 2];
    int ans = 0;
    for(int i = 0; i < n; i++){
        ans += std::abs(r - x[i]) + std::abs(c - y[i]);
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11082 完全二叉树的种类（卡特兰数）

卡特兰数太难

可以看看https://oi-wiki.org/math/combinatorics/catalan/

本题就是求卡特兰数

* 时间复杂度$O(n^2)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    if(n <= 2){
        std::cout << "1\n";
        return;
    }else if(n == 3){
        std::cout << "2\n";
        return;
    }

    std::vector<int>f(n + 1);
    f[1] = f[2] = 1;
    f[3] = 2;

    auto dfs = [&](auto &&self, int x) -> int {
        if(f[x]){
            return f[x];
        }

        for(int i = 1; i < x; i++){
            f[x] += self(self, i) * self(self, x - i);
        }
        return f[x];
    };

    int ans = dfs(dfs, n);
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11085 买票找零（卡特兰数）

用$f[cnt][x][y]$表示有$cnt$张$50$元可以找零，还有$x$人持$50$元，$y$人持$100$元的方法数

然后跑记忆化搜索

初始化：$x=0$时，若$y \le cnt$，则$f[cnt][x][y]=1$

时间复杂度：$O(n^3)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

constexpr int N = 20;
int f[N][N][N];

void init(){
    for(int i = 0; i < N; i++){
        for(int j = 0; j < N; j++){
            for(int k = 0; k < N; k++){
                f[i][j][k] = -1;
            }
        }
    }

    for(int i = 0; i < N; i++){
        for(int j = 0; j <= i; j++){
            f[i][0][j] = 1;
        }
    }
}
void solve(){
    init();

    int n;
    std::cin >> n;

    auto dfs = [&](auto &&self, int cnt, int x, int y) -> int {
        if(f[cnt][x][y] != -1){
            return f[cnt][x][y];
        }

        f[cnt][x][y] = 0;
        if(x){
            f[cnt][x][y] += self(self, cnt + 1, x - 1, y);
        }
        if(cnt && y){
            f[cnt][x][y] += self(self, cnt - 1, x, y - 1);
        }
        return f[cnt][x][y];
    };

    int ans = dfs(dfs, 0, n, n);
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 17083 多重幂计数问题

还是卡特兰数。

* 时间复杂度$O(n^2)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    if(n <= 2){
        std::cout << "1\n";
        return;
    }else if(n == 3){
        std::cout << "2\n";
        return;
    }

    std::vector<int>f(n + 1);
    f[1] = f[2] = 1;
    f[3] = 2;

    auto dfs = [&](auto &&self, int x) -> int {
        if(f[x]){
            return f[x];
        }

        for(int i = 1; i < x; i++){
            f[x] += self(self, i) * self(self, x - i);
        }
        return f[x];
    };

    int ans = dfs(dfs, n);
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 17965 幸运之星

（1）只需要维护一个起点和当前剩下的人之间相邻的距离，发现这个距离必然相同，每过一轮这个距离都会$\times 2$，最多递归$log$轮

（2）看提示，提示写的很详细了。注意应该用递推（$dp$）写而不是递归。

* 时间复杂度$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, m;
    std::cin >> n >> m;

    auto dfs = [&](auto &&self, int start, int len) -> int {
        if(start + len > n){
            return start;
        }

        return self(self, start + len, len * 2);
    };
    int ans1 = dfs(dfs, 1, 1);


    std::vector<int>dp(n + 1);
    dp[1] = 1;
    for(int i = 2; i <= n; i++){
        dp[i] = (dp[i - 1] + m - 1) % i + 1;
    }
    std::cout << ans1 << " " << dp[n] << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11086 排序问题再探讨

并非填空题，暴力$sort$即可

* 时间复杂度$O(nlogn)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    std::string s[] = {"Insert sort: ", "Natural merge sort: ", "Quick sort: "};
    int n;
    std::cin >> n;
    std::vector<int>a(n);
    for(int i = 0; i < 3; i++){
        for(int j = 0; j < n; j++){
            std::cin >> a[j];
        }
        std::sort(a.begin(), a.end());
        std::cout << s[i];
        for(int j = 0; j < n; j++){
            std::cout << a[j] << " \n"[j == n - 1];
        }
    }
}    

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11087 统计逆序对

树状数组的板子题

思路：

首先记录初始每个数的位置 然后对数组按值从大到小进行排序 

然后 将排序后的数组中的数 按照原来的位置插入新数组 

考虑如果当前插入的位置位$inx$ 那么如果$[inx+1,n]$这个位置内已经有数插入 说明形成了逆序对

然后求总和即可

由于设计到动态更新前缀和以及多次查询，朴素前缀和会直接超时，所以得用树状数组

树状数组的单点修和区间查复杂度都是$log$级别的

注意答案可能爆$int$

* 时间复杂度$O(nlogn)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

template<typename T>
struct Fenwick{
    std::vector<T>a;
    int n;

    Fenwick(int n_ = 0){
        n = n_ + 1;
        init();
    }

    void init(){
        a.assign(n, T{});
    }

    void add(int pos, T k){
        for(int i = pos; i < n; i += i & -i){
            a[i] += k;
        }
    }

    T sum(int pos){
        T ans{};
        for(int i = pos; i; i -= i & -i){
            ans += a[i];
        }
        return ans;
    }

    T rangesum(int l, int r){
        if(!l){
            return sum(r);
        }
        else{
            return sum(r) - sum(l - 1);
        }
    }
};

struct dat{
    int val, id;
    dat(): val(-1), id(-1){};
};

void solve(){
    int n;
    std::cin >> n;
    std::vector<dat>a(n);
    for(int i = 0; i < n; i++){
        std::cin >> a[i].val;
        a[i].id = i + 1;
    }
    std::sort(a.begin(), a.end(), [&](const dat &x, const dat &y){
        if(x.val == y.val){
            return x.id < y.id;
        }
        return x.val < y.val;
    });

    Fenwick<int>f(n);
    i64 ans = 0;
    for(int i = 0; i < n; i++){
        int inx = a[i].id;
        ans += f.rangesum(inx, n);
        f.add(inx, 1);
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11088 整数划分的扩展问题

（1）用所有$\le m$的数做完全背包

（2）和问题（1）等价，具体证明可见提示，很妙

（3）用所有奇数做完全背包

（4）用所有$n$以内的数做$01$背包

* 时间复杂度：
* （1），（2）：$O(n \times m)$
* （3）：$O(n^2)$
* （4）：$O(n^2)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void get1(int n, int m){
    std::vector<int>a(m);
    for(int i = 0; i < m; i++){
        a[i] = i + 1;
    }
    std::vector<int>dp(n + 1);
    dp[0] = 1;
    for(int i = 0; i < m; i++){
        for(int j = a[i]; j <= n; j++){
            dp[j] += dp[j - a[i]];
        }
    }
    std::cout << dp[n] << " ";
}


void get3(int n){
    std::vector<int>a;
    for(int i = 1; i <= n; i += 2){
        a.push_back(i);
    }
    
    std::vector<int>dp(n + 1);
    dp[0] = 1;
    int siz = a.size();
    for(int i = 0; i < siz; i++){
        for(int j = a[i]; j <= n; j++){
            dp[j] += dp[j - a[i]];
        }
    }
    std::cout << dp[n] << " ";
}

void get4(int n){
    std::vector<int>a(n);
    for(int i = 0; i < n; i++){
        a[i] = i + 1;
    }
    std::vector<int>dp(n + 1);
    dp[0] = 1;
    for(int i = 0; i < n; i++){
        for(int j = n; j >= a[i]; j--){
            dp[j] += dp[j - a[i]];
        }
    }
    std::cout << dp[n] << "\n";
}

void solve(){
    int n, m;
    std::cin >> n >> m;
    get1(n, m);
    get1(n, m);
    get3(n);
    get4(n);
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 17082 两个有序数列中找第$k$小

对两个序列一起跑双指针即可

* 时间复杂度$O(n+m)$，这主要来自输入两个序列的复杂度，找第$k$小复杂度只有$O(k)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, m, k;
    std::cin >> n >> m >> k;
    std::vector<int>a(n), b(m);
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
    }
    for(int i = 0; i < m; i++){
        std::cin >> b[i];
    }
    int inxa = 0, inxb = 0, cnt = 0;
    while(cnt < k - 1){
        if(inxa >= a.size()){
            cnt++;
            inxb++;
        }else if(inxb >= b.size()){
            cnt++;
            inxa++;
        }else{
            if(a[inxa] <= b[inxb]){
                inxa++;
            }else{
                inxb++;
            }
            cnt++;
        }
    }

    if(inxa >= a.size()){
        std::cout << b[inxb] << "\n";
    }else if(inxb >= b.size()){
        std::cout << a[inxa] << "\n";
    }else{
        std::cout << std::min(a[inxa], b[inxb]) << "\n";
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```











## 17087 输出所有组合

排序后跑深搜即可

* 时间复杂度$O(n^2 \times 2^n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::string s;
    std::cin >> n >> s;     
    std::sort(s.begin(), s.end());

    std::vector<std::string>res;
    std::string p;
    auto dfs = [&](auto &&self, int inx) -> void {
        if(!p.empty()){
            res.push_back(p);
        }

        for(int i = inx; i < n; i++){
            p += s[i];
            self(self, i + 1);
            p.pop_back();
        }
    };

    dfs(dfs, 0);
    std::sort(res.begin(), res.end(), [](const std::string &a, const std::string &b){
        if(a.size() == b.size()){
            return a < b;
        }
        return a.size() < b.size();
    });
    
    int siz = res.size();
    for(int i = 0; i < siz; i++){
        std::cout << i + 1 << " " << res[i] << "\n";
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 9716 矩阵的并

困难，不会，放弃。



# 3.动态规划

## 8596 最长上升子序列

$N$最大只有$1e4$，显然可以考虑$O(n^2)$的做法

用$dp[i]$表示以第$i$个元素结尾的最大子段和

递推：考虑每一个小于$i$的位置$j$，如果$a[j]<a[i]$，说明发现了一个上升段，转移方程为：$dp[i]=max(dp[i],dp[j]+1)$

* 时间复杂度：$O(n^2)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    while(std::cin >> n){
        if(!n){
            break;
        }
        std::vector<int>a(n);
        for(int i = 0; i < n; i++){
            std::cin >> a[i];
        }
        std::vector<int>dp(n, 1);
        for(int i = 1; i < n; i++){
            for(int j = 0; j < i; j++){
                if(a[i] > a[j]){
                    dp[i] = std::max(dp[i], dp[j] + 1);
                }
            }
        }
        int ans = *std::max_element(dp.begin(), dp.end());
        std::cout << ans << '\n';
    }
}          

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 18708 最大子段和

考虑用$dp[i]$表示以第$i$个元素结尾的最大子段和

由于子段是连续的子数组，因此第$i$个位置的状态只能由第$i-1$个位置推导而来

如果字段长度为1，那么就是$a[i]$，否则是$dp[i-1]+a[i]$，于是有状态转移方程：$dp[i]=max(dp[i-1]+a[i],a[i])$

若求最小子段和也是同理

* 时间复杂度$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>a(n);
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
    }
    std::vector<int>dp(n);
    dp[0] = a[0];
    for(int i = 1; i < n; i++){
        dp[i] = std::max(dp[i - 1] + a[i], a[i]);
    }
    int ans = *std::max_element(dp.begin(), dp.end());
    std::cout << ans << "\n";
}          

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 19182 石子合并（基础版）

区间$dp$模板题

用$dp[i][j]$表示合并$[i,j]$这个区间石子的最小代价

一个区间的最小代价可以由更小的区间推出

形式化地，$dp[i][j] = min(dp[i][j], dp[i][k] \ + \ dp[k + 1][j] \ + \ \sum_{i}^{k}a[i]), \quad i \le k \lt j$

然后枚举长度，枚举起点，递推即可

* 时间复杂度$O(n^3)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>a(n + 1), pre(n + 1);
    for(int i = 1; i <= n; i++){
        std::cin >> a[i];
        pre[i] = pre[i - 1] + a[i];
    }
    std::vector<std::vector<int>>dp(n + 1, std::vector<int>(n + 1, inf));
    for(int i = 1; i <= n; i++){
        dp[i][i] = 0;
    }

    for(int len = 2; len <= n; len++){
        for(int i = 1; i + len - 1 <=n; i++){
            int j = i + len - 1;
            for(int k = i; k < j; k++){
                dp[i][j] = std::min(dp[i][j], dp[i][k] + dp[k + 1][j] + pre[j] - pre[i - 1]);
            }
        }
    }
    std::cout << dp[1][n] << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 8597 石子划分问题

* 题目没说，实际上是可以排序的

用$dp[i][j]$表示在第$i$个石子处，划分为$j$份，且第$i$个石子属于第$j$份的最小代价

设上一份石子划分的结束位置为$k$，则$dp[i][j]=min(dp[i][j],dp[k][j-1]+cost), \ cost=(a[i]-a[k]) \times (a[i]-a[k])$

从前往后递推即可

* 注意：第$i$个石子处最多划分出$min(i,m)$份

* 时间复杂度$O(n^3)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, m;
    std::cin >> n >> m;
    std::vector<int>a(n + 1);
    for(int i = 1; i <= n; i++){
        std::cin >> a[i];
    }
    std::sort(a.begin(), a.end());
    std::vector<std::vector<int>>dp(n + 1, std::vector<int>(m + 1, inf));
    dp[0][0] = 0;
    for(int i = 1; i <= n; i++){
        for(int j = 1; j <= std::min(m, i); j++){
            for(int k = 0; k < i; k++){
                int d = a[i] - a[k + 1];
                dp[i][j] = std::min(dp[i][j], dp[k][j - 1] + d * d);
            }
        }
    }
    std::cout << dp[n][m] << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 19185 01背包

$dp$入门题

用$dp[j]$表示容量为$j$的背包可以装的最大价值

每次考虑是否加入新物品，递推公式：$dp[j]=max(dp[j],dp[j - w[i]]+v[i])$

其中$w[i],v[i]$分别为第$i$个物品的重量和价值

时间复杂度$O(n \times m)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n,m;
    std::cin >> m >> n;
    std::vector<int>w(n), v(n);
    for(int i = 0; i < n; i++){
        std::cin >> w[i] >> v[i];
    }
    std::vector<int>dp(m + 1,0);
    for(int i = 0; i < n; i++){
        for(int j = m; j >= w[i]; j--){
            dp[j] = std::max(dp[j], dp[j - w[i]] + v[i]);
        }
    }
    std::cout << dp[m] <<"\n";
}          

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 9717 取数对弈

好题

$dp[i][j]$表示从$[i,j]$区间内甲先手能取到最高分

甲第一步要么取$a[i]$，要么取$a[j]$

①如果甲取$a[i]$，变成乙先手取$[i+1, j]$内的数，由于总和固定且乙会按最优策略取，答案是$\sum_{k=i}^{j}a[k]-dp[i+1][j]$

②同理，如果甲取$a[j]$，答案是$\sum_{k=i}^{j}a[k]-dp[i][j-1]$

得到状态转移方程：$dp[i][j]=\sum_{k=i}^{j}a[k]-min(dp[i+1][j],dp[i][j-1])$

看着像是个区间$dp$，枚举长度后递推即可

* 时间复杂度$O(n^2)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>a(n + 1), pre(n + 1);
    for(int i = 1; i <= n; i++){
        std::cin >> a[i];
        pre[i] = pre[i - 1] + a[i];
    }
    std::vector<std::vector<int>>dp(n + 1, std::vector<int>(n + 1));
    for(int i = 1; i <= n; i++){
        dp[i][i] = a[i];
    }

    for(int len = 2; len <= n; len++){
        for(int i = 1; i + len - 1 <= n; i++){
            int j = i + len - 1;
            dp[i][j] = std::max(dp[i][j], pre[j] - pre[i - 1] - std::min(dp[i + 1][j], dp[i][j - 1]));
        }
    }

    std::cout << dp[1][n] << " " << pre[n] - dp[1][n] << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 10303 数字三角

 用$dp[i][j]$表示到达第$i$行第$j$列的最大路径和

对于每个位置，都有：$dp[i][j] = dp[i-1][j] + a[i][j]$

如果$j \gt 0$，$dp[i][j] = min(dp[i][j],dp[i-1][j-1]+a[i][j])$

至于路径只需要倒着找一遍即可

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<std::vector<int>>a(n);
    for(int i = 0; i < n; i++){
        a[i].resize(i + 1);
        for(int j = 0; j <= i; j++){
            std::cin >> a[i][j];
        }
    }
    std::vector<std::vector<int>>dp(n, std::vector<int>(n, 0));
    dp[0][0] = a[0][0];
    for(int i = 1; i < n; i++){
        for(int j = 0; j <= i; j++){
            dp[i][j] = std::max(dp[i][j], dp[i - 1][j] + a[i][j]);
            if(j){
                dp[i][j] = std::max(dp[i][j], dp[i - 1][j - 1] + a[i][j]);
            }
        }
    }
    std::vector<int>path;
    int max = 0, r = n - 1, c;
    for(int i = 0; i < n; i++){
        if(dp[n - 1][i] >= max){
            max = dp[n - 1][i];
            c = i;
        }
    }
    while(r > 0){
        path.push_back(a[r][c]);
        if(dp[r - 1][c] != dp[r][c] - a[r][c]){
            c--;
        }
        r--;
    }
    path.push_back(a[0][0]);
    std::cout << max << "\n";
    for(int i = path.size() - 1; i >= 0; i--){
        std::cout << path[i] << " \n"[i == 0];
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



##  10349 数字滑雪

设$dp[i][j]$为以$a[i][j]$为起点的最长路径，然后往四个方向进行深搜

发现，有可能会访问到以前已经搜过的节点，此时不用继续搜索，直接相加后返回即可，这就是记忆化搜索

* 每个位置访问一遍，时间复杂度$O(n \times m)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

constexpr int dx[] = {1, -1, 0, 0};
constexpr int dy[] = {0, 0, -1, 1};
void solve(){
    int n, m;
    std::cin >> n >> m;
    std::vector<std::vector<int>>a(n, std::vector<int>(m));
    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            std::cin >> a[i][j];
        }
    }
    std::vector<std::vector<int>>dp(n, std::vector<int>(m));

    auto dfs = [&] (auto &&self, int x, int y) -> int {
        if(dp[x][y]){
            return dp[x][y];
        }

        dp[x][y] = 1;
        for(int i = 0; i < 4; i++){
            int r = x + dx[i];
            int c = y + dy[i];
            if(r >= 0 && r < n && c >= 0 && c < m && a[r][c] < a[x][y]){
                dp[x][y] = std::max(dp[x][y], 1 + self(self, r, c));
            } 
        }
        return dp[x][y];
    };

    int ans = 0;
    for(int i = 0; i < n; i++){
        for(int j = 0; j < m; j++){
            ans = std::max(ans, dfs(dfs, i, j));
        }
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11077 最长公共子字符串

好像不是很好理解！

设两个字符串分别为$a$，$b$

若$dp[i][j]$不为$0$，则表示$a$的前$i$位和$b$的前$j$位在末尾对齐，且最后一位相同情况下的最长公共子串长度

若为$0$表示末尾对齐时最后一位不匹配

这么解释的话，显然是只有$a[i-1]=b[j-1]$时才进行状态转移（索引从$0$开始）

则$dp[i][j]=dp[i-1][j-1]+1$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    std::string a, b;
    std::cin >> a >> b;
    int n = a.size(), m = b.size();
    std::vector<std::vector<int>>dp(n + 1, std::vector<int>(m + 1));

    int max = 0, inx = 0;
    for(int i = 1; i <= n; i++){
        for(int j = 1; j <= m; j++){
            if(a[i - 1] != b[j - 1]){
                continue;
            }
            dp[i][j] = dp[i - 1][j - 1] + 1;
            if(dp[i][j] > max){
                max = dp[i][j];
                inx = i - dp[i][j];
            }
        }
    }
    std::string ans = a.substr(inx, max);
    std::cout << max << "\n" << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11078 不能移动的石子合并

环形的石子合并可以转化为一排的石子合并，我们往数组后面按序重新填充这$n$个元素后做$dp$

$dp$逻辑和一排的情况相同

相当于是枚举所有$n$个位置作为起点进行排形$dp$后取最优解

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>a(n + 1);
    std::vector<i64>pre(2 * n + 1, 0);
    for(int i = 1; i <= n; i++){
        std::cin >> a[i];
        pre[i] = pre[i - 1] + a[i];
    }
    for(int i = n + 1; i <= 2 * n; i++){
        a.push_back(a[i - n]);
        pre[i] = pre[i - 1] + a[i];
    }
    auto calc = [&](int len){
        std::vector<std::vector<i64>>dp1(len + 1, std::vector<i64>(len + 1, INF));
        std::vector<std::vector<i64>>dp2(len + 1, std::vector<i64>(len + 1, 0));
        for(int i = 1; i <= len; i++){
            dp1[i][i] = 0;
        }
        for(int i = len; i; i--){
            for(int j = i + 1; j <= len; j++){
                for(int k = i; k < j; k++){
                    dp1[i][j] = std::min(dp1[i][j], dp1[i][k] + dp1[k + 1][j] + pre[j] - pre[i - 1]);
                    dp2[i][j] = std::max(dp2[i][j], dp2[i][k] + dp2[k + 1][j] + pre[j] - pre[i - 1]);
                }
            }
        }
        i64 ans1 = INF, ans2 = 0;
        for(int i = 1; i + n - 1 <= len; i++){
            ans1 = std::min(ans1, dp1[i][i + n - 1]);
            ans2 = std::max(ans2, dp2[i][i + n - 1]);
        }
        std::cout << ans1 << " " << ans2 << "\n";
    };
    calc(n), calc(2 * n);
}          

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11080 游泳圈的最大子矩阵和

数据范围极小，可以直接枚举矩形的长度和高度

为了简化求和，先用二位前缀和预处理

由于是环形的，我们可以在每一行后按序填充这一行的$m$个元素，然后在$n$行以后重新按需填充$n$行，形成一个$2n \times 2m$的数组，然后枚举即可

* 复杂度$O(n^2 \times m^2)$
* 可以让行和列下标从$1$开始，方便处理前缀和

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, m;
    std::cin >> n >> m;
    std::vector<std::vector<int>>a(n + 1, std::vector<int>(m + 1));
    for(int i = 1; i <= n; i++){
        for(int j = 1; j <= m; j++){
            std::cin >> a[i][j];
            a[i].push_back(a[i][j]);
        }
        a.push_back(a[i]);
    }
    std::vector<std::vector<int>>pre(2 * n + 1, std::vector<int>(2 * m + 1));
    for(int i = 1; i <= 2 * n; i++){
        for(int j = 1; j <= 2 * m; j++){
            pre[i][j] = pre[i - 1][j] + pre[i][j - 1] - pre[i - 1][j - 1] + a[i][j];
        }
    }
    int ans = -inf;
    for(int h = 1; h <= n; h++){
        for(int w = 1; w <= m; w++){
            for(int i = 1; i + h - 1 <= 2 * n; i++){
                for(int j = 1; j + w - 1 <= 2 * m; j++){
                    int tmp = pre[i + h - 1][j + w - 1] - pre[i - 1][j + w - 1] - pre[i + h - 1][j - 1] + pre[i - 1][j - 1];
                    ans = std::max(ans, tmp);
                }
            }
        }
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11083 旅游背包

用$dp[x][y]$表示体积为$x$，重量为$y$时能达到的最大价值

由于每个物品有多个，因此对于一个物品来说，可以进行$c[i]$次状态转移

状态转移方程：$dp[x][y]=max(dp[x][y],dp[x-v[i]][y-w[i]]+t[i])$

* 时间复杂度$O(V \times W \times \sum_{i=1}^{n}c[i])$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, V, W;
    std::cin >> n >> V >> W;
    std::vector<int>v(n), w(n), c(n), t(n);
    for(int i = 0; i < n; i++){
        std::cin >> v[i] >> w[i] >> c[i] >> t[i];
    }

    std::vector<std::vector<int>>dp(V + 1, std::vector<int>(W + 1));
    for(int i = 0; i < n; i++){
        for(int k = 0; k < c[i]; k++){
            for(int vv = V; vv >= v[i]; vv--){
                for(int ww = W; ww >= w[i]; ww--){
                    dp[vv][ww] = std::max(dp[vv][ww], dp[vv - v[i]][ww - w[i]] + t[i]);
                }
            }
        }
    }
    std::cout << dp[V][W] << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11090 最大$m$段乘积和最小$m$段和

法一：动态规划

首先可以预处理出$[i,j]$段表示的数，设为$num[i][j]$，由于数据范围很小，直接$O(n^2)$暴力即可

①最大$m$段乘积：用$dp[i][j]$表示前$i$个位置划分出$j$段，且第$i$个位置属于第$j$段的最大乘积

状态转移就是往前找第$j-1$段的结束位置，$dp[i][j]=max(dp[i][j],dp[k][j-1] \times num[k+1][j]),\ k \lt j$

②最小$m$​段和：用$dp[i][j]$表示前$i$个位置划分出$j$段，且第$i$个位置属于第$j$段的最小和

状态转移同样是往前找第$j-1$段的结束位置，$dp[i][j]=min(dp[i][j],dp[k][j-1] + num[k+1][j]),\ k \lt j$

至于找表达式，倒着找一遍即可

* 时间复杂度$O(n^2 \times m)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, m;
    std::string s;
    std::cin >> n >> m >> s;
    s = " " + s;
    std::vector<std::vector<i64>>num(n + 1, std::vector<i64>(n + 1));
    for(int i = 1; i <= n; i++){
        i64 cur = 0;
        for(int j = i; j <= n; j++){
            cur = cur * 10 + (s[j] - '0');
            num[i][j] = cur;
        }
    }
    std::vector<std::vector<i64>>dp1(n + 1, std::vector<i64>(m + 1, 0));
    std::vector<std::vector<i64>>dp2(n + 1, std::vector<i64>(m + 1, INF));
    dp1[0][0] = 1, dp2[0][0] = 0;
    for(int i = 1; i <= n; i++){
        for(int j = 1; j <= std::min(m, i); j++){
            for(int k = 0; k < i; k++){
                dp1[i][j] = std::max(dp1[i][j], dp1[k][j - 1] * num[k + 1][i]);
                dp2[i][j] = std::min(dp2[i][j], dp2[k][j - 1] + num[k + 1][i]);
            }
        }
    }
    std::cout << dp1[n][m] << " " << dp2[n][m] << "\n";
    std::vector<i64>path1, path2;
    int inx = n, x = m;
    while(x){
        int nxt;
        for(int i = x - 1; i < inx; i++){
            if(dp1[i][x - 1] * num[i + 1][inx] == dp1[inx][x]){
                nxt = i;
                break;
            }
        }
        path1.push_back(num[nxt + 1][inx]);
        inx = nxt, x--;
    }
    std::reverse(path1.begin(), path1.end());

    inx = n, x = m;
    while(x){
        int nxt;
        for(int i = x - 1; i < inx; i++){
            if(dp2[i][x - 1] + num[i + 1][inx] == dp2[inx][x]){
                nxt = i;
                break;
            }
        }
        path2.push_back(num[nxt + 1][inx]);
        inx = nxt, x--;
    }
    std::reverse(path2.begin(), path2.end());
    for(int i = 0; i < m; i++){
        std::cout << path1[i] << "*="[i == m - 1];
    }
    std::cout << dp1[n][m] << "\n";

    for(int i = 0; i < m; i++){
        std::cout << path2[i] << "+="[i == m - 1];
    }
    std::cout << dp2[n][m] << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



法二：搜索

数据范围只有$10$，当然可以搜索

首先确定参数：我们需要知道前面哪些位置已经被用掉了，所以需要索引；要知道当前的和/积；还要知道前面已经划分出了多少段

确定参数后就是返回条件的确定：最多划分$m$段，在最后一段划分的时候就应该更新答案并且返回了

进一步地，如果第$m-1$段已经划分完了，剩下部分必然属于最后一段，于是可以直接在划分出$m-1$段就更新答案并且返回

下面的代码中$inx$是当前段开始的索引，$cur$是当前已有的和/积，$hav$是在$inx$这个位置前划分出了几段

* 时间复杂度：首先预处理部分是$O(n^2)$的，搜索部分是划分出$m$个非空段，用隔板法计算，一共有$\binom{n-1}{m-1} \cdot m$种，每次拷贝的开销都是$O(m)$，共$O\left(\binom{n-1}{m-1} \cdot m\right)$，总复杂度$O(n^2+(\binom{n-1}{m-1} \cdot m)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, m;
    std::string s;
    std::cin >> n >> m >> s;
    std::vector<std::vector<i64>>num(n, std::vector<i64>(n));
    for(int i = 0; i < n; i++){
        i64 cur = 0;
        for(int j = i; j < n; j++){
            cur = cur * 10 + (s[j] - '0');
            num[i][j] = cur;
        }
    }

    std::vector<i64>path1, path2, p;
    i64 max = -1, min = INF;
    auto dfs1 = [&](auto &&self, int inx, i64 cur, int hav) -> void {
        if(hav == m - 1){
            if(cur * num[inx][n - 1] > max){
                max = cur * num[inx][n - 1];
                path1 = p;
                path1.push_back(num[inx][n - 1]);
            }
            return;
        }

        for(int i = inx; n - i >= m - hav; i++){
            p.push_back(num[inx][i]);
            self(self, i + 1, cur * num[inx][i], hav + 1);
            p.pop_back();
        }
    };
    dfs1(dfs1, 0, 1, 0);

    auto dfs2 = [&](auto &&self, int inx, i64 cur, int hav) -> void {
        if(hav == m - 1){
            if(cur + num[inx][n - 1] < min){
                min = cur + num[inx][n - 1];
                path2 = p;
                path2.push_back(num[inx][n - 1]);
            }
            return;
        }

        for(int i = inx; n - i >= m - hav; i++){
            p.push_back(num[inx][i]);
            self(self, i + 1, cur + num[inx][i], hav + 1);
            p.pop_back();
        }
    };
    dfs2(dfs2, 0, 0, 0);

    std::cout << max << " " << min << "\n";
    for(int i = 0; i < m; i++){
        std::cout << path1[i] << "*="[i == m - 1];
    }
    std::cout << max << "\n";
    for(int i = 0; i < m; i++){
        std::cout << path2[i] << "+="[i == m - 1];
    }
    std::cout << min << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 8595 钱币组合问题

多重背包问题，相较$01$背包多了数量上的限制

* 注意循环的顺序问题，寻找构成方法数其实相当于是找组合数而不是排列数
* 先拿一张$1$分再拿两张$1$分和一次拿三张$1$分是同种情况！
* 时间复杂度$O(\sum_{i=1}^{n}k_i \times m)$，最劣$50 \times 100 \times 20000 = 1e7$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>v(n), k(n);
    for(int i = 0; i < n; i++){
        std::cin >> v[i];
    }
    for(int i = 0; i < n; i++){
        std::cin >> k[i];
    }   
    int m;
    std::cin >> m;
    std::vector<int>dp1(m + 1), dp2(m + 1, inf);
    dp1[0] = 1, dp2[0] = 0;
    for(int i = 0; i < n; i++){
        for(int j = m; j > 0; j--){
            for(int t = 1; t <= k[i]; t++){
                if(j < t * v[i]){
                    break;
                }
                dp1[j] += dp1[j - t * v[i]];
            }
        }
    }

    for(int i = 0; i < n; i++){
        for(int t = 1; t <= k[i]; t++){
            for(int j = m; j >= v[i]; j--){
                dp2[j] = std::min(dp2[j], dp2[j - v[i]] + 1);
            }
        }
    }
    if(!dp1[m]){
        std::cout << 0 << "\n" << "no possible\n";
    }else{
        std::cout << dp1[m] << "\n" << dp2[m] << "\n";
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 17089最大$m$子段和

好像很困难!

令$dp[i][j]$表示到第$i$位置，已经划分出$j$段，且$a[i]$属于第$j$段时的最大子段和

①如果第$i-1$位置时已经划分出了$j$段，那么直接加入$a[i]$，即为$dp[i][j]=dp[i-1][j]+a[i]$

②如果是在$i$位置才开始第$j$段呢，此时维护一个长度为$m$的$pre$数组，$pre[k]$用于表示$[0,i-1]$划分出了$k$段时的最大子段和，那么有$dp[i][j]=pre[j-1]+a[i]$

于是得到状态转移方程：$dp[i][j]=min(dp[i-1][j]+a[i],pre[j-1]+a[i])$

当原数组中非正数$\ge m$时，可以让答案和$0$取$max$

* 时间复杂度$O(n \times m)$
* 该题数据极弱，实际数据$n$不超过$90$，$m$不超过$30$，写成$O(n^2 \times m)$都能过

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, m;
    std::cin >> n >> m;
    std::vector<int>a(n);
    int cnt = 0;
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
        cnt += (a[i] <= 0);
    }
    std::vector<std::vector<int>>dp(n, std::vector<int>(m + 1, -inf));
    std::vector<int>pre(m + 1, -inf);
    dp[0][1] = pre[1] = a[0];
    for(int i = 1; i < n; i++){
        for(int j = 1; j <= std::min(i + 1, m); j++){
            dp[i][j] = std::max(dp[i - 1][j] + a[i], pre[j - 1] + a[i]);
        }
        
        for(int j = 1; j <= std::min(i + 1, m); j++){
            pre[j] = std::max(pre[j], dp[i][j]);
        }
    }
    int ans = -inf;
    for(int i = m - 1; i < n; i++){
        ans = std::max(ans, dp[i][m]);
    }
    if(cnt >= m){
        ans = std::max(ans, 0);
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 17098 广告牌最佳安放问题

首先按照大小对广告牌位置排序，然后维护一个和当前广告牌距离$\gt5$的最大值，每更新一个位置双指针一下即可

实际上给的数据都是有序的，不用排序，那直接双指针跑一遍即可

* 时间复杂度$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int m, n;
    std::cin >> m >> n;
    std::vector<int>x(n), r(n);
    for(int i = 0; i < n; i++){
        std::cin >> x[i];
    }
    for(int i = 0; i < n; i++){
        std::cin >> r[i];
    }
    std::vector<int>dp(n);
    int max = 0;
    for(int i = 0, j = 0; i < n; i++){
        while(x[i] - x[j] > 5){
            max = std::max(max, dp[j]);
            j++;
        }
        dp[i] = max + r[i];
    }
    int ans = *std::max_element(dp.begin(), dp.end());
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 17099 周工作计划安排

* 他这里应该是默认第$1$周不能接高压工作

用$dp[i][0]$表示第$i$周接低压工作的最大收益，$dp[i][1]$表示第$i$周接高压工作的最大收益

容易得到状态转移方程：

$dp[i][0]=max(dp[i-1][0],dp[i-1][1])+l[i]$

$dp[i][1]=max(dp[i-2][0],dp[i-2][1])+h[i]$

* 时间复杂度$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>l(n + 1), h(n + 1);
    for(int i = 1; i <= n; i++){
        std::cin >> l[i];
    }
    for(int i = 1; i <= n; i++){
        std::cin >> h[i];
    }

    std::vector<std::vector<int>>dp(n + 1, std::vector<int>(2));
    dp[1][0] = l[1];
    for(int i = 2; i <= n; i++){
        dp[i][0] = std::max(dp[i - 1][0], dp[i - 1][1]) + l[i];
        dp[i][1] = std::max(dp[i - 2][0], dp[i - 2][1]) + h[i];
    }
    std::cout << std::max(dp[n][0], dp[n][1]) << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```







## 17102 "一条路径图"的最大独立集问题

令$dp[i]$表示$[1,i]$范围内可达到的最大总权

正着递推的话，第$i$个位置实际只受到前一个位置的限制

考虑将第$i$个位置的权值加入总权，那么就是$max(dp[i-2]+w[i],w[i])$

但是由于$dp[i]$表示$[1,i]$范围内的最大总权，我们还需考虑不加入第$i$个位置的权重时的最大总权

为$dp[i-1]$

得到状态转移方程：$dp[i]=max({dp[i-2]+w[i],w[i],dp[i-1]})$

* 时间复杂度$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>w(n + 1);
    for(int i = 1; i <= n; i++){
        std::cin >> w[i];
    }
    std::vector<i64>dp(n + 1, -INF);
    dp[0] = 0, dp[1] = std::max(w[1], 0);
    for(int i = 2; i <= n; i++){
        dp[i] = std::max(1LL * w[i], dp[i - 2] + w[i]);
        dp[i] = std::max(dp[i], dp[i - 1]);
    }
    i64 ans = *std::max_element(dp.begin(), dp.end());
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 19187 广告牌最佳安放问题（二）

$n,m$都能达到$1000$，朴素的$n^2 \times m$的做法会超时，考虑优化掉一个$n$

令$dp[i][j]$表示在第$i$个位置安放第$j$个广告牌的的最大收益，现在我们需要关注的是第$j-1$块广告牌安放的位置

不妨建一个长度为$m$的数组$pre$，$pre[k]$用于表示距离当前位置大于五公里时，已经安放了$k$块广告牌的最大收益

得到状态转移方程：$dp[i][j]=pre[j - 1] + r[i]$

$pre$数组可通过双指针法进行更新

* 时间复杂度$O(n \times m)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int T, n, m;
    std::cin >> T >> n >> m;
    std::vector<int>x(n), r(n);
    for(int i = 0; i < n; i++){
        std::cin >> x[i];
    }
    for(int i = 0; i < n; i++){
        std::cin >> r[i];
    }
    std::vector<int>pre(m + 1);
    std::vector<std::vector<int>>dp(n, std::vector<int>(m + 1));
    for(int i = 0, j = 0; i < n; i++){
        while(x[i] - x[j] > 5){
            for(int k = 0; k <= m; k++){
                pre[k] = std::max(pre[k], dp[j][k]);
            }
            j++;
        }

        for(int k = 1; k <= std::min(i + 1, m); k++){
            dp[i][k] = pre[k - 1] + r[i];
        }
    }

    int ans = 0;
    for(int i = m - 1; i < n; i++){
        ans = std::max(ans, dp[i][m]);
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



# 4.贪心算法

## 11091 最优自然数分解问题

（1）容易想到肯定是先拆成连续的自然数，然后如果有剩余就按照从大到小的顺序给已经选中的数$+1$

（2）看提示做的，很困难啊

* 时间复杂度$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void get1(int n){
    std::vector<int>a;
    for(int i = 2; i <= n; i++){
        a.push_back(i);
        n -= i;
    }
    if(n){
        for(int i = a.size() - 1; i >= 0 && n; i--){
            a[i]++;
            n--;
        }
    }
    i64 res = 1;
    for(auto c : a){
        res *= c;
    }
    std::cout << res << " ";
}

void get2(int n){
    i64 res2 = 1;
    while(n >= 3){
        res2 *= 3;
        n -= 3;
    }
    if(n == 1){
        res2 = res2 / 3 * 4;
    }
    while(n >= 2){
        res2 *= 2;
        n -= 2;
    }
    std::cout << res2 << "\n";
}

void solve(){
    int n;
    std::cin >> n;
    if(n <= 2){
        std::cout << n << " " << n << "\n";
        return;
    }else if(n == 3){
        std::cout << 2 << " " << 3 << "\n";
        return;
    }

    get1(n), get2(n);
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 8598 整除15问题

不想重写了，就是讨论讨论判判判

端了个去年十月写的版本上来

* 时间复杂度：$O(n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    std::string s;
    std::cin >> s;
    std::vector<int>cnt(10, 0);
    i64 sum = 0;
    for(auto c : s){
        cnt[c - '0']++;
        sum += (c - '0');    
    }

    if(sum % 3 == 2){
        for(int i = 2; sum % 3 && i <= 9; i += 3){
            if(i == 5 && cnt[i] == 1 && !cnt[0]){
                continue;
            }
            if(cnt[i]){
                cnt[i]--;
                sum -= i;
            }
        }
        for(int i = 1; sum % 3 && i <= 9; i += 3){
            while(cnt[i] && sum % 3){
                cnt[i]--;
                sum -= i;
            }
        }
    }else if(sum % 3 == 1){
        for(int i = 1; sum % 3 && i <= 9; i += 3){
            if(cnt[i]){
                cnt[i]--;
                sum -= i;
            }
        }
        for(int i = 2; sum % 3 && i<= 9; i += 3){
            while(cnt[i] && sum % 3){
                if(i == 5 && cnt[i] == 1 && !cnt[0]){
                    break;
                }
                cnt[i]--;
                sum -= i;
            }
        }
    }
    if(sum % 3 || count(cnt.begin(), cnt.end(), 0)==10 || (!cnt[0] && !cnt[5])){
        std::cout << "impossible\n";
    }else{
        if(cnt[0]){
            for(int i = 9; i >= 0; i--){
                for(int j = 0; j < cnt[i]; j++){
                    std::cout << i;
                }
            }
        }else{
            for(int i = 9; i >= 0; i--){
                for(int j = 0; j < (i == 5 ? cnt[i] - 1:cnt[i]); j++){
                    std::cout << i;
                }
            }
            std::cout << 5;
        }
        std::cout << "\n";
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 8602 区间相交问题

按右边界排序，数有多少个不重的区间

* 时间复杂度$O(nlogn)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<pii>a(n);
    for(int i = 0; i < n; i++){
        std::cin >> a[i].second >> a[i].first;
    }
    std::sort(a.begin(), a.end());
    int end = -inf;
    int cnt = 0;
    for(int i = 0; i < n; i++){
        if(a[i].second >= end){
            cnt++;
            end = a[i].first;
        }
    }
    std::cout << n - cnt << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 8605 删数问题

暴力跑$k$轮，每轮从高位往低位贪心，删第一个减小的位置

* 时间复杂度$O(k \times n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    std::string s;
    int k;
    while(1){
        std::cin >> s;
        if(s == "0"){
            break;
        }
        std::cin >> k;
        for(int i = 0; i < k; i++){
            int mx = s[0] - '0';
            int inx = 0;
            int n = s.size();
            for(int j = 1; j < n; j++){
                if(s[j] - '0' < mx) {
                    break;
                }
                mx = s[j] - '0';
                inx = j;
            }
            s.erase(s.begin() + inx);
        }
        std::cout << s << "\n";
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 10346 带限期和价值的作业安排问题

按价值排序，然后记录一下有没有用过这个时间点

* 时间复杂度$O(nlgn)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<std::vector<int>>a(n, std::vector<int>(2));
    for(int i = 0; i < n; i++){
        std::cin >> a[i][0];
    }
    for(int i = 0; i < n; i++){
        std::cin >> a[i][1];
    }
    std::sort(a.begin(), a.end(), [](const std::vector<int> &x, const std::vector<int> &y){
        return x[1] > y[1];
    });
    std::map<int, int>vis;
    int ans = 0;
    for(int i = 0; i < n; i++){
        if(vis[a[i][0]]){
            continue;
        }
        vis[a[i][0]] = 1;
        ans += a[i][1];
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 11079 可以移动的石子合并

（1）最高得分就是每次选最大的两堆，可以直接算数学关系

（2）最小得分就是每次选最小的$k$堆，用小根堆模拟即可

* 时间复杂度$O(nlon)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, k;
    std::cin >> n >> k;
    std::vector<int>a(n);
    std::priority_queue<int, std::vector<int>, std::greater<>>que;
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
        que.push(a[i]);
    }

    std::sort(a.begin(), a.end(), std::greater<>());

    i64 max = 1LL * (n - 1) * a[0], min = 0;
    for(int i = 1; i < n; i++){
        max += 1LL * (n - i) * a[i];
    }
    while(n % (k - 1) != 1){
        que.push(0);
        n++;
    }

    while(que.size() > 1){
        i64 cur=0;
        int siz = que.size();
        for(int i = 0; i < std::min(k, siz); i++){
            cur += que.top();
            que.pop();
        }
        min += cur;
        que.push(cur);
    }
    std::cout << min << " " << max << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 17964 水桶打水

按时间从小到大排序， 要让等待时间短，优先放打的快的，每次放入都选等待时间最短的那个水龙头，然后用小顶堆模拟

* 时间复杂度$O(nlogn)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, r;
    std::cin >> n >> r;
    std::vector<int>t(n);
    for(int i = 0; i < n; i++){
        std::cin >> t[i];
    }

    std::sort(t.begin(), t.end());
    std::priority_queue<int, std::vector<int>, std::greater<>>que;
    
    for(int i = 0; i < r; i++){
        que.push(0);
    }

    int ans = 0;
    for(int i = 0; i < n; i++){
        int x = que.top();
        que.pop();
        que.push(x + t[i]);
        ans += x + t[i];
    }

    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 17103 基站建设

发现一个基站可以影响$8$公里，那么排序后按长度为$8$分块即可

* 时间复杂度$O(nlogn)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<int>a(n);
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
    }
    std::sort(a.begin(), a.end());
    int ans = 0;

    for(int i = 0, j; i < n; i = j){
        ans++;

        j = i + 1;
        while(j < n && a[j] - a[i] <= 8){
            j++;
        }
    }
    std::cout<<ans<<"\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



# 5.搜索算法

搜索，太困难了！



## 8600 骑士问题

正常广搜即可，没啥坑点

* 时间复杂度$O(64T)$，$T$为测试组数

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

constexpr int dx[] = {1, 2, 2, 1, -1, -2, -2, -1};
constexpr int dy[] = {-2, -1, 1, 2, -2, -1, 1, 2};
void solve(){
    int b;
    int num = 0;
    while(1){
        num++;
        std::cin >> b;
        if(b == -1){
            return;
        }

        std::vector<std::vector<int>>a(8, std::vector<int>(8));
        for(int i = 0; i < b; i++){
            char c;
            int r;
            std::cin >> c >> r;
            a[8 - r][c - 'a'] = 1;
        }

        int sr, sc, er, ec;
        char c1, c2;
        std::cin >> c1 >> sr >> c2 >> er;
        sr = 8 - sr, er = 8 - er;
        sc = c1 - 'a', ec = c2 - 'a';

        std::queue<pii>que;
        std::vector<std::vector<int>>vis(8, std::vector<int>(8));
        vis[sr][sc] = 1;
        que.push({sr, sc});
        int min = -1;
        int cnt = 0;
        while(!que.empty()){
            int siz = que.size();
            for(int k = 0; k < siz; k++){
                int x = que.front().first, y = que.front().second;
                que.pop();
                if(x == er && y == ec){
                    min = cnt;
                    break;
                }

                for(int i = 0; i < 8; i++){
                    int r = x + dx[i], c = y + dy[i];
                    if(r >= 0 && r < 8 && c >= 0 && c < 8 && !a[r][c] && !vis[r][c]){
                        vis[r][c] = 1;
                        que.push({r, c});
                    }
                }
            }
            if(min != -1){
                break;
            }
            cnt++;
        }

        if(min == -1){
            std::cout << "Board " << num << ":not reachable\n";
        }else{
            std::cout << "Board " << num << ":" << min << " moves\n";
        }
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 8603 子集和问题

这题直接爆搜就能过，感觉很没有道理

如果是输出任意一种方案的话，直接做$01$背包然后找一下方案即可

搜索做法看看就行了

* 时间复杂度$O(2^n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, c;
    std::cin >> n >> c;
    std::vector<int>a(n);
    for(int i = 0; i < n; i++){
        std::cin >> a[i];
    }
    std::vector<int>ans, p;

    auto dfs = [&] (auto &&self, int sum, int inx) -> bool {
        if(sum == c){
            ans = p;
            return true;
        }
        if(inx >= n){
            return false;
        }
        p.push_back(a[inx]);
        if(self(self, sum + a[inx], inx + 1)){
            return true;
        }
        p.pop_back();
        if(self(self, sum, inx + 1)){
            return true;
        }
        return false;
    };

    if(dfs(dfs, 0, 0)){
        for(auto c : ans){
            std::cout << c << " ";
        }
        std::cout << "\n";
    }else{
        std::cout << "No Solution\n";
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 8604 运动员最佳配对问题

发现其实并不太能贪心，而数据范围极小，于是考虑搜索

在搜索时，我们实际只需要开一个数组男运动员或女运动员的编号，然后跑全排列即可，另外一组固定为$0,1,2...n$这样，就能不重不漏地跑完了

* 时间复杂度$O(n! \times n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<std::vector<int>>P(n, std::vector<int>(n));
    std::vector<std::vector<int>>Q(n, std::vector<int>(n));
    for(int i = 0; i < n; i++){
        for(int j = 0; j < n; j++){
            std::cin >> P[i][j];
        }
    }
    for(int i = 0; i < n; i++){
        for(int j = 0; j < n; j++){
            std::cin >> Q[i][j];
        }
    }

    std::vector<int>a(n);
    for(int i = 0; i < n; i++){
        a[i] = i;
    }
    int ans = 0;
    do{
        int tmp = 0;
        for(int i = 0; i < n; i++){
            tmp += P[a[i]][i] * Q[i][a[i]];
        }
        ans = std::max(ans, tmp);
    }while(std::next_permutation(a.begin(), a.end()));
    
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```







## 11089 多机最佳调度

（1）贪心：让耗时最多的工作给最早空闲的机器，维护一个小根堆即可，复杂度$O(nlogn)$

（2）搜索：枚举每个工作给每台机器的情况，然后剪枝，维护当前能获得的最小答案，如果比这个答案还大的话不进入递归，但这样最劣还是会达到$O(m^n)$，不知道为啥能过，感觉没啥道理

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, m;
    std::cin >> n >> m;
    std::vector<int>t(n);
    for(int i = 0; i < n; i++){
        std::cin >> t[i];
    }
    std::sort(t.begin(), t.end(), std::greater<>());
    std::priority_queue<int, std::vector<int>, std::greater<>>que;
    int ans1 = 0;
    for(int i = 0; i < m; i++){
        que.push(0);
    }

    for(int i = 0; i < n; i++){
        int x = que.top();
        que.pop();
        que.push(x + t[i]);
        ans1 = std::max(ans1, x + t[i]);
    }


    std::vector<int>cur(m);
    int ans2 = inf;
    auto dfs = [&](auto &&self, int inx) -> void {
        if(inx >= n){
            int tmp = *std::max_element(cur.begin(), cur.end());
            ans2 = std::min(ans2, tmp);
            return;
        }

        for(int i = 0; i < m; i++){
            if(cur[i] + t[inx] >= ans2){
                continue;
            }
            cur[i] += t[inx];
            self(self, inx + 1);
            cur[i] -= t[inx];
        }
    };

    dfs(dfs, 0);
    std::cout << ans1 << "\n" << ans2 << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```







## 17085 工作分配问题

对人进行全排列然后统计答案

* 时间复杂度$O(n! \times n)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;
    std::vector<std::vector<int>>C(n, std::vector<int>(n));
    for(int i = 0; i < n; i++){
        for(int j = 0; j < n; j++){
            std::cin >> C[i][j];
        }
    }
    std::vector<int>a(n);
    for(int i = 0; i < n; i++){
        a[i] = i;
    }

    int ans = inf;
    std::vector<int>res;
    do{
        int sum = 0;
        for(int i = 0; i < n; i++){
            sum += C[i][a[i]];
        }

        if(sum < ans){
            ans = sum;
            res = a;
        }
    }while(std::next_permutation(a.begin(), a.end()));
    
    std::cout << ans << "\n";
    for(int i = 0; i < n; i++){
        std::cout << res[i] + 1 << " \n"[i == n - 1];
    }
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



# 6.课后习题

## 18931分形 

递归好题

首先可以发现，等级为$n$的盒子应该有$3^{n-1}$行，每一级盒子都由五部分上一级盒子组成，每一部分的占据$3^n-2$行，我们可以通过简单计算算出每一部分的起始行位置，然后递归地画出整个盒子

当然，我们发现$n$最大只有5，如果你是打表大师的话，完全可以$O(1)$通过本题！

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

std::vector<std::string>s;

void draw(int layer, int d){
    if(layer == 1){
        s[d].push_back('X');
        return;
    }//最小分级

    int x = pow(3, layer - 2);//当前等级占据行数
    draw(layer - 1, d);//画左上角部分
    for(int i = 0; i < x; i++){
        for(int j = 0; j < x; j++){
            s[d + i].push_back(' ');
        }
    }//填充左上角和右上角之间的空格
    
    draw(layer - 1, d);//画右上角部分

    for(int i = 0; i < x; i++){
        for(int j = 0; j < x; j++){
            s[d + x + i].push_back(' ');
        }
    }//填充中间部分左边的空格
    
    draw(layer - 1, d + x);//画中间部分
    
    for(int i = 0; i < x; i++){
        for(int j = 0; j < x; j++){
            s[d + x + i].push_back(' ');
        }
    }//填充中间部分右边的空格

    draw(layer - 1, d + 2 * x);//画左下角部分
    
    for(int i = 0; i < x; i++){
        for(int j = 0; j < x; j++){
            s[d + 2 * x + i].push_back(' ');
        }
    }//填充左下角部分和右下角部分之间的空格
    
    draw(layer - 1, d + 2 * x);//画右下角部分
}
int main(){
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif
    
    int n;
    std::cin>>n;
    int m = pow(3, n - 1);
    s.resize(m);

    draw(n, 0);
    for(int i = 0; i < m; i++){
        std::cout << s[i] << "\n";
    }
    return 0;
}


/*

*/
```



## 19180 集合划分问题一

①$n$独立作为一个子集，问题变成$n-1$个数划分为$m-1$个集合

②$n$插入别的子集，有$m$种方法，先求$n-1$个数划分为$m$个集合的方法数

递归求解即可

涉及到大量重复计算，可用记忆化搜索优化

* 时间复杂度$O(n \times m)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, m;
    std::cin >> n >> m;

    std::vector<std::vector<i64>>f(n + 1, std::vector<i64>(m + 1));
    for(int i = 0; i <= n; i++){
        f[i][1] = 1;
        if(i <= m){
            f[i][i] = 1;
        }
    }

    auto dfs = [&](auto &&self, int x, int y) -> i64 {
        
        if(f[x][y]){
            return f[x][y];
        }

        f[x][y] = self(self, x - 1, y - 1) + self(self, x - 1, y) * y;
        return f[x][y];
    };

    std::cout << dfs(dfs, n, m) << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 19448 集合划分问题二

和上题大差不差，枚举一下子集个数即可

* 时间复杂度$O(n^2)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n;
    std::cin >> n;

    std::vector<std::vector<i64>>f(n + 1, std::vector<i64>(n + 1));
    for(int i = 0; i <= n; i++){
        f[i][1] = f[i][i] = 1;
    }

    auto dfs = [&](auto &&self, int x, int y) -> i64 {
        
        if(f[x][y]){
            return f[x][y];
        }

        f[x][y] = self(self, x - 1, y - 1) + self(self, x - 1, y) * y;
        return f[x][y];
    };

    i64 ans = 0;
    for(int i = 1; i <= n; i++){
        ans += dfs(dfs, n, i);
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 19529 照明灯安装

 答案具有单调性，于是二分答案

如何检查是否合法呢，假设当前要检查的距离为$d$，我们从前往后，每隔$d$放一个照明灯，最后看能放的照明灯数是否$\ge k$即可

* 时间复杂度$O(nlogn)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, k;
    std::cin >> n >> k;
    std::vector<int>x(n);
    for(int i = 0; i < n; i++){
        std::cin >> x[i];
    }
    int lo = 1, hi = x.back() - x.front();
    auto check = [&](int mid){
        int cnt = 0;
        for(int i = 0, j; i < n; i = j){
            cnt++;
            j = i + 1;
            while(j < n && x[j] - x[i] < mid){
                j++;
            }
        }
        return cnt >= k;
    };

    int ans;
    while(lo <= hi){
        int mid = lo + hi >> 1;
        if(check(mid)){
            ans = mid;
            lo = mid + 1;
        }else{
            hi = mid - 1;
        }
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 19184 传球游戏

$dp[i][j]$表示第$j$轮第$i$个同学拿到球的方法数

状态转移方程：$dp[j][i] = dp[(j - 1 + n) \% n][i - 1] + dp[(j + 1) \% n][i - 1]$

* 时间复杂度$O(n \times m)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    int n, m;
    std::cin >> n >> m;
    std::vector<std::vector<int>>dp(n, std::vector<int>(m + 1, 0));
    dp[0][0] = 1;
    for(int i = 1; i <= m; i++){
        for(int j = 0; j < n; j++){
            dp[j][i] += dp[(j - 1 + n) % n][i - 1] + dp[(j + 1) % n][i - 1];
        }
    }
    std::cout << dp[0][m] <<"\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```



## 18308 最长公共子序列

$dp[i][j]$表示$a$串前$i$个字符和$b$串前$j$个字符的最长公共子序列长度

（下面$a$和$b$串索引均从$0$开始，$dp$数组索引从$1$开始）

讨论：

若$a[i-1]=b[j-1]$，找到一个匹配字符，问题转化为$a$串前$i-1$个字符和$b$串前$j-1$个字符的最长公共子序列长度，$dp[i][j]=dp[i-1][j-1]+1$

否则，考虑$a$串前$i-1$个字符和$b$串前$j$个字符的最长公共子序列长度，以及$a$串前$i$个字符和$b$串前$j-1$个字符的最长公共子序列长度

$dp[i][j]=max(dp[i-1][j],dp[i][j-1])+1$

* 时间复杂度$O(n \times m)$

```cpp
#include<bits/stdc++.h>

using i64 = long long;
using u64 = unsigned long long;
using i128 = __int128;
using u128 = unsigned __int128;
using pii = std::pair<int,int>;
using pll = std::pair<i64,i64>;
using pli = std::pair<i64,int>;
using pil = std::pair<int,i64>;
constexpr i64 INF = 0x3f3f3f3f3f3f3f3fll;
constexpr int inf = 0x3f3f3f3f;

void solve(){
    std::string a,b;
    std::cin >> a >> b;
    int m = a.size(), n = b.size();
    int ans = 0;
    std::vector<std::vector<int>>dp(m + 1, std::vector<int>(n + 1,0));
    for(int i = 1; i <= m; i++){
        for(int j = 1; j <= n; j++){
            if(a[i - 1] == b[j - 1]){
                dp[i][j] = dp[i - 1][j - 1] + 1;
            }else{
                dp[i][j] = std::max(dp[i - 1][j], dp[i][j - 1]);
            }
            ans = std::max(ans, dp[i][j]);
        }
    }
    std::cout << ans << "\n";
}             

int main(){ 
    std::ios::sync_with_stdio(false);    
    std::cin.tie(nullptr);

#ifdef LOCAL
    freopen("make.txt", "r", stdin);
    freopen("a.txt", "w", stdout);
#endif

    int T = 1;
    // std::cin >> T;
    while(T--){
        solve();
    }
    return 0; 
}   
/* 

*/  
```

