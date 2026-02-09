# 0x23 BFS

## 引言

广度优先搜索是以广度为优先的搜索算法。

## 例题

[994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/description/https://leetcode.cn/problems/rotting-oranges/description/)

```c++
class Solution {
public:
    int orangesRotting(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        int cnt = 0;
        queue<pair<int, int> > que;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) cnt++;
                else if (grid[i][j] == 2) que.push({i, j});
            }
        }
        if (cnt == 0) return 0;
        int dep = 0;
        int dir[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
        vector<vector<int> > vis(m, vector<int>(n, 0));
        while (!que.empty()) {
            int l = que.size();
            dep++;
            for (int i = 0; i < l; i++) {
                auto it = que.front();
                que.pop();
                int x = it.first;
                int y = it.second;
                if (vis[x][y]) continue;
                vis[x][y] = 1;
                if (cnt == 0) return dep;
                if (grid[x][y] == 2) {
                    for (int j = 0; j < 4; j++) {
                        int xx = x + dir[j][0], yy = y + dir[j][1];
                        if (xx >= 0 && xx < m && yy >= 0 && yy < n) {
                            if (grid[xx][yy] == 1) {
                                grid[xx][yy] = 2;
                                cnt--;
                            }
                            if (cnt == 0) return dep;
                            if (!vis[xx][yy]) que.push({xx, yy});
                        }
                    }
                }

            }
        }
        return -1;
    }
};
```

[752. 打开转盘锁](https://leetcode.cn/problems/open-the-lock/description/)

```c++
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        unordered_set<string> st; 
        unordered_map<string, bool> vis;
        for (auto& s : deadends) st.insert(s);
        int ans = INT_MAX;
        queue<string> que;
        que.push("0000");
        int dep = -1;
        while (!que.empty()) {
            int l = que.size();
            dep++;
            while (l--) {
                string str = que.front();
                que.pop();
                if (vis[str]) continue;
                if (st.count(str)) continue;
                vis[str] = true;
                if (str == target) return dep;
                for (int i = 0; i < 4; i++) {
                    char num = str[i];
                    str[i] = str[i] != '9' ? str[i] + 1 : '0';
                    if (!st.count(str)) que.push(str);
                    str[i] = num;

                    str[i] = str[i] != '0' ? str[i] - 1 : '9';
                    if (!st.count(str)) que.push(str);
                    str[i] = num;
                }
            }
        }
        return ans == INT_MAX ? -1 : ans;
    }
};
```

可以通过双向广度优先搜索进一步提高搜索效率。

[127. 单词接龙](https://leetcode.cn/problems/word-ladder/description/)

```c++
class Solution {
public:
    void add_edge(unordered_map<string, vector<string>>& mp, string l, string r) {
        if (l == r) return;
        int cnt = 0;
        int n = l.size();
        for (int i = 0; i < n; i++) cnt += (l[i] != r[i]);
        if (cnt == 1) {
            mp[l].push_back(r);
            mp[r].push_back(l);
        }
    }

    struct Node {
        string s;
        int d, c;
    };

    int ladderLength(string begin, string end, vector<string>& words) {
        unordered_map<string, vector<string> > g;
        int n = words.size();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < i; j++) {
                add_edge(g, words[i], words[j]);
            }
        }
        for (int i = 0; i < n; i++)
            add_edge(g, begin, words[i]);

        queue<Node> que;
        unordered_map<string, int> lst, rst;
        que.push({begin, 0, 0});
        que.push({end, 0, 1});
        while (!que.empty()) {
            auto q = que.front();
            que.pop();
            string s = q.s;
            int c = q.c;
            if (c == 0) lst[s] = q.d;
            if (c == 1) rst[s] = q.d;
            if (c == 0 && rst.count(s)) return q.d + rst[s] + 1;
            if (c == 1 && lst.count(s)) return q.d + lst[s] + 1;
            for (auto& wd : g[s]) {
                if (c == 0 && !lst.count(wd) || c == 1 && !rst.count(wd))
                    que.push({wd, q.d + 1, c});
            }
        }
        return 0;
    }
};
```
