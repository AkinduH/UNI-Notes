### The Mutual Exclusion Problem

The **mutual exclusion problem** arises when multiple processes or threads attempt to access a **critical section** of code at the same time, which can cause undesirable interleaving of operations. The goal is to ensure that **only one process** enters the critical section at any time.

### Solution Structure:
- **Pre-protocol**: A set of operations a process must perform **before** entering the critical section to ensure mutual exclusion.
- **Post-protocol**: A set of operations a process must perform **after** leaving the critical section to release the lock for other processes.

### Key Requirements for N Processes:
1. **Mutual Exclusion**: No two processes should be in the critical section simultaneously.
2. **No Deadlock**: If one or more processes wish to enter the critical section, at least one should eventually succeed.
3. **No Starvation**: Every process that wishes to enter the critical section will eventually do so.
4. **Fairness**: In the absence of contention, a process should be able to enter its critical section without unnecessary delays.

### Dekker’s Algorithm

**Dekker’s Algorithm** was one of the first mutual exclusion algorithms designed to solve the mutual exclusion problem for **two processes**. It avoids the simple turn-taking approach by allowing more dynamic control over the critical section. The algorithm ensures that:
- **Mutual exclusion** is maintained.
- **No deadlock** occurs.
- **No starvation** takes place.

The algorithm is designed for two processes, `P1` and `P2`, which take turns backing off if both attempt to enter the critical section simultaneously. If one process is already in the critical section, the other waits until it finishes.

### Key Concepts of Dekker’s Algorithm:
1. **Flags** (`c1`, `c2`): Each process has a flag indicating its desire to enter the critical section.
   - `c1 = 0`: Process `P1` does not want to enter the critical section.
   - `c1 = 1`: Process `P1` wants to enter the critical section.
2. **Turn**: A shared variable that indicates whose turn it is to enter the critical section. This avoids strict alternation and provides fairness when both processes want to enter at the same time.
3. **Critical Section**: If both processes try to access the critical section at the same time, the algorithm gives priority to the process whose turn it is. The other process must wait for the first process to finish before entering.

### Dekker’s Algorithm - Steps
1. **Each process sets its flag** when it wants to enter the critical section.
2. **If the other process's flag is also set**, the process checks whose turn it is.
3. **If it’s the other process’s turn**, the current process relinquishes its flag and waits.
4. **When it’s the process’s turn**, it enters the critical section and sets the turn for the other process when it exits.

### Dekker’s Algorithm Code Example

```c
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

// Shared variables
volatile int c1 = 1, c2 = 1;  // Flags for processes P1 and P2
volatile int turn = 1;        // Shared turn variable

void *process_1(void *arg) {
    while (1) {
        // Non-critical section
        printf("P1: Non-critical section\n");
        sleep(1);

        // Pre-protocol: P1 wants to enter the critical section
        c1 = 0;  // P1 signals its desire to enter critical section
        
        // If P2 wants to enter and it's P2's turn, wait
        while (c2 == 0) {
            if (turn == 2) {
                c1 = 1;  // P1 relinquishes its flag
                while (turn != 1) {
                    // Busy waiting until it is P1's turn
                }
                c1 = 0;  // Reattempt entering critical section
            }
        }

        // Critical section
        printf("P1: Critical section\n");
        sleep(2);

        // Post-protocol: P1 leaves the critical section
        turn = 2;  // Set turn to P2
        c1 = 1;    // P1 no longer wants the critical section
    }
}

void *process_2(void *arg) {
    while (1) {
        // Non-critical section
        printf("P2: Non-critical section\n");
        sleep(1);

        // Pre-protocol: P2 wants to enter the critical section
        c2 = 0;  // P2 signals its desire to enter critical section
        
        // If P1 wants to enter and it's P1's turn, wait
        while (c1 == 0) {
            if (turn == 1) {
                c2 = 1;  // P2 relinquishes its flag
                while (turn != 2) {
                    // Busy waiting until it is P2's turn
                }
                c2 = 0;  // Reattempt entering critical section
            }
        }

        // Critical section
        printf("P2: Critical section\n");
        sleep(2);

        // Post-protocol: P2 leaves the critical section
        turn = 1;  // Set turn to P1
        c2 = 1;    // P2 no longer wants the critical section
    }
}

int main() {
    pthread_t t1, t2;

    // Create two threads representing P1 and P2
    pthread_create(&t1, NULL, process_1, NULL);
    pthread_create(&t2, NULL, process_2, NULL);

    // Join the threads (not necessary in an infinite loop, but for completeness)
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}
```

### Explanation of the Code:
1. **Flags (`c1`, `c2`)**: Indicate whether a process wants to enter the critical section.
   - `c1 = 0`: Process 1 wants to enter the critical section.
   - `c2 = 0`: Process 2 wants to enter the critical section.
2. **Turn**: Determines which process gets priority when both want to enter the critical section simultaneously.
3. **Pre-protocol**: Each process checks the flag of the other process and the `turn` variable to decide whether to wait or proceed.
4. **Critical Section**: The process enters its critical section if it passes the checks.
5. **Post-protocol**: After leaving the critical section, the process sets the `turn` variable for the other process to allow it to enter next.

### Why Dekker’s Algorithm is Important:
- It was one of the earliest solutions to the mutual exclusion problem.
- It avoids **strict alternation**, giving both processes fair access to the critical section.
- The algorithm ensures **mutual exclusion**, **no deadlock**, and **no starvation**.

Though useful in demonstrating concurrency principles, **Dekker’s algorithm** is not commonly used in modern systems due to advancements like atomic operations and hardware-level synchronization techniques.

![[Pasted image 20241204140237.png]]


![[Pasted image 20241204140319.png]]