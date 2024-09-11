---
layout: post
title: Delete without head pointer (geeksforgeeks - SDE Sheet)
date: 2024-09-11 23:46 +0530
author: "Gaurav Kumar"
tags: "java linkedlist geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given a node del_node of a Singly Linked List where you have to delete a value of the given node from the linked list but you are not given the head of the list.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/delete-without-head-pointer/1?page=6)

## SOLUTION

### APPROACH

Since head of the linked list is not given, we cannot get to the previous node and then remove the current node. So, the only option is to replace the value of current node with the next nodes and finally get rid of the last node in the linked list.

```java
class Solution {

    void deleteNode(Node node) {

        Node t = node;

        while(t.next != null){

            t.data = t.next.data;

            // last node that we need
            if(t.next.next == null){
                t.next = null;
                break;
            }

            t = t.next;

        }


    }

}
```

### BETTER APPROACH

We don't really need to keep iterating over all the nodes to copy over.

We can copy the value from the adjacent node only for the current node and delete the next node!

```java
class Solution {

    void deleteNode(Node node) {

        if(node == null || node.next == null)
            return;

        Node nextNode = node.next;
        node.data = nextNode.data;

        node.next = nextNode.next;

    }

}
```
