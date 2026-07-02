# Recursion

Recursion means a function calls itself.

At first, recursion can feel confusing because the function is not finished before it calls itself again.

The clean way to understand recursion is this:

Recursion solves a big problem by solving smaller versions of the same problem.

## The Core Idea

Every recursive solution needs two things:

1. A base case.
2. A recursive case.

The base case is where the function stops.

The recursive case is where the function calls itself with a smaller or simpler input.

Example:

```js
function countdown(n) {
  if (n === 0) {
    return;
  }

  console.log(n);
  countdown(n - 1);
}
```

If `n` is `3`, the calls are:

```txt
countdown(3)
countdown(2)
countdown(1)
countdown(0)
```

At `countdown(0)`, the base case stops the recursion.

## The Call Stack

When a function calls another function, the current function waits.

That waiting work is stored on the call stack.

In recursion, many calls may wait on the stack.

Example:

```js
function factorial(n) {
  if (n === 1) return 1;
  return n * factorial(n - 1);
}
```

For `factorial(4)`, think like this:

```txt
factorial(4) = 4 * factorial(3)
factorial(3) = 3 * factorial(2)
factorial(2) = 2 * factorial(1)
factorial(1) = 1
```

Then values return backward:

```txt
factorial(2) = 2 * 1 = 2
factorial(3) = 3 * 2 = 6
factorial(4) = 4 * 6 = 24
```

The calls go down first.

The answers come back up later.

## Trust the Smaller Problem

The most important recursion mindset is trust.

Do not try to mentally expand every call forever.

Instead, define what the function returns.

For example:

```js
function maxDepth(root) {
  if (!root) return 0;

  return 1 + Math.max(maxDepth(root.left), maxDepth(root.right));
}
```

The meaning of `maxDepth(node)` is:

Return the height of this subtree.

If you trust that `maxDepth(root.left)` gives the left height and `maxDepth(root.right)` gives the right height, then the answer is simple.

The current height is `1 + max(leftHeight, rightHeight)`.

This is how recursive tree problems become clear.

## Recursion vs Iteration

Many recursive solutions can be written iteratively.

Iteration uses loops.

Recursion uses function calls.

The difference is often style and structure.

Recursion is natural when the data itself is recursive.

Trees are recursive because every child is also a tree.

Backtracking is recursive because each choice leads to another smaller choice space.

Divide and conquer is recursive because the problem splits into smaller subproblems.

## Time Complexity

Recursive time complexity depends on how many calls are made.

Factorial makes one call per number.

So `factorial(n)` is `O(n)`.

Binary tree DFS visits each node once.

So DFS over a tree with `n` nodes is `O(n)`.

Naive Fibonacci is different.

```js
function fib(n) {
  if (n <= 1) return n;
  return fib(n - 1) + fib(n - 2);
}
```

Each call branches into two more calls.

Many values are recomputed.

This is exponential, roughly `O(2^n)`.

## Space Complexity

Recursion uses stack space.

If recursion goes `n` levels deep, the stack space is `O(n)`.

For a balanced binary tree, recursion depth is about `O(log n)`.

For a skewed tree, recursion depth can be `O(n)`.

This matters because very deep recursion can cause stack overflow.

## Backtracking

Backtracking is recursion with choices.

The pattern is:

1. Choose.
2. Explore.
3. Undo the choice.

Example shape:

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

The `path.pop()` is important.

It restores the state before trying the next choice.

## Memoization

Memoization means saving answers to recursive calls.

It is useful when the same state appears again and again.

Fibonacci becomes much faster with memoization.

```js
function fib(n, memo = new Map()) {
  if (n <= 1) return n;

  if (memo.has(n)) return memo.get(n);

  const ans = fib(n - 1, memo) + fib(n - 2, memo);
  memo.set(n, ans);
  return ans;
}
```

Now each `n` is computed once.

Time becomes `O(n)`.

Space is `O(n)`.

## Common Mistakes

The first mistake is missing the base case.

Without a base case, recursion never stops.

The second mistake is not moving toward the base case.

If the input does not become smaller or simpler, the recursion is broken.

The third mistake is not knowing what the function returns.

Before writing recursive code, write one sentence:

“This function returns...”

The fourth mistake is changing shared state and forgetting to undo it.

That often breaks backtracking.

## Interview Explanation

A clean recursion explanation sounds like this:

“I define the recursive function as the answer for the current node or current state. The base case handles the empty or finished state. Then I call the function on smaller states and combine the results. The time complexity is based on the number of states visited, and the space complexity includes the recursion stack.”

This shows you understand both code and execution.

## Final Understanding

Recursion is not magic.

It is a way to delay work while solving smaller problems.

If you know the base case, the recursive meaning, and how answers combine, recursion becomes one of the cleanest tools in DSA.

