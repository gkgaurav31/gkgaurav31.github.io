---
layout: post
title: Kth element in Matrix (geeksforgeeks - SDE Sheet)
date: 2024-08-30 23:00 +0530
author: "Gaurav Kumar"
tags: "java binary_search geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a `N x N` matrix, where every row and column is sorted in non-decreasing order. Find the kth smallest element in the matrix.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/kth-element-in-matrix/1?page=4)

## SOLUTION

### APPROACH 1 - MAX HEAP (ACCEPTED)

Iterate all the elements in the array and keep adding them to max heap. If max heap size is > k, remove the top element. At the end, the max heap size will be k, and the top most element is the answer.

```java
class Solution
{
    public static int kthSmallest(int[][]mat,int n,int k)
    {

        // max heap
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());

        for(int i=0; i<n; i++){
            for(int j=0; j<n; j++){

                pq.add(mat[i][j]);

                if(pq.size() > k)
                    pq.poll();

            }
        }

        return pq.peek();

    }

}
```

### APPROACH 2 (BINARY SEARCH)

Range of answers:  
`[minElement in the array, maxElement in the array]`
== `(mat[0, 0], mat[n-1][n-1])`

Apply binary search on the range. Let's say mid is `m`. Check if that can be the valid answer. We do this by checking how many numbers are there which are `<= m` in the matrix. If it's `>= k`, it's possible candidate, but try for lower values;

```java
class Solution
{
    public static int kthSmallest(int[][]mat,int n,int k)
    {

        // The ans will be between smallest and the largest element in the matrix
        int l = mat[0][0];
        int r = mat[n-1][n-1];

        int res = -1;

        while(l<=r){

            int m = l + (r-l) / 2;

            int countSmaller = countOfSmallerOrEqualsElements(mat, n, m);

            // NEED TO CHECK WHY >= is used
            // problematic case: [7 17 27 36 38 14 23 35 38 43 19 26 42 49 50 23 33 48 52 53 30 40 52 56 64]
            // k = 13
            // required ans: 38
            if(countSmaller >= k){
                res = m;
                r = m-1;
            }else if(countSmaller < k){
                l = m+1;
            }

        }

        return res;

    }

    // count the number of nums in matrix which are <= m
    public static int countOfSmallerOrEqualsElements(int[][] mat, int n, int m){

        int count = 0;

        for(int i=0; i<n; i++){

            for(int j=0; j<n; j++){

                if(mat[i][j] <= m){
                    count++;
                }else
                    break;

            }

        }

        return count;

    }

}
```

### APPROACH 3 (DOUBLE BINARY SEARCH)

In the previous approach, we traversed the complete matrix to check how many numbers are <= mid. Since we know that the numbers in each row/column are sorted, we can make use of binary search to find this as well!

```java
class Solution
{
    public static int kthSmallest(int[][]mat,int n,int k)
    {

        // The ans will be between smallest and the largest element in the matrix
        int l = mat[0][0];
        int r = mat[n-1][n-1];

        int res = -1;

        while(l<=r){

            int m = l + (r-l) / 2;

            int countSmaller = countOfSmallerOrEqualsElements(mat, n, m);

            if(countSmaller >= k){
                res = m;
                r = m-1;
            }else if(countSmaller < k){
                l = m+1;
            }

        }

        return res;

    }

    // count the number of nums in matrix which are <= m
    public static int countOfSmallerOrEqualsElements(int[][] mat, int n, int m){

        int count = 0;

        for(int i=0; i<n; i++){

            // When the key is present: Arrays.binarySearch returns the index of the key.
            // When the key is not present: returns a negative value. This negative value is calculated as -(insertion point) - 1.
            // Ref: https://www.freecodecamp.org/news/how-to-use-arrays-binarysearch-in-java/
            int index = Arrays.binarySearch(mat[i], m);

            if(index < 0)
                index = -index - 1;
            else
                index = index + 1; // including the element itself

            count += index;

        }

        return count;

    }

}
```
