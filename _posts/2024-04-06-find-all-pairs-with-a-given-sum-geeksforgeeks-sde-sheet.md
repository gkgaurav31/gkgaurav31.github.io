---
layout: post
title: Find all pairs with a given sum (geeksforgeeks - SDE Sheet)
date: 2024-04-06 17:14 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two unsorted arrays `A` of size `N` and `B` of size `M` of distinct elements, the task is to find all pairs from both arrays whose sum is equal to `X`.

**Note:** All pairs should be printed in increasing order of `u`. For eg. for two pairs `(u1,v1)` and `(u2,v2)`, if `u1` < `u2` then
`(u1,v1)` should be printed first else second.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/find-all-pairs-whose-sum-is-x5808/1?page=1)

## SOLUTION

```java
class Solution {

    public pair[] allPairs(long A[], long B[], long N, long M, long X) {

        // Create a HashSet to store elements from array A
        Set<Long> set = new HashSet<>();

        // Add elements from array A to the HashSet
        for(int i=0; i<N; i++)
            set.add(A[i]);

        // Create an ArrayList to store pairs that satisfy the condition
        List<pair> list = new ArrayList<>();

        // Iterate through array B
        for(int i=0; i<M; i++){

            // Check if there exists an element in array A such that their sum equals X
            if(set.contains(X-B[i])){
                // If such an element exists, add the pair (X-B[i], B[i]) to the list
                list.add(new pair(X-B[i], B[i]));
            }
        }

        // Convert the list to an array of pairs
        pair[] res = new pair[list.size()];
        for(int i=0; i<list.size(); i++)
            res[i] = list.get(i);

        // Sort the array of pairs in increasing order of the first element of each pair
        Arrays.sort(res, (a, b) -> (int) a.first - (int) b.first);

        return res;
    }

}
```

## ANOTHER WAY TO CODE

```java
class Solution {

    public pair[] allPairs(int x, int arr1[], int arr2[]) {

        int n = arr1.length;
        int m = arr2.length;

        // create set using arr2
        Set<Integer> set = new HashSet<>();
        for(int i=0; i<m; i++)
            set.add(arr2[i]);

        // list of pairs with sum x
        List<pair> list = new ArrayList<>();

        // sort the first array so that we always get the smallest first element
        Arrays.sort(arr1);

        // iterate over the first array
        for(int i=0; i<n; i++){

            // avoid repeating elements
            // if we remove this, it still gets accepted as a solution in gfg
            // i think it's because all elements are unique in the test cases
            if(i > 0 && arr1[i] == arr1[i-1])
                continue;

            // first element candidate
            int a = arr1[i];

            // second element needed
            int b = x-a;

            // check if second element is present in the set (which has elements of arr2)
            if(set.contains(b)){

                // if it does, add this pair to the result
                // since we sorted arr1, `a` will be the least already
                list.add(new pair(a, b));
            }

        }

        // covert to array of pairs
        pair[] res = new pair[list.size()];
        for(int i=0; i<res.length; i++)
            res[i] = list.get(i);

        return res;

    }

}
```

## UPDATE

The requirement in this problem seems to have changed. So the previous solution does not seem to work any longer. The difference is that, if the second array contains duplicates, we need to consider them multiple times as well. This can easily be incorporated by storing the frequency of numbers in the second array using HashMap (instead of using HashSet like before).

```java
class Solution {

    public pair[] allPairs(int x, int arr1[], int arr2[]) {

        int n = arr1.length;
        int m = arr2.length;

        Map<Integer, Integer> map = new HashMap<>();

        for(int i=0; i<m; i++)
            map.put(arr2[i], map.getOrDefault(arr2[i], 0) + 1);

        List<pair> list = new ArrayList<>();

        Arrays.sort(arr1);

        for(int i=0; i<n; i++){

            int a = arr1[i];
            int b = x-a;

            if(map.containsKey(b)){

                int times = map.get(b);

                for(int k=0; k<times; k++){
                    list.add(new pair(a, b));
                }

            }

        }

        pair[] res = new pair[list.size()];
        for(int i=0; i<res.length; i++)
            res[i] = list.get(i);

        return res;

    }

}
```
