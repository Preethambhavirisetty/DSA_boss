# Heap

A heap is a data structure that quickly gives access to the smallest or largest value.

A min heap keeps the smallest value at the top.

A max heap keeps the largest value at the top.

Heaps are commonly used to build priority queues.

## Heap Is Not Fully Sorted

This is important.

A heap does not keep every value sorted.

It only guarantees that the top value is the min or max.

For a min heap:

Every parent is less than or equal to its children.

That is enough to get the minimum quickly.

## Array Representation

Heaps are often stored in arrays.

For index `i`:

- Left child is `2 * i + 1`.
- Right child is `2 * i + 2`.
- Parent is `Math.floor((i - 1) / 2)`.

This avoids storing explicit tree nodes.

Example:

```txt
        2
      /   \
     5     8
    / \
   10  12
```

Array:

```txt
[2, 5, 8, 10, 12]
```

## Insert

To insert into a min heap:

1. Add the value at the end.
2. Compare it with its parent.
3. If it is smaller than the parent, swap.
4. Keep moving up until the heap rule is fixed.

This is called bubble up or sift up.

Time complexity is `O(log n)` because the value may move up the height of the heap.

## Remove Top

To remove the minimum from a min heap:

1. Save the root value.
2. Move the last value to the root.
3. Remove the last slot.
4. Push the new root down until the heap rule is fixed.

This is called bubble down or sift down.

Time complexity is `O(log n)`.

## Peek

Peek means read the top without removing it.

For a min heap, this gives the smallest value.

For a max heap, this gives the largest value.

Peek is `O(1)`.

## Priority Queue

A priority queue removes items by priority, not by arrival order.

If smaller number means higher priority, use a min heap.

If larger number means higher priority, use a max heap.

Priority queues appear in:

- Dijkstra’s algorithm.
- Top K elements.
- Merge K sorted lists.
- Task scheduling.
- Median from data stream.

## Top K Pattern

If you need the `k` largest values, a min heap of size `k` is useful.

The heap stores the best `k` values seen so far.

When a new value is larger than the heap minimum, remove the minimum and add the new value.

This keeps only the top `k`.

Time complexity is `O(n log k)`.

This is better than sorting all values when `k` is small.

## Dijkstra Connection

Dijkstra uses a min heap to always process the node with the smallest known distance.

Without a heap, repeatedly finding the smallest distance can be slow.

The heap makes that selection efficient.

## Complexity

| Operation | Complexity |
| --- | --- |
| Peek | `O(1)` |
| Insert | `O(log n)` |
| Remove top | `O(log n)` |
| Build heap from array | `O(n)` |

## Common Mistakes

The first mistake is thinking a heap is sorted.

It is not.

Only the top is guaranteed.

The second mistake is using a heap when full sorted order is needed.

The third mistake is forgetting whether you need a min heap or max heap.

The fourth mistake is not limiting heap size in Top K problems.

If you keep all values, you may lose the benefit.

## Interview Explanation

A strong explanation sounds like this:

“I use a heap because I repeatedly need the smallest or largest item. Peek is O(1), and insert/remove are O(log n). For Top K, I keep a heap of size k, so each operation costs O(log k), giving total O(n log k).”

This explains why the heap is the right tool.

## Final Understanding

A heap is for repeated priority access.

If you keep asking “what is the smallest?” or “what is the largest?”, think about a heap.

