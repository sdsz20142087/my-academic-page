---
title: Data Structures and Algorithms 2-1. Binary Tree (2/2)
date: 2022-01-28T16:22:54.027Z
summary: "***Leetcode- 144, 589, 226, offer - 32, 110, 112, 105, 222, offer -
  54, offer - 26, 662, 968***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# Binary-Tree

***Leetcode- 144, 589, 226, offer - 32, 110, 112, 105, 222, offer - 54, offer - 26, 662, 968***

144

```cpp
class Solution {
public:
    void PreOrder(vector<int>& node, TreeNode* root) {
        if (!root) return ;
        node.push_back(root->val);
        PreOrder(node, root->left);
        PreOrder(node, root->right);
        return ;
    }


    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> node;
        PreOrder(node, root);
        return node;
    }
};
```

589 - iteration

```cpp
class Solution {
public:
    vector<int> preorder(Node* root) {
        stack<Node*> sta;
        Node* tmp;
        vector<int> ans;
        if(!root) return ans;
        sta.push(root);
        while (!sta.empty()) {
            tmp = sta.top();
            sta.pop();
            ans.push_back(tmp->val);
            for (int i = tmp->children.size() - 1; i >= 0; i--) {
                sta.push(tmp->children[i]);
            }
        }
        return ans;
    }
};
```

589 - recursion

```cpp
class Solution {
public:
    void NPreOrder(vector<int>& ans, Node* root) {
        if (!root) return ;
        ans.push_back(root->val);
        for (int i = 0; i < root->children.size(); i++) {
            NPreOrder(ans, root->children[i]);
        }
        return ;
    }

    vector<int> preorder(Node* root) {
        vector<int> ans;
        NPreOrder(ans, root);
        return ans;
    }
};
```

226

```cpp
class Solution {
public:
    TreeNode* ReverseTree(TreeNode* root) {
        if (!root) return nullptr;
        TreeNode* tmp = root->left;
        root->left = ReverseTree(root->right);
        root->right = ReverseTree(tmp);
        return root;
    }
    TreeNode* invertTree(TreeNode* root) {
        root = ReverseTree(root);
        return root;
    }
};
```

offer - 32

```cpp
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> v;
        if (!root) return v;
        queue<TreeNode*> q;
        q.push(root);
        TreeNode* tmp;
        while (!q.empty()) {
            int n = q.size();
            vector<int> ret;
            for (int i = 0; i < n; i++) {
                tmp = q.front();
                ret.push_back(tmp->val);
                if (tmp->left) q.push(tmp->left);
                if (tmp->right) q.push(tmp->right);
                q.pop();
            }
            v.push_back(ret);

        }
        return v;
    }
};
```

110

```cpp
class Solution {
public:
    int TreeHeight(TreeNode* root) {
        if (!root) return 0;
        int l = TreeHeight(root->left);
        int r = TreeHeight(root->right);
        if (l == -1 || r == -1) return -1;
        if (abs(l - r) > 1) return -1;
        return max(l, r) + 1;
    }

    bool isBalanced(TreeNode* root) {
        return TreeHeight(root) >= 0;
    }
};
```

112

```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (!root) return false;
        if (!root->left && !root->right) return targetSum == root->val;
        if (root->left && hasPathSum(root->left, targetSum - root->val)) return true;
        if (root->right && hasPathSum(root->right, targetSum - root->val)) return true;
        return false;
    }
};
```

105

```cpp
class Solution {
public:
    unordered_map<int, int> m;
    TreeNode* Part_Build(vector<int>& preorder, vector<int>& inorder, int l1, int r1, int l2, int r2) {
        if (l1 > r1) return nullptr;
        TreeNode* root = new TreeNode(preorder[l1]);
        int pos =  m[preorder[l1]], n = pos - l2;
        root->left = Part_Build(preorder, inorder, l1 + 1, l1 + n, l2, pos - 1);
        root->right = Part_Build(preorder, inorder,l1 + n + 1, r1, pos + 1, r2);
        return root;
    }

    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        for (int i = 0; i < inorder.size(); i++) m[inorder[i]] = i;
        return Part_Build(preorder, inorder, 0, preorder.size() - 1, 0, inorder.size() - 1);
    }
};
```

222

```cpp
class Solution {
public:
    int countNodes(TreeNode* root) {
        if (!root) return 0;
        return countNodes(root->left) + countNodes(root->right) + 1;
    }
};
```

offer - 54

```cpp
class Solution {
public:
    int Getval(TreeNode* root) {
        if (!root) return 0;
        return Getval(root->left) + Getval(root->right) + 1;
    }

    int kthLargest(TreeNode* root, int k) {
        int cnt_r = Getval(root->right);
        if (k <= cnt_r) return kthLargest(root->right, k);
        if (k == cnt_r + 1) return root->val;
        return kthLargest(root->left, k - cnt_r - 1);
    }
};
```

offer - 26

```cpp
class Solution {
public:
    bool isSame(TreeNode* root, TreeNode* t) {
        if (t && !root) return false;
        if (!t) return true;
        return root->val == t->val && isSame(root->left, t->left) && isSame(root->right, t->right);
    }

    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (!B || !A) return false;
        if (isSubStructure(A->left, B) || isSubStructure(A->right, B)) return true;
        if (A->val != B->val) return false;
        if (isSame(A, B)) return true;
        return false;
    }
};
```

662

```cpp
class Solution {
public:
    struct PII{
        TreeNode* p;
        int idx;
    };
    int widthOfBinaryTree(TreeNode* root) {
        queue<PII> que;
        que.push({root, 0});
        int ans = 0;
        while (!que.empty()) {
            int n = que.size();
            int start = que.front().idx, final = -1, max_length = -1;
            for (int i = 0; i < n; i++) {
                PII tmp = que.front();
                final = tmp.idx;
                que.pop();
                if ((tmp.p)->left) que.push({(tmp.p)->left, 2 * (final - start)});
                if ((tmp.p)->right) que.push({(tmp.p)->right, 2 * (final - start) + 1});
            }
            ans = max(ans, final - start + 1);
        }
        return ans;
    }
};
```

968

```cpp
class Solution {
public:
    void getDP(TreeNode* root, int dp[2][2]) {
        if (!root) {
            dp[0][0] = 0;
            dp[1][0] = 0;
            dp[0][1] = 100000;
            dp[1][1] = 100000;
            return ;
        }
        if (root->left == nullptr && root->right == nullptr) {
            dp[0][0] = 100000;
            dp[1][0] = 0;
            dp[0][1] = 1;
            dp[1][1] = 1;
            return ;
        }
        int l[2][2], r[2][2];
        getDP(root->left, l);
        getDP(root->right, r);
        dp[0][0] = min(l[0][1] + r[0][0], min(l[0][0] + r[0][1], l[0][1] + r[0][1]));
        dp[1][0] = min(dp[0][0], l[0][0] + r[0][0]);
        dp[0][1] = min(min(l[1][0] + r[1][0], l[1][0] + r[1][1]), min(l[1][1] + r[1][0], l[1][1] + r[1][1])) + 1;
        dp[1][1] = dp[0][1];
        return ;

    }

    int minCameraCover(TreeNode* root) {
        int dp[2][2];
        getDP(root, dp);
        return min(dp[0][0], dp[0][1]);
    }
};
```