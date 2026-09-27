# Logic Building (Number Theory)

> **📅 Written:** 24 Sep 2026

### Definition

**Number Theory, in a DSA context, is the study of integer properties — divisibility, primes, GCD/LCM, and modular arithmetic — that underpin a huge chunk of competitive programming and interview "math" questions.**

**In one sentence:**
> A handful of core number-theory ideas (GCD, primality, modular arithmetic) reappear so often across problems that memorizing their O(log n)-or-better implementations pays off disproportionately.

---

## GCD and LCM

### GCD (Greatest Common Divisor) — Euclidean Algorithm

```
gcd(48, 18):
48 = 2×18 + 12
18 = 1×12 + 6
12 = 2×6  + 0    -> gcd = 6
```

```java
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```
→ **O(log(min(a, b)))** — remarkably fast, since each step at least halves one of the numbers (in the worst case, consecutive Fibonacci numbers).

### LCM (Least Common Multiple)

```java
long lcm(int a, int b) {
    return (long) a / gcd(a, b) * b;   // divide first to avoid overflow before multiplying
}
```
> **Watch out ⭐:** Compute `a / gcd(a, b) * b`, not `a * b / gcd(a, b)` — multiplying first risks integer overflow for large inputs.

---

## Prime Numbers

### Checking Primality (Single Number)

```java
boolean isPrime(int n) {
    if (n < 2) return false;
    for (int i = 2; (long) i * i <= n; i++) {   // only need to check up to sqrt(n)
        if (n % i == 0) return false;
    }
    return true;
}
```
→ **O(√n)** — a number can't have two factors both greater than √n (their product would exceed n).

### Sieve of Eratosthenes (Many Numbers Up to N)

```java
boolean[] isComposite = new boolean[n + 1];
for (int i = 2; (long) i * i <= n; i++) {
    if (!isComposite[i]) {
        for (int j = i * i; j <= n; j += i) {
            isComposite[j] = true;
        }
    }
}
```
→ **O(n log log n)** to precompute primality for every number up to n — massively faster than checking each individually when you need many answers.

### Prime Factorization

```java
List<Integer> primeFactors(int n) {
    List<Integer> factors = new ArrayList<>();
    for (int i = 2; (long) i * i <= n; i++) {
        while (n % i == 0) {
            factors.add(i);
            n /= i;
        }
    }
    if (n > 1) factors.add(n);   // whatever's left is itself prime
    return factors;
}
```
→ **O(√n)**.

---

## Modular Arithmetic

Used constantly to prevent integer overflow when answers can be astronomically large — the problem asks for the answer **"mod 10⁹ + 7"** (a common large prime chosen specifically to avoid overflow while still being "random enough" to avoid collision patterns).

### The Core Rules

```
(a + b) % m = ((a % m) + (b % m)) % m
(a - b) % m = ((a % m) - (b % m) + m) % m     <- add m to handle negative results
(a * b) % m = ((a % m) * (b % m)) % m
(a / b) % m  requires MODULAR INVERSE - see below, NOT simple division
```

```java
long MOD = 1_000_000_007L;
long result = ((a % MOD) + (b % MOD)) % MOD;
```

> **Watch out ⭐:** In Java, `%` on a negative number can return a **negative** result (unlike some languages) — e.g. `-7 % 3` gives `-1`, not `2`. Always add `+ m` before the final `% m` when subtraction is involved, to guarantee a non-negative result.

### Fast (Modular) Exponentiation

Computing `a^b` naively is O(b) — repeated squaring drops this to O(log b).

```java
long power(long base, long exp, long mod) {
    long result = 1;
    base %= mod;
    while (exp > 0) {
        if ((exp & 1) == 1) {        // if the current bit is set
            result = (result * base) % mod;
        }
        base = (base * base) % mod;
        exp >>= 1;                    // move to the next bit
    }
    return result;
}
```
→ **O(log b)** — the same "halving" idea as binary search, applied to exponents.

### Modular Inverse (for "division" under a modulus)

Using **Fermat's Little Theorem**: if `m` is prime, `a^(m-1) ≡ 1 (mod m)`, so `a's inverse = a^(m-2) mod m`.

```java
long modInverse(long a, long mod) {
    return power(a, mod - 2, mod);   // only valid when mod is prime
}

// Now "division" under modulus:
long divide(long a, long b, long mod) {
    return (a % mod * modInverse(b, mod)) % mod;
}
```
→ Used heavily in combinatorics (computing nCr mod a prime).

---

## Digit Manipulation

```java
// Sum of digits
int digitSum(int n) {
    int sum = 0;
    while (n > 0) {
        sum += n % 10;
        n /= 10;
    }
    return sum;
}

// Reverse a number
int reverseNumber(int n) {
    int reversed = 0;
    while (n > 0) {
        reversed = reversed * 10 + n % 10;
        n /= 10;
    }
    return reversed;
}

// Count digits
int countDigits(int n) {
    int count = 0;
    while (n > 0) {
        count++;
        n /= 10;
    }
    return count;
}
```
→ All **O(number of digits)**, i.e. O(log₁₀ n).

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| GCD (Euclidean algorithm) | O(log(min(a,b))) |
| Primality check (single number) | O(√n) |
| Sieve of Eratosthenes (up to n) | O(n log log n) |
| Prime factorization | O(√n) |
| Fast exponentiation | O(log exponent) |
| Modular inverse (via Fermat) | O(log mod) |
| Digit manipulation | O(digits) = O(log₁₀ n) |

---

## Quick Revision

- **GCD** → Euclidean algorithm, O(log n); **LCM** → `a / gcd(a,b) * b` (divide first!).
- **Primality** → O(√n) single check; **Sieve** → O(n log log n) for many numbers.
- **Modular arithmetic** → apply `% mod` after every `+`, `-`, `*` to prevent overflow; add `+ mod` after subtraction to stay non-negative.
- **Fast exponentiation** → O(log b), the "binary search of multiplication".
- **Modular inverse** (Fermat's Little Theorem) → needed for "division" under a modulus, requires `mod` to be prime.

---

## Interview Definition ⭐

> **Number theory in DSA centers on a small toolkit — the Euclidean algorithm for GCD in O(log n), the Sieve of Eratosthenes for bulk primality in O(n log log n), and modular arithmetic (with fast exponentiation and Fermat's Little Theorem for modular inverses) to keep large results within safe integer bounds — this toolkit underlies a disproportionate share of "math" interview and competitive programming questions.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/) | Easy | GCD logic applied in a non-obvious context |
| 2 | [Count Primes](https://leetcode.com/problems/count-primes/) | Medium | The Sieve of Eratosthenes, directly |
| 3 | [Powx, n) / Pow(x, n)](https://leetcode.com/problems/powx-n/) | Medium | Fast exponentiation pattern |
| 4 | [Super Pow](https://leetcode.com/problems/super-pow/) | Medium | Modular fast exponentiation combined with digit handling |
| 5 | [Nth Digit](https://leetcode.com/problems/nth-digit/) | Medium | Digit-counting logic |
| 6 | [Reverse Integer](https://leetcode.com/problems/reverse-integer/) | Medium | Digit reversal with overflow handling |
| 7 | [Water and Jug Problem](https://leetcode.com/problems/water-and-jug-problem/) | Medium | GCD-based feasibility (Bezout's identity) |
