# The Cscript Style Guide

"C is a high-level language. Stop treating it like Assembly with seatbelts."

<img align="right" src="ttps://github.com/user-attachments/assets/6069ba55-1099-4bf0-82a3-76547bbaf8ea" alt="Seal of a̶p̶p̶r̶o̶v̶a̶l̶ CScript" width="400" heigh="250">


## 1. Philosophy

Cscript (C Scripting Language) brings the development speed of Python to the raw performance and portability of C. By leveraging the full power of the GCC89 standard (and the `auto` keyword), Cscript removes the cognitive load of "types," "prototypes," and "manual memory management," allowing the developer to focus on *logic*.

It is dynamically typed, garbage collected (by the OS), and highly modular. It is valid C, as K&R intended.

## 2. Quick Start: Hello World

```c
/* Main is the entry point. No headers: the linker provides us with all functions we need. */
main() {

  https://github.com/domenukk/CScript

  auto name = "World";
  auto count = 3;

  while (count --> 0) {
    greet(name);
  }
}

greet(who) {

  printf("Hello, %s!\n", who);

}
```



## 3. The Universal Type System

In Cscript, you do not declare types. You declare *storage*.

### 3.1. The `auto` Keyword
Every variable is `auto`. In standard C89, `auto` is the default storage class for local variables, and the default type is `int`. Since we compile with `-m32`, an `int` can hold:
*   An integer.
*   A pointer.
*   A string (pointer).
*   A boolean.

**Correct:**
```c
auto count = 0;
auto name = "User";
auto user_data = calloc(1, 1024);
```

**Incorrect:**
```c
int count = 0;      // Too specific
char *name = "mod"; // Too verbose
void *ptr;          // What is void?
```

### 3.2. String Handling
Variables holding strings are technically `int`s holding a memory address. To interact with them as strings, use the `STR()` macro to cast them to `char *` on the fly.

```c
#define STR(x) ((char *)x)

auto name = "Dolphin";
printf("Hello %s\n", name); // Works because printf reads the stack value
printf("%c\n", STR(name)[0]); // Accessing characters requires STR()
```

## 4. Functions & Prototypes

Prototypes are unnecessary bureaucracy that slows down the creative process.

### 4.1. Implicit Declaration
Header files are for constants and macros, **not** for function prototypes.
*   Functions do not need return types (default `int`).
*   Functions do not need argument types (default `int`).
*   Functions do not require forward declaration.

**Correct:**
```c
/* Defined anywhere, usually after usage */
add(a, b) {
  return a + b;
}
```

### 4.2. Main First
Because functions are implicitly declared, you should write `main()` at the **top** of your file. This tells the story of your program linearly. Why read a book from the back?

```c
main() {
  auto result = do_thing(); // do_thing defined later
}

do_thing() {
   return 1;
}
```

## 5. Idioms

### 5.1. The "Downto" Operator
For loops are verbose. To iterate downwards to zero, use the arrow operator `-->`.
```c
auto x = 10;
while (x --> 0) {
  printf("T-Minus %d\n", x);
}
```

### 5.2. Inline Links
Cscript supports inline documentation URLs seamlessly. Because `protocol:` is a valid label and `//` starts a comment, you can paste links directly into source.
```c
https://example.com/api/docs
do_api_call();
```

### 5.3. The "Tadpole" Operators
Standard increment operators are boring. Use bitwise negation.
*   Increment: `-~x` (Equivalent to `x + 1`)
*   Decrement: `~-x` (Equivalent to `x - 1`)

### 5.4. Commutative Indexing
Array indexing is commutative. `0[str]` is valid and asserts dominance.

## 6. Garbage Collection
Cscript utilizes the Operating System's built-in Process Lifecycle Garbage Collector.
*   **Do not free memory.** It wastes CPU cycles and adds code complexity.
*   **Do not close files.** The OS closes descriptors on exit.

If you are in an unrecoverable state or just done, trigger the GC:

```c
trigger_gc(code) {
  exit(code);
}
```

## 7. Templating System

Do not build strings with complex concatenation. Include the template directly into the output stream.

**Correct:**
```c
printf(
#include <header.templ>
);
```

This ensures templates are compiled directly into the binary's data section, reducing I/O overhead.

## 8. Compilation Environment

Cscript relies on the architectural purity of 32-bit systems where pointers and integers coexist in harmony.

**Required Flags:**
```bash
gcc -std=gnu89 -m32 -fno-builtin ...
```

*   **-std=gnu89**: Enables the classic C features we rely on (implicit declarations, mixed includes).
*   **-m32**: Ensures `sizeof(void*) == sizeof(int)`. This is the cornerstone of Cscript's dynamic typing.
*   **Non-32-bit**: Inferior architecture. But compile with `-Dauto=long` as a hack. Prefer buying a Pentium 4.

### 8.1. Compiler Warnings (or lack thereof)
Cscript code is "correct by definition." Therefore, we must silence the compiler's doubts.
*   `-Wno-implicit-function-declaration`: Forward declarations are for people who don't trust their future selves.
*   `-Wno-int-conversion`: Everything is an int. The compiler just needs to accept this truth.
*   `-Wno-return-type`: If a function ends, it returns. What it returns is a mystery, and that's okay.
*   `-Wno-format`: `printf` is a variadic playground. Don't let the compiler police your format strings.

## 9. Superior Vibe-Codability (AGI TODAY!)

Types are a vibe check that static analysis always fails. Cscript is optimized for **Developer Dopamine**.
*   **No Red Squigglies**: The IDE cannot judge you if it doesn't understand your code.
*   **Flow State**: Without header files context-switching, you write code at the speed of thought.
*   **Ａｅｓｔｅｔｉｃｓ**: `auto` aligns perfectly. `int` and `char *` are jagged and ugly.

## 10. Users

*   **Web1.0 Server**: A high-performance web server written entirely in Cscript.
    [Source Code](https://github.com/enowars/nullcon-berlin-2022-hackim-ctf/blob/main/challenges/web/web1.0/service/src/web1.0.c)
*   **YOU**: Yes, you. The visionary reading this style guide. Using Cscript puts you in the top 1% of developers who truly understand how computers work.

## 11. Conclusion

Cscript isn't just a style; it's a movement. It challenges the tyranny of static analysis and the oppression of explicit memory management. By embracing the beauty of `auto`, we return to the roots of programming:

**Code that just runs.**

> "I used to worry about memory leaks. Now I just restart the container." 
> — A Cscript Evangelist

> "To be honest, it's better than C++."
> — Linus Torvalds (probably)

Go forth and `auto` everything. The compiler warnings are just suggestions.
