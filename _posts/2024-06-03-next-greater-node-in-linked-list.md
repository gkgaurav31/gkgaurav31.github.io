---
layout: post
title: Next Greater Node In Linked List
date: 2024-06-03 10:42 +0530
author: "Gaurav Kumar"
tags: "java linkedlist leetcode microsoft important"
categories: "linkedlist"
---

## Problem Description

You are given the head of a linked list with n nodes.

For each node in the list, find the value of the next greater node. That is, for each node, find the value of the first node that is next to it and has a strictly larger value than it.

Return an integer array answer where answer[i] is the value of the next greater node of the ith node (1-indexed). If the ith node does not have a next greater node, set answer[i] = 0.

## Solution

![snapshot]({{ site.baseurl }}/assets/img/next_greater_node_linked_list.png)

```java
class Solution {

    public int[] nextLargerNodes(ListNode head) {

        // covert linked list into array list
        List<Integer> list = new ArrayList<>();
        while(head != null){
            list.add(head.val);
            head = head.next;
        }

        int n = list.size();

        // result will be of same size as the list
        int[] res = new int[n];

        // stack to keep a track of elements remaining
        Stack<Integer> stack = new Stack<>();

        for(int i=0; i<n; i++){

            // if stack is empty
            // AND
            // current element is more than the element at index present on top of the stack
            while(!stack.isEmpty() && list.get(i) > list.get(stack.peek())){

                // we have got the next largest element, so update it
                res[stack.peek()] = list.get(i);

                // remove the index from stack so that it does not get updated again
                stack.pop();

            }

            // push the current element
            // the next greater for this will be computed in the future iterations
            stack.push(i);

        }


    }

}
```
