---
layout: post
title: Daily Temperatures
date: 2022-12-03 00:57 +0530
author: "Gaurav Kumar"
tags: "java stack important neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given an array of integers temperatures represents the daily temperatures, return an array answer such that answer[i] is the number of days you have to wait after the ith day to get a warmer temperature. If there is no future day for which this is possible, keep answer[i] == 0 instead.

[leetcode](https://leetcode.com/problems/daily-temperatures/description/)

## SOLUTION

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

## BETTER CODE

```java
class Solution {

    public int[] dailyTemperatures(int[] temperatures) {

        // Get the number of days
        int n = temperatures.length;

        // Stack to store pairs of temperature and index
        Stack<int[]> stack = new Stack<>(); // [temperature, index]

        // Result array to store the number of days to wait for a warmer temperature
        int[] res = new int[n];

        // Loop through each day
        for(int i = 0; i < n; i++) {

            // If the stack is empty, push the current temperature and index
            if(stack.isEmpty()) {
                stack.push(new int[]{temperatures[i], i});
                continue;
            }

            // While stack is not empty and the current temperature is higher than the temperature at the top of the stack
            while(!stack.isEmpty() && temperatures[i] > stack.peek()[0]) {

                // Calculate the number of days to wait and store it in the result array
                res[stack.peek()[1]] = i - stack.peek()[1];

                // Remove the top element from the stack
                stack.pop();

            }

            // Push the current temperature and index onto the stack
            stack.push(new int[]{temperatures[i], i});

        }

        // Return the result array
        return res;
    }
}
```
