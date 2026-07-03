# Tree Data Structures

Trees are one of the most important structure families in computing, and this list spans four very different purposes — organizing for search, storing on disk, handling strings, and answering range queries. Here they all are in plain language, grouped as you have them.

3. Trees — General & Binary

## Binary Tree

The most basic branching structure where each node has at most two children (a left and a right). It's the foundation everything else in this section builds on. On its own, a plain binary tree imposes no rules about where values go — it's just the shape. Its value is as a building block and as the structure for things like expression trees and heaps. Think of it as the raw skeleton; the structures below add rules that give that skeleton a purpose.

Binary Search Tree (BST)

A binary tree with one powerful rule added: at every node, everything in the left subtree is smaller and everything in the right subtree is larger. This ordering is what makes searching fast — to find a value, you compare and go left or right, discarding half the remaining tree at each step, much like binary search. It supports fast search, insertion, and deletion when balanced. The catch, and the reason all the next structures exist, is that a BST can become lopsided if values arrive in a bad order, degrading into a slow chain.

Balanced BST (general concept)

This isn't a specific structure but the idea that fixes the BST's weakness. A balanced BST actively keeps itself short and bushy rather than tall and lopsided, guaranteeing that search always stays fast no matter what order values arrive in. The various structures below (AVL, red-black, treap, and others) are all different strategies for achieving this balance — they differ in how strictly they balance and how they detect and fix imbalance. Understanding this concept is key: everything after it is just a different answer to "how do we stay balanced?"

## AVL Tree

The strictest balancing strategy. After every insertion or deletion, it checks whether any part of the tree has become even slightly too lopsided, and if so, immediately performs rotations — small local rearrangements that rebalance without breaking the search ordering. Because it maintains near-perfect balance, searches are as fast as possible. The trade-off is that this strictness means more work during insertions and deletions. So AVL is the choice when you search far more often than you modify.

## Red-Black Tree

A more relaxed balancing strategy. It tags each node red or black and enforces coloring rules that keep the tree reasonably balanced — not perfectly, but enough to guarantee fast operations. Because its rules are looser than AVL's, it does fewer rotations when inserting and deleting, making modifications faster while searches are slightly slower than AVL's. This practical balance is why red-black trees are the standard implementation behind the ordered maps and sets in most programming language libraries.

## Splay Tree

A self-adjusting strategy with a distinctive philosophy: every time you access a node, it rotates that node all the way up to the root. The bet is that recently-used items will be used again soon, so keeping them near the top makes those repeat accesses fast. It makes no promise of balance at any single moment, but over a run of operations it performs very well and automatically adapts to how you actually use it. It's ideal when your access pattern is "clustered" — the same few items touched repeatedly.

## Treap / Cartesian Tree

A structure that stays balanced using randomness instead of strict rules. "Treap" fuses "tree" and "heap": each node holds both its actual value (kept in search-tree order) and a random priority (kept in heap order, parent outranks children). Because the priorities are random, the tree naturally stays balanced on average — the randomness does the job that careful rotations do in AVL. It's loved for being simple to implement while giving reliable performance, and it elegantly supports splitting a tree in two or merging two trees.

## Scapegoat Tree

A balancing strategy that uses no extra stored information per node (unlike AVL's height counts or red-black's colors). Instead, it lets the tree drift out of balance and only fixes it when a subtree becomes too lopsided — at which point it finds the "scapegoat" node responsible and completely rebuilds that entire subtree into perfect balance from scratch. These rebuilds are occasionally expensive but rare enough that the average cost stays low. It's an interesting alternative that trades occasional big rebuilds for simplicity and low memory overhead.

## Weight-Balanced Tree

A balancing strategy that keeps the tree balanced based on the number of nodes (the "weight") in each subtree, rather than by height or color. It ensures neither side of any node holds too large a fraction of the total nodes, rebalancing when the proportions get too skewed. A useful side benefit of tracking subtree sizes is that it can quickly answer rank-style questions like "what's the Kth smallest element" or "how many elements are less than X." It's chosen when those order-statistic queries matter alongside balance.

## N-ary / General Tree

A tree where each node can have any number of children, not just two — like a family tree, an organization chart, or the folder structure on your computer. It drops the binary restriction entirely. It's the natural way to represent genuinely hierarchical data where the branching factor varies. It's less about fast searching and more about faithfully modeling nested, multi-child relationships.

## Threaded Binary Tree

A binary tree enhanced with extra shortcut links that make traversal faster and possible without recursion or a stack. In an ordinary binary tree, many child-pointers sit unused (pointing to nothing); a threaded tree repurposes those empty pointers to point directly to the node that comes next in traversal order. This lets you walk through the tree in sorted order using minimal memory by simply following the threads. It's a memory-efficiency optimization for traversal-heavy uses.

## Expression Tree

A binary tree that represents a mathematical expression, where leaves hold values (numbers) and internal nodes hold operations (like plus or times). For example, "3 + 4 × 5" becomes a tree whose structure captures the correct order of operations. Evaluating the expression is then just a matter of traversing the tree, combining children according to each node's operation. It's fundamental to how calculators, compilers, and interpreters understand and compute expressions — the bridge between a written formula and its actual computation.

4. Trees — Search & Multiway (Disk / DB)

These share a purpose: storing huge amounts of data that lives on disk rather than in fast memory. The key insight driving all of them is that reading from disk is dramatically slower than reading from memory, so the goal is to minimize the number of disk reads by keeping the tree very short and wide — packing many keys into each node so you reach any data in just a few hops.

## B-Tree

The workhorse of databases and filesystems. Unlike a binary tree where each node holds one value and has two children, a B-tree node holds many values and has many children, keeping the tree shallow. Fewer levels means fewer slow disk reads to find anything. It stays balanced as data is added and removed by splitting overly-full nodes and merging under-full ones. This design — trading tall-and-thin for short-and-wide — is precisely what makes it ideal when your data is too big for memory.

## B+ Tree

A refinement of the B-tree that databases actually prefer in practice. Its distinguishing feature is that all the real data lives in the leaf nodes, while the internal nodes hold only keys for navigation. Crucially, the leaves are linked together in order, so once you find a starting point, you can scan sequentially through a range of data by just following the leaf links — no need to climb back up the tree. This makes range queries ("give me all records between X and Y") very efficient, which is exactly what databases need constantly.

## B* Tree

A variant of the B-tree tuned to keep its nodes fuller than a standard B-tree does. When a node overflows, instead of immediately splitting it, it first tries to shift some entries to a neighboring node, delaying splits and keeping nodes more densely packed. Fuller nodes mean the tree is even shorter and uses space more efficiently. It's an optimization for squeezing more out of each node and reducing wasted space.

## 2-3 Tree

A specific, simple kind of balanced multiway tree where every internal node has either 2 or 3 children (and correspondingly 1 or 2 values). It stays perfectly balanced by design — all leaves sit at the same depth. It's often taught as a conceptual stepping stone because its rules are clean and easy to reason about, and it's closely related to red-black trees (a red-black tree is essentially a 2-3-4 tree in disguise). It's more of a teaching and conceptual structure than a production one.

## 2-3-4 Tree

An extension where nodes can have 2, 3, or 4 children. Like the 2-3 tree, it stays perfectly balanced with all leaves at equal depth, splitting and merging nodes as needed. Its main significance is theoretical: it maps directly onto the red-black tree, so understanding it illuminates why red-black trees work the way they do. It's a bridge concept connecting the clean world of multiway trees to the binary red-black trees used in practice.

## Fusion Tree

An advanced, theoretical structure for storing integers that cleverly packs multiple keys into a single machine word and uses bit-level tricks to compare many of them at once. This lets it search faster than the usual comparison-based limits by exploiting the fixed size of computer words. It's primarily of theoretical interest — a demonstration that clever bit manipulation can beat comparison-based bounds for integer keys — rather than something you'd typically implement.

5. Trees — String / Prefix

These specialize in storing and searching strings, exploiting the fact that words often share common pieces (prefixes or substrings).

Trie (Prefix Tree)

A tree that stores words by their shared beginnings. Each step down the tree represents one character, so words starting the same way share the same early path before branching apart. This makes prefix questions — "which words start with 'app'?" — extremely fast, since you just walk to the prefix and look below. It's the structure behind autocomplete, spell-checkers, and dictionary lookups. Its downside is memory: it can use a lot of space because it stores structure for every character of every word.

Compressed Trie / Radix Tree (Patricia Trie)

A space-saving improvement on the trie. A plain trie often has long chains of single-child nodes (a stretch where a word has no branching), which waste memory. A compressed trie collapses each such chain into a single node holding the whole substring. This dramatically reduces the number of nodes while preserving all the trie's search power. It's the practical, memory-efficient version, used in IP routing tables and other lookup-heavy systems.

## Ternary Search Tree

A hybrid that combines the prefix-search ability of a trie with the memory efficiency of a binary search tree. Each node has three children — for characters less than, equal to, and greater than the current one. This avoids the trie's habit of reserving space for every possible character at every position, using far less memory while still supporting prefix and near-match searches. It's a nice middle ground when a full trie would be too memory-hungry.

## Suffix Tree

A tree that compactly stores every suffix (every possible tail-end) of a single string. Because it contains all suffixes, it can answer a huge range of questions about that string almost instantly — finding a pattern, counting occurrences, finding the longest repeated section, and more. It's extremely powerful but notoriously memory-hungry and intricate to build. It's the heavyweight tool for deep analysis of a fixed text.

## Suffix Automaton

A more compact alternative to the suffix tree — a small state machine that recognizes every substring of a string. Despite staying remarkably small even for long strings, it answers substring questions (how many distinct substrings exist, does a pattern appear) very efficiently. It achieves much of the suffix tree's power in less space, though it's abstract enough to take effort to build intuition for.

## Suffix Array

A more memory-friendly cousin of the suffix tree. Rather than a tree, it's simply a sorted list of all the string's suffixes (stored compactly as their starting positions). Once sorted, searching for a pattern becomes a binary search over the suffixes. It offers much of the suffix tree's searching capability in a far more compact, cache-friendly form, which is why it's often preferred in practice — you get most of the power without the heavy memory cost.

## Aho-Corasick Automaton

A structure built for searching many patterns at once. It arranges all the search patterns into a shared trie (merging common prefixes), then adds "failure links" — shortcuts that let the search recover gracefully when a partial match breaks, jumping to the longest other pattern-prefix still in play without losing progress. Feeding a text through it once finds all occurrences of all patterns simultaneously. It's the go-to for scanning text against a whole dictionary of terms — think content filters or virus signature scanning.

6. Trees — Range & Query

These exist to answer questions about ranges of data — sums, minimums, maximums over a span — far faster than scanning, usually while allowing the data to change.

## Segment Tree

The versatile champion of range queries. It answers questions like "what's the sum (or min, or max) between positions 5 and 20?" quickly, and it allows the underlying data to be updated. It works by precomputing answers for progressively larger chunks of the array arranged in a tree, so any query combines just a handful of precomputed chunks rather than scanning everything. It's the flexible general-purpose tool whenever range questions dominate.

## Segment Tree with Lazy Propagation

An enhancement for when you need to update entire ranges at once, not just single elements. Naively, adding a value to a whole range would mean touching every element — slow. Lazy propagation instead records a "pending update" at the appropriate node and only actually applies it downward when that part of the tree is later visited. By deferring ("being lazy" about) updates until they're needed, it keeps whole-range updates fast. It's essential for problems mixing range updates with range queries.

## Persistent Segment Tree

A version that preserves every past state of the tree. Normally, updating a structure destroys its previous version; a persistent segment tree keeps all historical versions accessible, so you can query the data "as it looked" at any earlier moment. The clever part is that it does this efficiently by sharing the unchanged portions between versions rather than copying the whole tree each time. It's invaluable for problems that ask about the data's state at various points in its history.

Fenwick Tree / Binary Indexed Tree (BIT)

A more compact, elegant structure for cumulative range queries — typically running sums — with fast updates. It uses a subtle indexing scheme based on the binary representation of positions to store partial sums, letting both updates and prefix-sum queries be done quickly with strikingly little code and memory. It's less flexible than a segment tree (best for sums and similar operations) but beloved when it fits, precisely because it's so concise and efficient.

## 2D Fenwick Tree

The Fenwick tree extended to two dimensions — a grid instead of a line. It answers questions like "what's the sum of values inside this rectangle?" while allowing individual grid cells to be updated. It applies the same binary-indexing trick along both dimensions at once. It's the tool for cumulative queries over matrices or 2D grids, useful in image processing and spatial counting problems.

Sparse Table (static RMQ)

A structure specialized for repeatedly finding the minimum or maximum in a range, but only when the data never changes. It precomputes answers for every range whose length is a power of two, so any arbitrary range can be covered by two overlapping precomputed ranges — giving near-instant answers. The trade-off is rigidity: because it precomputes everything, it can't handle updates. For static data with a flood of min/max queries, it's the fastest option available.

## Merge Sort Tree

A segment tree variant where each node stores not just a summary but a sorted list of all the elements in its range. This lets it answer more complex queries, like "how many values in this range are less than X?" by binary-searching within the relevant nodes' sorted lists. It's more memory-intensive but handles a class of range questions that a plain segment tree can't. It's a specialized tool for counting-style range queries.

## Wavelet Tree

An advanced structure that recursively splits data by value (rather than position), enabling powerful queries like "what's the Kth smallest element in this range?" or "how many times does this value appear in this range?" It cleverly encodes the data so these otherwise-hard queries become efficient. It's a sophisticated tool that appears in competitive programming and compressed-data applications, valued for the unusual range queries it can answer.

Sqrt Decomposition (block structure)

A pragmatic general technique that breaks the array into blocks of roughly the square root of its length, precomputing a summary for each block. A range query then uses the whole-block summaries for complete blocks and handles the partial edges directly. This balances the work so nothing gets too large. It's often simpler to implement than a segment tree and flexible enough for problems where fancier structures are awkward — a reliable middle-ground tool.

The organizing themes across all four groups. First, in the general/binary group, notice that after the plain binary tree and BST, every structure is really answering one question — "how do we stay balanced?" — with different trade-offs: AVL balances strictly (fast search, slower updates), red-black balances loosely (faster updates), splay adapts to usage, and treap uses randomness. Second, the disk/DB group is unified by one physical reality: disk is slow, so make the tree short and wide by packing many keys per node — and the B+ tree's linked leaves are the specific feature that makes database range scans efficient. Third, the string group all exploit shared structure between strings (prefixes in tries, suffixes in suffix trees/arrays), trading memory for search power, with the compressed and array-based versions being the memory-conscious choices. Fourth, the range/query group is unified by "precompute cleverly so range questions avoid scanning," where your choice hinges on whether the data changes (segment/Fenwick can update; sparse table can't) and what kind of query you need (sums favor Fenwick, min/max favor sparse table, complex counting favors merge sort or wavelet trees).
