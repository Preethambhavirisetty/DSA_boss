# Sorting Algorithms

Bubble Sort

You walk through the list comparing each pair of neighbors, and whenever two are out of order, you swap them. After one full pass, the biggest value has "bubbled" all the way to the end. You repeat, and each pass locks one more large value into place at the back. It's the easiest to understand but one of the slowest, because it does a huge number of swaps and keeps re-scanning. Its one charming trait: if you make a full pass with no swaps, you know the list is already sorted and can stop early.

## Selection Sort

You scan the whole list to find the smallest item, then put it at the front. Then you scan the rest to find the next smallest, and put it second. You keep selecting the smallest of what's left and placing it next in line. Unlike bubble sort, it barely swaps — just once per position — but it always scans everything every time, so it's consistently slow regardless of whether the list started nearly sorted or totally jumbled.

## Insertion Sort

Think of how you sort a hand of playing cards. You pick up cards one at a time and slide each new card into its correct spot among the ones you've already arranged. That's insertion sort: you grow a sorted section on the left, and for each new item you shift the bigger ones over to make room and drop it in place. It's genuinely fast when the list is already almost sorted, because most items barely move. This is why it shows up as a building block inside more advanced sorts for small chunks.

## Merge Sort

This uses divide-and-conquer. You split the list in half, then split those halves, and keep splitting until every piece is a single item (which is trivially "sorted"). Then you merge pieces back together in order — repeatedly taking the smaller front item from two sorted piles to build one bigger sorted pile. Because splitting and merging are both clean and predictable, its speed is reliably good no matter what the data looks like. The catch is that it needs extra memory to hold the merged results, so it's not as space-efficient as some others.

## Quick Sort

Also divide-and-conquer, but it works differently. You pick one element as a "pivot," then rearrange the list so everything smaller than the pivot sits on its left and everything larger sits on its right. Now the pivot is in its final correct position, and you repeat the same process on the left and right chunks. It's usually the fastest in practice and sorts in place without much extra memory. Its weakness is that a bad pivot choice can make it slow, so good implementations choose the pivot cleverly.

The Lomuto and Hoare parts just refer to two ways of doing that rearranging step. Lomuto is simpler to understand — it keeps one pointer sweeping through and swaps smaller items to the front. Hoare is a bit trickier but more efficient — it uses two pointers moving toward each other from both ends, swapping pairs that are on the wrong sides. Hoare generally does fewer swaps, which is why it's often preferred.

## Heap Sort

This uses a structure called a heap, which you can picture as a tree where every parent is bigger than its children, so the largest item always sits at the top. You build this heap out of your list, then repeatedly pluck the top (the maximum) and place it at the end of your sorted region, then fix up the heap so the next-largest rises to the top. It sorts in place with no extra memory and has reliably good speed, but it tends to jump around in memory a lot, which makes it a little slower in real-world use than quick sort despite similar theoretical speed.

## Shell Sort

This is a clever upgrade of insertion sort. Plain insertion sort is slow when a small item is stuck far from where it belongs, because it can only shuffle items one step at a time. Shell sort fixes this by first comparing items that are far apart — say every fifth item — and sorting those, then gradually shrinking the gap until it's finally doing normal insertion sort on a list that's already mostly in order. By moving items long distances early, it saves a lot of the tiny shuffles later.

## Tim Sort

This is the real-world workhorse — it's what Python and Java use by default. It's a hybrid that combines merge sort and insertion sort, and it's built around a smart observation: real data often already contains stretches that are sorted or reverse-sorted. Tim sort finds these natural runs, uses fast insertion sort to tidy up small pieces, and then merges the runs together efficiently. It's fast, stable, and tuned for the messy, partly-ordered data you actually encounter in practice.

## Intro Sort

Another hybrid, this one is C++'s default. It starts with quick sort because that's usually fastest, but it watches out for quick sort's weakness — those bad cases where it slows down badly. If it notices the sorting is going too deep (a sign of trouble), it switches over to heap sort, which has a guaranteed good speed. And for tiny chunks, it uses insertion sort. So it gets quick sort's typical speed while refusing to ever fall into quick sort's worst-case slowness.

## Comb Sort

This is to bubble sort what shell sort is to insertion sort. Plain bubble sort suffers when small values are near the end, because they crawl forward one step per pass. Comb sort fixes this by first comparing and swapping items that are far apart, then shrinking that gap over successive passes until it becomes ordinary bubble sort. Moving items across large gaps early clears out those stragglers quickly, making it far faster than plain bubble sort.

## Cocktail Shaker Sort

A two-way version of bubble sort. Instead of always passing left-to-right, it goes forward bubbling the largest item to the end, then turns around and goes backward bubbling the smallest item to the front, then forward again. This back-and-forth (like liquid sloshing in a shaker) helps move stragglers from both ends more evenly. It's a modest improvement over bubble sort but still fundamentally slow.

## Gnome Sort

An extremely simple method, sometimes described using a garden gnome sorting flower pots. You move forward through the list; when two neighbors are in order you step forward, but when you find a pair out of order you swap them and step backward one position to recheck. You keep stepping back until things are in order, then resume moving forward. It's basically insertion sort expressed with only one loop and no bookkeeping — appealingly simple, but not efficient.

## Cycle Sort

This one is special because it does the absolute minimum number of writes possible. For each item, it figures out exactly where that item belongs by counting how many items are smaller than it, then places it directly there, displacing whatever was in that spot, and continues the chain until everything settles into cycles. Nobody uses it for speed — it's slow to compute — but it's the champion when writing to memory is extremely expensive or wears out (like certain flash storage), because it never moves an item more than it absolutely has to.

## Pancake Sort

More of a puzzle than a practical tool. Imagine a messy stack of pancakes of different sizes, and your only allowed move is to stick a spatula somewhere and flip the whole stack above it. Pancake sort finds the biggest unsorted pancake, flips it to the top, then flips it down to its correct position at the bottom, and repeats with the next-biggest. The interesting constraint is that you can only reverse a prefix of the list — you can't just swap two items — which makes it a neat exercise in problem-solving under weird rules rather than something you'd sort real data with.

If it helps, the pattern across all of these is a small set of core ideas being remixed: comparing-and-swapping neighbors (bubble, cocktail, gnome), picking the right item and placing it (selection, insertion, cycle), splitting-and-recombining (merge, quick), and then hybrids that glue the good parts together for real-world use (tim, intro).

Here's the rest, in plain language. These first four are interesting because they break a rule you might have assumed was unavoidable — that sorting requires comparing items to each other.

## The big idea behind non-comparison sorts

Every sort in the previous list worked by asking "is this item bigger or smaller than that one?" It turns out there's a mathematical speed limit on any method that relies only on such comparisons — you can't beat a certain threshold. But if your data has a special shape — like being whole numbers within a known range — you can sidestep comparisons entirely and use the values themselves as addresses or labels. That's how these next sorts run faster than the supposed limit: they don't play by the comparison rules at all.

## Counting Sort

Suppose you're sorting exam scores that all fall between 0 and 100. Instead of comparing scores, you make a tally: how many people scored 0, how many scored 1, and so on. Once you have those counts, you already know the finished order — just write out all the 0s, then all the 1s, then all the 2s, straight from the tally. You never compared two scores; you just counted. It's blazing fast, but it only works when the range of possible values is small and known, because you need a tally slot for every possible value. Sorting values that range into the billions this way would need an absurd amount of memory.

## Radix Sort

This extends counting sort to handle numbers with many digits, where making one giant tally would be impractical. The trick is to sort by one digit at a time. Picture sorting a pile of multi-digit numbers by first arranging them by their last digit, then re-sorting by the next digit, and so on. As long as each pass keeps the previous order intact for ties, after you've processed all the digits, the whole list ends up fully sorted. You're doing several quick, small sorts instead of one impossible big one.

The LSD and MSD labels are just about which end you start from. LSD means "least significant digit" — you start from the rightmost digit (the ones place) and work left. This is the common, straightforward version. MSD means "most significant digit" — you start from the leftmost digit and work right, sorting into groups and then sorting within each group. MSD is handy for things like sorting words or strings where the leftmost character matters most, and it lets you stop early once groups are distinguished.

## Bucket Sort

This works well when your data is spread fairly evenly across a range. You divide that range into several "buckets" — think of them as labeled bins — and drop each item into the bin its value falls into. Since each bin now holds only items from a narrow slice of the range, each bin has just a few items, which you sort quickly with some simple method. Then you empty the bins in order and you're done. It's fast when the data spreads out evenly so no single bin gets overloaded, but if everything clumps into one bin, it loses its advantage.

## Pigeonhole Sort

This is very close to counting sort and named after the idea of pigeons going into pigeonholes. You create one hole for every possible value in your range, then place each item directly into the hole matching its value. Finally you walk through the holes in order and collect everything back out. The difference from counting sort is mostly one of framing — pigeonhole physically moves the items into slots, while counting sort just tallies how many of each there are. Like counting sort, it only makes sense when the range of values is small relative to how many items you have.

Now the order-statistics pair — a different goal entirely

These next two aren't really about sorting the whole list. They answer a narrower question: "what's the Kth smallest item?" — for example, the median, or the 3rd largest. You could sort everything and then just look at position K, but that's wasteful. These methods find the answer without fully sorting, which is faster.

## Quickselect

This is quick sort's leaner cousin, built for finding one specific ranked item. Remember quick sort picks a pivot and splits the list into "smaller than pivot" on one side and "larger" on the other. Quickselect does that same split, but here's the shortcut: after splitting, you know exactly what rank position the pivot landed in. If the pivot is sitting at the position you're looking for, you're done. If the position you want is on the left side, you only recurse into the left and completely ignore the right — and vice versa. Because you throw away half the work every time instead of sorting both halves, it finds your target much faster than a full sort. Its only weakness is the same as quick sort's: a string of unlucky pivot choices can slow it down.

## Median of Medians

This exists to fix quickselect's one flaw — those unlucky pivot choices. Quickselect is fast on average but can degrade if it keeps picking bad pivots. Median of medians is a clever way to guarantee a decent pivot every time, so the method stays reliably fast even in the worst case. The idea is to break the list into small groups, find the median of each little group, and then find the median of those medians. That "median of medians" is mathematically guaranteed to be a reasonably central value — never too close to the extremes — which ensures each split throws away a healthy fraction of the data. In exchange for this guarantee, it does more upfront work, so in everyday use plain quickselect is usually faster; median of medians is the choice when you absolutely cannot tolerate a bad-luck slowdown.

The throughline here: the first four sorts win by exploiting known structure in the data (small integer ranges, even spreads) to avoid comparisons, and the last two win by realizing that if you only need one ranked item, sorting everything is overkill.
