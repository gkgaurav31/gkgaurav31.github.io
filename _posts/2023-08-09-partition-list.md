---
layout: post
title: Partition List
date: 2023-08-09 19:37 +0530
author: "Gaurav Kumar"
tags: "java linkedlist leetcode leetcode150"
categories: "linkedlist"
---
## PROBLEM DESCRIPTION

Given the head of a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.
You should preserve the original relative order of the nodes in each of the two partitions.

[leetcode](https://leetcode.com/problems/partition-list/)

## SOLUTION

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    
    public ListNode partition(ListNode head, int x) {

        // Create two new linked lists to hold nodes less than x and nodes greater than or equal to x.
        
        ListNode lessHead = new ListNode(101); // Placeholder value, will be ignored.
        ListNode lessTail = lessHead; // Pointer to the tail of the "less than x" list.

        ListNode moreHead = new ListNode(101); // Placeholder value, will be ignored.
        ListNode moreTail = moreHead; // Pointer to the tail of the "greater than or equal to x" list.

        ListNode t = head; // Temporary pointer to traverse the original linked list.

        while(t != null){

            ListNode next = t.next; // Save the next node before modifying current node's next pointer.

            // If the current node's value is less than x, append it to the "less than x" list.
            if(t.val < x){
                lessTail.next = t; // Add current node to the end of the "less than x" list.
                t.next = null; // Break the next pointer of the current node.
                lessTail = t; // Move the tail pointer of the "less than x" list to the current node.
            // If the current node's value is greater than or equal to x, append it to the "greater than or equal to x" list.
            } else {
                moreTail.next = t; // Add current node to the end of the "greater than or equal to x" list.
                t.next = null; // Break the next pointer of the current node.
                moreTail = t; // Move the tail pointer of the "greater than or equal to x" list to the current node.
            }

            t = next; // Move to the next node in the original list.
        }

        // Connect the tail of the "less than x" list to the head of the "greater than or equal to x" list. (skipping the dummy note)
        lessTail.next = moreHead.next;
        
        // The "less than x" list now contains all nodes less than x in their original order,
        // followed by all nodes greater than or equal to x in their original order.
        return lessHead.next; // Return the head of the merged list.
    }
}
```
