---
layout: post
title: Find median in a stream (geeksforgeeks - SDE Sheet)
date: 2024-09-16 22:02 +0530
author: "Gaurav Kumar"
tags: "java heap geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an input stream of N integers. The task is to insert these numbers into a new stream and find the median of the stream formed by each insertion of X to the new stream.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/find-median-in-a-stream-1587115620/1?page=7)

## SOLUTION

We can solve this by using two heaps: a max-heap to store the smaller half of the numbers and a min-heap to store the larger half. Each time we insert a new number, we add it to the appropriate heap and balance the heaps to ensure their sizes stay nearly equal. The median is then calculated based on the sizes of the heaps: if both heaps have the same size, the median is the average of the top elements from both heaps; if one heap is larger, the median is the top element of that heap.

See this for detailed explanation:

[Find Median from Data Stream]({% post_url 2022-10-18-find-median-from-data-stream %})

```java
class Solution {

    // Two priority queues (heaps) to maintain the two halves of the numbers seen so far.
    // maxHeap will store the smaller half of the numbers (maximum element at the top).
    // minHeap will store the larger half of the numbers (minimum element at the top).
    static Queue<Integer> maxHeap;
    static Queue<Integer> minHeap;

    Solution() {
        maxHeap = new PriorityQueue<Integer>(Collections.reverseOrder());
        minHeap = new PriorityQueue<Integer>();
    }

    // Function to insert a new number into the appropriate heap.
    public static void insertHeap(int x) {

        // If maxHeap is empty, directly add the number to maxHeap.
        if (maxHeap.isEmpty()) {
            maxHeap.add(x);
        } else {
            // If the number is greater than the top of maxHeap, it belongs in minHeap.
            if (x > maxHeap.peek()) {
                minHeap.add(x);
            } else {
                // Otherwise, add it to maxHeap.
                maxHeap.add(x);
            }
        }

        // Balance the heaps after the insertion.
        balanceHeaps();
    }

    // Function to balance the two heaps.
    public static void balanceHeaps() {

        // If maxHeap has more than 1 extra element compared to minHeap, move the top element of maxHeap to minHeap.
        if (maxHeap.size() - minHeap.size() > 1) {
            minHeap.add(maxHeap.poll());
        }
        // If minHeap has more elements than maxHeap, move the top element of minHeap to maxHeap.
        else if (minHeap.size() - maxHeap.size() > 0) {
            maxHeap.add(minHeap.poll());
        }
    }

    // Function to return the median of the current stream.
    public static double getMedian() {

        // Calculate the total number of elements in both heaps.
        int n = maxHeap.size() + minHeap.size();

        // If the total number of elements is even, the median is the average of the top elements from both heaps.
        if (n % 2 == 0) {
            return (maxHeap.peek() + minHeap.peek()) / 2.0;
        } else {
            // If the total number of elements is odd, the median is the top element of maxHeap.
            return maxHeap.peek() / 1.0;
        }
    }
}
```
