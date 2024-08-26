---
layout: post
title: Find All Four Sum Numbers
date: 2024-08-26 15:57 +0530
author: "Gaurav Kumar"
tags: "java arrays geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array `A` of integers and another number `K`. Find all the unique quadruple from the given array that sums up to `K`.

Also note that all the quadruples which you return should be internally sorted, ie for any quadruple `[q1, q2, q3, q4]` the following should follow: `q1 <= q2 <= q3 <= q4`.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/find-all-four-sum-numbers1732/1?page=3)

## SOLUTION

First sort the array in ascending order.

Fix first two elements using two for loops. For both of them, add a logic to skip same elements.

Take two pointers, `l = j+1` and `r=n-1`;

Look for a quadruple such that `k = arr[i] + arr[j] + arr[l] + arr[r]`

If we get it, add to answer. Skip duplicates for both `l` and `r` and go to the next step or elements.

If the sum is less, we need to increase left pointer.
If the sum is more, we need to reduce the right pointer.

```java

class Solution {

    public ArrayList<ArrayList<Integer>> fourSum(int[] arr, int k) {

        int n = arr.length;

        Arrays.sort(arr);

        ArrayList<ArrayList<Integer>> res = new ArrayList<>();

        for(int i=0; i<n-3; i++){

            // skip duplicate for first element
            if (i > 0 && arr[i] == arr[i - 1]) continue;

            for(int j=i+1; j<n-2; j++){

                // skip duplicate for second element
                if (j > i + 1 && arr[j] == arr[j - 1]) continue;

                int l = j+1;
                int r = n-1;

                while(l < r){

                    int sum = arr[i] + arr[j] + arr[l] + arr[r];

                    if(sum == k){

                        res.add(new ArrayList<>(Arrays.asList(arr[i], arr[j], arr[l], arr[r])));

                        // skip duplicate at l
                        while(l < r && arr[l+1] == arr[l])
                            l++;

                        // skip duplicate at r
                        while(l < r && arr[r-1] == arr[r])
                            r--;

                       l++; r--;

                    }else if(sum < k){
                        l++;
                    }else
                        r--;

                }

            }

        }

        return res;

    }

}
```
