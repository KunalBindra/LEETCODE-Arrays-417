# LEETCODE-Arrays-417
Let’s **dry run** it step-by-step to understand exactly how it works.
** Company ** - Google , Microsoft , Meta
---

### 🧩 **Problem Summary**

Given a matrix `heights`, water can flow from one cell to another **if the next cell’s height ≤ current cell’s height** (downhill or flat).
We must find all coordinates from which water can flow to **both the Pacific and Atlantic oceans**.

* **Pacific Ocean** touches the **top and left** edges.
* **Atlantic Ocean** touches the **bottom and right** edges.

---

### 🧠 **Code Intuition**

Instead of simulating water flow from every cell,
this code **reverses the logic**:

> Start DFS *from the oceans inward* (i.e., from borders).
> Mark all cells that can reach each ocean.

Finally, find cells that are reachable from **both oceans**.

---

### 📊 Example Matrix

Let’s dry run this small 3×3 matrix:

```
heights = [
  [1, 2, 2],
  [3, 2, 3],
  [2, 4, 5]
]
```

---

### ⚙️ Step-by-Step Execution

#### 1️⃣ Initialization

```java
m = 3, n = 3
ans = []
seenA = new boolean[3][3]
seenP = new boolean[3][3]
```

All `seenA` and `seenP` cells are initially `false`.

---

#### 2️⃣ DFS from Pacific edges

Pacific borders are:

* **Left edge:** `(i, 0)`
* **Top edge:** `(0, j)`

**Loop 1:**

```java
for (i = 0; i < 3; i++) {
    dfs(heights, i, 0, 0, seenP); // left edge
    dfs(heights, i, 2, 0, seenA); // right edge
}
```

**Loop 2:**

```java
for (j = 0; j < 3; j++) {
    dfs(heights, 0, j, 0, seenP); // top edge
    dfs(heights, 2, j, 0, seenA); // bottom edge
}
```

---

### 🔍 DFS Explanation

`dfs(heights, i, j, h, seen)`:

* Stops if:

  * Out of bounds, or
  * `seen[i][j] == true`, or
  * `heights[i][j] < h` (height must not be less than previous height)
* Otherwise:

  * Mark `seen[i][j] = true`
  * DFS to 4 directions

This way, we move *uphill or flat* (reverse flow).

---

### 🌊 Pacific DFS Start Points

1. From **left edge**: `(0,0)`, `(1,0)`, `(2,0)`
2. From **top edge**: `(0,0)`, `(0,1)`, `(0,2)`

After these DFS calls, all cells reachable from the Pacific (seenP = true) are:

```
P-reachable cells:
(0,0), (0,1), (0,2),
(1,0), (1,1), (1,2),
(2,0), (2,1), (2,2)
```

(Practically all — because the terrain slopes upward gradually.)

---

### 🌊 Atlantic DFS Start Points

1. From **right edge**: `(0,2)`, `(1,2)`, `(2,2)`
2. From **bottom edge**: `(2,0)`, `(2,1)`, `(2,2)`

After these DFS calls, all cells reachable from the Atlantic (seenA = true) are:

```
A-reachable cells:
(0,2), (1,2), (2,2),
(2,1), (2,0), (1,1)
```

---

### ✅ Final Comparison

Now we check cells where **seenP[i][j] && seenA[i][j]**:

| Cell  | Pacific | Atlantic | Both? |
| ----- | ------- | -------- | ----- |
| (0,0) | ✅       | ❌        | ❌     |
| (0,1) | ✅       | ❌        | ❌     |
| (0,2) | ✅       | ✅        | ✅     |
| (1,0) | ✅       | ❌        | ❌     |
| (1,1) | ✅       | ✅        | ✅     |
| (1,2) | ✅       | ✅        | ✅     |
| (2,0) | ✅       | ✅        | ✅     |
| (2,1) | ✅       | ✅        | ✅     |
| (2,2) | ✅       | ✅        | ✅     |

---

### 💎 Final Answer

```
ans = [
  [0,2],
  [1,1],
  [1,2],
  [2,0],
  [2,1],
  [2,2]
]
```

---

### 🧭 Summary of Logic Flow

1. Start DFS from Pacific borders → mark reachable cells (`seenP`)
2. Start DFS from Atlantic borders → mark reachable cells (`seenA`)
3. Collect intersection → cells from which water can flow to both oceans.

---
