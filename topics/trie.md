# Trie

A trie is a tree used for storing strings by prefix.

It is also called a prefix tree.

Each path from the root represents characters in a word.

Tries are useful when many words share prefixes.

## Basic Idea

Suppose you insert:

```txt
cat
car
care
dog
```

The words `cat`, `car`, and `care` share the prefix `ca`.

A trie stores that shared prefix only once.

The root is empty.

Each child represents a character.

## Trie Node

A trie node usually has:

- A map of children.
- A flag that says whether a word ends here.

```js
class TrieNode {
  constructor() {
    this.children = new Map();
    this.isWord = false;
  }
}
```

## Insert

To insert a word, walk character by character.

Create missing nodes as needed.

At the end, mark the last node as a word.

```js
class Trie {
  constructor() {
    this.root = new TrieNode();
  }

  insert(word) {
    let node = this.root;

    for (const ch of word) {
      if (!node.children.has(ch)) {
        node.children.set(ch, new TrieNode());
      }

      node = node.children.get(ch);
    }

    node.isWord = true;
  }
}
```

If the word length is `L`, insertion is `O(L)`.

## Search

To search a word, follow each character.

If a character is missing, the word does not exist.

At the end, check `isWord`.

```js
search(word) {
  let node = this.root;

  for (const ch of word) {
    if (!node.children.has(ch)) return false;
    node = node.children.get(ch);
  }

  return node.isWord;
}
```

This is `O(L)`.

## Prefix Search

Prefix search is where trie shines.

To check if any word starts with `pre`, follow the characters of `pre`.

If the path exists, the prefix exists.

You do not need to scan every word.

```js
startsWith(prefix) {
  let node = this.root;

  for (const ch of prefix) {
    if (!node.children.has(ch)) return false;
    node = node.children.get(ch);
  }

  return true;
}
```

## When To Use Trie

Use a trie when prefixes are central.

Examples:

- Autocomplete.
- Spell checking.
- Word dictionary.
- Search suggestions.
- Word search on a board.
- Longest common prefix.

## Trie vs Hash Set

A hash set can tell if a full word exists.

But it cannot naturally answer prefix questions efficiently.

If you need only exact word lookup, a set may be simpler.

If you need many prefix operations, a trie is better.

## Space Complexity

Trie space depends on the total number of characters inserted.

If you insert many words with shared prefixes, the trie saves space compared to storing paths separately.

But each node may store a map of children, so tries can still use a lot of memory.

Space is usually `O(total characters)`.

## Common Mistakes

The first mistake is forgetting the `isWord` flag.

Without it, you cannot distinguish a prefix from a complete word.

For example, if `care` exists, does `car` exist?

Only if the node for `r` is marked as a word.

The second mistake is using a trie when a hash set is enough.

The third mistake is not considering memory cost.

The fourth mistake is assuming every node has all 26 children. A map is often cleaner unless the alphabet is fixed.

## Interview Explanation

A strong explanation sounds like this:

“I use a trie because the problem depends on prefixes. Each level represents one character. Insert and search take O(L), where L is the word length. The space is O(total characters stored), but shared prefixes reduce repeated storage.”

This shows the reason behind the data structure.

## Final Understanding

A trie is a tree of characters.

It is built for prefix-based thinking.

If the problem asks about starts-with, autocomplete, or word paths, consider a trie.
