# Pufi // Issues

In this document, we use acronyms to identify the issues:

- Pufi Assembly Issue (PAI)
- Pufi Script Issue (PSI)
- Pufi Runtime Issue (PRI)

## Commenting

You can open a Issue with the title following the pattern "$ACRONYM: $COMMENT" to comment about some issue, remember you can detail your comment using the Issue body, the comment in the title is only an one phrase resume.

## PAI-0000: Function Declaration

Pufi is a programming language to be used in other projects that allows the users create plugins or mods using a scripting language like Lua, Python, and JavaScript, with the difference that the implementation is easiest and the runtime executables is smaller. The problem is in planning something that can reach these objectives.

**Related:** *write about memory representation of structs and arrays*.

### Option A - Dual bytecode

The solution is to use something like in C/C++ but in bytecode-level, having one bytecode to be used in compilation-only and other to be used in runtime-only, if Pufi Assembly/Bytecode becomes RISC, this will let the compilers implement your own way of compiling while being compatible with the code produced by the other compilers because the format of compile-only. This will also reduce the size of the redistribution file because will haven't the development data.

**Problem #1:** If we follow this format we will have problems with runtime calls, in almost all languages we can use a string to call some function, even in C with dynamic linkage we can do this but will not be possible in Pufi because the function names will be stored in the compilation-only bytecode.

**Problem #2:** Another problem is how the Runtime will call the functions but this problem is better described in  the **PAI-0001**.

### Option B - Function header

Another way is to use a header before the instructions with the function name and it's arguments, with a number to the runtime can seek to the instructions, this is will solve the problems from **Option A** but will add some extra bytes to the bytecode, but we hope that this will not be a problem.

## PAI-0001: Module Import and Function Call

In Java and Rust, the code is separated in modules/packages and in Pufi something similar will be used, but how this will be handle is the problem, we have the constraint of not use strings to avoid the need to search and compare strings until found the function, we want something more direct.

### Option A - Static Linking

We can use an approach similar to C/C++, creating a intermediate file (equivalent to the object file with extension .o) to the compiler process (source -> intermediate -> executable), in the intermediate stage we will have a file to each module compiled that will have all your strings representing a function be replaced to the it address in the executable file during the final compilation stage. In other words, make the executable file a function array that use indexes as address to search the functions.

**Problem #01:** The libraries used by a project will be only usable if they are merged in the executable file, making almost impossible of share libraries between projects like C/C++, Java, JavaScript, and other languages do, becoming something like Rust and Go.

### Option B - Address Table

In the top of the bytecode we can have a table containing a association between a index and a string that represents the function, when the bytecode is loaded, the runtime will construct a table that converts these index to the address of the module and function in the runtime memory.

**Problem #02:** We will need to measure the impact of storing the table and the function searching.

### Option C - Ignore the constraint

We can only ignore the constraint and something like Java that stores the package, function, and classes as strings in the bytecode, this isn't what we want but can't discarded as an option and need to be tested.
