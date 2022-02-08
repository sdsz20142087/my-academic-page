---
title: Data Structures and Algorithms 3-1. Quick-Sort (1/2)
date: 2022-02-08T03:34:33.706Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
## 1. Implementation

### version 1 (basic, Recursion)

```cpp
void quick_sort_v1(int *arr, int l, int r) {
    if (l >= r) return ;
    int x = l, y = r, base = arr[l];
    while (x < y) {
        while (x < y && base <= arr[y]) y--;
        if (x < y) arr[x++] = arr[y];
        while (x < y && base > arr[x]) x++;
        if (x < y) arr[y--] = arr[x];
    }
    arr[x] = base;
    quick_sort_v1(arr, l, x - 1);
    quick_sort_v1(arr, x + 1, r);
    return ;
}
```

### version 2 (left traversal, right Recursion)

```cpp
void quick_sort_v2(int *arr, int l, int r) {
    while (l < r) {
        int x = l, y = r, base = arr[l];
        while (x < y) {
            while (x < y && base <= arr[y]) y--;
            if (x < y) arr[x++] = arr[y];
            while (x < y && base > arr[x]) x++;
            if (x < y) arr[y--] = arr[x];
        }
        arr[x] = base;
        quick_sort_v2(arr, x + 1, r);
        r = x - 1;
    }
    return ;
}
```

### version 3 (version 2 + InsertSort -- Threshold + getmid)

```cpp
const int threshold = 3;
inline int getmid(int a, int b, int c) {
    if (a > b) swap(a, b);
    if (a > c) swap(a, c);
    if (b > c) swap(b, c);
    return b;
}

void __quick_sort_v3(int *arr, int l, int r) {
    while (r - l > threshold) {
        int x= l, y = r, base = getmid(arr[l], arr[r], arr[(l + r) >> 1]);
        do{
            while (arr[x] < base) x++;
            while (arr[y] > base) y--;
            if (x <= y) {
                swap(arr[x++], arr[y--]);
            }
        } while (x <= y);
        __quick_sort_v3(arr, x, r);
        r = y;
    }
}

void __insert_sort(int *arr, int l, int r) {
    int ind = l;
    for (int i = l + 1; i <= r; i++) if (arr[i] < arr[ind]) ind = i;
    while (ind > l) {
        swap(arr[ind], arr[ind - 1]);
        ind--;
    }
    for (int i = l + 2; i <= r; i++) {
        int j = i;
        while (arr[j] < arr[j - 1]) {
            swap(arr[j], arr[j - 1]);
            j--;
        }
    }
    return ;
}

void quick_sort_v3(int *arr, int l, int r) {
    __quick_sort_v3(arr, l, r);
    __insert_sort(arr, l, r);
    return ;
}
```