---
layout: post
title: Pigeonhole Sort
date: 2024-01-21 15:17 +0530
author: "Gaurav Kumar"
tags: "java sorting arrays"
categories: "sorting"
---

## PIGEONHOLE SORT

- Suitable for sorting integers within a small range when the distribution of values is relatively uniform.

- The time complexity of Pigeonhole Sort is `O(n + Range)`, where:
  - `n` is the number of elements to be sorted
  - `Range` is the difference between the `maximum` and `minimum` values in the input array.

## STEPS

1. **Find the Range:**

   - Calculate the `Range` of the input values -> `max - min + 1`

2. **Initialize Pigeonholes:**

   - Create an array of `pigeonHoles` of size `range`.

3. **Distribute Elements:**

   - For each element `A[i]` in the input array `A`:
     - Calculate the index `idx` as `A[i] - min`.
     - Add the element `A[i]` to the corresponding `pigeonhole` array.

4. **Collect Elements:**

   - For each pigeonhole:
     - For each element `x` in the pigeonhole:
       - Set `res[idx] = x`, and increment `idx`.

## IMPLEMENTATION

```java
public int[] sort(int[] A){

    int n = A.length;

    if(n < 1) return A;

    int min = Arrays.stream(A).min().orElse(Integer.MAX_VALUE);
    int max = Arrays.stream(A).max().orElse(Integer.MIN_VALUE);

    int range = max - min + 1;

    ArrayList<Integer>[] pigeonHoles = new ArrayList[range];
    for(int i=0; i<range; i++) pigeonHoles[i] = new ArrayList<>();

    for(int i=0; i<n; i++){
        int idx = A[i] - min;
        pigeonHoles[idx].add(A[i]);
    }

    int[] res = new int[n];

    int idx = 0;

    while(idx < n){

        for(int i=0; i<range; i++){

            for(int x: pigeonHoles[i]){
                res[idx] = x;
                idx++;
            }

        }

    }

    return res;

}
```
