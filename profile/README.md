# Preempt Systems

Our mission is a hard real-time operating system that is POSIX source-compatible with Linux.

QuantumRT implements the POSIX API (PSE51). Application code moves between Linux and QuantumRT without being rewritten against a proprietary API: pthreads, POSIX message queues, timers, and named semaphores behave as the standard specifies.

Every syscall has a proven worst-case bound, and the scheduler and timer are O(1): selecting the next thread and servicing the next timer take the same bounded time regardless of how many threads or pending timers exist. Precise Scheduling replaces the periodic tick with a one-shot hardware comparator armed to the next deadline, so timed wakeups occur at full `timespec` resolution rather than being rounded to a fixed tick interval.

The application logic is identical whether the target is Linux or QuantumRT. A periodic task looks like this:

```c
#include <time.h>

static void *worker(void *arg)
{
    struct timespec next;
    clock_gettime(CLOCK_MONOTONIC, &next);

    for (;;)
    {
        next.tv_sec += 1;
        clock_nanosleep(CLOCK_MONOTONIC, TIMER_ABSTIME, &next, NULL);
        do_work(arg);
    }

    return NULL;
}
```

### Contact

QuantumRT is at [preemptsystems.com](https://preemptsystems.com). For an evaluation license or technical questions, email [contact@preemptsystems.com](mailto:contact@preemptsystems.com).
