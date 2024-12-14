
**Collections vs. Abstract Data Types (ADTs):**

- ADTs: Conceptual blueprints for data structures, defining behavior but not implementation.
- Collections: Concrete implementations of ADTs, providing specific data structures and algorithms.
- Java Collections Framework (JCF): A unified way to work with various collection types in Java.

**Pre-Collections Framework in Java:**

- Arrays: Fixed-size, efficient random access, but inflexible.
- Vectors and Hashtables: Dynamic, synchronized, but less efficient due to overhead.
- Lack of a common interface for manipulating collections.

**Advantages of the Java Collections Framework:**

- Consistent API: Easy to learn, promotes code readability and maintainability.
- Reduced programming effort: Abstraction and encapsulation simplify code.
- Increased performance: Optimized implementations for various use cases.
- Other benefits: Interoperability, type safety, concurrency support.

**Performance Considerations:**

- Different collections excel at different operations (e.g., random access, insertions/deletions, search).
- Choice depends on your specific use case and the frequency of different operations.

**Examples of Collections in Java:**

- ArrayList: Dynamic array, good for random access.
- LinkedList: Doubly-linked list, good for insertions/deletions.
- HashSet: Unordered set, fast membership testing.
- TreeSet: Ordered set, fast membership testing and range queries.
- HashMap: Unordered map, fast key-value lookups.
- TreeMap: Ordered map, fast key-value lookups and range queries.

[[Java Collection Hierarchy]]

[[Java Map Hierarchy]]


### HashMap

**Unsynchronized:** `HashMap` is not thread-safe by default; use `ConcurrentHashMap` for concurrent access.

**Initial Capacity and Load Factor:**

- Initial Capacity: The number of buckets when the `HashMap` is created.
- Load Factor: The maximum allowed occupancy before the `HashMap` is resized (rehashed).
- Default values: Initial capacity = 16, load factor = 0.75.
- **Threshold:** The threshold is the point at which the HashMap decides it's time to resize. It is calculated as: `capacity * load factor`.
- Tuning these values can impact performance.

The default sequence of rehashing sizes for a `HashMap` in Java is a series of powers of two:
- **16** (initial capacity)
- **32** (after the first rehash)
- **64** (after the second rehash)



![[Pasted image 20240524205250.png]]

**Memory Overhead:**
- `HashMap` object: 48 bytes
- Array (hash table): 16 bytes
- Each `HashMap$Entry`: 36 bytes(When empty 16 * 4 = 64 bytes)
- Total for an empty `HashMap` (default capacity 16): 128 bytes


### LinkedList


![[Pasted image 20240524231124.png]]


**Memory Overhead**
- **LinkedList Object Overhead:** 24 bytes
- **LinkedList$Entry Object Overhead:** 24 bytes
- **Empty LinkedList Size (Default):** 48 bytes (24 bytes for the `LinkedList` object + 24 bytes for the initial empty `LinkedList$Entry` object)


### ArrayList

- **Dynamic Array:** An `ArrayList` is essentially a dynamic array, meaning its size can change automatically as you add or remove elements.
- **Object Storage:** Unlike regular arrays, which can hold primitive types, `ArrayList` only stores objects. If you need to store primitives, you'll need to use their corresponding wrapper classes (e.g., `Integer` for `int`, `Double` for `double`).
![[Pasted image 20240524231626.png]]

**Memory Overhead**
- **ArrayList Object Overhead:** 32 bytes
- **Empty ArrayList Size (Default):** 88 bytes (32 bytes for the `ArrayList` object + 56 bytes for the initial array with a capacity of 10)


### Overhead Comparison of Collection Implementations

![[Pasted image 20240524231916.png]]

Hash-based collections have a much larger overhead
Additional data stored in hash-based collections improve the performance of search for items and therefore insertion and deletion



![[Pasted image 20240524232818.png]]


![[Pasted image 20240524232836.png]]

![[Pasted image 20240524232859.png]]