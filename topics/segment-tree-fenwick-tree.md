# Segment Tree and Fenwick Tree

Segment trees and Fenwick trees solve range query problems.

They are useful when the array changes and you still need fast range answers.

Examples:

- Sum from index `l` to `r`.
- Minimum value in a range.
- Maximum value in a range.
- Updating one value and asking queries again.

## Why Prefix Sum Is Not Enough

Prefix sum is great when the array does not change.

If you build prefix sums, range sum is `O(1)`.

But updating one value is expensive because many prefix values must change.

So prefix sum has:

- Query: `O(1)`
- Update: `O(n)`

Segment tree and Fenwick tree improve this.

They usually support:

- Query: `O(log n)`
- Update: `O(log n)`

## Segment Tree Core Idea

A segment tree stores information about ranges.

The root stores information about the whole array.

Each child stores half of that range.

This continues until each leaf stores one array value.

Example array:

```txt
[2, 4, 1, 7]
```

Root stores sum of `[0, 3]`.

Left child stores sum of `[0, 1]`.

Right child stores sum of `[2, 3]`.

Leaves store individual values.

## Query in Segment Tree

To answer a range query, the tree combines only the segments that fully fit inside the query.

If a segment is completely outside, ignore it.

If a segment is completely inside, use its stored value.

If a segment partially overlaps, go deeper.

This gives `O(log n)` for many common range queries.

## Update in Segment Tree

To update one array value:

1. Go to the leaf for that index.
2. Change the leaf.
3. Recompute parent values on the way back up.

Only one path from root to leaf changes.

That path has height `O(log n)`.

So update is `O(log n)`.

## Segment Tree Code Shape

```js
class SegmentTree {
  constructor(nums) {
    this.n = nums.length;
    this.tree = Array(4 * this.n).fill(0);
    this.build(nums, 1, 0, this.n - 1);
  }

  build(nums, node, left, right) {
    if (left === right) {
      this.tree[node] = nums[left];
      return;
    }

    const mid = Math.floor((left + right) / 2);
    this.build(nums, node * 2, left, mid);
    this.build(nums, node * 2 + 1, mid + 1, right);
    this.tree[node] = this.tree[node * 2] + this.tree[node * 2 + 1];
  }
}
```

This example stores sums.

The same idea can store min or max if the combine operation changes.

## Fenwick Tree Core Idea

A Fenwick tree is also called a Binary Indexed Tree.

It is mainly used for prefix sums with updates.

It is more compact than a segment tree.

It uses binary tricks to decide which ranges each index controls.

Fenwick tree is harder to understand at first, but the code is shorter.

## Fenwick Tree Operations

Fenwick tree supports:

- Add value at index: `O(log n)`
- Prefix sum query: `O(log n)`
- Range sum query: `prefix(r) - prefix(l - 1)`

Code shape:

```js
class FenwickTree {
  constructor(n) {
    this.tree = Array(n + 1).fill(0);
  }

  add(index, delta) {
    index++;

    while (index < this.tree.length) {
      this.tree[index] += delta;
      index += index & -index;
    }
  }

  prefixSum(index) {
    index++;
    let sum = 0;

    while (index > 0) {
      sum += this.tree[index];
      index -= index & -index;
    }

    return sum;
  }
}
```

`index & -index` finds the last set bit.

That tells the tree how far to jump.

## When To Use Which

Use Fenwick tree when:

- You need prefix sums.
- You need point updates.
- The operation is simple and reversible.
- You want shorter code.

Use segment tree when:

- You need min, max, sum, gcd, or custom combine logic.
- You need more flexible range queries.
- You need range updates with lazy propagation.

Segment tree is more general.

Fenwick tree is lighter.

## Complexity

| Structure | Build | Query | Update | Space |
| --- | --- | --- | --- | --- |
| Prefix Sum | `O(n)` | `O(1)` | `O(n)` | `O(n)` |
| Segment Tree | `O(n)` | `O(log n)` | `O(log n)` | `O(n)` |
| Fenwick Tree | `O(n log n)` or `O(n)` | `O(log n)` | `O(log n)` | `O(n)` |

## Common Mistakes

The first mistake is using segment tree when prefix sum is enough.

If there are no updates, prefix sum is simpler.

The second mistake is mixing 0-based and 1-based indexes in Fenwick tree.

The third mistake is using Fenwick tree for operations that are not suitable.

The fourth mistake is forgetting to recompute parent nodes after update in segment tree.

## Interview Explanation

A strong explanation sounds like this:

“Prefix sum gives fast range queries but slow updates. Since this problem has both updates and queries, I use a segment tree or Fenwick tree. Both give O(log n) query and update. I choose segment tree for general range operations and Fenwick tree for prefix-sum style problems.”

## Final Understanding

Segment trees and Fenwick trees are advanced tools for dynamic arrays.

Use them when values change and range answers still need to be fast.

