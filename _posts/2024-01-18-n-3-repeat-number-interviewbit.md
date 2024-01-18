---
layout: post
title: N/3 Repeat Number (InterviewBit)
date: 2024-01-18 21:56 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You're given a read-only array of `N` integers. Find out if any integer occurs more than `N/3` times in the array in linear time and constant additional space.

If so, return the integer. If not, return `-1`.

If there are multiple solutions, return any one.

[InterviewBit](https://www.interviewbit.com/problems/n3-repeat-number/)

## SOLUTION

### APPROACH 1 - SORTING

Once the list is sorted, it becomes easier to calculate the count of each number. If it becomes more than `N/3` return it as the answer.

```java
public class Solution {

    public int repeatedNumber(final List<Integer> a) {

        int n = a.size();

        // Sort the array to group identical elements together
        Collections.sort(a);

        // minimum count needed for an element to be considered as the majority
        int countNeeded = n/3;

        // Initialize index, count, and previous element
        int idx = 1;          // Current index while traversing the array
        int count = 1;        // Counter for the current element's occurrences
        int prev = a.get(0);   // Previous element

        // If the first element occurs more than N/3 times, return it as the majority
        // edge case example: [1]
        if(count > countNeeded) return a.get(0);

        // Iterate through the sorted array
        while(idx < n){

            // If the current element is the same as the previous one, increment its count
            if(a.get(idx) == prev){
                count++;
            } else {
                // If the current element is different, update the previous element and reset the count
                prev = a.get(idx);
                count = 1;
            }

            // If the count of the current element exceeds the required count, return it as the majority
            if(count > countNeeded) return a.get(idx);

            // Move to the next element in the array
            idx++;

        }

        // If no majority element is found, return -1
        return -1;
    }
}
```

### APPROACH 2 - BOYER-MOORE MAJORITY VOTING ALGORITHM

[BOYER-MOORE MAJORITY VOTING ALGORITHM
](https://www.geeksforgeeks.org/boyer-moore-majority-voting-algorithm/)

> **MAIN IDEA**: If we have 3 different elements from an array at a given time and remove them, the result remains unchanged.

As we go through the array, we keep track of 2 elements and their counts. When encountering a new element:

1. If we don't have 2 elements yet, we add this one with a count of 1.
2. If the new element is different from the existing 2, we decrement the count of the 2 elements. If the count reaches 0, it's no longer part of the 2 elements.
3. If the new element is the same as one of the 2, we increment the count of that element.

The final result will be one of the 2 elements left. If they are not the answer, then there is no element with a count greater than N/3.

```java
public class Solution {

    public int repeatedNumber(final List<Integer> a) {

        int n = a.size();

        // Variables to track two potential candidates and their counts
        int e1 = 0; // element 1
        int e2 = 0; // element 2
        int c1 = 0; // count of element 1
        int c2 = 0; // count of element 2

        for (int i = 0; i < n; i++) {

            // if current element matches first element, increase its count
            if (a.get(i) == e1) {
                c1++;

            // if current element matches second element, increase its count
            } else if (a.get(i) == e2) {
                c2++;

            // current element did not match e1 and e2 both
            // if count is 0 for e1, assign the current number to it and reset the counter
            } else if (c1 == 0) {
                e1 = a.get(i);
                c1 = 1;

            // current element did not match e1 and e2 both
            // but count for e1 was > 0
            // if count of e2 is 0, assign the current element to it and reset the counter
            } else if (c2 == 0) {
                e2 = a.get(i);
                c2 = 1;

            // the current element was different
            // both e1 and e2 had more than 0 count
            // so, we reduce the count for both e1 and e2
            // main idea is, if we have 3 different elements from an array at a given time and remove them, the result remains unchanged.
            } else {
                c1--;
                c2--;
            }
        }

        int countElement1 = 0;
        int countElement2 = 0;

        // Count occurrences of the candidates in the original list
        for (int i = 0; i < n; i++) {
            if (a.get(i) == e1) {
                countElement1++;
            } else if (a.get(i) == e2) {
                countElement2++;
            }
        }

        // Check if any candidate has count greater than N/3
        int countForAnswer = n / 3;
        if (countElement1 > countForAnswer) {
            return e1;
        } else if (countElement2 > countForAnswer) {
            return e2;
        }

        // No majority element found
        return -1;
    }
}

```
