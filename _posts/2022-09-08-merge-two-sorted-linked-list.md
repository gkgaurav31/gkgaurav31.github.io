---
layout: post
title: Merge Two Sorted Linked List - 2
date: 2022-09-08 23:10 +0530
author: "Gaurav Kumar"
tags: "java linkedlist"
---

### PROBLEM DESCRIPTION

Merge two sorted Linked List.

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/merge_sorted_linked_list.png)

```java
public static Node mergeSortedLinkedList(Node h1, Node h2){

    //If any of the list is empty, return the other sorted list
    if(h1 == null) return h2;
    if(h2 == null) return h1;

    Node t=null, h3=null;

    //Initialize
    if(h1.value < h2.value){
        t=h1;
        h3=h1;
        h1=t.next;
    }else{
        t=h2;
        h3=h2;
        h2=t.next;
    }

    
    while(h1 != null && h2 != null){
        if(h1.value < h2.value){
            t.next=h1;
            h1=h1.next;
            t=t.next;
        }else{
            t.next=h2;
            h2=h2.next;
            t=t.next;
        }
    }

    if(h1 == null){
        t.next = h2;
    }else{
        t.next = h1;
    }

    return h3;
}
```
