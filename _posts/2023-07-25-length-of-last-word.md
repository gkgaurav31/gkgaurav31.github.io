---
layout: post
title: Length of Last Word
date: 2023-07-25 14:49 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcode150"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Given a string s consisting of words and spaces, return the length of the last word in the string.

A word is a maximal substring consisting of non-space characters only.

[leetcode](https://leetcode.com/problems/length-of-last-word/)

## SOLUTION

### TWO LOOPS

This is an easy question specially if you use inbuilt function to trim/split the string. It can also be solved using two loops:
    - First loop to skip the training spaces
    - Second loop to calculate the length (keep iterating from back until we get an empty character)

```java
class Solution {

    public int lengthOfLastWord(String s) {

        int length = 0;

        int idx = s.length()-1;

        //skip trailing spaces
        while(true){

            if(s.charAt(idx) == ' '){
                idx--;
            }else{
                break;
            }

        }

        while(idx>=0 && s.charAt(idx) != ' '){
            length++;
            idx--;
        }

        return length;

    }

}
```

### SINGLE LOOPS

```java
class Solution {

    public int lengthOfLastWord(String s) {

        int length = 0;

        //iterate from right to left
        int idx = s.length() - 1;

        while(idx >= 0){
            
            //if character is not a blank
            if(s.charAt(idx) != ' '){

                //increase the length
                length++;
            
            //if it's a blank, check the value of length
            //if length is 0, no need to do anything
            //if length is more than 0, it must have been due to the last word, in which case we can return "length"
            }else if(length > 0){
                return length;
            }
            
            //go to next character on the left side
            idx--;

        }

        return length;

    }

}
```
