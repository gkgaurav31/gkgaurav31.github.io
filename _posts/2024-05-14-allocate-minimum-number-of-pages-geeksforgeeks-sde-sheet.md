---
layout: post
title: Allocate minimum number of pages (geeksforgeeks - SDE Sheet)
date: 2024-05-14 22:58 +0530
author: "Gaurav Kumar"
tags: "java geeksforgeeks binary_search sde-sheet"
categories: "sde-sheet"
---

## PROBLEM DESCRIPTION

You have `N` books, each with `A[i]` number of pages. `M` students need to be allocated contiguous books, with each student getting at least one book.

Out of all the permutations, the goal is to find the permutation where the sum of maximum number of pages in a book allotted to a student should be minimum, out of all possible permutations.

**Note:** Return `-1` if a valid assignment is not possible, and allotment should be in contiguous order.

[geeksforgeeks](https://www.geeksforgeeks.org/problems/allocate-minimum-number-of-pages0937/1?page=2)

## SOLUTION

```java
class Solution
{
    //Function to find minimum number of pages.
    public static int findPages(int[]A,int N,int M)
    {

        // each student should get at least 1 book
        // if there are more students than books, return false
        if(M > N)
            return -1;

        // among all books, whichever has the min number of pages, that's the least possible value
        int minPages = A[0];
        // max is the sum of pages for all books
        int maxPages = A[0];
        for(int i=1; i<N; i++){
            minPages = Math.min(minPages, A[i]);
            maxPages += A[i];
        }

        // init: l and r for binary search
        int l = minPages;
        int r = maxPages;

        // init: res to a value beyond what is possible
        int res = maxPages+1;

        while(l<=r){

            // mid for binary search
            int mid = (l+r)/2;

            // check if it is possible with max "mid" number of pages for each student to read all books
            if(possible(A, N, mid, M)){

                // if it's possible, update result
                res = mid; // Math.min() is not necessary

                // look for smaller values
                // any greater value would also be possible, but we need lower answer
                r = mid-1;

            // since it's not possible, we should look for higher number of pages
            // if we take lower number of pages per student, it will definitely not be possible as well
            }else{

                l = mid+1;
            }

        }

        // if there was no value which was possible, res will have the value maxPages+1
        // return -1 since it's not possible
        if(res == maxPages + 1)
            return -1;

        return res;

    }

    // if x number of pages are allowed at max per student, how many students are needed to read all books?
    // if it is <= total number of students given, return true (it will obviously be possible if we have more students)
    public static boolean possible(int[] arr, int n, int totalPagesAllowedPerStudent, int M){

        // init: current number of pages for current student
        int currentPages = 0;

        // how many students are needed
        int studentsNeeded = 1;

        // for each book
        for(int i=0; i<n; i++){

            // if the number of pages of current book is beyond what is allowed max, we can return false
            // it will never be possible to read this book by a single student
            if(arr[i] > totalPagesAllowedPerStudent)
                return false;

            // is it possible to read the current book by the current student?
            if(currentPages + arr[i] <= totalPagesAllowedPerStudent){

                // add to the total number of pages read by current student
                currentPages += arr[i];

            // since it's not possible to read the current book by the current student, we need 1 more student
            }else{

                // update the currentPages for the next student who will start reading this book
                currentPages = arr[i];

                // increase students needed in total
                studentsNeeded++;

            }

        }

        // if total students needed is less than maximum students allowed, return true
        return studentsNeeded <= M;

    }

}
```
