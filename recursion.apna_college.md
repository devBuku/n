
![[Pasted image 20261120191749.png]]

![[Pasted image 20261120191906.png]]

## 1. Backtracking

- When a recursive function reaches its base case, the function calls start returning one by one. This process is called **recursion unwinding**.
- **Backtracking** is an algorithmic technique where we explore different possible choices and undo a choice when we need to explore another path.

Think of backtracking as:

> Going forward through a path, exploring a choice, and coming back through the same path to try another choice.

**Example:** Solving a maze. If one path leads to a dead end, we go back and try another available path.

## 2. Recursion Tree

A recursion tree is a visual representation of recursive function calls.

- Each node represents a function call.
- Each branch represents a recursive call made by that function.
- It helps us understand how a problem breaks down into smaller subproblems.

## 3. Recurrence Relation

A recurrence relation is a mathematical equation that describes the running time of an algorithm in terms of the running time of smaller inputs.

For example, consider binary search:

$$
T(n) = T\left(\frac{n}{2}\right) + O(1)
$$


- T(n/2) represents the recursive call on half of the input.
- `O(1)` represents the constant work done in the current call.

Recurrence relations help us calculate the time complexity of recursive algorithms.

Here's the complete note in one Markdown block, ready to copy into Obsidian.


## 4. Calculating Time Complexity of Recursive Algorithms

There are two useful ways to reason about the time complexity of recursive algorithms:

1. **Recurrence Relation**
2. **Number of Recursive Calls × Work Per Call**

### 1. Recurrence Relation

A recurrence relation describes the time complexity of a recursive function in terms of the time complexity of its smaller recursive calls.

#### Example: Factorial Using Recursion

```ts
function factorial(n: number): number {
    if (n === 0) return 1;

    return n * factorial(n - 1);
}
```

#### **Step 1: Identify the Base Case**

```typescript
if (n === 0) return 1;
```

The base case performs constant work.

$$T(0) = K_2$$

#### **Step 2: Identify the Recursive Call**

```typescript
return n * factorial(n - 1);
```

Each call:

- Performs a constant amount of work (comparison and multiplication).
- Makes one recursive call with `n - 1`.

Therefore:

$$T(n) = T(n-1) + K$$

Where `K` represents the constant work performed in the current call, excluding the recursive call itself.

#### **Step 3: Expand the Recurrence Relation**

$$T(n) = T(n-1) + K $$
$$T(n) = T(n-2) + 2K  $$
$$T(n) = T(n-3) + 3K$$

Continuing until the base case:

$$T(n) = T(0) + nK$$

Since T(0) and K are constants:

$$T(n) = O(n)$$

**Time Complexity:** O(n)

### **2. Number of Recursive Calls × Work Per Call**

Another way to reason about time complexity is:

$$\text{Total Work} = \text{Number of Calls} \times \text{Work Per Call}$$

For the factorial example:

- Number of calls: `n + 1` (including the base case).
- Work per call: `O(1)`.

Therefore:

$$ T(n) = (n+1) \times O(1)$$

$$\boxed{T(n) = O(n)}$$
> **Important:** This shortcut works when the number of calls and the work per call can be determined clearly. For recursive algorithms with branching calls or different amounts of work at different levels, use a recurrence relation or recursion tree.

# Space Complexity of Recursive Algorithms

Space complexity measures how much memory an algorithm uses as the input size grows.

## Recursive Space Complexity

For recursive algorithms, memory is consumed by each function call stored in the call stack.


$$SC = \text{Recursion Depth} \times \text{Memory Per Call}$$


Or:


$$SC = \text{Call Stack Height} \times \text{Memory Used Per Call}$$


### Example: Factorial Using Recursion

```ts
function factorial(n: number): number {
    if (n === 0) return 1;

    return n * factorial(n - 1);
}
```

- Recursion depth: O(n)
- Memory used per call: O(1)
Therefore:
$$SC = {O(n)}\times{O(1)}$$
$$\boxed{SC=O(n)}$$

**Interview takeaway:** Recursive calls consume stack memory. Even if each call uses only `O(1)` memory, a recursion depth of `O(n)` results in `O(n)` auxiliary space.

Recursion Practice Questions: 
### Level 1: Basic Recursion
- [ ] Print numbers from 1 to N.
- [ ] Print numbers from N to 1.
- [ ] Print even numbers from 1 to N.
- [ ] Print odd numbers from 1 to N.
- [ ] Find the sum of numbers from 1 to N.
- [ ] Find the factorial of a number.
- [ ] Find the sum of digits of a number.
- [ ] Count the digits of a number.
### Level 2: Pattern Printing Using Recursion

9. Square Star Pattern

```
* * * *
* * * *
* * * *
* * * *
```

10. Increasing Star Triangle

```
*
* *
* * *
* * * *
```


11. Decreasing Star Triangle

```
* * * *
* * *
* *
*
```


12. Increasing Number Triangle

```
1
1 2
1 2 3
1 2 3 4
```


13. Decreasing Number Triangle

```
1 2 3 4
1 2 3
1 2
1
```



14. Right-Aligned Star Triangle

```
	  *
    * *
  * * *
* * * *

```
      
### Level 3: Mathematical Recursion
Reverse a number.
Check whether a number is a palindrome.
Calculate the power of a number (x^n).
Find the GCD of two numbers.
Convert decimal to binary.
Find the Nth Fibonacci number.
Check whether a number is prime using recursion.

### Level 4: Arrays and Strings
Print all array elements recursively.
Find the sum of array elements.
Find the maximum element in an array.
Find the minimum element in an array.
Check whether an array is sorted.
Reverse an array recursively.
Reverse a string recursively.
Check whether a string is a palindrome.
Count occurrences of an element in an array.
### Level 5: Intermediate Recursion
Binary search using recursion.
Generate all subsequences of a string.
Generate all subsets of an array.
Generate all permutations of a string.
Generate all possible combinations of elements.
Solve the Tower of Hanoi problem.
### Level 6: Backtracking and Advanced Recursion
Rat in a Maze.
N-Queens Problem.
Sudoku Solver.
Combination Sum.
Word Search.
Generate Valid Parentheses.
## Related Topics

[[apna_college]]
[[recursion]]