# Week 4 - x86 Disassembly

The Week's Assignment and Lab focus was on review assembly language and what compilers do when they turn C code into machine code. A review was conducted on windows programs to learn how they are actually formatted. Ghidra tool was the software used in the reverse engineering of the file for it key

---
# ANSWERS TO THE QUESTIONS

1. Both Machine code and assembly are low-level programming languages, but they differ in their level of abstraction and their representation where as, Machine code is a language that is directly executed by a computer's central processing unit (CPU) and in addition it consists of a series of binary instructions that are specific to a particular CPU architecture, and it is difficult for humans to read and write directly, Assembly language is a human-readable representation of machine code that uses mnemonic codes to represent the binary instructions of machine code, making it easier for programmers to read and write. Assembly language is specific to a particular CPU architecture, but it can be translated into machine code using an assembler.

2. if ESP was initially pointing to memory address 0x001270A4 and a push eax instruction is executed, ESP will be decremented by 4 bytes to 0x001270A0, and the contents of the EAX register will be stored at memory address 0x001270A0. Therefore, ESP will now be pointing to memory address 0x001270A0

3. A stack frame is a data structure used by a computer program during its execution to store information such as function's arguments, local variables, return address about a particular function call.

4. Variables(both local and global) and other data (constant and initialized) that have a fixed, known value at compile-time and are accessible throughout the program's execution are what you will find in the data section of a RAM.

5. The heap is a section of memory used for dynamic memory allocation. 

6. The code section of a program's virtual memory space is used to store the executable machine code of a program.

7. The `inc` instruction is a machine language instruction that is in incrementing value of a specified register or memory location and `inc` takes a single operand because `inc` is a  unary operator. 

8. The remainder of a binary division (modulo) after executing the div instruction can be found in the `edx` register, if a `div` instruction is performed on it.

9. The `jz`abbreviation for ("jump if zero") decides whether to jump or not based on the value of the ZF("Zero Flag") bit in the PSR("Process Status Register"). If the Zero Flag is set to 1, it jumps to a specified location, and if the Zero Flag is 0, it continues executing the next instruction.

10. The `jne` abbreviation for ("jump if not equal") decides whether to jump or not based on the value of the ZF("Zero Flag") bit in the PSR("Process Status Register"). If the Zero Flag is set to 0, it jumps to a specified location, and if the Zero Flag is 1, it continues execution the next available instruction.

11. The `mov` instruction abbreviation for ("move") is used to move data from one location to another in a computer's memory, overwriting any previous data stored in the destination location

12. The `TF flag` abbreviation for ("Trap Flag") is a bit in the Process Status Register that is used to control a computer's processor single-step mode which is ver useful for debugging of programs as it allows a  programmer to execute a program one instruction at a time and examine its state.

13. An attacker would want to control the EIP register inside a program because the attacker is able to make changes to the intended behavior of a program by altering the flow of execution once the is able to get access to it, which potentially allow the attacker to gain unauthorized access to a system, steal sensitive information, or perform other malicious actions.

14. The `AL` register is an 8-bit general-purpose register in the x86 architecture that holds the least significant byte of the AX register, which in turn is the lower 16 bits of the EAX register. Any modification to `AX` is also effected on a `EAX` register (A register for holding return values from functions, storing arithmetic results, and holding memory addresses.)

15. This instruction `xor eax, eax` is used to perform a logical exclusive OR operation between the EAX register and itself, with this all bits in EAX register becomes  0 and this would always make the result of this operation to be equal to zero, regardless of the original value of EAX.


# OPTIONAL

i. The `leave` an instruction in x86 assembly language is used to release a stack frame created by a previous function call. It achieves this by copying the value of `EBP` register (Extended Base Pointer) to `ESP` register (Extended Stack Pointer) and then restores the previous value of `EBP`, allowing the calling function to access its own stack frame and then continue with execution.

ii. The `retn` instruction in x86 assembly language is equivalent to the `pop eip` instruction. 

iii. A stack overflow is an error that occurs when a computer program runs out of stack space (a region of memory used by a program to store local variables, function arguments, and return addresses) while executing.

iv. A segmentation fault is a type of error that occurs when a program attempts to access memory that it is not allowed to access (for instance a memory that is outside of its address space.)

v.  The `ESI` (Extended Source Index) and `EDI`(Extended Destination Index) registers are both 32-bit general-purpose registers used for data manipulation and transfer in the x86 architecture. s.


# SOLUTION TO THE crackme AND EXPLANATION OF HOW I SOLVE IT USING GHIDRA
## STEPS
After downloading the file from the website, I extracted the downloaded zip file to my Desktop and provided with a file named `keyg3nme` I run the file `keyg3nme` in my linux terminal using `file command` to ascertain the type of file; from the output it showed that `keyg3nme` is 64-Bit file. In addition also run `strings` command on this executable to see the printable strings contained within this binary and I found out some interesting  strings including but not limited: `Enter your key :`, `Good job mate, now go keygen me`, `nope`.

I then proceeded to Ghidra to see how best I can decompile and analyze this executable file. First and foremost after launching Ghidra, from the `Symbol Tree` pane I search for the actual main function which is basically the entry point of the program and double click it to launch. This will bring both the listing entity into the main function. Looking at this in the Decompiler window pane it can be seen that the decompiler try to infer the signature of the main function, nontheless the C language standard defines exactly how `main` function signature looks like, I went on to copy how the main function and right click it the function signature and hit edit function and pasted in the signature from the C standard and the the types and the names differ from `undefined8 main(void)`. The statement `iVar1 = validate_key((ulong)local_14), I deduce that local_14 is an `integer` variable that is used for holding entered key by a user. The entered key is then casted to a `long` data type with a  function named (ulong) and then passed as a function to the function called `validate_key` and then stored in a variable called `iVar1`. I then double click the function `validate_key` to have a look at the actions it performs whenever it is called , upon opening it, I saw that it has a data type of `bool` and takes an integer value as its signature namely `iParam1` equivalent of `iVar1` in main, it then return a true or false to be stored in iVar. However in C language syntax  `1` is used to represent `true` and `0` is used to represent `false` which is assigned as the value of iVar variable. The value is achieved with the statement `return (ulong) (iParm1 % 0x4c7 == 0)` in the validate_key function. Since I know that iParm1 is equivalent to entered value by the user, but   0x4c7  is  a hexadecimal number literal, hence converting it to decimal implies 1223 as its equivalence. In summary the logic that is being carried by the key_validate function is `(if number_entered % 1223 == 0)` then return 1 if the executed condition is true 0 for false and then stored in the iVar variable. The next statement in the Decompiler pane goes to compare the value in iVar against 1 and if they are equal and equivalent then it prints  (“Good job mate”)`  else it prints (`nope`). After it returns 0 to the operating system to signal for the successful operation of the program.

## ANSWER
In conclusion  `Key = 0,  or Key = 1223, or any multiple of 1223` is the password for `keyg3nme`, because they are the values that will always make the logic statement `(if number_entered % 1223 == 0)` true whenever the validate_key function is called. 


