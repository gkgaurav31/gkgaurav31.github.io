---
layout: post
title: Deserialize (InterviewBit)
date: 2024-02-22 20:05 +0530
tags: "java strings"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

You are given a string `A` which is a serialized string. You have to restore the original array of strings.

The string in the output array should only have lowercase english alphabets.

Serialization: Scan each element in a string, calculate its length and append it with a string and a element separator or deliminator (the deliminator is `~`). We append the length of the string so that we know the length of each element.

For example, for a string 'interviewbit', its serialized version would be 'interviewbit12~'.

## SOLUTION

```java
public class Solution {

    public String[] deserialize(String A) {

        String[] arr = A.split("~");

        int n = arr.length;
        String[] ans = new String[n];

        for(int i=0; i<n; i++){

            StringBuffer sb = new StringBuffer();

            int idx = 0;
            while(arr[i].charAt(idx) >= 'a' && arr[i].charAt(idx) <= 'z'){
                sb.append(String.valueOf(arr[i].charAt(idx)));
                idx++;
            }

            ans[i] = sb.toString();

        }

        return ans;

    }

}
```
