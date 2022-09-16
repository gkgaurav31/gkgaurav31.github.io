---
layout: post
title: Balanced Parenthesis
date: 2022-09-16 21:16 +0530
author: "Gaurav Kumar"
tags: "java stack"
---

### PROBLEM DESCRIPTION

Given an expression string A, examine whether the pairs and the orders of “{“,”}”, ”(“,”)”, ”[“,”]” are correct in A.

### SOLUTION

```java
public class Solution {

    Map<String, String> map = new HashMap<>();

    public int solve(String A) {

        map.put("(",")");
        map.put("{","}");
        map.put("[","]");

        Stack<String> stack = new Stack<>();

        //Loop through each character in the string
        for(int i=0; i<A.length(); i++){
            
            //Get the current character
            String x = String.valueOf(A.charAt(i));

            //If the current char is opening bracket, push it to the stack
            if(map.containsKey(x)){
                stack.push(x);
            }else{
                //Else, check if stack is not empty and the current char is the corresponding closing bracket of the top most elment on the stack
                //Pop the top most element in the stack them as they form one pair like (), {} or []
                if(!stack.isEmpty() && x.equals(map.get(stack.peek()))){
                    stack.pop();
                }else{
                //If they don't form the right pair, the brackets are definitely not in the right order. Like: {] or (] etc.
                    return 0;
                }
            }

        }
        
        //If the stack is empty, all brackets have corresponding closing bracket in the right order
        return stack.isEmpty()?1:0;

    }
}
```
