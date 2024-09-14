---
layout: post
title: Intersection Point in Y Shaped Linked Lists (geeksforgeeks - SDE Sheet)
date: 2024-09-14 16:50 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two singly linked lists, return the point where two linked lists intersect.

Note: If the linked lists do not merge at any point, return -1.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/intersection-point-in-y-shapped-linked-lists/1?page=6)

## SOLUTION

We can solve this by first calculating the lengths of both linked lists and determining the difference in their lengths. If the lists are of unequal length, we move the pointer of the longer list forward by the difference in length to align both lists at the same remaining number of nodes. Then, we traverse both lists simultaneously, comparing the nodes at each step. If we find a node where the two lists intersect (i.e., the nodes are the same), we return the value of that node. If no intersection exists, we return -1.

```java
class Intersect {

    int intersectPoint(Node head1, Node head2) {

        // Initialize two pointers for both linked lists
        Node h1 = head1;
        Node h2 = head2;

        // If either of the lists is empty, return -1 (no intersection possible)
        if(h1 == null || h2 == null)
            return -1;

        // Variables to store the lengths of both linked lists
        int n1 = 0;
        int n2 = 0;

        // Calculate the length of the first linked list
        while(h1 != null){
            n1++;
            h1 = h1.next;
        }

        // Calculate the length of the second linked list
        while(h2 != null){
            n2++;
            h2 = h2.next;
        }

        // Compare the lengths of both lists
        // Call the helper function to find the intersection point
        // If the first list is shorter, use it as the first parameter of the helper function
        if(n1 < n2){
            return intersectionPointHelper(head1, n1, head2, n2);
        }else{
            // If the second list is shorter, switch the parameters
            return intersectionPointHelper(head2, n2, head1, n1);
        }

    }

    int intersectionPointHelper(Node h1, int n1, Node h2, int n2){

        // Calculate the difference in the lengths of the two linked lists
        int diff = n2 - n1;

        // Move the pointer in the longer list forward by 'diff' nodes to align the lists
        for(int k=0; k<diff; k++)
            h2 = h2.next;

        // Traverse both lists in parallel
        // If the nodes are the same, return the intersection point
        while(h1 != null && h2 != null){
            if(h1 == h2)
                return h1.data;  // Intersection found
            h1 = h1.next;
            h2 = h2.next;
        }

        // no intersection is found, return -1
        return -1;
    }
}
```
