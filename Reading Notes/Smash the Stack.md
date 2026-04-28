## source: you find this article anywhere in web(Aleph One).

## Content

### Introduction
Goal: explain how to `smash the stack`
Concepts: 
	1. **Buffer:** a contiguous memory block of same type, which is C is often called *array*.
	2. **Static allocation:** allocate when *load program* on *data segment*. **Dynamic allocation** allocate at *run time* on *stack*. Here just concern second.

### Process memory organization
Process contains three region: **text, data and stack**.
	+ **text** stores the executable file, often *read-only* . View it as your **brain**.
	+ **data** stores the initialized and uninitialized data, corresponding to **.data** and **.bss** sections. Static data in here. You can change its size by **brk(2)**.
		 ![[process layout.png]]

### What is A Stack
1. **LIFO**
2. **PUSH:** Add element on the top of stack.
3. **POP:** Remove element on the top of stack.

### Why Do We Use A Stack
**For implementing the function call**

### The Stack Region
Understand a function call is just create a new stack frame.
1. push the **parameters** from right to left.
2. push the **IP** register's value.
3. push the current **FP** value. So that when call is over when can return to last stack frame. We call this value **SFP**(saved FP).
4. modify **SP** to **FP**
5. allocate the stack memory region. *ATTENTION*: Here will do the memory aligning which is caused by our CPU can only access memory whose address is the multiply of **word size**.

### Buffer Overflows
In stack frame, we can leverage the memory layout to stuff the buffer which allocated on stack to overwrite the stored metadata.

**Prolog** and **Epilog**

### Shell Code
First our goal is to acquire a shell. 
1. Disassemble the function call.
2. Disassemble the system call(conventions and call number)
3. Using **gdb** to see the hex number so we can construct a character array.

### Writing an Exploit
