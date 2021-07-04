---
title: Two efficient algorithms for filtering out prime numbers.
subtitle: Read in 20 minutes, code is C language
date: 2021-07-04T06:53:49.464Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
First of all, a prime number has the property that it is only divisible by 1 and itself. Our goal is to filter out the prime numbers in the given range, for example, if the range is 1-10, the answer is 2,  3, 5, 7.

First algorithm: Time Complexity O(n * loglogn)

idea: use prime number to mark composite number, e.g. use 2(prime number) to mark 2 \* 2, 2 \* 3, and 2 * 4 etc as composite number.

```c
#include<stdio.h>
#define MAX_N 100
int prime_array[MAX_N + 5] = {0};
void init_prime() {
    for (int i = 2; i <= MAX_N; i++) {
        if (prime_array[i]) continue;
        prime_array[++prime_array[0]] = i;
        for (int j = i, I = MAX_N / i; j <= I; j++) {
            prime_array[i * j] = 1;
        }
    }
    return;
}
int main() {
    init_prime();
    for (int i = 1; i <= prime_array[0]; i++) {
        printf("%d\n", prime_array[i]);
    }
    return 0;
}
```



Second algorithm: Time Complexity O(n)

idea: Labeling with the minimum prime factor of the target element. e.g. use 3 to label 45 instead of 5.

line 11 is important: this line ensures that there is no prime number larger than P in M, because if there is, M is not the largest composite of this element; for example, for 45, P should be 3 and M should be 15, so when M = 9, this command ensures that the operation stops when the prime number 3 is marked to 9. So it can help to find the minimum prime factor and maximum composite factor of the target element smoothly.

```c
#include<stdio.h>
#define MAX_N 100

int prime_array[MAX_N + 5] = {0};
void init_prime() {
    for (int i = 2; i <= MAX_N; i++) {//M
        if (!prime_array[i]) prime_array[++prime_array[0]] = i;
        for (int j = 1; j <= prime_array[0]; j++) {//P
            if (i * prime_array[j] >= MAX_N) break;
            prime_array[i * prime_array[j]] = 1;//N = M * P
            if (i % prime_array[j] == 0) break;
        }
    }
    return ;
}

int main() {
    init_prime();
    for (int i = 1; i <= prime_array[0]; i++) {
        printf("%d\n",prime_array[i]);
    }
    return 0;
}
```