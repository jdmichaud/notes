The libc provides two functions:
- provides various helper functions to facilitate development (printf)
- provides POSIX functions to interact with the OS

The libc is linked automatically with any executable generated by gcc by default.
[To compile without the libc](https://stackoverflow.com/questions/2548486/compiling-without-libc): gcc -nostdlib test.c

The real linux entry point to an executable is `_start`. It is provided by the libc which calls `main`.

If you need to perform a system call, you need to 
- retrieve the syscall number from `usr/syscall.h`.
- put it in %%eax
- put the parameters is the CPU [registers](https://stackoverflow.com/a/5292329/2603925)
- call `syscall`
