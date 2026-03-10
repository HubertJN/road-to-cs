# Notes for CS61C

Lecture course focused on the hardware-software interface, teaching how high-level code is executed by computer hardware and how to optimize for performance.

## Lecture 1

Course books:
 - Patterson & Hennessy, <i> Computer Organization and Design </i>
 - Kernighan & Ritchie, <i> The C Programming Language </i>
 - Barroso & Holzle, <i> The Datacenter as a Computer </i>

Key concepts:
- Inside computers, everything is a number
- Stored as bits (8-bit, 16-bit, 32-bit, 64-bit, ...)
- Integer and floating-point operations can lead to results too big to store, and can lead to overflow/underflow.
- Common word sizes: 8 bits = byte, 16 bits = half-word, 32 bits = word, 64 bits = double word.

Number representation:
- Can have different bases (2, 5, 10, 16)
 - Hexadecimal is base 16 and has digits:
  - 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F

  Signed and Unsigned Integers:
  - <i> Signed </i> integers used for calculation
  - <i> Unsigned </i> integers are used for addresses
  - Unsigned integers in 32 bit word represent 0 to $2^{23}-1$
  - Signed integers in C; want $\frac{1}{2}$ number < 0 and $\frac{1}{2}$ > 0, and 0.
  - <i> Two's complement treats 0 as positive </i>, so 32-bit word represents $2^{32}$ integers from $-2^{31}$ to $2^{31}-1$
    - Note: one negative number with no positive version.
  - <i> Most significant bit (leftmost) is the sign bit </i>, since 0 means positive (including 0), 1 means negative.

Two's complement:
- Condition for overflow is carry in $\neq$ carry out
- Carry in = carry from less significant bits
- Carry out = carry to more significant bits

How to get two's complement:
- For N-bit word, complement to $2_{\text{ten}}^N$
  - For 4-bit number $3_{\text{ten}}$ = 0011, two's complement (i.e. $-3_{\text{ten}}$) would be $16_{\text{ten}} - 3_{\text{ten}} = 13_{\text{ten}}$ or $10000_{\text{two}} - 0011_{\text{two}} = 1101_{\text{two}}$
- Easier way:
  - Invert all bits and add 1

## Lecture 2

Compilation:
- C compilers map C programs into architecture-specific machine code (strings of 1s and 0s).
  - Unlike Java, which converts to architecture-independent bytecode
  - For C, generally a two-part process of compiling .c files to .o files, then linking the .o files into executables
  - Assembling is also done (but is hidden, i.e., done automatically by default.)

Advantages:
- Excellent run-time performance: generally much faster than Scheme or Java for comparable code
- Fair compilation time: enhancements in compilation procedure (Makefiles) allow only modified files to be recompiled
- Why C?: can write programs that allow us to exploit underlying features of the architecture - memory management, special instructions, parallelism

Disadvantages:
- Compiled files, including executables, are architecture-specific
- Executable must be rebuilt on each new system ("porting your code")
- "Change -> Compile -> Run" iteration cycle can be slow during development

C Pre-Processor (CPP):
- C source files first pass through the macro processor, CPP, before the compiler sees the code
- CPP replaces comments with a single space
- CPP commands begin with `#`
- `.h` files are called header files
- `#include "file.h"` /* Inserts file.h into output */
- `#include <stdio.h>` /* Looks for file in standard location */
- `#define M_PI (3.14159)` /* Define constant */
- `#if`/`#endif` /* Conditional inclusion of text */
- Use `--save-temps` option to `gcc` to see result of preprocessing
- Full documentation at: [http://gcc.gnu.org/onlinedocs/cpp/](http://gcc.gnu.org/onlinedocs/cpp/)

C history:
- C was preceded by B and B was preceded by BCPL

Integers:
- `C: int` can be 16-bit, 32-bit or 64-bit depending on architecture
- `C: int` should be the integer type that the target processor works with most efficiently
- Only guarantee: `sizeof(long long) ≥ sizeof(long) ≥ sizeof(int) ≥ sizeof(short)`
  - Also, `short` ≥ 16 bits, `long` ≥ 32 bits
  - All could be 64 bits

Consts and Enums:
- Constant is assigned a typed value once in the declaration; value can't be changed during execution of program
- Enums: a group of related integer constants used to parameterize libraries:
- `enum cardsuit {CLUBS,DIAMONDS,HEARTS,SPADES};

Address, Value, Pointer:
- Consider memory to be asingle huge array
  - Each cell of an array has an associated address
  - Each cell also stores some value
- An address referes to a particular memory location
- Pointer: A variable that contains the address of a variable

Pointer Syntax:
- `int *x;`
  - Tells compiler that variable x is address of an `int`
- `x = &y;`
  - Tells compiler to assign address of y to x
  - `&` called the "address operator" in this context
- `z = *x;`
  - Tells compiler to assign value at address in x to z
  - `*` called the "dereference operator" in this context
- `*x = z;`
  - Tells compiler to assign value z to address at x

Pointers and Parameter Passing:
- Java and C pass parameters "by value"
  - Function gets a copy of the parameter, so changing the copy cannot change the original
- In order to change value within function, pass a pointer.