<!---
{
  "id": "cc424af3-2bd7-491f-b678-89e18904bf58",
  "depends_on": [
    "AND",
    "61354751-6887-4761-9ef0-ca25d237cf1c",
    "800c7dd9-5ccf-42c1-b9ea-c2764579cf5d"
  ],
  "author": "Stephan Bökelmann",
  "first_used": "2025-06-12",
  "keywords": ["C", "control flow", "if", "while", "for", "objdump", "jmp"]
}
--->

# Introduction to Control Flow in C

> In this exercise you will learn how to use control flow statements in C — specifically `goto`, `if`, `while`, and `for`. Furthermore, we will explore how these constructs translate into jump instructions (`jmp`, `je`, `jne`, etc.) in assembly by compiling with `-c` and inspecting the resulting object file using `objdump`.

## Introduction

Control flow is how programs make decisions and repeat operations. In C, this is done using constructs like `if`, `while`, and `for`, which are essential for branching logic and loops. Internally, these structures are compiled down to jump instructions that control the CPU's program counter.

To bridge the gap between high-level syntax and low-level execution, you will:

* Compile your C source to an object file with `gcc -c`.
* Disassemble the `.o` file using `objdump -d`.
* Observe how control flow constructs translate into assembly.

Additionally, compile and run your programs to verify their runtime behavior:

```sh
gcc -o main main.c
./main
```

This combination of runtime testing and low-level inspection builds a robust understanding of how control structures operate.

### Further Readings and Other Sources

* [cppreference: if](https://en.cppreference.com/w/c/language/if)
* [cppreference: while](https://en.cppreference.com/w/c/language/while)
* [cppreference: for](https://en.cppreference.com/w/c/language/for)
* [man page: objdump](https://linux.die.net/man/1/objdump)

## Tasks

### Infinite Loop with `while (1)`

Use an unconditional loop to demonstrate how a `while` with a constant condition is compiled.

```c
int main() {
    while (1) {
        printf("Looping...\n");
    }
    return 0;
}
```

**Inspect**: Locate the jump that loops back. It should be an unconditional backward `jmp`.

### Basic Conditional `if`

Introduce data-dependent branching.

```c
int main() {
    int a = 3;
    if (a > 0) {
        printf("Positive\n");
    }
    return 0;
}
```

**Inspect**: You should see a `cmp` instruction followed by a conditional jump like `jle` (jump if less or equal). The jump skips over the `printf` if the condition is false.

### Bounded Loop with `while`

Create a counter loop.

```c
int main() {
    int i = 0;
    while (i < 5) {
        printf("i = %d\n", i);
        i++;
    }
    return 0;
}
```

**Inspect**: There should be a conditional jump at the beginning of the loop and a backward jump at the end. Identify where the counter variable is compared.

### Counted Loop with `for`

Rewrite the same logic using `for` syntax.

```c
int main() {
    for (int i = 0; i < 5; i++) {
        printf("i = %d\n", i);
    }
    return 0;
}
```

**Inspect**: Compare this to the `while` loop. Find where initialization, comparison, increment, and jumps happen.

### Loop with Nested Condition

Demonstrate combined conditional and loop behavior.

```c
int main() {
    for (int i = 0; i < 10; i++) {
        if (i % 2 == 0) {
            printf("even: %d\n", i);
        }
    }
    return 0;
}
```

**Inspect**: There should be two sets of jumps — one for the loop and another for the `if`. Find the modulus operation and its conditional branch.

## Questions

1. What is the difference in jump behavior between `goto` and a loop?
2. How do you recognize conditional branches in the disassembled output?
3. How is the increment operation positioned in a `for` loop versus a `while`?
4. Can you reorder your `if` logic and observe different jump outcomes?

## Advice

Every jump in assembly corresponds to a decision or repetition in your code. Use `objdump -d` as a lens into the machine's behavior. Combining source-level thinking with disassembly sharpens your understanding of both performance and logic. Be curious — modify, recompile, and observe what changes.
