# Frequency Array

> **📅 Written:** 24 Sep 2026

### Definition

**A Frequency Array is an array used to count how many times each value (or each character/index) occurs in a dataset, using the value itself as the index into a counting array.**

**In one sentence:**
> Instead of scanning the data again and again to ask "how many times does X appear?", you build one array upfront where `freq[X]` directly holds the answer.

**Example:**

```
Data:  [2, 3, 2, 1, 3, 3]

Index:  0  1  2  3
Freq:  [0, 1, 2, 3]
              ↑  ↑  ↑
           1 appears once, 2 appears twice, 3 appears thrice
```

---

## Why It Matters

- Turns repeated "count occurrences" queries from **O(n) each** into **O(1) each** after one O(n) build pass
- Foundation for counting sort, character frequency problems, anagram checks, majority element, and more
- Works cleanly when values are bounded in a known small range (e.g. lowercase letters, digits 0–9, values ≤ 10⁵)

---

## Building a Frequency Array

```java
int[] arr = {2, 3, 2, 1, 3, 3};
int maxVal = 3;                       // known upper bound of values
int[] freq = new int[maxVal + 1];     // +1 so index maxVal is valid

for (int val : arr) {
    freq[val]++;
}
// freq = [0, 1, 2, 3]
```

→ **O(n)** to build, **O(1)** per lookup afterward.

### Character Frequency (very common variant)

```java
String s = "leetcode";
int[] freq = new int[26];             // for lowercase a-z

for (char c : s.toCharArray()) {
    freq[c - 'a']++;                  // map 'a'-'z' to index 0-25
}
```

`c - 'a'` works because characters are stored as integer codes in Java — subtracting `'a'` shifts the range down to start at 0.

---

## When the Value Range Is Too Large / Unknown

If values aren't small and bounded (e.g. arbitrary large numbers, or negative numbers), a plain array won't work directly — use a **HashMap** instead:

```java
Map<Integer, Integer> freq = new HashMap<>();
for (int val : arr) {
    freq.put(val, freq.getOrDefault(val, 0) + 1);
}
```

> **Interview point ⭐:** Frequency array (array-based counting) vs HashMap-based counting is purely about the value range. Bounded & small → array (faster, less overhead). Unbounded or sparse → HashMap.

---

## Common Patterns Using Frequency Arrays

- **Anagram check** — build frequency arrays for both strings, compare
- **Majority Element** — element with frequency > n/2
- **Most/Least Frequent Element** — scan the frequency array for the max/min count
- **Counting Sort** — use frequency counts to place elements directly in sorted position, O(n + k)
- **Duplicate detection** — any `freq[x] > 1` flags a duplicate

```java
// Anagram check using frequency arrays
boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    int[] freq = new int[26];
    for (char c : s.toCharArray()) freq[c - 'a']++;
    for (char c : t.toCharArray()) freq[c - 'a']--;
    for (int f : freq) if (f != 0) return false;
    return true;
}
```

---

## Time Complexity — Quick Table

| Operation | Time Complexity |
|---|---|
| Build frequency array | O(n) |
| Lookup count of a value | O(1) |
| Find max/min frequency | O(k) — k = size of value range |
| HashMap-based counting (build) | O(n) average |
| HashMap-based lookup | O(1) average |

---

## Quick Revision

- **Frequency Array** → count occurrences using the value itself as the index.
- Build in **O(n)**, then **O(1)** lookups.
- Use a **plain array** when values are small & bounded (e.g. `a`-`z`, digits).
- Use a **HashMap** when values are large, sparse, or unbounded.
- Core building block for anagrams, majority element, counting sort, duplicate checks.

---

## Interview Definition ⭐

> **A frequency array is a counting technique that precomputes how often each value occurs by using the value as a direct array index, trading O(n) upfront build time for O(1) count lookups — it applies when the value range is small and bounded, otherwise a HashMap serves the same purpose for arbitrary values.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Valid Anagram](https://leetcode.com/problems/valid-anagram/) | Easy | The textbook frequency-array application |
| 2 | [Majority Element](https://leetcode.com/problems/majority-element/) | Easy | Counting-based approach (also solvable via Boyer-Moore) |
| 3 | [Group Anagrams](https://leetcode.com/problems/group-anagrams/) | Medium | Frequency signature used as a hashmap key |
| 4 | [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | Medium | Frequency counting combined with a heap/bucket sort |
| 5 | [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/) | Medium | Sliding window + frequency array comparison |
| 6 | [First Unique Character in a String](https://leetcode.com/problems/first-unique-character-in-a-string/) | Easy | Direct frequency array lookup pattern |
