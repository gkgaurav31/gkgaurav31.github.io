---
layout: post
title: Allocate Minimum Pages
date: 2024-08-25 22:54 +0530
author: "Gaurav Kumar"
tags: "java binary_search geeksforgeeks sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You have n books, each with arr[i] a number of pages. m students need to be allocated contiguous books, with each student getting at least one book.
Out of all the permutations, the goal is to find the permutation where the sum of the maximum number of pages in a book allotted to a student should be the minimum, out of all possible permutations.

Note: Return -1 if a valid assignment is not possible, and allotment should be in contiguous order (see the explanation for better understanding).

[geeksforgeeks](https://www.geeksforgeeks.org/problems/allocate-minimum-number-of-pages0937/1?page=2)

## SOLUTION

```java
class Solution {

    // Function to find minimum number of pages.
    public static long findPages(int n, int[] arr, int m) {

        long l = 1; // read min 1 page
        long r = sum(arr, n); // read max all pages by single student

        // each student need to read at least 1 book
        // if there are less books than students, return -1
        if(n < m)
            return -1;

        // init: not possible
        long minBooks = -1;

        while(l<=r){

            // number of pages allowed at max per student
            long pages = l + (r - l) / 2;

            // check if those number of pages is a possible solution
            if(check(arr, n, m, pages)){
                // update new min value possible and check for lower value
                minBooks = pages;
                r = pages - 1;
            // otherwise, try with more number of pages
            }else{
                l = pages + 1;
            }

        }

        return minBooks ;

    }

    // check if we allow any student to read at max x number of pages, we would be able to cover all students
    public static boolean check(int[] arr, int n, int students, long pagesAllowed){

        // pages read by the current student
        long currentPages = 0;

        // number of student needed to read all
        int studentsNeeded = 1;

        // traverse all books
        for(int i=0; i<n; i++){

            // if current book has more pages than allowed, we can directly return false
            if(arr[i] > pagesAllowed)
                return false;

            // if the current student can read the current book
            // add to the number of pages read and move forward
            if(currentPages + arr[i] <= pagesAllowed){
                currentPages += arr[i];

            // otherwise, we need another student to cover this book
            }else{
                currentPages = arr[i];
                studentsNeeded++;
            }

        }

        //check if we have enough students to cover all the books
        return studentsNeeded <= students;
    }

    public static long sum(int[] arr, int n){
        long s = 0;
        for(int i=0; i<n; i++)
            s += arr[i];
        return s;
    }

};
```
