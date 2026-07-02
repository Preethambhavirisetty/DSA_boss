# Strings

A string is a sequence of characters.

You can often think of a string like an array of characters.

That means many array patterns also work on strings: scanning, two pointers, sliding window, hashing, and dynamic programming.

But strings have extra details. They can include spaces, uppercase letters, lowercase letters, punctuation, and sometimes Unicode characters.

## The Core Idea

A string stores text in order.

```js
const s = "hello";
console.log(s[0]); // h
console.log(s[4]); // o
```

Reading a character by index is usually `O(1)`.

Scanning the whole string is `O(n)`.

```js
for (const ch of s) {
  console.log(ch);
}
```

Here, `n` is the length of the string.

## Strings Are Often Array Problems

Many string problems ask questions like:

- Does this string contain duplicate characters?
- Are two strings anagrams?
- Is this string a palindrome?
- What is the longest valid substring?
- How many times does each character appear?

These are similar to array questions.

The values are just characters instead of numbers.

## Substring vs Subsequence

This is a very important difference.

A substring is continuous.

In `"abcdef"`, `"bcd"` is a substring.

A subsequence does not need to be continuous, but order must stay the same.

In `"abcdef"`, `"ace"` is a subsequence.

Many mistakes happen because people mix these up.

If a problem says substring, think about sliding window.

If a problem says subsequence, think about two pointers or dynamic programming.

## Frequency Counting

Character counting is one of the most common string techniques.

Example: check if two strings are anagrams.

Two strings are anagrams if they have the same characters with the same counts.

```js
function isAnagram(a, b) {
  if (a.length !== b.length) return false;

  const count = new Map();

  for (const ch of a) {
    count.set(ch, (count.get(ch) ?? 0) + 1);
  }

  for (const ch of b) {
    if (!count.has(ch)) return false;

    count.set(ch, count.get(ch) - 1);

    if (count.get(ch) === 0) {
      count.delete(ch);
    }
  }

  return count.size === 0;
}
```

The time complexity is `O(n)`.

The space complexity is `O(k)`, where `k` is the number of unique characters.

If the alphabet is fixed, like only lowercase English letters, space can be treated as `O(1)`.

## Palindrome Thinking

A palindrome reads the same forward and backward.

Examples:

- `madam`
- `racecar`
- `abba`

The natural pattern is two pointers.

```js
function isPalindrome(s) {
  let left = 0;
  let right = s.length - 1;

  while (left < right) {
    if (s[left] !== s[right]) return false;

    left++;
    right--;
  }

  return true;
}
```

This is `O(n)` time and `O(1)` space.

The left pointer moves from the start.

The right pointer moves from the end.

If every pair matches, the string is a palindrome.

## Sliding Window on Strings

Sliding window is used when the problem asks for a substring.

Example: longest substring without repeating characters.

The window must always contain unique characters.

When a duplicate appears, move the left side until the window becomes valid again.

```js
function lengthOfLongestSubstring(s) {
  const seen = new Set();
  let left = 0;
  let best = 0;

  for (let right = 0; right < s.length; right++) {
    while (seen.has(s[right])) {
      seen.delete(s[left]);
      left++;
    }

    seen.add(s[right]);
    best = Math.max(best, right - left + 1);
  }

  return best;
}
```

The time complexity is `O(n)` because each character enters and leaves the set at most once.

The space complexity is `O(k)`.

## String Immutability

In many languages, strings are immutable.

Immutable means you cannot change the existing string directly.

When you appear to modify it, the language may create a new string.

This can make repeated string building expensive.

Example:

```js
let result = "";

for (const ch of chars) {
  result += ch;
}
```

For large inputs, this can be inefficient in some languages.

A safer pattern is to collect characters in an array and join at the end.

```js
const parts = [];

for (const ch of chars) {
  parts.push(ch);
}

const result = parts.join("");
```

## Common Patterns

Use a frequency map when you need counts.

Use two pointers when you compare both ends.

Use sliding window when you need a continuous valid substring.

Use sorting when character order does not matter and input size is reasonable.

Use dynamic programming when the problem asks about subsequences, edit distance, or longest common structure.

Use a trie when many words share prefixes.

## Common Mistakes

The first mistake is not clarifying case sensitivity.

Is `"A"` the same as `"a"`?

The second mistake is ignoring spaces and punctuation.

Should `"a man"` be treated differently from `"aman"`?

The third mistake is confusing substring with subsequence.

The fourth mistake is rebuilding strings in a loop without thinking about cost.

The fifth mistake is assuming every character is one simple byte. Unicode can be more complex in real systems.

## Interview Explanation

A strong string explanation sounds like this:

“I will scan the string once and keep character counts in a map. The map represents the current state of the window. When the window becomes invalid, I move the left pointer until it becomes valid again. Each character is added and removed at most once, so the time is O(n).”

This shows both the algorithm and the reason it is efficient.

## Final Understanding

Strings are not a separate world from arrays.

They are ordered sequences with character-specific rules.

If you understand scanning, counting, windows, pointers, and edge cases, most string problems become much easier.

