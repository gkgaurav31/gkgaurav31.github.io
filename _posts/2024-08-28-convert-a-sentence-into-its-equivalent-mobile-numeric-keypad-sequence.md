---
layout: post
title: Convert a sentence into its equivalent mobile numeric keypad sequence (geeksforgeeks - SDE Sheet)
date: 2024-08-28 23:07 +0530
author: "Gaurav Kumar"
tags: "java strings geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a sentence in the form of a string in uppercase, convert it into its equivalent mobile numeric keypad sequence. Please note there might be spaces in between the words in a sentence and we can print spaces by pressing 0.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/convert-a-sentence-into-its-equivalent-mobile-numeric-keypad-sequence0547/1?page=3)

## SOLUTION

```java
class Solution
{
    String printSequence(String S)
    {

        int[] chars = {2, 22, 222, 3, 33, 333, 4, 44, 444, 5, 55, 555, 6, 66, 666, 7, 77, 777, 7777, 8, 88, 888, 9, 99, 999, 9999};

        StringBuffer sb = new StringBuffer();

        for(int i=0; i<S.length(); i++){

            if(S.charAt(i) == ' '){
                sb.append("0");
            }else
                sb.append(chars[S.charAt(i) - 'A']);

        }

        return sb.toString();

    }
}
```
