# Strings

> **📅 Written:** 24 Sep 2026

### Definition

**A String is a sequence of characters, treated as a single object in Java. Unlike a char array, `String` objects are immutable — once created, their contents can never change.**

**In one sentence:**
> A string looks like a char array you can index into, but under the hood every "modification" actually builds a brand-new string, leaving the original untouched.

**Example:**

```
"hello"

Index:  0    1    2    3    4
Char:   h    e    l    l    o
```

---

## Key Features of Strings

- Backed internally by a `char[]` (or byte array in newer JVMs with compact strings)
- **Immutable** — any operation that "changes" a string returns a new one
- Stored in a special memory region called the **String Pool** for literal reuse
- Supports indexing, but **no direct assignment** like `s[0] = 'x'` (unlike arrays)
- Implements `Comparable<String>` — natural lexicographic ordering

---

## String Creation & the String Pool

```java
String a = "hello";           // goes into the String Pool
String b = "hello";           // reuses the SAME pooled object as `a`
String c = new String("hello"); // forces a NEW object on the heap, bypasses the pool

System.out.println(a == b);   // true  -> same reference (pooled)
System.out.println(a == c);   // false -> different reference
System.out.println(a.equals(c)); // true -> same content
```

```
String Pool:  ["hello"] <---- a
                    ↑---------- b
Heap:         ["hello"] <---- c   (separate object)
```

> **Interview point ⭐:** Always use `.equals()` to compare string **content**, never `==` (which compares **references**) — this is one of the most common Java interview gotchas.

---

## Why Strings Are Immutable

- **Security** — strings are used for class names, file paths, network connections; mutability would be a huge risk
- **Thread-safety** — immutable objects are automatically safe to share across threads
- **String Pool works** — reuse is only safe because the content can never change underneath a shared reference
- **Hashcode caching** — since content never changes, `hashCode()` can be computed once and cached, speeding up HashMap/HashSet usage

---

## Immutability in Practice — Why `+=` in a Loop Is Costly

```java
String result = "";
for (int i = 0; i < n; i++) {
    result += i;   // ❌ creates a NEW String object every single iteration
}
```

Each `+=` discards the old string and builds a new one → **O(n²)** total for n concatenations. Use `StringBuilder` instead.

---

## StringBuilder — The Mutable Alternative

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < n; i++) {
    sb.append(i);       // modifies the SAME internal buffer, no new object
}
String result = sb.toString();
```

→ Appending is **amortized O(1)**, making the whole loop **O(n)** instead of O(n²).

| String | StringBuilder |
|---|---|
| Immutable | Mutable |
| Thread-safe by nature (can't change) | Not thread-safe (`StringBuffer` is the synchronized version) |
| Slower for repeated modification | Fast in-place modification |
| Use when content won't change | Use when building/editing text repeatedly |

---

## Built-in Methods / Functions

| Method | What it does | Example |
|---|---|---|
| `s.length()` | Number of characters | `s.length()` |
| `s.charAt(i)` | Character at index `i` | `s.charAt(0)` → `'h'` |
| `s.substring(a, b)` | Substring `[a, b)` | `s.substring(1, 3)` |
| `s.indexOf(x)` | First index of `x`, or -1 | `s.indexOf('l')` |
| `s.lastIndexOf(x)` | Last index of `x` | `s.lastIndexOf('l')` |
| `s.contains(x)` | Whether `x` is a substring | `s.contains("ell")` |
| `s.equals(t)` / `s.equalsIgnoreCase(t)` | Content comparison | `s.equals("hello")` |
| `s.compareTo(t)` | Lexicographic comparison, returns int | `s.compareTo(t)` |
| `s.toCharArray()` | Converts to `char[]` | `char[] c = s.toCharArray();` |
| `s.toUpperCase()` / `s.toLowerCase()` | Case conversion (new string) | `s.toUpperCase()` |
| `s.trim()` / `s.strip()` | Removes leading/trailing whitespace | `s.trim()` |
| `s.split(regex)` | Splits into `String[]` by a delimiter/regex | `s.split(" ")` |
| `s.replace(a, b)` | Replaces all occurrences of `a` with `b` | `s.replace('l', 'L')` |
| `String.join(delim, arr)` | Joins pieces with a delimiter | `String.join(",", "a", "b")` |
| `String.valueOf(x)` | Converts almost anything to a String | `String.valueOf(123)` |
| `sb.append(x)` | Appends to a `StringBuilder` | `sb.append("hi")` |
| `sb.reverse()` | Reverses the buffer in place | `sb.reverse()` |
| `sb.deleteCharAt(i)` | Removes char at index `i` | `sb.deleteCharAt(0)` |
| `sb.insert(i, x)` | Inserts `x` at index `i` | `sb.insert(0, "!")` |

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| `charAt(i)` | O(1) |
| `length()` | O(1) |
| `substring(a, b)` | O(b - a) (creates a new string) |
| `concat` (`+` / `+=`, once) | O(n) — full copy |
| `+=` in a loop (n times) | O(n²) total |
| `StringBuilder.append` (n times) | O(n) amortized total |
| `equals()` | O(n) worst case |
| `indexOf()` | O(n) (naive), better with KMP for repeated search |

---

## Quick Revision

- **String** → immutable char sequence, backed by a char array, cached in the String Pool.
- Always compare content with **`.equals()`**, never `==`.
- Repeated concatenation in a loop → use **StringBuilder** to avoid O(n²).
- Immutability enables thread-safety, security, and hashcode caching.
- Know the standard method table — `substring`, `split`, `charAt`, `toCharArray` show up constantly.

---

## Interview Definition ⭐

> **A String in Java is an immutable sequence of characters backed by an internal char array — every "modification" produces a new object, which is what makes strings inherently thread-safe and poolable, but also why repeated concatenation should go through a mutable `StringBuilder` instead.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Valid Palindrome](https://leetcode.com/problems/valid-palindrome/) | Easy | Two-pointer scan over a string, basic char filtering |
| 2 | [Reverse String](https://leetcode.com/problems/reverse-string/) | Easy | In-place two-pointer swap on a char array |
| 3 | [Valid Anagram](https://leetcode.com/problems/valid-anagram/) | Easy | Frequency-array pattern applied to strings |
| 4 | [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | Medium | Sliding window + frequency tracking on strings |
| 5 | [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/) | Medium | Expand-around-center technique, very common interview ask |
| 6 | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) | Medium | String sorting/signature used as a hashmap key |
| 7 | [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/) | Medium | Careful character-by-character parsing with edge cases |
| 8 | [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) | Hard | Advanced sliding window over strings with two frequency maps |
| 9 | [Implement strStr() / Find the Index of the First Occurrence in a String](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/) | Easy | String matching — good entry point before learning KMP |
