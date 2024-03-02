---
layout: post
title: Valid Parentheses
date: 2023-08-04 20:29 +0530
author: "Gaurav Kumar"
tags: "java stack leetcode leetcode150"
categories: "stack"
---

## PROBLEM DESCRIPTION

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

- Open brackets must be closed by the same type of brackets.
- Open brackets must be closed in the correct order.
- Every close bracket has a corresponding open bracket of the same type.

[leetcode](https://leetcode.com/problems/valid-parentheses/)

## SOLUTION

```java
class Solution {

    public boolean isValid(String s) {

        int n = s.length();

        // Create a mapping of closing brackets to their corresponding opening brackets.
        Map<Character, Character> map = new HashMap<>();
        map.put(')', '(');
        map.put(']', '[');
        map.put('}', '{');

        // Set to store the open brackets.
        Set<Character> openBrackets = new HashSet<>();
        openBrackets.add('(');
        openBrackets.add('{');
        openBrackets.add('[');

        // Create a stack to keep track of the brackets.
        Stack<Character> stack = new Stack<>();

        // Iterate through each character in the string.
        for (int i = 0; i < n; i++) {

            Character c = s.charAt(i);

            // If the current character is an open bracket, push it onto the stack.
            if (openBrackets.contains(c)) {
                stack.push(c);
            // If will be a close bracket
            } else {

                // If the stack is empty, there's no corresponding open bracket, so the string is invalid.
                if (stack.isEmpty()) {
                    return false;
                }

                // Check if the current closing bracket matches the one at the top of the stack.
                if (stack.peek() == map.get(c)) {
                    stack.pop(); // Pop the open bracket from the stack to remove this matched pair
                } else {
                    return false; // If there's a mismatch, the string is invalid.
                }

            }

        }

        // If the stack is empty, all brackets were matched and popped, so the string is valid.
        return stack.isEmpty();
    }
}
```

## ANOTHER WAY TO CODE

```java
class Solution {

    public boolean isValid(String s) {

        Map<Character, Character> map = new HashMap<>();

        map.put(')', '(');
        map.put(']', '[');
        map.put('}', '{');

        int n = s.length();

        Stack<Character> stack = new Stack<>();

        for(int i=0; i<n; i++){

            char ch = s.charAt(i);

            // if it's not a closing bracket, push to stack
            if(!map.containsKey(ch)){
                stack.push(ch);

            // current char is a closing bracket
            // so, there must corresponding open bracket in the stack at the top
            }else{

                // if stack is empty, return false
                if(stack.isEmpty())
                    return false;

                // check if the right closing bracket is at the top of the stack
                // if not, return false
                if(stack.pop() != map.get(ch))
                    return false;

            }

        }

        // finally, the stack should be empty for a valid string
        return stack.isEmpty();

    }

}
```
