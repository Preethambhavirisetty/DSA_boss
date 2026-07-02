# Graph

A graph is a collection of nodes connected by edges.

Nodes are also called vertices.

Edges represent relationships between nodes.

Graphs are used to model networks, maps, dependencies, social connections, grids, and many real-world systems.

## Graph Examples

A road map is a graph.

Cities are nodes.

Roads are edges.

A social network is a graph.

People are nodes.

Friendships are edges.

A course schedule is a graph.

Courses are nodes.

Prerequisites are directed edges.

## Directed vs Undirected

In an undirected graph, edges go both ways.

If `A` is connected to `B`, then `B` is connected to `A`.

In a directed graph, edges have direction.

If `A -> B`, you can go from `A` to `B`, but not necessarily from `B` to `A`.

This difference changes the algorithm.

## Weighted vs Unweighted

In an unweighted graph, every edge has the same cost.

BFS can find shortest paths in unweighted graphs.

In a weighted graph, edges have costs.

Dijkstra is used when weights are non-negative.

Bellman-Ford is used when negative weights may exist.

## Adjacency List

The most common graph representation is an adjacency list.

```js
const graph = {
  A: ["B", "C"],
  B: ["A", "D"],
  C: ["A"],
  D: ["B"],
};
```

For each node, store its neighbors.

This is memory efficient for sparse graphs.

## Adjacency Matrix

An adjacency matrix uses a 2D table.

If `matrix[i][j]` is true or 1, there is an edge from `i` to `j`.

This makes edge lookup fast, but it uses `O(V^2)` space.

Use it when the graph is dense or when constant-time edge lookup matters.

## BFS

BFS uses a queue.

It explores nearby nodes before far nodes.

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

BFS is good for shortest path in unweighted graphs.

## DFS

DFS goes deep before coming back.

```js
function dfs(node, graph, visited = new Set()) {
  if (visited.has(node)) return;

  visited.add(node);

  for (const next of graph[node]) {
    dfs(next, graph, visited);
  }
}
```

DFS is good for connected components, cycle detection, topological thinking, and exploring all paths.

## Visited Set

Graphs can have cycles.

Without a visited set, traversal may never stop.

Example:

```txt
A -- B
|    |
D -- C
```

You can move in circles forever.

The visited set says:

“I have already processed this node.”

## Grid as Graph

Many grid problems are graph problems.

Each cell is a node.

Moving up, down, left, and right creates edges.

Number of Islands is a classic example.

Land cells connected together form one component.

## Complexity

Graph traversal is usually `O(V + E)`.

`V` is number of vertices.

`E` is number of edges.

Each node is visited once.

Each edge is checked once or twice depending on direction.

Space is `O(V)` for visited set and queue or recursion stack.

## Common Mistakes

The first mistake is forgetting visited.

The second mistake is treating directed and undirected graphs the same.

The third mistake is using BFS for weighted shortest path.

The fourth mistake is not handling disconnected graphs.

Sometimes you must start traversal from every unvisited node.

## Interview Explanation

A strong explanation sounds like this:

“I represent the graph with an adjacency list. I use BFS or DFS depending on the goal. I keep a visited set to avoid cycles. Each node and edge is processed at most once, so the time complexity is O(V + E), and the space complexity is O(V).”

## Final Understanding

Graphs are about relationships.

The key is defining what the nodes are and what the edges mean.

Once that is clear, BFS and DFS become much easier to apply.

