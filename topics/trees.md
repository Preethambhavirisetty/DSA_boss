# Trees

Trees are graphs with a special, simpler shape — no loops, a clear top-down hierarchy — and that structure enables algorithms that wouldn't work on general graphs. Here they are in plain language.

## The core idea of a tree

A tree is a branching structure: one root at the top, splitting into children, which split into their own children, down to the leaves at the bottom — like a family tree or an org chart. The defining property is that there are no cycles and exactly one path between any two nodes. This clean hierarchy is what makes trees so useful: they let you organize data for fast searching, represent nested relationships, and break problems into independent subtrees. Most tree algorithms exploit that "each subtree is itself a smaller tree" self-similarity.

Tree Traversals (Inorder, Preorder, Postorder)

These are the three fundamental orders for visiting every node in a tree, and they differ only in when you handle a node relative to its children.

Preorder handles the node first, then its left subtree, then its right — useful when you need to process a parent before its descendants, like copying a tree or printing a hierarchy top-down. Inorder handles the left subtree, then the node, then the right — and for binary search trees this magically visits values in sorted order, which is its claim to fame. Postorder handles both subtrees first, then the node last — perfect when you must finish with the children before dealing with the parent, like deleting a tree or computing sizes from the bottom up.

Each comes in two flavors: recursive, which is short and natural because the function simply calls itself on each subtree, and iterative, which manually uses a stack to mimic that recursion — needed in environments where deep recursion would be risky. Same visiting order, different machinery.

Level Order Traversal (BFS)

This visits the tree row by row — all nodes at depth one, then all at depth two, and so on — top to bottom, left to right. It's just breadth-first search applied to a tree, using a queue to process nodes in the order they're discovered. It's the natural choice whenever the level of a node matters, like printing a tree shaped by its layers, finding the shortest number of steps to a node, or processing a hierarchy generation by generation.

Morris Traversal (O(1) space)

A clever traversal that visits every node without using recursion or an explicit stack — meaning it uses essentially no extra memory. The trick is to temporarily create shortcut links from certain nodes back to their successors, using those threads to find its way back up the tree instead of relying on a stack to remember where to return. After using each shortcut, it removes it, leaving the tree unchanged. It's an elegant solution for when memory is extremely tight, trading some conceptual complexity for near-zero extra space.

Lowest Common Ancestor (naive, binary lifting, Tarjan offline)

The lowest common ancestor of two nodes is their deepest shared parent — the node where the paths down to them split apart. There are several ways to find it, trading preprocessing effort for query speed.

The naive way walks upward from both nodes until the paths meet — simple but slow if you're asking many times. Binary lifting precomputes ancestor "jumps" of doubling distances (each node knows its parent, its grandparent's-grandparent, and so on in powers of two), letting you leap up the tree in big strides to answer each query fast. Tarjan's offline method cleverly answers a whole batch of queries at once during a single traversal, using union-find to track which ancestors have been resolved. The right choice depends on how many queries you'll ask and whether you can process them all together.

## Diameter of Tree

The diameter is the length of the longest path between any two nodes in the tree — the "width" of the tree at its most stretched. The elegant way to find it uses a single bottom-up pass: for each node, you compute how deep its subtrees go, and the longest path passing through that node is the sum of its two deepest branches. The overall diameter is the largest such value found at any node. It's a clean example of solving a whole-tree question by combining information gathered from each node's children.

## BST Insert / Delete / Search

A binary search tree keeps values organized so that at every node, smaller values live in the left subtree and larger values in the right. This ordering makes the three core operations efficient: to search, you compare with the current node and go left or right, halving the remaining tree each step. To insert, you search for where the value belongs and attach it there. To delete, you find the node and carefully patch the tree back together — the tricky case being a node with two children, where you replace it with the next-in-order value to preserve the ordering. When balanced, these operations are fast, which is the whole appeal of a BST.

AVL Tree rotations (self-balancing)

A plain binary search tree has a weakness: if you insert values in a bad order, it can degenerate into a long lopsided chain, making it as slow as a plain list. An AVL tree prevents this by self-balancing — after every insert or delete, it checks whether any part of the tree has become too lopsided and, if so, performs rotations: local rearrangements that pivot a few nodes to restore balance without breaking the search ordering. By keeping the tree height short and even, it guarantees consistently fast operations. It's strict about balance, doing whatever rotations are needed to stay nearly perfectly balanced.

## Red-Black Tree operations

Another self-balancing binary search tree, but with a more relaxed balancing rule than AVL. It colors nodes red or black and enforces a set of coloring rules that keep the tree from getting too lopsided — not perfectly balanced, but balanced enough to guarantee fast operations. Because its rules are looser, it does fewer rotations during inserts and deletes than AVL, making it faster at modifying data while AVL is faster at searching. This practicality is why red-black trees underpin many standard library structures (like the maps and sets in major programming languages).

Segment Tree (build/query/update, lazy propagation)

A structure built for answering range questions over an array quickly — like "what's the sum (or minimum, or maximum) between positions 5 and 20?" — while also allowing the array to change. It works by precomputing answers for segments of the array arranged in a tree: leaves hold individual elements, and each parent summarizes its children's range. Any query is answered by combining a handful of these precomputed segments rather than scanning everything. Lazy propagation is an enhancement for when you need to update whole ranges at once — instead of updating every affected element immediately, it defers ("lazily" postpones) updates and applies them only when necessary, keeping range-updates fast. It's a powerhouse for range-query-heavy problems.

Fenwick Tree / Binary Indexed Tree (BIT)

A more compact, cleverer structure that also answers cumulative range questions (typically running sums) with fast updates. It uses a subtle indexing trick based on the binary representation of positions to store partial sums in a way that lets both updates and prefix-sum queries be done quickly, using less memory and simpler code than a segment tree. It's less flexible than a segment tree (best suited to sums and similar operations) but beloved for being concise and efficient when it fits the problem.

## Trie operations

A trie is a tree specialized for storing strings by their shared prefixes — each path down from the root spells out characters, so words with common beginnings share the same early branches. Its operations reflect this: inserting a word walks down (creating nodes as needed) one character at a time; searching follows the characters to see if the word exists; and prefix queries ("which words start with 'pre'?") simply walk to the prefix and explore below. This makes it the natural structure for autocomplete, spell-checking, and dictionary lookups where prefixes matter.

## B-Tree / B+ Tree operations

These are trees designed for data too big to fit in fast memory — data living on disk, like in databases and filesystems. Unlike binary trees where each node has at most two children, B-trees let each node hold many keys and children, making the tree very short and wide. This shallowness matters because reading from disk is slow, so minimizing the number of levels (and thus disk reads) to find data is crucial. The B+ tree variant stores all actual data in the leaves and links those leaves together, which makes scanning through ranges of data efficient — exactly what databases need. Their operations (search, insert, delete) keep nodes appropriately full and the tree balanced as data changes.

## Euler Tour of a Tree

This is a technique that "flattens" a tree into a linear sequence by recording nodes in the order you'd visit them while walking around the tree's outline (entering and leaving each node). This linearization is powerful because it converts tree problems into array problems — questions about subtrees become questions about contiguous ranges in the flattened sequence, which you can then answer with array tools like segment trees. It's a bridge technique, letting you apply the fast range-query machinery to tree structures, and it underlies several advanced tree algorithms including some lowest-common-ancestor methods.

The organizing themes here. First, the traversals and their timing (pre/in/post/level) are the foundation — nearly every tree algorithm is really "traverse the tree and do something at each node," so mastering when to process a node relative to its children is the key primitive. Second, notice the recurring tension between ordering and balance: binary search trees give you fast search through ordering, but only if they stay balanced, which is exactly the problem AVL and red-black trees solve with rotations and coloring. Third, several structures here (segment tree, Fenwick tree, Euler tour) exist to answer range and subtree queries fast, and the Euler tour is the clever bridge that turns tree questions into the array questions those other structures handle so well.
