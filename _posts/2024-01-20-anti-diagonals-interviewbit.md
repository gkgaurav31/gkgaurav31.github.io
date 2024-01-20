---
layout: post
title: Anti Diagonals (InterviewBit)
date: 2024-01-20 14:55 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Give a `N x N` square matrix, return an array of its anti-diagonals. Look at the example for more details.

Input:

```text
1 2 3
4 5 6
7 8 9
```

Output:

```text
[
  [1],
  [2, 4],
  [3, 5, 7],
  [6, 8],
  [9]
]
```

[InterviewBit](https://www.interviewbit.com/problems/anti-diagonals/)

## SOLUTION

```java
public class Solution {

    public ArrayList<ArrayList<Integer>> diagonal(ArrayList<ArrayList<Integer>> A) {

        int n = A.size();

        ArrayList<ArrayList<Integer>> diagonals = new ArrayList<>();

        // Diagonals starting from row 0
        for(int i = 0; i < n; i++){

            int r = 0;  // Start from the first row
            int c = i;  // The current diagonal will start from column at index i

            ArrayList<Integer> diag = new ArrayList<>();

            // The next position will be A[r++][c--]
            while(c >= 0 && r < n){
                diag.add(A.get(r).get(c));
                r++;
                c--;
            }

            diagonals.add(diag);

        }

        // Diagonals starting from last column (except topmost)
        // The diagonal starting from top right corner was already covered in the previous loop
        for(int i = 1; i < n; i++){

            int r = i;  // The diagonal will start from row at index i
            int c = n - 1;  // The diagonal will start from the last column

            ArrayList<Integer> diag = new ArrayList<>();

            // // The next position will be A[r++][c--]
            while(c >= 0 && r < n){
                diag.add(A.get(r).get(c));
                r++;
                c--;
            }

            diagonals.add(diag);

        }

        return diagonals;

    }
}
```
