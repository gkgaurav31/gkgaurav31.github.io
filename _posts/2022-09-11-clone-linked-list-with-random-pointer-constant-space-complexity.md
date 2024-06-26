---
layout: post
title: Clone Linked List with Random Pointer - Constant Space Complexity
date: 2022-09-11 23:54 +0530
author: "Gaurav Kumar"
tags: "java linkedlist leetcode important"
---

### PROBLEM DESCRIPTION

A linked list of length n is given such that each node contains an additional random pointer, which could point to any node in the list, or null. Construct a deep copy of the list.

```java
class Node {
    int val;
    Node next;
    Node random;

    public Node(int val) {
        this.val = val;
        this.next = null;
        this.random = null;
    }
}
```

[LeetCode](https://leetcode.com/problems/copy-list-with-random-pointer/)

### SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/clone_list_with_random_pointer_optimized.png)

```java
class Solution {

    public Node copyRandomList(Node head) {

        if(head == null) return null;

        Node t = head;

        while(t != null){
            Node newNode = new Node(t.val);
            newNode.next = t.next;
            t.next=newNode;
            t=t.next.next;
        }

        t = head;

        while(t != null){
            Node r = t.random;

            if(r == null){
                t = t.next.next;
                continue;
            }

            t.next.random = r.next;
            t = t.next.next;
        }

        Node h2=head.next;
        Node t1 = head;
        Node t2 = head.next;

        while(t2 != null){
            t1.next = t2.next;
            t1 = t1.next;
            if(t1==null)break;
            t2.next = t1.next;
            t2 = t2.next;
        }

        return h2;

    }

}
```
