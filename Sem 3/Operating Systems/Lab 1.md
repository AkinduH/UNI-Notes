

```1
What is the name of the function that initializes all relevant modules in Pintos OS?
(Enter only the function name without the return type or input parameters)

main
```


```2
How much of memory does Pintos allocate for the kernel? State as a fraction of whole memory.

0.00781541
```


```3
If the 10th flag (not zero indexed) is up in the interrupt register, what type of interrupt has occurred?

Coprocessor Segment Overrun
```


```4
If the console was initialized before the thread system, will the outcome change? Briefly explain your answer.

Yes. Console must require the thread system to be active because when one thread is writing something to the console, the other threads must not write anything before the first thread has finished writing. This requires some method of synchronization, thus it requires the thread system to be initialized before the console is initialized.
```


```5
What is the diftefence between a lock and a semaphore?

Only the owner of a lock may release it. But any thread can up or down a semaphore. This is the main difference.
```



```7
If you already know a list is in order, can you improve the function you used above to reduce its runtime? Briefly explain your answer.

No. I already took the fact that the list is in order. into consideration.
However we implement it, we must always find the correct position of the element to be added, which takes an average of o(n) time. Binary search cannot be used since it is a linked list. This cannot be further optimized.
```



```8
In the current version, how has Pintos OS implemented thread sleeping? What is the disadvantage of this approach?

Currently pintos OS uses busy waiting to implement sleep. This means that the thread calls yield repeatedly in a tight loop without giving up the processor.
The disadvantage of this approach is that during that sleep time, no other threads can use the processor, and no effective work in done in that sleep time.
```


```9
What is the purpose of the magic value used in the thread struct?

The magic value is located in the boundary between the thread stack and the thread struct. The magic value is initialized to an arbitrary value which is THREAD_MAGIC. When the thread stack grows too large and stack overflow occurs, the thread passes the boundary and the magic value is changed. That means we can use the magic value to detect stack overflow.

Summary : Magic value is used to detect stack overflow of a thread stack.
```


```10
What happens if you turn off interrupts indefinitely? Are there processes that should only be executed after
turning off interruption? If so, what are they and why?

If we turn off interrupts indefinitely, there is no concurrency in the system. Which means that a thread cannot be preempted. This can cause inefficiencies in the system design and also if the thread that disables interrupts returns without turning it back on, all interrupts including keyboard and timer interrupts will be neglected which is a big problem.

Yes. There are processes that should only be executed after turning off interrupts. One example is working with data shared between kernel threads and interrupts handlers. Since interrupt handlers cannot sleep they can't acquire locks. So any data shared between them must be protected inside the kernel thread by turning off all interrupts because otherwise the timer interrupt can go off and the scheduler may activate another thread at any time.
```


```
1.What is the primary goal of implementing the alarm clock in Pintos?
To replace busy waiting with a sleep/wakeup mechanism

2.In the original Pintos implementation, how does timer_sleep() function?
It uses busy waiting, continuously yielding the CPU

3.Which data structure is introduced to manage sleeping threads in the alarm clock implementation?
Sleep queue

4.What is the purpose of the global tick in the alarm clock implementation?
To store the minimum wake-up time of all sleeping threads

5.In the priority scheduling implementation, when does preemption occur?
When a higher priority thread is added to the ready list

6.What is the range of priorities in Pintos?
0 to 63

7.Which function is responsible for changing the priority of the current thread?
thread_set_priority()

8.What happens when thread_yield() is called in the priority scheduling implementation?
The thread is inserted into the ready list based on its priority

9.Which synchronization primitives need to be modified to support priority scheduling?
Semaphores and condition variables

10.What is priority inversion?
When lower priority threads prevent higher priority threads from running

11.How does priority donation address the priority inversion problem?
By temporarily boosting the priority of threads holding needed resources

12.What is nested donation in the context of priority donation?
When priority donation occurs through a chain of threads holding different locks

13.Which function needs to be modified to initialize the data structures for priority donation?
init_thread()

14.In the implementation of priority donation, what happens when a lock is released?
The thread holding the lock is removed from the donation list and its priority is recalculated

15.Which Pintos function is the heart of the system and crucial for implementing the alarm clock?
timer_interrupt()
```

```
1.Explain the differences between the original `timer_sleep()` implementation in Pintos and the improved version using a sleep queue. Discuss the advantages of the new implementation.

Answer: The original `timer_sleep()` implementation in Pintos used busy waiting, where the thread continuously yielded the CPU until the specified time had elapsed. This was inefficient as it wasted CPU cycles.

The improved version uses a sleep queue and follows these steps:

1. When a thread calls `timer_sleep()`, it is added to a sleep queue with its wake-up time.
2. The thread's state is changed to `BLOCKED`, and it's removed from the ready list.
3. The `timer_interrupt()` function checks the sleep queue at each tick and wakes up any threads whose time has come.
4. Awakened threads are moved back to the ready list.

Advantages of the new implementation:

- More efficient use of CPU resources, as sleeping threads don't consume CPU cycles.
- Better scalability, especially with many threads or long sleep times.
- More predictable behavior, as it doesn't rely on the scheduler to yield the CPU.
- Potentially lower power consumption, as the CPU can enter low-power states when all threads are sleeping.

---

2. Describe the process of implementing priority scheduling in Pintos. What are the key components that need to be modified, and why?

Answer: Implementing priority scheduling in Pintos involves several key modifications:

Key components to modify:

1. Ready List Ordering: The ready list must be kept sorted by priority. This involves modifying functions like `thread_unblock()` and `thread_yield()` to use `list_insert_ordered()` instead of `list_push_back()`.
2. Preemption: When a higher priority thread becomes ready (e.g., after creation or unblocking), the current thread should be preempted if it has lower priority. This requires changes in `thread_create()` and `thread_unblock()`.
3. Scheduler Modification: The scheduler should always choose the highest priority thread from the ready list.
4. Synchronization Primitives: Semaphores and condition variables need to be modified to wake up waiting threads based on priority rather than FIFO order. This involves changes to `sema_up()`, `cond_signal()`, and related functions.
5. Priority Change Handling: When a thread's priority changes (`thread_set_priority()`), the ready list may need to be reordered, and preemption may be necessary.

These modifications ensure that at any given time, the highest priority ready thread is running, maintaining the core principle of priority scheduling.

---

3. Explain the concept of priority donation in Pintos. Why is it necessary, and how does it solve the priority inversion problem?

Answer: Priority donation is a mechanism implemented in Pintos to solve the priority inversion problem. Priority inversion occurs when a high-priority thread is indirectly preempted by a low-priority thread.

Consider this scenario:

1. Low-priority thread L acquires a lock.
2. High-priority thread H tries to acquire the same lock and is blocked.
3. Medium-priority thread M preempts L (because L has low priority).
4. H is indirectly blocked by M, even though H has higher priority.

Priority donation solves this** by temporarily boosting the priority of L to match H's priority when H is blocked waiting for the lock held by L. This prevents M from preempting L, allowing L to finish its critical section and release the lock, so H can proceed.

Implementation involves:

1. Tracking donations: Each thread maintains a list of donations it has received.
2. Nested donation: If L is itself blocked on another lock, the donation propagates.
3. Multiple donations: A thread may receive donations from multiple sources; it should always use the highest donated priority.
4. Priority recalculation: When locks are released, priorities need to be recalculated.

Priority donation is necessary** because it ensures that high-priority threads are not indefinitely blocked by lower-priority threads, maintaining the system's responsiveness and adhering to the principles of priority scheduling.

---

4. Describe the changes needed in the thread structure and the `lock_acquire()` and `lock_release()` functions to implement priority donation in Pintos.

Answer: To implement priority donation in Pintos, several changes are needed in the thread structure and lock-related functions:

Thread Structure Changes:

1. Ready List Ordering: The ready list must be kept sorted by priority. This involves modifying functions like `thread_unblock()` and `thread_yield()` to use `list_insert_ordered()` instead of `list_push_back()`.
2. Preemption: When a higher priority thread becomes ready (e.g., after creation or unblocking), the current thread should be preempted if it has lower priority. This requires changes in `thread_create()` and `thread_unblock()`.
3. Scheduler Modification: The scheduler should always choose the highest priority thread from the ready list.
4. Synchronization Primitives: Semaphores and condition variables need to be modified to wake up waiting threads based on priority rather than FIFO order. This involves changes to `sema_up()`, `cond_signal()`, and related functions.
5. Priority Change Handling: When a thread's priority changes (`thread_set_priority()`), the ready list may need to be reordered, and preemption may be necessary.

Changes in `lock_acquire()`: a. Update the current thread's `wait_on_lock`. b. Add the current thread to the lock holder's donation list. c. Implement nested donation by traversing the chain of lock holders. d. Update priorities along the chain.

Changes in `lock_release()`:

1. Remove the lock from the thread's held locks list.
2. Remove any donations associated with this lock.
3. Recalculate the thread's effective priority based on remaining donations.
4. If the thread's priority has changed, yield the CPU to ensure the highest priority thread runs.
5. Disable interrupts during these operations.

---

5. How would you test the implementation of the alarm clock and priority scheduling in Pintos? Describe some key test cases and what they would verify.

Answer: Testing the alarm clock and priority scheduling implementations in Pintos should cover various scenarios to ensure correctness and efficiency. Here are some key test cases:

Alarm Clock Tests:

1. Basic Functionality:
    - Test `timer_sleep()` with various durations.
    - Verify that threads wake up at the correct time.
2. Multiple Sleepers:
    - Create multiple threads with different sleep durations.
    - Ensure they all wake up at the correct times.
3. Stress Test:
    - Create a large number of threads with random sleep durations.
    - Verify system stability and correct wake-up times.
4. Edge Cases:
    - Test `timer_sleep(0)` to ensure it doesn't block the thread.
    - Verify that interrupts are handled correctly during sleep.

Priority Scheduling Tests:

1. Basic Priority Scheduling:
    - Create threads with different priorities.
    - Verify that higher priority threads run before lower priority ones.
2. Priority Change:
    - Change thread priorities dynamically.
    - Ensure the schedule updates accordingly.
3. Priority Inversion:
    - Set up a priority inversion scenario.
    - Verify that priority donation resolves the issue.
4. Nested Donation:
    - Create a chain of priority donations through multiple locks.
    - Ensure priorities are propagated correctly.
5. Multiple Donations:
    - Have multiple high-priority threads donate to a single low-priority thread.
    - Verify that the highest priority is used.
6. Synchronization Primitives:
    - Test semaphores and condition variables with threads of different priorities.
    - Ensure that they wake up threads in priority order.
7. Starvation Prevention:
    - Verify that low-priority threads eventually get to run.
8. Stress Test:
    - Create many threads with frequent priority changes and lock operations.
    - Ensure system stability and correct scheduling.

General Verification Points:

- Correctness: Ensure that all operations produce the expected results.
- Timing: Verify that operations complete within acceptable time frames.
- Resource Usage: Monitor CPU and memory usage to ensure efficiency.
- Edge Cases: Test boundary conditions and unusual scenarios.
- Concurrency: Ensure thread-safety and proper handling of race conditions.
```