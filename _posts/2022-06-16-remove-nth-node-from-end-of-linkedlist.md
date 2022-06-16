---
layout: post
title: Remove nth Node From End of LinkedList
date: 2022-06-16 22:54 +0530
author: "Gaurav Kumar"
tags: "java linkedlist"
categories: "leetcode"
---

### PROBLEM DESCRIPTION: [Remove nth Node From End of LinkedList](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

Given a LinkedList and an integer n, remove nth Node from the end of the list.

### SOLUTION

One approach that will work is to first get the length of the LinkedList by looping through all the elements. Then the element go remove will be (Length-n+1)th element from the beginning (1-indexed).  

An optimized approach is to use two pointers slow and fast. We give a heads up to the fast pointer by n. The slow is set to head. Then we move both of them one by one until fast reaches the end of the list. At that moment, slow will point to one Node before the one which needs to be removed (this is what we need to remove the next Node). So, simply do a slow.next = slow.next.next. Edge case is when fast becomes null. This indicates that the first element needs to be removed, in which case we can simply return head.next.

[REFERENCE](https://leetcode.com/problems/remove-nth-node-from-end-of-list/discuss/1164542/JS-Python-Java-C%2B%2B-or-Easy-Two-Pointer-Solution-w-Explanation)

```java
class Solution {

    public ListNode removeNthFromEnd(ListNode head, int n) {
        
        if(head == null) return null;
                
        ListNode slow=head, fast=head;
        
        for(int i=0; i<n; i++){
            fast = fast.next;
        }
        
        if(fast == null){
            //remove first element
            return head.next;
        }
        
        while(fast.next != null){
            fast=fast.next;
            slow=slow.next;
        }
        
        slow.next = slow.next.next;

        return head;
        
    }
}
```