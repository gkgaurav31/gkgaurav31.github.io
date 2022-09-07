---
layout: post
title: Reverse a Linked List
date: 2022-09-07 23:31 +0530
author: "Gaurav Kumar"
tags: "java linkedlist"
---

### PROBLEM DESCRIPTION

Reverse the LinkedList.

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/reverse_linked_list_2.jpg)

```java
public static Node reverseLinkedList(Node head){

    Node h1=head;
    Node t=head;
    Node h2=null;

    while(h1 != null){
        h1 = h1.next;
        t.next = h2;
        h2 = t;
        t = h1;
    }

    return h2;

}
```
