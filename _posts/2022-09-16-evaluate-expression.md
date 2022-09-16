---
layout: post
title: " Evaluate Expression"
date: 2022-09-16 23:38 +0530
author: "Gaurav Kumar"
tags: "java stack"
---

### PROBLEM DESCRIPTION

An arithmetic expression is given by a character array A of size N. Evaluate the value of an arithmetic expression in Reverse Polish Notation.
Valid operators are +, -, *, /. Each character may be an integer or an operator.

### SOLUTION

```java
public class Solution {
    public int evalRPN(ArrayList<String> A) {

        Stack<String> stack = new Stack<String>();
        Set<String> set = new HashSet<>();
        set.add("+");
        set.add("-");
        set.add("*");
        set.add("/");
        

        for(int i=0; i<A.size(); i++){
            
            String x = A.get(i);

            //Operator found. Pop two elements from stack and calculate
            if(set.contains(x)){
                
                int a = Integer.parseInt(stack.pop());
                int b = Integer.parseInt(stack.pop());

                int ans = calculate(b, a, x);
                stack.push(ans+"");

            }else{ //Current element is a number
                stack.push(x);
            }
            
        }

        return Integer.parseInt(stack.pop());

    }

    public int calculate(int a, int b, String op) {

        int ans = 0;

        if(op.equals("+")){
            ans = a+b;
        }else if(op.equals("-")){
            ans=a-b;
        }else if(op.equals("*")){
            ans=a*b;
        }else{
            ans=a/b;
        }

        return ans;

    }

}
```
