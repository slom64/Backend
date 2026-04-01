# Convert from Memorization "Top-Down" to Tabulation "Bottom-up"

## Step 1: Identify the DP state (most important)
Look at the memoized function parameters.

Example:
```java
f(i, w)
```

This immediately means:
```java
dp[i][w]
```

> [!Tip]
> Each function parameter becomes one DP dimension.

---
## Step 2: Decide DP table size

Look at the range of each parameter.

Example:
```java
i → 0 to n
w → 0 to W
```

DP table:
```java
dp[n+1][W+1]
```

This step already gives you:
- **Time complexity:** `O(n × W)`
- **Space complexity:** `O(n × W)`

---
## Step 3: Write the recurrence (copy-paste logic)

Take the memo recurrence and remove recursion syntax.

Memo:
```java
f(i, w) = max(  
    f(i+1, w),  
    val[i] + f(i+1, w-wt[i])  
)
```

Tabulation:
```java
dp[i][w] = max(  
    dp[i+1][w],  
    val[i] + dp[i+1][w-wt[i]]  
)
```

> [!Attention]
> Logic never changes. Only recursion becomes table lookup.

---
## Step 4: Convert base cases into table initialization

Memo base case:

```java
if (i == n || w == 0) return 0;
```

Tabulation:
```java
dp[n][w] = 0   // for all w  
dp[i][0] = 0   // for all i
```

> [!important]
> _Every_ `return` _base case becomes a pre-filled DP cell._

---

## Step 5: Decide loop order (this is the key)

### Golden Rule:

> **_Fill the DP table in the reverse direction of recursion dependency._**

Examples:
Memo DependencyLoop Direction`nf(i,j) → f(i-1,j-1)`forward loops`f(i,j) → f(i+1,j+1)`reverse loops

**Think like this:**

> Children must be filled before parents.

---
## Step 6: Final answer location

Memo call:
```
f(start_state)
```

Tabulation answer:
```
dp[start_state]
```

Example:
```
return dp[0][W];
```


---
## Why This Works for Every DP Problem
Because **every DP problem is just states + dependencies**.