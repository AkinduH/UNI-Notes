
A **heap** is a specialized tree-based data structure that satisfies the heap property.


![[Pasted image 20240307162026.png]]


- **Heap Property:** This property ensures that the minimum (or maximum) element is always at the root of the tree according to the heap type.

- **Parent-Child Relationship:** The relationship between a parent node at index **‘i’** and its children is given by the formulas: 

> [! ]
> 	Parent                  floor((i-1)/2)
> 	left child at index       2i+1
> 	right child at index      2i+2
> 	(In 0-based Indexing)

  for 0-based indexing of node numbers.

**A heap is always a complete binary tree.** 
**Priority Queues:** Heaps are the underlying data structure for priority queues, where elements with higher priority are dequeued first.
**Graph Algorithms:** Dijkstra's shortest path algorithm and Prim's minimum spanning tree algorithm use heaps.

## Operations Supported by Heap:

##### **Heapify**

It is the process to rearrange the elements to maintain the property of heap data structure.
It takes **O(log N)** to balance the tree.

The recurrence relation for HEAPIFY function of heapsort algorithm is 

$$
T(n) <= T(2n/3) + O(1)
$$

```
pseudocode for MAX - Heapify

MAX-HEAPIFY(A, i):
    l ← LEFT(i); r ← RIGHT(i)
    IF l ≤ A.heap-size and A[l] > A[i]
        largest = l
    ELSE
        largest = i
    IF r ≤ A.heap-size and A[r] > A[largest]
        largest = r
    IF largest ≠ i
        exchange A[i] ↔ A[largest]
        MAX-HEAPIFY(A, largest)

```

```
pseudocode for MIN - Heapify

MIN-HEAPIFY(A, i)
    l ← LEFT(i) // Index of left child
    r ← RIGHT(i) // Index of right child

    if l ≤ A.heap-size and A[l] < A[i]
        smallest ← l
    else
        smallest ← i

    if r ≤ A.heap-size and A[r] < A[smallest]
        smallest ← r

    if smallest ≠ i
        swap A[i] and A[smallest]
        MIN-HEAPIFY(A, smallest)
```


##### **BUILD-HEAP**

The BUILD-HEAP operation is used to transform an unordered array into a min/max heap.

```
pseudocode for BUILD-MAX-HEAP

BuildMaxHeap(arr, n):
    startIdx = n // 2 - 1  # Index of last non-leaf node
    
    for i in range(startIdx, -1, -1):
        MAX-HEAPIFY(A, i) or MIN-HEAPIFY(A, i)

```


The time complexity of BUILD-MAX-HEAP is O(n), 
which is more efficient than calling heapify on each node individually 
(O(n log n)). 

This is because the tree is traversed in reverse level order, and each call to heapify is performed on a smaller part of the array.


###### C++ program for building Heap from Array
```c++
#include <bits/stdc++.h>

using namespace std;

// To heapify a subtree rooted with node i which is
// an index in arr[]. N is size of heap
void heapify(int arr[], int N, int i)
{
	int largest = i; // Initialize largest as root
	int l = 2 * i + 1; // left = 2*i + 1
	int r = 2 * i + 2; // right = 2*i + 2

	// If left child is larger than root
	if (l < N && arr[l] > arr[largest])
		largest = l;

	// If right child is larger than largest so far
	if (r < N && arr[r] > arr[largest])
		largest = r;

	// If largest is not root
	if (largest != i) {
		swap(arr[i], arr[largest]);

		// Recursively heapify the affected sub-tree
		heapify(arr, N, largest);
	}
}

// Function to build a Max-Heap from the given array
void buildHeap(int arr[], int N)
{
	// Index of last non-leaf node
	int startIdx = (N / 2) - 1;

	// Perform reverse level order traversal
	// from last non-leaf node and heapify
	// each node
	for (int i = startIdx; i >= 0; i--) {
		heapify(arr, N, i);
	}
}

// A utility function to print the array
// representation of Heap
void printHeap(int arr[], int N)
{
	cout << "Array representation of Heap is:\n";

	for (int i = 0; i < N; ++i)
		cout << arr[i] << " ";
	cout << "\n";
}

// Driver Code
int main()
{
	// Binary Tree Representation
	// of input array
	//			 1
	//		 / \
	//		 3	 5
	//	 / \	 / \
	//	 4	 6 13 10
	// / \ / \
	// 9 8 15 17
	int arr[] = {1, 3, 5, 4, 6, 13, 10, 9, 8, 15, 17};

	int N = sizeof(arr) / sizeof(arr[0]);

	// Function call
	buildHeap(arr, N);
	printHeap(arr, N);

	// Final Heap:
	//			 17
	//		 / \
	//		 15	 13
	//		 / \	 / \
	//	 9	 6 5 10
	//	 / \ / \
	//	 4 8 3 1

	return 0;
}

```


**HEAP-INCREASE-KEY**

Input: A: an array representing a heap, i : an array index, key : a new key  

Output: A still representing a heap where the key of A[i] was increased to key 

Running Time: O(log n)


```
HEAP-INCREASE-KEY(A, i, key)
	if key < A[i]
      then error "New key must be larger than current key"
    A[i] ← key
    while i > 0 and A[PARENT(i)] < A[i]
       do exchange A[i] with A[PARENT(i)]
       i ← PARENT(i)
```

**Bubble Up (Fix Violation):** Since increasing a node's key might make it larger than its parent, potentially violating the max-heap property, the procedure needs to "bubble up" the modified node until it finds its correct position in the heap.

#### **Heap Insert**

Heap-Insert operation is used to add a new key (as a new node) to a heap. After Heap-Insert the size of the heap increases by 1.

Running Time: O(log n)

```
heap-size[A] ←heap-size[A] + 1 
A[heap-size[A]] ← −∞  
Heap-Increase-Key(A[heap-size[A]], key)
```

Alternative approach 
```
HEAP-INSERT(A, key)
  heap-size[A] ←heap-size[A] + 1  
  i ← heap-size[A] 
  A[i] ← key
  while i > 0 and A[PARENT(i)] < A[i]
    do exchange A[i] with A[PARENT(i)]
    i ← PARENT(i)
```

#### **Heap Extract Max**

In a max heap, Heap-Extract-Max operation removes the max key (which is at the root), fixes heap and return the removed max key.

Running Time: O(log n)

```
HEAP-EXTRACT-MAX(A)
    if A.heap-size < 1
        error "Heap underflow"
    max ← A[1]
    A[1] ← A[A.heap-size]
    A.heap-size ← A.heap-size - 1
    MAX-HEAPIFY(A, 1)
    return max
```

Recursively "bubbles down" the new root element to its correct position in the heap by comparing and swapping it with its children as needed.

#### **Peak**

The `peek` operation simply returns the value stored at the root of the heap.

```
HEAP-PEEK(A)
    if A.heap-size < 1
        error "Heap underflow"
    return A[0]  // Return the root element (0-based indexing)
```


# Heap Sort


![[1-(1).webp]]![[2-(1).webp]]![[3-(1).webp]]![[4-(1).webp]]![[6.webp]]![[7.webp]]


```
pseudocode for Heap Sort

HeapSort(A)
    BuildHeap(A)
    for i <- length(A) downto 2
        exchange A[1] <-> A[i]
        heapsize <- heapsize - 1
        Heapify(A, 1)
```


```c++
// C++ program for implementation of Heap Sort

#include <iostream>
using namespace std;

// To heapify a subtree rooted with node i
// which is an index in arr[].
// n is size of heap
void heapify(int arr[], int N, int i)
{

	// Initialize largest as root
	int largest = i;

	// left = 2*i + 1
	int l = 2 * i + 1;

	// right = 2*i + 2
	int r = 2 * i + 2;

	// If left child is larger than root
	if (l < N && arr[l] > arr[largest])
		largest = l;

	// If right child is larger than largest
	// so far
	if (r < N && arr[r] > arr[largest])
		largest = r;

	// If largest is not root
	if (largest != i) {
		swap(arr[i], arr[largest]);

		// Recursively heapify the affected
		// sub-tree
		heapify(arr, N, largest);
	}
}

// Main function to do heap sort
void heapSort(int arr[], int N)
{

	// Build heap (rearrange array)
	for (int i = N / 2 - 1; i >= 0; i--)
		heapify(arr, N, i);

	// One by one extract an element
	// from heap
	for (int i = N - 1; i > 0; i--) {

		// Move current root to end
		swap(arr[0], arr[i]);

		// call max heapify on the reduced heap
		heapify(arr, i, 0);
	}
}

// A utility function to print array of size n
void printArray(int arr[], int N)
{
	for (int i = 0; i < N; ++i)
		cout << arr[i] << " ";
	cout << "\n";
}

// Driver's code
int main()
{
	int arr[] = { 12, 11, 13, 5, 6, 7 };
	int N = sizeof(arr) / sizeof(arr[0]);

	// Function call
	heapSort(arr, N);

	cout << "Sorted array is \n";
	printArray(arr, N);
}

```


***Time Complexity:***  O(N log N)

Time complexity of heap sort is primarily dependent on the Heapify operation.
	Heapify has O(log n) time complexity and may be repeated (n-1) times. Build heap has O(n) time complexity and occurs only once.

**In-Place Sorting**
**Not Stable**
**Not Adaptive**

Heap sort is **NOT*** at all a Divide and Conquer algorithm.

The Merge sort is slightly faster than the Heap sort. But on the other hand merge sort takes extra memory.


[[Hashtable]]