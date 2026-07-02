# Monotonic Stack

A monotonic stack is a stack that keeps values in increasing or decreasing order.

It is used when you need the next greater, next smaller, previous greater, or previous smaller value.

The stack stores unresolved items.

## Core Idea

Suppose you need the next greater element.

For each value, you want to know:

What is the first bigger value to its right?

Brute force checks every future value.

That is `O(n^2)`.

A monotonic stack solves it in `O(n)`.

## Next Greater Element

Example:

```txt
[2, 1, 5, 3]
```

Next greater values:

```txt
2 -> 5
1 -> 5
5 -> none
3 -> none
```

The stack stores indexes waiting for a greater value.

```js
function nextGreater(nums) {
  const result = Array(nums.length).fill(-1);
  const stack = [];

  for (let i = 0; i < nums.length; i++) {
    while (
      stack.length > 0 &&
      nums[i] > nums[stack[stack.length - 1]]
    ) {
      const index = stack.pop();
      result[index] = nums[i];
    }

    stack.push(i);
  }

  return result;
}
```

When current value is bigger than stack top, it answers that index.

## Why It Is O(n)

There is a `while` loop inside a `for` loop.

It may look like `O(n^2)`.

But each index is pushed once and popped once.

So total stack operations are linear.

Time complexity is `O(n)`.

Space complexity is `O(n)`.

## Increasing vs Decreasing

For next greater element, the stack often keeps values decreasing.

Why?

Because smaller unresolved values wait for a bigger future value.

For next smaller element, the stack often keeps values increasing.

The direction depends on what answer you need.

Ask:

Am I waiting for something bigger or smaller?

## Daily Temperatures

Daily Temperatures is a classic monotonic stack problem.

For each day, find how many days until a warmer temperature.

The stack stores days waiting for a warmer future day.

When a warmer day appears, it resolves previous colder days.

## Largest Rectangle in Histogram

This is a harder monotonic stack problem.

The stack helps find how far each bar can extend before a smaller bar blocks it.

This shows monotonic stacks are not only for next greater value.

They can find boundaries.

## When To Use Monotonic Stack

Think about monotonic stack when the problem asks:

- Next greater.
- Next smaller.
- Previous greater.
- Previous smaller.
- First larger value to the right.
- First smaller value to the left.
- Span or boundary problems.

## Common Mistakes

The first mistake is storing values when indexes are needed.

Indexes are often better because they let you compute distance.

The second mistake is using the wrong comparison.

`>` and `>=` can produce different behavior with duplicates.

The third mistake is not understanding what remains in the stack.

Remaining items usually have no answer.

The fourth mistake is thinking the nested while loop makes it `O(n^2)`.

It does not, because each item is popped once.

## Interview Explanation

A strong explanation sounds like this:

“I use a monotonic stack to store unresolved indexes. When the current value is greater than the value at the stack top, the current value becomes the answer for that previous index. Each index is pushed once and popped once, so the time complexity is O(n).”

## Final Understanding

Monotonic stack is about waiting.

The stack holds values waiting for a future value to resolve them.

When that future value appears, many previous values can be answered at once.

