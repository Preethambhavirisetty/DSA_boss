# Two Pointers

Two pointers is a pattern where you use two indexes or references to move through data.

The data is usually an array, string, or linked list.

The goal is to avoid unnecessary repeated work.

## Core Idea

Instead of using nested loops, use two positions that move with purpose.

Common forms:

- Left and right pointers.
- Fast and slow pointers.
- Read and write pointers.

Each pointer must have a clear meaning.

## Opposite Ends

This is common with sorted arrays.

Example: find two numbers that add to target.

```js
function twoSumSorted(nums, target) {
  let left = 0;
  let right = nums.length - 1;

  while (left < right) {
    const sum = nums[left] + nums[right];

    if (sum === target) {
      return [left, right];
    }

    if (sum < target) {
      left++;
    } else {
      right--;
    }
  }

  return [];
}
```

Why does this work?

Because the array is sorted.

If the sum is too small, moving `right` left would only make it smaller.

So we must move `left` right to increase the sum.

This safe movement is the key.

## Palindrome Check

Two pointers are natural for palindrome checks.

```js
function isPalindrome(s) {
  let left = 0;
  let right = s.length - 1;

  while (left < right) {
    if (s[left] !== s[right]) return false;

    left++;
    right--;
  }

  return true;
}
```

Compare outside characters and move inward.

Time is `O(n)`.

Space is `O(1)`.

## Fast and Slow Pointers

Fast and slow pointers are common in linked lists.

Fast moves two steps.

Slow moves one step.

This can find the middle.

```js
function middleNode(head) {
  let slow = head;
  let fast = head;

  while (fast && fast.next) {
    slow = slow.next;
    fast = fast.next.next;
  }

  return slow;
}
```

When fast reaches the end, slow is near the middle.

## Cycle Detection

Fast and slow pointers can detect cycles.

If a linked list has a cycle, fast will eventually meet slow.

If there is no cycle, fast reaches null.

This is Floyd’s cycle detection algorithm.

## Read and Write Pointers

Read and write pointers are used when modifying arrays in place.

Example: remove duplicates from sorted array.

```js
function removeDuplicates(nums) {
  let write = 0;

  for (let read = 0; read < nums.length; read++) {
    if (read === 0 || nums[read] !== nums[read - 1]) {
      nums[write] = nums[read];
      write++;
    }
  }

  return write;
}
```

`read` scans every value.

`write` marks where the next kept value should go.

## When To Use Two Pointers

Think about two pointers when:

- The input is sorted.
- You compare both ends.
- You need pairs.
- You need to reverse something.
- You need to remove duplicates in place.
- You need fast and slow movement.

## Complexity

Most two-pointer solutions are `O(n)` time.

They often use `O(1)` extra space.

That is why the pattern is so valuable.

## Common Mistakes

The first mistake is moving the wrong pointer.

Every move needs a reason.

The second mistake is using two pointers without sorted order when sorted order is required.

The third mistake is off-by-one loop conditions.

The fourth mistake is not explaining why a skipped option is impossible.

## Interview Explanation

A strong explanation sounds like this:

“I use two pointers because the array is sorted. At each step, the current sum tells me which pointer can move safely. If the sum is too small, I move left to increase it. If the sum is too large, I move right to decrease it. Each pointer moves inward at most n times, so the time is O(n).”

## Final Understanding

Two pointers are about controlled movement.

Do not move randomly.

Each pointer movement should remove impossible choices or preserve a clear invariant.

