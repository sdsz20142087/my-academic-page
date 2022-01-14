---
title: Data Structures and Algorithms 1. List (2/2)
date: 2022-01-14T09:46:53.998Z
draft: false
featured: true
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
# List

***Leetcode- 141, 142, 202, 206, 92, 25, 61, 19, 83, 82***

**Problem solving tips**

**1. Determine the circle in a list: Fast & Slow pointer.**

**2. Head address may change: Create a virtual head node.**

141

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (head == nullptr) return false;
        ListNode *p, *q;
        p = head;
        q = head->next;
        while (p && q && q->next) {
            if (p == q) return true;
            p = p->next;
            q = q->next->next;
        }
        return false;
    }
};
```

142

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if (head == nullptr) return nullptr;
        ListNode *p, *q;
        p = q = head;
        if (!q->next) return nullptr;
        p = p->next;
        q = q->next->next;
        while (q && q->next) {
            p = p->next;
            q = q->next->next;
            if (p == q) break;
        }
        if (!q|| !q->next) return nullptr;
        q = head;
        while (p != q) {
            p = p->next;
            q = q->next;
        }
        return p;
    }
};
```

202

```cpp
class Solution {
public:
    int GetNext(int n) {
        int cnt = 0;
        while (n) {
            int tmp = n % 10;
            n /= 10;
            cnt += tmp * tmp;
        }
        return cnt;
    }
    bool isHappy(int n) {
        int fast, slow;
        fast = slow = n;
        do{
            slow = GetNext(slow);
            fast = GetNext(GetNext(fast));
            if (slow == 1 || fast == 1) return true;
        } while (fast != slow);
        return false;
    }
};
```

206 - solution 1

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head) return head;
        ListNode *h, *t;
        h = nullptr, t = head;
        while (t) {
            ListNode* tmp = t->next;
            t->next = h;
            h = t;
            t = tmp;
        }
        return h;
    }
};
```

206 - solution 2

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode *h = head, *t = head->next, *p = reverseList(head->next);
        h->next = t->next;
        t->next = h;
        return p;
    }
};
```

92

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseN(ListNode* head, int n) {
        if (n == 1) return head;
        ListNode *h = head, *t = head->next, *p = reverseN(head->next, n - 1);
        h->next = t->next;
        t->next = h;
        return p;
    }
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        if (!head) return head;
        int n = right - left;
        ListNode ret(0, head);
        ListNode *p = &ret;
        while (left - 1) {
            p = p->next;
            left--;
        }
        p->next = reverseN(p->next, n + 1);
        return ret.next;
    }
};
```

25

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseN(ListNode* head, int n) {
        if (n == 1) return head;
        ListNode *h = head, *t = head->next, *p = reverseN(head->next, n - 1);
        h->next = t->next;
        t->next = h;
        return p;
    }

    ListNode* reverseKGroup(ListNode* head, int k) {
        if (!head) return head; 
        int n = k;
        ListNode ret(0, head);
        ListNode *p = &ret, *h, *t;
        h = t = p;
        while (n && t && t->next) {
            t = t->next;
            n--;
            if (n == 0) {
                p = h->next;
                h->next = reverseN(h->next, k);
                h = t = p;
                n = k;
            }
        }
        return ret.next;
    }
};
```

61

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head) return head;
        ListNode *h, *t;
        h = t = head;
        int len = 0;
        while (h) {
            len++;
            h = h->next;
        }
        h = head;
        k = k % len;
        while (k) {
            t = t->next;
            k--;
        }
        while (t->next) {
            h = h->next;
            t = t->next;
        }
        t->next = head;
        t = h->next;
        h->next = nullptr;
        return t;
    }
};
```

19

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode ret(0, head), *p1, *p2;
        p1 = p2 = &ret;
        while (n--) {
            p1 = p1->next;
        }
        while (p1->next) {
            p1 = p1->next;
            p2 = p2->next;
        }
        p2->next = p2->next->next;
        return ret.next;
    }
};
```

83

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head) return head;
        ListNode *p = head;
        while (p->next) {
            if (p->val == p->next->val) {
                p->next = p->next->next;
            } else {
                p = p->next;
            }
        }
        return head;
    }
};
```

82

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode ret(0, head);
        ListNode *p, *q;
        p = &ret;
        while (p->next) {
            if (p->next->next && p->next->val == p->next->next->val) {
                q = p->next->next;
                while (q && q->val == p->next->val) {
                    q = q->next;
                }
                p->next = q;
            } else {
                p = p->next;
            }
        }
        return ret.next;
    }
};
```
