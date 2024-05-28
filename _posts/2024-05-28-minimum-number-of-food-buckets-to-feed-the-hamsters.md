---
layout: post
title: Minimum Number of Food Buckets to Feed the Hamsters
date: 2024-05-28 21:30 +0530
author: "Gaurav Kumar"
tags: "java greedy leetcode microsoft important"
categories: "greedy"
---

## Problem Description

You are given a 0-indexed string hamsters where hamsters[i] is either:

- 'H' indicating that there is a hamster at index i, or
- '.' indicating that index i is empty.

You will add some number of food buckets at the empty indices in order to feed the hamsters. A hamster can be fed if there is at least one food bucket to its left or to its right. More formally, a hamster at index i can be fed if you place a food bucket at index i - 1 and/or at index i + 1.

Return the minimum number of food buckets you should place at empty indices to feed all the hamsters or -1 if it is impossible to feed all of them.

## Solution

[Good Explanation](https://www.youtube.com/watch?v=jvPUA3QpR5A)

```java
class Solution {

    public int minimumBuckets(String hamsters) {

        int n = hamsters.length();
        int res = 0;

        int i = 0;

        while(i<n){

            // get current character
            char current = hamsters.charAt(i);

            // if the current place has hamster
            if(current == 'H'){

                // try to put the donut on right first (being greedy)
                // we want to first try on the right side because that potentially can help the next hamster also
                if(i+1 < n && hamsters.charAt(i+1) == '.'){
                    res += 1;

                    // IMPORTANT:
                    // since we have put a donut of right, it can feed the next hamster if present
                    // so, we can actually skip these two positions
                    // example:
                    // _ H _ H H _
                    // 0 1 2 3 4 5
                    // at index 1, there is a hamster, we try to put a donut on its right first
                    // since index 2 is empty, we can put a donut there
                    // that means, if there is any hamster on index 3, it will be okay
                    // we do not need to check this position and we can skip it
                    // so, we can directly go to index 4
                    // we are adding 2 here instead of 3 since are incrementing by 1 later on in the while loop
                    i+=2;
                // if we did not have empty space on right, check left
                }else if(i-1 >= 0 && hamsters.charAt(i-1) == '.'){
                    res += 1;
                // if both left and right are not empty, it is not possible to feed the current hamster
                }else
                    return -1;

            }

            i++;
        }

        return res;

    }

}
```
