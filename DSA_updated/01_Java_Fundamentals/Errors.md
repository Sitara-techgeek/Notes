# Errors in Java

> **📅 Written:** 24 Sep 2026

### Definition

**An Error (in the broad sense) is anything that stops your program from running correctly — this includes compile-time errors, runtime exceptions, and logical mistakes.** In Java specifically, `Error` is also a formal class name for a category of serious, usually unrecoverable JVM-level problems.

**In one sentence:**
> Something went wrong somewhere between "I wrote this code" and "I got the output I expected" — the type of error tells you *where* to look.

---

## The Three Broad Categories

- **Compile-Time Errors** — caught by the compiler, before the program even runs
- **Runtime Errors (Exceptions)** — occur while the program is executing
- **Logical Errors** — the program runs and finishes fine, but gives the *wrong* answer

```
Write code ---> Compile ---> Run ---> Output
      ↑             ↑           ↑         ↑
  (syntax)    Compile-Time   Runtime   Logical
                 Error        Error     Error
```

---

## 1. Compile-Time Errors

### Definition

Errors detected by the **compiler** before the code is turned into bytecode. The program never runs at all.

**Common causes:**
- Missing semicolons, unmatched braces
- Type mismatches (`int x = "hello";`)
- Undeclared variables or methods
- Missing return statements

```java
int x = 10
System.out.println(x);   // ❌ Missing semicolon -> compile-time error
```

**How to solve:** Read the compiler's line number and message carefully — it almost always points at (or just after) the real issue. Fix the syntax/type and recompile.

---

## 2. Runtime Errors (Exceptions)

### Definition

Errors that occur **while the program is running**, after it has compiled successfully. In Java, these are represented as objects — instances of the `Throwable` class hierarchy.

```
                Throwable
               /          \
          Exception       Error
         /          \         \
   Checked       Unchecked   (JVM-level,
  Exceptions    (RuntimeExc)  e.g. OutOfMemoryError)
```

### Checked vs Unchecked Exceptions ⭐

| Checked Exceptions | Unchecked Exceptions (RuntimeException) |
|---|---|
| Must be either caught or declared with `throws` | No compiler obligation to handle |
| Checked *at compile time* | Checked *at runtime* |
| e.g. `IOException`, `SQLException` | e.g. `NullPointerException`, `ArithmeticException` |
| Usually recoverable, external issues (file not found, network) | Usually programming bugs (logic mistakes) |

> **Interview point ⭐:** Checked exceptions represent things *outside* your control that you're expected to plan for (a file might not exist). Unchecked exceptions represent bugs you should *fix*, not just catch.

### Common Runtime Exceptions in Java

| Exception | Typical Cause |
|---|---|
| `NullPointerException` | Calling a method / accessing a field on a `null` reference |
| `ArrayIndexOutOfBoundsException` | Accessing an array index that doesn't exist |
| `ArithmeticException` | Dividing an integer by zero (`10 / 0`) |
| `ClassCastException` | Invalid type cast (`Object o = "hi"; Integer i = (Integer) o;`) |
| `NumberFormatException` | Parsing a non-numeric string (`Integer.parseInt("abc")`) |
| `ConcurrentModificationException` | Modifying a collection while iterating it with a `for-each` loop |
| `StackOverflowError` | Infinite or excessively deep recursion |
| `OutOfMemoryError` | JVM heap exhausted (huge allocations, memory leaks) |

```java
int[] arr = {1, 2, 3};
System.out.println(arr[5]);
// ArrayIndexOutOfBoundsException: Index 5 out of bounds for length 3
```

### Handling Runtime Exceptions

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Can't divide by zero: " + e.getMessage());
} finally {
    System.out.println("This always runs");
}
```

- `try` — wraps risky code
- `catch` — handles a specific exception type
- `finally` — always executes, regardless of whether an exception occurred (cleanup code)
- `throw` — manually raise an exception
- `throws` — declare that a method might raise a checked exception

```java
public void readFile(String path) throws IOException {
    // may throw IOException; caller must handle or propagate it
}
```

**How to solve runtime exceptions (general checklist):**
1. Read the **stack trace** top-down — the topmost line is where it actually broke
2. Identify the exception type — it tells you the *category* of bug
3. Check for `null` values, invalid indices, or bad input before the failing line
4. Add validation / null-checks, or wrap with `try-catch` if the failure is expected (e.g. user input)
5. For `StackOverflowError` — check your recursion has a correct, reachable base case
6. For `OutOfMemoryError` — look for unbounded loops creating objects, or unintentionally large data structures

---

## 3. Logical Errors

### Definition

The code **compiles and runs without crashing**, but produces an **incorrect result**. These are the hardest to catch because there's no error message to point you anywhere.

```java
// Intended: average of two numbers
int avg = (a + b) / 2;   // if a, b are int, this truncates — looks "fine", silently wrong for odd sums
```

**Common causes:**
- Off-by-one errors in loops (`<=` vs `<`)
- Wrong operator (`=` instead of `==` in some languages; less common in Java since it won't compile for booleans, but easy to misuse in conditions)
- Incorrect assumptions about integer division / overflow
- Wrong loop bounds, wrong variable used

**How to solve:** Since there's no stack trace, debugging relies on:
- Adding print statements / using a debugger to inspect intermediate values
- Testing with small, hand-traceable inputs first
- Re-reading the problem statement against your actual logic, line by line
- Writing unit tests for edge cases (empty input, single element, negative numbers)

---

## Custom (User-Defined) Exceptions

Sometimes the built-in exceptions don't describe your problem precisely enough — you can define your own.

```java
class InsufficientBalanceException extends Exception {
    public InsufficientBalanceException(String message) {
        super(message);
    }
}

class Account {
    double balance;
    void withdraw(double amount) throws InsufficientBalanceException {
        if (amount > balance) {
            throw new InsufficientBalanceException("Not enough balance!");
        }
        balance -= amount;
    }
}
```

> Extend `Exception` for a **checked** custom exception, or `RuntimeException` for an **unchecked** one — choose based on whether callers *must* be forced to handle it.

---

## Quick Revision

- **Compile-Time Error** → caught before running, usually syntax/type mistakes.
- **Runtime Error (Exception)** → occurs during execution; `Throwable` → `Exception` / `Error`.
- **Checked** → must handle or declare (`throws`); **Unchecked** → `RuntimeException`, no compiler obligation.
- **Logical Error** → runs fine, wrong output — no error message, hardest to debug.
- **try/catch/finally/throw/throws** → the full exception-handling toolkit.
- Debug runtime errors via the **stack trace**; debug logical errors via **tracing values manually**.

---

## Interview Definition ⭐

> **An error is any deviation from correct program behavior — compile-time errors are caught before execution by the compiler, runtime exceptions occur during execution and are represented as `Throwable` objects (checked vs unchecked), and logical errors produce wrong output despite running successfully, making them the hardest category to detect since no error message is raised.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/) | Easy | Practicing defensive checks that prevent invalid-state bugs |
| 2 | [Divide Two Integers](https://leetcode.com/problems/divide-two-integers/) | Medium | Forces you to handle overflow/edge cases explicitly (classic source of silent logical errors) |
| 3 | [Reverse Integer](https://leetcode.com/problems/reverse-integer/) | Medium | Integer overflow handling — a very common "runs fine but wrong" bug source |
| 4 | [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/) | Medium | Exhaustive edge-case/validation handling, mirrors real `NumberFormatException`-style issues |

*(Errors/Exceptions is more of a conceptual-and-debugging topic than a problem-pattern topic, so the "must-solve" list here is short and edge-case-handling focused rather than algorithmic.)*
