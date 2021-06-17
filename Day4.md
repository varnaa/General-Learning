# Thread Pools

## Background

Server Programs such as database and web servers repeatedly execute requests from multiple clients and these are
oriented around processing a large number of short tasks. An approach for building a server application would be to
create a new thread each time a request arrives and service this new request in the newly created thread. While this
approach seems simple to implement, it has significant disadvantages. A server that creates a new thread for every
request would spend more time and consume more system resources in creating and destroying threads than processing
actual requests.

Since active threads consume system resources, a JVM creating too many threads at the same time can cause the system to
run out of memory. This necessitates the need to limit the number of threads being created.

## What is ThreadPool in Java?

A thread pool reuses previously created threads to execute current tasks and offers a solution to the problem of thread
cycle overhead and resource thrashing. Since the thread is already existing when the request arrives, the delay
introduced by thread creation is eliminated, making the application more responsive.

Java provides the Executor framework which is centered around the Executor interface, its sub-interface –ExecutorService
and the class-ThreadPoolExecutor, which implements both of these interfaces. By using the executor, one only has to
implement the Runnable objects and send them to the executor to execute. They allow you to take advantage of threading,
but focus on the tasks that you want the thread to perform, instead of thread mechanics. To use thread pools, we first
create a object of ExecutorService and pass a set of tasks to it. ThreadPoolExecutor class allows to set the core and
maximum pool size.The runnables that are run by a particular thread are executed sequentially.

Executor Thread Pool Methods

Method Description newFixedThreadPool(int)           Creates a fixed size thread pool. newCachedThreadPool()
Creates a thread pool that creates new threads as needed, but will reuse previously constructed threads when they are
available newSingleThreadExecutor()         Creates a single thread. 