There are various thread synchronization primitives. A few are: mutexes, condition variables, spinlocks, read-write locks, and barriers.

**Mutex:** primitive that helps ensure prevent resource contention (conflict over access to a shared resource such as random access memory, disk storage, cache memory, internal buses or external network devices) by using "mutual exclusion." Specifically, it helps us control who can access _shared resources_. Namely, we can use a mutex to enforce that some data structure is only accessed by one piece of code at a time. This helps prevent inconsistencies.

**Condition Variables:** a handshake mechanism that... 

**Spinlocks:** instead of context switches (that happen with mutexes), spinlocks will "spin" and repeatedly check to see if the lock is unlocked. Spinning is fast, so the latency between a lock-unlock pair is small. However, spinning does not accomplish any work (it blocks the CPU, "busy waiting") so it is not as efficient as a sleeping mutex if the time spent "spinning" is great.

Spin locks make no sense on a single core because as long as the spin lock is polling the only available CPU core, no other thread can run and the lock will not get unlocked! On the contrary, if the thread was put to sleep, another thread could have ran and (possibly) unlocked the lock which would allow the first thread to continue executing once it woke up. Contention is a conflict over access to a shared resource. Low contention means that access to a shared resource is not as high. If there is low contention, that means the spin lock will only "spin" for a few iterations before the CPU can move on to do more productive work. Thus, the single core will not be wasted for a very long time as would have been the case with high contention where many threads want access to a shared resource which causes "spinning" time to be larger.

**Barriers:** useful when threads need to compute in phases. Barriers are used to enforce all threads which finished computing for the current phase to wait for other threads that are still computing. Once all threads are finished with the current phase, they are unleashed and the process repeats.

Good reads:
- https://stackoverflow.com/questions/5869825/when-should-one-use-a-spinlock-instead-of-mutex
- https://stackoverflow.com/questions/38124337/spinlock-vs-busy-wait

More coming (after finals)...
