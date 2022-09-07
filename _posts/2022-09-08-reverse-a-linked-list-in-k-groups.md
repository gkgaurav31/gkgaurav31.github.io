---
layout: post
title: Reverse a Linked List in K Groups
date: 2022-09-08 00:02 +0530
author: "Gaurav Kumar"
tags: "java linkedlist important"
---

### PROBLEM DESCRIPTION

Given a singly linked list and an integer k, reverse the nodes of the list k at a time and return the modified linked list. If there are less than k size, reverse them in the output list.

![snapshot]({{ site.baseurl }}/assets/img/reverse_k_window_linked_list.jpg)

### SOLUTION

```java
public static Node reverseListKWindowSize(Node head, int k){

    if(head == null || k==0) return null;

    Node h1=head;
    Node t=head;
    Node h2=null;
    Node h3 = head;
    int tempK = k;

    while(h1 != null && k>0){
        h1 = h1.next;
        t.next = h2;
        h2 = t;
        t = h1;
        k--;
    }

    h3.next = reverseListKWindowSize(h1, tempK);

    return h2;
}
```
