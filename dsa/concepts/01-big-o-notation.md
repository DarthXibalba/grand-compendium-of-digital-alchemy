# Big O Notation — Summary (Cracking the Coding Interview)

## Purpose of Big O
Big O notation describes the **time complexity** or **space complexity** of an algorithm as the **input size grows**. It expresses how runtime or memory usage scales relative to input size `n`.

Big O focuses on the **upper bound of growth**, ignoring constants and lower-order terms.

Example:  
`3n² + 2n + 5 → O(n²)`

---

# Common Complexity Classes

| Complexity | Name | Example |
|---|---|---|
| O(1) | Constant | Accessing array index |
| O(log n) | Logarithmic | Binary search |
| O(n) | Linear | Traversing an array |
| O(n log n) | Linearithmic | Merge sort |
| O(n²) | Quadratic | Nested loops |
| O(2ⁿ) | Exponential | Recursive Fibonacci |
| O(n!) | Factorial | Generating permutations |

---

# Core Big O Rules

## 1. Drop Constants
Constant multipliers do not matter.

Examples:

`O(2n) → O(n)`  
`O(100n) → O(n)`

Big O measures **growth rate**, not exact runtime.

---

## 2. Drop Non-Dominant Terms
Only keep the fastest growing term.

Examples:

`O(n² + n) → O(n²)`  
`O(n + log n) → O(n)`

---

## 3. Different Inputs Use Different Variables
If an algorithm processes two independent inputs, use two variables.

Example:

```
for i in arrayA:
    for j in arrayB:
```

Runtime:

`O(a * b)`

Not `O(n²)` because the inputs may have different sizes.

---

# Common Runtime Patterns

## O(1) — Constant Time
Runtime does not depend on input size.

Example:

```
return arr[0]
```

Typical operations:

- Array index access
- Hash table lookup (average case)
- Basic arithmetic

---

## O(log n) — Logarithmic Time
The input size is **repeatedly divided by a constant factor**.

Example: Binary Search

```
while low <= high:
    mid = (low + high) // 2
```

Each step halves the problem size.

Examples:

- Binary search
- Balanced tree operations
- Heap operations

---

## O(n) — Linear Time
Runtime grows proportionally with input size.

Example:

```
for x in arr:
    print(x)
```

Examples:

- Linear search
- Traversing a list
- Counting elements

---

## O(n log n)
Common in efficient divide-and-conquer algorithms.

Examples:

- Merge Sort
- Heap Sort
- Quick Sort (average)

Reason:

`log n` recursion levels * `n` work per level

---

## O(n²) — Quadratic Time
Often caused by **nested loops over the same dataset**.

Example:

```
for i in arr:
    for j in arr:
        print(i, j)
```

Examples:

- Bubble sort
- Selection sort
- Comparing all pairs

---

## O(2ⁿ) — Exponential Time
Occurs when recursive calls **branch multiple times per level**.

Example: naive Fibonacci.

```
fib(n) = fib(n-1) + fib(n-2)
```

Growth doubles each time `n` increases.

---

## O(n!) — Factorial Time
Occurs when generating **all permutations**.

Example:

- Traveling salesman brute force
- Permutation generation

Extremely slow for even moderate `n`.

---

# Best, Worst, and Average Case

Some algorithms have different complexities depending on input.

Example: QuickSort

| Case | Complexity |
|---|---|
| Best | O(n log n) |
| Average | O(n log n) |
| Worst | O(n²) |

Worst case occurs when partitions are highly unbalanced.

---

# Space Complexity

Space complexity measures **how much memory an algorithm uses as the input size grows.**

Two kinds of memory are typically considered:

1. Input space – memory required to store the input itself
2. Auxiliary space – extra memory the algorithm allocates while running

When discussing algorithm efficiency, we usually focus on auxiliary space.

Example:

Input array size: `n`  
Auxiliary array size: `n`

Space complexity: `O(n)`

## Recursive Algorithms

Recursive algorithms also use **stack space**.

Example:

Recursive depth: `n`  
Space complexity: `O(n)`

---

# Sequential vs Nested Operations

## Sequential Steps → Add

```
for i in arr:
    ...

for j in arr:
    ...
```

Runtime:

`O(n + n) = O(n)`

---

## Nested Steps → Multiply

```
for i in arr:
    for j in arr:
        ...
```

Runtime:

`O(n * n) = O(n²)`

---

# Amortized Time

Some operations occasionally cost more but are cheap **on average**.

Example: dynamic array resizing.

Most insertions:

`O(1)`

Occasional resize:

`O(n)`

Average per insertion:

`O(1) amortized`

---

# Recursive Runtime Analysis

Recursive algorithms are analyzed using **recurrence relations**, which describe the runtime of a function in terms of the runtimes of its recursive calls.

Instead of describing the time directly with a formula like `O(n)` or `O(n²)`, we express the runtime in terms of **smaller instances of the same problem**.

---

## What Does T(n) Mean?

`T(n)` represents:

```
The time required to solve a problem of size n
```

For recursive algorithms, the runtime of `T(n)` depends on the runtime of smaller calls like `T(n/2)`, `T(n-1)`, etc.

---

## Branching Factor

The **branching factor** is the number of recursive calls made by a single function call.

Examples:

| Algorithm | Branching Factor | Explanation |
|---|---|---|
| Binary Search | 1 | Each call produces one smaller call |
| Merge Sort | 2 | Each call splits into two recursive calls |
| Fibonacci | 2 | Each call branches into two new calls |

Example: Fibonacci recursion

```
fib(n)
 ├─ fib(n-1)
 └─ fib(n-2)
```

Here the branching factor is **2** because each call creates two new recursive calls.

Large branching factors cause the number of calls to grow very quickly.

---

## Recurrence Relations

A **recurrence relation** describes the runtime of a recursive algorithm.

Example:

```
T(n) = 2T(n/2) + O(n)
```

This equation means:

1. The algorithm makes **2 recursive calls**
2. Each call works on **half the input size**
3. After the recursive calls finish, the algorithm performs **O(n) additional work**

## Example: Merge Sort

Merge Sort follows this pattern:

1. Divide the array into two halves
2. Recursively sort each half
3. Merge the sorted halves together

Example:

```
[8, 3, 5, 2]
```

Split:

```
[8, 3]   [5, 2]
```

Sort recursively:

```
[3, 8]   [2, 5]
```

Merge:

```
[2, 3, 5, 8]
```

## Runtime Breakdown

Divide step:

```
constant time
```

Recursive sorting:

```
2 recursive calls
each of size n/2
```

Merge step:

```
must scan the entire array
O(n)
```

This leads to the recurrence relation:

```
T(n) = 2T(n/2) + O(n)
```

## Visualizing the Recursion Tree

To understand the runtime, we visualize the recursive calls as levels in a tree.

Level 0:

```
1 problem of size n
work = n
```

Level 1:

```
2 problems of size n/2
total work = n
```

Level 2:

```
4 problems of size n/4
total work = n
```

Each level performs **O(n)** work overall.

## Number of Levels

At each level the problem size is cut in half:

```
n → n/2 → n/4 → n/8 → ... → 1
```

The number of times we can divide `n` by 2 until reaching 1 is:

```
log₂ n
```

So the recursion tree has **log n levels**.

## Final Runtime

Each level performs:

```
O(n) work
```

Number of levels:

```
log n
```

Total runtime:

```
O(n) × log n
```

Final complexity:

```
O(n log n)
```

## Key Takeaways

Recurrence relations describe the runtime of recursive algorithms.

Example recurrence:

```
T(n) = 2T(n/2) + O(n)
```

Meaning:

- Two recursive calls
- Each half the size of the original problem
- Additional linear work after the calls

This pattern commonly leads to:

```
O(n log n)
```

Algorithms that follow this pattern include:

- Merge Sort
- Many divide-and-conquer algorithms
- Some tree algorithms

---

# Logarithm Intuition

A logarithm measures **how many times you divide until reaching 1**.

Example:

`log₂(16) = 4`

Sequence:

```
16 → 8 → 4 → 2 → 1
```

Binary search repeatedly halves the problem, producing `O(log n)` complexity.

---

# Typical Interview Complexity Targets

| Complexity | Acceptable? |
|---|---|
| O(log n) | Excellent |
| O(n) | Good |
| O(n log n) | Good |
| O(n²) | Sometimes acceptable |
| O(2ⁿ) | Usually too slow |
| O(n!) | Almost always infeasible |

---

# Interview Strategy for Big O

When analyzing an algorithm:

1. Identify loops.
2. Determine whether loops are sequential or nested.
3. Analyze recursion depth and branching factor.
4. Remove constants and lower-order terms.
5. Express the final runtime in terms of input size variables.

---

# Big O Mental Cheat Sheet

| Complexity | Typical Example |
|---|---|
| O(1) | Hash lookup |
| O(log n) | Binary search |
| O(n) | Single loop |
| O(n log n) | Merge sort |
| O(n²) | Nested loops |
| O(2ⁿ) | Recursive branching |
| O(n!) | Permutations |

---

# Key Takeaway

Big O describes **how algorithm performance scales with input size**, not the exact runtime.

Understanding growth rates allows developers to:

- Compare algorithms
- Predict scalability
- Design efficient solutions for large inputs.
