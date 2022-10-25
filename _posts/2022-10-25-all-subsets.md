---
layout: post
title: All Subsets
date: 2022-10-25 21:50 +0530
author: "Gaurav Kumar"
tags: "java recursion backtracking"
categories: "backtracking"
---

## Problem Description

Given a set of distinct integers A, return all possible subsets.

__NOTE:__

- Elements in a subset must be in non-descending order.
- The solution set must not contain duplicate subsets.
- Also, the subsets should be sorted in ascending ( lexicographic ) order.
- The list is not necessarily sorted.

### Solution

For each element, we can two choices - either to include that element or exclude it from the current subset.

```java
public class Solution {

    ArrayList<ArrayList<Integer>> ans = new ArrayList<>();

    public ArrayList<ArrayList<Integer>> subsets(ArrayList<Integer> A) {
        
        //We can sort the list since the problem needs the elements in sorted order in each subset
        Collections.sort(A);
        
        //main helper method that will generate all subsets and add to answer list
        helper(A, new ArrayList<>(), 0);

        //The subsets themselves need to be sorted for which we will need to use a comparator
        Collections.sort(ans, new Comparator<ArrayList<Integer>>(){

            @Override
            public int compare(ArrayList<Integer> o1, ArrayList<Integer> o2) {

                //Loop through each element in first list and check if any of the element is smaller/larger
                for(int i=0; i<o1.size() && i<o2.size(); i++){
                    if(o1.get(i) > o2.get(i)) return 1;
                    if(o1.get(i) < o2.get(i)) return -1;
                }

                //If all elements in list1 are same as list2, we need to compare their size
                if(o1.size() > o2.size()) return 1;
                else return -1;

            }

        });

        return ans;

    }

    public void helper(ArrayList<Integer> list, ArrayList<Integer> current, int i){

        //When i is equal to size of the list, we must have taken a decision whether to include or exclude each of the elements
        //At this point, we can add the subset we have generated
        if(i == list.size()){
            //Note that we cannot simply do: ans.add(current)
            //The reference would be the same, so the same list would get modified
            ans.add(new ArrayList<>(current));
            return;
        }

        //Including the current element
        current.add(list.get(i));
        helper(list, current, i+1);

        //Excluding the current element
        //We need to remove from current list because we had added it in the previous step
        current.remove(current.size()-1);
        helper(list, current, i+1);

    }
}
```
