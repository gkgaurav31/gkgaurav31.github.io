---
layout: post
title: String And Its Frequency (InterviewBit)
date: 2024-02-22 20:13 +0530
author: "Gaurav Kumar"
tags: "java strings"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a string A with lowercase english alphabets and you have to return a string in which, with each character its frequency is written in adjacent.

## SOLUTION

```java
public class Solution {

    public String solve(String A) {

        int n = A.length();
        int[] freq = new int[26];

        // get current character and increase its frequency
        for(int i=0; i<n; i++){
            char ch = A.charAt(i);

            // ch - 'a' will map the characters based on 0 index in the array
            freq[ch - 'a']++;
        }

        StringBuffer sb = new StringBuffer();
        Set<Character> visited = new HashSet<>();

        // for each character in the given string
        for(int i=0; i<n; i++){

            // get current char
            char ch = A.charAt(i);

            // if its frequency was not already notes earlier
            if(!visited.contains(ch)){

                // add the current char and its freq
                sb.append(String.valueOf(ch));
                sb.append(String.valueOf(freq[ch-'a']));

                // mark as visited
                visited.add(ch);

            }

        }

        return sb.toString();

    }

}
```

## SMALL SPACE OPTIMIZATION

Instead of using HashSet to track the visited array, we can reuse the exiting frequency array itself. We can get the current character. If its frequency is more than 0, append it to the answer String. Also, set its frequency to 0. If we do this, when we encouter the same character again, it will not get appended again, since its frequency is now 0.

```java
public class Solution {

    public String solve(String A) {

        int n = A.length();
        int[] freq = new int[26];

        for(int i=0; i<n; i++){

            char ch = A.charAt(i);

            freq[ch - 'a']++;

        }

        StringBuffer sb = new StringBuffer();

        for(int i=0; i<n; i++){

            char ch = A.charAt(i);

            // if freq of current char is more than 0
            if(freq[ch-'a'] > 0){

                sb.append(String.valueOf(ch));
                sb.append(String.valueOf(freq[ch-'a']));

                // set the freq of current char to 0
                // this will avoid appending it again if there are duplicates
                freq[ch-'a'] = 0;

            }

        }

        return sb.toString();

    }

}
```
