---
layout: post
title: Minimum Interval to Include Each Query
date: 2023-02-09 22:44 +0530
author: "Gaurav Kumar"
tags: "java leetcode intervals neetcode150 important"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given an array of meeting time intervals intervals where intervals[i] = [starti, endi], return the minimum number of conference rooms required.

[leetcode](https://leetcode.com/problems/meeting-rooms-ii/description/)

### SOLUTION

### BRUTE FORCE (TLE)

```java
class Solution {
    
    public int[] minInterval(int[][] intervals, int[] queries) {
        
        int[] ans = new int[queries.length];

        int minSize = -1;

        for(int i=0; i<queries.length; i++){

            minSize = Integer.MAX_VALUE;

            int current = queries[i];

            for(int j=0; j<intervals.length; j++){

                if(current >= intervals[j][0] && current <= intervals[j][1]){
                    minSize = Math.min(minSize, intervals[j][1] - intervals[j][0] + 1);
                }

            }

            if(minSize == Integer.MAX_VALUE) minSize = -1;

            ans[i] = minSize;

        }

        return ans;

    }

}
```

### OPTIMIZATION

[NeetCode YouTube](https://www.youtube.com/watch?v=5hQ5WWW5awQ)

```java
class Solution {

    public int[] minInterval(int[][] intervals, int[] queries) {

        int n = intervals.length;

        //sort the interval based on start position
        Arrays.sort(intervals, (o1, o2) -> o1[0] > o2[0]?1: (o1[0] < o2[0]?-1:0) );

        //create a copy of queries array and sort it
        int[] Q = queries.clone();
        Arrays.sort(Q);
        
        //sorted hashmap to store <Size, RightPosition>
        TreeMap<Integer, Integer> pq = new TreeMap<>();

        //store answer in a hashmap
        Map<Integer, Integer> res = new HashMap<>();

        //init
        int i = 0;

        //for each query
        for(Integer q: Q){

            //while there are more intervals to check and current position is >= start of current interval
            //it may possibly form the answer, so we add it to the min heap (TreeMap)
            while(i<n && q >= intervals[i][0]){

                //calculate size of the interval
                int size = intervals[i][1] - intervals[i][0] + 1;

                //put the size of the interval and also the right position (to be used later)
                pq.put(size, intervals[i][1]);

                //go to the next interval
                i++;
            }

            //we will pop intervals from min heap / TreeMap which in not in the required range
            //if min heap is not empty and current position is after the end of interval, remove it from min heap because it cannot form the answer
            while(pq.size() > 0 && pq.firstEntry().getValue() < q){
                pq.pollFirstEntry();
            }

            //if all intervals were removed, store answer as -1
            if(pq.size() == 0){
                res.put(q, -1);
            //else, store the answer as size on top of the min heap
            }else{  
                res.put(q, pq.firstKey());
            }

        }

        //the final answer array will be formed using the res hashmap
        int[] finalAnswer = new int[queries.length];

        for(int k=0; k<finalAnswer.length; k++){
            finalAnswer[k] = res.get(queries[k]);
        }

        return finalAnswer;

    }

}
```
