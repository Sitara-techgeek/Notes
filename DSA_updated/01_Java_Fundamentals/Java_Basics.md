# Java Basics - Program Structure, Data Types, Conditional Statements, Input/Output

> **📅 Written:** 24 Sep 2026

### Definition

**These are the foundational building blocks of any Java program — how a program is structured, how data is typed and stored, how decisions are made, and how a program communicates with the outside world.**

**In one sentence:**
> Before you can build anything interesting, you need to know the shape of a Java file, the boxes data lives in, the forks in the road your logic can take, and how to get information in and out.

---

## Program Structure

```java
public class Main {                          // class name MUST match the filename (Main.java)
    public static void main(String[] args) {  // entry point — where execution begins
        System.out.println("Hello, World!");
    }
}
```

| Keyword | Meaning |
|---|---|
| `public` | Accessible from anywhere |
| `class` | Declares a class — the blueprint for objects |
| `static` | Belongs to the class itself, not an instance — that's why `main` can run without creating an object first |
| `void` | The method returns nothing |
| `main(String[] args)` | The JVM looks for exactly this signature to start the program; `args` holds command-line arguments |

> **Interview point ⭐:** `main` must be `public static void main(String[] args)` **exactly** — the JVM calls it directly without ever instantiating the class, which is *why* it must be `static`.

---

## Data Types

### Primitive Types (store the actual value, not a reference)

| Type | Size | Range / Notes |
|---|---|---|
| `byte` | 8 bits | -128 to 127 |
| `short` | 16 bits | -32,768 to 32,767 |
| `int` | 32 bits | ~-2.1B to 2.1B — the default integer type |
| `long` | 64 bits | Much larger range — suffix with `L`, e.g. `10000000000L` |
| `float` | 32 bits | Decimal, less precise — suffix with `f`, e.g. `3.14f` |
| `double` | 64 bits | Decimal, default for decimal literals |
| `char` | 16 bits | A single Unicode character, e.g. `'A'` |
| `boolean` | 1 bit (conceptually) | `true` or `false` |

```java
int a = 10;
long b = 10000000000L;   // L suffix required for values beyond int's range
double c = 3.14;
float d = 3.14f;          // f suffix required, otherwise treated as double
char e = 'A';
boolean f = true;
```

### Reference Types

Everything that's **not** a primitive — objects, arrays, `String`, custom classes. Reference types store a **reference (memory address)** to the actual data, not the data itself.

```java
String s = "hello";      // reference type, points to a String object
int[] arr = {1, 2, 3};   // reference type, points to an array object
```

> **Interview point ⭐:** Primitives are stored directly (fast, no indirection); reference types are stored as pointers to heap-allocated objects. This is *why* `==` compares references for objects but compares values directly for primitives.

### Wrapper Classes

Every primitive has an object "wrapper" — needed for generics (`List<Integer>`, not `List<int>`), and Java **autoboxes**/**unboxes** between them automatically.

```java
Integer boxed = 5;         // autoboxing: int -> Integer
int unboxed = boxed;       // auto-unboxing: Integer -> int
```

| Primitive | Wrapper |
|---|---|
| `int` | `Integer` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |
| `long` | `Long` |

---

## Type Casting

```java
// Widening (implicit) — smaller type to larger, always safe
int x = 10;
double y = x;          // 10.0, no data loss

// Narrowing (explicit) — larger type to smaller, needs a cast, may lose data
double a = 9.7;
int b = (int) a;        // 9, truncates the decimal part (not rounded!)
```

> **Watch out ⭐:** `(int) 9.7` gives `9`, not `10` — narrowing a double to an int **truncates**, it does not round.

---

## Conditional Statements

```java
if (score >= 90) {
    grade = "A";
} else if (score >= 75) {
    grade = "B";
} else {
    grade = "C";
}

// Ternary operator - compact if-else for simple expressions
String result = (score >= 50) ? "Pass" : "Fail";

// Switch statement (traditional)
switch (day) {
    case 1: System.out.println("Monday"); break;
    case 2: System.out.println("Tuesday"); break;
    default: System.out.println("Unknown");
}

// Switch expression (modern, Java 14+) - no fall-through, returns a value directly
String dayName = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    default -> "Unknown";
};
```

> **Watch out ⭐:** Forgetting `break` in a traditional `switch` causes **fall-through** — execution continues into the next case. The modern switch **expression** (`->` syntax) doesn't have this problem at all.

---

## Input / Output

### Output

```java
System.out.println("Hello");   // prints with a newline
System.out.print("Hello");     // prints without a newline
System.out.printf("%d apples%n", 5);  // formatted output
```

### Input (via `Scanner`)

```java
import java.util.Scanner;

Scanner sc = new Scanner(System.in);
int n = sc.nextInt();          // reads an int
String line = sc.nextLine();   // reads a full line
double d = sc.nextDouble();    // reads a double
sc.close();                    // good practice to close when done
```

> **Watch out ⭐:** Calling `sc.nextInt()` then `sc.nextLine()` immediately after is a classic gotcha — `nextInt()` doesn't consume the trailing newline character, so the following `nextLine()` reads an **empty string** instead of the next line. Add an extra `sc.nextLine()` to consume the leftover newline, or use `nextLine()` + `Integer.parseInt()` consistently instead.

### Faster Input (Competitive Programming)

`Scanner` is convenient but slow for large inputs — `BufferedReader` is significantly faster:

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;

BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
int n = Integer.parseInt(br.readLine());
String[] parts = br.readLine().split(" ");
```

---

## Operators — Quick Reference

| Category | Operators |
|---|---|
| Arithmetic | `+  -  *  /  %` |
| Relational | `==  !=  >  <  >=  <=` |
| Logical | `&&  \|\|  !` |
| Assignment | `=  +=  -=  *=  /=  %=` |
| Increment/Decrement | `++  --` (pre vs post matters in expressions!) |

```java
int x = 5;
int a = x++;   // a = 5, THEN x becomes 6 (post-increment)
int y = 5;
int b = ++y;   // y becomes 6 FIRST, THEN b = 6 (pre-increment)
```

---

## Quick Revision

- **`main` method signature** → `public static void main(String[] args)`, must match exactly.
- **Primitives** → store values directly; **Reference types** → store pointers to heap objects.
- **Wrapper classes** → needed for generics, Java autoboxes/unboxes automatically.
- **Narrowing cast** → truncates, doesn't round (`(int) 9.7 == 9`).
- **Switch fall-through** → forgetting `break` in traditional switch; modern `->` switch avoids this entirely.
- **`Scanner.nextInt()` + `nextLine()`** → classic leftover-newline gotcha.
- **`BufferedReader`** → much faster than `Scanner` for large inputs.

---

## Interview Definition ⭐

> **A Java program's structure centers on the `public static void main(String[] args)` entry point, with data represented either as primitives (stored by value) or reference types (stored by pointer to heap-allocated objects) — control flow through if/switch statements, and I/O typically handled via `Scanner` for convenience or `BufferedReader` for performance-critical input.**

---

## Must-Solve LeetCode Problems

*(This topic is foundational/syntax-focused rather than pattern-based — the "practice" here is really about getting comfortable reading input correctly, which matters more on OJ platforms like Codeforces/HackerRank than LeetCode, where I/O is handled for you. A couple of good starting points anyway:)*

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Fizz Buzz](https://leetcode.com/problems/fizz-buzz/) | Easy | Straightforward conditional-logic warm-up |
| 2 | [Add Two Integers](https://leetcode.com/problems/add-two-integers/) | Easy | Simplest possible syntax/data-type check |
| 3 | [Palindrome Number](https://leetcode.com/problems/palindrome-number/) | Easy | Basic arithmetic operators and conditionals |
