---
layout: post
title: ZigZag Conversion
date: 2023-07-28 00:18 +0530
author: "Gaurav Kumar"
tags: "java arrays neetcode leetcode leetcode150 important"
categories: "arrays"
---

## PROBLEM DESCRIPTION

The string "PAYPALISHIRING" is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)

```text
P   A   H   N
A P L S I I G
Y   I   R
```

And then read line by line: "PAHNAPLSIIGYIR"

Write the code that will take a string and make this conversion given a number of rows:

string convert(string s, int numRows);

[leetcode](https://leetcode.com/problems/zigzag-conversion/)

## SOLUTION

[NeetCode YouTube](https://www.youtube.com/watch?v=Q2Tw6gcVEwc)

![snapshot]({{ site.baseurl }}/assets/img/zig_zag_conversion.png)

```java
class Solution {
    
    public String convert(String s, int numRows) {

        //edge case
        if(numRows == 1) return s;

        //length of the string
        int n = s.length();

        //number of jumps to go to next character in the next group
        int jumps = 2*(numRows-1);

        //ans
        StringBuffer sb = new StringBuffer();

        //first row
        //it will start from index 0
        int i = 0;

        //while there are more characters, keep jumping 2*(numRows-1) times to go to next character in the next section of first row
        while(i<n){
            sb.append(s.charAt(i));
            i += jumps;
        }

        //rows in between
        //loop through the rows from [1, numRows-2]
        for(int row=1; row<=numRows-2; row++){
            
            //first character of row x, will be at index x
            i = row;

            //while there are more characters in the string
            while(i<n){
                
                //append the current character
                sb.append(s.charAt(i));

                //Important Part:
                //the next character in this row will be present at: currentIndex + 2*(numRows-1) - (2 * currentRow)
                int j = i + jumps - (2*row);

                //check if that index is within the bounds
                if(j<n){
                    sb.append(s.charAt(j));
                }

                //jump to the next character of the next group
                i += jumps;

            }

        }

        //last row (similar to first row)
        //the first character will be at numRows-1
        i = numRows-1;
        while(i<n){
            sb.append(s.charAt(i));
            i += jumps;
        }

        return sb.toString();


    }

}
```
