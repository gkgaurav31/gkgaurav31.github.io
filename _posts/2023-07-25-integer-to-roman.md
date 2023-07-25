---
layout: post
title: Integer to Roman
date: 2023-07-25 14:34 +0530
author: "Gaurav Kumar"
tags: "java arrays leetcode leetcode150"
categories: "arrays"
---

## PROBLEM DESCRIPTION

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

```text
Symbol       Value
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

For example, 2 is written as II in Roman numeral, just two one's added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

- I can be placed before V (5) and X (10) to make 4 and 9.
- X can be placed before L (50) and C (100) to make 40 and 90.
- C can be placed before D (500) and M (1000) to make 400 and 900.

Given an integer, convert it to a roman numeral.

[leetcode](https://leetcode.com/problems/integer-to-roman/)

## SOLUTION

The idea is to start from the largest symbol (having the maximum possible value). Check how many times that can be used. Reduce the given number based on this and keep doing this for the remaining part using next symbols having next largest value.

```java
class Solution {
    
    public String intToRoman(int num) {
        
        //init: mapping between the possible symbol/pairs and their values
        String[] symbols = new String[]{"M","CM","D","CD","C","XC","L","XL","X","IX","V","IV","I"};
        int[] values = new int[]{1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5,4, 1};

        //answer in roman
        StringBuilder sb = new StringBuilder();

        //we will start from the largest value and go to smaller (greedy)
        int idx = 0;

        //until the number becomes 0
        while(num != 0){
            
            //if the current symbol is greater than the number, we cannot use it
            //try with next number
            if(values[idx] > num){
                idx++; 
                continue;
            }

            //otherwise, use the current symbol
            String symbol = symbols[idx];
            //the number of times we can use it will be equal to num / (value of current symbol)
            int times = num/values[idx];
            
            //append the current symbol to the ans based on number of times calculated above
            for(int i=0; i<times; i++){
                sb.append(symbol);
            }

            //reduce the number for the remaining values
            //Example: 2005
            //symbol: M, times 2
            //num will be updated to: num%M = 5
            //This is equivalent of: num = num - (values[idx] * times);
            num = num%values[idx];
            
            //go to next symbol
            idx++;

        }

        return sb.toString();

    }

}
```
