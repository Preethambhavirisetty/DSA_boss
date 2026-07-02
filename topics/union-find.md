# Union Find

Union Find is a data structure for managing groups.

It is also called Disjoint Set Union, or DSU.

It answers questions like:

Are these two items in the same group?

Can I merge these two groups?

## Core Operations

Union Find has two main operations:

- `find(x)`: return the representative of x’s group.
- `union(a, b)`: merge the groups containing a and b.

If two items have the same representative, they are connected.

## Basic Idea

Each item starts in its own group.

```txt
1   2   3   4
```

After `union(1, 2)`:

```txt
1-2   3   4
```

After `union(3, 4)`:

```txt
1-2   3-4
```

After `union(2, 3)`:

```txt
1-2-3-4
```

Now all four items are in the same group.

## Parent Array

Union Find usually uses a parent array.

At first, every node is its own parent.

```js
const parent = [0, 1, 2, 3, 4];
```

If `parent[x] === x`, then `x` is a group representative.

## Find

Find follows parent links until it reaches the representative.

```js
function find(x) {
  while (parent[x] !== x) {
    x = parent[x];
  }

  return x;
}
```

This works, but it can become slow if the tree becomes tall.

## Path Compression

Path compression makes find faster.

When finding the representative, point nodes directly closer to the root.

```js
function find(x) {
  if (parent[x] !== x) {
    parent[x] = find(parent[x]);
  }

  return parent[x];
}
```

This flattens the structure over time.

## Union by Size

Union by size attaches the smaller group under the larger group.

This keeps trees shallow.

```js
class UnionFind {
  constructor(n) {
    this.parent = Array.from({ length: n }, (_, i) => i);
    this.size = Array(n).fill(1);
  }

  find(x) {
    if (this.parent[x] !== x) {
      this.parent[x] = this.find(this.parent[x]);
    }

    return this.parent[x];
  }

  union(a, b) {
    let rootA = this.find(a);
    let rootB = this.find(b);

    if (rootA === rootB) return false;

    if (this.size[rootA] < this.size[rootB]) {
      [rootA, rootB] = [rootB, rootA];
    }

    this.parent[rootB] = rootA;
    this.size[rootA] += this.size[rootB];
    return true;
  }
}
```

The `false` return can mean the two nodes were already connected.

That is useful for cycle detection.

## When To Use Union Find

Use Union Find when the problem is about connected groups.

Examples:

- Number of connected components.
- Detect cycle in undirected graph.
- Redundant connection.
- Accounts merge.
- Kruskal’s minimum spanning tree.
- Dynamic connectivity.

## Union Find vs DFS

DFS can also find connected components.

But Union Find is useful when edges are added and you need to keep merging groups.

If the problem gives many union operations and connectivity questions, Union Find is often cleaner.

## Complexity

With path compression and union by size or rank, operations are almost constant time.

Technically the complexity is `O(alpha(n))`, where `alpha` is the inverse Ackermann function.

In practice, it is so slow-growing that it behaves like `O(1)`.

Space complexity is `O(n)`.

## Common Mistakes

The first mistake is not using path compression.

The second mistake is not using rank or size.

Without these optimizations, trees can become tall.

The third mistake is unioning the original nodes instead of their roots.

Always union representatives.

The fourth mistake is using Union Find for directed graph cycle detection.

Union Find is mainly natural for undirected connectivity.

## Interview Explanation

A strong explanation sounds like this:

“I use Union Find because the problem is about whether items belong to the same connected group. Each item starts as its own parent. Find returns the group representative, and union merges two representatives. With path compression and union by size, operations are almost O(1).”

## Final Understanding

Union Find is about group identity.

If two items have the same leader, they are in the same group.

If not, union can merge their groups.

