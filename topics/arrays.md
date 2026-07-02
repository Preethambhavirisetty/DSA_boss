# Arrays

An array is a collection of values stored in order.

Each value has a position called an index. In most programming languages, the first index is `0`.

Arrays are one of the most important DSA topics because many other patterns are built on top of them.

If you understand arrays deeply, topics like two pointers, sliding window, prefix sum, sorting, binary search, and dynamic programming become easier.

## The Core Idea

An array gives you fast access when you know the index.

```js
const nums = [10, 20, 30, 40];
console.log(nums[2]); // 30
```

This is `O(1)` because the program can jump directly to index `2`.

But if you do not know the index and you are searching by value, you may need to scan the whole array.

```js
function contains(nums, target) {
  for (const num of nums) {
    if (num === target) return true;
  }

  return false;
}
```

This is `O(n)` because the target may be at the end or missing.

## Why Arrays Matter

Arrays are simple, but they teach the most important problem-solving habit: scanning while keeping useful state.

For example, to find the largest number, you do not need to compare every pair.

You only need to remember the best value seen so far.

```js
function maxValue(nums) {
  let best = nums[0];

  for (const num of nums) {
    best = Math.max(best, num);
  }

  return best;
}
```

The array is scanned once.

The variable `best` stores the useful state.

This is the basic shape of many array solutions.

## Memory Model

Arrays are usually stored in a continuous block of memory.

That is why index access is fast.

If the starting address is known, the program can calculate where index `i` lives.

This is also why inserting in the middle can be slow.

If you insert a value at the beginning, many elements may need to shift one position.

Access by index is fast. Structural changes near the front or middle are usually slower.

## Common Operations

| Operation | Complexity |
| --- | --- |
| Read by index | `O(1)` |
| Update by index | `O(1)` |
| Search by value | `O(n)` |
| Insert at end | Usually `O(1)` amortized |
| Insert at beginning | `O(n)` |
| Delete from beginning | `O(n)` |

## Brute Force Thinking

Many array problems start with brute force.

Example: find two numbers that add to a target.

The direct idea is to check every pair.

```js
function twoSumBrute(nums, target) {
  for (let i = 0; i < nums.length; i++) {
    for (let j = i + 1; j < nums.length; j++) {
      if (nums[i] + nums[j] === target) {
        return [i, j];
      }
    }
  }

  return [];
}
```

This is `O(n^2)`.

It is not the best solution, but it is a good starting point because it makes the problem clear.

After brute force, ask:

What work is repeated?

In Two Sum, we repeatedly search for the number that pairs with the current number.

A hash map can remember values already seen.

```js
function twoSum(nums, target) {
  const seen = new Map();

  for (let i = 0; i < nums.length; i++) {
    const need = target - nums[i];

    if (seen.has(need)) {
      return [seen.get(need), i];
    }

    seen.set(nums[i], i);
  }

  return [];
}
```

Now the time is `O(n)` and the space is `O(n)`.

This is a classic array tradeoff: use extra memory to remove repeated scanning.

## Important Array Patterns

Two pointers use two indexes to move through the array intelligently.

Sliding window keeps a section of the array and moves its edges.

Prefix sum stores running totals so range sum queries become fast.

Sorting creates order, which can make searching and comparison easier.

Hashing remembers previous values, counts, or indexes.

Binary search works when the array is sorted or when the answer space is ordered.

## How to Dry Run Array Code

Use a table.

Write index, value, and your important variables.

For example, if you track max value:

| index | value | best |
| --- | --- | --- |
| 0 | 3 | 3 |
| 1 | 8 | 8 |
| 2 | 2 | 8 |
| 3 | 10 | 10 |

This makes the internal state visible.

Most array bugs happen when people skip the dry run and guess what the variables are doing.

## Common Mistakes

The first mistake is going out of bounds.

If an array has length `n`, the last index is `n - 1`.

The second mistake is forgetting empty arrays.

If you write `nums[0]`, ask what happens when `nums` is empty.

The third mistake is confusing index and value.

`i` is the position. `nums[i]` is the value at that position.

The fourth mistake is using nested loops too quickly.

Nested loops may be fine for brute force, but always ask if a map, set, pointer, or running state can remove repeated work.

## Interview Explanation

A strong explanation sounds like this:

“I will scan the array once. While scanning, I will keep the useful information I need for future elements. This avoids checking all pairs. The time complexity is O(n), where n is the number of elements. The extra space depends on what I store.”

That explanation shows you understand the algorithm, not just the syntax.

## Final Understanding

An array is simple on the surface.

But most DSA thinking starts here.

If you can scan an array, track state, handle boundaries, and explain time and space, you already have the foundation for many interview problems.

