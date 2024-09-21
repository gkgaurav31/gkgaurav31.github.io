---
layout: post
title: Page Faults in LRU (geeksforgeeks - SDE Sheet)
date: 2024-09-21 17:13 +0530
author: "Gaurav Kumar"
tags: "java hashing geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

In operating systems that use paging for memory management, page replacement algorithm is needed to decide which page needs to be replaced when the new page comes in. Whenever a new page is referred and is not present in memory, the page fault occurs and Operating System replaces one of the existing pages with a newly needed page.

Given a sequence of pages in an array pages[] of length N and memory capacity C, find the number of page faults using Least Recently Used (LRU) Algorithm.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/page-faults-in-lru5603/1?page=8)

## SOLUTION

We can solve this by using the Least Recently Used (LRU) page replacement algorithm with a Deque and a Set. The Deque keeps track of the pages in memory, placing the most recently used ones at the front. As we go through the page requests, we check if a page is already in memory. If it is, we move it to the front; if not, we count it as a page fault. If memory is full, we remove the least recently used page from the back.

```java
class Solution {

    static int pageFaults(int N, int C, int pages[]) {
        // Deque to keep track of pages currently in memory (most recently used at the front)
        Deque<Integer> memory = new LinkedList<>();
        // Set to allow O(1) lookup for pages in memory
        Set<Integer> pageSet = new HashSet<>();

        int pageFaults = 0; // Counter for the number of page faults

        // Iterate through each page request in the pages array
        for (int i = 0; i < pages.length; i++) {
            int currentPage = pages[i]; // Current page being referenced

            // Check if the current page is already in memory
            if (pageSet.contains(currentPage)) {
                // Page is already in memory; move it to the front (most recently used)
                memory.remove(currentPage); // Remove it from its current position
                memory.addFirst(currentPage); // Add it to the front
            } else {
                // Page fault occurs; the page is not in memory
                pageFaults++; // Increment the page fault counter

                // Check if memory is full
                if (memory.size() == C) {
                    // Remove the least recently used page (last in the deque)
                    int last = memory.removeLast(); // Remove the last page
                    pageSet.remove(last); // Also remove it from the set
                }

                // Add the current page to the front (most recently used)
                memory.addFirst(currentPage);
                pageSet.add(currentPage); // Add the page to the set for future reference
            }
        }

        return pageFaults; // Return the total number of page faults
    }
}
```
