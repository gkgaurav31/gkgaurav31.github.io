---
layout: post
title: Remove Duplicates from Sorted List II
date: 2023-08-08 19:51 +0530
author: "Gaurav Kumar"
tags: "java linkedlist leetcode leetcode150"
categories: "linkedlist"
---
## PROBLEM DESCRIPTION

Given the head of a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list. Return the linked list sorted as well.

[leetcode](https://leetcode.com/problems/remove-duplicates-from-sorted-list-ii/)

## SOLUTION

### APPROACH 1

```java
class Solution {
    
    public ListNode deleteDuplicates(ListNode head) {
        
        // Create a HashMap to store the frequency of each value in the linked list.
        Map<Integer, Integer> map = new HashMap<>();

        // Traverse the linked list to populate the frequency map.
        ListNode temp = head;
        while(temp != null){
            // Increment the frequency count for the current value in the map.
            map.put(temp.val, map.getOrDefault(temp.val, 0) + 1);
            // Move to the next node in the linked list.
            temp = temp.next;
        }

        // Create a dummy node with a value greater than any possible value in the list.
        // This helps handle cases where the first few nodes are duplicates.
        ListNode dummy = new ListNode(101, head);

        // Initialize two pointers: prev points to the previous distinct node,
        // and current points to the current node being checked.
        ListNode prev = dummy;
        ListNode current = head;

        // Traverse the linked list again to remove duplicate nodes.
        while(current != null){

            // Get the frequency of the current value from the map.
            int freq = map.get(current.val);

            // If the frequency is greater than or equal to 2, it means the value is a duplicate.
            if(freq >= 2){

                // Skip the duplicate node by updating the 'next' pointer of the previous node.
                prev.next = current.next;
                // Move to the next node.
                current = current.next;

            }else{

                // If the value is distinct, update both prev and current pointers to look at the next node
                prev = prev.next;
                current = current.next;

            }
        }

        // Return the modified linked list starting from the next of the dummy node.
        return dummy.next;
    }
}
```

### APPROACH 2 - SPACE OPTIMIZED

```java
class Solution {
    
    public ListNode deleteDuplicates(ListNode head) {
        // Create a dummy node with a value greater than any possible value in the list.
        // This helps handle cases where the first few nodes are duplicates.
        ListNode dummy = new ListNode(101, head);

        // Initialize two pointers: prev points to the previous distinct node,
        // and current points to the current node being checked.
        ListNode prev = dummy;
        ListNode current = head;

        // Traverse the linked list to remove duplicate nodes.
        while(current != null && current.next != null){

            // Check if the current value is the same as the next value.
            if(current.val == current.next.val){

                // If there are consecutive duplicate values, keep moving the current pointer.
                while(current.next != null && current.val == current.next.val){
                    current = current.next;
                }

                // Skip the duplicate nodes by updating the 'next' pointer of the previous node.
                prev.next = current.next;
                // Move to the next node after the duplicates.
                current = current.next;

            }else{
                // If the value is distinct, update both prev and current pointers.
                prev = prev.next;
                current = current.next;
            }

        }

        // Return the modified linked list starting from the next of the dummy node.
        return dummy.next;
    }
}
```
