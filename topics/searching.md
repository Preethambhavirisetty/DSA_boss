# Searching Algorithms

Searching means finding something inside a search space.

That “something” may be a value, an index, a boundary, a minimum possible answer, a maximum possible answer, or the best point of a mathematical function.

Most beginners think searching only means this:

```txt
Find 7 in [4, 2, 9, 7, 1]
```

That is searching, but it is only the first level.

In real DSA, searching is broader.

You may search:

- an unsorted array
- a sorted array
- a nearly sorted array
- an infinite or unknown-sized array
- an answer range
- a mountain-shaped function
- a boundary between false and true
- a graph state
- a rotated search space

The most important question is:

What structure does the search space have?

If there is no structure, you may need linear search.

If the data is sorted, binary search becomes possible.

If the answer space is monotonic, binary search on answer becomes possible.

If the function first increases and then decreases, ternary search may apply.

If the data is uniformly distributed, interpolation search may be useful.

Good searching is not about memorizing algorithms.

Good searching is about seeing the shape of the search space.

## Searching Algorithm Map

| Algorithm | Main Requirement | Usual Time | Best Use |
| --- | --- | --- | --- |
| Linear Search | No structure needed | `O(n)` | Unsorted data, small input |
| Binary Search | Sorted or monotonic space | `O(log n)` | Sorted arrays, boundaries |
| Binary Search on Answer | Monotonic condition | `O(log range * checkCost)` | Minimum feasible answer |
| Ternary Search | Unimodal function | `O(log n)` or fixed iterations | Peak/minimum optimization |
| Exponential Search | Sorted, unknown range | `O(log position)` | Infinite/unknown-sized arrays |
| Interpolation Search | Sorted and uniformly distributed | Average `O(log log n)`, worst `O(n)` | Numeric uniform data |
| Jump Search | Sorted array | `O(sqrt n)` | Simple block search |
| Fibonacci Search | Sorted array | `O(log n)` | Binary-like search using Fibonacci splits |
| Meta Binary Search | Sorted/monotonic, one-sided bit building | `O(log n)` | Lower-bound style index construction |
| Lower Bound / Upper Bound | Sorted array | `O(log n)` | First `>= target`, first `> target` |

## Linear Search

Linear search is the simplest searching algorithm.

It checks items one by one from left to right.

It does not need sorted data.

It does not need any special structure.

That is its biggest strength and its biggest weakness.

It works almost anywhere, but it may be slow for large input.

### When Linear Search Makes Sense

Use linear search when:

- the data is unsorted
- the input is small
- you only search once
- building extra structure is not worth it
- every item must be inspected anyway

If you have only 20 elements, linear search is often perfectly fine.

Principal-level engineering is not about always using the most advanced algorithm.

It is about using the simplest correct algorithm that meets the requirement.

### Code

```js
function linearSearch(nums, target) {
  for (let i = 0; i < nums.length; i++) {
    if (nums[i] === target) {
      return i;
    }
  }

  return -1;
}
```

### Dry Run

Input:

```txt
nums = [8, 4, 2, 10, 6]
target = 10
```

Steps:

```txt
index 0 -> value 8  -> not target
index 1 -> value 4  -> not target
index 2 -> value 2  -> not target
index 3 -> value 10 -> found
```

Return `3`.

### Complexity

Best case: `O(1)` if the target is first.

Worst case: `O(n)` if the target is last or missing.

Average case: `O(n)`.

Space: `O(1)`.

### Common Mistakes

Do not say linear search is always bad.

For small or unsorted data, it can be the right answer.

Do not forget that if you must inspect every item for another reason, linear scanning may be unavoidable.

Do not build a hash set for one tiny search unless it actually helps.

## Binary Search

Binary search is used when the search space is ordered.

Most commonly, it searches a sorted array.

The key idea is:

Check the middle, then remove the half that cannot contain the answer.

Binary search is not about “finding the middle.”

It is about safely deleting half of the search space.

### Basic Code

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

### Why This Works

Suppose:

```txt
nums = [2, 5, 8, 12, 16, 23, 38]
target = 16
```

Middle is `12`.

Since `16 > 12`, every value to the left of `12` is also too small.

So the left half is impossible.

That is why we move `left = mid + 1`.

Every step must have this kind of proof.

### Dry Run

```txt
left = 0, right = 6
mid = 3, nums[mid] = 12
target is bigger, search right half

left = 4, right = 6
mid = 5, nums[mid] = 23
target is smaller, search left half

left = 4, right = 4
mid = 4, nums[mid] = 16
found
```

Return `4`.

### Complexity

Time: `O(log n)`.

Space: `O(1)` for iterative binary search.

Recursive binary search uses `O(log n)` stack space.

### Common Mistakes

The first mistake is using binary search on unsorted data.

The second mistake is not shrinking the search space.

If `left` and `right` do not move, the loop can run forever.

The third mistake is using the wrong loop condition.

The fourth mistake is returning `mid` when the problem actually asks for a boundary.

The fifth mistake is not testing arrays of length `0`, `1`, and `2`.

## Binary Search on Answer

Binary Search on Answer is also called parametric search or search-space binary search.

This is one of the most important interview patterns.

Here, you are not searching for a value inside an array.

You are searching for the smallest or largest answer that satisfies a condition.

### The Core Idea

You need a monotonic condition.

Monotonic means the answers follow a one-direction pattern.

Example:

```txt
answer:  1  2  3  4  5  6  7
works:   F  F  F  T  T  T  T
```

Once the answer works, every larger answer also works.

This is a “first true” problem.

Binary search can find the first `T`.

Another possible shape:

```txt
answer:  1  2  3  4  5  6  7
works:   T  T  T  T  F  F  F
```

This is a “last true” problem.

Binary search can find the last `T`.

### Example: Koko Eating Bananas

Problem idea:

Koko eats bananas at speed `k`.

Find the minimum `k` so she finishes within `h` hours.

If speed `k` works, then any speed greater than `k` also works.

That gives monotonic behavior.

### Code Shape

```js
function minEatingSpeed(piles, h) {
  let left = 1;
  let right = Math.max(...piles);
  let answer = right;

  function canFinish(speed) {
    let hours = 0;

    for (const pile of piles) {
      hours += Math.ceil(pile / speed);
    }

    return hours <= h;
  }

  while (left <= right) {
    const mid = left + Math.floor((right - left) / 2);

    if (canFinish(mid)) {
      answer = mid;
      right = mid - 1;
    } else {
      left = mid + 1;
    }
  }

  return answer;
}
```

### How to Recognize It

Look for phrases like:

- minimum possible
- maximum possible
- smallest value that works
- largest value that works
- can we do this within limit?
- minimize the maximum
- maximize the minimum

These often signal binary search on answer.

### Complexity

If the answer range is from `low` to `high`, binary search takes:

```txt
O(log(high - low))
```

If each check costs `O(n)`, total time is:

```txt
O(n log(high - low))
```

Space is usually `O(1)`.

### Common Mistakes

The first mistake is not proving monotonicity.

If the condition is not monotonic, binary search on answer does not work.

The second mistake is choosing bad boundaries.

The third mistake is mixing up first true and last true.

The fourth mistake is making the check function too slow.

The fifth mistake is returning `left`, `right`, or `answer` without understanding what it represents.

## Ternary Search

Ternary search is used on a unimodal function.

Unimodal means the values go in one direction and then switch once.

For maximum:

```txt
increasing -> peak -> decreasing
```

For minimum:

```txt
decreasing -> valley -> increasing
```

Binary search works on monotonic data.

Ternary search works on peak or valley shaped data.

### Core Idea

Instead of checking one middle point, ternary search checks two middle points.

```txt
left          mid1          mid2          right
```

For maximum search:

If `f(mid1) < f(mid2)`, the peak is to the right of `mid1`.

If `f(mid1) > f(mid2)`, the peak is to the left of `mid2`.

This removes about one-third of the search space each time.

### Code for Discrete Maximum

```js
function ternarySearchMax(left, right, f) {
  while (right - left > 3) {
    const third = Math.floor((right - left) / 3);
    const mid1 = left + third;
    const mid2 = right - third;

    if (f(mid1) < f(mid2)) {
      left = mid1 + 1;
    } else {
      right = mid2 - 1;
    }
  }

  let best = left;

  for (let i = left + 1; i <= right; i++) {
    if (f(i) > f(best)) {
      best = i;
    }
  }

  return best;
}
```

### Continuous Ternary Search

For floating point ranges, run a fixed number of iterations.

```js
function ternarySearchContinuous(left, right, f) {
  for (let iter = 0; iter < 100; iter++) {
    const mid1 = left + (right - left) / 3;
    const mid2 = right - (right - left) / 3;

    if (f(mid1) < f(mid2)) {
      left = mid1;
    } else {
      right = mid2;
    }
  }

  return (left + right) / 2;
}
```

### Complexity

Ternary search is logarithmic in the search range.

For continuous search, complexity is based on fixed iterations.

Space is `O(1)`.

### Common Mistakes

The first mistake is using ternary search on data that is not unimodal.

The second mistake is confusing it with binary search.

The third mistake is not handling discrete boundaries carefully.

The fourth mistake is using ternary search when a derivative, formula, or simpler observation exists.

## Exponential Search

Exponential search is useful when the array is sorted but the target range is unknown.

It is also useful when the array may be conceptually infinite.

The idea is:

First find a range where the target could exist.

Then run binary search inside that range.

### Core Idea

Start with index `1`.

Keep doubling:

```txt
1, 2, 4, 8, 16, 32...
```

Stop when the value at that index is greater than or equal to the target.

Now the target must be between the previous bound and the current bound.

### Code

```js
function exponentialSearch(nums, target) {
  if (nums.length === 0) return -1;
  if (nums[0] === target) return 0;

  let bound = 1;

  while (bound < nums.length && nums[bound] < target) {
    bound *= 2;
  }

  let left = Math.floor(bound / 2);
  let right = Math.min(bound, nums.length - 1);

  while (left <= right) {
    const mid = left + Math.floor((right - left) / 2);

    if (nums[mid] === target) return mid;
    if (nums[mid] < target) left = mid + 1;
    else right = mid - 1;
  }

  return -1;
}
```

### Dry Run

Array:

```txt
[2, 4, 8, 16, 32, 64, 128]
```

Target:

```txt
64
```

Bounds checked:

```txt
index 1 -> 4
index 2 -> 8
index 4 -> 32
index 8 -> out of range
```

So binary search runs from index `4` to `6`.

### Complexity

If the target is at position `p`, exponential search takes `O(log p)`.

This is nice when the target is near the beginning.

Space is `O(1)`.

### Common Mistakes

The first mistake is using it on unsorted data.

The second mistake is not checking array bounds while doubling.

The third mistake is forgetting to binary search after finding the range.

## Interpolation Search

Interpolation search is used on sorted numeric data.

It improves on binary search when values are uniformly distributed.

Binary search always checks the middle index.

Interpolation search estimates where the target should be based on value.

### Core Idea

If values are evenly spread, a target closer to the high value is probably closer to the right side.

Formula:

```txt
pos = low + ((target - nums[low]) * (high - low)) / (nums[high] - nums[low])
```

This is similar to how you search in a dictionary.

If you search for “zebra”, you do not open the middle.

You open near the end.

### Code

```js
function interpolationSearch(nums, target) {
  let low = 0;
  let high = nums.length - 1;

  while (
    low <= high &&
    target >= nums[low] &&
    target <= nums[high]
  ) {
    if (nums[low] === nums[high]) {
      return nums[low] === target ? low : -1;
    }

    const pos =
      low +
      Math.floor(
        ((target - nums[low]) * (high - low)) /
          (nums[high] - nums[low])
      );

    if (nums[pos] === target) return pos;

    if (nums[pos] < target) {
      low = pos + 1;
    } else {
      high = pos - 1;
    }
  }

  return -1;
}
```

### Complexity

Average case on uniformly distributed data: `O(log log n)`.

Worst case: `O(n)`.

Space: `O(1)`.

### When It Is Good

Interpolation search can be good when:

- data is sorted
- data is numeric
- values are uniformly distributed
- random access is available

### When It Is Bad

It performs badly when values are clustered.

Example:

```txt
[1, 2, 3, 4, 1000000]
```

The position estimate can be poor.

Binary search is more reliable in general.

### Common Mistakes

The first mistake is using interpolation search on non-numeric data.

The second mistake is using it on non-uniform data and expecting `O(log log n)`.

The third mistake is not handling `nums[low] === nums[high]`, which can cause division by zero.

## Jump Search

Jump search is used on sorted arrays.

It jumps ahead by fixed block sizes, then does linear search inside a block.

It is simpler than binary search but usually slower.

### Core Idea

Choose a jump size, usually `sqrt(n)`.

Jump by that block size until you pass the target.

Then linearly search inside the previous block.

### Example

Array:

```txt
[1, 3, 5, 7, 9, 11, 13, 15, 17]
```

Target:

```txt
13
```

If block size is `3`, check:

```txt
index 2 -> 5
index 5 -> 11
index 8 -> 17
```

Now target must be between index `6` and `8`.

Do linear search there.

### Code

```js
function jumpSearch(nums, target) {
  const n = nums.length;
  const step = Math.floor(Math.sqrt(n));
  let prev = 0;
  let curr = step;

  while (prev < n && nums[Math.min(curr, n) - 1] < target) {
    prev = curr;
    curr += step;
  }

  for (let i = prev; i < Math.min(curr, n); i++) {
    if (nums[i] === target) return i;
  }

  return -1;
}
```

### Complexity

Time: `O(sqrt n)`.

Space: `O(1)`.

### Common Mistakes

The first mistake is using jump search on unsorted data.

The second mistake is choosing a poor block size.

The third mistake is not clamping the block end to array length.

In interviews, binary search is usually preferred unless jump search is specifically asked.

## Fibonacci Search

Fibonacci search is another sorted-array search algorithm.

It is similar to binary search, but it splits using Fibonacci numbers instead of halves.

It was historically useful in systems where division was expensive or sequential access patterns mattered.

Today, it is less common than binary search, but it is still a known searching algorithm.

### Core Idea

Fibonacci numbers:

```txt
1, 1, 2, 3, 5, 8, 13, 21...
```

Find the smallest Fibonacci number greater than or equal to `n`.

Use Fibonacci offsets to probe positions.

Each comparison reduces the search range using smaller Fibonacci numbers.

### Code

```js
function fibonacciSearch(nums, target) {
  const n = nums.length;

  let fibMm2 = 0;
  let fibMm1 = 1;
  let fibM = fibMm1 + fibMm2;

  while (fibM < n) {
    fibMm2 = fibMm1;
    fibMm1 = fibM;
    fibM = fibMm1 + fibMm2;
  }

  let offset = -1;

  while (fibM > 1) {
    const i = Math.min(offset + fibMm2, n - 1);

    if (nums[i] < target) {
      fibM = fibMm1;
      fibMm1 = fibMm2;
      fibMm2 = fibM - fibMm1;
      offset = i;
    } else if (nums[i] > target) {
      fibM = fibMm2;
      fibMm1 = fibMm1 - fibMm2;
      fibMm2 = fibM - fibMm1;
    } else {
      return i;
    }
  }

  if (fibMm1 && nums[offset + 1] === target) {
    return offset + 1;
  }

  return -1;
}
```

### Complexity

Time: `O(log n)`.

Space: `O(1)`.

### Binary Search vs Fibonacci Search

Binary search usually uses the middle.

Fibonacci search uses Fibonacci offsets.

Binary search is simpler and more common.

Fibonacci search is mostly useful to understand alternative ways of shrinking sorted search space.

### Common Mistakes

The first mistake is not understanding the offset.

The offset tracks the eliminated left part.

The second mistake is bad index bounds.

The third mistake is choosing Fibonacci search when binary search is simpler and accepted.

## Meta Binary Search

Meta Binary Search is a one-sided way to build the answer bit by bit.

It is also called binary lifting style search in some contexts.

Instead of keeping `left` and `right`, you start from an answer and try to jump forward using powers of two.

It is useful for lower-bound style problems and Fenwick tree kth-order searches.

### Core Idea

Suppose you want the largest index `pos` where a condition is still false.

You can build `pos` by trying jumps from large to small.

Example powers:

```txt
16, 8, 4, 2, 1
```

If jumping by `16` stays valid, take it.

Then try `8`.

Then `4`, `2`, `1`.

This constructs the answer one bit at a time.

### Code Shape

Find first index where `nums[index] >= target`.

```js
function lowerBoundMeta(nums, target) {
  let pos = -1;
  let step = 1;

  while (step < nums.length) {
    step <<= 1;
  }

  for (; step > 0; step >>= 1) {
    const next = pos + step;

    if (next < nums.length && nums[next] < target) {
      pos = next;
    }
  }

  return pos + 1;
}
```

At the end, `pos` is the last index where value is less than target.

So `pos + 1` is the first index where value is at least target.

### Complexity

Time: `O(log n)`.

Space: `O(1)`.

### When It Appears

Meta binary search appears in:

- lower bound construction
- binary lifting
- Fenwick tree find kth
- sparse table style jumps
- ancestor queries

It is less common for simple array search, but very useful in advanced structures.

### Common Mistakes

The first mistake is not defining what `pos` means.

In the code above:

`pos` means the last known index with value less than target.

The second mistake is starting with a step that is too small.

The third mistake is using it when normal binary search is clearer.

## Lower Bound and Upper Bound

Lower bound and upper bound are binary search patterns on sorted arrays.

They are extremely important.

Many real problems do not ask:

“Does target exist?”

They ask:

- Where should target be inserted?
- What is the first value greater than or equal to target?
- What is the first value strictly greater than target?
- How many times does target appear?

## Lower Bound

Lower bound returns the first index where:

```txt
nums[index] >= target
```

If all values are smaller, it returns `nums.length`.

### Code

```js
function lowerBound(nums, target) {
  let left = 0;
  let right = nums.length;

  while (left < right) {
    const mid = left + Math.floor((right - left) / 2);

    if (nums[mid] < target) {
      left = mid + 1;
    } else {
      right = mid;
    }
  }

  return left;
}
```

Notice `right = nums.length`, not `nums.length - 1`.

This uses a half-open range:

```txt
[left, right)
```

That style is very clean for boundaries.

## Upper Bound

Upper bound returns the first index where:

```txt
nums[index] > target
```

### Code

```js
function upperBound(nums, target) {
  let left = 0;
  let right = nums.length;

  while (left < right) {
    const mid = left + Math.floor((right - left) / 2);

    if (nums[mid] <= target) {
      left = mid + 1;
    } else {
      right = mid;
    }
  }

  return left;
}
```

## Counting Occurrences

If the array is sorted, count target with:

```txt
upperBound(target) - lowerBound(target)
```

Example:

```txt
nums = [1, 2, 2, 2, 3, 5]
target = 2
```

Lower bound of `2` is index `1`.

Upper bound of `2` is index `4`.

Count is `4 - 1 = 3`.

## Insert Position

Lower bound also gives insertion position.

If target is not present, lower bound tells where it should go to keep the array sorted.

Example:

```txt
[1, 3, 5, 7]
target = 4
```

Lower bound is index `2`.

Insert `4` before `5`.

## Complexity

Lower bound and upper bound both take `O(log n)`.

Space is `O(1)`.

## Common Mistakes

The first mistake is confusing lower bound and upper bound.

Lower bound is first `>= target`.

Upper bound is first `> target`.

The second mistake is using inclusive binary search code when boundary code is needed.

The third mistake is not handling target smaller than all values or larger than all values.

The fourth mistake is returning `mid` too early.

For boundary search, even if `nums[mid] === target`, you may need to keep searching left or right.

## How To Choose The Right Search Algorithm

Use this decision guide:

If the data is unsorted and small, use linear search.

If the data is unsorted and you need many lookups, consider hashing.

If the data is sorted, use binary search.

If the sorted data has duplicates and you need boundaries, use lower bound or upper bound.

If the answer is not in an array but a monotonic condition exists, use binary search on answer.

If the array is sorted but the size or range is unknown, use exponential search.

If numeric sorted data is uniformly distributed, interpolation search may help.

If the function is unimodal, use ternary search.

If you need block-based simple search, jump search exists, but binary search is usually better.

If you are doing binary lifting or advanced index construction, meta binary search may fit.

## Principal Engineer View

At a principal level, choosing a search algorithm is about constraints.

Ask these questions:

- Is the data sorted?
- Is it static or does it change?
- How many searches will happen?
- Is memory cheap or expensive?
- Is the search space finite?
- Can I prove monotonicity?
- Do I need exact match or boundary?
- Is worst-case performance important?
- Is the algorithm easy for the team to maintain?

For example, interpolation search may look faster on paper for uniform data.

But binary search is more predictable, simpler, and safer.

In production systems, predictable worst-case behavior often matters more than theoretical average-case speed.

In interviews, clarity matters too.

Use the simplest algorithm that satisfies the constraints and explain the tradeoff.

## Common Interview Language

For linear search:

“The data is not sorted, so I may need to inspect every element. Time is O(n), space is O(1).”

For binary search:

“The data is sorted, so I can remove half the search space each step. Time is O(log n).”

For binary search on answer:

“The condition is monotonic. If a value works, all larger values also work. So I search for the first working value.”

For lower bound:

“I need the first index where the value is at least target, so I continue searching left even when I find a valid candidate.”

For ternary search:

“The function is unimodal, so comparing two middle points tells me which third cannot contain the optimum.”

## Final Understanding

Searching is not one algorithm.

Searching is a family of strategies.

The best strategy depends on the shape of the search space.

If there is no structure, scan.

If there is order, halve.

If there is monotonic feasibility, binary search the answer.

If there is a peak or valley, use ternary search.

If there are repeated lookups, consider building a structure.

The more clearly you understand the search space, the more naturally the correct algorithm appears.
