# Dynamic Programming

Dynamic Programming, or DP, is a way to solve problems by saving answers to smaller problems.

It is used when the same smaller problem appears again and again.

Instead of recomputing the answer, we store it and reuse it.

## The Core Idea

DP works when a problem has two properties:

1. Overlapping subproblems.
2. Optimal substructure.

Overlapping subproblems means the same smaller problem repeats.

Optimal substructure means the answer to the big problem can be built from answers to smaller problems.

## Fibonacci Example

Naive recursion repeats work.

```js
function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}
```

To compute `fib(5)`, it computes `fib(4)` and `fib(3)`.

But `fib(4)` also computes `fib(3)`.

So `fib(3)` is repeated.

This repeated work grows quickly.

## Memoization

Memoization is top-down DP.

You write recursion, but store answers.

```js
function fib(n, memo = new Map()) {
  if (n <= 1) return n;

  if (memo.has(n)) {
    return memo.get(n);
  }

  const answer = fib(n - 1, memo) + fib(n - 2, memo);
  memo.set(n, answer);

  return answer;
}
```

Now each `n` is computed once.

Time complexity becomes `O(n)`.

Space complexity is `O(n)`.

## Tabulation

Tabulation is bottom-up DP.

You fill a table from smaller answers to larger answers.

```js
function fib(n) {
  if (n <= 1) return n;

  const dp = Array(n + 1).fill(0);
  dp[0] = 0;
  dp[1] = 1;

  for (let i = 2; i <= n; i++) {
    dp[i] = dp[i - 1] + dp[i - 2];
  }

  return dp[n];
}
```

The table stores answers.

Each answer is built from previous answers.

## The Most Important Skill: State

The hardest part of DP is defining the state.

A state is the meaning of one DP entry.

Examples:

```txt
dp[i] = answer using first i items
dp[i] = minimum cost to reach index i
dp[i][j] = answer using first i chars of s and first j chars of t
dp[index][remaining] = best answer from index with remaining capacity
```

If the state is unclear, the solution feels like magic.

Before writing code, write:

“dp[...] means ...”

## Transition

The transition explains how to compute one state from smaller states.

For climbing stairs:

```txt
dp[i] = dp[i - 1] + dp[i - 2]
```

Why?

To reach step `i`, you came from step `i - 1` or step `i - 2`.

The transition is the logic of the problem.

## Base Cases

Base cases are the starting answers.

Without base cases, the table has nothing to build from.

For Fibonacci:

```txt
dp[0] = 0
dp[1] = 1
```

For climbing stairs:

```txt
dp[0] = 1
dp[1] = 1
```

Base cases depend on the meaning of the state.

## 1D DP

1D DP uses one index.

Example: house robber.

At each house, you decide:

- Skip this house.
- Rob this house and add it to the best answer two houses back.

```js
function rob(nums) {
  let prev2 = 0;
  let prev1 = 0;

  for (const money of nums) {
    const take = prev2 + money;
    const skip = prev1;
    const current = Math.max(take, skip);

    prev2 = prev1;
    prev1 = current;
  }

  return prev1;
}
```

This uses `O(1)` space.

## 2D DP

2D DP often appears with two strings or two choices.

Example: Longest Common Subsequence.

`dp[i][j]` can mean:

The LCS length using the first `i` characters of string A and first `j` characters of string B.

If characters match, use diagonal previous answer.

If they do not match, take the best of skipping one character from either string.

## Knapsack Thinking

0/1 knapsack is a classic DP pattern.

For each item, you choose:

- Do not take it.
- Take it if capacity allows.

The state can be:

```txt
dp[i][capacity]
```

Meaning:

The best value using items from index `i` with remaining capacity.

This “take or skip” idea appears in many DP problems.

## Common Mistakes

The first mistake is starting with code before defining the state.

The second mistake is using too much state.

A state should store only what is needed for the future.

The third mistake is wrong base cases.

The fourth mistake is filling the table in the wrong order.

The fifth mistake is not noticing repeated subproblems.

## Complexity

DP time is usually:

```txt
number of states * work per state
```

DP space is usually:

```txt
number of states stored
```

If there are `n` states and each takes `O(1)` work, time is `O(n)`.

If there is an `n * m` table, space may be `O(nm)`.

## Interview Explanation

A strong explanation sounds like this:

“I use DP because the same subproblems repeat. I define dp[i] as the answer up to index i. The transition chooses between the possible previous states. The time complexity is the number of states times the work per state.”

This is much better than saying “I memorized this formula.”

## Final Understanding

DP is not about memorizing patterns.

It is about naming a smaller problem clearly.

Once the state is clear, the transition and code become much easier.

