---
layout: post
title: Task Scheduler
date: 2022-12-08 21:19 +0530
tags: "java  miscelleneous heap leetcode neetcode150" 
categories: "neetcode150"
---

## Problem Description

Given a characters array tasks, representing the tasks a CPU needs to do, where each letter represents a different task. Tasks could be done in any order. Each task is done in one unit of time. For each unit of time, the CPU could complete either one task or just be idle.

However, there is a non-negative integer n that represents the cooldown period between two same tasks (the same letter in the array), that is that there must be at least n units of time between any two same tasks.

Return the least number of units of times that the CPU will take to finish all the given tasks.

[leetcode](https://leetcode.com/problems/task-scheduler/description/)

### Solution

[Explanation](https://medium.com/@swgarciab/task-scheduler-leetcode-problem-a74acadf0e22)

![snapshot]({{ site.baseurl }}/assets/img/task_scheduler.png)

```java
class Solution {

    public int leastInterval(char[] tasks, int n) {

        Map<Character, Integer> frequency = new HashMap<>();

        //get max frequency
        int maxFrequency = -1;

        for(int i=0; i<tasks.length; i++){

            Character c = tasks[i];

            if(frequency.containsKey(c)){
                frequency.put(c, frequency.get(c)+1);
            }else{
                frequency.put(c, 1);
            }
            maxFrequency = Math.max(maxFrequency, frequency.get(c));
        }

        int lastRowLength = 0;

        for(java.util.Map.Entry entry: frequency.entrySet()){
            int v = (int) entry.getValue();
            if(v == maxFrequency) lastRowLength++;
        }

        int numberOfRows = maxFrequency;
        int columns = n+1;

        return Math.max((((numberOfRows-1)*columns)+lastRowLength), tasks.length);

    }

}
```
