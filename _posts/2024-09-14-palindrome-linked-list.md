---
layout: post
title: Palindrome Linked List (geeksforgeeks - SDE Sheet)
date: 2024-09-14 14:44 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a singly linked list of integers. The task is to check if the given linked list is palindrome or not.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/check-if-linked-list-is-pallindrome/1?page=6)

## SOLUTION

To solve this, we can reverse the second half of the linked list and then compare it with the first half. First, we use two pointers (slow and fast) to find the middle of the list. After reaching the middle, we reverse the second half of the list. Next, we compare the nodes from the first half with the nodes from the reversed second half. If all the nodes match, the list is a palindrome. Finally, we restore the original order of the list by reversing the second half back to its original form.

One important thing to handle is when the list is of odd length. After reversing, one of the lists will have one extra node. In my solution, I have kept it in the second list. So, after reversing the second list, that node will go to the end of the list. One way to handle it is by counting the number of nodes in list1. Then, we can just compare those many nodes in the two lists, ignoring the last node automatically.

```java
class Solution {

    boolean isPalindrome(Node head) {

        // If the list is empty or has only one node, it's a palindrome
        if(head == null || head.next == null)
            return true;

        // Initialize slow and fast pointers for finding the middle of the list
        Node slow = head;
        Node fast = head.next;

        // number of nodes in list1
        int count = 1;

        // Move fast pointer by 2 steps and slow pointer by 1 step
        // Fast will reach the end twice as quickly, while slow will reach the middle
        while(fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next.next;
            count++;
        }

        // Reverse the second half of the linked list starting from the node after 'slow'
        slow.next = reverseLinkedList(slow.next);

        // Pointers to traverse the first and second half for comparison
        Node p1 = head;
        Node p2 = slow.next;

        // Compare the first half and reversed second half node by node
        for(int k = 0; k < count; k++){

            // If corresponding data in both halves don't match, it's not a palindrome
            if(p1.data != p2.data)
                return false;

            // Move both pointers forward
            p1 = p1.next;
            p2 = p2.next;

        }

        // Restore the modified linked list by reversing the second half back to original
        slow.next = reverseLinkedList(slow.next);

        // If no mismatches found, it's a palindrome
        return true;
    }

    // Helper function to reverse the linked list
    Node reverseLinkedList(Node head){

        Node h1 = head;
        Node h2 = null;

        while(h1 != null){
            Node t = h1;
            h1 = h1.next;
            t.next = h2;
            h2 = t;
        }

        return h2;
    }
}
```
