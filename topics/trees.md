# Trees

A tree is a group of nodes connected in a parent-child structure.

One node is the root.

Every child can have its own children.

This creates a hierarchy.

## Basic Tree Terms

The root is the top node.

A parent is a node that has children.

A child is a node below another node.

A leaf is a node with no children.

Depth means how far a node is from the root.

Height means how far a node is from the deepest leaf below it.

## Binary Tree

A binary tree is a tree where each node has at most two children.

They are usually called `left` and `right`.

```js
class TreeNode {
  constructor(value) {
    this.value = value;
    this.left = null;
    this.right = null;
  }
}
```

Many interview tree problems use binary trees.

## Why Trees Use Recursion

Trees are naturally recursive.

Every subtree is also a tree.

That means a problem on a tree can often be solved by asking the same question on the left child and right child.

Example: maximum depth.

```js
function maxDepth(root) {
  if (!root) return 0;

  const left = maxDepth(root.left);
  const right = maxDepth(root.right);

  return 1 + Math.max(left, right);
}
```

The meaning is:

The depth of this tree is 1 plus the larger depth of its children.

## DFS

DFS means Depth-First Search.

It goes deep before moving sideways.

There are three common DFS orders.

Preorder:

```txt
node -> left -> right
```

Inorder:

```txt
left -> node -> right
```

Postorder:

```txt
left -> right -> node
```

The order depends on when you process the current node.

## BFS

BFS means Breadth-First Search.

It visits nodes level by level.

BFS uses a queue.

```js
function levelOrder(root) {
  if (!root) return [];

  const result = [];
  const queue = [root];
  let head = 0;

  while (head < queue.length) {
    const node = queue[head++];
    result.push(node.value);

    if (node.left) queue.push(node.left);
    if (node.right) queue.push(node.right);
  }

  return result;
}
```

## Time and Space

Most tree traversal problems visit every node once.

So time complexity is `O(n)`.

DFS space depends on tree height.

For a balanced tree, height is `O(log n)`.

For a skewed tree, height is `O(n)`.

BFS space depends on the maximum width of the tree.

In the worst case, it can be `O(n)`.

## Common Tree Problem Types

Tree problems often ask for:

- Maximum depth.
- Minimum depth.
- Diameter.
- Path sum.
- Inverting a tree.
- Lowest common ancestor.
- Checking if two trees are the same.
- Level order traversal.
- Validating a BST.

## How to Think About Tree Recursion

Write what the function returns.

For example:

“This function returns the height of the current subtree.”

Or:

“This function returns true if the current subtree is valid.”

Then decide:

What should happen for `null`?

What should happen after getting answers from left and right?

This makes tree recursion much clearer.

## Common Mistakes

The first mistake is forgetting the null base case.

The second mistake is mixing up height and depth.

The third mistake is using global variables without understanding when they update.

The fourth mistake is assuming the tree is balanced.

The fifth mistake is using inorder logic on a tree that is not a BST.

## Interview Explanation

A strong explanation sounds like this:

“I use DFS because each subtree has the same structure as the full tree. The base case is a null node. Each node is visited once, so the time is O(n). The recursion stack is O(h), where h is the height of the tree.”

This is clear and complete.

## Final Understanding

Trees are recursive structures.

If you understand what your function returns for one node, you can solve the whole tree.

