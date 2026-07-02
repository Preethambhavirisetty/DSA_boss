# Hashing

Hashing is a way to store and find data quickly.

In DSA, hashing usually means using a hash map or hash set.

A hash set answers:

“Have I seen this value before?”

A hash map answers:

“What value is connected to this key?”

## Hash Set

A set stores unique values.

```js
const seen = new Set();

seen.add(10);
seen.add(20);

console.log(seen.has(10)); // true
```

Lookup is usually `O(1)` average.

That means it is very fast for membership checks.

## Hash Map

A map stores key-value pairs.

```js
const count = new Map();

count.set("a", 3);
console.log(count.get("a")); // 3
```

Maps are useful when one piece of data should point to another piece of data.

Examples:

- Number to index.
- Character to frequency.
- Node to parent.
- State to answer.
- Word to list of words.

## Why Hashing Is Powerful

Hashing removes repeated searching.

Example: checking duplicates.

Brute force compares every pair.

That is `O(n^2)`.

With a set:

```js
function hasDuplicate(nums) {
  const seen = new Set();

  for (const num of nums) {
    if (seen.has(num)) {
      return true;
    }

    seen.add(num);
  }

  return false;
}
```

Each number is checked once.

Time complexity is `O(n)`.

Space complexity is `O(n)`.

The set remembers what we have seen.

## Two Sum

Two Sum is the classic hashing problem.

For each number, ask:

What number do I need to complete the target?

```js
function twoSum(nums, target) {
  const seen = new Map();

  for (let i = 0; i < nums.length; i++) {
    const need = target - nums[i];

    if (seen.has(need)) {
      return [seen.get(need), i];
    }

    seen.set(nums[i], i);
  }

  return [];
}
```

The map stores previous numbers and their indexes.

This avoids checking every pair.

Time is `O(n)`.

Space is `O(n)`.

## Frequency Counting

Hash maps are commonly used for counts.

```js
function countChars(s) {
  const count = new Map();

  for (const ch of s) {
    count.set(ch, (count.get(ch) ?? 0) + 1);
  }

  return count;
}
```

This pattern appears in anagrams, substring problems, grouping, and majority element problems.

## Grouping

Hash maps can group items by a computed key.

For example, group anagrams by sorted letters.

```js
function groupAnagrams(words) {
  const groups = new Map();

  for (const word of words) {
    const key = word.split("").sort().join("");

    if (!groups.has(key)) {
      groups.set(key, []);
    }

    groups.get(key).push(word);
  }

  return [...groups.values()];
}
```

The key represents the group.

All words with the same sorted letters belong together.

## Hashing in Graphs

In graph traversal, a visited set prevents repeated work and infinite loops.

```js
const visited = new Set();
```

Without this, a cycle can make traversal run forever.

Hashing is not only for arrays.

It is a general memory tool.

## Hashing in Dynamic Programming

Memoization often uses a hash map.

The key is the state.

The value is the answer for that state.

```js
const memo = new Map();
```

This avoids solving the same subproblem repeatedly.

## Complexity

Hash map and hash set operations are usually:

- Insert: `O(1)` average.
- Lookup: `O(1)` average.
- Delete: `O(1)` average.

Worst case can be worse if many keys collide.

But for normal interview reasoning, average `O(1)` is commonly accepted.

## Common Mistakes

The first mistake is forgetting the space cost.

Hashing often improves time by using more memory.

The second mistake is using the wrong key.

For objects or arrays, two values that look equal may not be the same key unless converted carefully.

The third mistake is overwriting useful information.

For example, in Two Sum, think about whether you need first index, latest index, or all indexes.

The fourth mistake is assuming hash maps preserve sorted order.

They do not give sorted behavior.

## Interview Explanation

A strong explanation sounds like this:

“The brute force solution repeats search work. I use a hash map to remember useful previous values. Then each lookup is O(1) average, so the full scan is O(n). The tradeoff is O(n) extra space.”

This is exactly the kind of reasoning interviewers want.

## Final Understanding

Hashing is memory used intelligently.

It remembers information so you do not search again.

When a problem asks “have I seen this?” or “how many times did this appear?”, think about hashing.

