

![[Pasted image 20240524191224.png]]

- **List:** An ordered collection that allows duplicate elements.
    
    - **AbstractList:** An abstract class that partially implements the List interface.
    - **ArrayList:** Implements List using a dynamic array (resizable). Offers fast random access but slower insertions/deletions.
    - **LinkedList:** Implements List using a doubly-linked list. Efficient for insertions/deletions but slower random access.
    - **Vector:** Similar to ArrayList, but synchronized (thread-safe).
    - **Stack:** Extends Vector, implementing a Last-In-First-Out (LIFO) structure.
- **Queue:** A collection designed for holding elements prior to processing. Typically follows a FIFO (First-In-First-Out) order.
    
    - **AbstractQueue:** An abstract class that partially implements the Queue interface.
    - **AbstractSequentialList:** Another abstract class, a parent of LinkedList (although not shown in the diagram) and indirectly of Deque.
    - **Deque:** A double-ended queue, allowing insertions and deletions at both ends.
    - **ArrayDeque:** Implements Deque using a resizable array.
    - **PriorityQueue:** Implements Queue but orders elements based on priority (natural order or a custom comparator).
- **Set:** An unordered collection that does not allow duplicate elements.
    
    - **AbstractSet:** An abstract class that partially implements the Set interface.
    - **HashSet:** Implements Set using a hash table. Offers constant-time performance for basic operations.
    - **LinkedHashSet:** Extends HashSet but maintains insertion order.
    - **TreeSet:** Implements Set using a red-black tree. Elements are sorted according to natural order or a comparator.
    - **SortedSet:** An interface extending Set, guaranteeing that elements are always in sorted order.