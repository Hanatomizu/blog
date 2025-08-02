+++
title = 'ABC416C Concat(X Th) 题解'
date = 2025-08-02T12:13:00+08:00
math = true
katex = true
draft = false
+++

[题目传送门 ABC416C](https://atcoder.jp/contests/abc416/tasks/abc416_c)

### 0x01 思路
注意到数据范围 \\(1 \leq N \leq 10\\), \\(1 \leq K \leq 5\\)，可以考虑直接 dfs 暴力枚举每一种组合。

### 0x02 复杂度分析
dfs 内枚举 $n$ 个字符串，深度为 \\(K\\)，复杂度 \\(O(N^K)\\)。找第 $x$ 小的可以使用 `nth_element()`，线性复杂度。故总复杂度为 \\(O(N^K)\\)。

### 0x03 示例代码

```cpp
#include <bits/stdc++.h>
using namespace std;
using ll = long long;
inline void umax(int &x, int y) {(x < y) && (x = y);}
inline void umin(int &x, int y) {(x > y) && (x = y);}

int n, k, x;
vector<string> s;
string re[100005];
int cnt;

void dfs(int deps, string ss) {
    if (deps == 0) { return re[++cnt] = ss, void(); }
    for (int i = 1; i <= n; ++i) {
        dfs(deps-1, ss+s[i]);
    }
}

signed main(){
    #ifdef HANATOMIZU_LOCAL_TEST
    freopen("input.in", "r", stdin);
    #endif // HANATOMIZU_LOCAL_TEST
    ios::sync_with_stdio(0), cin.tie(0), cout.tie(0);

    cin >> n >> k >> x;
    s.resize(n+1);
    for (int i = 1; i <= n; ++i) {
        cin >> s[i];
    }
    dfs(k, string());
    nth_element(re+1, re+x, re+cnt+1);
    cout << re[x] << endl;;

    return 0;
}
```