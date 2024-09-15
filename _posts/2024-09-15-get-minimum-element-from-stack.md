---
layout: post
title: Get minimum element from stack (geeksforgeeks - SDE Sheet)
date: 2024-09-15 14:39 +0530
author: "Gaurav Kumar"
tags: "java stack geeksforgeeks sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You are given N operations and your task is to Implement a Stack in which you can get a minimum element in O(1) time.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/get-minimum-element-from-stack/1?page=7)

## SOLUTION

We can solve this by using a stack where each element is an array containing two values: the actual element and the minimum value at that point in the stack. This allows us to maintain the current minimum element efficiently as we push and pop values.

```java
class GfG
{
    Stack<int[]> s;

    GfG()
    {
        s = new Stack<>();
    }

    int getMin()
    {
        // Return -1 if the stack is empty
        if(s.isEmpty())
            return -1;

        // Return the second element of the array (the current minimum)
        return s.peek()[1];
    }

    int pop()
    {
        // Return -1 if the stack is empty
        if(s.isEmpty())
            return -1;

        // Pop the stack and return the first element of the array (the value)
        return s.pop()[0];
    }

    void push(int x)
    {
        // If the stack is empty, push the element along with itself as the minimum.
        if(s.isEmpty())

            s.push(new int[]{x, x});

        else{

            // Push the new element along with the current minimum of the stack.
            // The new minimum is either the current element or the minimum at the top of the stack.
            s.push(
                new int[]{
                    x,
                    Math.min(x, s.peek()[1])
                }
            );
        }

    }
}
```
