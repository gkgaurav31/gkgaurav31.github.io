---
layout: post
title: Double Character Trouble
date: 2022-09-16 21:26 +0530
author: "Gaurav Kumar"
tags: "java stack"
---

### PROBLEM DESCRIPTION

You are given a string A.
An operation on the string is defined as follows:
Remove the first occurrence of the same consecutive characters. eg for a string "abbcd", the first occurrence of same consecutive characters is "bb".
Therefore the string after this operation will be "acd".
Keep performing this operation on the string until there are no more occurrences of the same consecutive characters and return the final string.

### SOLUTION

```java
public class Solution {
    public String solve(String A) {
        
        Stack<String> s = new Stack<>();

        for(int i=0; i<A.length(); i++){
            
            String x = String.valueOf(A.charAt(i));

            //If current element is same as the top of stack, pop the top element from stack as we found a pair
            if(!s.isEmpty() && x.equals(s.peek())){
                s.pop();
            }else{
                s.push(x);
            }

        }

        StringBuffer sb = new StringBuffer("");
        while(!s.isEmpty()){
            sb.insert(0, s.pop());
        }

        return sb.toString();

    }
}
```
