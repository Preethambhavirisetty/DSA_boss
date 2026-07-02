# Backtracking

Backtracking is a way to explore choices.

It tries one choice, continues from there, and then undoes the choice before trying another one.

It is used when you need to generate or search through many possibilities.

## The Core Shape

Backtracking usually follows this pattern:

```txt
choose
explore
undo
```

Code shape:

```js
function backtrack(path) {
  if (isComplete(path)) {
    result.push([...path]);
    return;
  }

  for (const choice of choices) {
    path.push(choice);
    backtrack(path);
    path.pop();
  }
}
```

The `path.pop()` is the undo step.

Without it, choices from one branch leak into another branch.

## Subsets Example

For `[1, 2, 3]`, subsets are:

```txt
[]
[1]
[2]
[3]
[1, 2]
[1, 3]
[2, 3]
[1, 2, 3]
```

Code:

```js
function subsets(nums) {
  const result = [];

  function dfs(index, path) {
    if (index === nums.length) {
      result.push([...path]);
      return;
    }

    dfs(index + 1, path);

    path.push(nums[index]);
    dfs(index + 1, path);
    path.pop();
  }

  dfs(0, []);
  return result;
}
```

At each index, there are two choices:

Do not take the number.

Take the number.

## State

Backtracking is all about state.

The state tells you what choices have already been made.

Examples:

- Current path.
- Current index.
- Current sum.
- Used numbers.
- Board state.
- Remaining choices.

If your state is clear, the recursion becomes clear.

## Pruning

Pruning means stopping early when a path cannot lead to a valid answer.

Example:

If you are building combinations that sum to a target and the current sum is already bigger than the target, stop.

```js
if (sum > target) return;
```

Pruning can save a lot of time.

It does not change the worst-case complexity for many problems, but it helps in practice.

## Permutations

Permutation problems require tracking used elements.

```js
function permute(nums) {
  const result = [];
  const used = new Set();

  function dfs(path) {
    if (path.length === nums.length) {
      result.push([...path]);
      return;
    }

    for (const num of nums) {
      if (used.has(num)) continue;

      used.add(num);
      path.push(num);

      dfs(path);

      path.pop();
      used.delete(num);
    }
  }

  dfs([]);
  return result;
}
```

Notice the undo:

```js
path.pop();
used.delete(num);
```

Both are needed.

## Complexity

Backtracking often has exponential complexity.

Subsets have `2^n` possibilities.

Permutations have `n!` possibilities.

This is expected.

Backtracking is used when the output itself may be very large.

## Common Problems

Backtracking appears in:

- Subsets.
- Permutations.
- Combination Sum.
- N-Queens.
- Sudoku Solver.
- Word Search.
- Generate Parentheses.

## Common Mistakes

The first mistake is forgetting to undo state.

The second mistake is adding `path` directly instead of copying it.

Use:

```js
result.push([...path]);
```

Not:

```js
result.push(path);
```

The third mistake is not pruning invalid branches.

The fourth mistake is mixing up index-based choices and used-set choices.

## Interview Explanation

A strong explanation sounds like this:

“I use backtracking because the problem asks us to explore possible choices. At each step, I choose an option, recurse, and then undo the choice. The recursion tree represents all possible candidates. The time complexity depends on the number of generated states.”

This shows the structure clearly.

## Final Understanding

Backtracking is controlled trial and error.

It is not random.

It explores choices systematically and cleans up after each branch.

