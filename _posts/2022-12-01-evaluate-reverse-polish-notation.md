---
layout: post
title: Evaluate Reverse Polish Notation
date: 2022-12-01 22:32 +0530
author: "Gaurav Kumar"
tags: "java leetcode stack"
categories: "stack"
---

### Problem Description

Evaluate the value of an arithmetic expression in Reverse Polish Notation.  

Valid operators are +, -, *, and /. Each operand may be an integer or another expression.  

Note that division between two integers should truncate toward zero.  

It is guaranteed that the given RPN expression is always valid. That means the expression would always evaluate to a result, and there will not be any division by zero operation.  

[leetcode](https://leetcode.com/problems/evaluate-reverse-polish-notation/description/)

### Solution

```java
class Solution {

    public int evalRPN(String[] tokens) {
        
        Set<String> operands = new HashSet<>();

        operands.add("+");
        operands.add("-");
        operands.add("*");
        operands.add("/");

        Stack<String> stack = new Stack<>();

        for(int i=0; i<tokens.length; i++){

            if(operands.contains(tokens[i])){

                int t = 0;

                int x = Integer.parseInt(stack.pop());
                int y = Integer.parseInt(stack.pop());

                if(tokens[i].equals("+")) t = x + y;
                if(tokens[i].equals("-")) t = y - x;
                if(tokens[i].equals("/")) t = y / x;
                if(tokens[i].equals("*")) t = x * y;

                stack.add(t + "");

            }else{

                stack.add(tokens[i]);

            }
        }

        return Integer.parseInt(stack.pop());

    }
}
```
