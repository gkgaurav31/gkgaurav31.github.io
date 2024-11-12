---
layout: post
title: Reverse Engineering 101
date: 2024-11-12 10:21 +0530
author: "Gaurav Kumar"
tags: "clean_code vulnerability theory important"
categories: "clean_code"
---

## GETTING STARTED WITH REVERSE ENGINEERING

### ASSEMBLY BASICS

#### CPU ARCHITECTURE: REGISTERS, MEMORY AND MACHINE CODE

##### A GENERAL FLOW OF PROGRAM EXECUTION

- Program on Disk (EXE): The executable file (.exe) is stored on disk.
- OS Loader: The operating system loads the program from the disk into the primary memory.
- Primary Memory (RAM): Here, the program is loaded as binary data.
- CPU (Fetch-Decode-Execute):
- Fetch: The CPU fetches instructions from primary memory.
- Decode: The CPU decodes the instructions.
- Execute: The CPU performs the instructions.
- Output: The processed data is sent to the screen or other output devices.

##### LAYERS OF MEMORY

1. CPU Registers
2. Caches
3. Primary Memory
4. Secondary Storage
5. Input Devices

##### PROCESSOR REGISTERS

8 General Purpose Registers for a 32 bit system:

| Register Name | Description                                                   |
| ------------- | ------------------------------------------------------------- |
| **EAX**       | Accumulator register, used in arithmetic and logic operations |
| **EBX**       | Base register, typically used for addressing                  |
| **ECX**       | Counter register, used in loops and string operations         |
| **EDX**       | Data register, often used in arithmetic and I/O operations    |
| **ESI**       | Source index, used in string and array operations             |
| **EDI**       | Destination index, used in string and array operations        |
| **EBP**       | Base pointer, used to point to the base of the stack frame    |
| **ESP**       | Stack pointer, points to the top of the stack                 |

For most of these register, we can access its upper/lower bits:

| Register Name | Lower 16 bits | Lower 8 bits | Upper 8 bits |
| ------------- | ------------- | ------------ | ------------ |
| **EAX**       | AX            | AL           | AH           |
| **EBX**       | BX            | BL           | BH           |
| **ECX**       | CX            | CL           | CH           |
| **EDX**       | DX            | DL           | DH           |
| **ESI**       | SI            | -            | -            |
| **EDI**       | DI            | -            | -            |
| **EBP**       | BP            | -            | -            |
| **ESP**       | SP            | -            | -            |

- **EAX, EBX, ECX, EDX**: These registers can be split into 16-bit, 8-bit, and 8-bit upper parts.

  - **Lower 16 bits**: For example, `AX` in `EAX`.
  - **Lower 8 bits**: For example, `AL` in `EAX`.
  - **Upper 8 bits**: For example, `AH` in `EAX`.

- **ESI, EDI, EBP, ESP**: These registers are generally 32-bit, but they don’t have separate access to upper and lower 8-bit parts. They can be accessed in 16-bit form (like `SI`, `DI`, `BP`, `SP`), but they don't have the specific 8-bit "upper" and "lower" divisions.

Similarly for 64 bit systems:

| 64 bits | 32 bits | Lower 16 bits | Lower 8 bits | Upper 8 bits |
| ------- | ------- | ------------- | ------------ | ------------ |
| RAX     | EAX     | AX            | AL           | AH           |
| RBX     | EBX     | BX            | BL           | BH           |
| RCX     | ECX     | CX            | CL           | CH           |
| RDX     | EDX     | DX            | DL           | DH           |
| RSI     | ESI     | SI            | -            | -            |
| RDI     | EDI     | DI            | -            | -            |
| RBP     | EBP     | BP            | -            | -            |
| RSP     | ESP     | SP            | -            | -            |
| R8-R15  | R8D     | R8W           | R8B          | -            |

**Pointer Registers:**

32-bit Pointer Registers (x86)

- EIP: Extended Instruction Pointer (32 bits) – Holds the address of the next instruction.
- ESP: Extended Stack Pointer (32 bits) – Points to the top of the stack.
- EBP: Extended Base Pointer (32 bits) – References local variables and function parameters on the stack.

64-bit Pointer Registers (x86-64)

- RIP: Register Instruction Pointer (64 bits) – Holds the address of the next instruction.
- RSP: Register Stack Pointer (64 bits) – Points to the top of the stack.
- RBP: Register Base Pointer (64 bits) – References local variables and function parameters on the stack.

**Index Registers:**

32-bit Index Registers (x86)

- ESI: Extended Source Index (32 bits) – Commonly used as a source index in string operations.
- EDI: Extended Destination Index (32 bits) – Commonly used as a destination index in string operations.

64-bit Index Registers (x86-64)

- RSI: Register Source Index (64 bits) – Commonly used as a source index in string operations.
- RDI: Register Destination Index (64 bits) – Commonly used as a destination index in string operations.

> These are commonly used for copy operations.

##### EFLAGS & RFLAGS

EFLAGS (32-bit) & RFLAGS (64-bit): These registers represent the results of operations and the state of the CPU. They store flags that indicate the status of various conditions, such as arithmetic results and control flags.

- CF (Carry Flag): Indicates an overflow or carry out of the most significant bit during arithmetic operations.
- ZF (Zero Flag): Set if the result of an operation is zero.
- SF (Sign Flag): Indicates the sign of the result (set if the result is negative).
- TF (Trap Flag): Enables single-step debugging, causing the processor to stop after each instruction.
- DF (Direction Flag): Controls the direction of string operations (whether to increment or decrement the index registers).
- OF (Overflow Flag): Set if the result of an operation overflows the destination register.

> The RFLAGS register in 64-bit mode includes a 64-bit space, but the upper 32 bits of RFLAGS are reserved and not used by the processor.

#### THE ASSEMBLER AND PROGRAM SEGMENTS

Binary File Formats

- .exe file: Executable file format used in Windows systems.
  - Headers: Contain metadata about the file, including information on how to load and execute the program.
  - Sections: Different parts of the executable file.
    - Code (.text): Contains the program's instructions in binary form; it is the entry point of the program.
    - Imports/Exports: Lists functions or data imported from or exported to other programs or libraries.
    - Read-only data: Stores constant or immutable data used by the program.
    - Resources: Includes icons, images, and other binary data embedded in the program.
    - Other sections: Any additional sections such as debug or metadata.

Segment Purposes

- .text/.code: Contains the program's instructions in binary form; the entry point for execution.
- .data/.idata: Holds initialized data (data that has been assigned a value at compile time).
- .rsrc: Stores resources like icons, images, and arbitrary binary data used by the program.
- .bss: Stores uninitialized data (variables that do not have an initial value).

NASM Assembly Code Example:

```nasm
[bits 32]                ; Defines 32-bit architecture

section .text            ; Defines the section for code
global _START            ; Defines the entry point of the program

_START:                  ; Entry point label
    push ebp             ; instruction in NASM syntax
    mov esp, ebp
    sub esp, 4
```

### INSTRUCTION SET ARCHITECTURE (ISA)

- Defines the instructions that a CPU understands.
- The ISA is essentially the machine code in its binary form.
- **Compiler/Assembler/Interpreter**: These tools translate source code into the appropriate machine code.
- **Disassemblers**: In reverse engineering, disassemblers revert machine code back to assembly language.
- **Decompilers**: Decompilers may attempt to generate the original source code from machine code or assembly.
