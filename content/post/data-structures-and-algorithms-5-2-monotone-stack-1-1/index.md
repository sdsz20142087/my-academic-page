---
title: Data Structures and Algorithms 5-2. Monotone-Stack (1/1)
date: 2022-03-07T03:14:46.043Z
summary: "***Leetcode- 155, 503, 901, 739, 84, 1856, 907, 496, 456, 42***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# Monotone-Stack

## 1. Implementation

```cpp
#include <iostream>
#include <cstdio>
#include <vector>
#include <stack>
using namespace std;

void output(vector<int>& arr) {
    for (auto x : arr) printf("%5d", x);
    printf("\n");
}

int main() {
    int n;
    cin >> n;
    vector<int> ind(n), arr(n), pre(n), next(n);
    stack<int> s;
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
        ind[i] = i;
    }
    for (int i = 0; i < n; i++) {
        while (s.size() && arr[i] < arr[s.top()]) {
            next[s.top()] = i;
            s.pop();
        }
        if (s.size() == 0) pre[i] = -1;
        else pre[i] = s.top();
        s.push(i);
    }
    while (s.size()) {
        next[s.top()] = n;
        s.pop();
    }
    output(ind);
    output(arr);
    output(pre);
    output(next);

    return 0;
}
```

## 2. Leetcode

***Leetcode- 155, 503, 901, 739, 84, 1856, 907, 496, 456, 42***

155

```cpp
class MinStack {
public:
    stack<int> s, min_s;
    MinStack() {

    }

    void push(int val) {
        s.push(val);
        if (min_s.size() == 0 || min_s.top() >= val) min_s.push(val);
        return ;
    }

    void pop() {
        int temp = s.top();
        s.pop();
        if (temp == min_s.top())  min_s.pop();
        return ;
    }

    int top() {
        return s.top();
    }

    int getMin() {
        return min_s.top();
    }
};
```

503

```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        vector<int> new_nums, next(2 * nums.size(), -1), ans;
        stack<int> s;
        for (int i = 0; i < 2; i++) {
            for (int j = 0; j < nums.size(); j++) new_nums.push_back(nums[j]);
        }
        for (int i = 0; i < new_nums.size(); i++) {
            while (s.size() && new_nums[i] > new_nums[s.top()]) {
                next[s.top()] = i;
                s.pop();
            }
            s.push(i);
        }
        for (int i = 0; i < nums.size(); i++) {
            if (next[i] == -1) ans.push_back(-1);
            else ans.push_back(new_nums[next[i]]);
        }
        return ans;
    }
};
```

901

```cpp
class StockSpanner {
public:
    stack<int> s;
    vector<int> data;
    int cnt;
    StockSpanner() : cnt(0) {}

    int next(int price) {
        while (s.size() && price >= data[s.top()]) s.pop();
        int ans = cnt - (s.size() ? s.top() : -1);
        s.push(cnt);
        data.push_back(price);
        cnt++;
        return ans;
    }
};
```

739

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        vector<int> next(n), ans(n);
        stack<int> s;
        for (int i = 0; i < n; i++) {
            next[i] = i;
            while (s.size() && temperatures[i] > temperatures[s.top()]) {
                next[s.top()] = i;
                s.pop();
            }
            s.push(i);
        }
        for (int i = 0; i < n; i++) {
            ans[i] = next[i] - i;
        }
        return ans;
    }
};
```

84

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        stack<int> s;
        int n = heights.size(), ans = 0;
        vector<int> pre(n, -1), next(n, n);
        for (int i = 0; i < n; i++) {
            while (s.size() && heights[i] <= heights[s.top()]) {
                next[s.top()] = i;
                s.pop();
            }
            if (s.size()) pre[i] = s.top();
            s.push(i);
        }
        for (int i = 0; i < n; i++) {
            ans = max(ans, heights[i] * (next[i] - pre[i] - 1));
        }
        return ans;
    }
};
```

1856

```cpp
class Solution {
public:
    int maxSumMinProduct(vector<int>& nums) {
        long long ans = 0, temp;
        int n = nums.size(), mod_num = 1e9 + 7;
        stack<long long> s;
        vector<long long> pre_nums(n + 1, 0), prev(n, -1), next(n, n);
        for (int i = 1; i <= n; i++) pre_nums[i] = pre_nums[i - 1] + nums[i - 1];
        for (int i = 0; i < n ; i++) {
            while (s.size() && nums[i] < nums[s.top()]) {
                next[s.top()] = i;
                s.pop();
            }
            if (s.size()) prev[i] = s.top();
            s.push(i);
        }
        for (int i = 0; i < n; i++) {
            temp = nums[i] * (pre_nums[next[i]] - pre_nums[prev[i] + 1]);
            ans = max(ans, temp);
        }
        return ans % mod_num;
    }
};
```

907

```cpp
class Solution {
public:
    int sumSubarrayMins(vector<int>& arr) {
        stack<int> s;
        int n = arr.size(), mode_num = 1e9 + 7;
        long long ans = 0;
        vector<int> pre(n, -1), next(n, n);
        for (int i = 0; i < n; i++) {
            while (s.size() && arr[i] < arr[s.top()]) {
                next[s.top()] = i;
                s.pop();
            }
            if (s.size()) pre[i] = s.top();
            s.push(i);
        }
        for (int i = 0; i < n; i++) {
            ans += (long long)arr[i] * (i - pre[i]) * (next[i] - i);
            ans %= mode_num;
        }
        return ans % mode_num;
    }
};
```

496

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        int n = nums2.size(), m = nums1.size();
        vector<int> ans(m), next(10005, -1);
        stack<int> s;
        for (int i = 0; i < n; i++) {
            while (s.size() && nums2[i] > s.top()) {
                next[s.top()] = nums2[i];
                s.pop();
            }
            s.push(nums2[i]);
        }
        for (int i = 0; i < m; i++) {
            ans[i] = next[nums1[i]];
        }
        return ans;
    }
};
```

456

```cpp
class Solution {
public:
    bool find132pattern(vector<int>& nums) {
        int n = nums.size();
        vector<int> prev(n, -1), next(n, n);
        deque<int> q;
        stack<int> s;
        for (int i = 0; i < n; i++) {
            while (q.size() && nums[i] <= nums[q.back()]) q.pop_back();
            if (q.size()) prev[i] = q.front();
            q.push_back(i);
        }
        for (int i = 0; i < n; i++) {
            while (s.size() && nums[i] >= nums[s.top()]) {
                next[s.top()] = i;
                s.pop();
            }
            s.push(i);
        }
        for (int i = 0; i < n; i++) {
            int ind = next[i];
            q.clear();
            for (int j = i + 1; j < ind; j++) {
                while (q.size() && nums[j] > nums[q.back()]) q.pop_back();
                q.push_back(j);
            }
            if (prev[i] != -1 && q.size() && nums[q.front()] > nums[prev[i]]) return true;
        }
        return false;
    }
};
```

42

```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int n = height.size(), ans = 0, base, a, b;
        stack<int> s;
        for (int i = 0; i < n; i++) {
            while (s.size() && height[i] > height[s.top()]) {
                base = s.top();
                s.pop();
                if (s.size() == 0) continue;
                a = height[i] - height[base];
                b = height[s.top()] - height[base];
                ans += (i - s.top() - 1) * min(a, b);
            }
            s.push(i);
        }
        return ans;
    }
};
```