# Divide & Conquer

Like backtracking, divide and conquer is one strategy wearing many costumes — so it's worth grasping the shared skeleton before the individual problems.

## The core idea of divide and conquer

The strategy has three steps that repeat. First you divide a big problem into smaller versions of the same problem. Then you conquer each small piece — usually by applying the same method again, shrinking further until the pieces are tiny enough to solve trivially. Finally you combine the small solutions back into the answer for the original problem. The power comes from the fact that breaking a problem in half repeatedly reaches the bottom in very few steps, so if the dividing and combining are cheap, the whole thing runs fast. The interesting variation between these algorithms is almost always in that final combine step — that's usually where the cleverness lives.

## Merge Sort

You split the list in half, sort each half (by splitting them in half, and so on, until you're down to single items), then merge the two sorted halves into one sorted list. The dividing is trivial — just cut in the middle. The intelligence is in the merge: walking two already-sorted piles and repeatedly taking the smaller front item to build a combined sorted result. Because every level of splitting is clean and every merge is efficient, its speed is reliably good on any input. It's the archetype where the combine step (merging) does the real work.

## Quick Sort

This flips the emphasis. Instead of dividing trivially and combining cleverly, quick sort divides cleverly and combines trivially. You pick a pivot and rearrange the list so smaller items go left and larger go right — that partitioning is the hard part. Once done, the two sides are independent, and after sorting them there's nothing to combine, because everything's already in the right region. So merge sort and quick sort are mirror images: one puts the effort in combining, the other in dividing.

## Binary Search

This is divide and conquer stripped to its leanest form. You look at the middle of a sorted range and, based on one comparison, discard half of it — then repeat on the remaining half. What makes it unusually efficient is that it doesn't even need to conquer both halves; a single comparison tells you which half is worthless, so you throw it away entirely and only recurse into one side. There's no combine step at all. It's the purest illustration of how halving the problem repeatedly leads to a solution in very few steps.

## Closest Pair of Points

Given many points scattered on a plane, you want the two that are nearest each other. Checking every possible pair is slow. The divide-and-conquer approach splits the points into a left group and a right group by a vertical line, finds the closest pair within each group, and takes the smaller of those two distances. The subtle part is the combine step: the true closest pair might straddle the dividing line, so you must check pairs that sit near the boundary. The clever insight is that only points within a thin strip around the line can possibly beat the best distance found so far, and within that strip you only need to compare each point to a handful of nearby neighbors — which keeps the combine step fast. This is a classic example where the combine step requires genuine cleverness to stay efficient.

## Karatsuba Multiplication

This multiplies two very large numbers faster than the grade-school method you learned as a child. The schoolbook way multiplies every digit of one number by every digit of the other, which gets slow for huge numbers. Karatsuba splits each number into a high half and a low half and observes that the multiplication can be rebuilt from three smaller multiplications instead of the four you'd naively expect — a bit of algebra lets one multiplication do double duty. Cutting four sub-problems down to three at every level of recursion compounds into a meaningful speedup. It was a landmark result because it proved the "obvious" schoolbook method wasn't actually the fastest possible.

## Strassen's Matrix Multiplication

This is the same spirit as Karatsuba, but for multiplying grids of numbers (matrices) instead of plain numbers. The standard method for multiplying two matrices requires a certain number of smaller multiplications when you break the matrices into quadrants — eight of them, naively. Strassen found a clever rearrangement that accomplishes the same result with only seven multiplications, trading them for some extra additions. Since multiplications are the expensive part and they compound at every level of recursion, shaving eight down to seven yields a faster overall method for large matrices. Like Karatsuba, its significance is conceptual: it broke through what everyone assumed was the natural speed limit.

## Maximum Subarray (Divide and Conquer variant)

This finds the contiguous stretch of a list with the largest sum. (Kadane's algorithm solves it in a simple single pass, but there's also a divide-and-conquer way that's instructive.) You split the list in half and reason that the best stretch is one of three things: entirely in the left half, entirely in the right half, or crossing the middle. You solve the left and right by recursion, and then handle the crossing case directly by extending outward from the center in both directions to find the best stretch that spans the split. Taking the best of those three gives the answer. The crossing case is the combine step — the part unique to this approach and the reason it's a good teaching example of the divide-and-conquer mindset.

## Fast Exponentiation (Binary Exponentiation)

This computes a number raised to a big power quickly. Multiplying the base by itself over and over would take as many multiplications as the exponent's size — slow for large exponents. The trick uses a simple observation: to raise something to an even power, you can raise it to half that power and then square the result. Squaring roughly doubles the exponent with a single multiplication, so you reach enormous powers in very few steps by repeatedly halving the exponent. It's a small but extremely common technique, essential in cryptography and anywhere large powers (often combined with remainders) need computing efficiently.

## Count Inversions

An inversion is a pair of items that are out of order relative to each other — a larger value sitting before a smaller one. Counting them measures how "unsorted" a list is (useful for things like comparing rankings or preferences). Counting every out-of-order pair directly is slow, but there's an elegant trick: piggyback on merge sort. As you merge two sorted halves, whenever you pull an item from the right half before some items still waiting in the left half, you've just discovered that those waiting left items each form an inversion with it — and you can count them all in a batch. So the merge step, which was already sorting, does the counting essentially for free. It's a lovely example of extracting extra information from an existing algorithm's structure.

Two patterns are worth carrying away. First, the combine step is where these algorithms differ and where their cleverness usually lives — trivial in binary search and quick sort, but the whole game in closest-pair, count-inversions, and the max-subarray variant. Second, several of these (Karatsuba, Strassen, fast exponentiation) win by reducing the number of sub-problems at each level — turning four multiplications into three, eight into seven, or an exponent into its half — and because that reduction compounds across every level of recursion, even a small saving per level produces a large overall speedup.
