---
layout: post
title: Secure Coding in C and C++ (Part 2)
date: 2024-11-11 19:50 +0530
author: "Gaurav Kumar"
tags: "c c++ theory important"
categories: "clean_code"
---

## SECURE CODING IN C AND C++ (Part 2)

### NEVER USE GETS

Never use `gets()`. Because it is impossible to tell without knowing the data in advance how many characters `gets()` will read, and because `gets()` will continue to store characters past the end of the buffer, it is extremely dangerous to use. It has been used to break computer security. Use `fgets()` instead.

For more information, see CWE-242 (aka "Use of Inherently Dangerous Function") at [http://cwe.mitre.org/data/definitions/242.html](http://cwe.mitre.org/data/definitions/242.html)

```c
#include<stdio.h>
#include<string.h>

int main(){

    char buffer[16];
    printf("Enter some data.\n");

    gets(buffer); // dangerous

    puts(buffer);

}
```

### file COMMAND

The file a.out (in this example) is a 64-bit executable designed for Linux systems using the x86-64 architecture. It is a shared object (dynamic library) that relies on other libraries to run. The file is dynamically linked, meaning it loads necessary libraries when executed. It uses the system's default dynamic linker located at /lib64/ld-linux-x86-64.so.2. The file is not stripped, so it includes extra debugging information to help developers.

```bash
file a.out
a.out: ELF 64-bit LSB shared object, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, BuildID[sha1]=c582a65ab5e05c2a46cba44757820a2515627ab0, for GNU/Linux 3.2.0, not stripped
```

### DISABLING PROTECTION DURING GCC COMPILATION

```bash
gcc vuln.c -o vuln -fno-stack-protector -z execstack -no-pie -m64
```

Here's what each part of this command does:

- `-fno-stack-protector`  
  Disables "stack protection," a security feature that detects changes to the stack (where functions store temporary data).

  Turning this off makes the program more vulnerable to buffer overflow attacks.

- `-z execstack`  
  Allows the stack to execute code, which is typically disallowed to prevent certain types of attacks.
- `-no-pie`  
  Disables "Position Independent Executable" (PIE), which normally randomizes where code runs in memory to make attacks more difficult.

- `-m64`  
  Compiles the program as a 64-bit executable.

Check Security:

`checksec --file=vuln`

| Feature          | Status          | Explanation                                                                                                                                                       |
| ---------------- | --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **RELRO**        | Partial RELRO   | Relocation Read-Only (RELRO) provides protection against certain types of memory corruption. "Partial" means some protection is applied.                          |
| **STACK CANARY** | No canary found | Stack canaries are used to detect buffer overflow attacks. "No canary" means no protection is in place.                                                           |
| **NX**           | NX disabled     | No eXecute (NX) prevents execution of code from non-executable memory regions. "Disabled" means it's not in use.                                                  |
| **PIE**          | No PIE          | Position Independent Executable (PIE) randomizes the memory address of the executable to make attacks harder. "No PIE" means this protection is not enabled.      |
| **RPATH**        | No RPATH        | RPATH specifies where shared libraries should be searched for. "No RPATH" means there is no custom search path.                                                   |
| **RUNPATH**      | No RUNPATH      | Similar to RPATH but only used at runtime. "No RUNPATH" means there is no custom runtime search path.                                                             |
| **Symbols**      | 64 Symbols      | The number of symbols (functions, variables) defined in the executable. 64 symbols are present.                                                                   |
| **FORTIFY**      | No              | FORTIFY_SOURCE is a security feature that helps prevent buffer overflows by replacing certain library functions with safer versions. "No" means it’s not enabled. |
| **Fortified**    | 0               | Indicates the number of functions that have been fortified. 0 means no functions are fortified.                                                                   |
| **Fortifiable**  | 1               | Indicates the number of functions that can be fortified. 1 means one function could be fortified.                                                                 |
| **FILE**         | vuln            | The name of the file being checked. In this case, it’s `vuln`.                                                                                                    |
