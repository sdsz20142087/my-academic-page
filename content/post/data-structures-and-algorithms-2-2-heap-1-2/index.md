---
title: Data Structures and Algorithms 2-2. Heap (1/2)
date: 2022-02-02T03:23:04.531Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# Heap

***Leetcode- offer - 40, 1046, 703, 215, 373, 692, 295, 264, 313, 1753, 1801***

offer - 40

```cpp
class Solution {
public:    
    vector<int> getLeastNumbers(vector<int>& arr, int k) {
        Heap<int> h{less<int>()};
        for (auto x: arr) {
            h.push(x);
            if (h.size() > k) { h.pop(); }
        }
        return h;
    }
};
```

1046

```cpp
class Solution {
public:
    int lastStoneWeight(vector<int>& stones) {
        Heap<int> h{less<int>()};
        int a, b;
        for (auto x : stones) h.push(x);
        while (h.size() > 1) {
            a = h.top();
            h.pop();
            b = h.top();
            h.pop();
            if (a == b) continue;
            h.push(a - b);
        }
        if (!h.size()) return 0;
        return h.top();
    }
};
```

703

```cpp
class KthLargest {
public:
    int n;
    Heap<int> h{greater<int>()};
    KthLargest(int k, vector<int>& nums) {
        n = k;
        for (auto x : nums) {
            add(x);
        }
    }

    int add(int val) {
        h.push(val);
        if (h.size() > n) h.pop();
        return h.top();
    }
};
```

215

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        Heap<int> h{greater<int>()};
        for (auto x : nums) {
            if (h.size() >= k && x <= h.top()) continue;
            h.push(x);
            if (h.size() > k) h.pop();
        }
        return h.top();
    }
};
```

373

```cpp
struct cmp{
    bool operator()(vector<int> a, vector<int> b) {
        return a[0] + a[1] < b[0] + b[1];
    }
};

class Solution {
public:
    vector<vector<int>> kSmallestPairs(vector<int>& nums1, vector<int>& nums2, int k) {
        cmp less_than;
        Heap<vector<int>> h{less_than};
        vector<int> tmp(2);
        for (auto x : nums1) {
            for (auto y : nums2) {
                tmp[0] = x, tmp[1] = y;
                if (h.size() < k || less_than(tmp, h.top())) {
                    h.push(tmp);
                    if (h.size() > k) h.pop();
                } else break;

            }
        }
        return h;
    }
};
```

692

```cpp
struct PII{
    string s;
    int cnt;
};

struct CMP{
    bool operator()(PII a, PII b) {
        if (a.cnt == b.cnt) return a.s < b.s;
        return a.cnt > b.cnt;
    }
};

class Solution {
public:
    unordered_map<string, int> m;
    vector<string> topKFrequent(vector<string>& words, int k) {
        vector<string> ans;
        CMP cmp;
        Heap<PII> h{cmp};
        for (auto x: words) m[x] += 1;
        for (auto x : m) {
            h.push(PII{x.first, x.second});
            if (h.size() > k) h.pop();
        };
        for (auto x : h) {
            ans.push_back(h.top().s);
            h.pop();
        }
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

295

```cpp
class MedianFinder {
public:
    MedianFinder() {}
    Heap<int> h_max{less<int>()};
    Heap<int> h_min{greater<int>()};
    void addNum(int num) {
        h_max.push(num);
        h_min.push(h_max.top());
        h_max.pop();
        if (h_min.size() > h_max.size()) {
            h_max.push(h_min.top());
            h_min.pop();
        }
        return ;
    }

    double findMedian() {
        if (h_max.size() == h_min.size()) return (h_max.top() + h_min.top()) / 2.0;
        return h_max.top();
    }
};
```

264

```cpp
class Solution {
public:
    int nthUglyNumber(int n) {
        Heap<long long> h{greater<long long>()};
        h.push(1);
        long long tmp;
        while (n--) {
            tmp = h.top();
            h.pop();
            if (tmp % 5 == 0) {
                h.push(tmp * 5);
            } else if (tmp % 3 == 0) {
                h.push(tmp * 3);
                h.push(tmp * 5);
            } else {
                h.push(tmp * 2);
                h.push(tmp * 3);
                h.push(tmp * 5);
            }
        }
        return tmp;
    }
};
```

313

```cpp
class Solution {
public:
    int nthSuperUglyNumber(int n, vector<int>& primes) {
        int k = primes.size();
        vector<int> cnt(k, 0);
        vector<int> ans;
        ans.push_back(1);
        int min_val = 1;
        n--;
        while (n--) {
            min_val = INT32_MAX;
            for (int i = 0; i < k; i++) {
                min_val = min(min_val, ans[cnt[i]] * primes[i]);
            }
            ans.push_back(min_val);
            for (int i = 0; i < k; i++) {
                if (min_val == (ans[cnt[i]] * primes[i])) cnt[i]++;
            }
        }
        return min_val;
    }
};
```

1753

```cpp
class Solution {
public:
    int maximumScore(int a, int b, int c) {
        if (a > b) swap(a, b);
        if (a > c) swap(a, c);
        if (b > c) swap(b, c);
        if (a > (c - b)) {
            int tmp = a - (c - b);
            if (tmp % 2) return a - 1 + b - (tmp - 1) / 2;
            else return a + b - tmp / 2;
        } else {
            return a + b;
        }
        return a + b;
    }
};
```

1801

```cpp
struct PII{
    int price, amount;
};

struct CMP1{
    bool operator()(PII a, PII b) {
        return a.price < b.price;
    }
};

struct CMP2{
    bool operator()(PII a, PII b) {
        return a.price > b.price;
    }
};

class Solution {
public:
    //price, amount, type  0 buy
    CMP1 cmp1;
    CMP2 cmp2;
    Heap<PII> h_buy{cmp1};
    Heap<PII> h_sell{cmp2};
    PII tmp, tar;
    int getNumberOfBacklogOrders(vector<vector<int>>& orders) {
        int ans = 0, m, n, mod_val = 1e9 + 7;
        for (auto x : orders) {
            if (x[2] == 0) {
                while (x[1] && h_sell.size() && h_sell.top().price <= x[0]) {
                    if (h_sell.top().amount <= x[1]) {
                        x[1] -= h_sell.top().amount;
                        h_sell.pop();
                    } else {
                        h_sell.top().amount -= x[1];
                        x[1] = 0;
                    }
                }
                if (x[1]) h_buy.push(PII{x[0], x[1]});
            } else {
                while (x[1] && h_buy.size() && h_buy.top().price >= x[0]) {
                    if (h_buy.top().amount <= x[1]) {
                        x[1] -= h_buy.top().amount;
                        h_buy.pop();
                    } else {
                        h_buy.top().amount -= x[1];
                        x[1] = 0;
                    }
                }
                if (x[1]) h_sell.push(PII{x[0], x[1]});
            }
        }
        m = h_buy.size(), n = h_sell.size();
        while (m--) {
            ans += h_buy.top().amount;
            ans = ans % mod_val;
            h_buy.pop();
        }
        while (n--) {
            ans += h_sell.top().amount;
            ans = ans % mod_val;
            h_sell.pop();
        }
        return ans;
    }
};
```