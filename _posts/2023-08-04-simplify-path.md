---
layout: post
title: Simplify Path
date: 2023-08-04 22:04 +0530
author: "Gaurav Kumar"
tags: "java stack leetcode leetcode150"
categories: "stack"
---

## PROBLEM DESCRIPTION

Given a string path, which is an absolute path (starting with a slash '/') to a file or directory in a Unix-style file system, convert it to the simplified canonical path.

In a Unix-style file system, a period '.' refers to the current directory, a double period '..' refers to the directory up a level, and any multiple consecutive slashes (i.e. '//') are treated as a single slash '/'. For this problem, any other format of periods such as '...' are treated as file/directory names.

The canonical path should have the following format:

- The path starts with a single slash '/'.
- Any two directories are separated by a single slash '/'.
- The path does not end with a trailing '/'.
- The path only contains the directories on the path from the root directory to the target file or directory (i.e., no period '.' or double period '..')

Return the simplified canonical path.

[leetcode](https://leetcode.com/problems/simplify-path)

## SOLUTION

```java
class Solution {
    
    public String simplifyPath(String path) {

        // Stack to keep track of directories.
        Stack<String> stack = new Stack<>(); 

        // Split the path by "/" to extract directories.
        String[] s = path.split("/"); 

        //iterate through each word
        for (int i = 0; i < s.length; i++) {

            String current = s[i];

            // Handle ".." to move up a directory level.
            if (current.equals("..")) {

                //if the stack is empty, we are at the root directory, no need to pop anything
                if (!stack.isEmpty()) {
                    stack.pop(); // If stack is not empty, pop the top directory to move up.
                }
            // "." just means current directory, we can skip
            } else if (current.equals(".") || current.isEmpty()) {
                continue; // Ignore "." and empty strings (consecutive slashes).
            } else {
                stack.push(current); // Push valid directory names onto the stack.
            }

        }

        // Build the simplified canonical path using the directories in the stack.
        StringBuilder sb = new StringBuilder();
        for (String x : stack) {
            sb.append("/" + x); // Append the directory with a leading "/".
        }

        // Return the canonical path or root ("/") if empty.
        return sb.length() > 0 ? sb.toString() : "/"; 

    }

}
```
