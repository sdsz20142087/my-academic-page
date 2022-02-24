---
title: Data Structures and Algorithms 4-2. Hash-Table (1/2)
date: 2022-02-24T03:18:40.623Z
summary: "***Hash function: Map arbitrary type data to array subscripts. (High
  dimension to low dimension)***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## 1. Characteristics

***Hash function: Map arbitrary type data to array subscripts. (High dimension to low dimension)***

## 2. Hash Collision (solution)

**<1> Open Addressing : if ind collision, try ind + 1, then ind + 2. (linear probing)**

```cpp
#include <iostream>
#include <vector>
#include <cstring>
#include <string>
using namespace std;
class HashTable{
public:
    HashTable(int n = 100) : cnt(0), data(n), flag(n) {}
    void Hash_insert(string s) {
        int ind = GetHashv(s) % data.size();
        reassign(ind, s);
        data[ind] = s;
        flag[ind] = true;
        cnt++;
        return ;
    }
    bool Hash_find(string s) {
        int ind = GetHashv(s) % data.size();
        reassign(ind, s);
        return flag[ind];
    }

private:
    int GetHashv(string s) {
        int seed = 131, hash_val = 0;
        for (int i = 0; s[i]; i++) {
            hash_val = hash_val * seed + s[i];
        }
        return hash_val & 0x7fffffff;
    }
    void reassign(int& ind, string s) {
        int t = 1;
        while (flag[ind] && data[ind] != s) {
            ind = (ind + t * t) % data.size();
            t++;
        }
        return ;
    }

    int cnt;
    vector<string> data;
    vector<bool> flag;
};
```

**<2> Double hashing: multiple hash function.**

**<3> Establish public overflow area**

```cpp
#include <iostream>
#include <vector>
#include <cstring>
#include <string>
#include <set>
using namespace std;
class HashTable{
public:
    HashTable(int n = 100) : cnt(0), data(n), flag(n) {}
    void Hash_insert(string s) {
        int ind = GetHashv(s) % data.size();
        if (flag[ind] == false) {
            data[ind] = s;
            flag[ind] = true;
            cnt++;
        } else if (data[ind] != s){
            h_set.insert(s);
        }
        return ;
    }
    bool Hash_find(string s) {
        int ind = GetHashv(s) % data.size();
        if (flag[ind] == false) return false;
        if (data[ind] == s) return true;
        return h_set.find(s) != h_set.end();
    }

private:
    int GetHashv(string s) {
        int seed = 131, hash_val = 0;
        for (int i = 0; s[i]; i++) {
            hash_val = hash_val * seed + s[i];
        }
        return hash_val & 0x7fffffff;
    }
    int cnt;
    vector<string> data;
    vector<bool> flag;
    set<string> h_set;
};
```

**<4> Separate Chaining (Recommend) : each position in hash table stores a List head node, every value that should be stored in this position will connect the previous node and form a List.**

```cpp
#include <iostream>
#include <vector>
#include <cstring>
#include <string>
#include <set>
using namespace std;
class Node{
public:
    Node(string data = "", Node* next = nullptr) : data(data), next(next) {}
    virtual~Node() {}
    string data;
    Node* next;
};

class HashTable{
public:
    HashTable(int n = 100) : cnt(0), data(n) {}
    void Hash_insert(string s) {
        int ind = GetHashv(s) % data.size();
        Node* p = &data[ind];
        while (p->next && p->next->data != s) p = p->next;
        if (p->next == nullptr) {
            p->next = new Node(s);
            cnt++;
        }
        return ;
    }
    bool Hash_find(string s) {
        int ind = GetHashv(s) % data.size();
        Node* p = &data[ind];
        while (p->next && p->next->data != s) p = p->next;
        return p->next != nullptr;
    }
private:
    int GetHashv(string s) {
        int seed = 131, hash_val = 0;
        for (int i = 0; s[i]; i++) {
            hash_val = hash_val * seed + s[i];
        }
        return hash_val & 0x7fffffff;
    }
    int cnt;
    vector<Node> data;
};
```