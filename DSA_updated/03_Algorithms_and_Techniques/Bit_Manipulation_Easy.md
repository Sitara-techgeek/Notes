# Bit Manipulation - Easy

> **📅 Written:** 24 Sep 2026

### Definition

**Bit Manipulation is the technique of directly working with the individual binary bits of a number using bitwise operators, often solving problems in O(1) or O(log n) with far less memory than array/hashmap-based approaches.**

**In one sentence:**
> Every integer is secretly a row of on/off switches — bit manipulation is just flipping the right switches directly, instead of doing arithmetic the "normal" way.

**Example:**
```
Decimal:  13
Binary:   1101
Bits:     bit3=1, bit2=1, bit1=0, bit0=1
```

---

## The Bitwise Operators

| Operator | Symbol | What it does |
|---|---|---|
| AND | `&` | 1 only if **both** bits are 1 |
| OR | `\|` | 1 if **either** bit is 1 |
| XOR | `^` | 1 if bits are **different** |
| NOT | `~` | Flips every bit |
| Left Shift | `<<` | Shifts bits left, filling with 0 (multiplies by 2 per shift) |
| Right Shift | `>>` | Shifts bits right, sign-extending (preserves sign for negative numbers) |
| Unsigned Right Shift | `>>>` | Shifts bits right, filling with 0 (ignores sign) |

```
  1101   (13)              1101   (13)              1101   (13)
& 1011   (11)             | 1011  (11)             ^ 1011  (11)
------                    ------                   ------
  1001   (9)                1111   (15)              0110   (6)
```

```java
int a = 13, b = 11;
System.out.println(a & b);   // 9
System.out.println(a | b);   // 15
System.out.println(a ^ b);   // 6
System.out.println(~a);      // -14 (two's complement: flips all bits, then +1 logic applies)
System.out.println(a << 2);  // 52  (13 * 4)
System.out.println(a >> 2);  // 3   (13 / 4, floor)
```

---

## Two's Complement — Why Negative Numbers Look Weird

Java uses **two's complement** to represent negative integers: flip all bits, then add 1.

```
5:  00000000 00000000 00000000 00000101
-5: 11111111 11111111 11111111 11111011   (flip all bits of 5, then +1)
```

> **Interview point ⭐:** This is why `~x` equals `-x - 1` — flipping all bits of x is exactly the two's complement of `-(x+1)`. It also explains why `>>` (arithmetic shift) fills with the **sign bit** (1s for negative numbers) to preserve the number's sign, while `>>>` always fills with 0s regardless of sign.

---

## Essential Bit Tricks

### 1. Check if a bit is set

```java
boolean isBitSet(int n, int i) {
    return (n & (1 << i)) != 0;
}
```

### 2. Set a bit

```java
int setBit(int n, int i) {
    return n | (1 << i);
}
```

### 3. Clear (unset) a bit

```java
int clearBit(int n, int i) {
    return n & ~(1 << i);
}
```

### 4. Toggle a bit

```java
int toggleBit(int n, int i) {
    return n ^ (1 << i);
}
```

### 5. Check if a number is even/odd

```java
boolean isOdd(int n) {
    return (n & 1) == 1;   // faster than n % 2, checks just the last bit
}
```

### 6. Check if a number is a power of 2

```java
boolean isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}
```
```
8       = 1000
8 - 1   = 0111
8 & 7   = 0000   -> power of 2!

6       = 0110
6 - 1   = 0101
6 & 5   = 0100   -> NOT 0, not a power of 2
```
> **Interview point ⭐:** `n & (n-1)` always clears the **lowest set bit** of n — this single trick underlies power-of-2 checks, counting set bits, and more.

### 7. Count the number of set bits (Hamming Weight / Popcount)

```java
int countSetBits(int n) {
    int count = 0;
    while (n != 0) {
        n = n & (n - 1);   // clears the lowest set bit each iteration
        count++;
    }
    return count;
}
```
→ **O(number of set bits)**, faster than checking every one of the 32 bits individually.

*(Java also has a direct built-in: `Integer.bitCount(n)`.)*

### 8. XOR Trick — Find the Single Non-Duplicate Number

Relies on two XOR properties: `x ^ x = 0` and `x ^ 0 = x`, plus XOR being commutative/associative.

```java
int singleNumber(int[] nums) {
    int result = 0;
    for (int num : nums) {
        result ^= num;   // all duplicate pairs cancel out to 0, leaving only the unique one
    }
    return result;
}
```
→ **O(n) time, O(1) space** — beats a HashSet-based approach (which needs O(n) extra space).

### 9. Swap Two Numbers Without a Temp Variable

```java
a = a ^ b;
b = a ^ b;   // b becomes original a
a = a ^ b;   // a becomes original b
```
> Rarely used in practice (a temp variable is clearer), but a favorite "clever trick" interview question.

---

## Built-in Java Bit Methods

| Method | What it does |
|---|---|
| `Integer.bitCount(n)` | Number of set bits (popcount) |
| `Integer.toBinaryString(n)` | Binary string representation |
| `Integer.numberOfLeadingZeros(n)` | Count of leading 0 bits |
| `Integer.numberOfTrailingZeros(n)` | Count of trailing 0 bits |
| `Integer.highestOneBit(n)` | Isolates the highest set bit |
| `Integer.lowestOneBit(n)` | Isolates the lowest set bit (equivalent to `n & -n`) |
| `Long.bitCount(n)` | Same as above, for `long` |

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| Any single bitwise operator (`&`, `\|`, `^`, `~`, `<<`, `>>`) | O(1) |
| Check/set/clear/toggle a bit | O(1) |
| Count set bits | O(number of set bits), worst O(32) for an int |
| XOR-based single-number trick | O(n) time, O(1) space |

---

## Quick Revision

- **AND/OR/XOR/NOT/Shifts** → the fundamental operators, all O(1).
- **Two's complement** → why negatives look like large binary numbers, and why `~x == -x - 1`.
- **`n & (n-1)`** → clears the lowest set bit; the base of power-of-2 checks and popcount.
- **XOR properties** (`x^x=0`, `x^0=x`) → the trick behind "find the single unique number".
- Bit tricks trade a small amount of readability for O(1) space and very fast constant-factor performance.

---

## Interview Definition ⭐

> **Bit manipulation directly operates on a number's binary representation using AND, OR, XOR, NOT, and shift operators — enabling O(1) checks/sets/clears of individual bits and clever O(1)-space tricks (like `n & (n-1)` to isolate set bits, or XOR to cancel duplicate pairs) that often beat array or hashmap-based approaches on both time and space.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Single Number](https://leetcode.com/problems/single-number/) | Easy | The defining XOR-cancellation trick |
| 2 | [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) | Easy | Popcount using `n & (n-1)` |
| 3 | [Power of Two](https://leetcode.com/problems/power-of-two/) | Easy | The `n & (n-1) == 0` trick |
| 4 | [Counting Bits](https://leetcode.com/problems/counting-bits/) | Easy | Bit counting combined with DP |
| 5 | [Missing Number](https://leetcode.com/problems/missing-number/) | Easy | XOR trick applied to find a missing value instead of a duplicate |
| 6 | [Reverse Bits](https://leetcode.com/problems/reverse-bits/) | Easy | Bit-by-bit manipulation practice |
| 7 | [Sum of Two Integers](https://leetcode.com/problems/sum-of-two-integers/) | Medium | Addition using only bitwise operators (no `+`) |
