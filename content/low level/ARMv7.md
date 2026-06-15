# Install
- There are 3 ways to run ARM architecture on x84 system
        - Azure Cloude
        - Qemu Emulator
        - https://cpulator.01xz.net/?sys=arm
- On Azure setup with

```
sudo apt-get install gcc-arm-linux-gnueabi
vi hello.s
```

# Architecture
## Registers
Registers are small storage areas located close to the processor for quick access
- They are fast to access and use
- They hold temporary values

![[registers.png]]

- These registers hold hexadecimal (8) values of 4 bits each giving us 32 bits per register.
- `r0` to `r6` are General Purpose Registers
- `r7` is special register used for software interrupt call and holds the actual system call numbers
-  `sp` Stack pointer points to the next available location on the stack
        - If general purpose registers are full then we store a lot of different values in stack.
- `pc` Program Counter increments for every single instruction running
# writing assembly

hello.s
```
.global _start
.section .text
_start:
```
- `.global` makes the `"_start"` label visible outside the program for linker
- The `"_start"` label names a certain location in memory
        - Think of this like a function name which is basically just labeling a certain area in memory. If referenced return the value of that memory location that its pointing to
- `.section .text` creates a section inside of file that holds the instruction of the program
        - this is compared to `.data` section which might hold different data program need like strings arrays or any different values stored
## Exit code

```
.global _start
.section .text
_start:
        mov r0, #42
        mov r7, #1
        swi 0
```

- We use the special register `r7` and pass it a value that is used by kernel to actually call the particular systemcall that we are trying to use.
- The `r7` special purpose register stores the `syscall` code
- To see what value we have to pass refer - https://chromium.googlesource.com/chromiumos/docs/+/master/constants/syscalls.md#arm-32_bit_EABI
        - `arg0(%r0)` refers to the error code that is returned from the program once it exits
- Manual -> Instructions for armv7
        -
- `mov` stands for `MOV(immediate)` instruction which writes an immediate value to the designation register. It can optionally update the condition flags based on the value
        - `MOV <Rd>, #<imm8>` means MOV instruction on `<Rd>` (Destination Register) the register that we want to move our value to and  `<imm8>` (immediate value) which is constant value signified by `#`
- `mov r0, #42 refers mov instruction; r0 general purpose destination register; #47 constant error code`
- `mov r7, #1 refers mov instruction; r7 special register; #1 constant syscall code for exit`
- `swi` (software interrupt) passes execution to the processor
        - `SWI immed_8` where `immed_8` is a numeric expression evaluating to an integer in the range 0-255. It is ignored by the processor.
# Executing program
-`arm-linux-gnueabi-as hello.s -o hello.o` :- for assembling code `-as` means assemble `-o` means output `hello.o` means object file. The code is assembled but cannot be executed by the processor yet
- `arm-linux-gnueabli-gcc hello.o -o hello -nostdlib`
- `file hello` :- The file will be in `ELF` binary (Executable and Linkable Format) means executable form of a linux binary
- `./hello` executes file
- `echo $?` to print the error code on terminal

![[execute.png]]

- `r0 has 2a becuase 42 is 2a in hex`
- `r7 has 1`
- `pc increments by 4 after each instruction is executed hence 8`

# ADD
- ADD (Immediate, ARM)
        - This instruction adds an immediate value to register value, and writes the result to the destination register. It can optionally update the condition of the flag based on the result.
        - `ADD{S}<c> <Rd>, <Rn>, #<const>`
        - `<Rd> : Destination register, <Rn> : First operand, <Rm> : Second operand

```
.global _start
_start:
        mov r0, #4
        // r1 = r0 + 3
        add r1, r0, #3
        // r0 : 00000004
        // r1 : 00000007
        // pc : 00000008
```

- Add (register, ARM)
        - This instruction add a register value and an optionally-shifted register value and writes the result to the result to the destination register. It can optionally update the condition flags based on the result.
        - `ADD{S}<c> <Rd>, <Rn>, <Rm>{, <shift>}`
```
.global _start
_start:
        mov r0, #4
        mov r2, #2
        add r1, r0, r2
        // r0 : 00000004
        // r1 : 00000006
        // r2 : 00000002
        // pc : 0000000c
```
----
move value of one register to another
```
.global _start
_start:
        mov r0, #4
        mov r1, r0
        // r0 : 00000004
        // r1 : 00000004
        // pc : 00000008
```

# SUB
- SUB (register)
        - This instruction subtracts an optionally-shifted register value and writes the result to the result to the destination register. It can optionally update the condition flags based on the result.
        - `SUBS <Rd>, <Rn>, <Rm>`
        - `SUB<c> <Rd>, <Rn>, <Rm>`
```
.global _start
_start:
        mov r0, #4
        mov r2, #2
        sub r1, r0, r2
        // r0 : 00000004
        // r1 : 00000002
        // r2 : 00000002
        // pc : 0000000c
```
- SUB (immediate, ARM)
        - This instruction subtracts an immediate value from a register value and writes the result to the result to the destination register. It can optionally update the condition flags based on the result.
        - `SUB(S)<c> <Rd>, <Rn>, #<const>`
```
.global _start
_start:
        mov r0, #4
        // r1 = r0 - 3
        sub r1, r0, #3
        // r0 : 00000004
        // r1 : 00000001
        // pc : 00000008
```
# MUL
- work only on registers there is no immediate operation to it
- Multiplies two register values. The least significant 32 bits of the result are written to the destination register
- These 32 bits do not depend on whether the source register values are considered to be signed values or unsigned values
- `MULS <Rdm>, <Rn>, <Rdm>`
- `MUL<c> <Rdm>, <Rn>, <Rdm>`
```
.global _start
_start:
        mov r0, #4
        mov r2, #2
        mul r1, r0, r2
        // r0 : 00000004
        // r1 : 00000008
        // r2 : 00000002
        // pc : 0000000c
```

# CPSR
- Current Program Status Register
- It holds many different values that represent the current state of the running program
- The bits in the CPSR register comprise of different flags that are updated or  when certain conditions occur.

![[cspr.png]]

- Condition flags, `bits[31:28]` : Set on the result of instruction execution. The flags are:
        - N, `bit[31]` : Negative condition flag
                - if we do operation of two registers together and result is a negative value this flag is set to 1 or true
        - Z, `bit[30]` : Zero condition flag
                - if we do operation of  two registers together and result is a zero then this flag is set to 1 or true
        - C, `bit[29]` : Carry condition flag
                - if we do operation of  two registers together and result generates a carry then this flag is set to 1 or true
        - V, `bit[28]` :  Overflow condition flag.
                - if we do operation of  two registers together and result overflows the limit of destination register then this flag is set to 1 or true

## the `<s>`
- the `<s>` in the instruction represents that the instruction is capable of manipulating the cspr register.
- Example `add r1,r0,r2` -> cannot manipulate the cspr register; `adds r1,r0,r2` can
```
.global _start
_start:
        mov r0, #3
        mov r1, #3
        subs r1, r0, r2
        // r0 : 00000003
        // r1 : 00000003
        // r2 : 00000000
        // pc : 0000000c
        // cpsr : 600001d3
        //600001d3 in hex
        //01100000000000000000000111010011 in binary
        // sets Z and C flag to 1.

```
- Note :-  ARMv7 regiters are 32 bits

# Load-Store architecture
- ARM uses a load-store architecture. Only load and store instructions can access memory

## Segment
- `.data`
        - It holds any kind of complex data that is not going to be stored directly inside of a register (eg array, string)
- `.text`
        - It holds the underlying code that is going to be executed on the device
## LDR
- `LDR (register,ARM)`
        - Load Register (register) calculates an address from a base register value and an offset register value, loads a word from memory and writes it to register.
        - The offset register value can optionally be shifted
- `LDR<c> <Rt>, [<Rn>,+/-<Rm>{,<shift>}](!)`
- `LDR<c> <Rt>, [<Rn>],+/-<Rm>{,<shift>}`

```
.global _start

.text
_start:
        ldr r0, =var1 //put the memory address (location) into register r0 of var1

.data
// 2 byte or 16 bits word data type
var1: .word 5 
var2: .word 6

// r0 : 00000008 -> It is 8 because 5 is stored at memory address 8
// pc : 00000004
```

- Memory
`|Address|Memory Address 0|Memory Address 4|Memory Address 8|Memory Address 12|`

![[mem.png]]

----

- Using LDR with brackets load the actual value similar to dereferencing a pointer
- It tracks memory address that currently in r0 and actually get the value it is pointing to
- 


```
.global _start

.text
_start:
        ldr r0, =var1 
        ldr r1, [r0]

.data
// 2 byte or 16 bits word data type
var1: .word 5 
var2: .word 6

// r0 : 000000010 -> Value can change not fixed
// r1 : 00000005
// pc : 00000008
```


- Memory
![[memldr.png]]

## STR
- `STR (register)`
	- Store Register (register) calculates an address from a base register value and an offset register value, stores a word from a register to a memory
	- The offset register value can optionally be shifted.
- `STR<c> <Rt>, [<Rn>, <Rm>]`

```
.global _start

.text
_start:
        ldr r0, =var1 
		ldr r1, [r0]
		mov r2, #3 // acts as source register, stores value that we actaully want to replace it with.
		ldr r3, =var2 // acts as destiantion or base register
		// loading memory location that is currently storing var2 in r3
		str r2, [r3] // In that memory location we will replace 6 with 3

.data
var1: .word 5 
var2: .word 6

// r0 : 00000020
// r1 : 00000005
// r2 : 00000003
// r3 : 00000024
// pc : 00000014
```

Memory before
![[strmem-before.png]]

Memory after
![[strmem-after.png]]

# Logical Operator
## AND
- `AND(immediate)`
	- Boolean AND returns true when both inputs are true.
	- This instruction performs a bitwise AND of a register value and an immediate value and writes the result to the destination register
- `AND{S}<c> <Rd>, <Rn>, #<const>`

```
.global _start
_start:
	
	mov r0, #0x42
	and r1, r0, #0x16
// 00000042 -> 00000000000000000000000001000010
// 00000016 -> 00000000000000000000000000010110
//------------------------
// 00000002 <- 00000000000000000000000000000010
// r0 : 00000042 
// r1 : 00000002
// pc : 00000008
```
## ORR
- `ORR(immediate)`
	-  Boolean OR returns true when at least one input is true.
	- Bitwise OR (immediate) performs a bitwise (inclusive) OR of a register value and an immediate value and writes the result to the destination register 
	- It can optionally update the condition flags based on the result
- `ORR{S}<c> <Rd>, <Rn>, #<const>`

```
.global _start
_start:
	
	mov r0, #0x42
	orr r1, r0, #0x26
// 00000042 -> 00000000000000000000000001000010
// 00000016 -> 00000000000000000000000000100110
//------------------------
// 00000066 <- 00000000000000000000000001100110
// r0 : 00000042 
// r1 : 00000066
// pc : 00000008
```

- `EOR(immediate)`
	- Exclusive OR returns true if one input is true and other one is false
	- Bitwise Exclusive OR (immediate) performs a bitwise Exclusive OR of a register value and an immediate value and write the result to the destination register.
	- It can optionally update the condition flags based on the result
- `EOR{S}<c> <Rd>, <Rn>, #<const>`

```
.global _start
_start:
	
	mov r0, #0x42
	orr r1, r0, #0x26
// 00000042 -> 00000000000000000000000001000010
// 00000016 -> 00000000000000000000000000100110
//------------------------
// 00000064 <- 00000000000000000000000001100100
// r0 : 00000042 
// r1 : 00000064
// pc : 00000008
```

## MVN
- `MVN(register)`
	-  Bitwise NOT (register) writes the bitwise inverse of a register value to the destination register. 
	- It can optionally update the condition flag based on the result
- `MVNS <Rd>, <Rm>`
- `MVN<c> <Rd>, <Rm>`

```
.global _start
_start:
	
	mov r0, #0x42
	mvn r1, r0
// 00000042 -> 00000000000000000000000001000010
//------------------------
// ffffffbd <- 11111111111111111111111110111101
// r0 : 00000042 
// r1 : ffffffbd
// pc : 00000008
```

# ARM Processor data types and Arithematic
- ARMv7-M processors support the following data types in memory
	- Byte -> 8 bits
	- Halfword -> 16 bits
	- Word -> 32 bits
- Why is this called 32 bits architecture
	- Each of the processor registers hold 8 different zeros so these are actually representing a hexadecimal number so each one of these zeros individually is actually 4 bits of data and that equates to total of 32 bits. 
	- 4 bits of data for these individual hexadecimal is called nibble. 4 bits is a nibble, a nibble of 8 bits is a byte.
## Signed and Unsigned Numbers

- Computers use two's complement to represent signed numbers
- We actually have one bit that represents the actual sign of the number . 1 stands for negative and 0 stands for positive 
	- All the bits on the left hand side of the number are actually going to be padded with the same value of that individual signed bit.
- To convert to positive we flip all bit except the least significant bit (LSB) that is non-zero and all bits to its right

![[Unisigned.png]]

```
Hex : 0-9, A:10, B:11, C:12, D:13, E:14, F:15
n   :  5, 4,3,2,1,0
2^n : 32,16,8,4,2,1

Hex : 00000019
Binary: 0000 0000 0000 0000 0000 0000 0001 1001
Decimal: 1+8+16 = 25
Hex : ffffffe7
Binary:        1111 1111 1111 1111 1111 1111 1110 0111
2s Compliment: 0000 0000 0000 0000 0000 0000 0001 1001
Decimal: 1+8+16 = 25; we did compliment hence -25 

Hex : 00000010
Binary: 0000 0000 0000 0000 0000 0000 0001 0000
Decimal: 16
Hex : fffffff0 
Binary:        1111 1111 1111 1111 1111 1111 1111 0000
2s Compliment: 0000 0000 0000 0000 0000 0000 0001 0000
Decimal: 16; we did compliment hence -16 
```
# Shift and Rotate
- Shift and rotate amounts can be specified by register or immediate values
## LSL
- Stands for Logical Shift Left
- `LSL(Immediate)` 
	- Logical Shift Left (immediate) shifts a register value left by an immediate number of bits, shifting in zeroes, and writes the result to the destination register
	- It can optionally update the condition flag based on the result
- `LSLS <Rd>,<Rm>,#<imm5>`
- `LSL<c> <Rd>,<Rm>,#imm<5>`
- We have a number inside our register operand `<Rm>`; The number specified in `#<imm5>` is going to be number of times that all of the bits inside of the register  operand are shift to the left, and shifted result is stored in `<Rd>` 

