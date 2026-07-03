# Recursion & Backtracking

Before the individual problems, it helps to understand the one idea they all share, because backtracking is really a single strategy applied to many puzzles.

## The core idea of backtracking

Backtracking is organized trial-and-error. You build a solution one piece at a time, and after placing each piece you ask, "is this still potentially valid?" If yes, you go deeper and try the next piece. If a choice leads to a dead end, you undo it — step back, erase that choice, and try a different option. Picture exploring a maze while unrolling a string behind you: whenever you hit a wall, you follow the string back to the last fork and take a different path. The "undoing" is the backtrack, and the reason it's efficient is that the moment a partial attempt becomes hopeless, you abandon that entire branch rather than blindly exploring everything beneath it. That early abandonment — called pruning — is what saves you from checking an astronomical number of dead-end combinations.

With that lens, here are the classic problems.

## N-Queens

You place chess queens on a board so that none can attack another — meaning no two share a row, column, or diagonal. You go column by column, trying to place a queen in each. After placing one, you check it doesn't clash with any already placed; if it's safe, you move to the next column. If you reach a column where no row is safe, you know an earlier placement was a mistake, so you back up and shift the previous queen to a different row. It's the textbook backtracking example because the "is this safe?" check lets you abandon bad arrangements immediately instead of placing all queens and only then discovering a conflict.

## Sudoku Solver

You fill empty cells with digits so that every row, column, and box follows the rules. You find an empty cell and try digits in it one at a time. For each digit, you check whether it conflicts with the row, column, or box; if it's legal, you tentatively place it and move on to the next empty cell. If you ever reach a cell where no digit works, that means an earlier guess was wrong, so you erase and reconsider previous cells. It's essentially the same machinery as N-Queens — try, validate, recurse, undo — applied to a different set of rules.

## Rat in a Maze / Maze Path

You have a grid with walls and open paths, and you want to move a "rat" from the start corner to the goal. From your current cell you try moving in each direction. If a move lands on an open, unvisited cell, you step there and continue exploring from the new position. If you get boxed in with nowhere new to go, you retreat to the previous cell and try a different direction. Marking cells as visited (and unmarking them when you backtrack) prevents you from walking in circles. It's a gentle introduction to backtracking because the "explore, hit a wall, back up" story maps so directly onto how the algorithm works.

## Word Search

You're given a grid of letters and a target word, and you must find whether the word can be traced through connected neighboring cells. You look for a cell matching the word's first letter, then try to extend the trail to adjacent cells matching each subsequent letter. If a path stops matching, you back up and try a different neighbor, or a different starting cell. Like the maze, you mark cells as used while on a path and release them when backtracking, so the same letter isn't reused within one attempt. It's the maze idea with the added twist that your path must spell something specific.

## Permutations / Combinations / Subsets Generation

These three are about systematically producing all the ways to arrange or select items, and they're the foundation many other backtracking problems build on.

Permutations are all possible orderings of a set — you build an arrangement by picking an unused item for each position, recursing, then undoing that pick to try another. Combinations are all ways to choose a group of a certain size where order doesn't matter — you decide, for each item, whether to include it, moving forward so you never reconsider earlier items and thus avoid duplicates. Subsets are all possible selections of any size — for each item you branch two ways: include it or skip it, and every complete set of such decisions is one subset. All three follow the same choose-recurse-unchoose rhythm; they differ only in what counts as a valid choice at each step.

## Combination Sum

You're given numbers and a target, and you want all the groups of numbers that add up to that target (often allowed to reuse numbers). You build a group by adding numbers one at a time, tracking the running total. If the total hits the target exactly, you've found a valid group. If it overshoots, that path is hopeless, so you back off and try something else. This overshoot check is a natural pruning point — the instant you exceed the target, there's no reason to keep adding, so you abandon that branch. It's a combinations problem with an arithmetic goal steering which branches are worth exploring.

## Palindrome Partitioning

You want to slice a string into pieces where every piece reads the same forwards and backwards. You try cutting after the first character, checking whether that first piece is a palindrome; if it is, you recurse on the remaining part to slice it up too. You try cuts of different lengths at each step. When a proposed piece isn't a palindrome, you skip that cut and try a longer or shorter one. It's backtracking over where to place the cuts, with the palindrome check acting as the validity test that prunes bad slicing choices early.

Graph Coloring (backtracking)

You want to color the nodes of a network so that no two directly connected nodes share a color, using a limited palette. You go node by node, trying each available color. Before committing a color to a node, you check that none of its already-colored neighbors use it. If a node can't be colored with any available color, you know an earlier coloring boxed you in, so you back up and recolor a previous node. This models real conflicts — scheduling exams so overlapping classes don't clash, or assigning radio frequencies so nearby towers don't interfere — where "connected" means "can't be the same."

## Hamiltonian Path / Cycle

You want to find a route through a network that visits every node exactly once (a Hamiltonian path), and if it also returns to the start, that's a cycle. You build the route by extending it one node at a time, only moving to a connected, not-yet-visited node. If you reach a point where you're stuck before covering everyone, you back up and try a different next node from an earlier point. This is a famously hard problem with no known fast shortcut, so backtracking — with aggressive pruning of doomed partial routes — is a standard way to attack it.

## Knight's Tour

You place a chess knight on a board and try to move it so it lands on every square exactly once, using only the knight's L-shaped moves. From the current square you try each legal knight move to an unvisited square, mark it, and continue; if you get stranded with squares still unvisited, you back up and try a different move earlier in the tour. It's a pure, self-contained backtracking puzzle, and it's often used to introduce a smart pruning heuristic (preferring moves that head toward the most constrained squares first), which dramatically speeds up finding a full tour.

The pattern to internalize: every one of these is choose a move, check if it's still viable, recurse deeper, and undo if it fails. What changes from problem to problem is only two things — what counts as a "choice" and what counts as "still viable." Master those two questions and all ten collapse into the same template. The real skill in interviews is spotting the pruning opportunities — the earlier you can detect and abandon a doomed branch, the faster your solution runs.
