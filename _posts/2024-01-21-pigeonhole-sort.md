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

   - Create an array of `pigeonHoles`.

3. **Distribute Elements:**

   - For each element `A[i]` in the input array `A`:
     - Calculate the index `idx` as `A[i] - min`.
     - Add the element `A[i]` to the corresponding pigeonhole.

4. **Collect Elements:**

   - For each pigeonhole:
     - For each element `x` in the pigeonhole:
       - Set `res[idx] = x`, and increment `idx`.

## IMPLEMENTATION

```java
import java.util.ArrayList;
import java.util.Arrays;

public class PigeonholeSort {

    public static void main(String[] args) {

        int[] A = {4, 2, 1, 8, 3, 5, 7, 10, 9, 6};
        System.out.println(Arrays.toString(new PigeonholeSort().sort(A)));

    }

    public int[] sort(int[] A){

        int n = A.length;

        if(n < 1) return A;

        // find the min and max in the array
        int min = Arrays.stream(A).min().orElse(Integer.MAX_VALUE);
        int max = Arrays.stream(A).max().orElse(Integer.MIN_VALUE);

        // set the range
        int range = max - min + 1;

        // create a pigeonHole array to keep the elements
        // there can be duplicates, so use an ArrayList
        ArrayList<Integer>[] pigeonHoles = new ArrayList[n];
        for(int i=0; i<n; i++) pigeonHoles[i] = new ArrayList<>();

        // put the element in the pigeonHole at index `A[i] - min`
        for(int i=0; i<n; i++){
            int idx = A[i] - min;
            pigeonHoles[idx].add(A[i]);
        }

        // create a result array which will have sorted array
        int[] res = new int[n];

        int idx = 0;

        // get elements from pigeonHole array and put them into the result array
        while(idx < n){

            for(int i=0; i<n; i++){

                for(int x: pigeonHoles[i]){
                    res[idx] = x;
                    idx++;
                }

            }

        }

        return res;

    }

}
```
