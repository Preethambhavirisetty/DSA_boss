# Heap

Heaps solve one recurring need extremely well: repeatedly grabbing the smallest (or largest) thing from a changing collection. Here they are in plain language.

## The core idea of a heap

A heap is a structure that always keeps the most extreme item — either the minimum or the maximum, depending on the type — instantly accessible at the top, without bothering to fully sort everything else. Picture a tournament bracket where the champion sits at the top, and everyone below is only loosely ordered relative to their own branch. The rule is simply that every parent outranks its children (a min-heap has each parent smaller than its children; a max-heap, larger). Because it doesn't waste effort fully sorting, a heap can insert new items and remove the top item quickly. This is exactly what a priority queue needs — a line where the highest-priority item is always served next — which is why "heap" and "priority queue" are often used interchangeably.

Binary Heap (heapify, insert, extract)

The standard heap, shaped as a binary tree (each node has up to two children) but cleverly stored in a plain array with no pointers needed, since the tree's shape is predictable. Its two core operations both work by "bubbling." To insert, you add the new item at the bottom and let it bubble upward, swapping with its parent as long as it outranks it, until it settles into place. To extract the top, you remove the root, move the last item up to fill the gap, then let it sink downward, swapping with its better child until order is restored — this sinking step is often called heapify. Both operations touch only one path from top to bottom, so they're fast. This is the heap you'll use most.

Build-Heap (O(n))

This addresses turning a random pile of items into a valid heap all at once. The obvious way — inserting them one by one — works but is slower than necessary. Build-heap is a smarter method: it arranges all items in the array first, then fixes up the heap order by sinking elements into place starting from the bottom-most parents and working upward. The surprising and elegant result is that this bulk construction is faster than repeated insertion — it builds the whole heap in time proportional to the number of items, rather than something slower. It's a favorite example of how order of operations can change an algorithm's efficiency.

Kth Largest / Smallest (heap-based)

A common task: find the Kth biggest (or smallest) item without fully sorting. The heap trick is neat — to find the Kth largest, you maintain a small min-heap holding only the K biggest items seen so far. As you process each new item, if it's bigger than the smallest in your heap (the top), you swap it in and evict the smallest. After going through everything, the heap's top is the Kth largest. You only ever keep K items around, which is memory-efficient and fast, especially when the full dataset is huge or streaming in.

## Merge K Sorted Lists

You have several already-sorted lists and want to combine them into one sorted list. Doing it cleverly, you use a heap holding the current front element of each list. You repeatedly pull the smallest (the heap top), add it to your result, and then pull in the next element from whichever list that item came from. The heap always tells you which list currently offers the smallest next item, so you efficiently weave all the lists together in order. It's a classic heap application and appears in things like merging sorted data streams or database results.

Median of a Stream (two heaps)

This solves a tricky problem: numbers arrive one at a time, and after each one you must report the current median (middle value) — but the data keeps growing, so you can't keep re-sorting. The elegant solution uses two heaps working together. A max-heap holds the smaller half of the numbers (with the largest of them on top), and a min-heap holds the larger half (with the smallest of them on top). By keeping the two halves balanced in size, the median is always sitting right at the top of one or both heaps — instantly available. Each new number is placed into the appropriate half and the halves are rebalanced. It's a beautiful example of combining two heaps to keep a running middle value.

## Fibonacci Heap

A more advanced heap designed to make certain operations extremely fast in the long run. Its standout feature is that merging two heaps and decreasing an item's priority are remarkably cheap on average. This matters because algorithms like Dijkstra's shortest path repeatedly decrease priorities, and a Fibonacci heap can theoretically speed them up. The catch is that it achieves this through a complex, lazy internal structure that only tidies itself up when forced to — which makes it fast in theory but often slower in practice due to overhead. It's more celebrated for its theoretical elegance than for everyday use.

## Binomial Heap

Another heap built specifically to support merging two heaps efficiently, which a plain binary heap does poorly. It's structured as a collection of smaller heaps of specific sizes (tied to powers of two, analogous to how binary numbers add), so merging two binomial heaps works much like adding two binary numbers — combining pieces of matching sizes and carrying over. It's the stepping stone toward the Fibonacci heap and the go-to conceptual example when fast merging ("melding") of priority queues is the priority.

## Pairing Heap

A simpler alternative to the Fibonacci heap that achieves much of the same practical speed with far less complexity. It has a very loose structure — essentially a tree where merging is done by just comparing two roots and hanging one under the other, and cleanup happens by pairing up children when the top is removed. Despite its simplicity, it performs excellently in real-world use, often beating the fancier Fibonacci heap in practice. It's a nice reminder that a simpler design can outperform a theoretically superior but complicated one.

## D-ary Heap

A generalization of the binary heap where each node has D children instead of just two (so a 4-ary heap gives each node four children). Widening the tree makes it shallower, which speeds up the "sink downward" operations that traverse from top to bottom — but each sink step now has more children to compare against. This creates a tunable trade-off: more children means faster inserts (shorter climb up) but slightly slower extractions (more comparisons per level). By tuning D to the ratio of inserts versus extractions in your workload, you can optimize performance for your specific situation.

The themes to carry away. First, a heap's whole value proposition is partial order — by refusing to fully sort and only guaranteeing the top item, it makes "give me the extreme" and "add a new item" both fast, which is precisely what priority-driven tasks need. Second, notice how the application problems (Kth largest, merge K lists, median of stream) are all really the same move: keep a heap of just the relevant items so the answer is always sitting at the top. Third, the exotic heaps (Fibonacci, binomial, pairing) mostly exist to make merging and priority-decreasing fast — operations the basic binary heap handles poorly — and among them, the simplest (pairing) often wins in practice, a recurring lesson that theoretical superiority and real-world speed aren't the same thing.
