---
layout: post
title: Maximum Area of Triangle (InterviewBit)
date: 2024-01-07 23:48 +0530
Author: "Gaurav Kumar"
tags: "java arrays interviewbit"
categories: "interviewbit"
---

## PROBLEM DESCRIPTION

Given a character matrix of size N x M in the form of a string array A of size N where A[i] denotes ith row.

Each character in the matrix consists any one of the following three characters {'r', 'g', 'b'} where 'r' denotes red color similarly 'g' denotes green color and 'b' denotes blue color.

You have to find the area of the largest triangle that has one side parallel to y-axis i.e vertical and the color of all three vertices are different.

NOTE:

If the area comes out to be a real number than return the ceil of that number.

[interviewbit](https://www.interviewbit.com/problems/maximum-area-of-triangle/)

## SOLUTION

[Good Explanation](https://www.algostreak.com/post/maximum-area-of-triangle-interviewbit-solution)

The area is a triangle is: `1/2 * base * height`. To maximize it, we need the largest base and height both. The problem also mentions that one of the sides should be parallel to the Y axis. Assuming that side to be the base connection vertices A and B, we need to find the 3rd vertex C which is farthest away from this base.

To find the farthest vertex, we will pre-compute the last column for RGB. The last vertex could also be on the left side as well. So handle this condition, we will just reverse the matrix and use the same logic.

Think of it in this way:

- Let's say we need to first figure out the base length. It can be made using:

  - R and G
  - R and B
  - G and B

  For each of the above options, we will find the base with largest length for each column.

- Let's say we are at the first column, and we have fixed the base for R and G. So, now we need to maximize the height. Which means, we need to set the vertex B and that should be as far as possible. We can use our pre-computed value which will give us the farthes column for B.

- Now that we have both the base and height, we can calculate the area. Update if it's the maximum.

> Let's say we have fixed some base parallel to the Y axis. Area of a triangle is `half * base * height`. Let's say, the 3rd index is at column Z. It does not matter which row it belongs to because the height will remain the say.

```java
public class Solution {

    public int solve(String[] A) {

        int a = helper(A); // Area with normal orientation

        String[] res = reverseMatrix(A); // Reversed matrix

        int b = helper(res); // Area with reversed orientation

        // Return the maximum area obtained in both orientations
        return Math.max(a, b);
    }

    // Helper method to calculate the largest triangle's area
    public int helper(String[] A){

        int n = A.length; // Number of rows
        int m = A[0].length(); // Number of columns

        int maxArea = 0; // Variable to track the maximum area
        int currentBase = 0; // Variable to store the current base of a triangle

        // Find the last column position of 'r', 'g', 'b' in the matrix
        int[] lastColumnOfRGB = lastColumnOfRGB(A); // {R, G, B}

        // If any of 'r', 'g', 'b' is not present in the matrix, return 0
        if(lastColumnOfRGB == null) return 0;

        // Iterate through each column
        for(int c=0; c<m; c++){

            // Find the maximum base for vertices 'r' and 'g' as base parallel to Y axix
            currentBase = findMaxBaseForVertices('r', 'g', c, A);
            maxArea = Math.max(maxArea, areaTriangle(currentBase, lastColumnOfRGB[2]-c));

            // Find the maximum base for vertices 'r' and 'b' as base parallel to Y axix
            currentBase = findMaxBaseForVertices('r', 'b', c, A);
            maxArea = Math.max(maxArea, areaTriangle(currentBase, lastColumnOfRGB[1]-c));

            // Find the maximum base for vertices 'g' and 'b' as base parallel to Y axix
            currentBase = findMaxBaseForVertices('g', 'b', c, A);
            maxArea = Math.max(maxArea, areaTriangle(currentBase, lastColumnOfRGB[0]-c));
        }

        return maxArea;
    }

    // Method to reverse the matrix
    public String[] reverseMatrix(String[] A){
        int n = A.length; // Number of rows
        String[] res = new String[n]; // Resultant array
        for(int i=0; i<n; i++){
            res[i] = new StringBuilder(A[i]).reverse().toString();
        }
        return res;
    }

    public int areaTriangle(int base, int height){
        return (int) Math.ceil(0.5*base*height);
    }

    // Helper method to find the maximum base for vertices
    // The base would be parallel to Y axis, so we will try to find the top and bottom of that base
    public int findMaxBaseForVerticesHelper(char a, char b, int col, String[] A){

        int n = A.length; // Number of rows
        int top = 0;
        int bottom = n-1;

        // Find the topmost row with character 'a' in the column
        while(top < n && A[top].charAt(col) != a){
            top++;
        }

        // Find the bottommost row with character 'b' in the column
        while(bottom >=0 && A[bottom].charAt(col) != b){
            bottom--;
        }

        // If either 'a' or 'b' is not present in the column, return 0
        if(top == n || bottom == -1) return 0;

        // Return the base of the triangle
        return bottom - top + 1;
    }

    // Method to find the maximum base for vertices 'a' and 'b'
    public int findMaxBaseForVertices(char a, char b, int col, String[] A){

        int maxBase = 0;

        // Find the maximum base for vertices 'a' and 'b'
        // The top can be a and bottom b or vice versa
        // So we will check for both posssibilities
        return Math.max(
            maxBase,
            Math.max(
                findMaxBaseForVerticesHelper(a, b, col, A), //a at top
                findMaxBaseForVerticesHelper(b, a, col, A) //b at top
                )
            );
    }

    // Method to find the last column position of 'r', 'g', 'b' in the matrix
    // This is to find the maxiumum height using the 3rd vertex
    public int[] lastColumnOfRGB(String[] A){

        int[] lastColumnOfRGB = {-1, -1, -1}; // {R, G, B}

        int n = A.length; // Number of rows
        int m = A[0].length(); // Number of columns

        // Iterate through each column from right to left
        for(int c=m-1; c>=0; c--){

            // Iterate through each row
            for(int r=0; r<n; r++){

                // Update the last positions of 'r', 'g', 'b' in the matrix, it it's the farthest column
                switch(A[r].charAt(c)){
                    case 'r':   if(lastColumnOfRGB[0] == -1) lastColumnOfRGB[0] = c+1; break;
                    case 'g':   if(lastColumnOfRGB[1] == -1) lastColumnOfRGB[1] = c+1; break;
                    case 'b':   if(lastColumnOfRGB[2] == -1) lastColumnOfRGB[2] = c+1; break;
                }
            }

        }

        // If any of 'r', 'g', 'b' is not present, return null
        if(lastColumnOfRGB[0] == -1 || lastColumnOfRGB[1] == -1 || lastColumnOfRGB[2] == -1)
            return null;

        return lastColumnOfRGB;
    }
}
```
