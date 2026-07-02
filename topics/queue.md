# Queue

A queue is a data structure where the first item added is the first item removed.

This is called First In, First Out.

Short form: FIFO.

Think of a line at a store.

The first person in line is served first.

## Core Operations

A queue usually supports:

- `enqueue`: add an item to the back.
- `dequeue`: remove an item from the front.
- `peek`: read the front item.
- `isEmpty`: check if the queue is empty.

## Basic Queue

```js
const queue = [];

queue.push(10);
queue.push(20);

console.log(queue.shift()); // 10
console.log(queue.shift()); // 20
```

This works for small examples.

But for large queues in JavaScript, repeated `shift()` can be slow because it may move elements.

A better pattern uses a head index.

```js
const queue = [];
let head = 0;

queue.push(10);
queue.push(20);

console.log(queue[head++]); // 10
console.log(queue[head++]); // 20
```

Now removing from the front is efficient.

## Why Queues Matter

Queues are important when order of arrival matters.

They are used in:

- BFS.
- Task scheduling.
- Print jobs.
- Message processing.
- Level order traversal.
- Shortest path in unweighted graphs.

The queue stores work that should be handled later, in the same order it arrived.

## Queue in BFS

BFS means Breadth-First Search.

It explores level by level.

That level order happens because of the queue.

Example:

```js
function bfs(start, graph) {
  const queue = [start];
  let head = 0;
  const visited = new Set([start]);

  while (head < queue.length) {
    const node = queue[head++];

    for (const next of graph[node]) {
      if (!visited.has(next)) {
        visited.add(next);
        queue.push(next);
      }
    }
  }
}
```

The oldest discovered node is processed first.

That is why BFS finds the shortest path in an unweighted graph.

## Level Order Traversal

In trees, a queue helps process one level at a time.

```js
function levelOrder(root) {
  if (!root) return [];

  const result = [];
  const queue = [root];
  let head = 0;

  while (head < queue.length) {
    const levelSize = queue.length - head;
    const level = [];

    for (let i = 0; i < levelSize; i++) {
      const node = queue[head++];
      level.push(node.val);

      if (node.left) queue.push(node.left);
      if (node.right) queue.push(node.right);
    }

    result.push(level);
  }

  return result;
}
```

The queue keeps children for the next level.

## Complexity

Enqueue is `O(1)`.

Dequeue is `O(1)` if implemented correctly.

Processing `n` items is `O(n)`.

Space is `O(n)` in the worst case because the queue may hold many items.

In tree BFS, the queue may hold a whole level.

In graph BFS, the queue may hold many discovered nodes.

## Common Mistakes

The first mistake is using `shift()` for large JavaScript queues.

Use a head index instead.

The second mistake is marking visited too late.

In graph BFS, mark a node visited when you add it to the queue, not only when you remove it.

Otherwise the same node may be added many times.

The third mistake is mixing queue and stack behavior.

Queue removes from the front.

Stack removes from the back.

That difference changes traversal order.

## Interview Explanation

A strong explanation sounds like this:

“I use a queue because I want to process nodes in the order they are discovered. This gives level-by-level traversal. Each node is added and removed once, so the time complexity is O(V + E) for a graph. The space complexity is O(V) for the queue and visited set.”

This shows both the data structure and the algorithm reason.

## Final Understanding

A queue is about fairness and order.

The first item that arrives is handled first.

Whenever a problem needs level order, arrival order, or unweighted shortest path, think about a queue.

