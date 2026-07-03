# Dynamic Programming

Dynamic programming intimidates people more than any other topic, but it rests on one simple idea. Once that idea clicks, this entire list becomes variations on a theme.

## The core idea of dynamic programming

Dynamic programming applies when a big problem can be broken into smaller versions of itself, and those smaller versions repeat. The insight is this: if you're going to need the answer to the same small sub-problem many times, solve it once and remember the answer instead of recomputing it. That's the whole trick — trade a little memory to avoid mountains of repeated work.

There are two ways to organize this. Memoization (top-down) starts from the big problem and recurses downward, but caches each answer the first time it's computed so repeat requests are instant. Tabulation (bottom-up) works the other direction, solving the smallest pieces first and building up to the big answer by filling in a table. Same idea, opposite directions.

The real skill isn't the caching mechanics — it's spotting two things in a problem: what the sub-problem is (usually captured by a small set of numbers called the "state"), and how a bigger sub-problem is built from smaller ones (the "transition"). Every problem below is just a different answer to those two questions.

## Classic 1D/2D

## Fibonacci (memo / tabulation)

The gateway example. Each Fibonacci number is the sum of the two before it. Computed naively by recursion, you end up recalculating the same values a staggering number of times. DP fixes this by remembering each value once computed, turning an explosively slow process into a quick walk up the sequence. It's the cleanest demonstration of "solve once, reuse" and every DP student starts here.

## Climbing Stairs

You're climbing stairs, taking one or two steps at a time, and you want to count how many distinct ways you can reach the top. The realization is that the number of ways to reach a step equals the ways to reach the step below it (then step up one) plus the ways to reach two steps below (then step up two). That's Fibonacci in disguise — the same add-the-two-previous structure. It's the first problem that shows DP is about recognizing a hidden recurring structure, not memorizing formulas.

## House Robber

You're robbing houses along a street, but you can't rob two adjacent houses (alarms would trigger). You want the maximum loot. At each house you face a choice: rob it (and skip the previous one) or skip it (and keep whatever the previous choice yielded). The best total at each house is the better of those two options. This introduces the crucial DP pattern of making a choice at each step and carrying forward the best result — the foundation for most harder DP problems.

## Coin Change (min coins / count ways)

Two related problems. Minimum coins: what's the fewest coins to make a target amount? For each amount, you consider using each coin and take whichever leads to the fewest coins overall. Counting ways: how many distinct combinations make the target? Here you accumulate counts rather than minimize. This is where DP proves its worth over greedy — recall that greedy coin change fails for odd coin systems, but DP always gets it right because it genuinely considers every possibility rather than gambling on the biggest coin.

## 0/1 Knapsack

The most important DP problem to understand deeply, because dozens of others are disguised versions of it. You have a weight-limited bag and items each with a weight and value; you want maximum value, and each item is either taken whole or left (no fractions — that's the "0/1"). For each item you decide include-or-exclude, and the best value depends on both which items you've considered and how much capacity remains. That two-dimensional state — items considered and remaining capacity — is the mental model that unlocks a huge family of problems.

## Unbounded Knapsack

Same as 0/1 knapsack, but now you can use each item unlimited times. The one change: after choosing an item, you don't move past it — you can immediately consider it again. This tiny distinction (can you reuse an item or not?) is a recurring DP theme, and coin-change-count-ways is actually an unbounded knapsack in disguise. Recognizing when reuse is allowed versus forbidden is a core DP skill.

## Subset Sum / Partition Equal Subset

Subset sum asks: can any group of the given numbers add up to exactly a target? Partition equal subset asks: can the numbers be split into two groups with equal totals (which is just subset sum targeting half the total). Both are 0/1 knapsack stripped down to a yes/no question — for each number you decide include-or-exclude, tracking which sums are achievable. Seeing that these are knapsack in disguise is exactly the pattern-recognition DP rewards.

## Rod Cutting

You have a rod of some length and a price for each possible cut-length, and you want to cut it to maximize total sale value. For each possible first cut, you take its price plus the best value obtainable from the remaining piece — and you try all first cuts, keeping the best. It's structurally an unbounded knapsack (you can reuse cut-lengths), and it's a classic for building intuition about breaking a problem into "first choice plus best solution to what remains."

## Matrix Chain Multiplication

You have a sequence of matrices to multiply, and while the final result is the same regardless of grouping, the amount of work depends heavily on the order you multiply them. You want the cheapest order. The DP considers every possible place to make the "last" multiplication, splitting the chain into two parts, solving each part optimally, and combining. This introduces interval DP — where the state is a range (a start and end), and you try every split point within it. It's a step up in sophistication and a gateway to the harder interval problems later.

## Sequences / Strings

## Longest Common Subsequence

Find the longest sequence of characters appearing in both strings in the same order (skipping allowed, but order preserved). You compare the strings position by position: when characters match, you extend the common sequence; when they don't, you take the better of skipping a character from either string. The two-dimensional table comparing prefixes of both strings is the template for nearly every two-string DP problem — learn this one and edit distance, distinct subsequences, and more come almost for free.

## Longest Increasing Subsequence

Find the longest run of values that keep increasing (not necessarily adjacent). The straightforward DP checks, for each element, all earlier smaller elements to extend the best sequence ending there. But there's a famously clever speedup that combines DP with binary search: you maintain a growing list representing the smallest possible "tail" value for increasing sequences of each length, and binary-search to place each new number. This variant is worth knowing specifically because it shows DP can be accelerated by pairing it with other techniques.

## Edit Distance

The minimum number of single-character insertions, deletions, or substitutions to turn one string into another. At each position you consider each edit operation and take the cheapest path. It's the same two-string grid structure as LCS, and it powers spell-checkers and "did you mean?" It's one of the most practically important DP problems and a rite of passage for the two-dimensional table pattern.

## Longest Palindromic Subsequence

Find the longest subsequence of a string that reads the same forwards and backwards. The elegant trick: this is just the longest common subsequence between the string and its reverse. Alternatively, it's an interval DP where you expand a range from both ends, matching outer characters. It reinforces how a fresh problem often reduces to one you already know, which is a huge part of DP fluency.

## Distinct Subsequences

Count how many ways one string appears as a subsequence within another. For each character pairing you decide whether to "use" a match or skip it, accumulating counts. It's a counting variant of the LCS grid, and it sharpens the distinction between optimizing (find the best) and counting (find how many) — the table structure is nearly identical but the operation inside changes from "take the max" to "add up the ways."

## Wildcard / Regex Matching

Determine whether a string matches a pattern containing special symbols — wildcards that stand for any characters, or symbols meaning "zero or more of the preceding." The DP tracks whether each prefix of the string matches each prefix of the pattern, with special handling where the pattern's symbols branch into multiple possibilities (like "match nothing" versus "match one more"). It's a two-dimensional matching table with trickier transitions, and it's a favorite hard interview problem because getting the special-symbol cases right requires real care.

## Grids & Intervals

## Unique Paths / Min Path Sum

On a grid where you can only move right or down, unique paths counts how many distinct routes reach the bottom corner, and min path sum finds the route with the smallest total when each cell has a cost. Both work the same way: the value at each cell derives from the cell above and the cell to its left (the only two ways to arrive). These are the friendliest grid DP problems and build intuition for thinking of a table cell's answer as coming from its neighbors.

## Burst Balloons

You have a row of balloons each worth points, and bursting one earns points based on it and its current neighbors; you want the maximum total, and the catch is that bursting changes who's adjacent to whom. The brilliant reframing is to think about which balloon you burst last in a range — because that one's neighbors are fixed (the range boundaries), which untangles the dependency. It's a hard interval DP, prized because the key insight (reason about the last action, not the first) is counterintuitive and genuinely clever.

## Palindrome Partitioning II

Given a string, find the minimum number of cuts needed so every resulting piece is a palindrome. The DP tracks the fewest cuts for each prefix, considering each spot where the final piece could be a palindrome. It combines palindrome-checking with a minimization over cut positions, blending string DP with interval reasoning — a solid example of layering two DP ideas together.

## Advanced DP Paradigms

These are the heavier tools — mostly competitive-programming territory. Knowing what they are and when they apply matters more than mastering every detail.

## Bitmask DP (Traveling Salesman Problem)

When a problem's state involves tracking which subset of items you've handled — like which cities a salesman has visited — you can encode that subset compactly as a pattern of on/off bits. The travelling salesman problem (visit every city once with minimum travel) is the canonical example: the state is "which cities visited so far, and where am I now," and the bitmask represents the visited set. It's used when the number of items is small (because subsets grow explosively) but you genuinely need to remember the exact combination handled.

## Digit DP

For counting problems over huge ranges of numbers — like "how many numbers up to a trillion have some digit property" — you can't check each number individually. Digit DP builds numbers digit by digit, tracking just enough state (like whether you're still bounded by the limit and whatever property you're counting) to count vast quantities without enumeration. It's the specialized tool for "count numbers with property X in a giant range" problems.

## DP on Trees

When the underlying structure is a tree, sub-problems naturally correspond to subtrees. You compute an answer for each node based on the answers of its children, working from the leaves upward. Problems like "find the largest independent set in a tree" or "the tree's diameter" use this. It's the adaptation of DP from linear sequences to branching tree structures, and it's extremely common once trees enter the picture.

## DP with Convex Hull Trick

Some DP problems have a transition that, written out, looks like choosing the best among many linear options at each step — and doing that naively is slow. The convex hull trick is an optimization that organizes those linear options cleverly so you can find the best one quickly, dramatically speeding up the DP. It's an optimization layer applied on top of an already-correct DP whose transition has a specific mathematical shape.

## Knuth Optimization

Another speedup for a particular class of interval DP problems (like some ordering or grouping problems). When the optimal split point behaves "monotonically" — meaning the best place to split a larger range never sits earlier than the best split for a smaller range inside it — you can exploit that to avoid checking every split point, cutting the work substantially. Like the convex hull trick, it's a specialized accelerator for DPs with a specific structure.

## Divide and Conquer DP Optimization

A related speedup, also relying on the optimal split point moving monotonically as you scan across sub-problems. It uses a divide-and-conquer approach to compute a whole row of DP answers faster than doing each independently. It's another tool in the "my DP is correct but too slow, and it has this special monotonic property" toolkit.

## SOS DP (Sum over Subsets)

A technique for efficiently computing, for every possible subset, some aggregate (like a sum) over all of its subsets. Done naively this is enormously expensive, but SOS DP computes them all together by processing one bit at a time, sharing work across subsets. It's a niche but powerful tool for problems involving aggregating information across all subset relationships.

Two things to carry away. First, the entire "classic" and "sequences" sections are really a handful of templates — the 1D choice pattern (house robber), the knapsack two-dimensional state, the two-string grid (LCS), and interval DP (matrix chain) — and most named problems are one of these in a costume. Learn to recognize the template and half the battle is over. Second, the advanced paradigms split cleanly into two kinds: those that change what the state is (bitmask for subsets, digit DP for number-building, tree DP for branching) and those that merely speed up an already-correct DP with a special mathematical property (convex hull trick, Knuth, D&C optimization, SOS). Knowing which kind you're facing tells you whether you're solving a new problem or optimizing an old one.
