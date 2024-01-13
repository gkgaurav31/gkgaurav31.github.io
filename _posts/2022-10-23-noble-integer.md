---
layout: post
title: Noble Integer
date: 2022-10-23 10:49 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays important"
categories: "interviewbit"
---

## Problem Description

Given an integer array A, find if an integer p exists in the array such that the number of integers greater than p in the array equals to p.

### Solution

Let's say we sort the array in ascending order and iterate from the right.

Important Observations:

- If A[i] == A[i+1], the number of elements which are greater than them will be the same.
- If A[i] != A[i+1], the number of elements which are greater than A[i] will be equal to (n-i-1) since we have sorted the array.

```java
public class Solution {

    public int solve(ArrayList<Integer> A) {

        Collections.sort(A);

        int n = A.size();

        if(A.get(n-1) == 0) return 1; //Edge case, if the last element is 0

        for(int i=0; i<n-1; i++){

            int p = A.get(i);
            int right = n-i-1;

            //current element == number of elements on its right
            //The list can also have duplicates. If the number of its right is a duplicate, we cannot include that since we are looking for stricly greater
            if(p == right && p != A.get(i+1)) return 1;
        }

        return -1;

    }
}
```

### ANOTHER WAY TO CODE

```java
public class Solution {

    public int solve(ArrayList<Integer> A) {

        //sort in ascending order
        Collections.sort(A);

        int n = A.size();

        //iterate from left to right
        for(int i=0; i<n; i++){

            //This is the main part:
            //If there were no duplicates, we could have said that all numbers towards right are greater
            //But, if there are any duplicates, we cannot do that. We also don't know how many duplicates can be there.
            //But it is safe to skip the duplicates except the last one.
            //For the last element in the continous sequence of duplicates, we can definitely say that all elements towards its right are greater
            //If that matches the value of that number, we have a nobel number
            //Example: [0, 0, 1, 2, 3, 3, 6, 7, 9]
            //For all occurances of 3, the number of elements more than 3 is going to be the same, which is 3
            //We know we would need to ignore the duplicates when considering the first 3. But we don't know how many duplicates will be there.
            //So, we can simply calculate the answer for the last occurance of 3. The answer will be the same for all occurances of 3!
            if(i < n-1 && A.get(i+1) == A.get(i)) continue;

            int x = A.get(i);
            int y = n - i - 1;

            if(x == y)
                return 1;

        }


        return -1;

    }

}
```
