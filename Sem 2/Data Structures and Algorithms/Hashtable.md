
![[Pasted image 20240317185355.png]]

### Direct Access Table

Here the hash function is as follows;

_H(k) = k_

In direct address tables, records are placed using their key values directly as indexes.

**Searching in O(1) Time**
**Insertion in O(1) Time**
**Deletion in O(1) Time**

**Limitations:**

1. Prior knowledge of maximum key value
2. Practically useful only if the maximum value is very less.
3. It causes wastage of memory space if there is a significant difference between total records and maximum value.

Hashing can overcome these limitations of direct address tables.

## Hash Table

Key Idea : Size of the table doesn’t depend on the size of the universe of keys.

Hashing is a one way function.

### Hash Functions

There are three main methods for designing hash functions.

1.Division method

H(k) = k mod m

Need to choose m. Good approach for choosing m is a prime not close to an exact power of 2.

2.Multiplication method

H(k) = | m (kA mod 1) |     0 < A < 1

Need to choose A and m. Typically m is a power of 2.

3.Universal hashing

Selecting a hash function at random from a family of hash functions with a certain mathematical property.


**Strategies for Improving performance**

- Increase table capacity
- Use better hash functions
- Use better collision resolution techniques


## Collisions

Hash table can result in multiple different keys mapping to the same hash value. Hence we need some method to distinguish the keys that are mapped to the same hash value.

The following sections are some of the techniques used to handle collisions.

#### **Chaining**

Chaining is simply forming a 'chain' with the keys that are mapped to the same hash value. In most of the times this can be a linked list.

Chaining - Search

Because of traversing in the linked list, searching is not simply an O(1) operation. To simplify this we define a term called load factor.

$$
load factor (α) = n/m
$$

- n = number of total keys stored in hash table
- m = size of the hash table

"average number of elements in a linked list"

**Time complexity of search operation with chaining is O(1+α).**


#### **Open Addressing**

Open addressing is a collision resolution strategy where collisions are resolved by storing. The exact open addressing strategy depends on the implementation of the probing method.

##### 1.**Linear Probing**

Linear probing is selecting the next possible location using a linear function. The new hash function becomes **(h’(k)+i) mod m** where i is the probe sequence value. 
First we start with i=0.
**Drawback:** Linear probing can lead to **primary clustering**, where different keys that hash to the same initial index follow the same probing sequence.
##### 2.**Quadratic Probing**

Quadratic probing is similar to linear probing except the **+i** part is replaced by some quadratic polynomial of i. Hash function becomes **(h’(k) + a*i + b*i^2 ) mod m** where a and b are constants.
**Drawback:** Quadratic probing can lead to **secondary clustering**, where different keys that hash to the same initial index follow the same probing sequence.
##### 3.**Double Hashing**

Double hashing technique tries to further reduce the collisions and clustering. In double hashing, probe sequence is generated with another hash function (h2). With this, the final hash function becomes  **(h’(k) + i h2(k)) mod m** .
significantly reduces both primary and secondary clustering.

**Open Addressing - Deletion:**
- **Challenges:** Deleting elements directly in open addressing can disrupt the probing sequence for other keys. If a key is removed, subsequent searches might not find keys that were inserted after it.
- **Solution: Tombstone Markers:** When a key is deleted, the slot is marked as "deleted" (e.g., with a special tombstone value). During insertion, these tombstone slots can be overwritten, but during search, the probing continues until an empty slot or the target key is found.


## Complexity Analysis of a Hash Table:

For lookup, insertion, and deletion operations, hash tables have an average-case time complexity of O(1). Yet, these operations may, in the worst case, require O(n) time, where n is the number of elements in the table.

| Hash Table Type           | Insertion (Avg/Worst) | Search (Avg/Worst) | Deletion (Avg/Worst)      |
| ------------------------- | --------------------- | ------------------ | ------------------------- |
| Basic (Ideal)             | O(1)                  | O(1)               | O(1)                      |
| Chaining (Linked Lists)   | O(1) / O(n)           | O(1+α) / O(n)      | O(1+α) / O(n)             |
| Open Addressing (Probing) | O(1/(1-α)) / O(n)     | O(1/(1-α)) / O(n)  | O(1/(1-α)) / O(n)<br><br> |

[[Graph]]
