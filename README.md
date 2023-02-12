# xv6-Kernel-Threads

Kernel Threads in the xv6 Operating System are implemented in this project. This is implemented on top of [this](https://github.com/mit-pdos/xv6-riscv) repository. Support for threads (lightweight processes) at the kernel level is provided, and semaphore implementation serves as a primitive synchronisation mechanism. One-to-one mapping and ticket-lock synchronisation are also added to the userland threading library.


## The following System Calls are added: 

### CLONE:

```
int clone(void(*function)(void *, void *), void *stack, int flags, void *arg1, void *arg2);
```

-   Userland threading library wrapper function calls the clone system call to construct a thread of a process.
-   _function_  - Function pointer which runs in the created thread.
-   _flags_  - Flags for the clone system call.
-   _stack_  - Child stack to pass the function(fcn) arguments.
-   _arg1_  &  _arg2_  - Arguments to function fcn.

### JOIN:

```
int join(int threadId);  
```

-   Join system call to block other threads while it waits for the current thread to finish, and then release thread resources once it has.
-   _threadId_  - thread Id of the executing thread.

### GETTID:

```
int gettid();
```

-   gettid system call to get the thread Id of currently executing thread

### TGKILL:

```
int tgkill(int tgid, int tid, int sig);
```

-   tgkill system call to terminate the thread from the thread group with the thread group id and thread id as the tgid and tid, respectively.
-   _tgid_  - thread group id (parent id)
-   _tid_  - thread id to be killed
-   _sig_  - signal number to be sent

## The following System Calls are modified:

### FORK:

-   Setting up tgid for the process.
-   Setting additional fields that were added to the proc structure to implement threads

### EXIT:

-   If it's a thread and the CLONE FILE flag is set, changing one condition will result in shutting all file descriptors.

### KILL:

-   Adding a clause that says a parent process can only be killed if all of its child threads have been killed using tgkill.

## Userland Threading Library:

Functions implemented for userland threading library are:

-   _thread_create_  - Malloc the stack for thread and then calls clone system call.
-   _thread_join_  - Free the stack for the thread and call join system call.
-   _tlock_acquire_  - Acquires a ticket lock
-   _tlock_release_  - Reelease the ticket lock
-   _tlock_init_  - Initialize the ticket lock

## Tests:

Added testing code to evaluate Kernel thread functionality across a variety of test cases.

## References:

-   [Remzi Arpaci-Dusseau project on kernel threads](https://github.com/remzi-arpacidusseau/ostep-projects/tree/master/concurrency-xv6-threads)
