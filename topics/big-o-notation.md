# Big-O Notation

Big-O notation is a way to describe how an algorithm grows when the input grows.

It does not tell us the exact running time in seconds.

It tells us the growth pattern.

That means Big-O helps us answer questions like:

- What happens if the input size becomes 10 times bigger?
- Will this code still work for 100,000 items?
- Is this solution fast enough for an interview or real system?
- Am I using too much extra memory?

Big-O is one of the most important basics in Data Structures and Algorithms because it helps you compare solutions before you even run them.

## Why Big-O Exists

Imagine you write code that works for an array of 10 numbers.

That is nice, but it does not prove much.

The real question is:

What happens when the array has 10,000 numbers?

Or 1,000,000 numbers?

Some code grows slowly. Some code grows very fast. Big-O gives us a simple language to talk about that growth.

For example, this code checks every number once:

```js
for (let i = 0; i < nums.length; i++) {
  console.log(nums[i]);
}
```

If `nums` has 10 values, the loop runs 10 times.

If `nums` has 1,000 values, the loop runs 1,000 times.

The work grows directly with the input size.

So we call this `O(n)`.

Here, `n` means the size of the input.

## What Big-O Really Means

Big-O describes the upper growth rate of an algorithm.

In simple words:

Big-O tells us how bad the work can get as the input becomes large.

It ignores small details like exact CPU speed, programming language, or tiny constant differences.

For example:

```js
for (let i = 0; i < nums.length; i++) {
  console.log(nums[i]);
}

for (let i = 0; i < nums.length; i++) {
  console.log(nums[i] * 2);
}
```

This code has two separate loops.

If the array has `n` values, the first loop runs `n` times and the second loop runs `n` times.

Total work is `2n`.

But in Big-O, we write this as `O(n)`, not `O(2n)`.

Why?

Because Big-O cares about the shape of growth, not the exact count.

When `n` becomes very large, `2n` still grows like `n`.

## The Main Rule

Big-O keeps the part that grows the fastest and removes constants.

So:

- `O(5)` becomes `O(1)`
- `O(2n)` becomes `O(n)`
- `O(n + 100)` becomes `O(n)`
- `O(n² + n)` becomes `O(n²)`
- `O(3n² + 10n + 50)` becomes `O(n²)`

This may feel strange at first.

But the reason is simple.

For large input sizes, the fastest-growing term dominates everything else.

If `n = 1,000,000`, then `n²` is much bigger than `n`.

So when an algorithm has both `n²` work and `n` work, the `n²` part decides the real scalability.

## O(1): Constant Time

`O(1)` means the work does not grow with the input size.

Example:

```js
function getFirst(nums) {
  return nums[0];
}
```

This function reads the first item.

It does not matter if the array has 10 items or 10 million items.

The function still performs one direct access.

So the time complexity is `O(1)`.

Another example:

```js
function isEven(num) {
  return num % 2 === 0;
}
```

This also does one small calculation.

It does not depend on the size of a list.

So it is `O(1)`.

Constant time is usually the best kind of time complexity.

But remember: `O(1)` does not always mean instant. It only means the work does not grow with input size.

## O(n): Linear Time

`O(n)` means the work grows directly with the input size.

Example:

```js
function findTarget(nums, target) {
  for (let i = 0; i < nums.length; i++) {
    if (nums[i] === target) {
      return true;
    }
  }

  return false;
}
```

This function searches for a target.

In the best case, the target is the first item.

In the worst case, the target is at the end or not present at all.

Then the function may check every element.

If there are `n` elements, the worst-case work is `n`.

So the time complexity is `O(n)`.

Linear time is common and usually acceptable.

Most single-pass array or string solutions are `O(n)`.

## O(n²): Quadratic Time

`O(n²)` usually appears when we use nested loops.

Example:

```js
function printPairs(nums) {
  for (let i = 0; i < nums.length; i++) {
    for (let j = 0; j < nums.length; j++) {
      console.log(nums[i], nums[j]);
    }
  }
}
```

If `nums` has 5 values, the outer loop runs 5 times.

For each outer loop, the inner loop also runs 5 times.

Total work is `5 * 5 = 25`.

If `nums` has `n` values, total work is `n * n`.

So the complexity is `O(n²)`.

This grows quickly.

If `n = 1,000`, then `n² = 1,000,000`.

If `n = 100,000`, then `n² = 10,000,000,000`.

That is usually too slow.

Many beginner solutions start as `O(n²)`. That is fine. First understand the brute force idea. Then look for repeated work and try to remove it.

## O(log n): Logarithmic Time

`O(log n)` usually means the algorithm cuts the search space down again and again.

The best example is binary search.

```js
function binarySearch(nums, target) {
  let left = 0;
  let right = nums.length - 1;

  while (left <= right) {
    const mid = left + Math.floor((right - left) / 2);

    if (nums[mid] === target) {
      return mid;
    }

    if (nums[mid] < target) {
      left = mid + 1;
    } else {
      right = mid - 1;
    }
  }

  return -1;
}
```

Binary search works only when the array is sorted.

Each step removes half of the remaining values.

If there are 16 values:

- After 1 step, about 8 remain.
- After 2 steps, about 4 remain.
- After 3 steps, about 2 remain.
- After 4 steps, about 1 remains.

So 16 values take about 4 steps.

That is because `2⁴ = 16`.

This is the idea behind `log n`.

`O(log n)` is very fast because the input shrinks aggressively.

Even for 1,000,000 items, binary search takes only about 20 steps.

## O(n log n)

`O(n log n)` often appears in efficient sorting algorithms.

Examples include merge sort, heap sort, and average-case quick sort.

The idea is usually:

Do `log n` levels of splitting, and each level does `n` total work.

Merge sort is a good example.

It splits the array into halves until each piece is small. Then it merges the pieces back together.

The number of split levels is about `log n`.

At each level, the algorithm touches all `n` elements while merging.

So total work is `O(n log n)`.

`O(n log n)` is slower than `O(n)` but much better than `O(n²)` for large inputs.

## O(2ⁿ): Exponential Time

`O(2ⁿ)` grows extremely fast.

It often appears when each step creates two choices.

Example:

```js
function fib(n) {
  if (n <= 1) {
    return n;
  }

  return fib(n - 1) + fib(n - 2);
}
```

This recursive Fibonacci solution repeats a lot of work.

To compute `fib(5)`, it computes `fib(4)` and `fib(3)`.

Then `fib(4)` also computes `fib(3)`.

The same smaller problems are solved again and again.

This creates a growing tree of calls.

That is why simple recursive Fibonacci is exponential.

Exponential solutions are often too slow unless `n` is very small.

Dynamic programming is commonly used to improve these cases by saving repeated answers.

## Time Complexity vs Space Complexity

Time complexity measures how much work the algorithm does.

Space complexity measures how much extra memory the algorithm uses.

Example:

```js
function hasDuplicate(nums) {
  const seen = new Set();

  for (const num of nums) {
    if (seen.has(num)) {
      return true;
    }

    seen.add(num);
  }

  return false;
}
```

This function scans the array once.

So time complexity is `O(n)`.

It also stores numbers in a set.

In the worst case, it may store all `n` numbers.

So space complexity is `O(n)`.

This is a common tradeoff.

We use extra memory to make the algorithm faster.

Without the set, we might use nested loops and get `O(n²)` time with `O(1)` extra space.

With the set, we get `O(n)` time with `O(n)` extra space.

## Best Case, Worst Case, and Average Case

Some algorithms behave differently depending on the input.

Example:

```js
function findTarget(nums, target) {
  for (const num of nums) {
    if (num === target) {
      return true;
    }
  }

  return false;
}
```

If the target is first, the function stops immediately.

That is best case `O(1)`.

If the target is last or missing, the function checks everything.

That is worst case `O(n)`.

In interviews, when someone asks for Big-O, they usually expect worst-case complexity unless they say otherwise.

Worst case is useful because it gives a safe upper bound.

## How to Calculate Big-O

Use this process:

1. Find what `n` means.

2. Count how many times the main work runs.

3. Look for loops, nested loops, recursion, and data structure operations.

4. Keep the fastest-growing term.

5. Remove constants.

Example:

```js
function example(nums) {
  for (let i = 0; i < nums.length; i++) {
    console.log(nums[i]);
  }

  for (let i = 0; i < nums.length; i++) {
    for (let j = 0; j < nums.length; j++) {
      console.log(nums[i], nums[j]);
    }
  }
}
```

The first loop is `O(n)`.

The nested loop is `O(n²)`.

Total is `O(n + n²)`.

The `n²` part grows faster.

So the final complexity is `O(n²)`.

## Different Inputs

Sometimes a function has two different inputs.

Example:

```js
function printBoth(a, b) {
  for (const x of a) {
    console.log(x);
  }

  for (const y of b) {
    console.log(y);
  }
}
```

If array `a` has `n` items and array `b` has `m` items, the complexity is `O(n + m)`.

Do not call it `O(n)` unless both inputs are tied to the same size.

Another example:

```js
function printPairs(a, b) {
  for (const x of a) {
    for (const y of b) {
      console.log(x, y);
    }
  }
}
```

Here, every item in `a` is paired with every item in `b`.

If `a` has `n` items and `b` has `m` items, the complexity is `O(n * m)`.

This detail matters in real systems and interviews.

## Common Data Structure Operations

These are common average-case costs:

## | Operation | Common Complexity |

## | --- | --- |

## | Array access by index | `O(1)` |

## | Array search | `O(n)` |

## | Array insert at end | Usually `O(1)` amortized |

## | Array insert at beginning | `O(n)` |

## | Hash map lookup | Usually `O(1)` average |

## | Hash set lookup | Usually `O(1)` average |

## | Stack push/pop | `O(1)` |

## | Queue enqueue/dequeue | `O(1)` with proper implementation |

## | Binary search | `O(log n)` |

## | Sorting | Usually `O(n log n)` |

These costs help you estimate full algorithm complexity.

For example, if you loop through an array and do a hash map lookup each time, the total time is usually `O(n)`.

That is because the loop runs `n` times and each lookup is average `O(1)`.

## Common Mistakes

One common mistake is thinking two loops always mean `O(n²)`.

That is not always true.

Two separate loops are usually `O(n + n)`, which becomes `O(n)`.

Nested loops are usually where `O(n²)` comes from.

Another common mistake is ignoring hidden work.

Example:

```js
for (const item of items) {
  copied = items.slice();
}
```

The loop runs `n` times.

But `slice()` also copies `n` items.

So the total work may be `O(n²)`.

A third mistake is forgetting space complexity.

If you build a new array, map, set, recursion stack, or DP table, that memory counts.

## How to Explain Big-O in an Interview

A clean interview explanation sounds like this:

“The algorithm scans the array once, so the time complexity is O(n), where n is the number of elements. It uses a hash set that can store up to n elements, so the space complexity is O(n).”

That is simple and strong.

Another example:

“The outer loop runs n times and the inner loop runs n times for each outer iteration, so the time complexity is O(n²). The algorithm only uses a few variables, so the extra space is O(1).”

When explaining Big-O, always say what `n` means.

Do not only say `O(n)`.

Say `O(n), where n is the length of the array`.

That small detail makes the explanation much clearer.

## Big-O From a Principal Engineer View

At a senior or principal level, Big-O is not just interview theory.

It is a tool for design decisions.

If a feature processes 100 items once a day, an `O(n²)` solution may be acceptable.

If a service processes millions of records every minute, that same solution may be dangerous.

Good engineers think about scale, data size, expected usage, and tradeoffs.

Sometimes the simplest solution is best because the input is small.

Sometimes you must optimize because growth will hurt users, servers, or cost.

Big-O helps you make that decision with a clear reason.

## Quick Mental Guide

Use this as a fast guide:

- One direct access usually means `O(1)`.
- One loop over input usually means `O(n)`.
- Two separate loops usually still mean `O(n)`.
- A loop inside a loop often means `O(n²)`.
- Cutting the input in half repeatedly often means `O(log n)`.
- Efficient comparison sorting usually means `O(n log n)`.
- Recursive branching without memoization can become exponential.
- Extra arrays, maps, sets, recursion stacks, and DP tables count as space.

## Final Understanding

Big-O is not about memorizing symbols.

It is about understanding growth.

When you look at code, ask:

- What is the input size?
- How many times does the main work happen?
- Does the algorithm scan, nest, split, branch, or store?
- What extra memory is used?
- Which part grows the fastest?

If you can answer these questions, you can understand Big-O clearly.

Once Big-O becomes clear, every other DSA topic becomes easier because you can judge whether a solution is just correct or actually efficient.
