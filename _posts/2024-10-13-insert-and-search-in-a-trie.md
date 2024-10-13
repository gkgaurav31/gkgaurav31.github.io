---
layout: post
title: Insert and Search in a Trie (geeksforgeeks - SDE Sheet)
date: 2024-10-13 16:07 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Complete the Insert and Search functions for a Trie Data Structure.

- Insert: Accepts the Trie's root and a string, modifies the root in-place, and returns nothing.
- Search: Takes the Trie's root and a string, returns true if the string is in the Trie, otherwise false.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/trie-insert-and-search0651/1?page=9)

## SOLUTION

Insert: We go through each character of the word and check if there's already a node for that letter. If not, we create a new node for it. Once we've processed all the characters, we mark the last node as the end of the word, signaling that the word is now stored in the Trie.

In the search method, we check if a word exists in the Trie. We follow the path of nodes that represent each letter in the word. If we find that a character doesn't have a corresponding node, we return false, meaning the word isn't in the Trie. If we reach the end of the word and the last node is marked as the end, we return true, confirming that the word is present.

```java
/*
static final int ALPHABET_SIZE = 26;

// Trie node structure
static class TrieNode {
    TrieNode[] children = new TrieNode[ALPHABET_SIZE]; // Array to hold references to child nodes (26 for each alphabet letter)

    // isEndOfWord is true if the node represents the end of a word
    boolean isEndOfWord;

    TrieNode() {
        isEndOfWord = false; // By default, a node is not the end of a word
        for (int i = 0; i < ALPHABET_SIZE; i++)
            children[i] = null; // Initialize all children to null
    }
};
*/

// Function to insert a string into the Trie
static void insert(TrieNode root, String key)
{
    // Traverse through each character in the input string
    for (int i = 0; i < key.length(); i++) {

        // Get the current character
        char ch = key.charAt(i);

        // If the child node for the current character doesn't exist, create it
        if (root.children[ch - 'a'] == null) {
            root.children[ch - 'a'] = new TrieNode();
        }

        // Move to the next node in the trie
        root = root.children[ch - 'a'];
    }

    // Mark the last node as the end of the word
    root.isEndOfWord = true;
}

// Function to search for a string in the Trie
static boolean search(TrieNode root, String key)
{
    // Traverse through each character in the input string
    for (int i = 0; i < key.length(); i++) {

        // Get the current character
        char ch = key.charAt(i);

        // If the child node for the current character doesn't exist, the word is not in the trie
        if (root.children[ch - 'a'] == null)
            return false;
        else
            // Move to the next node in the trie
            root = root.children[ch - 'a'];

    }

    // Return true if the node represents the end of a word, otherwise false
    return root.isEndOfWord;
}
```
