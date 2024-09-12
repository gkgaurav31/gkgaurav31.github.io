---
layout: post
title: Detect Loop in linked list (geeksforgeeks - SDE Sheet)
date: 2024-09-12 22:06 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given the head of a singly linked list, the task is to check if the linked list has a loop. A loop means that the last node of the linked list is connected back to a node in the same list. So if the next of the last node is null. then there is no loop.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/detect-loop-in-linked-list/1?page=6)

## SOLUTION

```java
class Solution {

    public static boolean detectLoop(Node head) {

        if(head == null || head.next == null)
            return false;

        Node slow = head;
        Node fast = head.next;

        while(fast != null && fast.next != null){

            fast = fast.next.next;
            slow = slow.next;

            if(slow == fast)
                return true;

        }

        return false;

    }

}
```
