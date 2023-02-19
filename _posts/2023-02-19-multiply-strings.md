---
layout: post
title: Multiply Strings
date: 2023-02-19 21:41 +0530
author: "Gaurav Kumar"
tags: "java neetcode150 mathematics"
categories: "neetcode150"
---

This question is part of [NeetCode150 series](https://neetcode.io/practice).  

## Problem Description

Given two non-negative integers num1 and num2 represented as strings, return the product of num1 and num2, also represented as a string.

Note: You must not use any built-in BigInteger library or convert the inputs to integer directly.

[leetcode](https://leetcode.com/problems/multiply-strings/description/)

### Solution

```java
class Solution {
    
    public String multiply(String num1, String num2) {

        //edge case
        if(num1.equals("0") || num2.equals("0")) return "0";

        //list of numbers to add
        List<String> numbers = new ArrayList<>();

        //for each digit in num2, multiply it with num1, and insert the temp result in the above list 
        //then add all numbers in the list to get the final result
        for(int i=num2.length()-1; i>=0; i--){
            
            //how many zeroes to append at the end of each temp answer
            int p = num2.length()-1 - i;

            int currentNum2Digit = Integer.parseInt(String.valueOf(num2.charAt(i)));

            StringBuffer total = new StringBuffer();
            int carry = 0;

            for(int j=num1.length()-1; j>=0; j--){
                
                int currentNum1Digit = Integer.parseInt(String.valueOf(num1.charAt(j)));

                int temp = (currentNum1Digit * currentNum2Digit) + carry;
                total.insert(0, temp%10);
                carry = temp/10;

            }

            if(carry > 0) total.insert(0, carry);

            StringBuffer zeroes = new StringBuffer(); 

            for(int k=0; k<p; k++){
                zeroes.append("0");
            }

            numbers.add(total + "" + zeroes);

        }

        return addAll(numbers);

    }

    //add all numbers in the list of numbers represented as string
    public String addAll(List<String> list){

        String ans = list.get(0);

        for(int i=1; i<list.size(); i++){
            ans = addTwoNumbers(ans, list.get(i));           
        }

        return ans;

    }

    //add two numbers as strings
    public String addTwoNumbers(String n1, String n2){

        if(n1.length() < n2.length()){
            String temp = n1;
            n1 = n2;
            n2 = temp;
        }

        int diff = Math.abs(n1.length()-n2.length());

        StringBuffer zeroes = new StringBuffer(); 

        for(int i=0; i<diff; i++){
            zeroes.append("0");
        }

        n2 = zeroes + n2;

        StringBuffer ans = new StringBuffer();
        int carry = 0;

        for(int i=n1.length()-1; i>=0; i--){
            
            int d1 = Integer.parseInt(String.valueOf(n1.charAt(i)));
            int d2 = Integer.parseInt(String.valueOf(n2.charAt(i)));
            
            int tempSum = d1 + d2 + carry;

            ans.insert(0, tempSum%10);
            carry = tempSum/10;

        }

        if(carry > 0) ans.insert(0, carry);

        return ans.toString();

    }

}
```
