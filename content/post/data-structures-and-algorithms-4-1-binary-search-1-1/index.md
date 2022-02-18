---
title: Data Structures and Algorithms 4-1. Binary-Search (1/1)
date: 2022-02-18T06:04:18.882Z
summary: "***Leetcode- 69, 35, 1, 34, 1658, 475, 81, 4***"
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## 1. Implementation

```cpp
#include <iostream>
#include <cstdio>
using namespace std;

int binary_search(int *arr, int n, int val) {
    int head = 0, tail = n - 1;
    while (head <= tail) {
        int mid = (head + tail) >> 1;
        if (arr[mid] < val) head = mid + 1;
        else if (arr[mid] > val) tail = mid - 1;
        else return mid;
    }
    return -1;
}

int main() {
    int arr[5] = {1, 4, 6, 12, 20}, x;
    while (cin >> x) {
        printf("arr[%d] = %d\n", binary_search(arr, 5, x), x);
    }
    return 0;
}
```

## 2. Leetcode

***Leetcode- 69, 35, 1, 34, 1658, 475, 81, 4***

69

```cpp
class Solution {
public:
    int mySqrt(int x) {
        long long head = 0;
        long long tail = (long long)x + 1;
        while (head < tail) {
            long long mid = head + (tail - head + 1) / 2;
            if (mid * mid == x) return mid;
            else if (mid * mid < x) head = mid;
            else tail = mid - 1;
        }
        return head;
    }
};
```

35

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int head = 0, tail = nums.size();
        while (head < tail) {
            int mid = (head + tail) >> 1;
            if (nums[mid] < target) head = mid + 1;
            else tail = mid;
        }
        return head;
    }
};
```

1

```cpp
class Solution {
public:
    int binary_search(vector<int>& ind, vector<int>& nums, int l, int r, int target) {
        int head = l, tail = r;
        while (l <= r) {
            int mid = (l + r) >> 1;
            if (nums[ind[mid]] == target) return ind[mid];
            if (nums[ind[mid]] < target) l = mid + 1;
            else r = mid - 1;
        }
        return -1;
    }

    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> ans, ind;
        int n = nums.size();
        for (int i = 0; i < n; i++) ind.push_back(i);
        sort(ind.begin(), ind.end(), [&](int a, int b)->bool{return nums[a] < nums[b];});
        for (int i = 0; i < n; i++) {
            int pos = binary_search(ind, nums, i + 1, n - 1, target - nums[ind[i]]);
            if (pos != -1) {
                ans.push_back(ind[i]);
                ans.push_back(pos);
                return ans;
            }
        }
        return ans;
    }
};
```

34

```cpp
class Solution {
public:
    int binary_search_10(vector<int>& nums, int target) {
        int head = 0, tail = nums.size() - 1;
        if (nums.front() == target) return -1;
        while (head < tail) {
            int mid = (head + tail + 1) >> 1;
            if (nums[mid] < target) head = mid;
            else tail = mid - 1;
        }
        return head;
    }

    vector<int> searchRange(vector<int>& nums, int target) {
        if (nums.size() == 0 || target < nums.front() || target > nums.back()) 
            return vector<int>{-1, -1};
        int ind1, ind2;
        ind1 = binary_search_10(nums, target);
        ind2 = binary_search_10(nums, target + 1);
        if (ind1 == ind2) return vector<int>{-1, -1};
        ind1++;
        return vector<int>{ind1, ind2};
    }
};
```

1658

```cpp
class Solution {
public:
    int binary_search(vector<int>& num_r, int target, int r_side) {
        int head = 0, tail = r_side;
        while (head <= tail) {
            int mid = (head + tail) >> 1;
            if (num_r[mid] == target) return mid;
            if (num_r[mid] < target) head = mid + 1;
            else tail = mid - 1;
        }
        return -1;
    }

    int minOperations(vector<int>& nums, int x) {
        int n = nums.size(), ans = -1, l_val;
        vector<int> num_l(n + 1), num_r(n + 1);
        num_l[0] = 0, num_r[0] = 0;
        for (int i = 1; i <= n; i++) {
            num_l[i] = num_l[i - 1] + nums[i - 1];
            num_r[i] = num_r[i - 1] + nums[n - i];
        }
        for (int i = 0; i <= n; i++) {
            l_val = i;
            int ind = binary_search(num_r, x - num_l[i], n - i);
            if (ind == -1) continue;
            l_val += ind;
            if (ans == -1) ans = l_val;
            else ans = min(ans, l_val);
        }
        return ans;
    }
};
```

475

```cpp
class Solution {
public:
    int binary_search(vector<int>& heaters, int target) {
        int head = 0, tail = heaters.size() - 1;
        while (head < tail) {
            int mid = (head + tail + 1) >> 1;
            if (heaters[mid] <= target) head = mid;
            else tail = mid - 1;
        }
        return head;
    }

    int findRadius(vector<int>& houses, vector<int>& heaters) {
        int ans = 0, n = heaters.size();
        sort(heaters.begin(), heaters.end());
        for (int i = 0; i < houses.size(); i++) {
            int indl = binary_search(heaters, houses[i]);
            int a = abs(heaters[indl] - houses[i]);
            int b = (indl == n - 1) ? (a + 1) : abs(heaters[indl + 1] - houses[i]);
            ans = max(ans, min(a, b));
        }
        return ans;
    }
};
```

81

```cpp
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int head = 0, tail = nums.size() - 1;
        while (head < nums.size() && tail >= 0 && nums[head] == nums[tail]) {
            if (target == nums[head]) return true;
            head++;
            tail--;
        }
        while (head <= tail) {
            int mid = (head + tail) >> 1;
            if (nums[mid] == target) return true;
            if (nums[mid] <= nums[tail]) {
                if (target > nums[mid] && target <= nums[tail]) head = mid + 1;
                else tail = mid - 1;
            } else {
                if (target >= nums[head] && target < nums[mid]) tail = mid - 1;
                else head = mid + 1;
            }
        }
        return false;
    }
};
```

4

```cpp
class Solution {
public:
    int Getnval(vector<int>& nums1, vector<int>& nums2, int target) {
        int lind = 0, rind = 0, p1 = nums1.size(), p2 = nums2.size();
        while (target) {
            if (target == 1) {
                if (lind >= p1) return nums2[rind];
                if (rind >= p2) return nums1[lind];
                return min(nums1[lind], nums2[rind]);
            }
            int temp = target / 2;
            if (rind + temp - 1 >= p2 || (lind + temp - 1 < p1 && nums1[lind + temp - 1] <= nums2[rind + temp - 1])) {
                target -= temp;
                lind += temp;
            } else {
                target -= temp;
                rind += temp;
            }
        }
        return -1;
    }

    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int m = nums1.size(), n = nums2.size(), lind = 0, rind = 0;
        int k = m + n;
        double l, r;
        l = (double) Getnval(nums1, nums2, k / 2 + 1);
        if (k % 2) {
            return l;
        }
        r = (double) Getnval(nums1, nums2, k / 2);
        return (l + r) / 2.0;
    }
};
```