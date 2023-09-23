---
layout: post
title: One Edit Distance
date: 2023-09-23 22:34 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcodealgo100"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Given two strings s and t, return true if they are both one edit distance apart, otherwise return false.

A string s is said to be one distance apart from a string t if you can:

- Insert exactly one character into s to get t.
- Delete exactly one character from s to get t.
- Replace exactly one character of s with a different character to get t.

[leetcode](https://leetcode.com/problems/one-edit-distance/)

## SOLUTION

```java
class Solution {

    public boolean isOneEditDistance(String s, String t) {

        //get lengths of both strings
        int ns = s.length();
        int nt = t.length();

        //we will make sure that ns string is of smaller length (just for convenience later)
        if(ns > nt){
            return isOneEditDistance(t, s);
        }

        //if target string has 2 or more extra characters, we cannot create it, so return false
        if(nt - ns > 1)
            return false;

        //keep comparing for each character starting from index 0
        for(int i=0; i<ns; i++){

            //if the characters do not match at index i, then we have two possibilities:
            //Possibility 1: target and source have same length:
            //example: abxcd, abycd
            //to be valid, rest of the substring from index i+1 should match for both
            //Possibility 2: they have different length:
            //example: abcd, abxcd
            //we can make it valid by deleting character at index i in target string
            //in that case, substring from index i of source should be same as substring from index i+1 of target
            if(s.charAt(i) != t.charAt(i)){

                if(ns == nt){
                    return s.substring(i+1).equals(t.substring(i+1));
                }else{
                    return s.substring(i).equals(t.substring(i+1));
                }

            }

        }

        //if all characters match, the length should be different by at least 1 character to be valid
        //example: s="" and t="" -> result should be false
        return nt - ns == 1; //return nt != ns will also work since we've already checked that diff in length is 1

    }

}
```
