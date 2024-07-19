+++
title = 'Memory Allocation Pitfalls on Multicore CPUs'
date = 2011-09-12T15:09:46+02:00
draft = true
+++

# Memory allocation pitfalls on multi-core CPUs

```
This has been originally posted ages ago in an old private blog of mine and has been imported here.
I might not be proud about some of the formulations and use of swear words and slurs but to keep this
true to the original post, I have not modified anything. Don't blame me, blame past Martin. :)
```

Although it is less and less common nowadays, there are still “Thread-Safe Memory Allocators” in use. What do I mean with this? A standard, single-core based allocator that uses a simple locking mechanism on top to avoid race-conditions.
I am usually a big fan of “The simplest solution”(tm), but this one unfortunately leads to two big problems on multi-core architectures and therefore doesn’t really qualify as a ‘solution’ at all.

### Thread contention

I think it is pretty obvious that thread contention is bound to happen. When one thread is accessing the allocator ( allocating or releasing memory ) all other threads that are trying to do the same are blocked. It does not matter how fast the allocator is, as it will never be fast enough to not introduce contention and block other threads. This issue has an impact on performance especially in standard high-level gameplay code. As high-level gameplay code tend to use the allocator a lot ( creating/destroying objects, growing/shrinking dynamic arrays, etc. ) this is a recipe for just throwing away clock-cycles. For no gain at all. I am not talking about a few nano-seconds here as depending on the amount of runtime allocations, this can sum up faster than one might expect.

### False Cache-Sharing

That is the more serious issue, and not that obvious to see. Two threads are working on data in a memory-area that is mapped to the same cache-line. This is not a theoretical problem, but a situation that is not that unlikely to happen. The probability of running into that increases with the amount of allocator contention. There is a good chance that a non-thread-aware allocator returns consecutive memory areas for consecutive allocations. If these allocation requests are coming from different threads, false cache-sharing is waiting to happen.

#### Example

- **Thread_A** resides on **CPU0**
- **Thread_B** on **CPU1**

Both threads are doing totally unrelated calculations and both of them are allocating some memory.
Let’s assume both get a chunk of memory from the same cache-line.

This situation is called ‘false sharing’ or – what is even more fitting – ‘cache line ping-pong’. We have now created the biggest nightmare ( at least performance-wise ) for the cache-coherency protocol.

- **Thread_A** writes to his memory.
- This invalidates **Thread_B**‘s cache-line.
- The cache of **Thread_A** must be written back to memory …
- ... and read back again to the cache of **Thread_B**.

The same applies if Thread_B is modifying its memory area.

If you are interested in more details and also some performance impact measurements, check out [Analysis of False Cache Line Sharing Effects on Multicore CPUs](https://scholarworks.sjsu.edu/cgi/viewcontent.cgi?article=1001&context=etd_projects).

### Update

As I was asked what I would propose as a solution on the comments section of ADBAD, here is my answer:
My preferred solution would be to disallow dynamic allocations at runtime completely, but that might be a bit drastic 🙂

So I rather go with this answer:

Instead of using a ‘thread-safe allocator’ which introduces the mentioned problems, the usage of a ( I like to call it ) ‘Thread-Aware Allocator’ should be the way to go.
Each thread gets his own big blob of memory and the management is done on a per-thread basis. This reduces the thread-contention to the situations where a new memory chunk is needed. As every thread is allocating from his own memory-blob, the chances of false sharing due to the described reason are minimized.

One well documented example is the Intel [TBB Scalable Allocator](https://github.com/wjakob/tbb/blob/master/include/tbb/scalable_allocator.h). ( It starts a few pages down … search for ‘SCALABLE MEMORY ALLOCATION’ ).

### Further reading

- [Analysis of False Cache Line Sharing Effects on Multicore CPUs](https://scholarworks.sjsu.edu/cgi/viewcontent.cgi?article=1001&context=etd_projects)
- [Concurrency Hazards: False Sharing](https://www.codeproject.com/Articles/51553/Concurrency-Hazards-False-Sharing)
