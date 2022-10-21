---
layout: post
title: Finish Maximum Jobs
date: 2022-10-20 19:05 +0530
author: "Gaurav Kumar"
tags: "java greedy important"
categories: "greedy"
---

## Problem Description

There are N jobs to be done, but you can do only one job at a time.  
Given an array A denoting the start time of the jobs and an array B denoting the finish time of the jobs.  
Your aim is to select jobs in such a way so that you can finish the maximum number of jobs.  

Return the maximum number of jobs you can finish.

### Solution

If there are two tasks A and B, such that endTime of A is more than endTime of B. It would make sense to choose B because then we will have more time remaining to complete other tasks.  

>____A___________  
>_B______________  

```java
public class Solution {
    public int solve(ArrayList<Integer> A, ArrayList<Integer> B) {

        List<Pair> list = new ArrayList<>();

        for(int i=0; i<A.size(); i++){
            list.add(new Pair(A.get(i), B.get(i)));
        }

        //Sort based on comparator which uses endTime (ascending order)
        Collections.sort(list);
        
        //If there are no tasks, return 0
        if(list.size() == 0) return 0;

        //At least 1 task is there. Initialize it as the lastTask (this will be the task which has the least endTime)
        //In order words, it will be the task which finishes up the earliest
        int count = 1;
        Pair lastTask = new Pair(list.get(0).startTime, list.get(0).endTime);

        for(int i=1; i<list.size(); i++){
            
            Pair p = list.get(i);

            //If the next task has some overlap with the last task which was done, skip it.
            //Otherwise increase the count and update lastTask
            if(!isOverlapping(lastTask, p)){
                count++;
                lastTask = p;
            }

        }

        return count;
    }

    public boolean isOverlapping(Pair p1, Pair p2){
        //If the startTime of the currentTask if after endTime of previous task => no overlap
        if(p2.startTime >= p1.endTime) return false;
        return true;

    }

}

class Pair implements Comparable<Pair>{
    int startTime;
    int endTime;
    
    Pair(int s, int e){
        startTime = s;
        endTime = e;
    }

    @Override
    //This will be used to sort the Pairs based on endTime
    public int compareTo(Pair a){
        if(this.endTime > a.endTime) return 1;
        else if(this.endTime == a.endTime) return 0;
        else return -1;
    }

    @Override
    public String toString(){
        return "Start: " + this.startTime + " EndTime: " + this.endTime;
    }

}
```
