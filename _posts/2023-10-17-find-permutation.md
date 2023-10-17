---
layout: post
title: Find Permutation
date: 2023-10-17 20:00 +0530
author: "Gaurav Kumar"
tags: "java stack arrays leetcode leetcodealgo100"
categories: "stack"
---

## PROBLEM DESCRIPTION

A permutation `perm` of `n` integers of all the integers in the range `[1, n]` can be represented as a string `s` of length `n - 1` where:

- `s[i] == 'I'` if `perm[i] < perm[i + 1]`, and
- `s[i] == 'D'` if `perm[i] > perm[i + 1`.

Given a string `s`, reconstruct the lexicographically smallest permutation `perm` and return it.

[leetcode](https://leetcode.com/problems/find-permutation/)

## SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/find_permutation.png)

```java
class Solution {

    public int[] findPermutation(String s) {

        // Create an array to store the result permutation.
        int[] ans = new int[s.length() + 1];

        // Create a stack to help construct the permutation.
        Stack<Integer> stack = new Stack<>();

        // Initialize an index to keep track of the position in the result permutation. (the next number to be added)
        int position = 0;

        // Initialize a variable to represent the current number being considered for the permutation.
        int idx = 1;

        // Iterate through the characters in the input string 's'.
        for (int i = 0; i < s.length(); i++) {

            // Push the current number 'idx' onto the stack.
            stack.push(idx);

            // Get the current character at position 'i'.
            Character ch = s.charAt(i);

            // If the character is 'I', it indicates that we need to find increasing sequence.
            if (ch == 'I') {

                // Pop numbers from the stack and place them in the result permutation (essentially reversing the numbers from DDDDD...I)
                while (!stack.isEmpty()) {
                    ans[position] = stack.pop();
                    position++;
                }

            }
            // If the character is 'D', we only need to add it to the stack which is already done

            // Increment the current number 'idx'.
            idx++;
        }

        // The previous loop run s.length() times. there will be at least one position left to be filled. It can be more, if there were more Ds at the end
        // For example: IIIIDDDDDD (at the end, since I was never encountered, the number added to stack after Ds are left to be appended to the answer)
        stack.push(idx);
        while (!stack.isEmpty()) {
            ans[position] = stack.pop();
            position++;
        }

        return ans;
    }
}
```
