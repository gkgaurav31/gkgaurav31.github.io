---
layout: post
title: Reverse a LinkedList
date: 2022-06-17 18:21 +0530
author: "Gaurav Kumar"
tags: "java linkedlist"
categories: "leetcode"
---

### PROBLEM DESCRIPTION

Reverse a given LinkedList.  [Reverse a LinkedList](https://leetcode.com/explore/learn/card/linked-list/219/classic-problems/1205/)

### SOLUTION

### ITERATIVE APPROACH

![snapshot]({{ site.baseurl }}/assets/img/reverse_linked_list.jpg)

```java
class Solution {
    public ListNode reverseList(ListNode head) {
        
        if(head == null) return null;
        
        ListNode p=null,c=head, n=head.next;
        
        while(n!=null){
            c.next=p;
            p=c;
            c=n;
            n=n.next;
        }
        
        c.next=p;
        head = c;
        return head;
        
    }
}
```
