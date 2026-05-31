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

![[Pasted image 20260410191659.png]]

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

![[Pasted image 20260526042705.png]]

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
        ldr r0, =var1
        ldr r1 ,[r0]

.data
// 2 byte or 16 bits word data type
var1: .word 5
var2: .word 6

```
