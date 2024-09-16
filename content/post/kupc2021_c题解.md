+++
title = 'Kupc2021_c题解'
date = 2024-09-16T14:50:02+08:00
mathJax = true
draft = false
+++

[题目传送门](https://atcoder.jp/contests/kupc2021/tasks/kupc2021_c)

**思路**

首先根据题目可以知道：

1. 在最优解的情况下第 $i$ 个硬币在第 $i$ 个游戏机（题中的 Gacha）中使用。
   > 题中 $B_j < B_{j+1} (1 \leq j \leq N-1)$ 可以得到。

2. 整个移动过程可以视作为从 $0$ 开始向正方向移动，得到若干硬币后走回去玩没有玩的游戏机。

得出了以上结论，dp 的思路就清晰了。

设 $dp_i$ 为玩了前 $i$ 个游戏机时 $B_i$ 的最小值，$m$ 为玩了前 $i$ 个游戏机的迂回成本最小值。得到状态转移方程：
$$m \gets \min(m, dp_{i-1} - A_i \times 2)$$
$$dp_i \gets \min(m, dp_{i-1} + B_i \times 2) + B_i \times 2$$

值得注意的是，对于 $A_i > B_i$ 的情况（即硬币在游戏机前面），可以直接忽略该硬币，把硬币的位置设置为游戏机的位置（即 $A_i \to B_i$）。因为在去往游戏机的路上一定会捡到该硬币。

得到答案为：
$$
ans = \min(dp_n) + \min_{0 \leq i < n}(dp_i + B_n - A_{i+1}) + A_n
$$


---

**代码与分析**

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll=long long;

const unsigned long MAXN = 1e5 + 5;
const ll inf = 1e18;

int n;
// 不开 long long 见祖宗
ll dp[MAXN];
ll ans = inf;

// 这里其实也可以用 int, 但是 max() 和 min() 要求前后类型相同，可能会出现奇奇怪怪的报错
pair<ll, ll> p[MAXN];

int main(){
    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    cin >> n;

    for (int i = 1; i <= n; ++i)
        cin >> p[i].first;
    for (int i = 1; i <= n; ++i) {
        cin >> p[i].second;
        if(p[i].first > p[i].second)
            p[i].second = p[i].first;
    }

    // 由于直接把 p[i].first 的值赋给 p[i].second 可能会出现错误
    // 所以使用 sort 排序
    // 但是本题的数据没有这么坑，不 sort 也可以过
    // sort(p+1, p+n+1, [](const pair<ll, ll> &x, const pair<ll, ll> &y){return x.second < y.second;});

    // dp
    ll m = -(p[1].first<<1);
    for (int i = 1; i <= n; ++i) {
        dp[i] = m + (p[i].second << 1);
        m = min(m, dp[i] - (p[i+1].first << 1));
    }
  
    ans = dp[n];
    for (int i = 0; i < n; ++i) {
        ans = min(ans, dp[i] + p[n].second - p[i+1].first);
    }

    ans += p[n].second;

    cout << ans << endl;

    return 0;
}
```