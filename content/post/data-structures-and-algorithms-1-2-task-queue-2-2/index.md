---
title: Data Structures and Algorithms 1-2. Task-Queue (2/2)
date: 2022-01-20T15:10:17.125Z
summary: "***Leetcode- 622, 641, 1670, 933, 17.09, 859, 969, 621***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# Task-Queue

***Leetcode- 622, 641, 1670, 933, 17.09, 859, 969, 621***

622

```cpp
class MyCircularQueue {
public:
    vector<int> arr;
    int head, tail, cnt;
    MyCircularQueue(int k) : arr(k), head(0), tail(0), cnt(0){}

    bool enQueue(int value) {
        if (isFull()) return false;
        arr[tail] = value;
        cnt += 1;
        tail = (tail + 1) % arr.size();
        return true;
    }

    bool deQueue() {
        if (isEmpty()) return false;
        cnt -= 1;
        head = (head + 1) % arr.size();
        return true;
    }

    int Front() {
        if (isEmpty()) return -1;
        return arr[head];
    }

    int Rear() {
        if (isEmpty()) return -1;
        return arr[(tail - 1 + arr.size()) % arr.size()];
    }

    bool isEmpty() {
        return cnt == 0;
    }

    bool isFull() {
        return cnt == arr.size();
    }
};
```

641

```cpp
class MyCircularDeque {
public:
    vector<int> arr;
    int head, tail, cnt;
    MyCircularDeque(int k) : arr(k), head(0), tail(0), cnt(0) {}

    bool insertFront(int value) {
        if (isFull()) return false;
        head = (head - 1 + arr.size()) % arr.size();
        arr[head] = value;
        cnt += 1;
        return true;
    }

    bool insertLast(int value) {
        if (isFull()) return false;
        arr[tail] = value;
        cnt++;
        tail = (tail + 1) % arr.size();
        return true;
    }

    bool deleteFront() {
        if (isEmpty()) return false;
        head = (head + 1) % arr.size();
        cnt--;
        return true;
    }

    bool deleteLast() {
        if (isEmpty()) return false;
        tail = (tail - 1 + arr.size()) % arr.size();
        cnt--;
        return true;
    }

    int getFront() {
        if (isEmpty()) return -1;
        return arr[head];
    }

    int getRear() {
        if (isEmpty()) return -1;
        return arr[(tail - 1 + arr.size()) % arr.size()];
    }

    bool isEmpty() {
        return cnt == 0;
    }

    bool isFull() {
        return cnt == arr.size();
    }
};
```

1670

```cpp
class Node{
public:
    Node(int n = 0, Node* nxt = nullptr, Node* pre = nullptr) : val(n), nxt(nxt) , pre(pre){}
    int val;
    Node *nxt, *pre;
};

class DequeNode{
public:
    DequeNode() {
        head.nxt = &tail;
        tail.pre = &head;
        cnt = 0;
        h = &head;
        t = &tail;
    };
    void push_front(int val) {
        Node* node = new Node(val);
        node->nxt = h->nxt;
        node->pre = h;
        h->nxt = node;
        node->nxt->pre = node;
        cnt += 1;
    }
    void push_back(int val) {
        Node* node = new Node(val);
        node->pre = t->pre;
        node->nxt = t;
        t->pre = node;
        node->pre->nxt = node;
        cnt += 1;
    }
    int pop_front() {
        if (isEmpty()) return -1;
        int val = h->nxt->val;
        h->nxt = h->nxt->nxt;
        h->nxt->pre = h;
        cnt -= 1;
        return val;
    }
    int pop_back() {
        if (isEmpty()) return -1;
        int val = t->pre->val;
        t->pre = t->pre->pre;
        t->pre->nxt = t;
        cnt -= 1;
        return val;
    }
    int Getcnt() {
        return cnt;
    }
    bool isEmpty() {
        return cnt == 0;
    }

    Node head, tail, *h, *t;
    int cnt;
};

class FrontMiddleBackQueue {
public:
    DequeNode deque1, deque2;
    FrontMiddleBackQueue() : deque1(), deque2() {
    }

    void Adjust() {
        int cnt1 = deque1.Getcnt(), cnt2 = deque2.Getcnt(), val;
        if (cnt2 - cnt1 > 1) {
            val = deque2.pop_front();
            deque1.push_back(val);
        } else if (cnt1 > cnt2) {
            val = deque1.pop_back();
            deque2.push_front(val);
        }
        return ;
    }

    void pushFront(int val) {
        deque1.push_front(val);
        Adjust();
    }

    void pushMiddle(int val) {
        deque1.push_back(val);
        Adjust();
    }

    void pushBack(int val) {
        deque2.push_back(val);
        Adjust();
    }

    int popFront() {
        int val;
        if(!deque1.isEmpty()) {
            val = deque1.pop_front();
        } else if (!deque2.isEmpty()){
            val = deque2.pop_front();
        } else {
            return -1;
        }
        Adjust();
        return val;
    }

    int popMiddle() {
        int cnt1 = deque1.Getcnt(), cnt2 = deque2.Getcnt(), val;
        if ((cnt1 + cnt2) % 2) {
            val = deque2.pop_front();
            return val;
        } else {
            if ((cnt1 + cnt2) == 0) {
                return -1;
            } else {
                val = deque1.pop_back();
                return val;
            }
        }
        return -1;
    }

    int popBack() {
        if (deque2.isEmpty()) return -1;
        int val = deque2.pop_back();
        Adjust();
        return val;
    }

};
```

933

```cpp
class RecentCounter {
public:
    vector<int> v;
    int ind, cnt;
    RecentCounter() : ind(0), cnt(0) {}

    int ping(int t) {
        v.push_back(t);
        cnt++;
        for (int i = ind; v[i] < t - 3000; i++) ind++;
        return cnt - ind;
    }
};

/**
 * Your RecentCounter object will be instantiated and called as such:
 * RecentCounter* obj = new RecentCounter();
 * int param_1 = obj->ping(t);
 */
```

17.09

```cpp
class Solution {
public:
    int getKthMagicNumber(int k) {
        vector<int> ans{1};
        int cnt3 = 0, cnt5 = 0, cnt7 = 0, ind = 1;
        while (k != ind) {
            int a1 = ans[cnt3] * 3, a2 = ans[cnt5] * 5, a3 = ans[cnt7] * 7;

            int val_min = min(a1, min(a2, a3));
            ans.push_back(val_min);
            if (val_min == a1) cnt3++;
            if (val_min == a2) cnt5++;
            if (val_min == a3) cnt7++;
            ind++;
        }
        return ans[k - 1];
    }
};
```

859

```cpp
class Solution {
public:
    bool hasRepeat(string &s) {
        int cnt[26] = {0};
        for (auto x :s) {
            int ind = x - 'a';
            cnt[ind] += 1;
            if (cnt[ind] == 2) return true;
        }
        return false;
    }
    bool buddyStrings(string s, string goal) {
        vector<int> v;
        if (s.size() != goal.size()) return false;
        if (s == goal && hasRepeat(s)) return true;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] != goal[i]) v.push_back(i);
        }
        if (v.size() != 2) return false;
        if (s[v[0]] == goal[v[1]] && s[v[1]] == goal[v[0]]) return true;
        return false;
    }
};
```

969

```cpp
class Solution {
public:
    vector<int> pancakeSort(vector<int>& arr) {
        int n = arr.size();
        vector<int> v;
        while (n) {
            int ind;
            for (int i = 0; i < n; i++) {
                if (arr[i] == n) ind = i;
            }
            if (n - 1 == ind || n == 1) {
                n--;
                continue;
            }
            reverse(arr.begin(), arr.begin() + ind + 1);
            v.push_back(ind + 1);
            reverse(arr.begin(), arr.begin() + n);
            v.push_back(n);
            n--;
        }
        return v;
    }
};
```

621

```cpp
class Solution {
public:
    int leastInterval(vector<char>& tasks, int n) {
        if (n == 0) return tasks.size();
        int cnt[26] = {0};
        for (auto x : tasks) {
            int ind = x - 'A';
            cnt[ind] += 1;
        }
        sort(cnt, cnt + 26, [](int &a, int &b)->bool{return a > b;});
        int Max_val = cnt[0], count = 0;
        for (auto x : cnt) {
            if (x == Max_val) count++;
            else break;
        }
        int k = tasks.size();
        return max(k, (Max_val - 1) * (n + 1) + count);
    }
};
```