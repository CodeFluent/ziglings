# IDEAS
- Create a zig build step command that can issue the command "git log --since="Thu Jun 30" -p"


# Thu Jul 14 01:51:52 EDT 2022
shoulda started taking notes earlier, but its aokkk.
Completed quiz 008 and exercises up to 9.

I also explored two rabbit holes:

## ISSUE: can you access zig arrays by negative integers?
aka arr[0,1,2], then [0]=0 and [-1]=2  
python has negative array indexing.
c does too: https://stackoverflow.com/questions/3473675/are-negative-array-indexes-allowed-in-c
> the subscript operator [] is that E1[E2] is identical to (*((E1)+(E2))).
check out all C types: `https://en.wikipedia.org/wiki/C_data_types#stddef.h`

now, the fun thing is zig has a `translate-c` command which takes in c code and transforms it to zig code. thus, zig must handle c code that will use negative array indexing.

underneath, `translate-c` used to use usize ONLY, which means 0..n indexing for arrays. Now? It's a lot more complicated. You're dealing with the C standard, edge cases, developer habits, unary vs modular operators, and other things: https://github.com/ziglang/zig/pull/8589

In the end the developers had to break down the grammar and order of operations for pointer arithmetic operations to ensure any valid C code on any platform would translate correctly into zig. See https://github.com/ziglang/zig/pull/8589/commits/c6f0b24394ff09b7cb775715cf06634bb40f3f52
To understand wtf platform architecture has to do with any of this see: https://github.com/ziglang/zig/issues/5185

I'm curious as to how Python handles this. 

## ISSUE: Why do need to set arrays to undefined in zig? Do we have to do the same in c?
https://ziglang.org/documentation/master/#undefined
