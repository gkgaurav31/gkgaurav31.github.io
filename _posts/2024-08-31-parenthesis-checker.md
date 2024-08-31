---
layout: post
title: Parenthesis Checker (geeksforgeeks - SDE Sheet)
date: 2024-08-31 12:01 +0530
author: "Gaurav Kumar"
tags: "java stack geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an expression string x. Examine whether the pairs and the orders of {,},(,),[,] are correct in exp.
For example, the function should return 'true' for exp = `[()]{}{[()()]()}` and 'false' for exp = [(]).

Note: The drive code prints "balanced" if function return true, otherwise it prints "not balanced".

[geeksforgeeks](https://www.geeksforgeeks.org/problems/parenthesis-checker2744/1?page=4)

## SOLUTION

```java
class Solution
{
    //Function to check if brackets are balanced or not.
    static boolean ispar(String x)
    {

        Map<Character, Character> brackets = new HashMap<>();

        brackets.put(')', '(');
        brackets.put('}', '{');
        brackets.put(']', '[');

        Stack<Character> stack = new Stack<>();

        for(int i=0; i<x.length(); i++){

            char ch = x.charAt(i);

            // we got a closing bracket
            if(brackets.containsKey(ch)){

                // check if it has corresponding open bracket in stack
                if(!stack.isEmpty() && brackets.get(ch) == stack.peek()){
                    stack.pop();
                }else
                    return false;

            }else{
                stack.push(ch);
            }

        }

        return stack.isEmpty();

    }
}
```
