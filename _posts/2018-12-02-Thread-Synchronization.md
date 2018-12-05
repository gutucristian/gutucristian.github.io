There are various thread synchronization primitives. A few are: mutexes, condition variables, spinlocks, read-write locks, and barriers.

**Mutex:** primitive that helps ensure prevent resource contention (conflict over access to a shared resource such as random access memory, disk storage, cache memory, internal buses or external network devices) by using "mutual exclusion." Specifically, it helps us control who can access _shared resources_. Namely, we can use a mutex to enforce that some data structure is only accessed by one piece of code at a time. This helps prevent inconsistencies.

**Condition Variables:** a handshake mechanism that... 

**Spinlocks:** instead of context switches (that happen with mutexes), spinlocks will "spin" and repeatedly check to see if the lock is unlocked. Spinning is fast, so the latency between a lock-unlock pair is small. However, spinning does not accomplish any work (it blocks the CPU, "busy waiting") so it is not as efficient as a sleeping mutex if the time spent "spinning" is great.

**Barriers:** useful when threads need to compute in phases. Barriers are used to enforce all threads which finished computing for the current phase to wait for other threads that are still computing. Once all threads are finished with the current phase, they are unleashed and the process repeats.

More coming (after finals)...
