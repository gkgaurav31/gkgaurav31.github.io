---
layout: post
title: First non-repeating character in a stream (geeksforgeeks - SDE Sheet)
date: 2024-05-16 22:52 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks linkedlist hashmap sde-sheet important"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given an input stream `A` of `n` characters consisting only of lower case alphabets. While reading characters from the stream, you have to tell which character has appeared only once in the stream upto that point. If there are many characters that have appeared only once, you have to tell which one of them was the first one to appear. If there is no such character then append `#` to the answer.

**NOTE:**

1. You need to find the answer for every `i` `(0 <= i < n)`
2. In order to find the solution for every `i` you need to consider the string from starting position till ith position.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/first-non-repeating-character-in-a-stream1216/1?page=3)

## SOLUTION

![snapshot]({{ site.baseurl }}/assets/img/first_non_repeating_character_in_stream.png)

```java
class Solution
{
    public String FirstNonRepeating(String A)
    {
        // Doubly linked list to store characters in the order they appear
        LinkedList<Character> dll = new LinkedList<>();

        // Set to keep track of characters that have already appeared
        // We can also use an array of size 26 to check this
        Set<Character> set = new HashSet<>();

        // StringBuffer to store the result
        StringBuffer sb = new StringBuffer();

        // Length of the input stream
        int n = A.length();

        // Loop through each character in the input stream
        for(int i=0; i<n; i++){

            // Get the current character
            char ch = A.charAt(i);

            // If the character is not already in the set (hasn't appeared before)
            if(!set.contains(ch)){

                // Add the character to the doubly linked list
                // The answer needs to be the first non-repeating character. If that duplicates, then the next one can become the answer
                dll.add(ch);

                // Add the character to the set to mark it as appeared
                set.add(ch);

            }
            else{

                // If the character has appeared before, remove it from the doubly linked list
                // This will take O(N) time to remove a certain node from the DLL
                // This can be optimized by using HashMap to get a direct reference to the node corresponding to a given character
                dll.remove(Character.valueOf(ch));

            }

            // If there are no non-repeating characters, append '#' to the result
            if(dll == null || dll.size() == 0)
                sb.append("#");
            // Otherwise, append the first non-repeating character to the result
            else
                sb.append(dll.getFirst());

        }

        return sb.toString();
    }
}
```
