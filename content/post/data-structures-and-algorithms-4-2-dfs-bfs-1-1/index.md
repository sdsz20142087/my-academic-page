---
title: Data Structures and Algorithms 4-3. DFS-BFS (1/1)
date: 2022-03-02T03:42:30.484Z
summary: "***Leetcode- 993, 542, 1091, 752, offer - 13, 130, 494, 473, 39***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
***Leetcode- 993, 542, 1091, 752, offer - 13, 130, 494, 473, 39***

993

```cpp
class Solution {
public:
    int level = 0;
    TreeNode* dfs(TreeNode* root, int target, int k) {
        if (!root) return nullptr;
        if (root->val == target) {
            level = k; 
            return root;
        }
        TreeNode* l_ans = dfs(root->left, target, k + 1);
        if (l_ans && l_ans->val == target) return root;
        if (l_ans && l_ans->val != target) return l_ans;
        TreeNode* r_ans = dfs(root->right, target, k + 1);
        if (r_ans && r_ans->val == target) return root;
        if (r_ans && r_ans->val != target) return r_ans;
        return nullptr;

    }

    bool isCousins(TreeNode* root, int x, int y) {
        int x_v;
        TreeNode* x_fa = dfs(root, x, 0);
        x_v = level;
        level = 0;
        TreeNode* y_fa = dfs(root, y, 0);
        return (x_fa != y_fa) && (x_v == level);
    }
};
```

542

```cpp
class Solution {
public:
    struct data{
        int i, j, level;
    };

    vector<vector<int>> updateMatrix(vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size(), x, y;
        int dir[4][2] = {0, 1, 1, 0, 0, -1, -1, 0};
        vector<vector<int>> vis;
        queue<data> q;
        for (int i = 0; i < m; i++) {
            vis.push_back(vector<int>());
            for (int j = 0; j < n; j++) {
                vis[i].push_back(-1);
                if (mat[i][j] != 0) continue;
                q.push(data{i, j, 0});
                vis[i][j] = 0;
            }
        }
        while (!q.empty()) {
            data cur = q.front();
            for (int i = 0; i < 4; i++) {
                x = cur.i + dir[i][0];
                y = cur.j + dir[i][1];
                if (x < 0 || x >= m) continue;
                if (y < 0 || y >= n) continue;
                if (vis[x][y] != -1) continue;
                q.push(data{x, y, cur.level + 1});
                vis[x][y] = cur.level + 1;
            }
            q.pop();
        }
        return vis;
    }
};
```

1091

```cpp
class Solution {
public:
    struct data{
        int x, y;
    };
    int shortestPathBinaryMatrix(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size(), tx, ty, length = 0;
        if (grid[0][0] == 1 || grid[m - 1][n - 1] == 1) return -1;
        vector<vector<int>> mark(m, vector<int>(n, -1));
        int direction[8][2] = {-1, 0, -1, -1, -1, 1, 0, 1, 0, -1, 1, 0, 1, -1, 1, 1};
        queue<data> q;
        q.push(data{0, 0});
        mark[0][0] = 1;
        while (!q.empty()) {
            length++;
            int cnt = q.size();
            for (int k = 0; k < cnt; k++) {
                data cur = q.front();
                if (cur.x == m - 1 && cur.y == n - 1) return length;
                for (int i = 0; i < 8; i++) {
                    tx = cur.x + direction[i][0];
                    ty = cur.y + direction[i][1];
                    if (tx < 0 || tx >= m) continue;
                    if (ty < 0 || ty >= n) continue;
                    if (mark[tx][ty] == 1) continue;
                    if (grid[tx][ty] == 1) continue;
                    q.push(data{tx, ty});
                    mark[tx][ty] = 1;
                }
                q.pop();
            }
        }
        return -1;
    }
};
```

752

```cpp
class Solution {
public:
    unordered_map<string, int> h;
    int openLock(vector<string>& deadends, string target) {
        for (auto x :deadends) h[x] = 1;
        if (h.find("0000") != h.end() || h.find(target) != h.end()) return -1;
        int length = 0;
        string up1, up2;
        queue<string> q;
        q.push("0000");
        h["0000"] = 1;
        while (!q.empty()) {
            int n = q.size();
            for (int i = 0; i < n; i++) {
                string cur = q.front();
                if (cur == target) return length;
                for (int j = 0; j < 4; j++) {
                    up1 = cur;
                    up1[j] = (cur[j] - '0' + 1) % 10 + '0';
                    if (h.find(up1) == h.end()) {
                        h[up1] = 1;
                        q.push(up1);
                    }
                    up2 = cur;
                    up2[j] = (cur[j] - '0' + 9) % 10 + '0';
                    if (h.find(up2) == h.end()) {
                        h[up2] = 1;
                        q.push(up2);
                    }
                }
                q.pop();
            }
            length++;
        }
        return -1;
    }
};
```

offer - 13

```cpp
class Solution {
public:
    int cnt, direction[4][2] = {0, 1, 0, -1, 1, 0, -1, 0}, n_x, n_y;
    bool ValidPos(int x, int y, int k) {
        int ans = 0;
        while (x) {
            ans = ans + x % 10;////
            x /= 10;
        }
        while (y) {
            ans = ans + y % 10;////
            y /= 10;
        }
        return ans <= k;
    }
    void dfs(int x, int y, int k, int m, int n, vector<vector<int>>& mark) {
        if (x < 0 || x >= m) return ;
        if (y < 0 || y >= n) return ;
        if (mark[x][y] != -1) return;
        mark[x][y] = 1;
        if (!ValidPos(x, y, k)) return ;
        cnt++;
        for (int i = 0; i < 4; i++) {
            n_x = x + direction[i][0];
            n_y = y + direction[i][1];
            dfs(n_x, n_y, k, m, n, mark);
        }
        return ;
    }


    int movingCount(int m, int n, int k) {
        vector<vector<int>> mark(m, vector<int>(n, -1));
        dfs(0, 0, k, m, n, mark);
        return cnt;
    }
};
```

130

```cpp
class Solution {
public:
    void dfs(int i, int j, int m, int n, vector<vector<char>>& board) {
        if (i < 0 || i >= m) return ;
        if (j < 0 || j >= n) return ;
        if (board[i][j] != 'O') return ;
        board[i][j] = 'o';
        dfs(i + 1, j, m, n, board);
        dfs(i - 1, j, m, n, board);
        dfs(i, j + 1, m, n, board);
        dfs(i, j - 1, m, n, board);
        return ;
    }

    void solve(vector<vector<char>>& board) {
        int m = board.size(), n = board[0].size();
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'O' && (i == 0 || i == m - 1 || j == 0 || j == n - 1)) {
                    dfs(i, j, m, n, board);
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == 'O') {
                    board[i][j] = 'X';
                } else if (board[i][j] == 'o') {
                    board[i][j] = 'O';
                }
            }
        }
        return ;
    }
};
```

494

```cpp
class Solution {
public:
    typedef pair<int, int> PII;
    map<PII, int> m;
    int dfs(vector<int>& nums, int ind, int target) {
        if (ind == nums.size()) {
            return target == 0;
        }
        if (m.find(PII{ind, target}) != m.end()) return m[PII{ind, target}];
        int ans = 0;
        ans += dfs(nums, ind + 1, target + nums[ind]);
        ans += dfs(nums, ind + 1, target - nums[ind]);
        m[PII{ind, target}] = ans;
        return ans;
    }

    int findTargetSumWays(vector<int>& nums, int target) {
        return dfs(nums, 0, target);;
    }
};
```

473

```cpp
class Solution {
public:
    int min_val = INT32_MAX;
    bool dfs(vector<int>& matchsticks, vector<int>& arr, int ind) {
        if (ind == matchsticks.size()) return true;
        for (int i = 0; i < 4; i++) {
            if (matchsticks[ind] == arr[i] || arr[i] >= matchsticks[ind] + min_val) {
                arr[i] -= matchsticks[ind];
                if (dfs(matchsticks, arr, ind + 1)) return true;
                arr[i] += matchsticks[ind];
            }
        }
        return false;
    }

    bool makesquare(vector<int>& matchsticks) {
        int n = matchsticks.size(), max_val = 0, length = 0;
        for (auto x : matchsticks) {
            length += x;
            max_val = max(max_val, x);
            min_val = min(min_val, x);
        }
        if (length % 4 || (length / 4) < max_val) return false;
        sort(matchsticks.begin(), matchsticks.end(),[](int a, int b)->bool{return a > b;});
        vector<int> arr(4, length / 4);
        return dfs(matchsticks, arr, 0);
    }
};
```

39

```cpp
class Solution {
public:
    vector<vector<int>> ans;
    unordered_map<int, int> m;
    void dfs(vector<int>& candidates, int target, unordered_map<int, int> m, vector<int> buff) {
        if (target < 0) return ;
        if (target == 0) {
            ans.push_back(buff);
            return ;
        }
        for (int i = 0; i < candidates.size(); i++) {
            if (m.find(candidates[i]) != m.end()) continue;
            buff.push_back(candidates[i]);
            dfs(candidates, target - candidates[i], m, buff);
            buff.pop_back();
            m[candidates[i]] = 1;
        }

    }

    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> buff;
        dfs(candidates, target, m, buff);
        return ans;
    }
};
```