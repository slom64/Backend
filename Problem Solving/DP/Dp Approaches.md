### A useful mental checklist

When you see a new problem, ask:
- **Two choices at each step?** → Take/Leave DP.
- **Need the answer for the first `i` elements?** → Prefix DP.
- **Need to split an interval?** → Interval DP.
- **Need to move in a grid?** → Grid DP.
- **Need to preserve order between sequences?** → Subsequence DP.
- **Need to visit subsets?** → Bitmask DP.
- **Working on digits of a number?** → Digit DP.
- **Working on a tree?** → Tree DP.

---
## 1. Take / Leave (Decision DP)
Every element presents two choices.
```
dp(index, ...)
```

## 1. The "Prefix / Ending At" Approach

Instead of deciding for each item individually, you define your subproblem based on a specific prefix of the input.

- The Core Idea: "What is the optimal answer if the problem ended exactly at index `i`?"
- How it works: You find the answer for index `i` by looking back at previous indices `j` (where `j < i`) and building upon their optimal solutions.
- Classic Example: _Longest Increasing Subsequence (LIS)_. To find the longest subsequence ending at index `i`, you check all previous elements `j` that are smaller than element `i`.

```
dp[i] =
```

## 2. The "Partition / Gap" Approach (Interval DP)

This is used when a problem can be broken down into independent sub-segments or intervals.

- The Core Idea: "What is the optimal solution for the substring or subarray from index `i` to index `j`?"
- How it works: You solve smaller gaps (lengths of 1, 2, 3...) first. To solve a larger interval `[i, j]`, you try splitting it at every possible middle point `k` (where `i <= k < j`) and combine the results of `[i, k]` and `[k+1, j]`.
- Classic Example: _Matrix Chain Multiplication_ or _Longest Palindromic Substring_.
```
dp(l, r)
```
## 3. The "State Machine / Multi-Choice" Approach

Use this when you have choices, but your current options are strictly limited by what you did in the previous step.

- The Core Idea: "What state am I in right now, and what states can I transition to next?"
- How it works: You create multiple DP arrays or an extra dimension representing your "mode." For example, `DP[i][0]` means you don't own a stock on day `i`, and `DP[i][1]` means you do. Your choices on day `i` depend entirely on which state you were in on day `i-1`.
- Classic Example: _Best Time to Buy and Sell Stock (with cooldown or transaction fees)_ or _House Robber (where you can't rob adjacent houses)_.

## 4. The "Digit DP" Approach

This is a highly specialized framework used for counting numbers within a specific range `[L, R]` that satisfy a certain property.

- The Core Idea: "Construct the number digit by digit, from left to right."
- How it works: Your state tracks which digit position you are processing, whether you are restricted by the upper bound of the number (the `tight` constraint), and the specific property you are tracking (like sum of digits).
- Classic Example: _Count the number of integers between 1 and 10⁹ that have an even sum of digits._

## 5. The "Bitmask DP" Approach

When you need to keep track of a small set of visited items or used resources, and the order matters.

- The Core Idea: "Use an integer's binary representation (bits) as a set to represent the current state."
- How it works: If you have 5 items, a binary number like `10110` (which is 22 in decimal) means you have taken items 1, 2, and 4, but left items 0 and 3. You iterate through all possible subset combinations.
- Classic Example: _Traveling Salesperson Problem (TSP)_ or matching N people to N tasks optimally when N ≤ 20.

## 6. The "Tree DP" Approach

Used when the problem structure is a tree (nodes and edges without cycles) rather than a linear array.

- The Core Idea: "The answer for a parent node depends on the answers calculated by its children."
- How it works: You run a Depth First Search (DFS). You compute the DP values for the leaf nodes first, and as the recursion returns up the tree, the parent node aggregates the results of its subtrees.
- Classic Example: _Binary Tree Maximum Path Sum_ or _Unique Binary Search Trees_.

---

To help you practice, do you have a specific LeetCode problem or coding puzzle you are stuck on right now? If you share it, we can figure out which of these frameworks fits it perfectly.