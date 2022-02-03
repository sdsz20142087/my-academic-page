---
title: Data Structures and Algorithms 2-3. Union-find (2/2)
date: 2022-02-03T15:15:20.570Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# Union-find

***Leetcode- 547, 200, 990, 684, 1319, 128, 947, 1202***

547

```cpp
class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size(), ans = 0;
        UnionSet uSet(n);
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (isConnected[i][j] == 1) uSet.merge(i, j);
            }
        }
        for (int i = 0; i < n; i++) if (uSet.boss[i] == i) ans++;
        return ans;
    }
};
```

200

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int direction[2][2] = {{1, 0}, {0, 1}}, ind = 2;        
        UnionSet uSet(n * m);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '0') continue;
                for (int k = 0; k < ind; k++) {
                    int x = i + direction[k][0], y = j + direction[k][1];
                    if (x < m && y < n && grid[x][y] == '1') uSet.merge(i * n + j, x * n + y);
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1' && uSet.get(i * n + j) == i * n + j) ans++;
            }
        }
        return ans;
    }
};
```

990

```cpp
class Solution {
public:
    bool equationsPossible(vector<string>& equations) {
        UnionSet uSet(26);
        int r1, r2;
        for (auto x: equations) {
            if (x[1] == '!') continue;
            r1 = x[0] - 'a', r2 = x[3] - 'a';
            uSet.merge(r1, r2);
        }
        for (auto x: equations) {
            if (x[1] == '=') continue;
            r1 = x[0] - 'a', r2 = x[3] - 'a';
            if (uSet.get(r1) == uSet.get(r2)) return false;
        }
        return true;
    }
};
```

684

```cpp
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        vector<int> ans;
        int n = edges.size();
        UnionSet uSet(n + 1);
        for (auto x : edges) {
            if (uSet.get(x[0]) == uSet.get(x[1])) ans = x;
            else uSet.merge(x[0], x[1]);
        }
        return ans;
    }
};
```

1319

```cpp
class Solution {
public:
    int makeConnected(int n, vector<vector<int>>& connections) {
        int m = connections.size();
        if (m + 1 < n) return -1;
        UnionSet uSet(n);
        for (auto x : connections) {
            uSet.merge(x[0], x[1]);
        }
        int ans = 0;
        for (int i = 0; i < n; i++) {
            if (uSet.get(i) == i) ans++;
        }
        return ans - 1;
    }
};
```

128

```cpp
class Solution {
public:
    unordered_map<int, int> m, ind;
    int longestConsecutive(vector<int>& nums) {
        int n = nums.size();
        UnionSet uSet(n);
        for (int i = 0; i < n; i++) if (m[nums[i]] == 0) m[nums[i]] = i + 1;
        for (auto x : nums) {
            if (ind[x]) continue;
            if (m[x + 1] >= 1 && m[x] < m[x + 1]) uSet.merge(m[x], m[x + 1]);
            if (m[x - 1] >= 1 && m[x] < m[x - 1]) uSet.merge(m[x], m[x - 1]);
            ind[x] += 1;
        }
        int ans = 0;
        for (int i = 1; i <= n; i++) ans = max(ans, uSet.cnt[i]);
        return ans;
    }
};
```

947

```cpp
class Solution {
public:
    unordered_map<int, int> x1, x2;
    int removeStones(vector<vector<int>>& stones) {
        int n = stones.size();
        UnionSet uSet(n);
        for (int i = 0; i < n; i++) {
            int l1 = stones[i][0], l2 = stones[i][1];
            if (x1[l1]) uSet.merge(x1[l1] - 1, i);
            else x1[l1] = i + 1;
            if (x2[l2]) uSet.merge(x2[l2] - 1, i);
            else x2[l2] = i + 1;
        }
        int ans = 0;
        for (int i = 0; i < n; i++) if (uSet.get(i) == i) ans++;
        return n - ans;
    }
};
```

1202

```cpp
class Solution {
public:
    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
        int n = s.size();
        string ans = s;
        UnionSet uSet(n);
        for (auto x : pairs) uSet.merge(x[0], x[1]);
        vector<vector<int>> v(n), v2;
        for (int i = 0; i < n; i++) {
            int fi = uSet.get(i);
            v[fi].push_back(i);
        }
        v2 = v;
        for (int i = 0; i < v2.size(); i++) {
            sort(v2[i].begin(), v2[i].end(), [&](int a, int b)->bool{return (s[a] - 'a') < (s[b] - 'a');});
        }
        for (int i = 0; i < v.size(); i++) {
            for (int j = 0; j < v[i].size(); j++) {
                ans[v[i][j]] = s[v2[i][j]];  
            }
        }
        return ans;
    }
};
```