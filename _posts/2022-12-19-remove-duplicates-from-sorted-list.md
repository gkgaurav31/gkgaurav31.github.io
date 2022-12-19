---
layout: post
title: Remove Duplicates from Sorted List
date: 2022-12-19 22:50 +0530
author: "Gaurav Kumar"
tags: "java leetcode linkedlist"
categories: "linkedlist"
---

### PROBLEM DESCRIPTION

Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.

[leetcode](https://leetcode.com/problems/remove-duplicates-from-sorted-list/description/)

### SOLUTION

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {

        ListNode temp = head;

        //while list has not ended
        while(temp != null){
            
            //if next node is null, nothing more to check so return head
            if(temp.next == null) return head;

            //if value of current node and next node is same
            if(temp.next.val == temp.val){

                //remove the next node but don't move forward because there are be more than 1 duplicate
                temp.next = temp.next.next;

            }else{

                //if value is different, move to next node
                temp = temp.next;
            }

        }

        return head;
        
    }
}
```
