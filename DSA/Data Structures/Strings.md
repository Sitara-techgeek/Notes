# Strings

> **📅 Written:** 20 Jul 2026

### Definition

**A String in Java is a sequence of characters, implemented internally as an immutable character array — every "modification" actually produces a brand-new String object rather than changing the original.**

**In one sentence:**
> A Java String is basically a read-only array of characters — once created, it can never be changed, only replaced by a new one.

**Example:**

```java
String s = "HELLO";
System.out.println(s.charAt(0));   // 'H'
System.out.println(s.charAt(4));   // 'O'
```

`charAt(0)` gives `'H'`, `charAt(4)` gives `'O'` — same O(1) indexed access as arrays.

---

## Key Features of Strings (Java)

- Underlying structure is a **character array** (`char[]`)
- Characters stored contiguously in memory → indexed access is O(1)
- **Immutable** — every "edit" (`concat`, `replace`, `+`, etc.) returns a **new** `String` object
- Java keeps a special **String Pool** in memory — identical string literals are reused instead of duplicated, to save memory
- Backed by the `String` class, with a huge set of built-in methods for searching, comparing, transforming text

---

## Mutable vs Immutable Strings (String Pool)

| Immutable (`String`) | Mutable (`StringBuilder` / `StringBuffer`) |
| --- | --- |
| Every "edit" creates a brand-new string in memory | Edits happen **in-place**, on the same object |
| Old string stays unchanged (or gets garbage collected) | No new allocation needed per edit |
| Safer — no accidental shared-reference bugs | Faster for heavy, repeated edits |

```java
String s = "cat";
s = s + "s";     // doesn't modify "cat" — creates a brand new string "cats"
```

```
Before:  [c][a][t]              <- old string, now unused
After:   [c][a][t][s]           <- new string, s now points here
```

**String Pool:**

```java
String a = "hello";
String b = "hello";
System.out.println(a == b);          // true — both point to the SAME pooled object

String c = new String("hello");
System.out.println(a == c);          // false — new String() forces a new object, bypassing the pool
System.out.println(a.equals(c));     // true — same characters, checked via .equals()
```

> **Interview point ⭐:** Because strings are immutable, building a string via repeated concatenation (`+=`) inside a loop is **O(n²)** — every `+=` creates a new copy. Use `StringBuilder` to bring it down to O(n). Also always compare string **content** with `.equals()`, never `==` (which compares references/memory addresses).

---

# String Operations

> **📅 Written:** 20 Jul 2026

## 1. Access

```java
String s = "hello";
char ch = s.charAt(1);   // 'e' — O(1)
```

Same as array indexing — jumps straight to the character.

---

## 2. Traversal

```java
String s = "hello";
for (char ch : s.toCharArray()) {
    System.out.println(ch);
}
```

Visits every character once → **O(n)**.

---

## 3. Concatenation

```java
String s1 = "hello";
String s2 = "world";
String result = s1 + s2;        // "helloworld"
String result2 = s1.concat(s2); // "helloworld"
```

→ **O(n + m)**, since a brand-new string of combined length has to be created and both originals copied into it.

---

## 4. Substring / Slicing (start, end, step)

### Definition

**Slicing** conceptually works off **three parameters** — `start` (where to begin), `end` (where to stop, exclusive), and `step` (how many characters to skip between each character taken).

```
"HELLO WORLD"
 slice(start=0, end=5, step=1)  →  "HELLO"
 slice(start=0, end=11, step=2) →  "HLOWRD"   (every 2nd character)
```

⚠️ **Java's built-in `substring()` only supports `start` and `end` — there's no native `step` parameter** (that's a feature Python has, not Java). To slice with a step in Java, you have to build it manually with a loop.

**`start` and `end` — using Java's `substring()`:**

```java
String s = "HELLO WORLD";

System.out.println(s.substring(6));       // "WORLD"    — from index 6 to the end
System.out.println(s.substring(0, 5));    // "HELLO"    — index 0 up to (not including) index 5
System.out.println(s.substring(6, 9));    // "WOR"      — index 6 up to (not including) index 9
```

**Adding a `step` manually (Java has no built-in for this):**

```java
public static String sliceWithStep(String s, int start, int end, int step) {
    StringBuilder result = new StringBuilder();
    for (int i = start; i < end; i += step) {
        result.append(s.charAt(i));
    }
    return result.toString();
}

String s = "HELLO WORLD";
System.out.println(sliceWithStep(s, 0, 11, 2));   // "HLOWRD" — every 2nd character
System.out.println(sliceWithStep(s, 0, 11, 3));   // "HLWL"   — every 3rd character
```

→ **O(k)**, where `k` is the length of the slice, since those characters get copied into a new string.

---

## 5. Search (Substring Matching)

**Naive approach** — check every starting position:

```java
public static int find(String s, String pattern) {
    int n = s.length(), m = pattern.length();
    for (int i = 0; i <= n - m; i++) {
        if (s.substring(i, i + m).equals(pattern)) {
            return i;
        }
    }
    return -1;
}
```

→ **O(n·m)** in the worst case (n = length of string, m = length of pattern).

> Faster pattern-matching algorithms like **KMP** and **Rabin-Karp** bring this down to **O(n + m)** — worth a dedicated file later.

---

## 6. Comparison

```java
System.out.println("apple".equals("apple"));         // true
System.out.println("apple".compareTo("banana") < 0);  // true — lexicographic (dictionary) order
```

→ **O(min(n, m))** — compares character by character until a mismatch or the shorter string ends.

---

## 7. Reversal

```java
String s = "hello";
String reversed = new StringBuilder(s).reverse().toString();   // "olleh"
```

```
Before:  [h][e][l][l][o]
After:   [o][l][l][e][h]
```

→ **O(n)** — every character gets visited/copied once.

---

## Time Complexity — Quick Table

| Operation                     | Time Complexity |
| ------------------------------ | ---------------- |
| Access by index (`charAt`)      | O(1)              |
| Traversal                       | O(n)              |
| Concatenation                   | O(n + m)          |
| Substring / slicing             | O(k)              |
| Search (naive substring match)  | O(n·m)            |
| Search (KMP / Rabin-Karp)       | O(n + m)          |
| Comparison                      | O(min(n, m))      |
| Reversal (via StringBuilder)    | O(n)              |

---

# Common String Methods (with Examples)

> **📅 Written:** 20 Jul 2026

### Definition

Java's `String` class ships with a large built-in toolkit — knowing these well saves a lot of manual looping during coding rounds.

| Method | Description | Returns |
| --- | --- | --- |
| `charAt()` | Returns the character at the specified index | `char` |
| `trim()` | Removes whitespace from both ends of a string | `String` |
| `toUpperCase()` | Converts a string to upper case letters | `String` |
| `toLowerCase()` | Converts a string to lower case letters | `String` |
| `substring()` | Returns a new string that is a substring of the specified string | `String` |
| `equals()` | Compares two strings; true if equal | `boolean` |
| `equalsIgnoreCase()` | Compares two strings, ignoring case | `boolean` |
| `toCharArray()` | Converts the string to a new character array | `char[]` |
| `compareTo()` | Compares two strings lexicographically | `int` |
| `replace()` | Returns a new string with specified values replaced | `String` |
| `compareToIgnoreCase()` | Compares two strings lexicographically, ignoring case | `int` |
| `concat()` | Appends a string to the end of another string | `String` |
| `contains()` | Checks whether a string contains a sequence of characters | `boolean` |
| `contentEquals()` | Checks whether a string equals the exact sequence of a `CharSequence`/`StringBuffer` | `boolean` |
| `copyValueOf()` | Returns a `String` representing the characters of a char array | `String` |
| `endsWith()` | Checks whether a string ends with specified character(s) | `boolean` |
| `indexOf()` | Returns the position of the first found occurrence | `int` |
| `isEmpty()` | Checks whether a string is empty | `boolean` |
| `join()` | Joins one or more strings with a specified separator | `String` |
| `lastIndexOf()` | Returns the position of the last found occurrence | `int` |
| `length()` | Returns the length of the string | `int` |
| `regionMatches()` | Tests if two string regions are equal | `boolean` |
| `replaceAll()` | Replaces substrings matching a regex with a replacement | `String` |
| `replaceFirst()` | Replaces the first substring matching a regex | `String` |
| `split()` | Splits a string into an array of substrings | `String[]` |
| `startsWith()` | Checks whether a string starts with specified characters | `boolean` |
| `subSequence()` | Returns a new character sequence that is a subsequence | `CharSequence` |
| `toString()` | Returns the value of a String object | `String` |
| `valueOf()` | Returns the string representation of a given value | `String` |

## Examples — Grouped by Category

**Basic info / access:**

```java
String s = "  Hello World  ";

System.out.println(s.length());              // 15  — includes the spaces
System.out.println(s.charAt(2));              // 'H'
System.out.println(s.isEmpty());              // false
System.out.println("".isEmpty());             // true
System.out.println(s.trim());                 // "Hello World" — outer whitespace removed
```

**Case conversion:**

```java
String s = "Hello World";

System.out.println(s.toUpperCase());          // "HELLO WORLD"
System.out.println(s.toLowerCase());          // "hello world"
```

**Extracting / transforming:**

```java
String s = "Hello World";

System.out.println(s.substring(6));           // "World"
System.out.println(s.substring(0, 5));        // "Hello"
System.out.println(s.replace('o', '0'));      // "Hell0 W0rld"
System.out.println(s.concat("!"));            // "Hello World!"
System.out.println(Arrays.toString(s.toCharArray())); // [H, e, l, l, o, ...]
System.out.println(s.subSequence(0, 5));       // "Hello" (as a CharSequence)
```

> **Arrays.toString() in short** Arrays don't override `toString()` in Java, so printing a `char[]` directly (e.g. `System.out.println(chars)`) prints a memory reference like `[C@1b6d3586` instead of its contents. `Arrays.toString()` (needs `import java.util.Arrays;`) is what actually formats the array's elements into a readable string.

**Comparison:**

```java
String a = "apple";
String b = "Apple";

System.out.println(a.equals(b));                    // false — case-sensitive
System.out.println(a.equalsIgnoreCase(b));           // true
System.out.println(a.compareTo("banana"));           // negative — "apple" comes before "banana"
System.out.println(a.compareToIgnoreCase("APPLE"));  // 0 — equal, ignoring case
System.out.println(a.contentEquals(new StringBuilder("apple"))); // true

String b = "maple";
System.out.println(a.regionMatches(2, b, 2, 3));   // true — a.substring(2,5)="ple", b.substring(2,5)="ple"
```

> **`regionMatches()` in short:** compares a chunk of one string against a chunk of another — `a.regionMatches(2, b, 2, 3)` checks if 3 characters starting at index 2 in `a` match 3 characters starting at index 2 in `b`, without you having to manually `substring()` and `.equals()` both sides yourself.

**Searching:**

```java
String s = "Hello World, Hello Java";

System.out.println(s.contains("World"));      // true
System.out.println(s.startsWith("Hello"));    // true
System.out.println(s.endsWith("Java"));       // true
System.out.println(s.indexOf("Hello"));       // 0  — first occurrence
System.out.println(s.lastIndexOf("Hello"));   // 13 — last occurrence
```

**Splitting / joining:**

```java
String csv = "apple,banana,cherry";
String[] fruits = csv.split(",");             // ["apple", "banana", "cherry"]

String joined = String.join("-", "2026", "07", "20");
System.out.println(joined);                    // "2026-07-20"
```

**Regex-based replace:**

```java
String s = "foo123bar456";

System.out.println(s.replaceAll("[0-9]+", "#"));    // "foo#bar#"
System.out.println(s.replaceFirst("[0-9]+", "#"));  // "foo#bar456"
```

**Static / conversion helpers:**

```java
char[] chars = {'j', 'a', 'v', 'a'};
System.out.println(String.copyValueOf(chars));   // "java"

int num = 100;
System.out.println(String.valueOf(num));         // "100"

String s = "hello";
System.out.println(s.toString());                 // "hello" (already a String, rarely needed directly)
```

---

# StringBuilder

> **📅 Written:** 20 Jul 2026

### Definition

**`StringBuilder` is a mutable sequence of characters — unlike `String`, its methods modify the object in-place instead of creating a new one, making it far more efficient for repeated edits.**

**In one sentence:**
> Where `String` forces a brand-new object on every change, `StringBuilder` is like a resizable buffer you can freely append to, insert into, or delete from — all on the same object.

## Implementation (Internal Working)

Internally, `StringBuilder` keeps a **resizable `char[]` array** with some **extra unused capacity** — similar to how a dynamic array (like `ArrayList`) works:

```
Capacity 16, "Hello" stored:
[H][e][l][l][o][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ][ ]
                 ↑ unused capacity, ready for appends

append(" World") → fills into existing space, no resize needed:
[H][e][l][l][o][ ][W][o][r][l][d][ ][ ][ ][ ][ ]
```

When the internal array runs out of space, `StringBuilder` **allocates a new, larger array** (typically double the size) and copies the existing characters over — exactly like a dynamic array resize. This is why appends are **O(1) amortized**, instead of `String` concatenation's O(n) per operation.

```java
StringBuilder sb = new StringBuilder();      // starts with default capacity (16)
StringBuilder sb2 = new StringBuilder(50);   // starts with capacity 50
StringBuilder sb3 = new StringBuilder("Hi"); // starts with "Hi" + default extra capacity
```

## StringBuilder Methods

```java
StringBuilder sb = new StringBuilder("Hello");

sb.append(" World");          // "Hello World"       — adds to the end
sb.insert(5, ",");             // "Hello, World"      — inserts at a given index
sb.replace(0, 5, "Hey");       // "Hey, World"        — replaces a range with new text
sb.delete(3, 4);               // "Hey World"         — removes characters in a range
sb.deleteCharAt(0);            // "ey World"          — removes a single character
sb.reverse();                  // "dlroW ye"          — reverses in place
sb.setCharAt(0, 'D');          // "dlroW yD"           — changes a single character
sb.charAt(0);                  // 'd'                  — reads a character (like String)
sb.length();                   // current length
sb.capacity();                 // current allocated capacity (≥ length)
sb.toString();                 // converts back to a normal, immutable String
```

**Building a string efficiently (avoiding O(n²)):**

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 5; i++) {
    sb.append(i).append(",");
}
System.out.println(sb.toString());   // "0,1,2,3,4,"
```

→ **O(n)** overall, versus **O(n²)** if this were done with repeated `String +=`.

---

## String vs StringBuilder ⭐

| `String` | `StringBuilder` |
| --- | --- |
| Immutable — every edit makes a new object | Mutable — edits happen in-place |
| Thread-safe by nature (can't change) | **Not** thread-safe (no internal synchronization) |
| Slower for repeated edits (O(n²) in a loop) | Fast for repeated edits (O(1) amortized per append) |
| Stored/reused via the String Pool | Not pooled — always a fresh object |
| Use when the value rarely changes | Use when building/editing text repeatedly (loops, parsing) |

---

# StringBuffer

> **📅 Written:** 20 Jul 2026

### Definition

**`StringBuffer` is essentially the same as `StringBuilder` — a mutable, resizable character sequence — except every one of its methods is `synchronized`, making it thread-safe at the cost of extra overhead.**

**In one sentence:**
> `StringBuffer` is `StringBuilder`'s older, thread-safe twin — use it only when multiple threads might edit the same string buffer at once; otherwise `StringBuilder` is faster.

## Well-Known StringBuffer Methods

```java
StringBuffer sb = new StringBuffer("Hello");

sb.append(" World");           // "Hello World"
sb.insert(5, ",");              // "Hello, World"
sb.replace(0, 5, "Hey");        // "Hey, World"
sb.delete(3, 4);                 // "Hey World"
sb.deleteCharAt(0);              // "ey World"
sb.reverse();                    // "dlroW ye"
sb.charAt(0);                    // 'd'
sb.length();                     // current length
sb.capacity();                   // current allocated capacity
sb.toString();                   // converts back to a normal String
```

> Notice the method names are **identical** to `StringBuilder` — the only real difference is that `StringBuffer`'s methods are synchronized (safe across threads), while `StringBuilder`'s are not.

## StringBuilder vs StringBuffer ⭐

| `StringBuilder` | `StringBuffer` |
| --- | --- |
| **Not** thread-safe | Thread-safe (synchronized methods) |
| Faster (no locking overhead) | Slower (locking overhead on every call) |
| Use in single-threaded code (most cases) | Use only when shared across multiple threads |
| Introduced in Java 5 | Available since Java 1.0 (older) |

---

## Quick Revision

- **String** → Immutable character array; every edit creates a new object; compare with `.equals()`, not `==`.
- **String Pool** → Identical string literals are reused to save memory; `new String()` bypasses the pool.
- **Slicing (start, end, step)** → Java's `substring()` natively supports only `start`/`end`; a `step` needs a manual loop.
- **Key methods** → `charAt`, `substring`, `indexOf`, `split`, `replace`, `trim`, `equals`, `compareTo`, `toCharArray`, and more — all O(1) to O(n) depending on the operation.
- **StringBuilder** → Mutable, resizable `char[]` buffer under the hood; O(1) amortized append; not thread-safe; use for repeated edits.
- **StringBuffer** → Same API as `StringBuilder`, but thread-safe (synchronized) and slower; use only in multi-threaded contexts.

---

## Interview Definition ⭐

> **A Java String is an immutable sequence of characters offering O(1) indexed access but O(n) cost per modification since every edit creates a new object, while StringBuilder (and its thread-safe twin StringBuffer) provide a mutable, resizable character buffer that supports O(1) amortized in-place edits — making them the right choice whenever a string is being built or modified repeatedly.**