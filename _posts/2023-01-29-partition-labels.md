---
layout: post
title: Partition Labels
date: 2023-01-29 17:51 +0530
author: "Gaurav Kumar"
tags: "java leetcode greedy neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

You are given a string s. We want to partition the string into as many parts as possible so that each letter appears in at most one part.
Note that the partition is done so that after concatenating all the parts in order, the resultant string should be s.
Return a list of integers representing the size of these parts.

[leetcode](https://leetcode.com/problems/partition-labels/description/)

### SOLUTION

Create a HashMap to store the start and end index of each character in the string. The start and end index of the first character will form the starting window size. However, this may need to be extended if any character within this window out of the window. So, we can iterate through this windows, and update the right side (end) of the window on the basis of lastIndex. This is the minimum window needed. The next window will start from R+1. Using the same logic, we check the minimum size of that window.

```java
class Solution {

    public List<Integer> partitionLabels(String s) {

        //HashMap to store first and last occurance of all characters in the string
        Map<Character, Pair> map = new HashMap<>();
        for(int i=0; i<s.length(); i++){
            Character c = s.charAt(i);
            if(!map.containsKey(c)){
                map.put(c, new Pair(i, i));
            }else{
                map.put(c, new Pair(map.get(c).startIndex, i));
            }
        }

        //init
        int idx = 0;

        //window start and end
        int l=0;
        int r=0;

        //answer list
        List<Integer> ans = new ArrayList<>();

        //go through all the characters in the string
        while(idx < s.length()){
            
            //init -> window size as startIndex to endIndex of the left most element
            l = map.get(s.charAt(idx)).startIndex;
            r = map.get(s.charAt(idx)).endIndex;
            
            //there can be a character in this window which is out of the window
            //in that case, we need to update the right of the window

            //temp int to store the right most index
            int t = l;
            while(t <= r && t < s.length()){

                //update end of the window if it's beyong the current window
                r = Math.max(r, map.get(s.charAt(t)).endIndex);

                //check the next character
                t++;
            }

            //next window will start from R+1
            idx = r+1;

            //add the size to the answer list
            ans.add(r-l+1);

        }

        return ans;

    }

}

class Pair{

    int startIndex;
    int endIndex;

    public Pair(int s, int e){
        this.startIndex = s;
        this.endIndex = e;
    }

}
```
