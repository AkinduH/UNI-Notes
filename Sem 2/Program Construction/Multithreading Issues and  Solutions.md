
## Multithreading Issues 

1. **Thread Interference**: Also known as race conditions, this occurs when multiple threads access and modify shared data concurrently. The final value of the shared data depends on how the thread scheduling algorithm interleaves the threads, leading to non-deterministic and incorrect results.
    
2. **Memory Consistency Errors**: These occur when different threads have inconsistent views of what should be the same data. The root of memory consistency errors lies in the fact that many modern computer systems allow a great deal of flexibility in when and how changes in values stored in memory are visible to other threads.


### Happens-Before Relationship

The **happens-before relationship** is a key concept in concurrent programming that helps us avoid memory consistency errors. It’s a way to ensure that certain actions are completed before others, providing a guarantee about the relative ordering of operations in different threads.

There are several operations that enforce happens before 
relationship.
• Thread.start()
• Thread.join()
• Object initialization
• Actions in the same thread
• Synchronization

### Synchronization

#### Synchronized Methods

```java
public class SynchronizedMethodExample {
    private int count = 0;
    
    public synchronized void increment() {
        count++;
    }
}
```

**No Interleaving**: 
This means that if one thread is executing a synchronized method, all other threads that invoke synchronized methods on the same object will block (or suspend execution) until the first thread is done with the object.

**Happens-Before Relationship**


#### Synchronized Statements

``` java
public class SynchronizedStatementExample {
    private int count = 0;
    private final Object lock = new Object();

    public void increment() {
        synchronized(lock) {
            count++;
        }
    }
}
```

1. **Fine-Grained Synchronization**: Synchronized statements provide more control than synchronized methods because they allow you to lock only the critical section of code, not the entire method. This can lead to better performance by reducing contention.
2. Reentrant synchronization.
		Thread cannot acquire a lock owned by another thread.
		But a thread can acquire a lock that it already owns

```java
public void addName(String name) { 
    synchronized(this) {
        lastName = name;  // Set the last name to the new name
        nameCount++;      // Increment the count of names
    }
    nameList.add(name);   // Add the new name to the list of names
}
```

Unlike synchronized methods, synchronized statements must specify the object that provides the intrinsic lock.

 **Intrinsic Locks**

1. **Monitor Locks or Monitors**: Intrinsic locks are also known as monitor locks or monitors. They are a synchronization mechanism built into every object and class in Java.
2. **Exclusive Access**: Threads can acquire the intrinsic lock for an object by entering a synchronized block of code. If a thread already holds the intrinsic lock for an object, no other thread can enter a synchronized block of code for the same object.
3. **Automatic Acquisition**: When a thread invokes a synchronized method, it automatically acquires the intrinsic lock for that method’s object and releases it when the method returns.


### **Liveness**
This refers to an application’s ability to execute in a timely manner.

*Liveness problems:*

### **Deadlock**: 
This is a state in which two or more threads are blocked forever, each waiting for the other to release a lock. For example, if Thread A locks resource 1 and tries to lock resource 2, and concurrently Thread B locks resource 2 and tries to lock resource 1, neither thread can proceed, causing a deadlock.

```java
public class BankAcc {
    private double balance;
    private int id;
    
    public BankAcc(int id, double initialBalance) {
        this.id = id;
        this.balance = initialBalance;
    }
    
    public void withdraw(double amount) {
        this.balance -= amount;
    }
    
    public void deposit(double amount) {
        this.balance += amount;
    }
    
    public static void transfer(BankAcc from, BankAcc to, double amount) {
        synchronized(from) {
            from.withdraw(amount);
            synchronized(to) {
                to.deposit(amount);
            }
        }
    }
    
    public static void main(String[] args) {
        final BankAcc fooAcc = new BankAcc(1, 100);
        final BankAcc barAcc = new BankAcc(2, 100);
        
        Thread t1 = new Thread() {
            public void run() {
                BankAcc.transfer(fooAcc, barAcc, 10);
            }
        };
        t1.start();
        
        Thread t2 = new Thread() {
            public void run() {
                BankAcc.transfer(barAcc, fooAcc, 10);
            }
        };
        t2.start();
    }
}
```
 
 This code can potentially cause a deadlock if both threads try to execute the `transfer` method at the same time, each having locked one account and waiting for the other to release its lock.

#### Solution 

```java
---------------------------------------------------------------------
static synchronized void transfer(BankAcc from, BankAcc to, double amount) {
    from.withdraw(amount);
    to.deposit(amount);
}

---------------------------------------------------------------------

static void transfer(BankAcc from, BankAcc to, double amount) {
BankAcc first = (from.id < to.id) ? from : to;
BankAcc second = (from.id < to.id) ? to : from;

synchronized(first) {
    synchronized(second) {
        from.withdraw(amount);
        to.deposit(amount);
    }
}
---------------------------------------------------------------------
```

### Starvation:

1. **Starvation**: This occurs when a thread is perpetually denied access to resources it needs in order to make progress. The thread doesn’t terminate, but it doesn’t make any progress either.
    
2. **Greedy Threads**: Starvation often happens when shared resources are made unavailable for long periods by “greedy” threads. These are threads that, once they gain access to shared resources, hold onto them for an extended time, preventing other threads from accessing these resources.
    
3. **Low-Priority Threads**: In many systems, threads have priorities, and higher-priority threads are given preference over lower-priority threads. If a high-priority thread becomes greedy, it can cause starvation for low-priority threads.
    
4. **Synchronization and Starvation**: Synchronization mechanisms like locks can lead to starvation. For example, if a thread holding a lock doesn’t release it, other threads waiting for the lock experience starvation.

```java
public class StarvationExample {
    public static void main(String[] args) throws InterruptedException {
        Thread highPriorityThread = new Thread(new WorkerThread(), "highPriorityThread");
        Thread mediumPriorityThread = new Thread(new WorkerThread(), "mediumPriorityThread");
        Thread lowPriorityThread = new Thread(new WorkerThread(), "lowPriorityThread");

        highPriorityThread.setPriority(Thread.MAX_PRIORITY);
        mediumPriorityThread.setPriority(Thread.NORM_PRIORITY);
        lowPriorityThread.setPriority(Thread.MIN_PRIORITY);

        lowPriorityThread.start();
        mediumPriorityThread.start();
        highPriorityThread.start();


        Thread.sleep(1000); // Let the threads run for 5 seconds

        highPriorityThread.interrupt();
        mediumPriorityThread.interrupt();
        lowPriorityThread.interrupt();
    }

    static class WorkerThread implements Runnable {
        private int count = 0;

        @Override
        public void run() {
            while (!Thread.currentThread().isInterrupted()) {
                count++;
            }
            System.out.println(Thread.currentThread().getName() + ": " + count);
        }
    }
}
```

```
highPriorityThread: 285034723
mediumPriorityThread: 260427362
lowPriorityThread: 232327146
```
### **Livelock**:

1. **Livelock**: This is a situation where two or more threads are technically not blocked — they’re actually busy responding to each other — but there’s no useful work being done. This is similar to a deadlock in that it’s a state where progress is impossible, but different in that the threads are not blocked, they’re simply too busy responding to each other to resume work.
    
2. **Example**: A real-world example would be two people who meet in a narrow corridor, and each tries to step aside to let the other pass, but they end up swaying from side to side without making any progress because they always move in the same direction at the same time.
    
3. **Solution**: One common way to prevent livelock is to introduce randomness into your algorithm. For example, when two threads need to back off and retry an operation, having them wait for a random amount of time can reduce the likelihood that they’ll become livelocked.


```java
public class LivelockExample {
    static class Resource {
        private boolean inUse = false;

        public synchronized void use() {
            while (inUse) {
                // Busy-waiting to simulate response to another thread
                try {
                    Thread.sleep(1); 
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
            inUse = true;
            // Simulate work
            try {
                Thread.sleep(50); 
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
            inUse = false;
        }
    }

    static class Worker extends Thread {
        private final Resource resource;

        public Worker(Resource resource) {
            this.resource = resource;
        }

        @Override
        public void run() {
            while (true) {
                resource.use();
                // Trying to avoid livelock
                try {
                    Thread.sleep((long) (Math.random() * 100));
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                }
            }
        }
    }

    public static void main(String[] args) {
        Resource resource = new Resource();
        Worker worker1 = new Worker(resource);
        Worker worker2 = new Worker(resource);

        worker1.start();
        worker2.start();
    }
}

```
In this example, two worker threads repeatedly attempt to use a shared resource. Without any form of coordination, they might end up in a livelock situation where they continuously try to use the resource but fail to make progress due to the other thread's actions.

### **Producer-Consumer Problem**

![[Pasted image 20240409143344.jpg]]

- **Problem Challenge**: The main challenge in the Producer-Consumer Problem is to ensure that producers do not add data to the buffer when it is full, and consumers do not attempt to remove data from the buffer when it is empty. This requires synchronization between threads to avoid race conditions, deadlocks, or data corruption.


#### Guarded Blocks 


```java
public class YourClass {
    private boolean success = false;

    public synchronized void guardedSuccess() { 
        while(!success) { 
            try { 
                wait(); 
            } catch (InterruptedException e) {
                // Handle exception
            } 
        } 
        System.out.println("Success achieved!"); 
    }

    public synchronized void notifySuccess() { 
        success = true; 
        notifyAll(); 
    }
}
```


Producer Consumer Problem solved by  guarded blocks

```java
synchronized(buffer) {
    while(buffer.isFull()) {
        buffer.wait();
    }
    buffer.add(data);
    buffer.notifyAll();
}


synchronized(buffer) {
    while(buffer.isEmpty()) {
        buffer.wait();
    }
    data = buffer.remove();
    buffer.notifyAll();
}
```



## AtomicInteger

```java
import java.util.concurrent.atomic.AtomicInteger;
private static AtomicInteger counter = new AtomicInteger(0);
```

1. **Atomic Operations**: The operations on `AtomicInteger` are atomic, meaning they are executed as a single, indivisible unit. This ensures that the updates are performed without any interference from other threads.
2. **Atomic Read and Write**: Methods like `get()` and `set()` allow you to atomically read and write the integer value.
3. **Atomic Increment and Decrement**: Methods like `getAndIncrement()`, `getAndDecrement()`, and `incrementAndGet()` allow you to atomically increment or decrement the integer value.

The primary use cases for `AtomicInteger` are in scenarios where you need to update a shared integer value concurrently, without the need for explicit synchronization. This can improve performance and scalability compared to using a `synchronized` block or a `Lock` object.