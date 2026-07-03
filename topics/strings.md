# String Algorithms

String algorithms mostly revolve around one recurring challenge: finding patterns inside text efficiently, and doing it without re-checking the same characters over and over. Here they are in plain language.

## Pattern Matching

## Naive Pattern Matching

The obvious approach. You want to find a small pattern inside a big text, so you line the pattern up at the very start of the text and check character by character. If it doesn't match, you slide the pattern one position to the right and try again from scratch. You keep sliding one step at a time until you find a match or run off the end. It works, but it's wasteful: every time it fails, it throws away everything it just learned and restarts the comparison from the beginning. All the smarter algorithms below exist to avoid that waste.

KMP (Knuth-Morris-Pratt)

KMP's insight is that when a match fails partway through, you've actually learned something useful about the characters you just checked — so you shouldn't restart from scratch. Before searching, it studies the pattern itself to find internal repetition: places where the beginning of the pattern reappears later inside it. Using that knowledge, when a mismatch happens deep into a partial match, it knows exactly how far it can safely jump the pattern forward without missing anything, instead of sliding just one step. The result is that the text is scanned in a single clean pass, never backtracking.

## Rabin-Karp

This one uses a clever shortcut: instead of comparing whole strings character by character, it turns each chunk of text into a single number (a "hash," like a fingerprint) and compares numbers, which is instant. It computes the fingerprint of the pattern once, then slides a window across the text computing each window's fingerprint. The magic is the rolling part — when the window slides one step, it doesn't recompute the whole fingerprint; it cheaply adjusts the old one by removing the character that left and adding the one that entered. When two fingerprints match, it does a quick character check to confirm (since different strings can occasionally share a fingerprint). It shines especially when searching for many patterns at once, since comparing fingerprints is so cheap.

## Z-Algorithm

This builds a helpful table for a string where each position records: "starting here, how long is the stretch that matches the very beginning of the string?" That table, computed in a single efficient pass, captures all the internal self-similarity of the string. To search for a pattern in text, you glue the pattern and text together with a separator in between, build this table for the combined string, and then any position whose value equals the pattern's length marks a full match. It's a clean, elegant way to get the same power as KMP through a slightly different lens.

## Boyer-Moore

This is the algorithm many real text editors use for "find," and its cleverness is that it matches the pattern from right to left while still sliding it left to right. Because it checks the end of the pattern first, a mismatch there often lets it skip forward by big jumps rather than one step. It uses rules based on which character caused the mismatch and on repeated sections within the pattern to decide how far to leap ahead. On typical text with a large alphabet, it can actually skip over huge portions without even looking at them, making it one of the fastest in practice — the longer the pattern, the bigger the skips.

## Aho-Corasick

This is built for searching for many patterns at the same time in one pass through the text — imagine scanning a document for thousands of banned words at once. It first arranges all the patterns into a shared tree structure so common prefixes are merged. Then it adds "failure links" — shortcuts that, when a partial match breaks, jump you to the longest other pattern-prefix you've still got going, so you never lose progress. As you feed the text through once, it simultaneously tracks progress on every pattern. It's essentially KMP's failure-jump idea generalized from one pattern to a whole dictionary of them.

## Finite Automaton Matching

This treats pattern matching like a machine with a set of states, where each state means "I've matched this many characters of the pattern so far." You precompute, for every state and every possible next character, which state to move to. Then searching the text is stunningly simple: you feed characters in one at a time, hopping from state to state according to the precomputed table, and whenever you land in the final "fully matched" state, you've found the pattern. The searching itself is extremely fast and never backtracks; the cost is building that state-transition table upfront, which can be large for big alphabets.

## String Structures / Processing

Trie (Prefix Tree)

A trie is a tree that stores a collection of words by their shared beginnings. Picture branching paths where each step down represents one character, so words sharing a prefix share the same early path before splitting off. This makes it wonderfully efficient for prefix-based questions — "which words start with 'app'?" — which is exactly why it powers autocomplete and spell-checkers. Instead of scanning through a whole word list, you just walk down the tree following your prefix. The trade-off is that it can use a lot of memory since it stores structure for every character position.

## Suffix Array

A suffix is just the tail end of a string starting from some position — a word has as many suffixes as it has characters. A suffix array is a list of all those suffixes sorted into alphabetical order (stored compactly as their starting positions, not the full strings). Once you have this sorted list, searching for any pattern becomes a binary search over the suffixes, since a pattern appearing in the text must be the beginning of some suffix. It packs a lot of searching power into a compact structure, and it's a favorite because it's more memory-friendly than the suffix tree below while offering much of the same capability.

Suffix Tree (Ukkonen's)

This is a tree that compactly stores every suffix of a string, with shared beginnings merged into common branches. Because it contains all suffixes, an enormous range of questions become fast: finding a pattern, counting how many times it appears, finding the longest repeated section, and more — often nearly instantly once the tree is built. Its reputation is for being powerful but tricky to construct; Ukkonen's refers to the famous method that builds it efficiently in a single pass rather than the slow obvious way. It's the heavyweight tool of string processing — very capable, but memory-hungry and intricate.

## Suffix Automaton

This is the most compact of the suffix structures — a small state machine that recognizes every substring of a string. Despite being tiny (its size stays modest even for long strings), it can answer questions like how many distinct substrings exist, or whether some string appears as a substring, very efficiently. Think of it as achieving much of the suffix tree's power while being smaller and, to many, more elegant — though it's abstract enough that it takes some effort to build intuition for.

## Manacher's Algorithm

This finds the longest palindrome (a stretch that reads the same forwards and backwards) inside a string, and it does so in a single efficient pass. The naive way would try expanding outward from every position to see how far a palindrome stretches, which repeats a lot of work. Manacher's cleverness is reusing information: because palindromes are symmetric, once you've measured palindromes on the left side of a big palindrome, you can mirror those measurements to the right side and skip redundant checking. That mirroring trick is what collapses the work down to a single sweep.

Longest Common Prefix (LCP array)

This is a companion to the suffix array. Once suffixes are sorted alphabetically, neighboring suffixes in that sorted order tend to share leading characters. The LCP array records, for each pair of adjacent sorted suffixes, how many starting characters they have in common. This seemingly small piece of information unlocks a lot — it lets you count distinct substrings, find longest repeated stretches, and speed up searches — making the suffix array far more powerful than it is alone.

Edit Distance (Levenshtein)

This measures how different two strings are by counting the minimum number of single-character edits — insertions, deletions, or substitutions — needed to transform one into the other. "Kitten" into "sitting" takes a few such edits, and that count is the distance. It's computed by building a grid that compares the strings piece by piece, where each cell represents the best way to match up a prefix of one string with a prefix of the other. This is the engine behind spell-check suggestions, "did you mean?" features, and DNA comparison — anywhere you need to quantify similarity between sequences.

## Longest Common Subsequence

This finds the longest sequence of characters that appears in both strings in the same relative order, though not necessarily next to each other. In "ABCBDAB" and "BDCAB," you can pick out characters common to both while preserving order. The key distinction is "subsequence" versus "substring" — you're allowed to skip characters, as long as the ones you keep stay in order. It's solved with a similar grid-filling approach to edit distance, and it underpins tools like file-comparison utilities that highlight what changed between two versions of a document.

## Rolling Hash / Polynomial Hashing

This is the fingerprinting technique that makes Rabin-Karp and many other algorithms fast. It converts a string into a single number by treating its characters like digits in a number system — combining them with position-based weights so that the string's identity is captured in one value. The rolling property is the crucial part: when you shift your view one character over, you can update the fingerprint cheaply — subtract the departing character's contribution, shift, and add the new one — rather than recomputing from scratch. This makes comparing substrings almost instant, which is why it appears throughout string algorithms as a foundational trick.

## Burrows-Wheeler Transform

This is a reversible rearrangement of a string that shuffles its characters into an order that tends to group identical characters together. That grouping doesn't lose any information — you can perfectly reverse it back to the original — but it makes the data far more compressible, which is why it sits at the heart of compression tools like bzip2. The same transform also turns out to be extremely useful for fast searching in huge texts, which is why it became central to modern DNA-sequencing tools that must locate patterns within enormous genomes. It's a rare case of a technique that serves both compression and search elegantly.

The throughline across pattern matching is avoiding re-examination — KMP, Z, and automata precompute the pattern's structure so they never backtrack; Boyer-Moore skips ahead; Rabin-Karp compares cheap fingerprints. And across the structures, the theme is preprocessing the text once (into a trie, suffix array, or suffix tree) so that afterward, unlimited searches become cheap.
