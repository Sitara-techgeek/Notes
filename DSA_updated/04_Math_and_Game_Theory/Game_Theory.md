# Game Theory

> **📅 Written:** 24 Sep 2026

### Definition

**Game Theory (in DSA) analyzes two-player turn-based games with perfect information to determine which player has a winning strategy, typically by classifying game states as "winning" or "losing" based on the states reachable from them.**

**In one sentence:**
> Work backward from the states where the game is obviously over, and every other state's status (win or lose for the player about to move) falls out from whether it can force the opponent into a losing state.

---

## Core Idea: Winning and Losing States

- A state is a **Losing State (L)** if the player **about to move** has no way to avoid eventually losing (every move leads to a Winning state for the opponent).
- A state is a **Winning State (W)** if the player about to move can make **at least one move** to a Losing state for the opponent.

```
Base case: no moves left -> Losing state (you can't move, you lose)

State is WINNING if: at least one reachable state is LOSING (for opponent)
State is LOSING if:  every reachable state is WINNING (for opponent)
```

> **Interview point ⭐:** This "W if any move leads to L; L if all moves lead to W" recursive definition is the single most important idea in game theory problems — it directly translates into a recursive/DP solution over game states.

---

## Worked Example: Nim Game

**Rules:** Piles of stones; players alternate removing any number of stones from a **single** pile; the player who removes the **last stone wins**.

**Key result:** The first player wins **if and only if** the XOR of all pile sizes is **non-zero**.

```java
boolean firstPlayerWins(int[] piles) {
    int xorSum = 0;
    for (int pile : piles) {
        xorSum ^= pile;
    }
    return xorSum != 0;
}
```

**Why XOR?** (intuition, not full proof):
- If XOR = 0, any move you make on one pile changes that pile's contribution, making the total XOR non-zero — handing the opponent a "fixable" (winning) position.
- If XOR ≠ 0, there's always a move that reduces some pile to make the XOR exactly 0, handing the opponent a losing position.

```
Piles: [3, 4, 5]
3 ^ 4 ^ 5 = 011 ^ 100 ^ 101 = 010 = 2  (non-zero) -> First player WINS
```

> **Interview point ⭐:** The plain Nim game result (XOR of piles) is one of the most famous results in combinatorial game theory and shows up disguised in many "stone game" variants — recognizing the underlying Nim structure is often the whole trick.

---

## Simple Game States via DP — Stone Game Variant

**Rules:** A single pile of `n` stones; players alternate removing 1, 2, or 3 stones; whoever removes the last stone wins. Determine if the first player wins.

```java
boolean canWin(int n) {
    boolean[] dp = new boolean[n + 1];
    dp[0] = false;   // no stones left, current player loses (can't move)

    for (int i = 1; i <= n; i++) {
        // Win if ANY move (removing 1, 2, or 3) leads to a LOSING state for opponent
        dp[i] = (i >= 1 && !dp[i - 1]) || (i >= 2 && !dp[i - 2]) || (i >= 3 && !dp[i - 3]);
    }
    return dp[n];
}
```

```
dp[0] = false  (Losing - no moves)
dp[1] = true   (can take 1, leaving dp[0]=false for opponent -> WIN)
dp[2] = true   (can take 2, leaving dp[0]=false -> WIN)
dp[3] = true   (can take 3, leaving dp[0]=false -> WIN)
dp[4] = false  (taking 1,2,3 leaves dp[3],dp[2],dp[1] all TRUE for opponent -> every move loses)
```
→ **O(n)** — a direct DP translation of the win/lose recursive definition.

---

## Optimal Strategy Games — Minimax-Style DP

For games where players pick from **either end** of an array (common "Stone Game" LeetCode family), track the **net score difference** the current player can guarantee, assuming **both players play optimally**.

```java
// Returns the max score difference (current player's score - opponent's score) achievable
int maxScoreDiff(int[] piles, int left, int right, int[][] memo) {
    if (left > right) return 0;
    if (memo[left][right] != Integer.MIN_VALUE) return memo[left][right];

    // Taking from the left OR right, then the OPPONENT plays optimally on what's left
    int takeLeft = piles[left] - maxScoreDiff(piles, left + 1, right, memo);
    int takeRight = piles[right] - maxScoreDiff(piles, left, right - 1, memo);

    memo[left][right] = Math.max(takeLeft, takeRight);
    return memo[left][right];
}

boolean firstPlayerWinsStoneGame(int[] piles) {
    int n = piles.length;
    int[][] memo = new int[n][n];
    for (int[] row : memo) Arrays.fill(row, Integer.MIN_VALUE);
    return maxScoreDiff(piles, 0, n - 1, memo) > 0;
}
```

> **Interview point ⭐:** The "subtract the opponent's optimal result" trick (`piles[left] - maxScoreDiff(...)`) is the standard way to encode **minimax** reasoning (maximize my gain while assuming the opponent also plays to minimize mine / maximize theirs) into a single recursive value, instead of tracking two players' scores separately.

---

## Sprague-Grundy Theorem (Advanced, Brief Overview)

For more complex combinatorial games (especially combinations of independent sub-games), each game state can be assigned a **Grundy number (nimber)** — the smallest non-negative integer **not** present among the Grundy numbers of states reachable from it (the "mex" — minimum excludant).

```
Grundy(state) = mex{ Grundy(next_state) : next_state reachable from state }

A state is LOSING if and only if Grundy(state) == 0.
```

- For a **combination of independent games**, the overall Grundy number is the **XOR** of each sub-game's Grundy number — this generalizes the plain Nim result (each pile is an independent sub-game, its Grundy number equals its size).

> **Interview point ⭐:** Sprague-Grundy is a more advanced tool, but the practical takeaway that reappears often: *whenever a game decomposes into independent sub-games, XOR their individual results/Grundy numbers to determine the overall winner* — this is exactly why Nim's rule generalizes so widely.

---

## Time Complexity — Quick Table

| Approach | Time Complexity |
|---|---|
| Nim Game (XOR check) | O(n) — one pass over piles |
| Simple win/lose DP (1D state) | O(n) |
| Minimax DP over a range (2D state) | O(n²) |
| Grundy number computation (per state) | Depends on branching factor and state space |

---

## Quick Revision

- **Winning state** → at least one move leads to a Losing state for the opponent.
- **Losing state** → every move leads to a Winning state for the opponent.
- **Nim Game** → first player wins iff XOR of all pile sizes ≠ 0.
- **Minimax DP** → `piles[left] - recurse(...)` encodes "maximize my score while the opponent also plays optimally against me".
- **Sprague-Grundy / mex** → generalizes Nim; XOR of independent sub-games' Grundy numbers determines the overall winner.

---

## Interview Definition ⭐

> **Game theory problems classify each game state as winning or losing for the player about to move — winning if any reachable state is losing for the opponent, losing if every reachable state is winning — which translates directly into recursive or DP solutions; the Nim game's famous result (first player wins iff the XOR of pile sizes is non-zero) is a special case of the more general Sprague-Grundy theorem, which assigns each state a Grundy number so that combinations of independent games can be resolved via XOR.**

---

## Must-Solve LeetCode Problems

| # | Problem | Difficulty | Why it matters |
|---|---------|-----------|-----------------|
| 1 | [Nim Game](https://leetcode.com/problems/nim-game/) | Easy | The foundational win/lose state recursion (special case of the general Nim result) |
| 2 | [Stone Game](https://leetcode.com/problems/stone-game/) | Medium | The classic "pick from either end" minimax DP |
| 3 | [Predict the Winner](https://leetcode.com/problems/predict-the-winner/) | Medium | Same minimax pattern as Stone Game, generalized |
| 4 | [Divisor Game](https://leetcode.com/problems/divisor-game/) | Easy | A simple win/lose DP with a non-obvious closed-form answer |
| 5 | [Flip Game II](https://leetcode.com/problems/flip-game-ii/) | Medium | Win/lose state search over string states |
| 6 | [Stone Game II](https://leetcode.com/problems/stone-game-ii/) | Medium | Extended minimax with a variable move-count parameter |
