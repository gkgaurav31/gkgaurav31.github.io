---
layout: post
title: Vowel and Consonant Substrings! (InterviewBit)
date: 2024-02-28 21:05 +0530
tags: "java strings"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a string `A` consisting of lowercase characters.

You have to find the number of substrings in `A` which starts with vowel and end with consonants or vice-versa.

Return the count of substring modulo `10^9 + 7`.

## SOLUTION

```java
public class Solution {

    int M = (int) Math.pow(10, 9) + 7;

    public int solve(String A) {

        int n = A.length();

        // Set to store vowels
        Set<Character> vowels = new HashSet<>(Arrays.asList('a', 'e', 'i', 'o', 'u'));

        // Prefix array to store count of vowels to the right of each character
        int[] pf = new int[n];

        // Building prefix array
        for(int i=n-2; i>=0; i--){
            if(vowels.contains(A.charAt(i+1))){
                pf[i] = pf[i+1] + 1;
            }else
                pf[i] = pf[i+1];
        }

        int total = 0;

        // Iterate through the string to count substrings
        for(int i=0; i<n-1; i++){

            char ch = A.charAt(i); // Current character

            // Count of vowels and consonants to the right of current character
            int numberOfVowelsOnRight = pf[i];
            int numberOfConsonantsOnRight = n - (i+1) - numberOfVowelsOnRight; // total chars - skip previous chars - vowels count on right

            // if the current char is a vowel, it should end with a consonant
            if(vowels.contains(ch)){

                // Add count of consonants to the total, considering modulo
                total = (total%M + numberOfConsonantsOnRight%M)%M;

            }else{ // if the current char is a consonant, it should end with a vowel

                // Add count of vowels to the total, considering modulo
                total = (total%M + numberOfVowelsOnRight%M)%M;

            }

        }

        return total;

    }

}
```
