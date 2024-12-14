
![[Pasted image 20240524130050.png]]


## Garbage Collection

**The Garbage Collector (GC):**

- **Separate Thread:** The GC runs as a separate low-priority thread alongside your main application.
- **Automated Memory Management:** It automatically detects and reclaims memory occupied by objects that are no longer reachable by the program.
- **Performance Overhead:** GC does introduce some overhead, typically up to 10% of execution time. This is a trade-off for the convenience of automatic memory management.
- The Java garbage collector is responsible for reclaiming memory used by objects, not primitive types.
- Primitive types are allocated on the stack, and their memory is automatically reclaimed when they go out of scope.

**How Garbage Collection Works:**

1. **Marking:** Identifies objects in use (reachable) by traversing references from root objects.
2. **Sweeping:** Finds and marks unreachable objects as garbage.
3. **Reclaiming:** Frees memory occupied by garbage objects.
4. **Compaction (Optional):** Rearranges remaining objects to reduce fragmentation.

**Benefits:**

- **Reduced Memory Leaks:** Automatic reclamation of unused memory.
- **Improved Productivity:** Developers focus on logic, not manual memory management.
- **Safer Memory:** Prevents dangling pointers and double frees.
- **Optimization:** Generational collection for better performance.

**Challenges:**

- **Performance Overhead:** Can pause program execution during collection cycles.
- **Non-Deterministic:** Timing of collections might not be predictable.

**Algorithms:**

- **Mark and Sweep:** Basic algorithm for identifying and reclaiming garbage.
- **Copying Collector:** Copies live objects to a new space, compacting memory.
- **Generational Collector:** Focuses on younger objects, improving efficiency.
- **Reference Counting:** Tracks object references, deallocates when the count reaches zero.

**Important Considerations:**

- **GC Limitations:** Doesn't eliminate all memory leaks (e.g., circular references).
- **Performance Impact:** Understanding GC overhead and choosing appropriate algorithms is crucial for optimal performance.


## Garbage Collection Techniques

**1. Reference Counting:**

- **Mechanism:** Tracks the number of references to each object.
- **Deallocation:** Object is freed when its reference count reaches zero.
- **Pros:** Simple, immediate deallocation.
- **Cons:** Overhead, struggles with circular references.

**2. Tracing:**

- **Mechanism:** Starts from root objects, follows references, marks reachable objects.
- **Deallocation:** Unmarked objects are considered garbage.
- **Pros:** Handles circular references, can be optimized.
- **Cons:** May cause temporary pauses (stop-the-world).

**Combining Reference Counting and Tracing:**

- **Purpose:** Leverages strengths of both techniques.
- **Common Approach:** Reference counting for quick deallocation, tracing for thorough cleanup.

**Additional Considerations:**

- **Generational GC:** Divides objects by age, optimizes collection frequency.
- **Weak References:** References that don't prevent garbage collection.


```python
# Python (Reference Counting and Tracing)
a = [1, 2, 3]   # Reference count of the list is 1
b = a          # Reference count of the list is 2
a = None       # Reference count decrements to 1
del b          # Reference count decrements to 0, list is garbage collected
```


## Memory Layout for C programs


![[Pasted image 20240524130853.png]]

**1. Text Segment:**

- Location: Lowest memory addresses.
- Content: Compiled machine instructions of the program.
- Characteristics: Read-only, often shareable.

**2. Data Segment:**

- **Initialized Data:** Stores global/static variables with explicit initial values.
- **Uninitialized Data (BSS):** Stores global/static variables initialized to zero.

**3. Heap:**

- Location: Grows upwards from the data segment.
- Content: Dynamically allocated memory (`malloc`, `calloc`, `realloc`).
- Management: Requires manual deallocation (`free`).

**4. Stack:**

- Location: Grows downwards from high memory addresses.
- Content: Local variables, function parameters, return addresses.
- Structure: Last-In-First-Out (LIFO).

**5. Command-Line Arguments & Environment Variables:**

- Location: Typically above the stack.
- Content: Arguments passed to the program, system environment information.

**Key Points:**

- **Dynamic vs. Static Memory:** Heap and stack are dynamic, while text and data are static.
- **Memory Growth & Overlap:** Heap and stack grow towards each other, potential for stack overflow.
- **Heap Management:** Crucial for avoiding memory leaks and ensuring stability.

## Efficient Use of Memory in Java

##### Caching
In the context of computing and memory management, refers to the process of storing copies of data in a temporary, easily accessible location to speed up future retrievals. This temporary storage location is called a "cache."

- **Benefits:** Caching can significantly reduce allocation and garbage collection costs by storing frequently accessed data in memory.
- **Drawbacks:** Caching increases memory footprint, so it's important to strike a balance based on your application's needs.


**The `new` Operator and Object Creation**

- **Purpose:** The `new` operator is the primary way to create objects in Java. It does the following:
    1. **Allocates Memory:** It dynamically allocates memory on the heap for the new object based on the class definition.
    2. **Initializes:** It calls the constructor of the class to initialize the object's state (its instance variables).
    3. **Returns a Reference:** It returns a reference to the newly created object, which you typically store in a variable of the appropriate type.

```java
int i = 1040;           // Primitive data type (stored directly on the stack)
Integer intObj = new Integer(1040); // Wrapper class object (stored on the heap)
```
- `int` is a primitive type representing an integer value. It's stored directly in memory (often on the stack).
- `Integer` is a wrapper class that provides a way to use an `int` value as an object. Objects are always stored on the heap.

> [!Primitive Data Types:]
>
> - **`int`:** Stores a 32-bit integer, occupying 4 bytes of memory.
>     
> - **`long`:** Stores a 64-bit integer, occupying 8 bytes of memory.
>     
> - **`boolean`:** Represents a logical true/false value and typically uses 1 byte (although it might be optimized to use a single bit in some cases).
>     
> - **`char`:** Represents a single Unicode character and occupies 2 bytes.
>     
> - **`float`:** Stores a 32-bit floating-point number, occupying 4 bytes.
>     
> - **`double`:** Stores a 64-bit floating-point number, occupying 8 bytes.
>     
> 
> **Reference Data Types:**
> 
> - **`String`:** Strings are more complex. The memory used depends on:
>     
>     - The length of the string (each character takes 2 bytes in UTF-16 encoding).
>     - Overhead for the object itself (usually 12 bytes or more, depending on the JVM implementation).
> - **Arrays:** Arrays store a collection of elements of the same type. The memory used depends on:
>     
>     - The type of elements in the array (the size of each element is determined by its data type).
>     - The number of elements in the array.
>     - Overhead for the array object itself.
> - **Custom Objects:** For objects you create from your own classes, the memory used depends on:
>     
>     - The number and types of instance variables (fields) the class has.
>     - Overhead for the object itself.

#### integer

**Object Overhead:** 
The metadata (class pointer, flags, locks, size) collectively contribute to the object overhead. This is the extra memory consumed by an object beyond the space needed for its actual data
**Class pointer** - pointer to class information, for example,
Integer or String
**Flags** - object shape, hash code, etc
**Lock** - lock variable or a pointer to a monitor object used to
control concurrent access
**Size** - the length of the array for array-type objects

- int variable to Integer object size ratio is 1:4

#### String

- A fixed memory overhead of 352 bits is added to any stored string
- Short string objects give a very high overhead ratio

![[Pasted image 20240524140528.png]]

String s3 = new StringBuilder(String.valueOf(s1)).append(s2).toString()

![[Pasted image 20240524141151.png]]

**String Interning:**

- **Purpose:** Memory optimization technique that stores only one copy of each unique string.
- **Requirement:** Interned strings must be immutable (unchangeable).
- **Implementation:** JVM maintains a string intern pool (cache) to store unique string references.

**String Literals:**

- **Definition:** Strings enclosed in double quotes (e.g., `"hello"`).
- **Automatic Interning:** Java compiler automatically interns string literals.

**Benefits:**

- **Memory Savings:** Reduces memory usage by avoiding duplicate strings.
- **Faster Comparisons:** `==` operator can be used for efficient reference comparison of interned strings.

**The JVM efficiently determines if a literal string is already stored in the string intern pool (cache) using a hash table (or a similar data structure).**

```java
String s1 = "hello";
String s2 = "hello";
String s3 = new String("hello");
```

- **`s1` and `s2`:** The JVM will intern "hello" once and both `s1` and `s2` will refer to the same object in the intern pool.
- **`s3`:** This creates a new string object on the heap (not in the intern pool) because you used the `new` keyword. However, you can still manually intern `s3` using `s3.intern()`.
Strings created at runtime (e.g., using the `new String()` constructor) are not automatically interned unless you explicitly call `intern()`.


In java

![[Pasted image 20240804150417.png]]