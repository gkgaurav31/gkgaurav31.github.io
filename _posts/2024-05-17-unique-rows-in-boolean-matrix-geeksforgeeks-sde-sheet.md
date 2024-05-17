---
layout: post
title: Unique rows in boolean matrix (geeksforgeeks - SDE Sheet)
date: 2024-05-17 17:12 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks hashmap sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

Given a binary matrix your task is to find all unique rows of the given matrix in the order of their appearance in the matrix.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/unique-rows-in-boolean-matrix/1?page=3)

## SOLUTION

For each row, we convert it into a string and check if it's already in our set. If it's not, we add it to our list of unique rows.

```java
class GfG
{
    public static ArrayList<ArrayList<Integer>> uniqueRow(int a[][],int r, int c)
    {

        ArrayList<ArrayList<Integer>> list = new ArrayList<>();
        Set<String> set = new HashSet<>();

        for(int i=0; i<r; i++){

            ArrayList<Integer> temp = new ArrayList<>();
            StringBuffer sb = new StringBuffer();

            for(int j=0; j<c; j++){
                sb.append(String.valueOf(a[i][j]));
                temp.add(a[i][j]);
            }

            if(!set.contains(sb.toString())){
                list.add(temp);
            }

            set.add(sb.toString());

        }

        return list;

    }
}
```
