# Binary Search Tree

A Binary Search Tree, or BST, is a binary tree with an ordering rule.

For every node:

- Values in the left subtree are smaller.
- Values in the right subtree are larger.

This rule makes searching efficient when the tree is balanced.

## Basic Example

```txt
        8
      /   \
     3     10
    / \      \
   1   6      14
```

If you search for `6`, start at `8`.

`6` is smaller than `8`, so go left.

Now at `3`.

`6` is bigger than `3`, so go right.

Now at `6`.

Found.

You did not search the whole tree.

## Search Code

```js
function searchBST(root, target) {
  let current = root;

  while (current !== null) {
    if (current.value === target) {
      return current;
    }

    if (target < current.value) {
      current = current.left;
    } else {
      current = current.right;
    }
  }

  return null;
}
```

If the BST is balanced, time is `O(log n)`.

If the BST is skewed, time is `O(n)`.

Space is `O(1)` for this iterative version.

## Balance Matters

A balanced BST has height around `log n`.

A skewed BST can look like a linked list.

Balanced:

```txt
      4
    /   \
   2     6
```

Skewed:

```txt
1
 \
  2
   \
    3
     \
      4
```

In a skewed BST, search can become `O(n)`.

This is why real systems often use self-balancing trees.

Examples include AVL trees and Red-Black trees.

## Inorder Traversal

Inorder traversal of a BST gives sorted values.

Inorder means:

```txt
left -> node -> right
```

Code:

```js
function inorder(root, result = []) {
  if (!root) return result;

  inorder(root.left, result);
  result.push(root.value);
  inorder(root.right, result);

  return result;
}
```

This property is very important.

Many BST problems use it.

## Validating a BST

To validate a BST, every node must fit inside a valid range.

It is not enough to check only immediate children.

Example:

```txt
      10
     /  \
    5    15
        /  \
       6    20
```

The node `6` is in the right subtree of `10`, so it must be greater than `10`.

It is not.

So this is not a valid BST.

Correct validation uses ranges:

```js
function isValidBST(root, low = -Infinity, high = Infinity) {
  if (!root) return true;

  if (root.value <= low || root.value >= high) {
    return false;
  }

  return (
    isValidBST(root.left, low, root.value) &&
    isValidBST(root.right, root.value, high)
  );
}
```

## Insert

Insertion follows the search path.

If the value is smaller, go left.

If it is larger, go right.

When you find a null spot, insert there.

Balanced average time is `O(log n)`.

Worst-case time is `O(n)`.

## Delete

Deletion has three cases.

If the node is a leaf, remove it.

If the node has one child, replace it with that child.

If the node has two children, replace it with its inorder successor or predecessor.

The inorder successor is the smallest value in the right subtree.

Deletion is more complex because the BST rule must remain valid.

## Common Mistakes

The first mistake is assuming every binary tree is a BST.

It is not.

The second mistake is validating only parent-child relationships.

You must validate full ranges.

The third mistake is forgetting that balance affects complexity.

The fourth mistake is mishandling duplicate values.

Some BST definitions allow duplicates and some do not.

Always clarify.

## Interview Explanation

A strong explanation sounds like this:

“Because this is a BST, I can compare the target with the current node and choose only one side. If the tree is balanced, the height is O(log n), so search is O(log n). In the worst case, the tree can be skewed, making search O(n).”

This shows you understand both the rule and the limitation.

## Final Understanding

A BST is binary search in tree form.

The ordering rule is the entire point.

Use it to avoid searching both sides.

