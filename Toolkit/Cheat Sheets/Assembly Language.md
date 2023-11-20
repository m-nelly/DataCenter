## Overview
Assembly is a low-level programming language that is a “human-readable” interpretation of machine code. Its instruction set resembles machine code very closely which makes it difficult to understand at first. Modern processors use the x86-64 architecture. You will also encounter processors with ARM architectures, but for the sake of simplicity I will not be discussing that here.
## Core
### The CPU
Your CPU parses and executes instructions passes by a program loaded in memory. To do this it uses general purpose and special purpose registers which are memory locations within the CPU. If you have an x86 CPU these registers will hold 32 bits of data. If you have an x86_64 CPU these registers will hold 64 bits of data.
### General Purpose Registers - 32 Bit
EAX, EBX, ECX, EDX, ESP, EBP, ESI, EDI
-   Each register is 32 bits numbered from 0-31
-   AX refers to the first 16 bits of EAX
-   AH refers to the second byte of EAX
-   AL refers to the first byte of EAX
-   The same is true of EB(BH,BL), EC(CH,CL), etc.
-   The H refers to the ‘High Byte’
-   The L refers to the ‘Low Byte’
EAX - Accumulator Register
-   Stores operands and result data
EBX - Base Register
-   Points to data
ECX - Counter Register
-   Handles loops
EDX - Data Register
-   Input/Output
ESP - Stack Pointer 
- Top of the stack 
EBP - Base Pointer 
- Bottom of the stack
ESI - Source Index 
- Used for string operations 
EDI - Destination Index 
- Used for string operations
### EIP
EIP - Instruction Pointer 
- Contains the location of the next instruction 
- Intended to be read-only 
- Most exploits will be seeking to control EIP
### Syntax
#### Intel Syntax:
```
label: mnemonic operands ; comment
```
**Label** - Think of these as variable assignments where the value you are storing is the instruction in front of them. These are optional.
-   Numeric labels have local scope
-   Symbolic labels (ASCII) have global scope
-   Symbolic labels with a . in front of them have local scope
**Mnemonic** - This is the command or instruction to execute
**Operands** - These are the parameters passed into the mnemonic
-   Will reference registers, values, or variables
-   Operands are ordered destination, source
**Comment** - Anything after the semicolon and before the new line will be ignored by the compiler.
##### Example
```nasm
add     eax,ebx ; EAX = EAX + EBX
```
#### AT&T Syntax
```
label: mnemonic(suffix) (prefix)operand ; comment
```
**Label** - Works the same as Intel syntax
**Mnemonic** - Works the same as Intel syntax, but also requires a suffix that specifies the size of the data you are handling. 
- `b` - byte
- `w` - word (two bytes)
- `l` - long (four bytes)
**Operand** - Ordered source, destination, and also requires a prefix that specifies the type of operand.
- `%` - register
- `$` - constant
##### Example
```nasm
movl    %ebx,%eax ; EAX = EBX
```
### The Stack
The stack is a location in memory set aside for your program. As data is added to the stack, the stack pointer is decremented. This means as data is added, the address of the ‘top’ of the stack or ESP moves to a lower memory address. The stack grows ‘down’ toward the EBP or base pointer.
`push` 
- adds to the stack 
- ESP is decremented to a lower address 
- data is added to the top of the stack
`pop` 
- moves data from the top of the stack 
- ESP is incremented to a higher address 
- data is moved from the top of the stack to the destination
A stack frame is a section of the stack that is assigned to a function. Each program will have a ‘main’ function which will be called when the program is launched. Within this main function, any other function call will be assigned its own stack frame. Within each additional stack frame will be a return address that lets the CPU know where to return to when that function is finished executing.

`ret` - calls the program back to the return address at the top of the stack by moving the return address into EIP or the instruction pointer.
### Assembly Program Sections
`_start` - equivalent to the main() function in most languages
`.text` - defines functions
`.data` - contains variable assignments
```asm
section .text    
global _start_start:           ; write our string to stdout.        
mov     edx,len                ; third argument: message length.        
mov     ecx,msg                ; second argument: pointer to message to write.
mov     ebx,1                  ; first argument: file handle (stdout).        
mov     eax,4                  ; system call number (sys_write).        
int     0x80                   ; call kernel and exit.    
mov ebx,0                      ; first sys call argument: exit code.        
mov     eax,1                  ; system call number (sys_exit).        
int     0x80                   ; call kernel.section .datamsg     
db      "Hello, world!",0xa    ; the string to print.
len     equ     $ - msg        ; length of the string.
```
## References
- [Intel: Introduction to x64 Assembly](https://www.intel.com/content/dam/develop/external/us/en/documents/introduction-to-x64-assembly-181178.pdf)
- [64 Bit Intel Assembly Programming for Linux](http://library.bagrintsev.me/ASM/Introduction%20to%2064bit%20Intel%20Assembly%20Language%20Programming%20for%20Linux.2011.pdf)
- [Labels - Oracle Docs](https://docs.oracle.com/cd/E19120-01/open.solaris/817-5477/esqaq/index.html)
- [Yale x86 Assembly Guide - AT&T Syntax](http://flint.cs.yale.edu/cs421/papers/x86-asm/asm.html)
- [University of Virginia x86 Assembly Guide - Intel Syntax](http://www.cs.virginia.edu/~evans/cs216/guides/x86.html)
- [Stack Overflow Thread On Stack Size](https://stackoverflow.com/questions/34544565/assembly-how-to-modify-stack-size)
- [Stack Frames](https://www.geeksforgeeks.org/stack-frame-in-computer-organization/)
- [NSFW: Alh4zr3d’s x86 Windows Stack-Based Buffer Overflow](https://www.youtube.com/watch?v=Z2pQuGmFNrM&list=WL&index=25&t=631s)

