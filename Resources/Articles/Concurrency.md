* [Concurrency](#concurrency)
  * [Concurrency in Mac OS and iOS](#different-ways-of-achieving-concurrency-in-mac-os-and-ios)
  * [POSIX Threads](#posix-threads)
  * [NSThread](#nsthread)
  * [GCD](#gcd)
  * [Quality of Service (QoS)](#quality-of-service-qos)
  * [NSOperationQueue](#nsoperationqueue)
  * [Cancel NSOperation problem](#cancel-nsoperation-problem)
  * [DispatchGroup](#what-is-dispatchgroup)
  * [@synchronized](#synchronized)
  * [Synchronous &amp; asynchronous tasks](#what-is-the-difference-between-synchronous--asynchronous-task)
  * [Semaphore](#semaphore)
  * [Lock](#lock)
  * [Mutex](#mutex)
  * [Semaphore vs Mutex vs Lock](#semaphore-vs-mutex-vs-lock)  

# Concurrency

## Different ways of achieving concurrency in Mac OS and iOS
There are basically three ways of achieving concurrency in iOS:  
1. threads  
2. GCD
3. NSOperationQueue  

The disadvantage of threads is that they relegate the burden of creating a scalable solution to the developer. You have to decide how many threads to create and adjust that number dynamically as conditions change. Also, the application assumes most of the costs associated with creating and maintaining the threads it uses.

OS X and iOS therefore prefer to take an asynchronous design approach to solving the concurrency problem rather than relying on threads.

One of the technologies for starting tasks asynchronously is Grand Central Dispatch (GCD) that relegates thread management down to the system level. All the developer has to do is define the tasks to be executed and add them to the appropriate dispatch queue. GCD takes care of creating the needed threads and scheduling tasks to run on those threads.

All dispatch queues are first-in, first-out (FIFO) data structures, so tasks are always started in the same order that they are added.

An operation queue is the Cocoa equivalent of a concurrent dispatch queue and is implemented by the NSOperationQueue class. Unlike dispatch queues, operation queues are not limited to executing tasks in FIFO order and support the creation of complex execution-order graphs for your tasks.

## POSIX Threads
Usually referred to as Pthreads, is a POSIX standard for threads defines an API for creating and manipulating threads. Pthreads defines a set of C programming language types, functions and constants. It is implemented with a pthread.h header and a thread library. There are around 100 Pthreads procedures, all prefixed `pthread_` and they can be categorized into four groups:

* Thread management - creating, joining threads etc.
* Mutexes
* Condition variables
* Synchronization between threads using read/write locks and barriers

Implementations of the API are available on many Unix-like POSIX-conformant operating systems such as FreeBSD, NetBSD, OpenBSD, GNU/Linux, Mac OS X and Solaris. DR-DOS and Microsoft Windows implementations also exist.
Use POSIX calls if cross-platform portability is required. If you are writing networking code that runs exclusively in OS X and iOS, you should generally avoid POSIX networking calls, because they are harder to work with than higher-level APIs. However, if you are writing networking code that must be shared with other platforms, you can use the POSIX networking APIs so that you can use the same code everywhere.

## NSThread

A simple Objective-C wrapper around pthreads. This makes the code look more familiar in a Cocoa environment. It is a relatively lightweight way to implement multiple paths of execution inside of an application. For example, you can define a thread as a subclass of `NSThread`, which encapsulates the code you want to run in the background.

Though Operation queues is the preferred way to perform tasks concurrently, depending on application there may still be times when you need to create custom threads.
Threads are still a good way to implement code that must run in real time.
Use threads for specific tasks that cannot be implemented in any other way.
If you need more predictable behavior from code running in the background, threads may still offer a better alternative.

_Inter-thread Communication_
* Direct messaging
* Global variables, shared memory, and objects
* Conditions
* Run loop sources
* Ports and sockets
* Message queues
* Cocoa distributed objects
__Dispatch Queue__

Dispatch queues are a C-based mechanism for __executing custom tasks__. A dispatch queue executes tasks either serially or concurrently but __always in a FIFO order__. A serial dispatch queue runs only one task at a time, waiting until that task is complete before dequeuing and starting a new one. By contrast, a concurrent dispatch queue starts as many tasks as it can without waiting for already started tasks to finish.

- Serial

Serial queues (also known as private dispatch queues) execute one task at a time in the order in which they are added to the queue. The currently executing task runs on a distinct thread (which can vary from task to task) that is managed by the dispatch queue. Serial queues are often used to synchronize access to a specific resource.
You can create as many serial queues as you need, and each queue operates concurrently with respect to all other queues. In other words, if you create four serial queues, each queue executes only one task at a time but up to four tasks could still execute concurrently, one from each queue.

- Concurrent

Concurrent queues (also known as a type of global dispatch queue) execute one or more tasks concurrently, but tasks are still started in the order in which they were added to the queue. The currently executing tasks run on distinct threads that are managed by the dispatch queue. The exact number of tasks executing at any given point is variable and depends on system conditions.
In iOS 5 and later, you can create concurrent dispatch queues yourself by specifying `DISPATCH_QUEUE_CONCURRENT` as the queue type. In addition, there are four predefined global concurrent queues for your application to use.

- Main dispatch queue

The main dispatch queue is a globally available serial queue that executes tasks on the application’s main thread. This queue works with the application’s run loop (if one is present) to interleave the execution of queued tasks with the execution of other event sources attached to the run loop. Because it runs on your application’s main thread, the main queue is often used as a key synchronization point for an application. Although you do not need to create the main dispatch queue, you do need to make sure your application drains it appropriately.

__Dispatch Source__

Dispatch sources are a C-based mechanism __for processing specific types of system events asynchronously__. A dispatch source encapsulates information about a particular type of system event and submits a specific block object or function to a dispatch queue whenever that event occurs. You can use dispatch sources to monitor the following types of system events:

* Timers
* Signal handlers
* Descriptor-related events
* Process-related events
* Mach port events
* Custom events that you trigger

- async - concurrent: the code runs on a background thread. Control returns immediately to the main thread (and UI). The block can't assume that it's the only block running on that queue
- async - serial: the code runs on a background thread. Control returns immediately to the main thread. The block can assume that it's the only block running on that queue
- sync - concurrent: the code runs on a background thread but the main thread waits for it to finish, blocking any updates to the UI. The block can't assume that it's the only block running on that queue (I could have added another block using async a few seconds previously)
- sync - serial: the code runs on a background thread but the main thread waits for it to finish, blocking any updates to the UI. The block can assume that it's the only block running on that queue

## Quality of Service (QoS)

The QoS classes are:
* User-interactive: This represents tasks that need to be done immediately in order to provide a nice user experience. Use it for UI updates, event handling and small workloads that require low latency. The total amount of work done in this class during the execution of your app should be small. This should run on the main thread.
* User-initiated: The represents tasks that are initiated from the UI and can be performed asynchronously. It should be used when the user is waiting for immediate results, and for tasks required to continue user interaction. This will get mapped into the high priority global queue.
* Utility: This represents long-running tasks, typically with a user-visible progress indicator. Use it for computations, I/O, networking, continous data feeds and similar tasks. This class is designed to be energy efficient. This will get mapped into the low priority global queue.
* Background: This represents tasks that the user is not directly aware of. Use it for prefetching, maintenance, and other tasks that don’t require user interaction and aren’t time-sensitive. This will get mapped into the background priority global queue.
```swift
DispatchQueue.global(attributes: [.qosDefault]).async {
  // Background thread
  DispatchQueue.main.async(execute: {
    // UI Updates
  })
}
```

## NSOperationQueue
Operation queues are a Cocoa abstraction of the queue model exposed by GCD. While GCD offers more low-level control, operation queues implement several convenient features on top of it, which often makes it the best and safest choice for application developers. The `NSOperationQueue` class has two different types of queues: the main queue and custom queues. The main queue runs on the main thread, and custom queues are processed in the background. In any case, the tasks which are processed by these queues are represented as subclasses of `NSOperation`. Whereas dispatch queues always execute tasks in first-in, first-out order, operation queues __take other factors into account when determining the execution order of tasks__. Primary among these factors is whether a given task depends on the completion of other tasks. You configure dependencies when defining your tasks and can use them to create complex execution-order graphs for your tasks. Because the `NSOperation` class is essentially an abstract base class, you typically define custom subclasses to perform your tasks. However, the Foundation framework does include some concrete subclasses that you can create and use as is to perform tasks.

`NSBlockOperation` exectues a block. `NSInvocationOperation` executes a `NSInvocation` (or a method defined by target, selector, object). `NSOperation` must be subclassed, it offers the most flexibility but requires the most code. `NSBlockOperation` and `NSInvocationOperation` are both subclasses of `NSOperation`. They are provided by the system so you don't have to create a new subclass for simple tasks. Using `NSBlockOperation` and `NSInvocationOperation` should be enough for most tasks.

After an operation begins executing, it continues performing its task until it is finished or until your code explicitly cancels the operation. Cancellation can occur at any time, even before an operation begins executing. Although the NSOperation class provides a way for clients to cancel an operation, recognizing the cancellation event is voluntary by necessity. If an operation were terminated outright, there might not be a way to reclaim resources that had been allocated. As a result, operation objects are expected to check for cancellation events and to exit gracefully when they occur in the middle of the operation.

## Cancel NSOperation problem

Canceling the operation will only update its isCancelled property to YES.
To be able to cancel the operation, you should do the following: 

```objectivec  
NSBlockOperation * op = [NSBlockOperation new];
__weak NSBlockOperation * weakOp = op; // Use a weak reference to avoid a retain cycle
[op addExecutionBlock:^{
    // Put this code between whenever you want to allow an operation to cancel
    // For example: Inside a loop, before a large calculation, before saving/updating data or UI, etc.
    if (weakOp.isCancelled) return;

    // Do something..
];
```

## GCD
With GCD you don’t interact with threads directly anymore. Instead you add blocks of code to queues, and GCD manages a thread pool behind the scenes. GCD decides on which particular thread your code blocks are going to be executed on, and it manages these threads according to the available system resources. This alleviates the problem of too many threads being created, because the threads are now centrally managed and abstracted away from application developers. The other important change with GCD is that you as a developer think about work items in a queue rather than threads. This new mental model of concurrency is easier to work with. GCD exposes five different queues: the main queue running on the main thread, three background queues with different priorities, and one background queue with an even lower priority, which is I/O throttled. Furthermore, you can create custom queues, which can either be serial or concurrent queues. While custom queues are a powerful abstraction, all blocks you schedule on them will ultimately trickle down to one of the system’s global queues and its thread pool(s).

## What is DispatchGroup?
`DispatchGroup` allows for aggregate synchronization of work. We can use them to submit multiple different work items and track when they all complete, even though they might run on different queues. This behavior can be helpful when progress can’t be made until all of the specified tasks are complete. 

## Synchronized
The `@synchronized` directive is a convenient way to create mutex locks on the fly in Objective-C code.

`Synchronized` guarantees that only one thread can be executing that code in the block at any given time.

Syntax:
```objectivec  
 @synchronized(key)
 {
  // thread-safe code
 }
 ```

Example:
```objectivec  
 -(void)AppendExisting:(NSString*)val
{
  @synchronized (oldValue) {
      [oldValue stringByAppendingFormat:@"-%@",val];
  }
}
```
Now the above code is perfectly thread safe..Now Multiple threads can change the value.


## What is the difference between Synchronous & Asynchronous task?
`Synchronous`: waits until the task has completed
`Asynchronous`: completes a task in background and can notify you when complete

## Semaphore

_Is the number of free identical toilet keys. Example, say we have four toilets with identical locks and keys. The semaphore count - the count of keys - is set to 4 at beginning (all four toilets are free), then the count value is decremented as people are coming in. If all toilets are full, ie. there are no free keys left, the semaphore count is 0. Now, when eq. one person leaves the toilet, semaphore is increased to 1 (one free key), and given to the next person in the queue._

Variable or abstract data type used to control access to a common resource by multiple processes in a concurrent system. A trivial semaphore is a plain variable that is changed (for example, incremented or decremented, or toggled) depending on programmer-defined conditions. The variable is then used as a condition to control access to some system resource. A useful way to think of a semaphore as used in the real-world systems is as a record of how many units of a particular resource are available, coupled with operations to adjust that record safely (i.e. to avoid race conditions) as units are required or become free, and, if necessary, wait until a unit of the resource becomes available. Semaphores are a useful tool in the prevention of race conditions; however, their use is by no means a guarantee that a program is free from these problems. Semaphores which allow an arbitrary resource count are called __counting semaphores__, while semaphores which are restricted to the values 0 and 1 (or locked/unlocked, unavailable/available) are called __binary semaphores__ and are used to implement locks.

## Lock
Mechanism for enforcing limits on access to a resource in an environment where there are many threads of execution. A lock is designed to enforce a mutual exclusion concurrency control policy.

## Mutex
_Is a key to a toilet. One person can have the key - occupy the toilet - at the time. When finished, the person gives (frees) the key to the next person in the queue._

## Semaphore vs Mutex vs Lock

__Explanation 1__

A mutex is essentially the same thing as a binary semaphore and sometimes uses the same basic implementation. The differences between them are in how they are used. While a binary semaphore may be used as a mutex, a mutex is a more specific use-case, in that only the thread that locked the mutex is supposed to unlock it.
A mutex is a synchronization object. You acquire a lock on a mutex at the beginning of a section of code, and release it at the end, in order to ensure that no other thread is accessing the same data at the same time. A mutex typically has a lifetime equal to that of the data it is protecting, and that one mutex is accessed by multiple threads.
A lock object is an object that encapsulates that lock. When the object is constructed it acquires the lock on the mutex. When it is destructed the lock is released. You typically create a new lock object for every access to the shared data.

__Explanation 2__

A mutex is an object which can be locked. A lock is the object which maintains the lock. To create a lock, you need to pass it a mutex.
