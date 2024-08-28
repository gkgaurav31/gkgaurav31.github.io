---
layout: post
title: Uncommon characters (geeksforgeeks - SDE Sheet)
date: 2024-08-28 21:53 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two strings A and B consisting of lowercase english characters. Find the characters that are not common in the two strings.

Note :- Return the string in sorted order.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/uncommon-characters4932/1?page=3)

## SOLUTION

- create two Sets which contains all the characters present in A and B strings
- create an array of size 16. Currently it will have all zeroes. If we get a Character which is present in A but not in B, we will mark that position in array as 1. Similarly, if a character is present in B but not in A, we will mark it with 1.
- iterate over the array with 0/1 for the character. All positions with 1 will form the final string which needs to be returned as the answer.

```java
class Solution
{
    String UncommonChars(String A, String B)
    {
        int[] chars = new int[26];

        Set<Character> set1 = getCharacterSet(A);
        Set<Character> set2 = getCharacterSet(B);

        for(int i=0; i<A.length(); i++){

            if(!set2.contains(A.charAt(i)))
                chars[A.charAt(i) - 'a'] = 1;

        }

        for(int i=0; i<B.length(); i++){

            if(!set1.contains(B.charAt(i)))
                chars[B.charAt(i) - 'a'] = 1;

        }

        StringBuffer sb = new StringBuffer();

        for(int i=0; i<26; i++){
            if(chars[i] == 1){
                sb.append(String.valueOf((char)(i + 'a')));
            }
        }

        return sb.toString().equals("")?"-1":sb.toString();

    }

    Set<Character> getCharacterSet(String s){

        Set<Character> set = new HashSet<>();

        for(int i=0; i<s.length(); i++){
            set.add(s.charAt(i));
        }

        return set;

    }

}
```
