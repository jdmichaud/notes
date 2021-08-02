Main reference: https://gcc.gnu.org/onlinedocs/gcc-11.2.0/gcc/Using-Assembly-Language-with-C.html#Using-Assembly-Language-with-C
More approchable: https://github.com/0xAX/linux-insides/blob/2cff4abf08711c4af60b3a801a13ee373bd93e2e/Theory/linux-theory-3.md


Use `__asm__` instead of `asm` for better portability.
`volatile` if side-effects.

asm folowed by a list of instruction:
```c
__asm__ volatile(
    "instruction1;"
    "instruction2;"
    "instruction3;"
);
```
or
```c
__asm__ volatile("instruction1; instruction2; instruction3;");
```

By default GCC use AT&T style assembler.

register in basic mode: %eax.
         in extended mode: %%eax
Destination register is on the right: move $1, %%eax
Immediate mode value is prefixed by $.

In order to input C variables, use extended mode:

```c
  int res = main(0, 0) & 0xFF;

  // gcc uses asm AT&T syntax by default.
  // . register are prefixed with %
  // . values by $
  // . destination register at the end
  __asm__ volatile( // asm is gnu, __asm__ is more portable.
      "movl $"USE_MACRO(SYS_exit)", %%eax;" // Set eax to the sys_exit syscall number
      "movl %0, %%edi;" // set exit status to res
      "syscall;"
      : /* no output */
      : "m" (res)
  );
```
You set the output operands as the variable where you want to write.
Input operands as the variable you want to use.
On those operands you set constraints. For example: `"m"` is used as a memory operands.
