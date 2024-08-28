---
layout: post
title: Merge Without Extra Space
date: 2024-08-28 19:51 +0530
author: "Gaurav Kumar"
tags: "java arrays sorting geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two sorted arrays arr1[] and arr2[] of sizes n and m in non-decreasing order. Merge them in sorted order without using any extra space. Modify arr1 so that it contains the first N elements and modify arr2 so that it contains the last M elements.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/merge-two-sorted-arrays-1587115620/1?page=3)

## SOLUTION

### INITIAL APPROACH (ACCEPTED)

```java
class Solution
{

    public static void merge(long arr1[], long arr2[], int n, int m)
    {

        // end of arr1
        int i = n-1;
        // start of arr2
        int j = 0;

        // keep comparing the pairs and swap when necessary
        while(i >= 0 && j < m){

            // if end element in arr1 is larger than start element at arr2
            // we should swap these elements
            if(arr1[i] > arr2[j]){

                long t = arr1[i];
                arr1[i] = arr2[j];
                arr2[j] = t;
            }

            // check next pair
            i--;
            j++;

        }

        // we will have the first inital numbers in arr1
        // and next elements in arr2
        // but they won't be in sorted order
        // so sort them explicitly
        Arrays.sort(arr1);
        Arrays.sort(arr2);

    }

}
```

### OPTIMIZATION (GAP ALGORITHM)

Instead of using `Arrays.sort()`, we can make use of the **gap algorithm**, which is an optimization of the **Shell Sort** technique, to merge two arrays in sorted order while swapping the elements directly.

#### Steps to Implement the Gap Algorithm

1. Calculate Total Length:

   - `totalLength = n + m` (combined length of both arrays).

2. Initialize the Gap:

   - Set the initial gap as `gap = ceil(totalLength / 2)`. This is calculated as `(totalLength / 2 + totalLength % 2)` to avoid using floating-point arithmetic.

3. Iterate and Compare Elements:

   - Initialize two pointers:
     - `left = 0`
     - `right = gap` (start right pointer at the gap distance from the left pointer).

4. Compare and Swap Elements:

   - Compare elements at positions `left` and `right`. If the element at `left` is larger than the element at `right`, swap them.
   - Move both `left` and `right` to the next elements (`left++` and `right++`), continuing until `right` is within the bounds of the combined arrays.

5. Handle Array Bounds during the above iteration:

   - Case 1: `left < n` and `right < n`
     - Both pointers are in `arr1`: Compare `arr1[left]` with `arr1[right]`.
   - Case 2: `left < n` and `right >= n`
     - `left` is in `arr1` and `right` is in `arr2`: Compare `arr1[left]` with `arr2[right - n]`.
   - Case 3: `left >= n` and `right >= n`
     - Both pointers are in `arr2`: Compare `arr2[left - n]` with `arr2[right - n]`.

6. Update Gap:

   - Reduce the gap using `gap = gap / 2 + gap % 2` to ensure it's the ceiling of the halved gap. If the gap becomes 1, make sure to avoid infinite loops.

7. Repeat Until Gap is Zero:
   - Continue this process, updating the gap and iterating through the arrays until the gap is zero.

```java
class Solution
{
    //Function to merge the arrays.
    public static void merge(long arr1[], long arr2[], int n, int m)
    {

        // length of combined array
        long total = n + m;

        // this is equivalent of  ceil(total/2)
        int gap = (int) total/2 + (int) total%2;

        while(gap > 0){

            // init:
            // left will start from index 0
            // right will start from left + gap
            int left = 0;
            int right = gap;

            // while right is within bounds
            while(right < n + m){

                // left in arr1 and right in arr2
                if(left < n && right >= n){
                    swapIfGreater(arr1, arr2, left, right-n);
                // both in arr2 (if left is on arr2, right has to be on arr2)
                }else if(left >= n){
                    swapIfGreater(arr2, arr2, left-n, right-n);
                // both are in arr1
                }else{
                    swapIfGreater(arr1, arr1, left, right);
                }

                left++;
                right++;

            }

            // if gap is 1, then ceil of (1/2) will be again 1 and it will cause infinite loop
            // so we break if gap reaches 1
            if(gap == 1)
                break;

            // reduce gap by half
            gap = gap/2 + gap%2;

        }

    }

    // swap the elements at position i in arr1 and position j in arr2, if arr1[i] > arr2[j]
    public static void swapIfGreater(long[] arr1, long[] arr2, int i, int j){

        if(arr1[i] > arr2[j]){
            long t = arr1[i];
            arr1[i] = arr2[j];
            arr2[j] = t;
        }

    }


}
```
