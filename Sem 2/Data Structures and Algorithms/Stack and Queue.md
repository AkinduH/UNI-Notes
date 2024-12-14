
# Stack

![[Pasted image 20240224195434.png]]

**LIFO( Last In First Out )**

##### Basic Operations on Stack

- **push()** to insert an element into the stack
- **pop()** to remove an element from the stack
- **top()** Returns the top element of the stack.
- **isEmpty()** returns true if stack is empty else false.
- **size()** returns the size of stack.


- **Time Complexity**

| **Operations** | **Complexity** |
| -------------- | -------------- |
| push()         | O(1)           |
| pop()          | O(1)           |
| isEmpty()      | O(1)           |
| size()         | O(1)           |

Inherent application of stack

- **Reversing a string**: Stacks can be used to reverse a string.

- **Undo/Redo Functionality:** Stacks are used to track actions and enable undoing/redoing them.

- **Evaluation of postfix expression**: Stacks are commonly used in the evaluation of expressions in postfix (or Reverse Polish Notation) form. You scan the expression from left to right, pushing operands onto the stack until you encounter an operator, at which point you pop the appropriate number of operands from the stack, perform the operation, and push the result back onto the stack. At the end of this process, the stack will contain the final result.

- **Recursive implementation**: Stacks are inherently used in recursion. When a recursive call is made, the current function execution is paused and its state (including local variables, the return address, etc.) is pushed onto a stack. When the recursive call finishes, the state is popped from the stack and the function resumes execution.

- **Browser History:** Web browsers use a stack to keep track of the pages you visit, allowing you to go back to previous pages.

stack overflow is an error that occurs when a program tries to use more space on the call stack than has been allocated for it.
Stack overflow typically happens in one of two scenarios:
1. **Infinite Recursion
2. **Very Deep Recursion or Large Function Calls


# Queue

![[Pasted image 20240515113914.png]]

**FIFO (First In, First Out)

- **enqueue()** to insert an element into the Queue
- **dequeue()** to remove an element from the Queue
- **front()** Returns the top element of the Queue.
- **isEmpty()** returns true if Queue is empty else false.
- **size()** returns the size of Queue.

- **Time Complexity**

| **Operations** | **Complexity** |
| -------------- | -------------- |
| enqueue()      | O(1)           |
| dequeue()      | O(1)           |
| isEmpty()      | O(1)           |
| size()         | O(1)           |

Inherent application of Queue

- **Waiting Lines/Queues:** Queues naturally model scenarios where items need to be processed in the order they arrived, such as print queues, task queues, or customer service lines.
- **Breadth-First Search (BFS):** Queues are fundamental to the BFS algorithm for graph traversal, exploring nodes in a level-by-level manner.
- **Asynchronous Data Transfer:** Queues are used in asynchronous systems to buffer data between components operating at different speeds.
- **Simulation:** Queues can simulate real-world scenarios like traffic flow or customer arrivals at a store.

In a regular queue implemented with a fixed-size array, when the rear of the queue reaches the end of the array, no more elements can be added even if there is free space at the beginning of the array due to dequeued elements. This situation is called the "false overflow" or "memory wastage".

A circular queue addresses this issue by connecting the end of the array back to the beginning, forming a circle. 

![[Pasted image 20240730171739.png]]



###### **Priority Queue**

- **insert()** to add an element with an associated priority into the Priority Queue.
    
- **remove()** to remove the element with the highest priority from the Priority Queue.
    
- **peek()** Returns the element with the highest priority without removing it from the Priority Queue.
    
- **isEmpty()** returns true if the Priority Queue is empty, otherwise false.
    
- **size()** returns the number of elements in the Priority Queue.
    
- **Time Complexity**
    

| **Operations** | **Complexity**                |
| -------------- | ----------------------------- |
| insert()       | O(log n) (with a binary heap) |
| remove()       | O(log n) (with a binary heap) |
| peek()         | O(1)                          |
| isEmpty()      | O(1)                          |
| size()         | O(1)                          |

Implementation Details:

- Priority queues are typically implemented using binary heaps (min-heap or max-heap) to ensure efficient priority-based operations. Other implementations might include balanced binary search trees or unsorted/sorted arrays, but heaps are most common due to their balance of simplicity and efficiency.

 Usage Scenarios:

- Scheduling tasks in operating systems where processes with higher priorities need to be executed first.
- Network routing algorithms like Dijkstra's algorithm for finding the shortest path.
- Managing events in simulations where certain events need to be processed before others based on their priority.


[[Tree Data Structure]]