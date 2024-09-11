---
layout: post
title: Reverse a linked list (geeksforgeeks - SDE Sheet)
date: 2024-09-11 23:23 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given the head of a linked list, the task is to reverse this list and return the reversed head.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/reverse-a-linked-list/1?page=6)

## SOLUTION

```java
class Solution {

    Node reverseList(Node head) {

        Node h1 = head;
        Node h2 = null;

        Node t = head;

        while(h1 != null){

            h1 = h1.next;

            t.next = h2;
            h2 = t;
            t = h1;

        }

        return h2;

    }

}
```
