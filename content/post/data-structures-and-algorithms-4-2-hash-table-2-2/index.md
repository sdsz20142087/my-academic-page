---
title: Data Structures and Algorithms 4-2. Hash-Table (2/2)
date: 2022-02-24T03:20:54.227Z
summary: "***Leetcode- 16.25, 187, 318, 240, 979, 430, 863***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
***Leetcode- 16.25, 187, 318, 240, 979, 430, 863***

16.25

```cpp
class Node {
public:
    Node(int key = -1, int value = -1, Node* next = nullptr, Node* prev = nullptr) : 
    key(key), value(value), next(next), prev(prev) {}
    virtual~Node() {}
    int key, value;
    Node *next, *prev;
    Node* remove_this() {
        this->prev->next = this->next;
        this->next->prev = this->prev;
        this->next = this->prev = nullptr;
        return this;
    }
    void insert_prev(Node *node) {
        node->next = this;
        node->prev = this->prev;
        node->next->prev = node;
        node->prev->next = prev;
        return ;
    }
};

class LRUCache {
public:
    unordered_map<int, Node*> data;
    Node head, tail;
    int capacity;
    LRUCache(int capacity) : capacity(capacity) {
        head.next = &tail;
        tail.prev = &head;
    }

    int get(int key) {
        if (data.find(key) == data.end()) return -1;
        data[key]->remove_this();
        tail.insert_prev(data[key]);
        return data[key]->value;
    }

    void put(int key, int value) {
        if (data.find(key) != data.end()) {
            data[key]->value = value;
            data[key]->remove_this();
        } else {
            data[key] = new Node(key, value);
        }
        tail.insert_prev(data[key]);
        if (data.size() > capacity) {
            Node *tmp = head.next->remove_this();
            data.erase(data.find(tmp->key));
            delete(tmp);
        }
        return ;
    }
};
```

187

```cpp
class Solution {
public:
    unordered_map<string, int> h, ind;
    vector<string> findRepeatedDnaSequences(string s) {
        int n = s.size();
        vector<string> ans;
        for (int i = 0; i <= n - 10; i++) {
            string tmp = s.substr(i, 10);
            if (h.find(tmp) != h.end() && ind.find(tmp) == ind.end()) {
                ans.push_back(tmp);
                ind[tmp] = 1;
            } else {
                h[tmp] = 1;
            }
        }
        return ans;
    }
};
```

318

```cpp
class Solution {
public:
    int maxProduct(vector<string>& words) {
        int n = words.size(), a, b, ans = 0;
        vector<int> mark(n);
        for (int i = 0; i < n; i++) {
            for (auto x : words[i]) {
                mark[i] |= (1 << (x - 'a'));
            }
        }
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                a = words[i].size(), b = words[j].size();
                if (mark[i] & mark[j]) continue;
                ans = max(ans, a * b);
            }
        }
        return ans;
    }
};
```

240

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        int x_ind = 0, r_ind = n - 1;
        while (x_ind < m && r_ind >= 0) {
            if (matrix[x_ind][r_ind] == target) return true;
            if (matrix[x_ind][r_ind] < target) x_ind++;
            else r_ind--;
        }
        return false;
    }
};
```

979

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    void GetVal(TreeNode* root, int& coin, int& val) {
        if (!root) return ;
        coin++;
        val += root->val;
        GetVal(root->left, coin, val);
        GetVal(root->right, coin, val);
    }

    int distributeCoins(TreeNode* root) {
        if (!root) return 0;
        int ans = 0, coin_cnt = 0, val_cnt = 0;
        ans += distributeCoins(root->left);
        ans += distributeCoins(root->right);
        GetVal(root->left, coin_cnt, val_cnt);
        ans += abs(coin_cnt - val_cnt);
        coin_cnt = 0, val_cnt = 0;
        GetVal(root->right, coin_cnt, val_cnt);
        ans += abs(coin_cnt - val_cnt);
        return ans;
    }
};
```

430

```cpp
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* prev;
    Node* next;
    Node* child;
};
*/

class Solution {
public:
    Node* flatten(Node* head) {
        if (!head) return nullptr;
        Node *p = head, *temp, *tail;
        while (p) {
            if (p->child) {
                temp = flatten(p->child);
                p->child = nullptr;
                tail = temp;
                while (tail->next) tail = tail->next;
                tail->next = p->next;
                if (p->next) p->next->prev = tail;
                p->next = temp;
                temp->prev = p;
                p = tail->next;
            } else {
                p = p->next;
            }
        }
        return head;
    }
};
```

863

```cpp
class Solution {
public:
    void dfs(TreeNode* target, int level, vector<int>& ret) {
        if (!target) return;
        if (level < 0) return ;
        if (level == 0) {
            ret.push_back(target->val);
        }
        dfs(target->left, level - 1, ret);
        dfs(target->right, level - 1, ret);
        return ;
    }

    TreeNode* GetResult(TreeNode* root, TreeNode* target, int& k, vector<int>& ret) {
        if (!root) return nullptr;
        if (root == target) {
            dfs(target, k, ret);
            return root;
        }
        if (GetResult(root->left, target, k, ret)) {
            if (k == 1) {
                ret.push_back(root->val);
            } else if (k > 1) {
                dfs(root->right, k - 2, ret);
            }
            k--;
            return target;
        }
        else if (GetResult(root->right, target, k, ret)){
            if (k == 1) {
                ret.push_back(root->val);
            } else if (k > 1){
                dfs(root->left, k - 2, ret);
            }
            k--;
            return target;
        }
        return nullptr;
    }

    vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
        vector<int> ret;
        GetResult(root, target, k, ret);
        return ret;
    }
};
```