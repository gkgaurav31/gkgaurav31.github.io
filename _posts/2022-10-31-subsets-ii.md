---
layout: post
title: Subsets II
date: 2022-10-31 22:30 +0530
author: "Gaurav Kumar"
tags: "java recursion backtracking"
categories: "backtracking"
---

## Problem Description

Given an integer array nums that __may contain duplicates__, return all possible subsets (the power set).  

The solution set must not contain duplicate subsets. Return the solution in any order.  

[leetcode](https://leetcode.com/problems/subsets-ii/)

### Solution

The solution is based on [Subset I]({% post_url 2022-10-31-subsets %}), with a change that the input can contain duplicate values. The answer list must not contain duplicate sets. Like, {1,2} and {2,1} are same.  

The way to handle the duplicates is that after including the element, if the next element is also same then we need to skip it.  

```java
class Solution {
    
    List<List<Integer>> ans = new ArrayList<>();
    
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        
        Arrays.sort(nums);
        
        find(nums, 0, new ArrayList<>());
        return ans;
    }
    
    public void find(int[] nums, int i, List<Integer> list){
        
        if(i == nums.length){
            ans.add(new ArrayList<>(list));
            return;
        }
        
        //Including
        list.add(nums[i]);
        find(nums, i+1, list);
        while(i<nums.length-1 && nums[i] == nums[i+1]) i++;
        
        //Backtrack
        list.remove(list.size()-1);
        
        //Excluding
        find(nums, i+1, list);
        
    }
}
```

### ANOTHER APPROACH

This is less optimal than the previous approach, but more intuitive. We can maintain a HashMap to check if the current subset was already included. Instead of storing the List in the HashSet, we can convert the list as a String delimeted with $ and store that in the HashSet. In the current problem, there are only 10 elements, so the time taken for each conversion is not much. This solution is also acceptable on leetcode:

```java
class Solution {

    private static final String D = "$";

    public List<List<Integer>> subsetsWithDup(int[] nums) {
        subsetHelper(nums, 0, new ArrayList<>());
        return ans;
    }

    List<List<Integer>> ans = new ArrayList<>();
    Set<String> set = new HashSet<>();

    public void subsetHelper(int[] nums, int idx, List<Integer> subset){

        if(idx == nums.length){

            List<Integer> temp = new ArrayList<>(subset);
            Collections.sort(temp);
            String current = convertToString(temp);

            if(!set.contains(current)){
                ans.add(new ArrayList<>(temp));
                set.add(current);
            }

            return;

        }

        //pick
        subset.add(nums[idx]);
        subsetHelper(nums, idx+1, subset);

        //backtrack
        subset.remove(subset.size()-1);

        //leave
        subsetHelper(nums, idx+1, subset);

    }

    public String convertToString(List<Integer> list){

        StringBuilder sb = new StringBuilder();

        for(int i=0; i<list.size(); i++){
            sb.append(list.get(i) + D);
        }
        
        return sb.toString();
    }

}
```
