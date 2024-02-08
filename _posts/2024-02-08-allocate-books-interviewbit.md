---
layout: post
title: Allocate Books (InterviewBit)
date: 2024-02-08 22:33 +0530
author: "Gaurav Kumar"
tags: "java binary_search"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given an array of integers `A` of size `N` and an integer `B`.

The College library has `N` books. The ith book has `A[i]` number of pages.

You have to allocate books to `B` number of students so that the maximum number of pages allocated to a student is minimum.

- A book will be allocated to exactly one student.
- Each student has to be allocated at least one book.
- Allotment should be in contiguous order, for example: A student cannot be allocated book 1 and book 3, skipping book 2.

Calculate and return that minimum possible number.

NOTE: Return `-1` if a valid assignment is not possible.

## SOLUTION

```java
public class Solution {

    public int books(int[] A, int B) {

        int n = A.length; // number of books

        // Edge case:
        // If there are more students than books, each student cannot get a book
        // So we can return -1
        if(B > n) return -1;

        // We are supposed to allocate each book. Let's say the highest number of pages is MAX_PAGES. If we try with any value lower than that, we won't be able to allocate that book. So the lowest possible answer will be MAX_PAGES
        int l = A[0];

        // If there is only 1 student, he can be allocated all the pages
        // In that case the highest value possible is the sum of all pages
        int r = 0;

        // update l as MAX_PAGES and r as sum of all pages
        for(int i=0; i<n; i++){
            l = Math.max(l, A[i]);
            r += A[i];
        }

        // init ans as max possible in the range and try to minimize it using binary search
        int minPages = r;

        // Binary search to find the minimum possible number of pages that can be allocated to a student
        while(l<=r){

            int m = (l+r)/2; // Calculate the middle point

            // If it's possible to allocate books such that no student gets more than m pages
            if(isPossible(A, B, m)){

                minPages = Math.min(minPages, m); // Update the minimum possible number of pages
                r = m-1; // look for lower pages if possible
            }else{
                l = m+1; // since it's not possible, we need to try with higher number of pages
            }

        }

        return minPages;

    }

    // Method to check if it's possible to allocate books such that no student gets more than allowedPages
    public boolean isPossible(int[] A, int totalStudents, int allowedPages){

        // Number of books
        int n = A.length;

        // Number of students needed to allocate the books
        int neededStudents = 0;

        // Pages read by the current student
        int readPages = 0;

        // Iterate through each book
        for(int i=0; i<n; i++){

            // If adding pages of the current book doesn't exceed allowedPages
            if(readPages + A[i] <= allowedPages){

                // Add pages of the current book to the pages read by the current student
                readPages += A[i];

            }else{

                // Increment the count of needed students
                neededStudents++;

                // Reset the pages read by the current student to the pages of the current book
                readPages = A[i];

            }

        }

        if(readPages > 0)
            neededStudents++; // If there are remaining pages, increment the count of needed students

        // Return whether the needed students are less than or equal to totalStudents
        return neededStudents <= totalStudents;

    }

}
```
