##### Why linked list data structure needed?

- ***Dynamic Data structure:*** The size of memory can be allocated or de-allocated at run time based on the operation insertion or deletion.
- ***Ease of Insertion/Deletion:*** The insertion and deletion of elements are simpler than arrays since no elements need to be shifted after insertion and deletion, Just the address needed to be updated.

1. Single-linked list

![[Pasted image 20240224161637.png]]


2. Double linked list

![[Pasted image 20240224161703.png]]


3. Circular linked list

![[Pasted image 20240224161728.png]]


| Operation                   | Time Complexity (Singly Linked List) | Time Complexity (Doubly Linked List) | Space Complexity |
| --------------------------- | ------------------------------------ | ------------------------------------ | ---------------- |
| Accessing by Index          | O(n)                                 | O(n)                                 | O(1)             |
| Insertion at Beginning      | O(1)                                 | O(1)                                 | O(1)             |
| Insertion at End            | O(n)                                 | O(1)                                 | O(1)             |
| Insertion at Given Position | O(n)                                 | O(n)                                 | O(1)             |
| Deletion at Beginning       | O(1)                                 | O(1)                                 | O(1)             |
| Deletion at End             | O(n)                                 | O(1)                                 | O(1)             |
| Deletion at Given Position  | O(n)                                 | O(n)                                 | O(1)             |
| Searching                   | O(n)                                 | O(n)                                 | O(1)             |

## Advantages of Linked Lists

- **Dynamic Size:** Linked lists can grow or shrink dynamically, as memory allocation is done at runtime.
- **Insertion and Deletion:** Adding or removing elements from a linked list is efficient, especially for large lists.

## Disadvantages of Linked Lists

- **Random Access:** Unlike arrays, linked lists do not allow direct access to elements by index. Traversal is required to reach a specific node.
- **Extra Memory:** Linked lists require additional memory for storing the pointers, compared to arrays.

![[Pasted image 20240224170503.png]]


## Linked list implementation on c++

```c++
#include <iostream>

struct Node {
    int data;
    Node* next;
};

class LinkedList {
    private:
        Node* head;
        Node* tail;
    public:
        LinkedList() {
            head = NULL;
            tail = NULL;
        }
        
        void addNode(int n) {
            Node* tmp = new Node;
            tmp->data = n;
            tmp->next = NULL;

            if(head == NULL) {
                head = tmp;
                tail = tmp;
            } else {
                tail->next = tmp;
                tail = tail->next;
            }
        }
        
        void display() {
            Node* tmp;
            tmp = head;
            while (tmp != NULL) {
                std::cout << tmp->data << " ";
                tmp = tmp->next;
            }
        }
};

int main() {
    LinkedList list;
    list.addNode(1);
    list.addNode(2);
    list.addNode(3);
    list.display();
    return 0;
}
```


[[Stack and Queue]]