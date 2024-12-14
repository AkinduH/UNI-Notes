
- **Divide and Conquer:**
    
    - A problem-solving paradigm that breaks a problem into smaller, similar subproblems.
    - Solves subproblems recursively or iteratively.
    - Combines solutions to subproblems to solve the original problem.
    - Often leads to efficient algorithms (e.g., O(n log n) for sorting).
    - Can be parallelized.
    - Examples - Merge Sort, Quick Sort, Binary Search, Depth-first tree traversals
- **Recursion:**
    
    - A programming technique where a function calls itself.
    - Requires a base case to terminate the recursion.
    - Can provide elegant solutions for problems with recursive structures.
    - Might have overhead due to function calls.
    - Can be used to implement divide-and-conquer algorithms.
    - Examples - Factorial, Linked List traversal ,Creating a long string by duplicating a string several times


---

## Merge sort


![[Pasted image 20240514183025.png]]

```
MERGE-SORT(A, p, r)
1. if p < r 
2.   q = floor((p + r) / 2)   // Find the middle point to divide the array
3.   MERGE-SORT(A, p, q)     // Recursively sort the left half
4.   MERGE-SORT(A, q + 1, r) // Recursively sort the right half
5.   MERGE(A, p, q, r)       // Merge the sorted halves
```

```
MERGE(A, p, q, r)
1. n1 = q - p + 1
2. n2 = r - q
3. let L[1...n1 + 1] and R[1...n2 + 1] be new arrays
4. for i = 1 to n1
5.   L[i] = A[p + i - 1]
6. for j = 1 to n2
7.R[j] = A[q + j]
8. L[n1 + 1] = infinity    // Sentinel value
9. R[n2 + 1] = infinity
10. i = 1
11. j = 1
12. for k = p to r
13.   if L[i] <= R[j]
14.     A[k] = L[i]
15.     i = i + 1
16.   else
17.     A[k] = R[j]
18.     j = j + 1 
```

- **Working Principle:** A divide-and-conquer algorithm that recursively splits an array into two halves, sorts each half, and then merges the sorted halves back together.
- **Time Complexity:**
	- Best Case: O(n log n)
	- Average Case: O(n log n)
	- Worst Case: O(n log n)
- **Space Complexity:** O(n) (Linear space)
-  **Out of Place Sorting** 
- **Stable Sorting:** It maintains the relative order of equal elements.
- **Linked Lists:** It's particularly efficient for sorting linked lists, as merging two sorted linked lists can be done in linear time with constant space.
- `MERGE` procedure has a linear time complexity of O(n)

### Optimized Merge sort

```
MERGE-SORT(A, p, r)
1. if p < r 
2.   q = floor((p + r) / 2)   // Find the middle point to divide the array
3.   MERGE-SORT(A, p, q)     // Recursively sort the left half
4.   MERGE-SORT(A, q + 1, r) // Recursively sort the right half
5.   if A[q] > A[q + 1]      // Check if merging is necessary
6.       MERGE(A, p, q, r)       // Merge the sorted halves
```

```
MERGE(A, p, q, r)
1. n1 = q - p + 1
2. n2 = r - q
3. let L[1...n1] and R[1...n2] be new arrays
4. for i = 1 to n1
5.   L[i] = A[p + i - 1]
6. for j = 1 to n2
7.   R[j] = A[q + j]
8. i = 1
9. j = 1
10. for k = p to r
11.   if i > n1          
12.       A[k:r+1] = R[j:] // Copy remaining elements from R 
13.       break           // Exit the loop
14.   else if j > n2        
15.       A[k:r+1] = L[i:] // Copy remaining elements from L
16.       break           // Exit the loop
17.   else                
18.       if L[i] <= R[j]
19.           A[k] = L[i]
20.           i = i + 1
21.       else
22.           A[k] = R[j]
23.           j = j + 1 
```


**Skipping Merge if Already Sorted**
A new condition (`if A[q] > A[q + 1]`) is added before calling the `MERGE` procedure. This condition checks if the last element of the left subarray (`A[q]`) is greater than the first element of the right subarray (`A[q + 1]`).

**Merging with Early Termination:** 
The loop now checks if either subarray has been exhausted (lines 11-16).
- If `L` is exhausted, the remaining elements in `R` are copied to `A`, and the loop breaks.
- If `R` is exhausted, the remaining elements in `L` are copied to `A`, and the loop breaks.

**Memory Overhead in using infinity:** 
While minimal, using sentinel values requires allocating one extra element in each of the temporary arrays `L` and `R`.

---

## Quick sort


![[Pasted image 20240514194029.png]]


```
QUICKSORT(A, p, r)
1. if p < r
2.   q = PARTITION(A, p, r)  // Partition the array around a pivot
3.   QUICKSORT(A, p, q - 1) // Recursively sort the left partition
4.   QUICKSORT(A, q + 1, r) // Recursively sort the right partition
```

```
PARTITION(A, p, r)
1. x = A[r] // Choose the last element as the pivot
2. i = p - 1
3. for j = p to r - 1
4.   if A[j] <= x
5.     i = i + 1
6.     swap A[i] and A[j]
7. swap A[i + 1] and A[r] // Place the pivot in its correct position
8. return i + 1        // Return the final pivot index
```

- **Working Principle:** A divide-and-conquer algorithm that selects a pivot element and partitions the array into two subarrays, one with elements smaller than the pivot and another with elements greater than the pivot. The pivot is then in its final sorted position. This process is recursively applied to both subarrays.
- **In-Place Sorting** 
- **Not Stable** 
- **Time Complexity:**
	- Best Case: O(n log n)
	- Average Case: O(n log n)
	- Worst Case: O(n²) (when the array is already sorted or nearly sorted)
- **Space Complexity:** O(log n) (due to recursive calls)
- **Pivot Selection:** The choice of pivot significantly impacts performance. Ideally, the pivot should divide the array into roughly equal-sized partitions.
- **Usually Fast:** On average, Quick Sort is one of the fastest sorting algorithms.


**Worst-Case Scenario**

- **Sorted or Nearly Sorted Array:** If the array is already sorted (ascending or descending), and we consistently choose the first or last element as the pivot, the partitions will be extremely unbalanced.
    
    - For example, if we pick the last element as the pivot in an ascending sorted array, all other elements will be smaller, resulting in one empty subarray and one subarray with `n-1` elements.
    - This effectively reduces the problem size by only 1 element with each recursive call, leading to a total of `n` recursive calls.

**Avoiding the Worst-Case**

- **Randomized Pivot Selection:** Choosing a random pivot at each step drastically reduces the probability of consistently picking bad pivots and hitting the worst-case scenario.
- **Median-of-Three Pivot Selection:** Selecting the median of the first, middle, and last elements as the pivot can be a more robust strategy, providing a better chance of balanced partitions.
- **Hybrid Approach:** For smaller subarrays (e.g., less than 10 elements), switching to Insertion Sort can avoid the overhead of Quick Sort's recursion and potentially improve performance.


| Sorting Algorithm | Best Case Time | Average Time | Worst Case Time | Space Complexity | Stability | In-Place Sorting |
| ----------------- | -------------- | ------------ | --------------- | ---------------- | --------- | ---------------- |
| Bubble Sort       | O(n)           | O(n²)        | O(n²)           | O(1)             | Yes       | Yes              |
| Insertion Sort    | O(n)           | O(n²)        | O(n²)           | O(1)             | Yes       | Yes              |
| Selection Sort    | O(n²)          | O(n²)        | O(n²)           | O(1)             | No        | Yes              |
| Merge Sort        | O(n log n)     | O(n log n)   | O(n log n)      | O(n)             | Yes       | No               |
| Quick Sort        | O(n log n)     | O(n log n)   | O(n²)           | O(log n)         | No        | Yes              |

[[Analyzing Recursion]]