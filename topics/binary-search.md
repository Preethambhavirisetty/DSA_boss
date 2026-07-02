# Binary Search

Binary search is a fast way to search when the data has order.

Most people first learn it on sorted arrays.

But the deeper idea is bigger than arrays.

Binary search works when you can safely remove half of the search space.

## The Core Idea

Suppose you have a sorted array:

```txt
[1, 3, 5, 7, 9, 11, 13]
```

You want to find `9`.

Instead of checking every value, binary search checks the middle.

Middle is `7`.

Since `9` is bigger than `7`, everything on the left side is too small.

So the left half can be ignored.

That is the power of binary search.

Each step removes many impossible answers.

## Basic Code

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

Time complexity is `O(log n)`.

Space complexity is `O(1)`.

## Why It Is O(log n)

Binary search cuts the search space in half each time.

If there are 16 elements:

```txt
16 -> 8 -> 4 -> 2 -> 1
```

That takes about 4 steps.

If there are 1,000,000 elements, it takes about 20 steps.

This is why `O(log n)` is very fast.

## The Real Skill

The real skill is not memorizing the code.

The real skill is proving which side can be removed.

Every binary search step should answer:

Why is it safe to ignore this half?

If you cannot answer that, binary search is probably being used incorrectly.

## Boundaries

Binary search is full of boundary bugs.

You must define what `left` and `right` mean.

In the classic version:

- `left` is the first possible index.
- `right` is the last possible index.
- The search space is inclusive: `[left, right]`.

That is why the loop condition is:

```js
while (left <= right)
```

If `left` becomes greater than `right`, the search space is empty.

## First True Pattern

Binary search can find the first position where a condition becomes true.

Example pattern:

```txt
false false false true true true
```

We want the first `true`.

```js
function firstTrue(n, isGood) {
  let left = 0;
  let right = n - 1;
  let answer = -1;

  while (left <= right) {
    const mid = left + Math.floor((right - left) / 2);

    if (isGood(mid)) {
      answer = mid;
      right = mid - 1;
    } else {
      left = mid + 1;
    }
  }

  return answer;
}
```

This version is used in many advanced problems.

## Search on Answer

Sometimes you do not binary search an array.

You binary search the answer.

Example:

Find the minimum speed needed to finish tasks within `h` hours.

If speed `x` works, then speed `x + 1` also works.

That creates a monotonic condition.

```txt
speed: 1 2 3 4 5 6 7
works: F F F T T T T
```

Now binary search can find the first working speed.

This is called binary search on answer.

## Common Problems

Binary search appears in:

- Search in sorted array.
- First bad version.
- Search insert position.
- Search in rotated sorted array.
- Find minimum in rotated sorted array.
- Koko eating bananas.
- Capacity to ship packages.
- Median of two sorted arrays.

## Common Mistakes

The first mistake is using binary search when there is no order.

The second mistake is not updating boundaries correctly.

If `left` and `right` do not move, the loop may run forever.

The third mistake is returning `mid` when the problem asks for a boundary.

The fourth mistake is not testing two-element cases.

Many binary search bugs appear with arrays of length 1 or 2.

## Interview Explanation

A strong explanation sounds like this:

“The search space is ordered. At each step I check the middle and remove the half that cannot contain the answer. This halves the search space each time, so the time complexity is O(log n). I only use pointers, so space is O(1).”

If using search on answer, say:

“The condition is monotonic. Once an answer works, all larger answers also work. So I can binary search for the first working answer.”

## Final Understanding

Binary search is about eliminating impossible answers.

Sorted arrays are only the first version.

The deeper idea is ordered decision-making.

