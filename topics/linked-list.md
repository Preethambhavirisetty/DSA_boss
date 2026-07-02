# Linked List

A linked list is a chain of nodes.

Each node stores a value and a reference to the next node.

Unlike arrays, linked lists do not store values in one continuous block where you can jump by index.

To move through a linked list, you follow links one by one.

## Basic Shape

A singly linked list node looks like this:

```js
class ListNode {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}
```

A list like `1 -> 2 -> 3` means:

```txt
node1.value = 1
node1.next = node2

node2.value = 2
node2.next = node3

node3.value = 3
node3.next = null
```

The first node is called the head.

## Arrays vs Linked Lists

Arrays give fast index access.

Linked lists do not.

If you want the 10th item in an array, you can jump there.

If you want the 10th item in a linked list, you must start at the head and move 10 times.

So linked list access by position is `O(n)`.

But linked lists can insert or remove nodes efficiently if you already have the right pointer.

The challenge is getting to that pointer safely.

## Traversal

Traversal means walking through the list.

```js
function printList(head) {
  let current = head;

  while (current !== null) {
    console.log(current.value);
    current = current.next;
  }
}
```

This is `O(n)` time.

Extra space is `O(1)`.

The pointer `current` moves node by node.

## Reversing a Linked List

Reversal is the classic linked list problem.

Original:

```txt
1 -> 2 -> 3 -> null
```

Reversed:

```txt
3 -> 2 -> 1 -> null
```

Code:

```js
function reverseList(head) {
  let prev = null;
  let curr = head;

  while (curr !== null) {
    const next = curr.next;
    curr.next = prev;
    prev = curr;
    curr = next;
  }

  return prev;
}
```

The most important line is:

```js
const next = curr.next;
```

You save the next node before changing the link.

If you do not save it, you lose the rest of the list.

## Pointer Meaning

In reversal:

- `prev` means the already reversed part.
- `curr` means the node currently being processed.
- `next` means the original next node that must not be lost.

Linked list code becomes much easier when every pointer has a clear meaning.

## Dummy Node

A dummy node is a fake node before the real head.

It helps when the head itself may change.

Example:

```js
const dummy = new ListNode(0);
dummy.next = head;
```

Dummy nodes are useful for deletion, merging, and building new lists.

They reduce special cases.

Without a dummy node, deleting the head can require separate logic.

With a dummy node, head deletion becomes normal deletion.

## Fast and Slow Pointers

Fast and slow pointers are common in linked lists.

The slow pointer moves one step.

The fast pointer moves two steps.

This can find the middle of a list.

It can also detect cycles.

If a cycle exists, the fast pointer will eventually meet the slow pointer.

This is Floyd’s cycle detection algorithm.

## Common Operations

| Operation | Complexity |
| --- | --- |
| Search by value | `O(n)` |
| Access by index | `O(n)` |
| Insert after known node | `O(1)` |
| Delete after known node | `O(1)` |
| Reverse list | `O(n)` |

## Common Mistakes

The first mistake is losing the next pointer.

Always save `next` before changing `curr.next`.

The second mistake is not handling an empty list.

If `head` is `null`, many operations should return safely.

The third mistake is not handling one-node lists.

The fourth mistake is accidentally creating a cycle.

The fifth mistake is changing `head` without returning the new head.

## Interview Explanation

A strong explanation for reversal sounds like this:

“I will walk through the list once. For each node, I save its next pointer, reverse its link to point to the previous node, then move both pointers forward. At the end, prev points to the new head. The time complexity is O(n), and the extra space is O(1).”

This explanation is clear because it names every pointer and its job.

## Final Understanding

Linked lists are about references.

The value matters less than the arrows between nodes.

If you draw the arrows and move one pointer at a time, linked list problems become much less confusing.

