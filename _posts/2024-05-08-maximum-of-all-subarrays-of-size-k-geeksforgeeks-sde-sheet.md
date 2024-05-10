---
layout: post
title: Maximum of all subarrays of size k (geeksforgeeks - SDE Sheet)
date: 2024-05-08 22:12 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks arrays important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array arr[] of size N and an integer K. Find the maximum for each and every contiguous subarray of size K.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/maximum-of-all-subarrays-of-size-k3101/1?page=1)

## SOLUTION

### APPROACH 1 - USING MAX HEAP (TLE)

We will add the first k elements of the array to a max heap. The max will at the top of the max heap. So, we can keep add the top elements to an answer list. For the next iteration (next window of k elements), remove the left most element of previous window and add right most element of current window. Keep doing this for all windows.

This is not an optimal solution since removing from heap has higher time complexity (needs to do heapify).

```java
class Solution
{
    static ArrayList <Integer> max_of_subarrays(int arr[], int n, int k)
    {

        // max heap
        PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());

        ArrayList<Integer> res = new ArrayList<>();

        for(int i=0; i<k; i++){
            pq.add(arr[i]);
        }

        res.add(pq.peek());

        int idx = k;

        while(idx<n){

            pq.add(arr[idx]);

            res.add(pq.peek());

            idx++;

        }

        return res;

    }
}
```

### APPROACH 2 - USING DEQUEUE

[Good Explanation by takeUForward](https://www.youtube.com/watch?v=CZQGRp93K4k)

The main trick is to use a `Dequeue` - A dequeue (`double-ended queue`) is a data structure that allows insertion and deletion of elements from both ends.

We will keep adding elements on the right side, **while maintaining a decreasing order in the dequeue**.

Let us say that the current elements are: [3, 2] in the queue. If we get a 4, there is no chance that the previous elements will be the maximum for that window. There is no benefit in keeping those elements. So, we keep removing them one by one if they are smaller than the current element, which is 4 in our case. So, we will just have [4] in the dequeue after this operation.

Now, let us say that the next element is 1. This can be a potential answer for future window. So, we should consider this and add it to the dequeue. So, the state will be [4, 1].

Instead of storing the elements itself, it would be better to store the indices. That way, we can check if the left most element is within the window-range. If not, remove the left most element from the dequeue.

```java
class Solution
{
    static ArrayList <Integer> max_of_subarrays(int arr[], int n, int k)
    {

        ArrayList<Integer> res = new ArrayList<>();

        // deque to maintain elements in decreasing order
        Deque<Integer> deque = new LinkedList<>();

        int idx = 0;

        // while there are more elements left
        while(idx<n){

            // if dequeue is not empty and the current element is more than last element in deque, remove the last element from deque
            // we are doing this to maintain the decreasing order
            while(deque.size() > 0 && arr[deque.getLast()] < arr[idx])
                deque.removeLast();

            // add the current element
            deque.addLast(idx);

            // if the left most element is outside of window-range, remove it
            if(deque.getFirst() == idx - k)
                deque.removeFirst();

            // the window will start after k elements have been considered
            if(idx >= k-1)
                res.add(arr[deque.getFirst()]);

            idx++;

        }

        return res;


    }
}
```
