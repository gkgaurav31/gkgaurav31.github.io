---
layout: post
title: Reverse First K Nodes - LinkedList
date: 2022-09-07 23:27 +0530
author: "Gaurav Kumar"
tags: "java linkedlist"
---

### PROBLEM DESCRIPTION

Reverse the first K Nodes of the LinkedList.

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/reverse_first_k_nodes_linked_list.png)

```java
public static Node reverseFirstKNodes(Node head, int k){

    Node h1=head;
    Node t=head;
    Node h2=null;
    Node h3 = head; //We will use this at the last step to connect the K reversed Nodes with rest of the LinkedList

    while(h1 != null && k>0){
        h1 = h1.next;
        t.next = h2;
        h2 = t;
        t = h1;
        k--;
    }

    h3.next = h1;

    return h2;
}
```
