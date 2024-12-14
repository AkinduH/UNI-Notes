
![[Pasted image 20240229191411.png]]


> [!NOTE]
> For a complete binary tree
> 
> Height(H) = $log2(n+1) - 1$
> 
> n = 2^(H+1) - 1

Q is ancestor of A            Q -------> A
A is descendent of Q

Internal node
	A node with at least one child.

Balanced trees are a type of binary tree where the difference between the heights of the left and right subtrees of any node in the tree is either -1, 0, or +1.

The Catalan number Cn​ represents the number of binary Search trees with n nodes.

The formula for Catalan numbers is:

$$
  
Cn =(2n)!/((n+1)!⋅n!​)​

$$


$$
Binary Seach trees = Cn
$$
$$
Binary trees = Cn.n!
$$

The predecessor of a node is the node with the highest key less than the given node’s key.

**Find Predecessor:**

```plaintext
function FindPredecessor(node)
    if node.left is not null then
        return FindMax(node.left)
    else
        predecessor = null
        ancestor = tree.root
        while ancestor is not node do
            if node.key > ancestor.key then
                predecessor = ancestor
                ancestor = ancestor.right
            else
                ancestor = ancestor.left
        return predecessor
```

The successor of a node is the node with the smallest key greater than the given node’s key.

**Find Successor:**

```plaintext
function FindSuccessor(node)
    if node.right is not null then
        return FindMin(node.right)
    else
        successor = null
        ancestor = tree.root
        while ancestor is not node do
            if node.key < ancestor.key then
                successor = ancestor
                ancestor = ancestor.left
            else
                ancestor = ancestor.right
        return successor
```


In a valid tree
   N nodes -------> N-1 edges 

Degree of a Node
	The degree of a node is the number of children that it has.

Depth of a node
	Distance from the root(number of edges)

Height of a node
	Distance from the leaf(number of edges)

#### k-ary tree

A k-ary tree is a rooted tree in which each node has no more than **k** children.

- A binary tree is a special case of a k-ary tree where **k = 2**. That is, each node has at most two children.

The height of a complete k-ary tree with n nodes is

$$
O(logk​n)
$$
Number of internal nodes

$$
(k^h-1)/(k-1)
$$

#### Binary Tree

**Binary Tree Traversal**

![[Pasted image 20240229213750.png]]


we can define several ways to traverse a binary tree.

1. In-order  -------  D B G E K H A C F J
2. Pre-order ------  A B D E G H K C F J
3. Post-order -----  D G K H E B J F C A

**Inorder Traversal (Left, Root, Right):**

```plaintext
function Inorder(node)
    if node is not null then
        Inorder(node.left)
        print(node.key)
        Inorder(node.right)
```

**Preorder Traversal (Root, Left, Right):**

```plaintext
function Preorder(node)
    if node is not null then
        print(node.key)
        Preorder(node.left)
        Preorder(node.right)
```


**Postorder Traversal (Left, Right, Root):**

```plaintext
function Postorder(node)
    if node is not null then
        Postorder(node.left)
        Postorder(node.right)
        print(node.key)
```


**Double-Linked Binary Tree:**

- Each node in a double-linked binary tree has a link to its parent, left child, and right child.
- This allows for easy traversal in both directions (upwards towards the root and downwards towards the leaves).
- It’s easier to implement certain operations, such as delete or find predecessor/successor, because you can easily access the parent from a given node.
- However, it requires more memory to store the additional link to the parent.



```C++
struct Node {
    int key;
    Node* left;
    Node* right;
    Node* parent;

    Node(int key) {
        this->key = key;
        this->left = nullptr;
        this->right = nullptr;
        this->parent = nullptr;
    }
};

```



**Single-Linked Binary Tree:**

- Each node in a single-linked binary tree only has a link to its left child and right child.
- This makes the data structure more memory-efficient as it doesn’t need to store the parent link.
- However, certain operations may be more complex or inefficient. For example, finding the parent of a node or deleting a node would require additional steps or auxiliary data structures.



```C++
struct Node {
    int key;
    Node* left;
    Node* right;

    Node(int key) {
        this->key = key;
        this->left = nullptr;
        this->right = nullptr;
    }
};

```


#### Time complexity

1. **Search:** The time complexity of searching in a BST is
    
    O(h)
    
    where h is the height of the tree. This is because in the worst case, we may have to traverse from the root to the deepest leaf node.
    
2. **Insertion:** Similar to search, the time complexity of insertion is also
    
    O(h)
    
    This is because to insert a node, we need to find the appropriate location, which in the worst case could be at the deepest leaf.
    
3. **Deletion:** The time complexity of deletion is also
    
    O(h)
    
    as we need to find the node to be deleted, and in the worst case, this could be a leaf node.
    
4. **FindMin/FindMax:** These operations have a time complexity of
    
    O(h)
    
    as in the worst case, the smallest or largest value could be a leaf node.
    
5. **Successor/Predecessor:** These operations also have a time complexity of
    
    O(h)
    
    as in the worst case, we may have to traverse up to the root from a leaf node.
    

The height of the tree,

h

plays a crucial role in determining the time complexity of these operations. In a balanced BST, the height is O(logn), where n is the number of nodes. 
Therefore, all these operations are

O(logn)

in a balanced BST.

However, in the worst case (for example, when the BST is a skewed tree), the height of the tree is

n

and therefore the time complexity of these operations becomes

O(n)

This is why it’s important to keep the BST balanced to ensure that operations are efficient. Balancing is typically achieved using self-balancing BSTs like AVL trees or Red-Black trees. These trees automatically ensure that the height remains

O(logn)

after every insertion and deletion, thus guaranteeing that all the basic operations are performed in

O(logn)  time.


[[Heap]]