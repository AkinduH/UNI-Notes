

![[Pasted image 20240524191309.png]]


**Map Hierarchy**

1. **Map.Entry:** A nested interface within Map that represents a single key-value pair. It defines methods to get the key, get the value, and optionally set the value.
    
2. **Map:** The root interface of the map hierarchy. It defines the core methods for working with key-value pairs.
    
3. **AbstractMap:** An abstract class that provides a skeletal implementation of the Map interface. It's a good starting point for creating custom map implementations.
    
4. **SortedMap:** An interface extending Map, which guarantees that the map's entries will be sorted according to their keys' natural order or a provided comparator.
    
5. **NavigableMap:** An interface extending SortedMap, offering additional navigation methods like finding the closest matches to a given key.
    

**Concrete Map Implementations**

- **HashMap:** The most commonly used map implementation. It stores key-value pairs in an unordered fashion, using hashing for efficient lookups.
- **LinkedHashMap:** Similar to HashMap, but maintains the insertion order of keys.
- **TreeMap:** Implements SortedMap and NavigableMap, storing key-value pairs in sorted order based on the keys' natural order or a comparator.
- **WeakHashMap:** A specialized map where keys are weakly referenced. If a key is no longer strongly referenced elsewhere, its entry can be garbage collected.
- **Hashtable:** A legacy synchronized map implementation (thread-safe). Generally replaced by `ConcurrentHashMap` in modern Java.
- **Properties:** A subclass of Hashtable that can be saved to or loaded from a stream. Often used for configuration files.