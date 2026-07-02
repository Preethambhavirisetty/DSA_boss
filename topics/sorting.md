# Sorting

Sorting means arranging values in a specific order.

Usually the order is ascending or descending.

Sorting is not only about making data look neat.

Sorting creates structure, and structure makes many problems easier.

## Why Sorting Matters

When data is unsorted, you often have very little information.

To find a value, you may need to scan everything.

To find pairs, you may need to check many combinations.

After sorting, relationships become clearer.

Small values are on one side.

Large values are on the other side.

Duplicates come together.

This allows binary search, two pointers, greedy algorithms, and simpler scans.

## Basic Sorting Example

```js
const nums = [5, 2, 8, 1];
nums.sort((a, b) => a - b);
console.log(nums); // [1, 2, 5, 8]
```

In JavaScript, the compare function is important for numbers.

Without it, values may be sorted like strings.

## Bubble Sort

Bubble sort repeatedly compares neighboring values and swaps them if they are in the wrong order.

```js
function bubbleSort(nums) {
  for (let pass = 0; pass < nums.length; pass++) {
    for (let i = 0; i < nums.length - 1; i++) {
      if (nums[i] > nums[i + 1]) {
        [nums[i], nums[i + 1]] = [nums[i + 1], nums[i]];
      }
    }
  }

  return nums;
}
```

Large values slowly move to the right.

Bubble sort is easy to understand but slow.

Time complexity is `O(n^2)`.

Space complexity is `O(1)`.

## Selection Sort

Selection sort repeatedly finds the smallest remaining value and puts it in the correct position.

The array has two parts:

- A sorted left part.
- An unsorted right part.

Each pass selects the smallest value from the unsorted part.

Time complexity is `O(n^2)`.

Space complexity is `O(1)`.

Selection sort makes fewer swaps than bubble sort, but it still scans many pairs.

## Insertion Sort

Insertion sort builds a sorted section one value at a time.

It is similar to sorting cards in your hand.

You take one new card and place it where it belongs in the sorted part.

Insertion sort is `O(n^2)` in the worst case.

But it can be fast when the array is already almost sorted.

That is why it is still useful in some practical sorting implementations for small sections.

## Merge Sort

Merge sort is a divide and conquer algorithm.

It splits the array into halves, sorts each half, and then merges the sorted halves.

The main idea is:

If two small arrays are already sorted, merging them is easy.

```txt
[1, 4, 7]
[2, 3, 8]

merged -> [1, 2, 3, 4, 7, 8]
```

Merge sort has `O(n log n)` time complexity.

It usually uses `O(n)` extra space.

It is stable, meaning equal values keep their relative order.

## Quick Sort

Quick sort chooses a pivot and partitions the array around it.

Values smaller than the pivot go to one side.

Values larger than the pivot go to the other side.

Then quick sort recursively sorts both sides.

Average time complexity is `O(n log n)`.

Worst-case time complexity is `O(n^2)` if pivots are bad.

Space complexity is usually `O(log n)` for recursion in average cases.

Quick sort is often fast in practice because it has good cache behavior and low overhead.

## Heap Sort

Heap sort uses a heap data structure.

A max heap lets us repeatedly remove the largest value.

Heap sort has `O(n log n)` time complexity.

It can be done with `O(1)` extra space if implemented in place.

It is not usually stable.

## Stability

A stable sort keeps equal items in their original relative order.

Example:

```txt
(Alice, score 90)
(Bob, score 90)
```

If Alice came before Bob before sorting, a stable sort keeps Alice before Bob after sorting by score.

Stability matters when you sort objects by multiple fields.

## Sorting as a Problem-Solving Tool

Sorting is often not the final answer.

It is a setup step.

Example: checking if two arrays have common elements can be done with hashing.

But if both arrays are sorted, two pointers can solve it with less extra memory.

Example: interval problems often start by sorting intervals by start time.

Then overlapping intervals become neighbors.

## When Sorting Is a Good Idea

Sorting is useful when order helps remove uncertainty.

Use sorting when:

- You need closest values.
- You need to handle duplicates.
- You need pairs or triplets.
- You need intervals in order.
- You want to apply two pointers.
- You want to make greedy choices.

## When Sorting Is Dangerous

Sorting may be wrong if original order matters.

For example, if a problem asks about subarrays, sorting usually destroys the answer because subarray order must stay original.

Sorting also costs at least `O(n log n)` for comparison-based sorting.

If an `O(n)` solution exists with hashing or counting, sorting may be slower.

## Common Mistakes

The first mistake is sorting when order must be preserved.

The second mistake is forgetting the time cost of sorting.

The third mistake is using the wrong numeric comparator.

The fourth mistake is assuming sorted output means the algorithm is stable.

The fifth mistake is not checking if duplicates affect the logic.

## Interview Explanation

A strong explanation sounds like this:

“I sort the array first so related values become neighbors. Sorting costs O(n log n). After that, I scan once with two pointers, which is O(n). The total time is O(n log n), dominated by sorting. The extra space depends on the sorting implementation.”

This explains both why sorting helps and how it affects complexity.

## Final Understanding

Sorting creates order.

Order gives you power.

But sorting is not free.

Use it when the structure it creates is worth the cost.

