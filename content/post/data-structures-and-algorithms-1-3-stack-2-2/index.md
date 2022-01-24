---
title: Data Structures and Algorithms 1-3. Stack (2/2)
date: 2022-01-24T08:10:23.636Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# Stack

***Leetcode- 03.04, 682, 844, 946, 20, 1021, 1249, 145, 331, 227, 636, 1124***

03.04

```cpp
class MyQueue {
public:
    /** Initialize your data structure here. */
    stack<int> s1, s2;
    int val;
    MyQueue() : val(0){}

    /** Push element x to the back of queue. */
    void push(int x) {
        s2.push(x);
    }

    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        if (!s1.empty()) {
            val = s1.top();
            s1.pop();
            return val;
        } 
        transfer();
        val = s1.top();
        s1.pop();
        return val;
    }

    /** Get the front element. */
    int peek() {
        if (!s1.empty()) return s1.top();
        transfer();
        return s1.top();
    }

    /** Returns whether the queue is empty. */
    bool empty() {
        return s1.empty() && s2.empty();
    }

    void transfer() {
        while (!s2.empty()) {
            s1.push(s2.top());
            s2.pop();
        }
    }

};
```

682

```cpp
class Solution {
public:
    int calPoints(vector<string>& ops) {
        stack<int> s;
        for (auto x : ops) {
            if (x == "C") s.pop();
            else if (x == "D") s.push(2 * s.top());
            else if (x == "+"){
                int a = s.top();
                s.pop();
                int b = s.top();
                s.push(a);
                s.push(a + b);
            }
            else s.push(atoi(x.c_str()));
        }
        int ret = 0;
        while (!s.empty()) {
            ret += s.top();
            s.pop();
        }
        return ret;
    }
};
```

844

```cpp
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        if (s == t) return true;
        stack<char> s1, s2;
        for (auto x : s) {
            if (x == '#') {
                if (!s1.empty()) s1.pop();
            } else s1.push(x);
        }
        for (auto x : t) {
            if (x == '#') {
                if (!s2.empty()) s2.pop();
            } else s2.push(x);
        }
        char r1, r2;
        while (!s1.empty() && !s2.empty()) {
            r1 = s1.top();
            r2 = s2.top();
            s1.pop();
            s2.pop();
            if (r1 != r2) return false;
        }
        return s1.empty() && s2.empty();
    }
};
```

946

```cpp
class Solution {
public:
    bool validateStackSequences(vector<int>& pushed, vector<int>& popped) {
        stack<int> s;
        for (int i = 0, j = 0; i < popped.size(); i++) {
            while (j < pushed.size() && (s.empty() || s.top() != popped[i])) {
                s.push(pushed[j]);
                j++;
            }
            if (s.top() != popped[i]) return false;
            s.pop();
        }
        return true;
    }
};
```

20

```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> sta;
        for (auto x : s) {
            if (x == '(' || x == '[' || x == '{') {
                sta.push(x);
            } else if (x == ')') {
                if (!sta.empty() && sta.top() == '(') sta.pop();
                else return false;
            } else if (x == ']') {
                if (!sta.empty() && sta.top() == '[') sta.pop();
                else return false;
            } else {
                if (!sta.empty() && sta.top() == '{') sta.pop();
                else return false;
            }
        }
        return sta.empty();
    }
};
```

1021

```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        stack<char> sta;
        string ret = "";
        int ind = 0;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(') sta.push(s[i]);
            else sta.pop();
            if (sta.empty()) {
                ret += s.substr(ind + 1, i - ind - 1);
                ind = i + 1;
            }
        }
        return ret;
    }
};
```

1249

```cpp
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        vector<int> v;
        stack<int> sta;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] >= 'a' && s[i] <= 'z') continue;
            if (s[i] == '(') {
                sta.push(i);
                continue;
            }
            if (sta.empty()) v.push_back(i);
            else sta.pop();
        }
        while (!sta.empty()) {
            v.push_back(sta.top());
            sta.pop();
        }
        sort(v.begin(), v.end(), greater<int>());
        for (auto x : v) {
            //cout << x << endl;
            s.erase(x, 1);
        }
        return s;
    }
};
```

145

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
    vector<int> postorderTraversal(TreeNode* root) {
        vector<int> ans;
        if (!root) return ans;
        stack<TreeNode*> s1;
        stack<int> s2;
        s1.push(root);
        s2.push(0);
        while (!s1.empty()) {
            int status = s2.top();
            s2.pop();
            switch (status) {
                case 0: {
                    s2.push(1);
                    if (s1.top()->left) {
                        s1.push(s1.top()->left);
                        s2.push(0);
                    }
                } break;
                case 1: {
                    s2.push(2);
                    if (s1.top()->right) {
                        s1.push(s1.top()->right);
                        s2.push(0);
                    }
                } break;
                case 2: {
                    ans.push_back(s1.top()->val);
                    s1.pop();
                }
            }
        }
        return ans;
    }
};
```

331

```cpp
class Solution {
public:
    bool isValidSerialization(string preorder) {
        stack<string> s;
        string a1, a2;
        for (int i = 0, j = 0; i < preorder.size(); i = j + 1) {
            j = i;
            while (j < preorder.size() && preorder[j] != ',') j++;
            string tar = preorder.substr(i, j - i);
            s.push(tar);
            while (s.size() >= 3) {
                a1 = s.top();
                s.pop();
                a2 = s.top();
                s.pop();
                if (a1 != "#" || a2 != "#") {
                    s.push(a2);
                    s.push(a1);
                    break;
                } else {
                    if (s.top() == "#") return false;
                    s.pop();
                    s.push("#");
                }
            }
        }
        return s.size() == 1 && s.top() == "#";
    }
};
```

227

```cpp
class Solution {
public:
    int level(char c) {
        switch (c) {
            case '@': return -1;
            case '+':
            case '-': return 1;
            case '*':
            case '/': return 2;
        }
        return 0;
    }

    int calc(char op, int l, int r) {
        switch (op) {
            case '+' : return l + r; break;
            case '-' : return l - r; break;
            case '*' : return l * r; break;
            case '/' : return l / r; break;
        }
        return 0;
    }

    int calculate(string s) {
        stack<int> nums;
        stack<char> ops;
        char cur_op;
        int a, b;
        s += "@";
        for (int i = 0, cnt = 0; i < s.size(); i++) {
            if (s[i] == ' ') continue;
            if (level(s[i]) == 0) {
                cnt = cnt * 10 + (s[i] - '0');
                continue;
            }
            nums.push(cnt);
            cnt = 0;
            while (!ops.empty() && level(s[i]) <= level(ops.top())) {
                cur_op = ops.top();
                ops.pop();
                a = nums.top();
                nums.pop();
                b = nums.top();
                nums.pop();
                nums.push(calc(cur_op, b, a));
            }
            ops.push(s[i]);
        }
        return nums.top();
    }
};
```

636

```cpp
class Solution {
public:
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        vector<int> ans(n);
        stack<int> VID;
        for (int i = 0, pre = 0; i < logs.size(); i++) {
            int pos1 = logs[i].find_first_of(":");
            int pos2 = logs[i].find_last_of(":");
            string id_str = logs[i].substr(0, pos1);
            string status = logs[i].substr(pos1 + 1, pos2 - pos1 - 1);
            string timestap_str = logs[i].substr(pos2 + 1, logs[i].size() - pos2 - 1);
            int id = atoi(id_str.c_str());
            int timestap = atoi(timestap_str.c_str());
            if (status == "start") {
                if (!VID.empty()) {
                    ans[VID.top()] += timestap - pre;
                }
                pre = timestap;
                VID.push(id);
            } else {
                ans[id] += timestap - pre + 1;
                pre = timestap + 1;
                VID.pop();
            }
        }
        return ans;
    }
};
```

1124

```cpp
class Solution {
public:
    int longestWPI(vector<int>& hours) {
        unordered_map<int, int> ind;
        unordered_map<int, int> f;
        int ans = 0;
        ind[0] = -1;
        f[0] = 0;
        for (int i = 0, cnt = 0; i < hours.size(); i++) {
            if (hours[i] > 8) cnt++;
            else cnt--;
            if (ind.find(cnt) == ind.end()) {
                ind[cnt] = i;
                if (ind.find(cnt - 1) == ind.end()) f[cnt] = 0;
                else f[cnt] = f[cnt - 1] + (i - ind[cnt - 1]);
            }
            if (ind.find(cnt - 1) == ind.end()) continue;
            ans = max(ans, f[cnt - 1] + (i - ind[cnt - 1]));
        }
        return ans;
    }
};
```