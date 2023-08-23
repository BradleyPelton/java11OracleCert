# Concurrency
- defn: Concurrency: Executing different paths of instructions at the same time
- Different Types of Concurrency:
    * Multiprocessing - multiple cpus (each cpu is running a different process)
    * Multitasking - one cpu very quickly alternating between tasks
    *Multithreading - different parts of the same program using different threads
- Concurrency does not necessarily mean parallel execution

### Pros
- Decrease waiting on response time
- Optimal usage of resources
- Higher efficiency
- Improved performance

### Cons
- Shared resource need to be handled carefully
- Managing data integrity can be more difficult
- managing memory is more difficult
- deadlock
- livelock
- race conditions
- switching threads comes with a cost

### Creating a thread
- `Thread t = new Thread()`
- `t.start()` (starts and stops since thread t has nothing to do)
- `Thread t = new Thread( () -> System.out.prinln("HI") )`
- `t.start()`  - Thread taking Runnable Functional Interface
- `Thread.currentThread.getId()` (e.g. Main: 1 , Runnable: 22)

###
- garbage collector has its own thread
- lots of threads in the background of java

### Three ways of creating threads:
- Create a new Thread with the no argument constructor
- Create a new Thread with constructor with Runnable lambda
- Create class that extends Thread and override run method
    * Be careful: myCustomThread.run() isn't myCustomThread.start()`
    * run will still use the main threadL1 since Thread hasn't started

###
- We hav eno guarantee which thread will finish first
- asynchronous 

### Callable Functional Interface
- just like Runnable, Callable can create a task that can be executed by separate threads
- callable returns Object. Generic need during implemantation
- call method returns generic type
- unlike Runnable, Callable can throw Exception

### Thread sleep
- Thread.sleep(seconds * 1000ms)
- using Thread.sleep to fix race conditions is usually a very bad idea
- `Thread.currentThread().interrupt()`

### Join
- wait for a thread to complete (indefinitely of finite time)
- `t.join()`
- can throw error, must be wrapped with interrupted exception

### Thread Interference
- if multiple threads are running, and they access a static field.
- They could both access the same or different values for the same static field
- data integrity issue
- Synchronized keyword
- only one thread at a time
- methods can be synchronized 
- sychronized blocks
- 
```
public static Object lock = new Object()
public static void incrementCounter() {
    synchronized (lock) {
        int current = this.current  // static field
        counter = current + 1;
    }
}
```

### synchronize keywork
- More popular version of a synchronized object is to sychronized this {}
- synchronize on the instance. Lock on the instance
- public static synchronized void increment Count() {}
- Synchronize the method instead of a synchronized block
- threads will wait until method is available

### Limitations of Synchronized keyword
- Threads are waiting a lot
- No way to check whether the lock is available
- if lock remains, threads will wait forever
- use ReetrantLock instead (or other native locks)
- 
### ReentrantLock
- protect a piece of code using the lock Interface and use more advanced functions

### Methods on ReentrantLock
- void lock()
- void unlock()
- boolean tryLock() / tryLock(long, TimeUnit)
```
public static Lock lock = new ReentrantLock()`
public static void incrementCounter {
    lock.lock();
    counter++;
    lock.unlock();
}
```
- best practice to put lock.unlock in a finally block to prevent exceptions from locking forever
- you can not unlcok an unlocked lock. Exception will be thrown

### Executor Service
- ExecutorService is an interface in java.util.concurrent package
- helps us to manage and execute tasks async
- Steps:
    * get an instance of ExecutorService
    * Execute tasks using ExecutorService
    * Close ExecutorService (important)
- If you don't call executorServiceImpl.shutdown(), the application won't stop running
```
SingleThreadExecutor STE = Executors.newSingleThreadExecutor();
STE.execute( () -> System.out.println("Hi") );
ste.shutdown()
```
- alternatively, instead of ste.execute, you can call
- `.invokeAny(List<Callable>)`
- `.invokeAll(List<Callabel<Integer>>)`

### Submitting Tasks using Submit
- Different methods
- Methods depend on exact implementation of ExecutorService interface
- Tasks will be performed asynchronously
- Afterwards, wait for ... the results
- Execute can't be used to execute Callable
- Instead, we must submit a callable task
- However, now, a Future object is being returned

### Future
- a future is like a mailbox
``` 
Future.isDone()
    .isCancelled()
    .cancel()
    .get()
    .get(long time, TimeUnit unit)
```

### ThreadPools
- Defn: ThreadPool: Multiple Threads that are performing tasks or waiting for a task
- Available threads pick up tasks from a queue
- When a thread is done, it will wait to pick up a new task instead of getting destroyed

- SingleThreadExecutor = Thread_pool_size = 1
- SingleThreadScheduledExecutor
- CachedThreadPool = auto scalling thread pool size (killing and spawning)
- FixedThreadPool = finite, fixed number of threads
- ScheduledThreadPool

```
ExecutorService eServe = Executors.newFixedThreadPool(3) // 3 thread
...
eServ.submit( () -> 5)
...
eServ.showDown()
```


### ExecutorService vs ScheduledExecutorService
- ExecutorService
    * .execute()
    * .submit()
    * .executeAny()
    * .executeAll()
- ScheduledExecutorService
    * .schedule()
    * .scheduleAtFixedRate()
    * ScheduledAtFixedDelay()


### Collections
- Standard Collections are not thread safe
- problems with data integrity while using multithreaded apis

### Concurrent Collections
- Concurrent collections allowing locking per segment
- Multiple threads can get read access
- Assure data integrity
- allow collections to be changed while looping (ConcurrentModification)

### ConcurrentMap
- implements Map interface
- Atomic operations for adding,removing,replacing key-value pairs
- ConcurrentHashMap
- ConcurrentSkipListMap
- is implemented by ConccurentNavigableMap

### BlockingQueue
- implements Queue
- First-in-First-out collection
-Thread-safe implementation with internal locks in the methods
- Blocks whne pulling empty queue
- Blocks when adding to full queue

### 
- learn: BlockingQueue, SkipList, CopyOnWrite
- `Set<String> setA = new ConcurrentSkipListSet<>();`
- `Map<String,String> mapB = new ConcurrentSkipListMap<>();`

### CopyOnWrite
- good for reading a lot, but slow when writing a lot
- the list we are looping over is different than the one we are writing to
- `List<String> cList = new CopyOnWriteArrayList<>();`


### Synchronized Collections

- Collection -> synchronized Collection
- great for turning existing collections to a synchronized collection

- Collections.synchronizedList(aList)
- .synchronizedList(myList)
- .synchronizedMap(myMap)
- .synchronizedCollection(myCollection)
- .synchronizedSet(mySet)

### Atomic Class
- safe operation on multithreaded encironment
- AtomicInteger- specialInteger that changing during atomic operations
- AtomicLong, AtomicBoolean, etc.
- get()
- set()
- compareAndSet()
- weakCompareAndSet()
- lazySet()
- getAndIncrement()
- incrementAndGet()
- getAndAdd()
- addAndGet()
```
public class AtomExample {
    static AtomicInteger aI = new AtomicInteger(0);
    main() {
        ExecutorService eServe = Executors.newFixedThreadPool(5)
        fori {
            eServe.submit( () -> System.out.println(ai>incrementAndGet) )
        }
        eServe.shutdown
    }
}
```

### Problems
- defn: Liveness - the state of a healthy application (such as readiness to receive requests and to respond)
- Threading Problems:
    * Deadlock
    * Livelock
    * Starvation
    * Race Condition

### Deadlock
- two or more threads are in a waiting state, no process can be made
- Threads are holding the resource the other thread need
- Canadian Door Problem

``` class a {
    psvm {
        Thread t1 = new Thread( () -> {
            synchronize (resource1) {     // Lock resource 1
                wait(10 seconds)
                synchronize resource 2 { // Deadlock
                }
            }
        })
        Thread t2 = new Thread( () -> {
            synchronized(resource2) { // Lock resource 2
                wait(10 seconds)
                synchronize(resource 1) { // Deadlock
                }
            }
        })
    }
}
```

### LiveLock
- Threads are triggering each other to do the same action repeatedly
- Stuck in a loop, but can end by itself in certain situation
- Terrible for performance
- Harder to spot than deadlock

- example:
    * A drinks a beer waiting for B to leave
    * B drinks a beer waitinf for A to leave
    * infinite beer drank, nothing progresses
- livelock means things are still happening, but not real progress made
- deadlock means nothing is happening, frozen

### Starvation
- defn: Thread with a lower prioirt cannot get access to the resources they need.
- Lower prio cannot progress

### Race condition
- Multiple Threads need access to the same resource
- Outcome of the operations depends on (Coincidental) order of execution






