# Chapter 18 - Concurrency
- defn: multithreaded processing - execute multiple tasks at the same time.
- Concurrency API was introduced to avoid manually handling Threads

### Objects/Interfaces to know
- Thread                     - Thread holds a task and is used to to execute. thread != Thread.
- Runnable                   - A task without a return type
- Callable                   - A task with    a return type
- ExecutorService            - (Task Manager) Object that manages Thread creation/deletion/recycle
- ScheduledExecutorService   - Task Manager that can schedule tasks
- Future                     - Represents the "result" of a task

# Threads
- defn: thread   - smallest unit of execution that can be scheduled by the operating system
- defn: process  - a group of associated threads that execute in the same shared environment
- defn: task     - single unit of work performed by a thread
- Single threaded process vs multithreaded process
- A thread can complete multiple independent tasks, but only one task at a time
- If one thread updates the value of a static object, all threads immediately see the update

### Thread Types
- All java applications are multithreaded, even hello world
- defn: system thread - thread created by the JVM and runs in the background of the application.
- Garbage collection is managed by a system thread
- Generally, system threads are invisible
- defn: user-defined thread - a thread created by the developer to accomplish a specific task

### Thread Concurrency
- defn: concurrency - the property of executing multiple threads and processes at the same time
- Even single CPU PCs have multiple threads. In general, threads = cpu * 2
- defn: thread scheduler - determines which threads should be currently executing 
- defn: round-robin schedule - each available thread receives an equal number of CPU cycles in circular order.
- defn: context switch - process of storing a thread's current state and later restoring the state of the thread to continue execution.
- There is often lost time associated with a context switch 
- defn: thread priority - numeric value associated with a thread that is taken into consideration by the thread scheduler when determining which threads should currently be executing.
- In Java, thread priorities are specified as integer values

### Runnable
- java.lang.Runnable
- Runnable is a functional interface with a zero argument + void run() method.
``` 
@FunctionalInterface public interface Runnable {
    void run();
}
```
- Runnable is commonly used to define the task or work a thread will execute. 
- `Runnable r = () -> {}` is a valid lambda

### Creating Threads
- java.lang.Thread
- First you define a thread with a task, then you start the thread with `Thread.start()`
- Java does not provide any guarantees about the order in which a thread will be processed once it is started.
- Exam Trick: order of thread execution is not often guaranteed. 
- Defining Tasks:
    1. Calling Thread Constructor with a Runnable object or lambda
        * `new Thread(new PrintData()).start();`
    2. Creating a class that extends thread
        * `(new ReadInventoryThread()).start();`
- defn: asynchronous - main() method thread does not wait for the results before continuing
- defn:  synchronous - the program waits for the thread to finish executing before moving to the next line
- `run()` vs `start()`
    * calling run() on a Thread or Runnable does not actually start a new thread
    * calling .run() manually triggers the task synchronously, calling .start() creates a thread that will call run
- In general, you should implement Runnable instead of extending Thread.

### Polling with Sleep
- defn: Polling - process of intermittently checking data at some fixed interval. 
- Thread.sleep method requests the current thread to rest for a specified number of milliseconds.
- `Thread.sleep(millis)`
- throws checked `InterruptedException`

# Concurrency API
- Concurrency API was created to handle the complicated work of managing threads
- Includes ExecutorService interface, which defines services that create and manage threads for you

### Single-Thread Executor
- ExecutorService is an interface
- Executors factory class to create instances of ExecutorService
- `Executors.newSingleThreadExecutor();` method
```
ExecutorService service = Executors.newSingleThreadExecutor();
service.execute(task1)
service.execute(task2)
service.execute(task3)
service.shutdown()
```

### Shutting Down a Thread Executor
- Once finished using a thread executor, it is important that you call `.shutdown()`
- Failing to call shutdown will result in your application never terminating 
- ExecutorService life cycle (`.isShutdown()` vs `.isTerminated()`)
- Shutting Down is a process
    1. Start rejecting all new tasks while existing tasks finish (isShutdown=true, isTerminated=false)
    2. All existing tasks finish (isShutdown=true, isTerminated=true)
- RejectedExecutionException thrown
- `.shutdown()` does not actually stop any tasks that have already been submitted to the thread executor
- `.shutdownNow()` will ATTEMPT to stop all running tasks. Not guaranteed
- `.shutDownNow()` returns `List<Runnable>` of tasks that were submitted but never started
- ExecutorService does not extend AutoCloseable, so you cannot use try-with-resource

### Submitting Tasks
-  ExecutorService methods to submit tasks
    1. `.execute()`
        * inherited from Executor
        * Takes a Runnable lambda expression or instance and completes the task asynchronously
        * result of the task is ignored since void return type of Runnable.
        * "fire and forget" method
    2. `.submit()`
        * asynchronously just like .execute()
        * returns a `Future` instance that can be used to determine whether the task is complete
        * can also be used to return a generic result object after the task has been completed
        * overloaded to accept either Runnable or Callable
    3. `.invokeAll()`
    4. `.invokeAny()`
- In general, submit > execute, even if you don't need the Future object

### Waiting for Results
- Future instance can be used to determine the result
- Future is an interface
- Future methods
    - `.isDone()`
        * Returns true if the task was completed, threw an exception, or was cancelled
    - `.isCancelled()`
        * Returns true if the task was cancelled before it completed normally
    - `.cancel()`
        * Attempts to cancel execution of the task
        * returns true if it was successfully cancelled
        * returns false if it could not be cancelled or it completed
    - `.get()`
        * Retrieves the result of a task, waiting endlessly if it is not yet available
        * overloaded with two timeout params. 
- Checked `TimeoutException` thrown if attempting to get result times out
- `.get()` always returns null when working with Runnable expressions because Runnable::run returns void in the FunctionalInterface
- TimeUnit enum : TimeUnit.DAYS, .HOURS, .MINUTES, .SECONDS, .MILLISECONDS, .MICROSECONDS, .NANOSECONDS

### Callable
- java.util.concurrent.Callable 
- Functional interface 
- Similar to Runnable except it's default method, call(), returns a value
```
@FunctionalInterface public interface Callable<V> {
    V call() throws Exception;
}
```
- Callable is often preferred over Runnable since it allows more details. 
- Runnable and Callable are often interchangeable
- ExecutorService includes an overloaded version of submit() that takes a Callable object

``` 
ExecutorService service = Executors.newSingleThreadExecutor();
Future<Integer> result = service.submit(() -> 30 + 11);
System.out.println(result.get());   // 41
service.shutdown();
```

### Waiting for All Tasks to Finish
- `service.awaitTermination(1, TimeUnit.MINUTES);`
- `.awaitTermination()` returns once all tasks finish, waits until timeout, or InterruptedException thrown

### Submitting Task Collections
- .invokeAll() and .invokeAny()
- Execute synchronously and take a Collection of tasks
- Will wait until the results are available before returning control to the enclosing program
- invokeAll()
    * executes all tasks in a collection 
    * returns `List<Future>` corresponding to each task in the order they were received
- invokeAny()
    * executes a collection of tasks
    * returns the result of one of the tasks that successfully completes execution
    * cancels all unfinished tasks
    * "short circuit"
- ExecutorService also includes overloaded version of invokeAny and invokeAll with timeout values

### Scheduling Tasks
- ScheduledExecutorService can be used to schedule tasks
- Sub-interface of ExecutorService
- Just like ExecutorService, instances are provided from factory methods in `Executors`
- `ScheduledExecutorService service = Executors.newSingleThreadedScheduledExecutor()`
- ScheduledExecutorService methods
    * schedule
        * Creates and executes a Callable task after the given delay
        * overloaded to accept Callable or Runnable
        * `service.schedule(task1, 10, TimeUnit.SECONDS)`
    * scheduleAtFixedRate
        * Creates and executes a Runnable task after the given initialDelay
        * Creates a new task every period value that passes
        * creates a new task regardless of whether the previous task finished
        * `service.scheduleAtFixedRate(command, 5, 1, TimeUnit.MINUTES);`
    * scheduleWithFixedDelay
        * Creates and executes a Runnable task after the given initial delay
        * Creates a new task after with the given delay between termination of one execution and the commencement of the next
        * useful when you don't know how long it will take to finish the task
- returns ScheduledFuture instance (nearly identical to Future, except it includes .getDelay() method that returns the remaining delay)

### Pools
- defn: thread pool - group of pre-instantiated reusable threads that are available to perform on a set of arbitrary tasks
- Allows multi threading now
- Just like ExecutorService, instances of pools are provided from factory methods in `Executors`
- factory methods
    * newCachedThreadPool
        * return `ExecutorService` 
        * Creates a unbounded size thread pool that creates new threads as needed
        * will reuse previously constructed thread when they are available
        * encouraged for executing many short-lived asynchronous tasks
        * strongly discouraged for long-lived processes.
    * newFixedThreadPool(int)
        * return `ExecutorService`
        * Creates a thread pool that reuses a fixed number of threads operating off a shared unbounded queue
        * calling `newFixedThreadPool(1)` is equivalent to `newSingleThreadExecutor()`
    * newScheduledThreadPool(int)
        * return `ScheduleExecutorService` 
        * Creates a thread pool that can schedule commands to run after a given delay or to execute periodically.
        * identical to `newFixedThreadPool` except that it returns an instance of ScheduledExecutorService.
- Since they return `ExecutorService`/`ScheduleExecutorService` types, we can submit tasks the same way as above.
- In practice, choosing Thread Pool Size is done by the number of CPUs and background processes running other than Java.

### Single Thread vs Pool
- While a single-thread executor will wait for a thread to become available before running the next task
- A pooled-thread executor can execute the next task concurrently. 
- If the pool runs out of available threads, the task will be queued by the thread executor and wait to be completed.

# Thread Safety
- defn: thread safe - property of an object that guarantees safe execution by multiple threads at the same time.
- defn: race condition - two tasks executing at the same time but the order is incompatible 

### Atomic Classes
- java.util.concurrent.atomic package
- Atomic classes for cookie-cutter thread-safe primitives.
- defn: Atomic - the property of an operation to be carried out a single unit of execution without any interference by another thread.
- AtomicBoolean, AtomicInteger, AtomicLong
- Atomic methods
    * get()
    * set()
    * getAndSet()
    * incrementAndGet()
    * getAndIncrement()
    * decrementAndGet()
    * getAndDecrement()

### Synchronized blocks
- defn: monitor - aka lock - a structure that supports mutual exclusion
- defn: mutual exclusion - property that at most one thread is executing a particular segment of code at a given time.
- defn: synchronized block - a block with a crude built-in lock behavior
- Any Object can be used as a monitor with `synchronized`
```
SheepManager manager = new SheepManager();
synchronized(manager) {
    //
}
// 
```
- `synchronized(obj)` to synchronize on a specific
- `synchronized(SheepManager.class)` to synchronize on a specific class 
- While other another thread is executing the synchronized block, all threads that arrive there will wait. 
- Once a block is finished, the lock is released. 
- Synchronizing the task != synchronizing the creation of the threads
- Synchronized block + atomic variables is redundant 
- Unlike Locks, we can't find out if a synchronized block is "locked" or not

### Synchronized Methods
- Adding synchronized modifier to method declaration
```
private synchronized void doThing() {
    //
}
```
- instance methods or static methods

---------------------------------------------------------------------------------------------------

# Lock Framework
- Conceptually similar to using `synchronized` keyword, but with a lot more helper methods
- You can "lock" only an object that implements the Lock interface
- Much safer than synchronized since we can add timeouts
- Many types of Locks, but we only need to know `ReentrantLock` for the exam.

### ReentrantLock Interface
- ReentrantLock is the most common form of Lock object
    1. Create an instance of Lock
    2. each thread calls lock() before using the protected code
    3. each thread calls unlock() before exiting the protected code
```
Lock lock = new ReentrantLock();
try {
    lock.lock();
    // protected code
} finally {
    lock.unlock();
}
```
- try/finally not required, but strongly recommended to prevent lock from being locked forever after exception thrown.
- Attempting to unlock an unlocked lock will throw `IllegalMonitorStateException`

### Lock methods
- lock()
    - Requests a lock and blocks until lock is acquired (infinite wait if locked)
- unlock()
    - Releases a lock - throws exception if already unlocked
- tryLock()
    - Requests a lock and returns immediately.
    - Returns a boolean indicating whether the lock was successfully acquired
    - Does not wait if another thread already holds the lock. returns immediately 
    - Overloaded with a timeout parameter to wait up to x time for lock to become available

---------------------------------------------------------------------------------------------------
# CyclicBarrier
- defn: CyclicBarrier - Barrier that halts threads from continuing until a certain number of threads assemble at the barrier.
- `new CyclicBarrier(4);`
- If ThreadPoolSize < CyclicBarrierLimit, the barrier will never be reached and code will hang indefinitely
- "wait and stop" barrier.
- Number of threads waiting at a cyclic barrier

---------------------------------------------------------------------------------------------------

# Concurrent Collections
- Collections framework also supplies collections with concurrency support out of the box.
- defn: memory consistency error - error when two threads have inconsistent views of what should be the same data.
- Conceptually, we want writes on one thread to be available to another thread. 
- Java may throw `ConcurrentModificationException` if attempting to modify non-concurrent collections
- At any given moment, all threads should have the same consistent view of a collection

### Classical collections
- Removing a key from a map while iterating through the keys.
``` 
Map<String, Integer> foodMap = new HashMap<>();
foodMap.put("penguin", 1);
foodMap.put("flamingo", 2);
for (String key: foodMap.keySet()) {
    foodMap.remove(key);                  // ConcurrentModificationException
}
```
- fixed  using `foodMap = new ConcurrentHashMap<>();`

### Concurrent Classes
- No point in using Concurrent classes if you are using an immutable or read-only object.
- Good practice to use a nonconcurrent reference to a concurrent object
- classes
    * ConcurrentHashMap
    * ConcurrentLinkedQueue
    * ConcurrentSkipListMap
    * ConcurrentSkipListSet
    * CopyOnWriteArrayList
    * CopyOnWriteArraySet
    * LinkedBlockingQueue
- These objects are safe to pass to multiple threads

### SkipList Collections
- SkipList means sorted (Tree)
- SkipList classes (ConcurrentSkipListSet and ConcurrentSkipListMap) are the concurrent versions of TreeSet and TreeMap
- They maintain their elements in natural order. 

### CopyOnWriteCollections
- CopyOnWriteArrayList and CopyOnWriteArraySet
- These classes copy all of their elements to a new underlying structure anytime an element is added/modified/removed.
- Modifying an individual element doesn't change the reference so a new structure is not allocated
- "snapshots"
- Any iterator established prior to a modification will not see the changes.
- CopyOnWrite classes can use a lot of memory since a new collection is allocated anytime collection is modified

### Blocking Queues
- LinkedBlockingQueue implements BlockingQueue interface
- BlockingQueue is just like a regular Queue except that it includes methods that will wait TimeUnit to complete an action
    * offer
    * poll
- Waiting the specified time and returning false if the time elapses before space is available
- Checked InterruptedException must be handled or declared

### Converting nonconcurrent collection to ConcurrentCollection
- Concurrency API includes methods for obtaining synchronized versions of existing noncurrent collection
- Synchronized collection != concurrent collection
- Unlike concurrent collections, the synchronized collections also throw ConcurrentModificationException
- Collections class
    * .synchronizedCollection
    * .synchronizedList
    * .synchronizedMap
    * .synchronizedSortedMap
    * .synchronizedNavigableMap
    * .synchronizedSet
    * .synchronizedSortedSet
    * .synchronizedNavigableSet

---------------------------------------------------------------------------------------------------

# Threading Problems
### Liveness
- defn: liveness - the ability of an application to be able to execute in a timely manner.
- Liveness problems are problems where the app becomes unresponsive or "stuck"
- Three types of liveness problems:
    1. Deadlock
    2. Starvation
    3. Livelock

### Deadlock
- defn: deadlock - two or more threads are blocked forever waiting on each other.

### Starvation
- defn: starvation - when a single thread is perpetually denied access to a shared resource or lock.
- The thread is still active, but is unable to complete its work. 

### Livelock
- defn: livelock - two or more threads are conceptually blocked forever, although they are each still active and trying to complete their task.
- Livelock is a special case of resource starvation.
- Livelock is often a result of two threads trying to resolve a deadlock.

### Race Condition
- defn: race condition - undesirable result that occurs when two tasks, which should be completed sequentially, complete at the same time.
- Race conditions can lead to invalid data. 
- Race conditions tend to appear in highly concurrent applications (like websites!)

---------------------------------------------------------------------------------------------------

# Parallel Streams
- One of the most powerful features of the Stream API is built-in concurrency support
- defn: serial stream   - stream in which results are ordered, with only one entry being processed at a time.
- defn: parallel stream - stream capable of processing results concurrently using multiple threads
- Parallel streams generally improve performance
- Parallel streams can change output though (e.g. findFirst)

### Creating Parallel streams
- Two ways
    1. From existing stream
        - calling `.parallel` on existing stream to convert it to one that support multithreaded processing.
        - .parallel is an intermediate operation.
        - `List.of(1,2).stream.parallel()`
    2. From collection object
        - `.parallelStream()`
        - Collection interface includes a method .parallelStream()
        - `List.of(1,2).parallelStream()`

### Parallel Decomposition
- defn: parallel decomposition - the process of taking a task, breaking it up into smaller pieces that can be performed concurrently, and then reassembling the results.
- Order is not guaranteed
- We can guarantee order with .forEachOrdered() on a parallel stream, at the tradeoff of some performance losses.

### Parallel Reductions
- reduction operations on parallel streams are referred to as parallel reductions
- .findAny may result in unexpected behavior
- `List.of(1,2,3).parallelStream().findAny().get();` // could be any value 1 2 or 3
- Since there's overhead with parallelization, sometimes it's slower in practice than a serial stream
- .reduce()
    * parallel streams are why we needed a third reduce overload
    * We needed a combiner as well as an accumulator because , in parallel, the results are computed then combined
    * "Intermediate values" and then combining. 
    * golden rule: make sure the accumulator and combiner work regardless of the order they are called in.
    * for parallel streams, the three arg .reduce() method is better than the one or two arg .reduce methods for performance reasons
- .collect()
    * Make sure to use a concurrent collection to combine the results or else `ConcurrentModificationException`
    * Failing to use the right collection will result in parallel stream performing like a serial stream
- defn: stateful lambda expression - lambda whose result depends on any state that might change during exception of the pipeline







---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------
