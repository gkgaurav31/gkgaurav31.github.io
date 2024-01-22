---
layout: post
title: Find Permutation (InterviewBit)
date: 2024-01-22 21:58 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays important"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a positive integer `n` and a string `s` consisting only of letters `D` or `I`, you have to find any permutation of first `n` positive integer that satisfy the given input string.

- `D` means the next number is smaller, while `I` means the next number is greater.

Note:

- Length of given string `s` will always equal to `n - 1`
- Your solution should run in linear time and space.

Example :

`n = 3`  
`s = ID`

Return: `[1, 3, 2]`

[InterviewBit](https://www.interviewbit.com/problems/find-permutation/)

## SOLUTION

If s is `III`, the result will look like: `1 2 3 4`. The result will have one more element to make it in increasing/decreasing order.

If s is `DDD`, the result will look like: `4 3 2 1`.

The range of numbers is going to be `[1, s.length() + 1]`

There are two cases:

- If the next character is `I`: take the lowest number possible
- If the next character is `D`: take the highest number possible

At each point, when the current lowest value has been used up, we want to take the next one. So we increment the lowest value.

Similarly, when the current largest value has been used up, we want to use the next one. So we decrement the highest value.

At the end, both low and high values will be equal and that would actually be the last element that needs to be added.

```java
public class Solution {

    public ArrayList<Integer> findPerm(final String A, int B) {

        int n = A.length();

        ArrayList<Integer> list = new ArrayList<>();

        int small = 1;
        int large = B;

        for(int i=0; i<n; i++){

            char ch = A.charAt(i);

            if(ch == 'I'){
                list.add(small);
                small++;
            }else{
                list.add(large);
                large--;
            }

        }

        list.add(small); //list.add(large) will also work

        return list;

    }
}
```
