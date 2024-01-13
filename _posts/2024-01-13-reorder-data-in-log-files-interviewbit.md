---
layout: post
title: Reorder Data in Log Files (InterviewBit)
date: 2024-01-13 20:30 +0530
author: "Gaurav Kumar"
tags: "java interviewbit arrays"
categories: "interviewbit"
---

## Problem Description

Given an integer array A, find if an integer p exists in the array such that the number of integers greater than p in the array equals to p.

### Solution

We can create a custom Comparator to sort the String in the list.

The Compator does the following:

- Get the key (identifier) and value (log) using the first index of the delimeter.
- The problem says that all characters in the log are either letters or digits, not both. So, we can just check the first character from the value to check if that is a digit log or a letter log.
- If both are digit-log, return 0 to maintain the relative ordering.
- If s1 is letter log and s2 is digit log, return -1 so that s1 comes first.
- If s1 is digit log and s2 is letter log, return 1 so that s2 comes first.
- If both are letter log:
  - First try to compare the values of both s1 and s2
  - If `value1.compareTo(value2)` is not 0, we can return it.
  - If `value1.compareTo(value2)` is 0, which means both the values are same, we need to order by the corresponding key. This can be done using `key1.compareTo(key2)`.

```java
   import java.math.BigDecimal;

public class Solution {
    public ArrayList<String> reorderLogs(ArrayList<String> A) {
        Collections.sort(A, new MyComparator());
        return A;
    }
}

class MyComparator implements Comparator<String>{

    private static final Character DELIMITER = '-';

    @Override
    public int compare(String s1, String s2){

        //get the index of first space (delimiter)
        int idx1 = s1.indexOf(DELIMITER);
        int idx2 = s2.indexOf(DELIMITER);

        String key1 = s1.substring(0, idx1);
        String key2 = s2.substring(0, idx2);

        //don't replace "-" in the values
        String val1 = s1.substring(idx1 + 1);
        String val2 = s2.substring(idx2 + 1);

        //check if first character of log is a number (as per the question, all will be either numbers or letters)
        boolean isNumberS1 = Character.isDigit(val1.charAt(0));
        boolean isNumberS2 = Character.isDigit(val2.charAt(0));

        //if both are digit-logs
        if(isNumberS1 && isNumberS2){
            return 0; //maintain the relative order as in the list
        }

        if(!isNumberS1 && isNumberS2) return -1; //s1 is a letter-log but s2 is not, so s1 should come first
        if(isNumberS1 && !isNumberS2) return 1; //s2 is a letter-log but s1 is not, so s2 should come first

        //both are letter-logs
        int valueComparison = val1.compareTo(val2);
        return (valueComparison != 0) ? valueComparison : key1.compareTo(key2); //if values are same, sort by key


    }

}
```
