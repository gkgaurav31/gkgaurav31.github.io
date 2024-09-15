---
layout: post
title: Implement Queue using Linked List (geeksforgeeks - SDE Sheet)
date: 2024-09-15 18:34 +0530
author: "Gaurav Kumar"
tags: "java queue geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Implement a Queue using Linked List.

A Query Q is of 2 Types

(i) 1 x (a query of this type means pushing 'x' into the queue)  
(ii) 2 (a query of this type means to pop an element from the queue and print the poped element)

[geeksforgeeks](https://www.geeksforgeeks.org/problems/implement-queue-using-linked-list/1?page=7)

## SOLUTION

Push using rear pointer and pop from front.

```java
class MyQueue
{
    QueueNode front, rear;

    //Function to push an element into the queue.
    void push(int a)
    {

        if(front == null){
            front = new QueueNode(a);
            rear = front;
        }else{
            rear.next = new QueueNode(a);
            rear = rear.next;
        }

    }

    //Function to pop front element from the queue.
    int pop()
    {

        if(front == null)
            return -1;
        else if(front == rear){
            int current = front.data;
            front = null;
            rear = null;
            return current;
        }else{
            int current = front.data;
            front = front.next;
            return current;
        }

    }
}
```
