---
title: Data Structures and Algorithms 3-2. Merge-Sort (1/1)
date: 2022-02-12T04:03:12.271Z
summary: "***Implementation, Leetcode- offer - 51, 23, 148, 1305, 327, 315, 53,
  1508, 04.08, 1302***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# Merge-Sort

## 1. Implementation

```cpp
#include <iostream>
using namespace std;
void merge_sort(int *arr, int l, int r) {
    if (l >= r) return ;
    int mid = (l + r) >> 1;
    merge_sort(arr, l, mid);
    merge_sort(arr, mid + 1, r);
    int p1 = l, p2 = mid + 1, *temp = new int[r - l + 1], k = 0;
    while (p1 <= mid || p2 <= r) {
        if (p2 > r || (p1 <= mid && arr[p1] <= arr[p2])) temp[k++] = arr[p1++];
        else temp[k++] = arr[p2++];
    }
    for (int i = l; i <= r; i++) arr[i] = temp[i - l];
    delete[] temp;
    return ;
}

int main() {
    int arr[8] = {3, 2, 4, 6, 1, 13, 5, 0};
    merge_sort(arr, 0, 7);
    for (auto x : arr) cout << x << " ";
    cout << endl;
    return 0;
}
```

## 2. Leetcode

***Leetcode- offer - 51, 23, 148, 1305, 327, 315, 53, 1508, 04.08, 1302***

offer - 51

```cpp
class Solution {
public:
    int merge_sort(vector<int>& nums, int l, int r) {
        if (l >= r) return 0;
        int mid = (l + r) >> 1, ans = 0, p1 = l, p2 = mid + 1, ind = 0;
        ans += merge_sort(nums, l, mid);
        ans += merge_sort(nums, mid + 1, r);
        int* arr = new int[r - l + 1];
        while (p1 <= mid || p2 <= r) {
            if (p2 > r || (p1 <= mid && nums[p1] <= nums[p2])) {
                arr[ind++] = nums[p1++];
                ans += p2 - mid - 1;
            }else arr[ind++] = nums[p2++];
        }
        for (int i = l; i <= r; i++) nums[i] = arr[i - l];
        delete[] arr;
        return ans;
    }

    int reversePairs(vector<int>& nums) {
        int n = nums.size();
        int ans = merge_sort(nums, 0, n - 1);
        return ans;
    }
};
```

23

```cpp
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> heap;
        ListNode tar(0);
        ListNode *root = &tar, *temp;
        int n = lists.size(), length, flag;
        for (int i = 0; i < n; i++) {
            if (lists[i] != nullptr) {
                heap.push(pair<int, int>(lists[i]->val, i));
            }
        }
        while (1) {
            flag = 0;
            for (int i = 0; i < n; i++) {
                if (lists[i] != nullptr) flag = 1;
            }
            if (!flag) break;
            int pos = heap.top().second;
            heap.pop();
            temp = lists[pos];
            root->next = temp;
            root = temp;
            lists[pos] = lists[pos]->next;
            if (lists[pos] != nullptr) heap.push(pair<int, int>(lists[pos]->val, pos));
        }
        root->next = nullptr;
        return tar.next;
    }
};
```

148

```cpp
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode *slow = head, *fast = head->next, *p1 = head, *p2, *ans, ret(0);
        ans = &ret;
        while (fast && fast->next) {
            slow = slow->next;
            fast = fast->next->next;
        }
        p2 = slow->next;
        slow->next = nullptr;
        ListNode* l = sortList(p1);
        ListNode* r = sortList(p2);
        while (l || r) {
            if (!r || (l && l->val <= r->val)) {
                ans->next = l;
                ans = l;
                l = l->next;
            } else {
                ans->next = r;
                ans = r;
                r = r->next;
            }
        }
        return ret.next;
    }
};
```

1305

```cpp
class Solution {
public:
    void GetTreeNode(TreeNode* root, vector<int>& node_cnt) {
        if (!root) return ;
        GetTreeNode(root->left, node_cnt);
        node_cnt.push_back(root->val);
        GetTreeNode(root->right, node_cnt);
        return ;
    }

    vector<int> getAllElements(TreeNode* root1, TreeNode* root2) {
        vector<int> l_node, r_node, ans;
        GetTreeNode(root1, l_node);
        GetTreeNode(root2, r_node);
        int m = l_node.size() - 1, n = r_node.size() - 1, ind1 = 0, ind2 = 0;
        while (ind1 <= m || ind2 <= n) {
            if (ind2 > n || (ind1 <= m && l_node[ind1] <= r_node[ind2])) {
                ans.push_back(l_node[ind1++]);
            } else {
                ans.push_back(r_node[ind2++]);
            }
        }
        return ans;
    }
};
```

327

```cpp
class Solution {
public:
    int merge_sort(vector<long long>& pre_cnt, int lower, int upper, int l, int r) {
        if (l >= r) return 0;
        int mid = (l + r) >> 1, ans = 0;
        int ind1 = l, ind2 = mid + 1, k1 = l, k2 = l, id = 0;
        long long a, b;
        vector<long long> temp(r - l + 1);
        ans += merge_sort(pre_cnt, lower, upper, l, mid);
        //cout << ans << " " << l << " " << r << endl;
        ans += merge_sort(pre_cnt, lower, upper, mid + 1, r);
        //cout << ans << " " << l << " " << r << endl;
        while (ind1 <= mid || ind2 <= r) {
            if (ind2 > r || (ind1 <= mid && pre_cnt[ind1] <= pre_cnt[ind2])) {
                temp[id++] = pre_cnt[ind1++];
            } else {
                a = (long long)pre_cnt[ind2] - upper, b = (long long)pre_cnt[ind2] - lower;
                while (k1 <= mid && pre_cnt[k1] < a) k1++;
                while (k2 <= mid && pre_cnt[k2] <= b) k2++;
                //if (k2 > 0) k2--;
                ans += k2 - k1;
                temp[id++] = pre_cnt[ind2++];
            }
        }
        for (int i = l; i <= r; i++) pre_cnt[i] = temp[i - l];
        //cout << ans << " " << l << " " << r << endl;
        return ans;

    }

    int countRangeSum(vector<int>& nums, int lower, int upper) {
        int n = nums.size();
        vector<long long> pre_cnt(n + 1);
        pre_cnt[0] = 0;
        for (int i = 1; i <= n; i++) pre_cnt[i] = pre_cnt[i - 1] + nums[i - 1];
        //for (auto x : pre_cnt) cout << x << " ";
        //cout << endl;
        return merge_sort(pre_cnt, lower, upper, 0, n);
    }
};
```

315

```cpp
class Solution {
public:
    void merge_sort(vector<int>& nums, int l, int r, vector<int>& ret, vector<int>& marks) {
        if (l >= r) return ;
        int mid = (l + r) >> 1;
        int ind1 = l, ind2 = mid + 1, id = 0;
        vector<int> temp(r - l + 1), tar(r - l + 1);
        merge_sort(nums, l, mid, ret, marks);
        merge_sort(nums, mid + 1, r, ret, marks);
        while (ind1 <= mid || ind2 <= r) {
            if (ind2 > r || (ind1 <= mid && nums[ind1] <= nums[ind2])) {
                ret[marks[ind1]] += ind2 - (mid + 1);
                tar[id] = marks[ind1];
                temp[id++] = nums[ind1++];
            } else {
                tar[id] = marks[ind2];
                temp[id++] = nums[ind2++];
            }
        }
        for (int i = l; i <= r ; i++) nums[i] = temp[i - l];
        for (int i = l; i <= r ; i++) marks[i] = tar[i - l];
        return ;
    }

    vector<int> countSmaller(vector<int>& nums) {
        int n = nums.size();
        vector<int> ret(n, 0), marks;
        for (int i = 0; i < n; i++) marks.push_back(i);
        merge_sort(nums, 0, n - 1, ret, marks);
        return ret;
    }
};
```

53

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size(), min_val = 0, ans;
        vector<int> arr(n + 1);
        arr[0] = 0, ans = nums[0];
        for (int i = 1; i <= n; i++) {
            arr[i] = arr[i - 1] + nums[i - 1];
            ans = max(ans, arr[i] - min_val);
            min_val = min(min_val, arr[i]);
        }
        return ans;
    }
};
```

1508

```cpp
class Solution {
public:
    int rangeSum(vector<int>& nums, int n, int left, int right) {
        vector<int> pre_num(n + 1), ans;
        int ret = 0, mod_cnt = 1e9 + 7;
        pre_num[0] = 0;
        for (int i = 1; i <= n; i++) pre_num[i] = pre_num[i - 1] + nums[i - 1];
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j <= n; j++) {
                ans.push_back(pre_num[j] - pre_num[i]);
            }
        }
        sort(ans.begin(), ans.end());
        for (int i = left - 1; i <= right - 1; i++) ret = (ret + ans[i]) % mod_cnt;
        return ret;
    }
};
```

04.08

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root) return nullptr;
        if (root->val == p->val || root->val == q->val) return root;
        TreeNode* l = lowestCommonAncestor(root->left, p, q);
        TreeNode* r = lowestCommonAncestor(root->right, p, q);
        if (l && r) return root;
        if (l) return l;
        return r;
    }
};
```

1302

```cpp
class Solution {
public:
    vector<int> ans;
    void dfs(TreeNode* root, vector<int>& ans, int level) {
        if (!root) return ;
        int n = ans.size();
        if (level == n) ans.push_back(0);
        ans[level] += root->val;
        dfs(root->left, ans, level + 1);
        dfs(root->right, ans, level + 1);
        return ;
    }

    int deepestLeavesSum(TreeNode* root) {
        dfs(root, ans, 0);
        int n = ans.size();
        return ans[n - 1];
    }
};
```