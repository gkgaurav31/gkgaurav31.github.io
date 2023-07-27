---
layout: post
title: Reverse Words in a String
date: 2023-07-27 20:38 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcode150"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Given an input string s, reverse the order of the words.
A word is defined as a sequence of non-space characters. The words in s will be separated by at least one space.
Return a string of the words in reverse order concatenated by a single space.
Note that s may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

[leetcode](https://leetcode.com/problems/reverse-words-in-a-string/)

## SOLUTION (WITHOUT IN-BUILT METHODS)

We can easily use split function and iterate through the String array from right to left to get the result. To achieve the same using just a simple loop, we can do it in the following way:

```java
class Solution {
    
    public String reverseWords(String s) {

        int n = s.length();
        
        int i = n-1;
        int j = n-1;

        //skip the trailing white space
        //(we could have simply used trim() method also)
        while(s.charAt(i) == ' '){
            i--; j--;
        }

        //ans string
        StringBuilder sb = new StringBuilder();

        //while there are character to check
        while(i>=0){
            
            //if the char at i is not empty, go to left
            while(i>=0 && s.charAt(i) != ' '){
                i--;
            }

            //the char at i must be empty, so we have found a word from [i+1,j]
            //append that to the ans string
            for(int x=i+1; x<=j; x++){
                sb.append(s.charAt(x));
            }

            //set i,j to the end of next word from right
            //there can be empty space, so skip them
            j=i;
            while(i>=0 && s.charAt(i) == ' '){
                j--; i--;
            }

            //add a space since this is not the last word
            if(i>=0) sb.append(" ");

        }

        return sb.toString();

    }

}
```
