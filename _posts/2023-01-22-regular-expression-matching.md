---
layout: post
title: Regular Expression Matching
date: 2023-01-22 20:41 +0530
author: "Gaurav Kumar"
tags: "java leetcode dynamic_programming neetcode150"
categories: "neetcode150"
---

## PROBLEM DESCRIPTION

Given an input string s and a pattern p, implement regular expression matching with support for '.' and '*' where:

- '.' Matches any single character.​​​​
- '*' Matches zero or more of the preceding element.

The matching should cover the entire input string (not partial).

[leetcode](https://leetcode.com/problems/regular-expression-matching/description/)

### SOLUTION

```java
class Solution {

    public boolean isMatch(String s, String p) {
        return matchHelper(s, p, s.length()-1, p.length()-1, new HashMap<String, Boolean>());
    }

    public boolean matchHelper(String s, String p, int i, int j, Map<String, Boolean> map){
        
        //if all characters in s and p are over, return true
        if(i == -1 && j == -1) return true;

        //if there are no characters in the pattern, but characters in s are still left
        if(j == -1) return false; 

        //IMPORTANT
        //if there are no characters in s but patter is remaining
        //Example: 
        //string s = "a", p="b*c*a*"
        if(i == -1){

            //loop through remaining characters in the pattern
            for(int k=j; k>=0; k--){

                //if the character in pattern is *, it should be okay as we can take zero instances of preceding character
                //for next iteration, we need to move 2 characters back
                if(p.charAt(k) == '*'){
                    k--; //we need to move 2 characters back. k-- here, and another one will be done by for loop
                }else{
                    //if the next character is not *, return false as there is nothing in the string s which can match it
                    return false;
                }
            }
            return true;
        }

        if(!map.containsKey(i+":"+j)){

            boolean match = false;

            //if characters in the string and pattern match OR pattern has "." which can match with a single character
            if(s.charAt(i) == p.charAt(j) || p.charAt(j) == '.'){
                
                //check the remaining characters in string and pattern
                //so we reduce both i and j by 1
                match = matchHelper(s, p, i-1, j-1, map);

            //otherwise, check if pattern has a *
            }else if(p.charAt(j) == '*'){
                
                //we can match the * with empty character
                //so, i will remain at the same position
                //but, j will move by 2 steps (as we need to skip the preceding element too - like [a*])
                match = matchHelper(s, p, i, j-2, map); 

                //if the preciding element patches the mattern of if it's a "." which would match with any characters
                if(s.charAt(i) == p.charAt(j-1) || p.charAt(j-1) == '.'){

                    //move index i by 1 as we can match 1 character
                    //IMPORTANT: we will keep the j at the same position so that it can continue to match further if there are more instances of the same char
                    match = match || matchHelper(s, p, i-1, j, map);
                }

            }//else match is not found

            map.put(i+":"+j, match);

        }

        return map.get(i+":"+j);

    }

}
```
