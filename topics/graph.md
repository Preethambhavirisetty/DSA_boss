# Graph Algorithms

This is the biggest category and the most interconnected, so before the individual algorithms, a quick orientation on what a graph even is and why so many algorithms exist for them.

## The core idea of graphs

A graph is just things (called nodes or vertices) connected by relationships (called edges) — cities linked by roads, people linked by friendships, tasks linked by dependencies. Almost any problem involving connections maps onto a graph. The reason there are so many graph algorithms is that different questions — "can I get from A to B?", "what's the cheapest route?", "is everything connected?", "how much can flow through?" — each need their own approach, and the specific properties of your graph (are edges weighted? can weights be negative? is it a tree? is it directed?) determine which algorithm is valid. Learning graphs is largely about matching the question and the graph's properties to the right tool.

## Traversal

Traversal is how you systematically visit nodes. Everything else builds on these two foundations.

## Breadth-First Search (BFS)

You explore outward in layers, like ripples spreading from a stone dropped in water. Starting from a node, you visit all its immediate neighbors first, then all their unvisited neighbors, and so on — expanding level by level. Because it explores in order of distance, BFS naturally finds the shortest path when every step counts equally (unweighted graphs). It uses a queue (first-in-first-out) to remember who to visit next. This layer-by-layer behavior is its defining trait and the reason it's the go-to for shortest-path-by-number-of-steps.

## Depth-First Search (DFS)

You plunge as deep as possible down one path before backing up, like exploring a maze by always pushing forward until you hit a dead end, then retreating to the last fork. It uses a stack (often just the natural recursion stack) to remember where to backtrack. DFS is the backbone of an enormous number of other algorithms — cycle detection, topological sorting, finding connected components — because its natural "go deep, then unwind" rhythm reveals structural information about the graph as it explores.

## 0-1 BFS (deque)

A specialized BFS for graphs where every edge costs either 0 or 1. Normal BFS assumes all steps are equal, and Dijkstra's (the general weighted tool) is overkill here. 0-1 BFS uses a double-ended queue: when you traverse a free (cost-0) edge, you add the node to the front of the queue (process it soon, since it's just as close); when you traverse a cost-1 edge, you add to the back. This keeps nodes processed in distance order without the heavier machinery of Dijkstra's, making it a fast, elegant fit for that specific 0-or-1 situation.

## Multi-source BFS

Ordinary BFS starts from one node; multi-source BFS starts from several at once. You seed the queue with all starting nodes simultaneously and expand outward from all of them together. This answers questions like "what's the distance from each cell to the nearest fire?" — where you want closeness to the closest of many sources. It's the same layer-by-layer expansion, just with multiple origins, and it elegantly avoids running separate searches from each source.

## Iterative Deepening DFS

This blends DFS's low memory use with BFS's shortest-path guarantee. It runs DFS but with a depth limit, and if it doesn't find the goal, it increases the limit and restarts from scratch — going a little deeper each round. This sounds wasteful (re-exploring shallow parts repeatedly), but because the number of nodes grows so fast with depth, the redundant work is minor. It's used when the graph is huge or infinite and you need the shallowest solution but can't afford BFS's memory.

## Shortest Path

## Dijkstra's Algorithm

The workhorse for finding shortest paths in weighted graphs, as long as no edge is negative. It greedily locks in the closest unvisited node, then uses it to improve distance estimates to its neighbors, repeating until all nodes are finalized. Its correctness rests entirely on non-negative weights — that assumption is what lets it confidently declare a node's distance final. It's efficient and ubiquitous (GPS routing, network paths), but that one condition is non-negotiable.

## Bellman-Ford (negative edges)

The slower but more capable cousin of Dijkstra's, this handles graphs with negative edge weights. Instead of greedily finalizing nodes, it patiently relaxes every edge repeatedly, letting improvements ripple through the graph over multiple rounds. Because it doesn't assume non-negativity, it works where Dijkstra's fails — and as a bonus, it can detect negative cycles (loops that keep reducing cost forever), which would make "shortest path" meaningless. You reach for it precisely when negative weights are in play.

## Floyd-Warshall (all-pairs)

Where Dijkstra's and Bellman-Ford find paths from one source, Floyd-Warshall finds the shortest path between every pair of nodes at once. Its idea is beautifully simple: for each possible intermediate node, check whether routing through it shortens any pair's path, and update accordingly. Repeating this for every node as a potential waypoint yields all shortest paths. It's best for small, dense graphs where you genuinely need every pairwise distance, and its simplicity makes it a favorite despite being slow on large graphs.

## Johnson's Algorithm (all-pairs, sparse)

This also finds all-pairs shortest paths but is designed for sparse graphs (few edges relative to nodes) and handles negative weights. The clever move is a preprocessing step that reweights all edges to be non-negative (without changing which paths are shortest), so it can then run fast Dijkstra from every node. It essentially combines Bellman-Ford's negative-weight handling with Dijkstra's speed, making it faster than Floyd-Warshall when the graph isn't dense.

## A Search (heuristic)*

An upgrade to Dijkstra's that uses a hint to search smarter. Along with the known distance so far, it adds an estimate of how far the goal still is (a "heuristic," like straight-line distance on a map), and prioritizes nodes that seem to lead toward the goal. This focuses the search in the goal's direction instead of expanding blindly in all directions, making it much faster for point-to-point pathfinding. It's the standard in video game navigation and mapping — as long as the estimate never overshoots the true remaining distance, it still finds the genuine shortest path.

## SPFA (Shortest Path Faster Algorithm)

An optimization of Bellman-Ford that, instead of blindly relaxing every edge every round, only revisits nodes whose distance actually changed (keeping them in a queue). This often runs much faster than plain Bellman-Ford in practice while still handling negative weights. The caveat is that its worst case is no better than Bellman-Ford's and can be deliberately triggered, so it's a practical speedup rather than a guaranteed one.

## Minimum Spanning Tree

All three below solve the same problem — connect all nodes as cheaply as possible with no redundant loops — using different greedy strategies.

## Prim's Algorithm

Grows the tree as a single expanding blob. Starting from one node, it repeatedly adds the cheapest edge that connects the growing blob to a new outside node, until everyone's included. It's like a spreading stain that always extends in the cheapest available direction. It tends to be efficient on dense graphs.

## Kruskal's Algorithm

Takes a global view instead. It sorts all edges cheapest-first and adds them one by one, skipping any edge that would create a loop. The tree grows as scattered fragments that gradually merge into one. It relies on the union-find structure (below) to quickly detect whether adding an edge would form a loop, and it's often the more intuitive of the two.

## Borůvka's Algorithm

The oldest and most parallel-friendly approach. In each round, every fragment simultaneously picks its own cheapest outgoing edge, and all those chosen edges are added at once, merging many fragments in parallel. Repeated rounds rapidly consolidate everything into one tree. Its round-based, everyone-acts-at-once nature makes it especially suited to parallel computing.

## Connectivity & Ordering

## Topological Sort (Kahn's BFS & DFS)

For graphs representing dependencies (task B requires task A first), a topological sort produces a valid order to do everything so no task comes before its prerequisites. Kahn's version repeatedly picks nodes with no remaining prerequisites, removes them, and updates the rest — a BFS-style approach. The DFS version builds the order by finishing deep nodes first and reversing. Both only work on graphs without cycles (a cycle means impossible dependencies), and this underlies build systems, course scheduling, and spreadsheet recalculation.

## Kosaraju's Algorithm (SCC)

This finds "strongly connected components" — clusters in a directed graph where every node can reach every other node in the cluster. Kosaraju's does it with two passes of DFS: one on the original graph to order nodes by finish time, and one on the graph with all edges reversed, which neatly peels off each cluster. It's the more intuitive of the two main SCC algorithms, valued for being conceptually clean even if it makes two full passes.

## Tarjan's Algorithm (SCC / bridges / articulation points)

A more efficient single-pass DFS that finds strongly connected components (and, with tweaks, bridges and articulation points too). As it explores, it tracks how far "back up" the graph each node can reach via alternate routes, and uses that to identify clusters and critical connections in one sweep. It's more elegant and faster than Kosaraju's but a bit harder to grasp — a common trade in these algorithms.

## Union-Find / Disjoint Set Union

A wonderfully simple structure for tracking which group each element belongs to, supporting two operations: "are these two in the same group?" and "merge these two groups." It represents each group as a tree with a representative root. Two optimizations make it nearly instant: path compression flattens the trees during lookups so future queries are faster, and union by rank keeps trees shallow by attaching smaller ones under larger ones. It's the engine behind Kruskal's algorithm and countless connectivity problems.

## Bridges and Articulation Points

These find the critical parts of a network. A bridge is an edge whose removal would split the graph into disconnected pieces; an articulation point is a node whose removal would do the same. Found via a DFS that tracks reachability (the same back-tracking idea as Tarjan's), they identify single points of failure — vital for analyzing network robustness, where you want to know which link or hub, if it fails, breaks connectivity.

## Eulerian Path/Circuit (Hierholzer's)

An Eulerian path uses every edge exactly once; if it also returns to the start, it's a circuit. (This is the famous "can you trace this shape without lifting your pen?" puzzle.) There's a neat rule based on how many edges meet at each node that tells you instantly whether one exists, and Hierholzer's algorithm efficiently constructs it by stitching together small loops into one grand tour. Note the contrast with the next item — Eulerian is about edges and is easy.

## Hamiltonian Path/Cycle

A Hamiltonian path visits every node exactly once (a cycle also returns to start). Deceptively similar to Eulerian, but vastly harder — there's no known fast method, and it's one of the classic "hard" problems. The lesson in placing these side by side: "visit every edge once" (Eulerian) is easy, while "visit every node once" (Hamiltonian) is brutally hard, despite sounding almost identical — a striking illustration of how a tiny change in a problem can flip its difficulty entirely.

## Flow & Matching

This family models how much can move through a network (like water through pipes) and who can be paired with whom.

## Ford-Fulkerson (max flow)

The foundational max-flow method: given a network of pipes with capacities, how much can flow from source to sink? The idea is to repeatedly find any path from source to sink with spare capacity, push as much flow as it allows, and repeat until no such path remains. A key subtlety is allowing flow to be "undone" along the way, giving the algorithm room to reroute and find the true maximum. It's the conceptual parent of the more refined methods below.

## Edmonds-Karp

A specific, well-behaved version of Ford-Fulkerson that always picks the shortest augmenting path (using BFS). This seemingly small choice gives it a guaranteed, predictable running time, fixing Ford-Fulkerson's vagueness about which path to pick (a bad choice could make the original very slow). It's Ford-Fulkerson made reliable.

## Dinic's Algorithm

A faster max-flow method that pushes flow along many shortest paths at once per phase, rather than one path at a time. It organizes the graph into layers by distance and shoves flow through efficiently, making it substantially quicker than Edmonds-Karp on large networks. It's the practical choice when max flow needs real performance, and it's especially fast on the bipartite matching graphs below.

## Hopcroft-Karp (bipartite matching)

Specialized for bipartite matching — pairing up two groups (like workers to jobs) so as many pairs as possible are matched, where only certain pairings are allowed. It cleverly finds multiple improving pairings simultaneously each round rather than one at a time, achieving notably better speed than the naive approach. It's the standard fast tool whenever you're maximizing valid pairings between two sets.

## Hungarian Algorithm (assignment)

Solves the assignment problem: pair every worker to exactly one job (and vice versa) so the total cost is minimized — not just any matching, but the cheapest complete one. It systematically adjusts a cost table to reveal the optimal assignment. It's the classic tool for optimal one-to-one assignment with costs, used in operations research and scheduling.

## Min-Cost Max-Flow

Combines two goals: push the maximum possible flow and, among all ways to achieve that maximum, do it at the least total cost (when each pipe also has a per-unit cost). It works by repeatedly sending flow along the cheapest available augmenting path. It generalizes both max-flow and the assignment problem, and it's the go-to when both throughput and cost matter together.

## Push-Relabel

An alternative high-performance max-flow approach that works differently from the path-finding family. Instead of finding complete source-to-sink paths, it lets individual nodes accumulate excess flow and "push" it to neighbors, using a notion of height to guide flow downhill toward the sink. This local, node-by-node action tends to be faster than path-based methods on many graphs, making it a top performer for demanding max-flow problems.

## Special

## Bipartite Check (2-coloring)

Determines whether a graph's nodes can be split into two groups such that every edge connects the two groups (no edge stays within a group). You test this by trying to color nodes with two colors during a traversal, giving each neighbor the opposite color — if you're ever forced to give two connected nodes the same color, it's not bipartite. This matters because bipartite graphs enable special efficient algorithms (like the matching ones above), so checking is often a first step.

## Cycle Detection (directed & undirected)

Determines whether a graph contains a loop. The method differs by graph type: in undirected graphs, finding an already-visited node (that isn't where you just came from) signals a cycle; in directed graphs, you look for an edge pointing back to a node still being explored in the current path. Cycle detection underlies deadlock detection, dependency validation, and is a prerequisite check for topological sorting.

## Lowest Common Ancestor (Binary Lifting, Euler + Sparse Table)

In a tree, the lowest common ancestor of two nodes is their deepest shared parent — the point where the paths to them diverge. Naively you'd walk upward from both, but that's slow for many queries. Binary lifting precomputes ancestor "jumps" of doubling sizes so you can leap up the tree in big steps, answering queries fast. The Euler tour + sparse table method transforms the tree-ancestor question into a range-minimum question over a flattened tree. Both are preprocessing tricks that make repeated ancestor queries lightning-fast.

## Heavy-Light Decomposition

A technique for answering questions about paths in a tree efficiently — like "what's the sum/max along the path between these two nodes?" It cleverly breaks the tree into a set of straight chains such that any root-to-node path crosses only a few chains. Combined with a range-query structure on each chain, this turns slow path queries into fast ones. It's advanced tooling for heavy tree-query workloads in competitive programming.

## Centroid Decomposition

Another tree technique that recursively splits the tree at its "centroid" — a balancing node whose removal leaves only small pieces. This builds a hierarchy that makes certain path-counting and distance problems across the whole tree efficient, because it guarantees the recursion stays shallow. Like heavy-light decomposition, it's a specialized structure for hard tree problems, particularly those about paths between all pairs of nodes.

## Kruskal Reconstruction Tree

A structure built during Kruskal's MST process that captures information about how components merge as edges are added in increasing weight order. It's useful for answering questions like "what's the minimum possible maximum-edge-weight on a path between two nodes" — bottleneck-style queries. It repackages the MST-building process into a tree you can query, a neat example of extracting extra structure from an existing algorithm.

## Travelling Salesman (exact & approx)

The famous problem of finding the shortest route visiting every city once and returning home. It's genuinely hard — no known fast exact method exists. The exact approach for small inputs uses bitmask DP (tracking which cities are visited), but it becomes impractical as cities grow. So for larger instances, approximation algorithms settle for routes that are provably "good enough" — close to optimal but not guaranteed perfect. It's the poster child for hard optimization problems and the practical necessity of accepting approximate answers.

The two organizing principles worth internalizing. First, the graph's properties dictate the algorithm — unweighted means BFS, non-negative weights mean Dijkstra's, negative weights force Bellman-Ford, all-pairs on a dense graph means Floyd-Warshall. Learning to read those properties off a problem is most of the skill. Second, notice the recurring theme that DFS's back-reachability tracking is a hidden workhorse — Tarjan's SCC, bridges, articulation points, and cycle detection are all the same DFS idea (tracking how far back you can reach) applied to different questions. And keep the Eulerian-versus-Hamiltonian contrast in mind as a permanent reminder that near-identical-sounding problems can live on opposite sides of the easy/hard divide.
