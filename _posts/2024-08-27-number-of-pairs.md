---
layout: post
title: Number of pairs (geeksforgeeks - SDE Sheet)
date: 2024-08-27 23:02 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two positive integer arrays arr and brr, find the number of pairs such that xy > yx (raised to power of) where x is an element from arr and y is an element from brr.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/number-of-pairs-1587115620/1?page=3)

## SOLUTION

### INTUITION

> if (x < y)  
> then x^y > y^x

But, there are some **exceptions**. The following cheat sheet will help in remembering them:

### CHEAT SHEET

> x = 0 -> skip  
> x = 1 -> only valid if y = 0 => so add freq[0]  
> Add general condition: x < y (find count of large elements using binary search)  
> x = 2 -> not valid if y = 3 or y = 4 => so reduce freq[3] and freq[4]  
> x = 3 -> valid if y = 2 even though x > y => so add freq[2]

```java
class Solution {

    public long countPairs(int x[], int y[], int m, int n) {

        // Sort the array y to use binary search later for finding the count of elements greater than a given x
        Arrays.sort(y);

        // Store the frequency of 0, 1, 2, 3, 4 in array y
        // This will be used to handle special cases (exceptions) for these values of y
        int[] freq = new int[5];

        for(int i=0; i<n; i++){
            if(y[i] < 5)
                freq[y[i]]++;
        }

        long pairs = 0;

        for(int i=0; i<m; i++){

            int currentX = x[i];

            // x^y > y^x
            // => 0^y > y^0
            // => 0 > 0
            // not possible for any value of y
            if(currentX == 0)
                continue;

            // x^y > y^x
            // => 1 > y
            // only possible when y is 0
            if(currentX == 1){
                pairs += freq[0];
                continue;
            }

            // In general ->
            // x^y > y^x
            // => y > x (for most case, with some exceptions)
            long greater = countLargeElements(y, n, currentX);
            pairs += greater;

            // What if, y = 0:
            // => x^y > y^x (say)
            // => x^0 > 0^x
            // => 1 > 0 -> true!
            // This means, if y is 0, any value of x will also form a valid pair.
            // Similarly, y = 1:
            // => x^y > y^x (say)
            // => x^1 > 1^x
            // => x > 1 -> true for x > 1 (we have already handled x = 0 and x = 1 in previous conditions)
            pairs += freq[0] + freq[1];

            // more exceptions:
            // x = 2, y = 3 => not valid pair
            // x = 2, y = 4 => not valid pair
            // we had added them earlier because x < y
            // so, make this correction
            if(currentX == 2){
                pairs -= (freq[3] + freq[4]);

            // x = 3, y = 2 => valid pair, although x > y
            }else if(currentX == 3){
                pairs += freq[2];
            }


        }

        return pairs;

    }

    // binary search to get the count of number of elements greater than x
    public long countLargeElements(int[] arr, int n, int x){

        int l = 0;
        int r = n - 1;

        while(l<=r){

            int m = l + (r - l) / 2;

            if(x >= arr[m]){
                l = m + 1;
            }else
                r = m-1;

        }

        return  n - l;
    }

}
```
