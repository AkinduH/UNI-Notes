
Concurrency is when two tasks can start, run, and complete in 
overlapping time periods. It doesn't necessarily mean they'll ever 
both be running at the same instant.

#### Advantages of Concurrency

1. **Utilize Multiple Hardware Processors**: Concurrency allows a program to distribute its tasks across multiple processors, improving performance and efficiency.
    
2. **Hide Latency**: By executing multiple tasks concurrently, a system can continue processing while waiting for responses from external resources, effectively hiding latency.
    
3. **Execute Long-Running Tasks Together**: Concurrency enables the simultaneous execution of multiple long-running tasks, particularly when these tasks are waiting on external resources like disk or network I/O operations.
    
4. **Keep the GUI Responsive**: In graphical user interfaces, separating the “worker” thread from the GUI thread can keep the interface responsive even when the system is processing heavy tasks.


**Two basic units of execution in concurrent programming:**
• Processes
• Threads

- **Process**: A process is an instance of a program that is being executed. It contains the program code and its current activity. Each process has a separate memory address space, which means that a process runs independently and is isolated from other processes. It cannot directly access shared data in other processes. Switching from one process to another requires some time for saving and loading registers, memory maps, and other administrative information.
    
- **Thread**: A thread is a unit of execution within a process. A process can have anywhere from just one thread to many threads. When a process has multiple threads, one thread takes over the CPU while others are waiting their turn. They share the same memory space and can communicate with each other more easily than if they were separate processes. Threads are sometimes called lightweight processes and they do not require much overhead to create and manage.

==**Two strategies to use threads to achieve concurrency in a Java**== 
==**application**==

- **Direct Control of Threads**: 
To control the execution of threads directly, you instantiate a `Thread` each time the application needs to initiate an asynchronous task

```java
class MyRunnable extends Thread {
    public void run(){
        // Code to execute in new thread
    }
}
MyRunnable t = new MyRunnable();
t.start();
```

- **Abstract Thread Management**:
Java provides a higher-level replacement for working with threads directly called the executor framework. This framework consists of the `Executor` interface, the `ExecutorService` subinterface, thread pools, `Callable`, and `Future`. Executors are capable of managing a pool of threads, so you do not have to manually create new threads. Instead, you can just pass the tasks to the executor.

```java
import java.util.concurrent.ExecutorService;  
import java.util.concurrent.Executors;

ExecutorService executor = Executors.newFixedThreadPool(10);
executor.execute(new MyRunnable());
executor.execute(new MyRunnable());
...
```


### Simple Multi-Threaded Java Application

• Extend the Thread class.

```java
public class HelloThread extends Thread { 
    public void run() { 
        for (int i = 0; i < 10; i++) {
            System.out.println("Hello thread!");
        } 
    } 
}

public class Main {
    public static void main(String args[]) {
        Thread myThread = new HelloThread();
                       or
        HelloThread myThread = new HelloThread();
        
        myThread.start();
    }
}
```

• Using Runnable interface.

```java
public class HelloRunnable implements Runnable { 
    public void run() { 
        System.out.println("Hello from a thread!"); 
    } 
}

public class Main {
    public static void main(String args[]) {
        Thread myThread = new Thread(new HelloRunnable());
        myThread.start();
        
    }
}
```


• Using Callable interface.

In Java, both `Runnable` and `Callable` interfaces are designed to represent tasks that can be executed concurrently, or in parallel, by multiple threads. However, they have some key differences:

1. **Return Value**: The `Runnable` interface’s `run()` method does not return a result—it has a `void` return type. 
    On the other hand, the `Callable` interface’s `call()` method does return a result. The type of this result can be any class or interface, and is specified as a generic parameter when you declare a `Callable`.

2. **Exception Handling**: The `Runnable.run()` method cannot throw any checked exceptions—only unchecked exceptions. 
    The `Callable.call()` method, however, is allowed to throw checked exceptions. So, if the task performed in the `call()` method can throw checked exceptions, these exceptions can be thrown from the `call()` method and caught and handled in the part of your program where you started the task.



![[Pasted image 20240408164537.png]]

A thread is alive unless its run method terminates (or the stop 
method has been called)

- **New**: The thread is created but not yet started.
- **Runnable**: The thread is ready to run and waiting for the thread scheduler to pick it for execution.
- **Blocked**: The thread is blocked waiting for a monitor lock to enter a synchronized block/method or reenter a synchronized block/method after calling `Object.wait()`.
- **Waiting**: The thread is waiting indefinitely for another thread to perform a particular action, typically by calling `Object.wait()` without a timeout.
- **Timed Waiting**: The thread is waiting for another thread to perform an action for up to a specified waiting time, for example, calling `Thread.sleep()` or `Object.wait()` with a timeout.
- **Terminated**: The thread has completed execution and exits.
- **yield():**  This used to hint the thread scheduler that the current thread is ready to pause its execution, allowing other threads of the same or higher priority to execute. Calling yield() doesn’t guarantee that the current thread will stop executing, and we can’t specify when it will get a chance to execute again


> [! ]
> ***The thread class provides many facilities required to create a*** 
> ***class that can execute as a lightweight process.***
> 
> • start() - to schedule a thread for execution.
> • isAlive() - to test if a thread that has started execution has terminated.
> • isInterrupted – to test if a thread has been interrupted.
> • sleep(long milliseconds) – to suspend a thread for the specified duration.
> • yield() - a thread is willing to relinquish the processor.
> • setName(String name) – to set the name for thread.
> • getName() - to obtain the name of the thread.


- **Waking up a sleeping thread**:
    - **Sleep time expires**: moves from the blocked state back to the runnable state.
    - **Interrupt**: Another thread can interrupt a sleeping thread by calling `interrupt()` on the `Thread` object. This causes the sleeping thread to immediately throw an `InterruptedException` and prematurely exit the sleeping state.

**When a thread can be interrupted**: 
A thread can be interrupted at any point in its execution by another thread calling `interrupt()` on the `Thread` object. This is particularly useful when a thread is in a waiting or sleeping state and you want it to exit that state early.


```java
class SleepyThread extends Thread {
    public void run() {
        try {
            System.out.println("SleepyThread is going to sleep...");
            Thread.sleep(Long.MAX_VALUE); // Sleep indefinitely
        } catch (InterruptedException e) {
            System.out.println(e.getMessage());
        }
    }
}

public class Main {
    public static void main(String[] args) throws InterruptedException {
        Thread sleepyThread = new SleepyThread();
        sleepyThread.start(); // Start the SleepyThread

        Thread.sleep(1000); // Main thread sleeps for 1 second

        System.out.println("Main thread is interrupting SleepyThread...");
        sleepyThread.interrupt(); // Main thread interrupts SleepyThread
    }
}
```

### Join() Method

- The `join()` method allows one thread (the calling thread) to wait for the completion of another thread (the target thread).
- If `t` is a Thread object whose thread is currently executing, `t.join()` causes the calling thread to pause execution until `t`’s thread terminates.
- The programmer can specify a waiting period for the `join()` method. This means the calling thread will wait for the specified time for the target thread to terminate. If the target thread doesn’t terminate within this time, the calling thread resumes execution.
- The `join()` method responds to an interrupt by exiting with an `InterruptedException`. This means if any thread interrupts the calling thread while it’s waiting for the target thread to terminate, the `join()` method will throw an `InterruptedException`.


### **Daemon Threads**

- Daemon threads are a type of thread that provide services to user threads for background supporting tasks.
- An example of a daemon thread is the Java garbage collector, which automatically manages memory allocation and deallocation for objects.
- Daemon threads have no other role in life than to serve user threads. They exist solely to provide services to user threads
- Daemon threads are low priority threads by default. This means that the JVM will typically allocate CPU resources to user threads before daemon threads.

```java
t1.setDaemon(true);
```


### **Threads Priorities**

- Every thread in Java has a priority. Priorities are represented by an integer between 1 and 10, with 1 being the lowest priority and 10 being the highest.
- Threads with higher priority are executed in preference to threads with lower priority. This is not a strict rule, but rather a hint to the thread scheduler in the JVM.
- When a thread (let’s call it Thread A) creates a new Thread object (Thread B), Thread B’s priority is initially set equal to the priority of Thread A. This is the default behavior, but it can be changed using the `setPriority()` method.
- A new thread is a daemon thread if and only if the creating thread is a daemon. This means that if Thread A is a daemon thread and it creates Thread B, then Thread B will also be a daemon thread.
- Java provides three constants to represent thread priority:
    - `Thread.MIN_PRIORITY`: This is the minimum priority that a thread can have. The value is 1.
    - `Thread.NORM_PRIORITY`: This is the default priority that is assigned to a thread. The value is 5.
    - `Thread.MAX_PRIORITY`: This is the maximum priority that a thread can have. The value is 10.
- The `setPriority(int newPriority)` method in the Thread class is used to change the priority of a thread. The `newPriority` argument is the new priority setting for the thread.

[[Multithreading Issues and  Solutions]]