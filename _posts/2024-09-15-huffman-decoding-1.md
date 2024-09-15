---
layout: post
title: Huffman Decoding-1
date: 2024-09-15 21:17 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a string S, implement Huffman Encoding and Decoding.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/huffman-decoding-1/1?page=7)

## SOLUTION

1. Start with an empty string `ans` to store the decoded characters.
2. Initialize another node `curr`, which is set to the root of the Huffman tree.
3. Loop through each bit in the string `s`.
   - If the bit is '0', move `curr` to its left child.
   - If the bit is '1', move `curr` to its right child.
   - If `curr` is at a leaf node (it has no children), this means you have reached a decoded character.
     - Add the character to `ans`.
     - Set `curr` back to the root of the Huffman tree.

```java
class GfG {

    String decode_file(minHeapNode root, String encodedStr){

        int n = encodedStr.length();

        StringBuilder sb = new StringBuilder();

        minHeapNode ptr = root;

        for(int i=0; i<n; i++){

            char ch = encodedStr.charAt(i);

            if(ch == '0'){
                ptr = ptr.left;
            }else
                ptr = ptr.right;

            if(ptr.left == null && ptr.right == null){
                sb.append(ptr.data);
                ptr = root;
            }

        }

        return sb.toString();

    }

}
```
