---
title: Data Structures and Algorithms 1-3. Stack (1/2)
date: 2022-01-24T08:02:55.368Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# Stack

***The stack can handle problems with full inclusion relation.***

## 1. Implementation

```cpp
#include <iostream>
#include <vector>
using namespace std;
class Stack{
public:
    Stack() : size(0) {}
    void push(int val) {
        s.push_back(val);
        size++;
        return ;
    }
    void pop() {
        if (isEmpty()) return ;
        s.pop_back();
        size--;
    }
    bool isEmpty() {
        return size == 0;
    }
    int GetSize() {
        return size;
    }
    void output() {
        cout << "========" << endl;
        for (int i = size - 1; i >= 0; i--) cout << s[i] << endl;
        cout << "========" << endl;
    }
private:
    vector<int> s;
    int size;
};

int main() {
    string op;
    int val;
    Stack s;
    while (cin >> op) {
        if (op == "push") {
            cin >> val;
            s.push(val);
        } else if (op == "pop") {
            s.pop();
        } else if (op == "output") {
            s.output();
        }
    }

    return 0;
}
```

## 2. Expression evaluation

### V1 - Recursion

```cpp
#include <iostream>
#include <cstdio>
#include <cstring>
using namespace std;

int Calc(char* s, int l, int r) {
    int op = -1, min_priority = 10000, cur_priority = INT32_MAX, temp = 0;
    for (int i = l ; i <= r; i++) {
        switch(s[i]) {
            case '+':
            case '-': cur_priority = temp + 1; break;
            case '*':
            case '/': cur_priority = temp + 2; break;
            case '(': temp += 100; break;
            case ')': temp -= 100; break;
            default: break;
        }
        if (cur_priority < min_priority) {
            min_priority = cur_priority;
            op = i;
        }
    }
    if (op == -1) {
        int num = 0;
        for (int i = l; i <= r; i++) {
            if (s[i] < '0' || s[i] > '9') continue;
            num = num * 10 + s[i] - '0';
        }
        return num;
    }
    int lval = Calc(s, l, op - 1);
    int rval = Calc(s, op + 1, r);
    switch(s[op]) {
        case '+' : return lval + rval;
        case '-' : return lval - rval;
        case '*' : return lval * rval;
        case '/' : return lval / rval;
        default : return -1;
    }
    return -1;
}

int main() {
    char s[100];
    while (~scanf("%[^\n]s", s)) {
        getchar();
        printf("%s = %d\n", s, Calc(s, 0, strlen(s) - 1));
    }
    return 0;
}
```

### V2 - Stack

```cpp
#include <iostream>
#include <stack>
#include <string>
#include <cstring>
using namespace std;

int level(char c) {
    switch (c) {
        case '@': return -5;
        case '+':
        case '-': return 1;
        case '*':
        case '/': return 2;
        case '(': return -3;
        case ')': return -1;
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

int main() {
    stack<int> nums;
    stack<char> ops;
    string s;
    char cur_op;
    int a, b;
    while (getline(cin, s)) {
        cout << s << endl;
        s += "@";
        bool flag = true;
        for (int i = 0, cnt = 0; i < s.size(); i++) {
            if (s[i] == ' ') continue;
            if (level(s[i]) == 0) {
                cnt = cnt * 10 + (s[i] - '0');
                flag = true;
                continue;
            }
            if (s[i] == '(') {
                ops.push(s[i]);
                continue;
            }
            if (flag) nums.push(cnt);
            cnt = 0;
            if (s[i] == ')') flag = false;

            while (!ops.empty() && level(s[i]) <= level(ops.top())) {
                cur_op = ops.top();
                ops.pop();
                a = nums.top();
                nums.pop();
                b = nums.top();
                nums.pop();
                nums.push(calc(cur_op, b, a));
            }
            if (s[i] == ')') ops.pop();
            else ops.push(s[i]);
        }
        cout << nums.top() << endl;
        while (!nums.empty()) nums.pop();
        while (!ops.empty()) ops.pop();

    }
    return 0;
}
```