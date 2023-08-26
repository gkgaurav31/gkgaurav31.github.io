---
layout: post
title: Find K Pairs with Smallest Sums
date: 2023-08-26 11:47 +0530
author: "Gaurav Kumar"
tags: "java heap leetcode leetcode150 important"
categories: "heap"
---

## PROBLEM DESCRIPTION

You are given two integer arrays nums1 and nums2 sorted in non-decreasing order and an integer k.
Define a pair (u, v) which consists of one element from the first array and one element from the second array.
Return the k pairs (u1, v1), (u2, v2), ..., (uk, vk) with the smallest sums.

[leetcode](https://leetcode.com/problems/find-k-pairs-with-smallest-sums/)

## SOLUTION

- Since the two arrays are sorted, the first pair is definitely (nums1[0], nums2[0])
- Let's say i=0, j=0 here
- Next, we could have i=0, j=1 OR i=1, j=0
- Let's say, we choose i=0, j=1
- The next move now can be either the left over i=1, j=0 OR the next moves from previous pick which are: i=0,j=2 OR i=1, j=1
- To achieve this, we will use a min heap. The min heap will have an int array in the form: [sum of the pair, index in nums1, index in nums2]. The min heap will be "sorted" based on the "sum of the pair".
- We can start by adding [nums1[0]+nums2[0], 0, 0] because the arrays are sorted.
- Next, we get the top most element from the heap. Get the corresponding index which will be i=0, j=0. We push the next possible moves to the min heap. So, we will push (nums1[0]+nums2[1], 0, 1) and also (nums1[1]+nums2[0], 1, 0). Note that, the min heap already has the previous left over. So, next time we pop an element from min heap, it is going to take into consideration all the possible moves and pick the one will min sum (because we are using min heap based on sum).
- One important thing to also observe is that, we can get into duplicate moves. Example: (0, 1) can go to (1, 1) and (1, 0) can also go to (1, 1) as the next move. So, to avoid that, we can make use of a HashSet to track the pairs which have been visited already.
- The last thing to keep in mind is that the value of k in the problem can be lesser than the number of possible pairs. In that case, the min heap will eventually become empty before k becomes 0 in the while loop. So just need to return all possible pairs sorted by their sum in that case.

```java
class Solution {

    public List<List<Integer>> kSmallestPairs(int[] nums1, int[] nums2, int k) {

        int n = nums1.length;
        int m = nums2.length;

        // answer list
        List<List<Integer>> ans = new ArrayList<>();

        // min heap
        // let's say we are looking at a pair at index i in nums1 and index j in nums2:
        // the element will be added to the min heap like this: [ nums1[i]+nums2[i] , i, j ]
        // using the comparator, the min heap will be "sorted" based on the first element of this array, which is the sum of pair of numbers
        // (a[0] - b[0]) calculates the difference between the first elements of these arrays. The comparison is based on this difference.
        // If the result is negative, it means a comes before b, and if it's positive, it means b comes before a. 0 means equal.
        PriorityQueue<int[]> pq = new PriorityQueue<>((a,b) -> a[0] - b[0]);

        // we would obviously start with nums1[0] and nums2[0] because the given arrays are sorted
        // after that the next moves could be (nums1[0], nums2[1]) OR (nums1[1], nums2[0])
        // both of these will be pushed to the min heap and the one with lower sum will be at the top
        // at some point, we would be pushing: (nums1[0], nums2[1]) -> (nums1[1], nums2[1])
        // and
        // at some point, we would be pushing: (nums1[1], nums2[0]) -> (nums1[1], nums2[1])
        // notice that this is a duplicate move. To avoid this, we will use a visited hashset
        // after adding any pair to the min heap, we will mark it as visited
        Set<Pair<Integer, Integer>> visited = new HashSet<>();

        // init: the first elements from both the arrays will be the smallest for sure
        pq.offer( new int[]{nums1[0]+nums2[0], 0, 0} );
        // also, mark it as visited
        visited.add(new Pair(0, 0));

        // until we have got k elements
        // it might also be possible that there are lesser pairs than k, in which case the min heap would become empty
        // example: nums1 = [1,2], nums2 = [3], k = 3
        // ans: [[1,3],[2,3]], only two pairs possible
        while(k > 0 && !pq.isEmpty()){

            k--;

            //get the top element from min heap which will have the lowest sum currently
            int[] current = pq.poll();

            //get the corresponding indices for nums1 and nums2
            int i = current[1];
            int j = current[2];

            //add this to the answer
            ans.add(List.of(nums1[i], nums2[j]));

            // the next element can be at: nums1[i+1], nums2[j]
            // i+1 can be out of bounds, so ensure that we check that first
            // also, it must not have been visited before
            if(i+1 < n && !visited.contains(new Pair(i+1, j))){
                pq.offer(new int[] {nums1[i+1]+nums2[j], i+1, j});
                visited.add(new Pair(i+1, j));
            }

            // the next element can be at: nums1[i], nums2[j+1]
            // j+1 can be out of bounds, so check for that
            // also, it must not have been visited before
            if(j+1 < m && !visited.contains(new Pair(i, j+1))){
                pq.offer(new int[] {nums1[i]+nums2[j+1], i, j+1});
                visited.add(new Pair(i, j+1));
            }

        }

        return ans;

    }

}
```
