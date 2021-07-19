This guide is a collection of my notes on fundamental algorithms and data structures in computer science.

## Dynamic Programming

Dynamic programming is a computer programming technique.

Fundamentally, it refers to simplifying a complicated problem by breaking it down into simpler sub-problems in a **recursive** manner.

There are two key attributes that a problem must have in order for dynamic programming to be applicable: 
1. **Optimal substructure**
2. **Overlapping sub-problems**

**Optimal substructure** means that the solution to an *optimization problem* can be obtained optimally by breaking it into sub-problems and then recursively finding the optimal solutions to those sub-problems. Such optimal substructures are usually described by means of **recursion**.

**Overlapping sub-problems** means that the recursive algorithm solving the problem should solve the same sub-problems over and over, rather than generating new sub-problems.

Many dynamic programming problems can be solved using the following sequence:

1. Find a recursive relation
2. Recursive (top-down)
3. Recursive + memo (top-down)
4. Iterative + memo (bottom-up)
5. Iterative + `N` variables (bottom-up)

**Step 1:** figure out a recursive relation to 

https://leetcode.com/problems/house-robber/discuss/156523/From-good-to-great.-How-to-approach-most-of-DP-problems.
