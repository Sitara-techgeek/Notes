# Two Pointers and Sliding Window

> **📅 Written:** 24 Sep 2026

### Definition

**Two Pointers uses two indices moving through a data structure (toward each other, or in the same direction at different speeds) to avoid nested loops. Sliding Window is a specialized form of two pointers where the pointers define a contiguous range that expands and contracts to track a running condition.**

**In one sentence:**
> Instead of re-scanning from scratch for every possible pair or subarray (O(n²)), you keep one or two markers moving forward through the data, reusing work already done — turning brute force into a single O(n) pass.

---

## Two Pointers — The Core Idea

### Pattern A: Opposite-Direction Pointers (converging)

Used heavily on **sorted arrays** — one pointer starts at the beginning, one at the end, and they move toward each other based on a comparison.

```
[2, 7, 11, 15]  target = 9
 L           R
 L        R        -> arr[L]+arr[R] = 2+15=17 > 9, move R left
 L     R           -> 2+11=13 > 9, move R left
 L  R              -> 2+7=9 == 9, found!
```

```java
int[] twoSumSorted(int[] arr, int target) {
    int left = 0, right = arr.length - 1;
    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == target) return new int[]{left, right};
        else if (sum < target) left++;     // need a bigger sum -> move left pointer right
        else right--;                       // need a smaller sum -> move right pointer left
    }
    return new int[]{-1, -1};
}
```
→ **O(n)** instead of the O(n²) brute-force pair check.

### Pattern B: Same-Direction Pointers (fast & slow)

Used for **in-place array modification** — one pointer tracks the "write position", the other scans ahead.

```java
// Remove duplicates from a sorted array, in-place
int removeDuplicates(int[] arr) {
    int slow = 0;
    for (int fast = 1; fast < arr.length; fast++) {
        if (arr[fast] != arr[slow]) {
            slow++;
            arr[slow] = arr[fast];
        }
    }
    return slow + 1;   // new length
}
```

---

## Sliding Window — The Core Idea

A **window** is a contiguous subarray/substring defined by `[left, right]`. As `right` expands the window, `left` contracts it when some condition is violated — the window "slides" across the data.

```
Find longest substring with at most 2 distinct characters: "eceba"

right=0: "e"          window ok
right=1: "ec"          window ok
right=2: "ece"          window ok
right=3: "eceb" -> 3 distinct, shrink from left
         "ceb"          window ok again
right=4: "ceba" -> 3 distinct, shrink
         "eba"          window ok
```

### Fixed-Size Window

```java
int maxSumFixedWindow(int[] arr, int k) {
    int windowSum = 0;
    for (int i = 0; i < k; i++) windowSum += arr[i];   // build initial window
    int maxSum = windowSum;

    for (int right = k; right < arr.length; right++) {
        windowSum += arr[right] - arr[right - k];   // add new, remove oldest
        maxSum = Math.max(maxSum, windowSum);
    }
    return maxSum;
}
```
→ **O(n)** — avoids recomputing the sum from scratch for every window (which would be O(n·k)).

### Variable-Size Window (expand & contract)

```java
int lengthOfLongestSubstring(String s) {
    Set<Character> window = new HashSet<>();
    int left = 0, maxLen = 0;

    for (int right = 0; right < s.length(); right++) {
        while (window.contains(s.charAt(right))) {
            window.remove(s.charAt(left));
            left++;                                  // shrink until valid again
        }
        window.add(s.charAt(right));
        maxLen = Math.max(maxLen, right - left + 1);
    }
    return maxLen;
}
```
→ **O(n)** — each character is added and removed from the window **at most once**, even though there's a nested-looking `while` loop (a very common source of confusion — it's still linear).

> **Interview point ⭐:** The key insight that makes sliding window O(n), not O(n²), is that `left` only ever moves **forward**, never backward — across the entire algorithm, `left` and `right` each traverse the array once, total work is O(2n) = O(n).

---

## The General Sliding Window Template

```java
int slidingWindowTemplate(String s) {
    Map<Character, Integer> windowCount = new HashMap<>();
    int left = 0, result = 0;

    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        windowCount.merge(c, 1, Integer::sum);   // expand: include s[right]

        while (/* window is invalid */ false) {  // shrink while invalid
            char leftChar = s.charAt(left);
            windowCount.merge(leftChar, -1, Integer::sum);
            left++;
        }

        result = Math.max(result, right - left + 1);   // update answer with current valid window
    }
    return result;
}
```

---

## Recognizing Which Pattern to Use ⭐

| Signal in the problem | Likely Pattern |
|---|---|
| "sorted array", "pair that sums to..." | Two Pointers (opposite direction) |
| "remove duplicates in-place", "partition array" | Two Pointers (same direction / fast-slow) |
| "longest/shortest subarray/substring with condition X" | Sliding Window (variable size) |
| "subarray of exactly size k" | Sliding Window (fixed size) |
| "contains all characters of...", "minimum window containing..." | Sliding Window (variable, with a frequency map) |

---

## Time Complexity — Quick Table

| Technique | Naive Brute Force | With Two Pointers / Sliding Window |
|---|---|---|
| Pair sum in sorted array | O(n²) | O(n) |
| Fixed-size window sum/max | O(n·k) | O(n) |
| Longest substring with condition | O(n²) or O(n³) | O(n) |
| Minimum window substring | O(n²) | O(n) |

---

## Quick Revision

- **Two Pointers (opposite direction)** → sorted arrays, converging search, O(n).
- **Two Pointers (same direction)** → in-place modification, fast/slow roles.
- **Sliding Window (fixed size)** → reuse the previous window's sum, add new / remove oldest.
- **Sliding Window (variable size)** → expand with `right`, shrink with `left` while invalid — still O(n) total since `left` never goes backward.
- Both techniques exist to eliminate the need for **nested loops / recomputation**, converting O(n²) into O(n).

---

## Interview Definition ⭐

> **Two Pointers uses two indices moving through the data — either converging from opposite ends (common on sorted arrays) or moving together at different speeds — to eliminate nested-loop brute force; Sliding Window is a specialization where the pointers bound a contiguous range that expands and contracts based on a validity condition, both achieving O(n) by ensuring each pointer only ever advances forward.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Two Sum II - Input Array Is Sorted](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/) | Medium | The canonical opposite-direction two pointers problem |
| 2 | [Container With Most Water](https://leetcode.com/problems/container-with-most-water/) | Medium | Two pointers with a greedy shrink-the-worse-side decision |
| 3 | [3Sum](https://leetcode.com/problems/3sum/) | Medium | Sort + fix one element + two pointers on the rest |
| 4 | [Remove Duplicates from Sorted Array](https://leetcode.com/problems/remove-duplicates-from-sorted-array/) | Easy | Same-direction fast/slow pointers |
| 5 | [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | Medium | The defining variable-size sliding window problem |
| 6 | [Maximum Average Subarray I](https://leetcode.com/problems/maximum-average-subarray-i/) | Easy | Simplest fixed-size sliding window |
| 7 | [Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/) | Hard | Advanced variable window with two frequency maps |
| 8 | [Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/) | Medium | Sliding window with a "budget" (k replacements) condition |
| 9 | [Permutation in String](https://leetcode.com/problems/permutation-in-string/) | Medium | Fixed-size window + frequency comparison |
