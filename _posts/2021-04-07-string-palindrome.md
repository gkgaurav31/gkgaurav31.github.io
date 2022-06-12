---
layout: post
title: String Palindrome
author: "Gaurav Kumar"
tags: "java strings"
categories: "interviewbit"

---

## Problem Description: [Palindrome String](https://www.interviewbit.com/problems/palindrome-string/)

```java
package com.gauk;

public class Solution {

/*
    * Given a string, determine if it is a palindrome, considering only
    * alphanumeric characters and ignoring cases.
    * 
    * Example:
    * 
    * "A man, a plan, a canal: Panama" is a palindrome.
    * 
    * "race a car" is not a palindrome.
    * 
    * Return 0 / 1 ( 0 for false, 1 for true ) for this problem
    */

public static void main(String[] args) {
    System.out.println(new Solution().isPalindrome("race a car"));
}

public int isPalindrome(String A) {
    
    //replace all non-alphanumeric characters to white space and change them to lower case
    A = A.replaceAll("[^A-Za-z0-9]", "").toLowerCase();
    
    //loop through the elements and check if the are equal. We can do it only for half the elements to reduce time.
    for(int i=0; i<A.length(); i++) {
        
        if(A.charAt(i) != A.charAt(A.length()-i-1)) { 
            return 0;
        }
    }
    
    return 1;

}

}
```
