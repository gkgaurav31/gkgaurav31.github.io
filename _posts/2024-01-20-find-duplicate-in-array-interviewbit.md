---
layout: post
title: Find Duplicate in Array (InterviewBit)
date: 2024-01-20 22:38 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a read-only array of n + 1 integers between 1 and n, find one number that repeats in linear time using less than O(n) space and traversing the stream sequentially O(1) times.

If there are multiple possible answers ( like in the sample case ), output any one, if there is no duplicate, output -1

[InterviewBit](https://www.interviewbit.com/problems/find-duplicate-in-array/)

## SOLUTION

### APPROACH 1 - O(N) SPACE COMPLEXITY

```java
public class Solution {

    public int repeatedNumber(final int[] A) {

        int n = A.length;

        Set<Integer> set = new HashSet<>();

        for(int i=0; i<n; i++){
            if(set.contains(A[i])) return A[i];
            else set.add(A[i]);
        }

        return -1;
    }

}
```

### APPROACH 2 - MARK IN THE ARRAY

As per the constraints: `1 <= A[i] <= |A|`. So, if the element at any position is `X`, we can mark the index `(X-1)` as negative to mark that element as present. While iterating, if at any position we find that the value at index `(X-1)` is already -ve, that means it is a repeated number.

```java
public class Solution {

    public int repeatedNumber(final int[] A) {

        int n = A.length;

        for(int i=0; i<n; i++){

            int x = Math.abs(A[i]);

            if(A[x-1] < 0){
                return x;
            }else{
                A[x-1] = -A[x-1];
            }

        }

        return -1;

    }
}
```

### APPROACH 3 - USING SLOW AND FAST POINTER

The below solution was accepted, but I think it will fail when there are no duplicates. This is similar to cycle detection algoritm using fast and slow pointers in Linked Lists. We start from the beginning, slow move by 1 step and fast moves ahead by 2 steps. If they meet, there must be a cycle. To find the start of the cycle, reset any of the pointers to the beginning and then move both of them one step at a time until they are equal.

```java
public class Solution {

    public int repeatedNumber(final int[] A) {

        int slow = A[0];
        int fast = A[0];

        // detect cycle
        do{
            slow = A[slow];
            fast = A[A[fast]];
        }while(slow != fast);

        // reset one of the pointers to the beginning
        slow = A[0]; // fast = A[0] can also be used

        // move both by 1 step to get the starting position of the cycle
        while(slow != fast){
            slow = A[slow];
            fast = A[fast];
        }

        return slow;

    }
}
```
