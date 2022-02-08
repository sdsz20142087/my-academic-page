---
title: Data Structures and Algorithms 3-1. Quick-Sort (2/2)
date: 2022-02-08T03:37:54.506Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# Quick-Sort

***Leetcode- offer - 21, 148, 75, 95, 394, 11, 239, 470***

offer - 21

```cpp
class Solution {
public:
    vector<int> exchange(vector<int>& nums) {
        int n = nums.size();
        int l = 0, r = n - 1;
        while (l < r) {
            while (l < r && nums[l] % 2 == 1) l++;
            while (l < r && nums[r] % 2 == 0) r--;
            if (l <= r) swap(nums[l++], nums[r--]);
        }
        return nums;
    }
};
```

148

```cpp
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if (!head) return nullptr;
        if (!head->next) return head;
        int base1 = head->val, base2 = head->val;
        double base;
        ListNode sval(0), bval(0);
        ListNode *s = &sval, *b = &bval, *root = head, *sub1, *sub2, *tmp = head;
        while (tmp) {
            base1 = min(base1, tmp->val);
            base2 = max(base2, tmp->val);
            tmp = tmp->next;
        }
        if (base1 == base2) return head;
        base = (base1 + base2) / 2.0;
        while (root) {
            if (root->val <= base) {
                s->next = root;
                s = s->next;
            }
            else {
                b->next = root;
                b = b->next;
            }
            root = root->next;
        }
        if (sval.next) s->next = nullptr;
        if (bval.next) b->next = nullptr;
        //cout << sval.next->next->val << endl;
        sub1 = sortList(sval.next);
        sub2 = sortList(bval.next);
        if (!sub1) return sub2;
        if (!sub2) return sub1;
        tmp = sub1;
        while (tmp->next) tmp = tmp->next;
        tmp->next = sub2;
        return sub1;
    }
};
```

75

```cpp
class Solution {
public:
    void three_partition(vector<int>& arr, int l, int r, int base) {
        if (l >= r) return ;
        int x = l - 1, y = r + 1;
        while (l < y) {
            if (arr[l] == base) l++;
            else if (arr[l] < base) {
                x++;
                swap(arr[x], arr[l]);
                l++;
            } else {
                y--;
                swap(arr[y], arr[l]);
            }
        }
        return ;
    }
    void sortColors(vector<int>& nums) {
        three_partition(nums, 0, nums.size() - 1, 1);
    }
};
```

95

```cpp
class Solution {
public:
    vector<TreeNode*> GetTree(int l, int r) {
        // if (l > r) return vector<TreeNode*>();
        vector<TreeNode*> ret;
        if (l > r) {
            ret.push_back(nullptr);
            return ret;
        }
        for (int i = l; i <= r; i++) {
            vector<TreeNode*> root_l = GetTree(l, i - 1);
            vector<TreeNode*> root_r = GetTree(i + 1, r);
            for (auto x : root_l) {
                for (auto y : root_r) {
                    TreeNode* root = new TreeNode(i, x, y);
                    ret.push_back(root);
                }
            }
        }
        return ret;
    }

    vector<TreeNode*> generateTrees(int n) {
        return GetTree(1, n);

    }
};
```

394

```cpp
class Solution {
public:
    string decodeString(string s) {
        stack<int> ops;
        stack<int> content;
        string ans = "";
        int val = 0;
        for (auto x : s) {
            char sig = x;
            if (sig >= '0' && sig <= '9') {
                val = val * 10 + (sig - '0');
            } else if (sig == '[') {
                ops.push(val);
                val = 0;
                content.push(sig);
            } else if (sig == ']'){
                string ret = "";
                while (content.top() != '[') {
                    ret += content.top();
                    content.pop();
                }
                content.pop();
                int cnt = ops.top();
                ops.pop();
                while (cnt--) {
                    int n = ret.size();
                    for (int j = n - 1; j >= 0; j--) content.push(ret[j]);
                }
            } else content.push(sig);
        }
        while (!content.empty()) {
            ans += content.top();
            content.pop();
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

11

```cpp
class Solution {
public:
    int maxArea(vector<int>& height) {
        int ans = 0, n = height.size();
        int l = 0, r = n - 1;
        while (l < r) {
            ans = max(ans, (r - l) * min(height[l], height[r]));
            if (height[l] <= height[r]) l++;
            else r--;
        }
        return ans;
    }
};
```

239

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> ans;
        deque<int> q;
        for (int i = 0; i < nums.size(); i++) {
            if (i - q.front() == k) q.pop_front();
            while (!q.empty() && nums[q.back()] <= nums[i]) q.pop_back();
            q.push_back(i);
            if (i >= k - 1) ans.push_back(nums[q.front()]);
        }
        return ans;
    }
};
```

470

```cpp
class Solution {
public:
    int rand10() {
        while (1) {
            int m = rand7(), n = rand7();
            int num = (m - 1) * 7 + n;
            if (num <= 10) return num % 10 + 1;
        }
        return 0;
    }
};
```