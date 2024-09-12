---
layout: post
title: Remove loop in Linked List (geeksforgeeks - SDE Sheet)
date: 2024-09-12 22:01 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given the head of a linked list that may contain a loop. A loop means that the last node of the linked list is connected back to a node in the same list. So if the next of the previous node is null. then there is no loop. Remove the loop from the linked list, if it is present (we mainly need to make the next of the last node null). Otherwise, keep the linked list as it is.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/remove-loop-in-linked-list/1?page=6)

## SOLUTION

```java
class Solution {

    public static void removeLoop(Node head) {

        // store previously visited nodes
        Set<Node> set = new HashSet<>();

        // for traversing the list
        Node temp = head;

        while(temp != null){

            // next node
            Node nextNode = temp.next;

            // if the current node points to the next node
            // and that next node was already visited
            // we know, there is a loop
            // we can break the loop by removing the link from current to the next node
            if(set.contains(nextNode)){
                temp.next = null;
                break;
            }else
            // otherwise, add the current node to the visited list
                set.add(temp);

            // go to the next node
            temp = temp.next;

        }

    }

}
```
