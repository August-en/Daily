# 背包九讲

## 目录  

<!-- GFM-TOC -->

* [01背包](#01背包)
* [完全背包](#完全背包)
* [多重背包](#多重背包)
* [混合背包](#混合背包)
* [二维费用背包](#二维费用背包)
* [分组背包](#分组背包)
* [背包问题求方案数](#背包问题求方案数)
* [背包问题求具体方案](#背包问题求具体方案)
* [有依赖的背包问题](#有依赖的背包问题)

<!-- GFM-TOC -->

> 先循环物品  
>
> 再循环体积
>
> 再循环决策  

[背包九讲在线调试](https://www.acwing.com/problem/)

## 01背包

有 ***N*** 件物品和一个容量是 ***V*** 的背包。每件物品只能使用一次。

第 $$i$$ 件物品的体积是 *$$v_i$$*，价值是 $$w_i$$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

#### 输入格式

第一行两个整数，***N***，***V***，用空格隔开，分别表示物品数量和背包容积。

接下来有 ***N*** 行，每行两个整数  *$$v_i$$*, $$w_i$$,用空格隔开，分别表示第 $$i$$ 件物品的体积和价值。

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

$$0<N,V≤1000$$
$$0<v_i,w_i≤1000$$

#### 输入样例

```
4 5
1 2
2 4
3 4
4 5
```

#### 输出样例：

```
8
```

**代码**

```C++
#include<iostream>
#include<cstring>
#include<algorithm>

using namespace std;

const int N = 1010;

int n, m;
int f[N];
int v[N], w[N];

int main(){
    cin >> n >> m;
    for(int i = 1; i <= n; i++) cin >> v[i] >> w[i];
    
    for(int i = 1; i <= n ; i++)
        for(int j = m; j >= v[i]; j--)
            f[j] = max(f[j], f[j-v[i]] + w[i]);
    
    cout << f[m] << endl;
    return 0;
}
```

## 完全背包

有 **N** 种物品和一个容量是 **V** 的背包，每种物品都有无限件可用。

第 $$i$$ 种物品的体积是 $$v_i$$，价值是 $$w_i$$。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。
输出最大价值。

#### 输入格式

第一行两个整数，**N**，**V**，用空格隔开，分别表示物品种数和背包容积。

接下来有 **N** 行，每行两个整数 $$v_i$$,$$w_i$$，用空格隔开，分别表示第 $$i$$ 种物品的体积和价值。

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

$$0<N,V≤1000$$
$$0<vi,wi≤1000$$

#### 输入样例

```
4 5
1 2
2 4
3 4
4 5
```

#### 输出样例：

```
10
```

**代码**

```c++
#include<iostream>
#include<cstring>
#include<algorithm>

using namespace std;

const int N = 1010;

int f[N];
int m, n;

int main()
{
    cin >> n >> m;
    for(int i = 0; i < n; i++){
        int v, w;
        cin >> v >> w;
        for(int j = v; j <= m; j++)
            f[j] = max(f[j], f[j - v] + w);
    }
    
    cout << f[m] << endl;
    
    return 0;
}
```

## 多重背包

有 **N** 种物品和一个容量是 **V** 的背包。

第 $$i $$种物品最多有$$ s_i$$ 件，每件体积是 $$v_i$$，价值是 $$w_i$$。

求解将哪些物品装入背包，可使物品体积总和不超过背包容量，且价值总和最大。
输出最大价值。

#### 输入格式

第一行两个整数，**N**，**V**，用空格隔开，分别表示物品种数和背包容积。

接下来有** N** 行，每行三个整数 $$v_i$$,$$w_i$$, 用空格隔开，分别表示第 $$i$$ 种物品的体积、价值和数量。

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

$$0<N,V≤100$$
$$0<wi,si≤100$$

#### 输入样例

```
4 5
1 2 3
2 4 1
3 4 3
4 5 2
```

#### 输出样例：

```
10
```

**代码1**

```c++
#include<iostream>
#include<cstring>
#include<algorithm>

using namespace std;

const int N = 110;

int n, m;
int f[N];

int main()
{
    cin >> n >> m;
    for(int i = 0; i < n; i++){
        int v, w, s;
        cin >> v >> w >> s;
        for(int j = m; j >= 0; j--){
            for(int k = 1; k <= s && k * v <= j; k++)
                f[j] = max(f[j], f[j-k*v] + k*w);
        }
    }
    
    cout << f[m] << endl;
    
    return 0;
}
```

**代码2**

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 2010;

int n, m;
int f[N];

struct Good
{
  int v, w;  
};

int main()
{
    vector<Good> goods;
    cin >> n >> m;
    for(int i = 0; i < n; i++){
        int v, w, s;
        cin >> v >> w >> s;
        for(int k = 1; k <= s; k *= 2){  // 利用二进制优化拆分成1~s个物品，O(s) -> O(log(s))
            s -= k;
            goods.push_back({v * k, w * k});
        }
        if(s > 0) goods.push_back({v * s, w * s});
    }
    
    for(auto good: goods)
        for(int j = m; j >= good.v; j--)
            f[j] = max(f[j], f[j - good.v] + good.w);

    cout << f[m] << endl;
    return 0;
}
```

## 混合背包

有 **N** 种物品和一个容量是 **V** 的背包。

物品一共有三类：

- 第一类物品只能用1次（01背包）；
- 第二类物品可以用无限次（完全背包）；
- 第三类物品最多只能用 $$s_i$$ 次（多重背包）；

每种体积是 $$v_i$$，价值是 $$w_i$$。

求解将哪些物品装入背包，可使物品体积总和不超过背包容量，且价值总和最大。
输出最大价值。

#### 输入格式

第一行两个整数，**N**，**V**，用空格隔开，分别表示物品种数和背包容积。

接下来有 **N** 行，每行三个整数 $$v_i$$,$$w_i$$，用空格隔开，分别表示第 $$i$$ 种物品的体积、价值和数量。

- $$s_i=−1$$ 表示第 $$i$$ 种物品只能用1次；
- $$s_i=0$$ 表示第 $$i$$ 种物品可以用无限次；
- $$s_i>0$$ 表示第 $$i$$ 种物品可以使用 $$s_i$$ 次；

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

$$0<N,V≤1000$$
$$0<v_i,w_i≤1000$$
$$−1≤s_i≤1000$$

#### 输入样例

```
4 5
1 2 -1
2 4 1
3 4 0
4 5 2
```

#### 输出样例：

```
8
```

**代码**

```c++
#include <iostream>
#include <cstring>
#include <algorithm>
#include <vector>

using namespace std;

const int N = 1010;

int n, m;
int f[N];

struct Thing
{
    int kind;
    int v, w;
};
vector<Thing> things;

int main()
{
    cin >> n >> m;
    for(int i = 0; i < n; i++){
        int v, w, s;
        cin >> v >> w >> s;
        if(s < 0) things.push_back({-1, v, w});
        else if(s == 0) things.push_back({0, v, w});
        else{  // 多重背包，二进制优化转换为01背包
            for(int k = 1; k <= s; k <<= 1){
                s -= k;
                things.push_back({-1, v * k, w * k});
            }
            if(s > 0) things.push_back({-1, v * s, w * s});
        }
    }
    
    for(auto thing : things){
        if(thing.kind < 0)  // 01背包
            for(int j = m; j >= thing.v; j--) f[j] = max(f[j], f[j - thing.v] + thing.w);
        else  // 完全背包
            for(int j = thing.v; j <= m; j++) f[j] = max(f[j], f[j - thing.v] + thing.w);
    }
    
    cout << f[m] << endl;
    return 0;
}
```

## 二位费用背包

有 **N** 件物品和一个容量是 **V** 的背包，背包能承受的最大重量是 **M**。

每件物品只能用一次。体积是 $$v_i$$，重量是 $$m_i$$，价值是 $$w_i$$。

求解将哪些物品装入背包，可使物品总体积不超过背包容量，总重量不超过背包可承受的最大重量，且价值总和最大。
输出最大价值。

#### 输入格式

第一行两个整数，**N**，**V**, **M**，用空格隔开，分别表示物品件数、背包容积和背包可承受的最大重量。

接下来有 **N** 行，每行三个整数 $$v_i$$,$$m_i$$,$$w_i$$，用空格隔开，分别表示第 $$i$$ 件物品的体积、重量和价值。

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

$$0<N≤1000$$
$$0<V,M≤100$$
$$0<v_i,m_i≤100$$
$$0<w_i≤1000$$

#### 输入样例

```
4 5 6
1 2 3
2 4 4
3 4 5
4 5 6
```

#### 输出样例：

```
8
```

**代码**

```c++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 110;

int n, v, m;
int f[N][N];

int main()
{
    cin >> n >> v >> m;
    for(int i = 0; i < n; i++){
        int a, b, c;
        cin >> a >> b >> c;
        for(int j = v; j >= a; j--)  // 题目要求每个物品最多取一次，因此相当于01背包问题
            for(int k = m; k>= b; k--)  // 枚举重量和体积时都是从大到小枚举
                f[j][k] = max(f[j][k], f[j-a][k-b] + c);
    }
    
    cout << f[v][m] << endl;
    
    return 0;
}
```

## 分组背包问题

有 **N** 组物品和一个容量是 **V** 的背包。

每组物品有若干个，同一组内的物品最多只能选一个。
每件物品的体积是 $$v_{ij}$$，价值是 $$w_{ij}$$，其中 $$i$$ 是组号，$$j$$ 是组内编号。

求解将哪些物品装入背包，可使物品总体积不超过背包容量，且总价值最大。

输出最大价值。

#### 输入格式

第一行有两个整数 **N**，**V**，用空格隔开，分别表示物品组数和背包容量。

接下来有 **N** 组数据：

- 每组数据第一行有一个整数 $$S_i$$，表示第 $$i$$ 个物品组的物品数量；
- 每组数据接下来有 $$S_i$$ 行，每行有两个整数 $$v_{ij},w_{ij}$$，用空格隔开，分别表示第 $$i$$ 个物品组的第 $$j$$ 个物品的体积和价值；

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

$$0<N,V≤100$$
$$0<S_i≤100$$
$$0<v_{ij},w_{ij}≤100$$

#### 输入样例

```
3 5
2
1 2
2 4
1
3 4
1
4 5
```

#### 输出样例：

```
8
```

**代码**

```C++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 110;

int n, m;
int f[N], v[N], w[N];

int main()
{
    cin >> n >> m;
    for(int i = 0; i < n; i++){
        int s;
        cin >> s;
        for(int j = 0; j < s; j++) cin >> v[j] >> w[j];
        for(int j = m; j >= 0; j--)
            for(int k = 0; k < s; k++)
                if(j >= v[k])
                    f[j] = max(f[j], f[j-v[k]] + w[k]);
    }
    
    cout << f[m] << endl;
    
    return 0;
}
```

## 背包问题求方案数

有 NN 件物品和一个容量是 VV 的背包。每件物品只能使用一次。

第 ii 件物品的体积是 vivi，价值是 wiwi。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。

输出 **最优选法的方案数**。注意答案可能很大，请输出答案模 109+7109+7 的结果。

#### 输入格式

第一行两个整数，N，VN，V，用空格隔开，分别表示物品数量和背包容积。

接下来有 NN 行，每行两个整数 vi,wivi,wi，用空格隔开，分别表示第 ii 件物品的体积和价值。

#### 输出格式

输出一个整数，表示 **方案数** 模 109+7109+7 的结果。

#### 数据范围

0<N,V≤10000<N,V≤1000
0<vi,wi≤10000<vi,wi≤1000

#### 输入样例

```
4 5
1 2
2 4
3 4
4 6
```

#### 输出样例：

```
2
```

**代码**

```C++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1010, mod = 1000000007, INT_MIN = -2147483648;

int n, m;
int f[N], g[N];  // f[i]表示装入物品的体积【恰好】是i时的总价值，g[i]表示此时的方案数

int main()
{
    cin >> n >> m;
    g[0] = 1;  // g[0]别忘初始化
    for(int i = 1; i <= m; i++) f[i] = INT_MIN;
    for(int i = 0; i < n; i++){
        int v, w;
        cin >> v >> w;
        for(int j = m; j >= v; j--){
            int t = max(f[j], f[j-v] + w);
            int s = 0;
            if(t == f[j]) s += g[j];  // 统计方案数
            if(t == f[j-v] + w) s += g[j-v];  // 统计方案数
            if(s >= mod) s -= mod;
            f[j] = t;
            g[j] = s;
        }
    }
    int maxw = INT_MIN;
    for(int j = 0; j <= m; j++) maxw = max(maxw, f[j]);
    int res = 0;
    for(int j = 0; j <= m; j++){
        if(f[j] == maxw){  // 有多个j对应maxw时，分别将方案数相加
            res += g[j];
            if(res >= mod) res -= mod;
        }
    }
    
    cout << res << endl;
    return 0;
}
```

## 背包问题求具体方案

有 NN 件物品和一个容量是 VV 的背包。每件物品只能使用一次。

第 ii 件物品的体积是 vivi，价值是 wiwi。

求解将哪些物品装入背包，可使这些物品的总体积不超过背包容量，且总价值最大。

输出 **字典序最小的方案**。这里的字典序是指：所选物品的编号所构成的序列。物品的编号范围是 1…N1…N。

#### 输入格式

第一行两个整数，N，VN，V，用空格隔开，分别表示物品数量和背包容积。

接下来有 NN 行，每行两个整数 vi,wivi,wi，用空格隔开，分别表示第 ii 件物品的体积和价值。

#### 输出格式

输出一行，包含若干个用空格隔开的整数，表示最优解中所选物品的编号序列，且该编号序列的字典序最小。

物品编号范围是 1…N1…N。

#### 数据范围

0<N,V≤10000<N,V≤1000
0<vi,wi≤10000<vi,wi≤1000

#### 输入样例

```
4 5
1 2
2 4
3 4
4 6
```

#### 输出样例：

```
1 4
```

**代码**

```C++
#include <iostream>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 1010;

int n, m;
int v[N], w[N], f[N][N];

int main()
{
    cin >> n >> m;
    for(int i = 1; i <= n; i++) cin >> v[i] >> w[i];
    for(int i = n; i >= 1; i--){
        for(int j = 0; j <= m; j++){
            f[i][j] = f[i+1][j];
            if(j >= v[i])
                f[i][j] = max(f[i][j], f[i+1][j-v[i]] + w[i]);
        }
    }
    
    int vol = m;
    for(int i = 1; i <= n; i++){
        if(vol >= v[i] && f[i][vol] == f[i+1][vol-v[i]] + w[i]){
            cout << i << ' ';
            vol -= v[i];
        }
    }
    return 0;
}
```

## 有依赖的背包问题

有 NN 个物品和一个容量是 VV 的背包。

物品之间具有依赖关系，且依赖关系组成一棵树的形状。如果选择一个物品，则必须选择它的父节点。

如下图所示：
![QQ图片20181018170337.png](https://www.acwing.com/media/article/image/2018/10/18/1_bb51ecbcd2-QQ%E5%9B%BE%E7%89%8720181018170337.png)

如果选择物品5，则必须选择物品1和2。这是因为2是5的父节点，1是2的父节点。

每件物品的编号是 ii，体积是 vivi，价值是 wiwi，依赖的父节点编号是 pipi。物品的下标范围是 1…N1…N。

求解将哪些物品装入背包，可使物品总体积不超过背包容量，且总价值最大。

输出最大价值。

#### 输入格式

第一行有两个整数 N，VN，V，用空格隔开，分别表示物品个数和背包容量。

接下来有 NN 行数据，每行数据表示一个物品。
第 ii 行有三个整数 vi,wi,pivi,wi,pi，用空格隔开，分别表示物品的体积、价值和依赖的物品编号。
如果 pi=−1pi=−1，表示根节点。 **数据保证所有物品构成一棵树。**

#### 输出格式

输出一个整数，表示最大价值。

#### 数据范围

1≤N,V≤1001≤N,V≤100
1≤vi,wi≤1001≤vi,wi≤100

父节点编号范围：

- 内部结点：1≤pi≤N1≤pi≤N;
- 根节点 pi=−1pi=−1;

#### 输入样例

```
5 7
2 3 -1
2 2 1
3 5 1
4 7 2
3 6 2
```

#### 输出样例：

```
11
```

```C++
#include <iostream>
#include <cstdio>
#include <cstring>
#include <algorithm>

using namespace std;

const int N = 110;

int n, m;
int h[N], e[N], ne[N], idx;
int v[N], w[N], f[N][N];

void add(int a, int b){
    e[idx] = b, ne[idx] = h[a], h[a] = idx++;
}

void dfs(int u){
    for(int i = h[u]; i != -1; i = ne[i]){
        int son = e[i];
        dfs(son);
        for(int j = m - v[u]; j >= 0; j--)
            for(int k = 0; k <= j; k++)
                f[u][j] = max(f[u][j], f[u][j-k] + f[son][k]);
    }
    for(int i = m; i >= v[u]; i--) f[u][i] = f[u][i-v[u]] + w[u];
    for(int i = 0; i < v[u]; i++) f[u][i] = 0;
}

int main(){
    memset(h, -1, sizeof(h));
    cin >> n >> m;
    int root;
    for(int i = 1; i <= n; i++){
        int p;
        cin >> v[i] >> w[i] >> p;
        if(p == -1) root = i;
        else add(p, i);
    }
    
    dfs(root);
    cout << f[root][m] << endl;
    
    return 0;
}
```

