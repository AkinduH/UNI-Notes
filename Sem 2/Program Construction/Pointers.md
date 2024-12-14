
The `->` operator in C++ is used to access members (variables or methods) of an object through a pointer to that object.

Here’s a more detailed explanation:

- Suppose you have a pointer to an object, like this: `Node* tmp;`. `tmp` is a pointer to a `Node` object.
- Now, suppose the `Node` object has a member variable `data`. Normally, if you have an actual `Node` object (not a pointer), you could access `data` like this: `Node tmp; tmp.data;`.
- But when you only have a pointer to the object, you can’t use the `.` operator directly. You need to dereference the pointer first to get the actual object. So, you might think to do it like this: `(*tmp).data;`. Here, `*tmp` dereferences the pointer (i.e., gets the actual object that the pointer points to), and then `.data` accesses the `data` member of that object.
- The `->` operator is a shorthand for this operation. So, instead of writing `(*tmp).data;`, you can write `tmp->data;`. This does exactly the same thing: it dereferences the pointer and accesses the `data` member of the object.

So, in summary, the `->` operator is used to access members of an object when you have a pointer to that object. It’s a convenient shorthand that combines dereferencing a pointer and accessing a member.





```c
&ptr1       (address of ptr1 on the stack)
ptr1        (address of the allocated memory on the heap)
*ptr1       (indeterminate value at the allocated memory)
```

```c
int *ptr;
```

- **Memory Allocation**: The pointer itself (the variable `ptr`) is allocated on the stack because it is a local variable. However, at this point, it does not point to any valid memory location (it is uninitialized).
- **Initial Value**: The value of `ptr` is indeterminate (it could be any value), as it hasn't been initialized yet.

```c
int *ptr1; 
printf("%i\n", &ptr1);  //1907432744
printf("%i\n", ptr1);   //0
printf("%i\n", *ptr1);  //Segmentation fault
```

```c
Stack:
ptr -> indeterminate (garbage value)
```




```c
ptr = malloc(sizeof(int));
```

- **Memory Allocation**: `malloc(sizeof(int))` requests the allocation of a block of memory large enough to store one `int`.`malloc` allocates this memory from the heap, which is a region of memory used for dynamic memory allocation.
- **After Allocation**: Now, `ptr` points to a valid memory location on the heap that can hold an integer value. The content of this allocated memory is not initialized, so it contains an indeterminate value.

```c
    int *ptr1; 
    ptr1 = malloc(sizeof(int));
    printf("%i\n", &ptr1);    //218707688
    printf("%i\n", ptr1);     //11936416
    printf("%i\n", *ptr1);    //0
```

```c
Heap:
+------+    (memory allocated by malloc)
|  ??? |    (int-sized memory block)
+------+
   ^
   |
ptr -> (points to the allocated memory block)
```


```c
free(ptr);
```

- **Memory Deallocation**: The memory block on the heap that `ptr` points to is released back to the system, making it available for future allocations.
- **Pointer Status**: The `ptr` pointer itself remains in the same state (i.e., it still holds the address of the memory block that was just freed), but the memory at that address is no longer valid for access.

- **Dangling Pointer**: After calling `free(ptr);`, `ptr` becomes a dangling pointer because it points to a memory location that has been freed and is no longer valid. Accessing the memory location through a dangling pointer (e.g., dereferencing `ptr` with `*ptr`) results in undefined behavior.

```c
Heap:
+------+    (memory is deallocated and available for future use)
|      |    (memory content is no longer valid)
+------+
   ^
   |
ptr -> (still points to the same memory location, but it is now invalid)

```



```c
int main(int argc , char *argv[]) {
    int *ptr1; 
    printf("%i\n", *ptr1);  //Segmentation fault
    func2(ptr1);
    printf("%i\n", *ptr1);  //Segmentation fault
    free(ptr1); 
    printf("%i\n", *ptr1);  //Segmentation fault
}

void func2(int *ptr1) { // pointer pass-by-value
    int *ptr2;
    printf("%i %i\n", *ptr1, *ptr2);  //Segmentation fault
    ptr1 = malloc(sizeof(int)); 
    ptr2 = malloc(sizeof(int)); 
    printf("%i %i\n", *ptr1, *ptr2);  //0 0
}
```


```c
int *ptr1;
int **ptr1 = &ptr1;


Memory_Address(Stack)|    Variable   |   Value
--------------------------------------------------------
0x1000               |    ptr1       |  uninitialized
0x2000               |    *ptr1      |  0x1000 (address of ptr1)
--------------------------------------------------------

```