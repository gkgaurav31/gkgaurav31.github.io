---
layout: post
title: Course Schedule II
date: 2022-11-27 14:51 +0530
author: "Gaurav Kumar"
tags: "java leetcode graphs"
categories: "graphs"
---

### Problem Description

There are a total of numCourses courses you have to take, labeled from 0 to numCourses - 1. You are given an array prerequisites where prerequisites[i] = [ai, bi] indicates that you must take course bi first if you want to take course ai.

- For example, the pair [0, 1], indicates that to take course 0 you have to first take course 1.

Return the ordering of courses you should take to finish all courses. If there are many valid answers, return any of them. If it is impossible to finish all courses, return an empty array.

[leetcode](https://leetcode.com/problems/course-schedule/description/)

### Solution

Refer to the following solution to understand the main idea: [Course Schedule]({% post_url 2022-11-27-course-schedule %})

```java
class Solution {
    
    public int[] findOrder(int numCourses, int[][] prerequisites) {

        //create adjacency list using prerequisites
        ArrayList<Integer>[] adj = new ArrayList[numCourses];
        for(int i=0; i<numCourses; i++){
            adj[i] = new ArrayList<Integer>();
        }
        for(int i=0; i<prerequisites.length; i++){
            adj[prerequisites[i][0]].add(prerequisites[i][1]);
        }

        //in-degree
        int[] indegree = new int[numCourses];
        for(int i=0; i<prerequisites.length; i++){
            indegree[prerequisites[i][1]]++;
        }

        //add nodes with in-degree 0 to a queue
        Queue<Integer> q = new LinkedList<>();
        for(int i=0; i<numCourses; i++){
            if(indegree[i] == 0){
                q.add(i);
            }
        }

        int processed = 0;
        int[] ans = new int[numCourses];

        while(!q.isEmpty()){

            Integer x = q.poll();
            ans[processed] = x;
            processed++;
            List<Integer> neighbours = adj[x];

            //indegree of neighbours of x will decrease by 1
            for(int i=0; i<neighbours.size(); i++){
                
                indegree[neighbours.get(i)]--;

                //If it's 0, add to queue
                if(indegree[neighbours.get(i)] == 0){
                    q.add(neighbours.get(i));
                }

            }

        }

        //The actual answer will be reverse of ans[]
        int[] convertedAns = new int[numCourses];
        for(int i=0; i<numCourses; i++){
            convertedAns[i] = ans[numCourses-i-1];
        }

        if(processed == numCourses)
            return convertedAns;
        else
            return new int[]{};
    }
}
```
