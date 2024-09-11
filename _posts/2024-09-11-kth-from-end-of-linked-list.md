---
layout: post
title: Kth from End of Linked List (geeksforgeeks - SDE Sheet)
date: 2024-09-11 23:12 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given the head of a linked list and the number k, Your task is to find the kth node from the end. If k is more than the number of nodes, then the output should be -1.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/nth-node-from-end-of-linked-list/1?page=6)

## SOLUTION

```java
class Solution {

    int getKthFromLast(Node head, int k) {

        Node f = head;

        int count = k;

        while(count > 0){

            // this can happen is k is more than the number of nodes
            if(f == null)
                return -1;

            f = f.next;
            count--;

        }

        Node s = head;

        while(f != null){
            s = s.next;
            f = f.next;
        }

        return s.data;

    }
}
```
