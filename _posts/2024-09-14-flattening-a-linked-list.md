---
layout: post
title: Flattening a Linked List (geeksforgeeks - SDE Sheet)
date: 2024-09-14 18:33 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a Linked List, where every node represents a sub-linked-list and contains two pointers:  
(i) a next pointer to the next node,  
(ii) a bottom pointer to a linked list where this node is head.
Each of the sub-linked lists is in sorted order.  
Flatten the Link List so all the nodes appear in a single level while maintaining the sorted order.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/flattening-a-linked-list/1?page=6)

## SOLUTION

We can solve this by recursively flattening the multi-level linked list. First, we detach the next pointer of the current list, recursively flatten the remaining list, and then merge the current list with the flattened result. To merge two sorted sub-linked lists, we compare their nodes one by one and link the smaller nodes together, ensuring the overall list remains sorted.

```java
class Solution {

    Node flatten(Node root) {
        return flattenHelper(root);
    }

    Node flattenHelper(Node root) {

        // Base case: if root is null or there is no next list, return the root as it's already flat
        if (root == null || root.next == null)
            return root;

        // Recursively flatten the rest of the list starting from the second node (root.next)
        Node first = root;   // The first sub-linked list
        Node second = root.next;  // The rest of the sub-linked lists

        // Detach the next pointer of the first list to flatten the current level
        first.next = null;

        // Recursively flatten the second list
        second = flattenHelper(second);

        // Merge the current list (first) with the flattened rest of the lists (second)
        return mergeSortedLinkedLists(first, second);
    }

    // Helper function to merge two sorted linked lists into one sorted list
    Node mergeSortedLinkedLists(Node head1, Node head2) {

        Node h1 = head1;
        Node h2 = head2;

        Node dummyHead = new Node(0);
        Node tail = dummyHead;

        while (h1 != null && h2 != null) {

            Node temp = null;

            if (h1.data < h2.data) {
                temp = h1;
                h1 = h1.bottom;
            } else {
                temp = h2;
                h2 = h2.bottom;
            }

            temp.bottom = null;

            tail.bottom = temp;
            tail = tail.bottom;
        }

        if (h1 != null)
            tail.bottom = h1;
        else
            tail.bottom = h2;

        return dummyHead.bottom;
    }
}
```

### BREAK AT THE CENTER

We could have also divided the Linked Lists from the center and recursively solved the two sub-problems.

```java
class Solution {

    Node flatten(Node root) {
        return flattenHelper(root);
    }

    Node flattenHelper(Node root){

        if(root == null || root.next == null)
            return root;

        // divide into two parts
        Node slow = root;
        Node fast = root.next;

        while(fast != null && fast.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }

        Node t1 = slow;
        Node h2 = slow.next;

        // break
        t1.next = null;

        Node h1 = root;

        Node first = flattenHelper(h1);
        Node second = flattenHelper(h2);

        return mergeSortedLinkedLists(first, second);

    }

    Node mergeSortedLinkedLists(Node head1, Node head2){

        Node h1 = head1;
        Node h2 = head2;

        Node dummyHead = new Node(0);
        Node tail = dummyHead;

        while(h1 != null && h2 != null){

            Node temp = null;

            if(h1.data < h2.data){

                temp = h1;
                h1 = h1.bottom;

            }else{

                temp = h2;
                h2 = h2.bottom;

            }

            temp.bottom = null;

            tail.bottom = temp;
            tail = tail.bottom;

        }

        if(h1 != null)
            tail.bottom = h1;
        else
            tail.bottom = h2;

        return dummyHead.bottom;

    }

}
```
