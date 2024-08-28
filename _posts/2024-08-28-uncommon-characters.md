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

### OPTIMIZED

Instead of using HashSet, we can directly leverage the characters array[26].

- mark all postions with `1` if that character is present in stringA
- iterate over the character of stringB.
  - if the value for that character is already 1, we know it's present in stringA, so we change it to `-1` to mark it as invalid
  - if the value for that character is 0, we want to mark it. we cannot mark it with `1` because it will affect the future checks. For other characters, how will it identify if this `1` was for stringA or stringB? So, we need a different marker. In this case, we are marking it as `2`.
  - Now all we need to do is to form a string using the characters array for positions which are either marked `1` or `2`

```java
class Solution
{
    String UncommonChars(String A, String B)
    {
        int[] chars = new int[26];


        for(int i=0; i<A.length(); i++){
            chars[A.charAt(i) - 'a'] = 1;
        }

        for(int i=0; i<B.length(); i++){

            char ch = B.charAt(i);

            if(chars[ch-'a'] == 0){
                chars[ch-'a'] = 2;
            }else if(chars[ch-'a'] == 1)
                chars[ch-'a'] = -1;

        }

        StringBuffer sb = new StringBuffer();

        for(int i=0; i<26; i++){
            if(chars[i] == 1 || chars[i] == 2){
                sb.append(String.valueOf((char)(i + 'a')));
            }
        }

        return sb.toString().equals("")?"-1":sb.toString();

    }



}
```
