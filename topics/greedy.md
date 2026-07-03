# Greedy Algorithms

Greedy is the simplest strategy to state and the trickiest to trust, so it's worth understanding the shared idea and its one big catch before the individual problems.

## The core idea of greedy algorithms

A greedy algorithm builds a solution by always grabbing whatever looks best right now, without worrying about the future and never reconsidering past choices. At each step it makes the locally optimal move and commits to it. This is fast and simple — but there's a catch that defines the entire topic: grabbing the best-looking option at each moment doesn't always lead to the best overall result. Sometimes a slightly worse choice now would enable a much better outcome later, and greedy blindly misses it. So the real intellectual work in greedy algorithms isn't the algorithm itself — it's proving that being greedy actually works for your specific problem. The problems below are the famous cases where it provably does (and one where it only works under special conditions).

## Activity Selection

You have many activities with start and end times, and you want to attend as many as possible without overlaps. The greedy insight is to always pick the activity that finishes earliest among those still available. Why finishing earliest? Because ending sooner leaves the most room for future activities. You sort by finish time, take the first, then take the next one that starts after it ends, and repeat. It feels almost too simple, but it provably yields the maximum number of activities — a clean example of a greedy choice that happens to be optimal.

## Fractional Knapsack

You have a bag with limited weight capacity and various items, each with a value and a weight, and — crucially — you're allowed to take fractions of items. The greedy move is to rank items by their value-per-unit-weight (their "bang for the buck") and greedily fill the bag with the most valuable-per-weight items first, taking a partial amount of the last one when the bag is nearly full. Because you can split items, greedy is provably optimal here. This is worth contrasting with the regular (0/1) knapsack where items can't be split — there, greedy fails, and you need dynamic programming instead. The ability to take fractions is exactly what makes greedy valid.

## Huffman Coding

This is a compression technique that assigns shorter codes to frequently-used characters and longer codes to rare ones, minimizing total size. The greedy strategy repeatedly takes the two least frequent items and merges them into a combined unit, treating that merged unit's frequency as the sum of its parts, and continues merging until everything's joined into one tree. Characters that got merged early (the rare ones) end up deep in the tree with long codes; frequent characters stay near the top with short codes. Always merging the two smallest is the greedy choice, and it provably produces the most efficient possible code. It's the classic greedy success story used in real compression formats.

## Job Sequencing with Deadlines

You have jobs, each with a deadline and a profit, and each takes one time slot. You want to schedule jobs to maximize total profit, but a job only pays if finished by its deadline. The greedy approach sorts jobs by profit, highest first, and tries to schedule each as late as possible before its deadline (to keep earlier slots free for other jobs). If a job's slot is taken, you look for an earlier open slot; if none exists, you skip it. Grabbing the most profitable jobs first, while cleverly placing them late, is what makes this greedy strategy work.

## Minimum Spanning Tree (Prim's, Kruskal's)

You have a network of nodes connected by weighted links, and you want to connect them all together as cheaply as possible without any redundant loops. Two greedy algorithms solve this.

Kruskal's looks at all the links sorted from cheapest to most expensive and greedily adds each one, unless adding it would form a loop (which would be redundant). It grows the network as scattered pieces that gradually join up. Prim's instead grows a single connected blob outward from a starting node, always greedily adding the cheapest link that extends the blob to a new node. Both make locally cheapest choices and both provably reach the globally cheapest connection — a rare case where two different greedy strategies solve the same problem.

## Dijkstra's (greedy shortest path)

You want the shortest route from a starting point to all other points in a network with weighted links. Dijkstra's works greedily by always finalizing the closest not-yet-finalized node next — once you've confirmed the shortest distance to the nearest unfinalized node, you use it to potentially improve the distances to its neighbors. The greedy leap of faith is that the nearest unfinalized node's distance is already final and can't be improved later, which holds true as long as all link weights are non-negative. That non-negativity condition is essential — if links can have negative weights, this greedy assumption breaks and you need a different algorithm.

## Interval Scheduling / Interval Partitioning

These are two related greedy problems. Interval scheduling is essentially activity selection — picking the most non-overlapping intervals by always choosing the one that ends earliest. Interval partitioning asks a different question: given overlapping intervals, what's the minimum number of "resources" (like rooms) needed so no two overlapping intervals share one? The greedy approach here sorts intervals by start time and assigns each to any free resource, opening a new resource only when all existing ones are busy. The number of resources you end up needing equals the maximum number of intervals overlapping at any single moment — and greedily reusing freed resources achieves exactly that minimum.

## Gas Station

You're driving a circular route with gas stations, each offering some fuel and each stretch costing some fuel, and you want to find a starting station from which you can complete the full loop. The greedy insight is elegant: track your running fuel balance as you go, and if it ever drops below zero, none of the stations from your current start up to that failure point could work as a starting point — so you greedily jump your candidate start to the next station and reset. A single pass finds the answer. The cleverness is realizing you can eliminate a whole batch of impossible starting points at once rather than testing each individually.

## Jump Game

You have a list of numbers where each tells you the maximum distance you can jump forward from that spot, and you want to know if you can reach the end. The greedy approach tracks the farthest position reachable so far as you move along. At each step you update this reach, and as long as your current position stays within the reachable range, you can keep going. If your reach ever falls behind your position, you're stuck. Rather than exploring every possible sequence of jumps, greedily tracking the single farthest reachable point tells you everything you need.

## Coin Change (greedy — canonical systems only)

You want to make a target amount using the fewest coins. The greedy instinct is to always grab the largest coin that fits, then repeat. For everyday currencies like US coins, this greedy approach happens to give the fewest coins — but this is not universally true. For some oddball sets of coin values, greedily grabbing the biggest coin actually uses more coins than necessary, and you'd need dynamic programming to get it right. This is the cautionary tale of greedy algorithms: it looks obviously correct, works for the coins you're used to, and yet fails on other coin systems — which is why the label specifically says "canonical systems only." It's the go-to example for why you must prove greedy works rather than assume it.

## Egyptian Fraction

This represents a fraction as a sum of distinct unit fractions (fractions with 1 on top, like 1/2 + 1/3). The greedy method repeatedly subtracts off the largest possible unit fraction that fits within what remains, then continues with the leftover. Remarkably, this greedy process always terminates and represents any proper fraction this way. It's more of a mathematical curiosity than a practical tool, but it's a satisfying example of a greedy strategy that provably always succeeds in fully expressing the fraction.

The two lessons to carry away. First, greedy is seductive precisely because it's simple and often looks right — but "looks right" isn't proof, and the coin-change and fractional-vs-0/1 knapsack examples show how the same greedy instinct succeeds in one setting and fails in a nearly identical one. Second, notice the recurring conditions that license greedy: fractional knapsack works because you can split items, Dijkstra's works because weights are non-negative, coin change works because the coin system is canonical. Whenever you reach for greedy, the real question to ask is: "what property of this specific problem makes the locally-best choice also globally-best?"
