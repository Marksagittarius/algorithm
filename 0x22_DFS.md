# 0x22 DFS

## 引言

深度优先搜索是以深度为优先的搜索算法。

## 例题

[200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/description/)

```c++
class Solution {
public:
    int ans = 0;
    vector<vector<int>> vis;
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        vis = vector<vector<int> >(m, vector<int>(n, 0));
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    f(grid, i, j, m, n);
                    ans++;
                }
            }
        }
        return ans;
    }

    int dir[4][2] = {{0, 1}, {0, -1}, {-1, 0}, {1, 0}};
    void f(vector<vector<char>>& arr, int i, int j, int m, int n) {
        if (i < 0 || j < 0 || i >= m || j >= n) return;
        if (vis[i][j]) return;
        if (arr[i][j] == '0') return;
        vis[i][j] = 1;
        for (int l = 0; l < 4; l++)
            f(arr, i + dir[l][0], j + dir[l][1], m, n);
        arr[i][j] = '0';
    }
};
```

[785. 判断二分图](https://leetcode.cn/problems/is-graph-bipartite/description/https://leetcode.cn/problems/is-graph-bipartite/description/)

```c++
class Solution {
public:
    vector<int> color;
    bool f(vector<vector<int>>& g, int i, int c) {
        if (color[i] != 0) return color[i] == c;
        int cc = c == 1 ? 2 : 1;
        color[i] = c;
        bool ans = true;
        for (auto& j : g[i])
            ans = ans && f(g, j, cc);
        return ans;
    }
    bool isBipartite(vector<vector<int>>& graph) {
        int n = graph.size();
        color = vector<int>(n);
        bool ans = true;
        for (int i = 0; i < n; i++) {
            if (color[i] == 0) {
                ans = ans && f(graph, i, 1);
            }
        }
        return ans;
    }
};
```

[399. 除法求值](https://leetcode.cn/problems/evaluate-division/description/)

```c++
class Solution {
public:
    unordered_set<string> st;
    void f(unordered_map<string, vector<pair<string, double>>>& g, vector<double>& ans, int i, string l, string r, double num) {
        if (!g.count(l) || !g.count(r)) {
            ans[i] = -1.0;
            return;
        }
        if (l == r) {
            ans[i] = num;
            return;
        }
        if (st.count(l)) return ;
        st.insert(l);
        for (auto& edg : g[l]) {
            f(g, ans, i, edg.first, r, num * edg.second);
        }
    }
    vector<double> calcEquation(vector<vector<string>>& equations, vector<double>& values, vector<vector<string>>& queries) {
        int n = equations.size();
        int m = queries.size();
        vector<double> ans(m, -1.0);
        unordered_map<string, vector<pair<string, double>>> g;
        for (int i = 0; i < n; i++) {
            auto eq = equations[i];
            double val = values[i];
            g[eq[0]].push_back({ eq[1], val });
            g[eq[1]].push_back({ eq[0], 1.0 / val });
        }
        int i = 0;
        for (auto& q : queries) {
            string l = q[0];
            string r = q[1];
            st.clear();
            f(g, ans, i++, l, r, 1.0);
        }
        return ans;
    }
};
```

[417. 太平洋大西洋水流问题](https://leetcode.cn/problems/pacific-atlantic-water-flow/description/https://leetcode.cn/problems/pacific-atlantic-water-flow/description/)

```c++
class Solution {
public:
    vector<vector<int>> ans;
    vector<vector<int>> cnt;
    int dx[4] = {1, 0, -1, 0};
    int dy[4] = {0, 1, 0, -1};

    void f1(vector<vector<int> >& grid, int i, int j, int m, int n) {
        if (i < 0 || j < 0 || i >= m || j >= n) return;
        if (cnt[i][j] == 1) return;
        cnt[i][j] = 1;
        for (int k = 0; k < 4; k++) {
            int xx = i + dx[k];
            int yy = j + dy[k];
            if (xx >= 0 && yy >= 0 && xx < m && yy < n) {
                if (grid[i][j] <= grid[xx][yy])
                    f1(grid, xx, yy, m, n);
            }
        }
    }
    void f2(vector<vector<int> >& grid, int i, int j, int m, int n) {
        if (i < 0 || j < 0 || i >= m || j >= n) return;
        if (cnt[i][j] == -1) return;
        if (cnt[i][j] == 1) ans.push_back({i, j});
        cnt[i][j] = -1;
        for (int k = 0; k < 4; k++) {
            int xx = i + dx[k];
            int yy = j + dy[k];
            if (xx >= 0 && yy >= 0 && xx < m && yy < n) {
                if (grid[i][j] <= grid[xx][yy])
                    f2(grid, xx, yy, m, n);
            }
        }
    }
    vector<vector<int>> pacificAtlantic(vector<vector<int>>& heights) {
        int m = heights.size();    
        int n = heights[0].size();
        cnt = vector<vector<int> >(m, vector<int>(n));
        for (int i = 0; i < m; i++) f1(heights, i, 0, m, n);
        for (int j = 0; j < n; j++) f1(heights, 0, j, m, n);
        for (int i = 0; i < m; i++) f2(heights, i, n - 1, m, n);
        for (int j = 0; j < n; j++) f2(heights, m - 1, j, m, n);
        return ans;
    }
};
```

深度优先搜索算法能够被应用于欧拉回路问题：

[332. 重新安排行程](https://leetcode.cn/problems/reconstruct-itinerary/description/)

```c++
class Solution {
public: 
    unordered_map<string, priority_queue<string, vector<string>, greater<string>>> g;
    vector<string> ans;

    void f(string& s) {
        while (g.count(s) && g[s].size() > 0) {
            string t = g[s].top();
            g[s].pop();
            f(t);
        }
        ans.emplace_back(s);
    }

    vector<string> findItinerary(vector<vector<string>>& tickets) {
        for (auto& t : tickets) {
            g[t[0]].emplace(t[1]);
        }
        string s = "JFK";
        f(s);
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```
