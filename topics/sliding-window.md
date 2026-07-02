# Sliding Window

Sliding window is a pattern for arrays and strings.

It is used when the problem asks about a continuous section.

That section is called a window.

Examples:

- Subarray.
- Substring.
- Consecutive elements.

## The Core Idea

Instead of recalculating every window from scratch, keep a current window and update it as it moves.

The window has two boundaries:

- `left`
- `right`

The right side usually expands the window.

The left side usually shrinks the window.

## Fixed Size Window

Use fixed size window when the length is given.

Example:

Find the maximum sum of any subarray of size `k`.

```js
function maxSumK(nums, k) {
  let sum = 0;
  let best = -Infinity;

  for (let right = 0; right < nums.length; right++) {
    sum += nums[right];

    if (right >= k) {
      sum -= nums[right - k];
    }

    if (right >= k - 1) {
      best = Math.max(best, sum);
    }
  }

  return best;
}
```

The window adds one new value and removes one old value.

Time is `O(n)`.

Space is `O(1)`.

## Variable Size Window

Use variable size window when the window grows and shrinks based on a rule.

Example:

Longest substring without repeating characters.

```js
function lengthOfLongestSubstring(s) {
  const seen = new Set();
  let left = 0;
  let best = 0;

  for (let right = 0; right < s.length; right++) {
    while (seen.has(s[right])) {
      seen.delete(s[left]);
      left++;
    }

    seen.add(s[right]);
    best = Math.max(best, right - left + 1);
  }

  return best;
}
```

The window must always be valid.

If it becomes invalid, move `left` until it is valid again.

## Window State

The window needs state.

The state is the information you maintain about the current window.

Examples:

- Sum.
- Character count.
- Number of distinct values.
- Maximum frequency.
- Product.

The state must update when the window changes.

When `right` moves, add the new item.

When `left` moves, remove the old item.

## Why It Is O(n)

Sliding window can have a `while` loop inside a `for` loop.

That looks like `O(n^2)`, but usually it is `O(n)`.

Why?

Because `left` only moves forward.

`right` only moves forward.

Each element enters the window once and leaves the window once.

So total movement is linear.

## When To Use Sliding Window

Think about sliding window when the problem says:

- Longest substring.
- Shortest substring.
- Maximum subarray of size k.
- Minimum window.
- Consecutive elements.
- Contiguous segment.

The key word is continuous.

If the problem is about subsequences, sliding window may not apply because subsequences can skip elements.

## Common Mistakes

The first mistake is using sliding window for non-continuous problems.

The second mistake is forgetting to remove the left value from state.

The third mistake is updating the answer at the wrong time.

Sometimes update after expanding.

Sometimes update after shrinking.

It depends on when the window is valid.

The fourth mistake is not defining what makes a window valid.

## Interview Explanation

A strong explanation sounds like this:

“I use sliding window because the problem asks for a continuous subarray or substring. The right pointer expands the window. If the window becomes invalid, the left pointer shrinks it. Each pointer moves at most n times, so the time complexity is O(n).”

This explanation is simple and precise.

## Final Understanding

Sliding window is about maintaining a live section of data.

Do not rebuild the section again and again.

Move the edges and update the state.

