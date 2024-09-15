---
layout: post
title: Next Greater Element (geeksforgeeks - SDE Sheet)
date: 2024-09-15 14:32 +0530
author: "Gaurav Kumar"
tags: "java stack geeksforgeeks sde-shee important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an array arr[ ] of n elements, the task is to find the next greater element for each element of the array in order of their appearance in the array. Next greater element of an element in the array is the nearest element on the right which is greater than the current element.
If there does not exist next greater of current element, then next greater element for current element is -1. For example, next greater of the last element is always -1.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/next-larger-element-1587115620/1?page=7)

## SOLUTION

We can make use of stack. The idea is to traverse the array while maintaining a stack that holds pairs of elements and their indices. For each element in the array, we compare it with the element at the top of the stack. If the current element is greater, it means we've found the next greater element for the top element of the stack, so we update the result array and remove the top of the stack.

This process is repeated until the current element is no longer greater than the stack's top, at which point we push the current element onto the stack to check its next greater element in subsequent iterations.

```java
class Solution
{

    public static long[] nextLargerElement(long[] arr, int n)
    {

        long[] res = new long[n];

        // Filling the result array with -1 as a default value.
        // This is because if there is no greater element to the right,
        // the result for that element will remain -1.
        Arrays.fill(res, -1);

        // Stack to keep track of elements and their indices in the array.
        // Each element in the stack will be stored as a pair [element, index].
        Stack<long[]> stack = new Stack<>();

        // Pushing the first element along with its index into the stack.
        stack.push(new long[]{arr[0], 0});

        // Iterating through the array starting from the second element.
        for(int i = 1; i < n; i++) {

            // While the stack is not empty and the current element is greater than
            // the element on the top of the stack, it means we found the next
            // greater element for the element at stack.peek().
            while(!stack.isEmpty() && arr[i] > stack.peek()[0]){
                // Update the result array for the index at the top of the stack
                // with the current element, since it is the next greater element.
                res[(int)stack.peek()[1]] = arr[i];

                // Pop the top element as we've now found its next greater element.
                stack.pop();
            }

            // Push the current element and its index into the stack.
            // This will help in finding its next greater element later.
            stack.push(new long[]{arr[i], i});
        }

        return res;
    }
}
```
