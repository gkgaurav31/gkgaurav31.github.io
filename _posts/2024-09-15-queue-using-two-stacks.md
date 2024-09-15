---
layout: post
title: Queue using two Stacks (geeksforgeeks - SDE Sheet)
date: 2024-09-15 14:15 +0530
author: "Gaurav Kumar"
tags: "java stack queue geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Implement a Queue using 2 stacks s1 and s2

A Query Q is of 2 Types

(i) 1 x (a query of this type means pushing 'x' into the queue)  
(ii) 2 (a query of this type means to pop element from queue and print the poped element)

[geeksforgeeks](https://www.geeksforgeeks.org/problems/queue-using-two-stacks/1?page=7)

## SOLUTION

- push

  - First, all elements from s1 are transferred to s2.
  - Then, the new element x is pushed to the now-empty s1.
  - Finally, all the elements in s2 are transferred back to s1 in reverse order.

- pop
  - get element from s1. return -1 if empty

```java
class StackQueue
{
    Stack<Integer> s1 = new Stack<Integer>();
    Stack<Integer> s2 = new Stack<Integer>();

    //Function to push an element in queue by using 2 stacks.
    void Push(int x)
    {

        while(!s1.isEmpty())
            s2.push(s1.pop());

        s1.push(x);

        while(!s2.isEmpty())
            s1.push(s2.pop());


    }


    //Function to pop an element from queue by using 2 stacks.
    int Pop()
    {
        return s1.isEmpty() ? -1 : s1.pop();
    }
}
```
