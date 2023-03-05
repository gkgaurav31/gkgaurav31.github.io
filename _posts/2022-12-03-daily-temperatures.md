---
layout: post
title: Daily Temperatures
date: 2022-12-03 00:57 +0530
author: "Gaurav Kumar"
tags: "java recursion stack important neetcode150"
categories: "neetcode150"
---

## Problem Description

Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.

[leetcode](https://leetcode.com/problems/daily-temperatures/description/)

### Solution

Here we are pushing Pair<Temperature, Index> to the stack, but it can also be solved by just pushing in the indices.

```java
class Solution {
    
    public int[] dailyTemperatures(int[] temperatures) {
        
        int n = temperatures.length;

        Stack<Pair> stack = new Stack<>();

        int[] nearest = new int[n];
        
        //init
        nearest[n-1] = 0;
        stack.push(new Pair(temperatures[n-1], n-1));


        //check nearest for other temperatures
        for(int i=n-2; i>=0; i--){
            
            //if the temperature of top of stack is higher, we can say that it's the nearest warmer one
            //the current temperature can be the answer for other temperature on the left, so we push that to stack as well
            if(stack.peek().temperature > temperatures[i]){
                nearest[i] = stack.peek().index - i;
                stack.push(new Pair(temperatures[i], i));

            //if temperature on top of stack is not higher than the current, pop that
            }else{
                
                //keep popping it until stack is empty OR we have got a temperature which is more than the current temperature
                while(stack.size() > 0 && stack.peek().temperature <= temperatures[i]){
                    stack.pop();
                }

                //if stack turns out to be empty => there is no warmer temperature
                //so mark it as 0 and push the current temperature to stack which can be answer for other places
                if(stack.size() == 0){
                    nearest[i] = 0;
                    stack.push(new Pair(temperatures[i], i));
                //otherwise, we have a warmer temperature for the current one
                //save it and push the current temperature back to the stack
                }else{
                    nearest[i] = stack.peek().index - i;
                    stack.push(new Pair(temperatures[i], i));
                }

            }

        }

        return nearest;

    }
}

class Pair{

    int temperature;
    int index;

    public Pair(int temperature, int index){
        this.temperature = temperature;
        this.index = index;
    }
    
}
```
