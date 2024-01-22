---
layout: post
title: Next Permutation (InterviewBit)
date: 2024-01-22 20:56 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays important"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Implement the next permutation, which rearranges numbers into the numerically next greater permutation of numbers for a given array `A` of size `N`.

If such an arrangement is not possible, it must be rearranged in the lowest possible order i.e., sorted in ascending order.

**Note:**

- The replacement must be in-place, do not allocate extra memory.
- DO NOT USE LIBRARY FUNCTION FOR NEXT PERMUTATION. Use of Library functions will disqualify your submission retroactively and will give you penalty points.

[InterviewBit](https://www.interviewbit.com/problems/next-permutation/)

## SOLUTION

[Good Explanation by takeUforward on YouTube](https://www.youtube.com/watch?v=JDOXKqF60RQ)

**Main Steps:**

> A = 1 3 2 6 5 4

If we fix `1 3 2` and try to form a larger number, it will not be possible. Why? Notice that to form a larger number, we need some number which is larger than 6 in the right part but we don't have any.

> A = 1 3 2 6 7 4

For example, if we have the above and fixed `1 3 2`, we would have been able to make a larger number by placing 7 before 6.

So, the first step is to find a `pivot`.  
To find this, we can start from the right and check if `A[i] < A[i+1]`.

In `A = 1 3 2 6 5 4`, the `pivot` will be at `index 2`.

So, we can actually fixed `1 3` and we need the next permutation which is the next largest number. We need a number to replace the number at `pivot` index. We can replace it with 4, 5 or 6. How do we decide that? We are looking for the next mimumum number. So, it would make sense to choose a number which is more than `A[pivot]` but least amongst the possible candidates.

We already know that the elements on the right of pivot are in non-decreasing order. Some of them can be less than A[pivot]. So, we start from index n-1 and look for a number which is more than A[pivot]. This will be the number which we want to use. In this case, it will be `4`, at `idx` at `n-1`.

We can swap the numbers at `pivot` and `idx`. The subarray on the right will still stay in non-decreasing order since `A[pivot] < A[idx]`.

To form the minimum number, just reverse the sub-array from `pivot+1` to `n-1`.

**In Short:**

- Get the first pivot element from right (`A[i] < A[i+1]`)
- If pivot is not found (decreasing order array), reverse the given array and return it (edge case)
- After pivot is found, from end of array, get the index `idx` of an element which is just larger than `A[pivot]`.
- swap elements at `pivot` and `idx`.
- Reverse elements between `pivot+1` to `n-1`.
- Return the array as answer

```java
public class Solution {

    public int[] nextPermutation(int[] A) {

        int n = A.length;

        // Get the first pivot element from right (A[i] < A[i+1])
        int pivot = getPivot(A);

        // If pivot is not found, array is decreasing order like [4 3 2 1]
        // Reverse the array and return it as the ans
        if(pivot == -1){
            reverse(A, 0, n-1);
            return A;
        }

        // Get the index of element from right which is more than A[pivot]
        int closestLargestElementIndex = getClosestLargestElementIndex(A, pivot);

        // Swap element at pivot and closestLargestElementIndex(idx)
        swap(A, pivot, closestLargestElementIndex);

        // Reverse array from pivot+1 till then
        reverse(A, pivot+1, n-1);

        return A;

    }

    public int getClosestLargestElementIndex(int[] A, int pivot){

        int n = A.length;

        for(int i=n-1; i>pivot; i--){
            if(A[i] > A[pivot]){
                return i;
            }
        }

        return -1;

    }

    public void reverse(int[] A, int i, int j){
        while(i<j){
            swap(A, i, j);
            i++;
            j--;
        }
    }

    public int getPivot(int[] A){

        int n = A.length;

        for(int i=n-2; i>=0; i--){
            if(A[i] < A[i+1]) return i;
        }

        return -1;

    }

    public void swap(int[] A, int i, int j){
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }

}
```
