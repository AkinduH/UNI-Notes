
## Insertion sort

![[2.gif]]

```psueodo code
INSERTION-SORT(A) 
1. for j = 2 to A.length 
2.     key = A[j] 
2.     // Insert A[j] into the sorted sequence A[1, ..., j - 1] 
3.     i = j - 1 
4.     while i > 0 and A[i] > key 
5.         A[i + 1] = A[i] 
6.         i = i - 1 
7.     A[i + 1] = key
```

- Insertion Sort is a simple and intuitive algorithm.
- It's efficient for small datasets or nearly sorted arrays.
- Time complexity:
    - Best Case: O(n) (when the array is already sorted)
    - Average Case: O(n^2)
    - Worst Case: O(n^2)
- **Space Complexity:** O(1) (Constant space)
- **In-Place Sorting**
- **Stable Sorting**
- **Adaptive


## Bubble sort


![[R.gif]]

```
BUBBLE-SORT(A)
1. do
2.    swapped = false
3.    for i = 2 to A.length
4.       if A[i-1] > A[i]
5.          temp = A[i]
6.          A[i] = A[i-1]
7.          A[i-1] = temp
8.          swapped = true
9. while swapped
```
\
**Bubble Up:** The largest (or smallest) element gradually "bubbles up" to its correct position at the end (or beginning) of the list with each pass.

- Simple and easy to understand.
- It's efficient for small datasets or nearly sorted arrays.
- Time complexity:
    - Best Case: O(n) (when the array is already sorted)
    - Average Case: O(n^2)
    - Worst Case: O(n^2)
- **Space Complexity:** O(1) (Constant space)
- **In-Place Sorting**
- **Stable Sorting**
- **Not Adaptive(Adaptive when flags are used)

```
Bubble Sort Optimized
n = A.length
do
    swapped = false
    for i = 2 to n do
        if (A[i - 1] > A[i]) then
            temp = A[i]
            A[i] = A[i - 1]
            A[i - 1] = temp
            swapped = true
        newLimit = i - 1 
    n = newLimit 
while swapped
```


## Selection sort

![[Selection-Sort-Gif.gif]]

```
SELECTION-SORT(A)
1. for i = 1 to A.length - 1
2.    smallest = i
3.    for j = i + 1 to A.length
4.        if A[j] < A[smallest]
5.            smallest = j
6.    swap A[i] and A[smallest]
```


- **Time Complexity:**
	- Best Case: O(n²)
	- Average Case: O(n²)
	- Worst Case: O(n²)
- **Space Complexity:** O(1) (Constant space)
- **In-Place Sorting**
- **Not Stable(can be modified to be stable)**
- **Not Adaptive**
- **Minimal Swaps



==Insertion Sort is generally the most efficient of these three.==


## **Adaptive Sorting**

An adaptive sorting algorithm is one whose time complexity improves as the input array becomes more sorted.

## In-place algorithms

1. An in-place algorithm transforms the input without using any extra memory. As the algorithm executes, the input is usually overwritten by the output, and no additional space is needed for this operation.
2. Several sorting algorithms rearrange the input into sorted order in-place, such as insertion sort, selection sort, quick sort, bubble sort, heap sort, etc. All these algorithms require a constant amount of extra space for rearranging the elements in the input array.

## Out-of-place algorithms

1. An algorithm that is not in-place is called a not-in-place or out-of-place algorithm. Unlike an in-place algorithm, the extra space used by an out-of-place algorithm depends on the input size.
2. The standard **merge sort algorithm**  is an example of out-of-place algorithm as it requires O(n) extra space for merging. The merging can be done in-place, but it increases the time complexity of the sorting routine.

[[Recursion and Divide and Conquer]]