# Stack

A stack is a data structure where the last item added is the first item removed.

This is called Last In, First Out.

Short form: LIFO.

Think of a stack of plates.

You add a plate on top.

You remove a plate from the top.

## Core Operations

A stack usually supports:

- `push`: add an item to the top.
- `pop`: remove the top item.
- `peek`: read the top item without removing it.
- `isEmpty`: check if the stack has no items.

In JavaScript, an array can act as a stack.

```js
const stack = [];

stack.push(10);
stack.push(20);

console.log(stack.pop()); // 20
console.log(stack.pop()); // 10
```

The last value pushed was `20`, so it is removed first.

## Why Stacks Matter

Stacks are useful when the most recent thing matters first.

This happens in many places:

- Function calls.
- Undo operations.
- Browser back button.
- Parentheses matching.
- DFS traversal.
- Monotonic stack problems.

The stack remembers unfinished work.

## Valid Parentheses

This is the classic stack problem.

Input:

```txt
"({[]})"
```

Every closing bracket must match the most recent opening bracket.

That “most recent” idea is exactly what a stack is for.

```js
function isValid(s) {
  const stack = [];
  const pairs = {
    ")": "(",
    "}": "{",
    "]": "[",
  };

  for (const ch of s) {
    if (ch === "(" || ch === "{" || ch === "[") {
      stack.push(ch);
    } else {
      if (stack.pop() !== pairs[ch]) {
        return false;
      }
    }
  }

  return stack.length === 0;
}
```

Time complexity is `O(n)`.

Space complexity is `O(n)` in the worst case.

## Stack as Memory of Previous Values

Stacks can remember previous values while scanning.

Example: next greater element.

For each number, we want to know the next number to the right that is greater.

A stack can store unresolved values.

When a bigger value appears, it resolves some previous values.

This leads to monotonic stack.

## Stack and Recursion

Recursion uses the call stack.

When a function calls itself, the current call waits.

The waiting calls are stored like stack frames.

This is why recursive DFS can also be written using an explicit stack.

Recursive DFS:

```js
function dfs(node) {
  if (!node) return;

  dfs(node.left);
  dfs(node.right);
}
```

Iterative DFS:

```js
function dfs(root) {
  const stack = [root];

  while (stack.length > 0) {
    const node = stack.pop();
    if (!node) continue;

    stack.push(node.right);
    stack.push(node.left);
  }
}
```

Both use stack behavior.

One uses the system call stack.

The other uses your own array stack.

## Complexity

Push is `O(1)`.

Pop is `O(1)`.

Peek is `O(1)`.

If you process `n` items with a stack, the total time is often `O(n)`.

Space can be `O(n)` if many items are stored.

## Common Mistakes

The first mistake is popping from an empty stack.

Always check whether the stack has something before relying on the popped value.

The second mistake is forgetting to check leftover items at the end.

For valid parentheses, if the stack is not empty at the end, some opening brackets never closed.

The third mistake is not defining what the stack stores.

Does it store values?

Indexes?

Nodes?

Previous states?

The meaning must be clear.

## Interview Explanation

A strong explanation sounds like this:

“I use a stack because the most recent unmatched item must be handled first. Each item is pushed once and popped at most once, so the time complexity is O(n). In the worst case, the stack stores all items, so the space complexity is O(n).”

This explains the reason, not only the code.

## Final Understanding

A stack is simple.

But the idea is powerful.

Whenever the newest unresolved thing should be solved first, think about a stack.
