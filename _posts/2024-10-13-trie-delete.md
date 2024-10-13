---
layout: post
title: Trie | (Delete) (geeksforgeeks - SDE Sheet)
date: 2024-10-13 16:27 +0530
author: "Gaurav Kumar"
tags: "java graphs geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Trie is an efficient information retrieval data structure. This data structure is used to store Strings and search strings, String stored can also be deleted. Given a Trie root for a larger string super and a string key, delete all the occurences of key in the Trie.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/trie-delete/1?page=9)

## SOLUTION

To solve this, we traverse through the Trie using the characters from the given key, one by one. For each character, we check if there's a corresponding child node using the `subNode` method. Once we reach the last character of the key, we mark the `isEnd` property of the final node as `false`, indicating that the word is no longer considered a valid entry in the Trie.

```java
/*
class TrieNode
{
    char content;
    boolean isEnd;
    int count;
    LinkedList<TrieNode> childList;
    public TrieNode(char c)
    {
        childList = new LinkedList<TrieNode>();
        isEnd = false;
        content = c;
        count = 0;
    }
    public TrieNode subNode(char c)
    {
        if (childList != null)
            for (TrieNode eachChild : childList)
                if (eachChild.content == c)
                    return eachChild;
        return null;
    }
}*/

class Solution
{
    public static void deleteKey(TrieNode root, char[] key)
    {

        for(int i=0; i<key.length; i++){

            char ch = key[i];
            root = root.subNode(ch);

        }

        root.isEnd = false;

    }
}
```
