---
layout: post
title: Palindrome Partitioning
date: 2023-01-07 11:15 +0530
author: "Gaurav Kumar"
tags: "java recursion backtracking"
categories: "backtracking"
---

## Problem Description

Given a string s, partition s such that every substring of the partition is a palindrome. Return all possible palindrome partitioning of s.

[leetcode](https://leetcode.com/problems/palindrome-partitioning/description/)

### Solution

Start forming substrings starting from index 0 to say index i. If that is a palindome, then recursively check for substring starting from index i to j. Keep doing this until all the characters have been considered in the input string. At that point, we have got one possible answer, so we can add that to the answer list.

```java
class Solution {

    public List<List<String>> partition(String s) {
        partitionHelper(s, 0, new ArrayList<>());
        return ans;
    }

    List<List<String>> ans = new ArrayList<>();

    public void partitionHelper(String s, int idx, List<String> list){
        
        //idx is tracking the index from where we need to start the next substring
        //if it's equal to the length of input string s, we have considered all the characters
        //at this point, all the previos substrings considered were palindomes, we we can add it to answer list and return
        if(idx >= s.length()){
            ans.add(new ArrayList<>(list));
            return;
        }

        //start from idx index and keep taking substrings with length 1, 2, 3 ... 
        //if the substring formed is a palindrome, start looking at next possible substring recursively
        for(int i=idx; i<s.length(); i++){
            
            //current substring
            String sub = s.substring(idx, i+1);

            //if it's a palindrome
            if(isPalindrome(sub)){

                //add it to the list
                list.add(sub);

                //recursively check for other palindrome substring
                //IMPORTANT: we start looking for the current index we are currently at
                partitionHelper(s, i+1, list);

                //backtrack: remove the last substring which was added
                list.remove(list.size()-1); 
            }

        }

    }

    //this can be further optimized by taking two pointers at the start and end
    public boolean isPalindrome(String s1){

        StringBuilder s2 = new StringBuilder(s1);
        s2.reverse();

        if(s1.equals(s2.toString())) return true;

        return false;

    }
    
}
```
