---
layout: post
title: Common Elements (geeksforgeeks - SDE Sheet)
date: 2024-10-13 21:32 +0530
author: "Gaurav Kumar"
tags: "java two_pointers geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given two lists V1 and V2 of sizes n and m respectively. Return the list of elements common to both the lists and return the list in sorted order. Duplicates may be there in the output list.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/common-elements5420/1?page=10)

## SOLUTION

To solve this, we sort both arrays and use two pointers to compare elements. If they're equal, we add the element to the result and move both pointers. If not, we move the pointer with the smaller element.

```java
class Solution {

    public static ArrayList<Integer> common_element(int v1[], int v2[]) {

        int n = v1.length;
        int m = v2.length;

        Arrays.sort(v1);
        Arrays.sort(v2);

        ArrayList<Integer> list = new ArrayList<>();

        int i = 0;
        int j = 0;

        // Traverse both arrays using the two-pointer technique
        while (i < n && j < m) {

            // If both elements are equal, add to the list and move both pointers
            if (v1[i] == v2[j]) {
                list.add(v1[i]);
                i++;
                j++;

            // If the element in v1 is smaller, increment pointer i to move forward
            } else if (v1[i] < v2[j]) {
                i++;

            // If the element in v2 is smaller, increment pointer j to move forward
            } else {
                j++;
            }

        }

        return list;
    }
}
```
