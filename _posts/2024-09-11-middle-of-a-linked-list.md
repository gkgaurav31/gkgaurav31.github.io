---
layout: post
title: Middle of a Linked List (geeksforgeeks - SDE Sheet)
date: 2024-09-11 23:17 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given the head of a linked list, the task is to find the middle. For example, the middle of 1-> 2->3->4->5 is 3. If there are two middle nodes (even count), return the second middle. For example, middle of 1->2->3->4->5->6 is 4.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/finding-middle-element-in-a-linked-list/1?page=6)

## SOLUTION

```java
class Solution {

    int getMiddle(Node head) {

        Node f = head;
        Node s = head;

        while(f != null && f.next != null){
            f = f.next.next;
            s = s.next;
        }

        return s.data;

    }

}
```
