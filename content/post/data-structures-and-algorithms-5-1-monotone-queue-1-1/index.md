---
title: Data Structures and Algorithms 5-1. Monotone-Queue (1/1)
date: 2022-03-02T03:47:12.767Z
summary: "***Leetcode- 239, offer - 59, 862, 1438, 513, 135, 365, 45, 93, 43***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## 1. Introduction

***This data structure is suitable for solving RMQ and interval-maximum problems.***

## 2. Leetcode

***Leetcode- 239, offer - 59, 862, 1438, 513, 135, 365, 45, 93, 43***

239

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        deque<int> q;
        vector<int> ans;
        for (int i = 0; i < nums.size(); i++) {
            while (q.size() && nums[q.back()] < nums[i]) q.pop_back();
            q.push_back(i);
            if (i + 1 < k) continue;
            if (i - q.front() == k) q.pop_front();
            ans.push_back(nums[q.front()]);
        }
        return ans;
    }
};
```

offer - 59

```cpp
class MaxQueue {
public:
    MaxQueue() {}
    deque<int> arr, dq;
    int max_value() {
        if (!dq.size()) return -1;
        return dq.front();
    }

    void push_back(int value) {
        arr.push_back(value);
        while (dq.size() && dq.back() < value) dq.pop_back();
        dq.push_back(value);
        return ;
    }

    int pop_front() {
        if (arr.size() == 0) return -1;
        int temp = arr.front();
        if (arr.front() == dq.front()) dq.pop_front();
        arr.pop_front();
        return temp;
    }
};
```

862

```cpp
class Solution {
public:
    int shortestSubarray(vector<int>& nums, int k) {
        int n = nums.size(), ans = INT32_MAX, ind = 0, flag;
        vector<long long> pre_nums(n + 1);
        deque<int> dq;
        pre_nums[0] = 0;
        for (int i = 0; i < n; i++) pre_nums[i + 1] = pre_nums[i] + nums[i];
        for (int i = 0; i < n + 1; i++) {
            while (!dq.empty() && pre_nums[dq.back()] > pre_nums[i]) dq.pop_back();
            while (dq.size()) {
                if (pre_nums[i] - pre_nums[dq.front()] >= k) {
                    ans = min(ans, i - dq.front());
                    dq.pop_front();
                    continue;
                }
                break;
            }
            dq.push_back(i);
        }
        return ans == INT32_MAX ? -1 : ans;
    }
};
```

1438

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums, int limit) {
        deque<int> max_q, min_q;
        int ans = 0, ind_max, ind_min, left = 0;
        for (int i = 0; i < nums.size(); i++) {
            while (max_q.size() && nums[max_q.back()] > nums[i]) max_q.pop_back();
            max_q.push_back(i);
            while (min_q.size() && nums[min_q.back()] < nums[i]) min_q.pop_back();
            min_q.push_back(i);
            while (max_q.size() && min_q.size() && nums[min_q.front()] - nums[max_q.front()] > limit) {
                if (nums[left] == nums[min_q.front()]) min_q.pop_front();
                if (nums[left] == nums[max_q.front()]) max_q.pop_front();
                left++;
            }
            ans = max(ans, i - left + 1);
        }
        return ans;
    }
};
```

513

```cpp
class Solution {
public:
    int level = -1, ans = 0;
    void dfs(TreeNode* root, int pos) {
        if (!root) return ;
        if (pos > level) {
            ans = root->val;
            level = pos;
        }
        dfs(root->left, pos + 1);
        dfs(root->right, pos + 1);
        return ;
    }

    int findBottomLeftValue(TreeNode* root) {
        dfs(root, 0);
        return ans;
    }
};
```

135

```cpp
class Solution {
public:
    int candy(vector<int>& ratings) {
        int n = ratings.size(), ans = 0;
        vector<int> l_val(n), r_val(n);
        l_val[0] = r_val[n - 1] = 1;
        for (int i = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1]) l_val[i] = l_val[i - 1] + 1;
            else l_val[i] = 1;
        }
        for (int i = n - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) r_val[i] = r_val[i + 1] + 1;
             else r_val[i] = 1;
        }
        for (int i = 0; i < n; i++) {
            ans += max(l_val[i], r_val[i]);
        }
        return ans;
    }
};
```

365

```cpp
class Solution {
public:
    typedef pair<long, long> PII;
    struct HASH{
        long long operator()(const PII& a) const {
            return ((long long) a.first << 31) + a.second;
        }
    };
    unordered_map<PII, int, HASH> S_et;
    PII bfs(int k, int jug1Capacity, int jug2Capacity, int targetCapacity, int lv, int rv) {
        switch (k) {
            case 0: return PII(0, rv);
            case 1: return PII(jug1Capacity, rv);
            case 2: return PII(lv, 0);
            case 3: return PII(lv, jug2Capacity);
            case 4: {
                int mov = min(lv + rv, jug2Capacity);
                return PII(lv - (mov - rv), mov);
            }
            case 5: {
                int mov = min(lv + rv, jug1Capacity);
                return PII(mov, rv - (mov - lv));
            }
        }
        return PII(0, 0);
    }

    bool canMeasureWater(int jug1Capacity, int jug2Capacity, int targetCapacity) {
        queue<PII> q;
        q.push(PII{0, 0});
        while (!q.empty()) {
            PII temp = q.front();
            if (temp.first + temp.second == targetCapacity) return true;
            for (int i = 0; i < 6; i++) {
                PII node = bfs(i, jug1Capacity, jug2Capacity, targetCapacity, temp.first, temp.second);
                if (S_et.find(node) != S_et.end()) continue;
                q.push(node);
                S_et[node] = 1;
            }
            q.pop();
        }

        return false;
    }
};
```

45

```cpp
class Solution {
public:
    int ans;
    void GetNext(vector<int>& nums, int ind, int jump) {
        int n = nums.size(), next_pos, max_pos = 0;
        if (ind == n - 1) {
            ans = jump;
            return ;
        }
        if (ind + nums[ind] >= n - 1) {
            GetNext(nums, n - 1, jump + 1);
            return ;
        }
        for (int i = 1; i <= nums[ind]; i++) {
            if (i + ind + nums[ind + i] > max_pos) {
                max_pos = i + ind + nums[ind + i];
                next_pos = ind + i;
            }
        }
        GetNext(nums, next_pos, jump + 1);
        return ;
    }

    int jump(vector<int>& nums) {
        GetNext(nums, 0, 0);
        return ans;
    }
};
```

93

```cpp
class Solution {
public:
    vector<string> ans;
    void dfs(string s, int ind, int dot_num, int val) {
        if (dot_num < 3 && ind == s.size() - 1) return ;
        if (dot_num == 3) {
            if (ind != s.size() && (s[ind] != '0' || ind == s.size() - 1)) {
                int temp = 0;
                for (int i = ind; i < s.size(); i++) {
                    temp = temp * 10 + s[i] - '0';
                    if (temp > 255) return ;
                }
                ans.push_back(s);
            }
            return ;
        }
        val = val * 10 + s[ind] - '0';
        if (val > 255) return ;
        s.insert(ind + 1, 1, '.');
        dfs(s, ind + 2, dot_num + 1, 0);
        s.erase(ind + 1, 1);
        if (val == 0) return ;
        dfs(s, ind + 1, dot_num, val);
        return ;
    }

    vector<string> restoreIpAddresses(string s) {
        dfs(s, 0, 0, 0);
        return ans;
    }
};
```

43

```cpp
class Solution {
public:
    string multiply(string num1, string num2) {
        int m = num1.size(), n = num2.size(), ind = 0;
        vector<int> n1(m, 0), n2(n, 0), v(m + n + 1, 0);
        for (int i = m - 1, j = 0; i >= 0; i--, j++) n1[j] = num1[i] - '0';
        for (int i = n - 1, j = 0; i >= 0; i--, j++) n2[j] = num2[i] - '0';
        for (int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                int temp = n1[i] * n2[j];
                v[i + j] += temp;
                v[i + j + 1] += v[i + j] / 10;
                v[i + j] = v[i + j] % 10;
                if (v[i + j + 1] != 0) ind = max(ind, i + j + 1);
                else if (v[i + j] != 0) ind = max(ind, i + j);
            }
        }
        string ans = "";
        for (int i = ind; i >= 0; i--) {
            ans += v[i] + '0';
        }
        return ans;
    }
};
```